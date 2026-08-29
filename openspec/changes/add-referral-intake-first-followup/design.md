# Design — evaluation-only Referral intake and first follow-up

## Context

See `proposal.md` — Why. The repository currently contains documentation only: there is no
`apps/`, no `packages/`, no migrations, and no `scripts/verify.sh`. This change is therefore both
the first vertical slice and the first time the monorepo skeleton exists, which shapes several
decisions below.

Constraints that drive the design:

- `AGENTS.md`: TypeScript modular monolith in a monorepo; React in `apps/web`; Express in
  `apps/api`; PostgreSQL for persistence; domain logic must not depend on HTTP, React, or database
  frameworks; shared request and response contracts in `packages/contracts`; no new service, queue,
  cache, or framework without a demonstrated requirement.
- The slice is evaluation-only: isolated, synthetic data, no real authentication, no external
  integration, production deployment disabled.
- Several behaviors in `specs/` are explicitly all-or-nothing (intake creation, activation, stage
  completion, NFAR closure, discard cascade, Follow-up reversal, and every audited change). That
  transactional requirement is the single strongest constraint on module boundaries.
- All business dates and times are in the `America/Edmonton` Firm Time Zone, and scheduling depends
  on a mutable Firm Business Day calendar.

## Goals / Non-Goals

**Goals:**

- Keep every atomic business transition expressible as one transaction that includes its Audit
  History entry.
- Keep Firm Time Zone and Firm Business Day arithmetic in one place that is testable without a
  database or an HTTP server.
- Give each capability in `specs/` one owning module with an explicit interface, so the next slice
  can extend outreach without reworking Household identity.
- Make the evaluation boundary (synthetic data, no egress, no production deploy) enforced by the
  build and deployment configuration, not by convention alone.

**Non-Goals:**

- Authentication, session management, authorization policy, and security logging. The acting
  identity is fixed and injected; there is no login.
- Any background scheduler, queue, or worker process.
- Multi-tenancy, horizontal scaling, caching, or performance tuning. The evaluation environment
  serves a handful of concurrent evaluators.
- A generic workflow or rules engine. The Follow-up Call stage is the only stage in this slice and
  is modelled directly.

## Decisions

### D1 — Modules and their interfaces

One module per capability in `specs/`, each exposing a small command/query interface in terms of
domain types only. No module imports Express, React, or the database client.

| Module | Owns | Public interface (shape, not signature) |
|---|---|---|
| `households` | Household, Household Member, Client Number, duplicate rules | `createHouseholdWithMembers`, `updateHousehold`, `establishPreExistingClientHousehold`, `findDuplicateCandidates`, `reactivateHousehold`, `searchHouseholds` |
| `referrals` | Referral, Referral Source, Referral Received Date, status, ownership | `saveDraft`, `activate`, `updateActiveReferral`, `changeOwner`, `addSource`, `discardDraft`, `searchReferrals` |
| `referral-outreach` | Attempts, stage outcome, dispositions, corrections | `recordAttempt`, `completeFollowUpCall`, `skipFollowUpCall`, `setDisposition`, `reopenFromNfar`, `correctAttempt`, `markAttemptEnteredInError`, `reverseFollowUpCompletion` |
| `tasks` | Task record, status, assignment, due dates, flags | `createTask`, `reschedule`, `reassign`, `completeTask`, `cancelTask`, `listTasks`, `derivedTaskState` |
| `scheduling` | Firm Time Zone, Firm Business Day calendar, due-date and reminder arithmetic | `isFirmBusinessDay`, `addFirmBusinessDays`, `firstFollowUpDueDate`, `defaultNextStageDueDate`, `reminderTime`, `earliestSelectableDueDate` |
| `reference-records` | Event and COI minimal records, their duplicate rules | `createEvent`, `createCoi`, `deactivate`, `reactivate`, `findReferenceDuplicates`, `searchReferences` |
| `identity` | Associate, the fixed `Evaluation User` development identity, calendar maintenance | `listAssociates`, `createAssociate`, `deactivateAssociate`, `currentIdentity`, `updateFirmCalendar` |
| `audit` | Audit History entries and their retrieval | `recordChange`, `historyForRecord` |

Dependency direction is one-way: `referral-outreach` → `referrals`, `tasks`, `scheduling`, `audit`;
`referrals` → `households`, `reference-records`, `tasks`, `scheduling`, `audit`; `tasks` →
`scheduling`, `audit`; every module → `identity` for the acting Platform User. `audit` and
`scheduling` depend on nothing. There are no cycles.

**Alternative considered:** a single `rms` module. Rejected because Household identity and outreach
workflow have different lifetimes — the next slices add conversion and appreciation on top of
Household while outreach keeps growing — and `docs/product/journeys.md` explicitly separates
durable identity from workflow.

### D2 — Atomic transitions and the audit entry share one transaction

Every command that the specs describe as all-or-nothing runs inside a single PostgreSQL
transaction that also inserts its Audit History rows. An application-service layer (thin, per
module) opens the transaction, calls pure domain functions to compute the new state and the audit
entries, and persists both. If the audit insert fails, the transaction rolls back and the business
change does not happen.

**Alternative considered:** write the audit entry asynchronously (outbox table, background drain).
Rejected: `docs/product/vision.md` requires that a change needing Audit History "succeeds with
its history entry or fails entirely", and an outbox accepts an unaudited change for later repair.
Also rejected: an application-level "audit first, then change" sequence, which can leave a recorded
change that never happened.

**This is hard to reverse and has a real alternative — it should be recorded as an ADR.**
`docs/adr/README.md` is currently empty, so this design proposes, but does not write:

> **Proposed ADR-0001 — Audit History entries are written in the same database transaction as the
> change they audit.** Alternatives: transactional outbox with asynchronous drain; separate audit
> service. Consequences: (a) the audit store becomes a hard dependency of every write — when it is
> unavailable, the platform accepts no business change at all; (b) audit growth is unbounded,
> because entries are append-only and nothing prunes them, so storage grows monotonically with
> use; and (c) the audit store is the sole accountability record for the platform while
> operational backup and recovery are deferred (see `proposal.md` — Data Sensitivity, which makes
> backup and recovery a precondition for real client use), so losing it loses all accountability
> history with no restore path.

Because task 3.3 implements this commitment, the ADR-0001 decision is taken at the `PLAN APPROVED`
gate (task 1.1) rather than at the end of the change.

### D3 — Time is stored as instants plus firm-calendar dates

Points in time (attempt timestamps, audit timestamps, completion times) are stored as UTC instants.
Date-only business values (Referral Received Date, task due date, Event date, Client Start Date,
firm calendar entries) are stored as calendar dates with no time zone. Due time (5:00 p.m.) and
reminder time (9:00 a.m.) are applied by the `scheduling` module in the Firm Time Zone at the
moment of comparison or display, using the IANA `America/Edmonton` zone so daylight saving is
handled by the zone database rather than by arithmetic.

**Alternative considered:** store due dates as timestamps already resolved to 5:00 p.m. local.
Rejected because a later firm-calendar or policy change to the due time would require rewriting
stored data, and because it invites accidental time-zone drift across daylight-saving boundaries.

**This choice is expensive to reverse — it should be recorded as an ADR.** This design proposes,
but does not write:

> **Proposed ADR-0002 — Business date-only values are stored as calendar dates; times of day are
> applied in the Firm Time Zone at comparison and display time.** Alternatives: store resolved
> local timestamps; store all values as UTC instants. Consequence: every read path that compares
> "now" to a due date must go through `scheduling`.

### D4 — Derived states are computed, not stored, and no scheduler exists

`Overdue`, the 9:00 a.m. reminder indicator, and `Due on non-business day` are derived on read from
the task's due date, its status, the current Firm Time Zone time, and the current firm calendar.
Nothing writes a notification record, and no background job flips a flag.

This directly satisfies three spec behaviors that are awkward to implement any other way: a
reminder that "appears immediately when the save succeeds" if its calculated time has already
passed; a `Due on non-business day` flag that appears the moment the calendar changes without
touching the task row; and the absence of dismiss, snooze, and separate notification records.

**Alternative considered:** a scheduled job that materializes reminders. Rejected — it adds a
process the slice does not need, and it would make "reminder appears immediately" a race.

The `Due on non-business day` flag is therefore a *derived* flag. Clearing it happens implicitly
when the Platform User reschedules onto a valid Firm Business Day; there is no separate "clear
flag" command.

### D5 — Optimistic concurrency with an explicit version token

Household and Referral responses carry a version token. Every mutating request echoes the token it
was loaded with; the API rejects a mismatch with a distinct conflict response that the web app
renders as "reload and reapply". Nothing is merged automatically and nothing is overwritten.

**Alternative considered:** last-write-wins (violates the specs) and pessimistic row locks across
a user's editing session (holds locks across think-time). The version token is also cheap to
extend to tasks and reference records if later slices need it.

### D6 — Duplicate detection is a query inside the same transaction

Duplicate checks for phone, email, Client Number, Event name-and-date, and COI phone/email/name run
inside the writing transaction, backed by normalized comparison columns (digits-only phone,
lower-cased email, trimmed and case-folded name) and unique constraints where the rule is an
absolute block. The unique constraint is the authority; the pre-check exists to produce a helpful
message, not to be the only guard. This makes concurrent duplicate creation impossible rather than
merely unlikely.

Exact-name matching is a warning, not a constraint, so it stays a query. The acceptance of that
warning is itself an audited fact recorded with the Household.

### D7 — Contracts and the API surface

`packages/contracts` holds the request and response types plus their runtime validators, imported
by both `apps/api` and `apps/web`. Endpoints are resource-oriented and mirror the module commands;
each all-or-nothing transition is exactly one endpoint call, so a partially applied transition
cannot be produced by a client that stops half-way. Validation errors return the complete set of
problems in one response, because activation must "present all validation problems together".

### D8 — Identity injection and the trust boundary

There is no authentication. `apps/api` injects the fixed `Evaluation User` Platform User identity
at the request boundary, in exactly one place, and every module receives the acting identity as an
explicit argument rather than reading a global. That injection point derives the identity solely
from the request boundary and ignores every client-supplied identity input: an actor, user, or
on-behalf-of value in a body, query string, or header is rejected or dropped and can never reach
the audit actor. This matters because the actor is the only accountability signal in the slice, and
`CONTEXT.md` binds against treating an editable Associate selection as the identity of the acting
user — the call performer is selectable, the actor is not. When real authentication arrives in a
later slice, that single injection point is replaced; no module signature changes.

**Trust boundaries:**

- The evaluation environment perimeter is the only security boundary. Everything inside it is
  trusted, which is precisely why it must not be publicly reachable. That is enforced in the
  application, not only by operator discipline: the API binds loopback by default, refuses to start
  on a non-loopback interface unless an explicitly named acknowledgement value is set, and logs the
  interface it bound at startup so an evaluator can see what was actually exposed.
- The browser is not trusted for business rules: every invariant in `specs/` is enforced in the
  API and domain layers, and the web app's disabled controls (notably the disabled
  `Discovery package sent` completion action) are duplicated as server-side refusals.
- The service is intended to make no outbound network calls. A dependency-manifest check alone
  cannot establish that, because Node ships global `fetch` and the `node:http`, `node:https`,
  `node:net`, `node:tls`, `node:dgram`, and `node:child_process` modules with no dependency to
  declare. Two controls therefore apply together: the manifest check (no HTTP client, mail
  transport, or third-party SDK in `apps/api`'s dependencies) and a source-level ban on importing
  those modules or calling global `fetch` anywhere in `apps/api` or the domain modules. These are
  static controls: they make an accidental egress path visible in review and in CI, and they are
  not a substitute for the network isolation of the evaluation environment itself.
- Secrets: the build has no production credentials. The database connection string is the only
  configuration value, supplied per environment and never committed.

### D9 — Data ownership

One PostgreSQL schema, one set of tables, but each table is written by exactly one module and read
by others only through that module's interface:

| Tables | Written by |
|---|---|
| `household`, `household_member`, `household_contact` | `households` |
| `referral`, `referral_source` | `referrals` |
| `outreach_attempt`, `referral_stage_outcome` | `referral-outreach` |
| `task` | `tasks` |
| `firm_calendar_day` | `identity` (maintenance) and read by `scheduling` |
| `event_reference`, `coi_reference` | `reference-records` |
| `associate` | `identity` |
| `audit_entry` | `audit` only |

`audit_entry` is append-only: no update or delete path exists in the module interface, and the
database role used by the application is granted `INSERT` and `SELECT` on it but not `UPDATE` or
`DELETE`.

### D10 — Seeding and reset live outside the application, behind their own preconditions

The known synthetic baseline and the reset operation are a repository script run against the
environment, not an API route and not a UI action. This is what makes reset "outside Platform User
actions and Audit History" true by construction rather than by a hidden permission check.

Reset is destructive and takes its target from configuration, so it is guarded rather than trusted:
it refuses to run unless the target database carries the evaluation-environment marker row that
reset itself seeds, and unless evaluation-mode configuration is present. The same marker row is
what API startup and the migration runner check, so the three-layer production guard in
`specs/evaluation-environment/spec.md` and the reset guard share one mechanism: a deployment-path
refusal, an evaluation-mode configuration marker, and a database marker row. Lifting any of them
requires a code change, which is the point — a mistyped `DATABASE_URL` must not be able to point an
ordinary build, a migration, or a reset at a database that was never meant for it. Reset also
leaves an out-of-band operator record of when it ran and against which target.

## Risks / Trade-offs

- **Audit availability becomes write availability (D2)** → Accepted deliberately; it is the
  product requirement. Mitigation: `audit_entry` is a simple append-only table in the same
  database, so it fails only when the database itself is failing, and read paths stay available.
- **Derived reminder and overdue state is recomputed on every list read (D4)** → Mitigation: the
  evaluation data volume is tiny and the computation is pure. If a later slice needs scale, the
  derivation is already isolated in `scheduling` and can be materialized without changing behavior.
- **Firm Business Day arithmetic is easy to get subtly wrong** (Friday-of-week versus the
  three-day floor, activation-day exclusion, after-hours activation, daylight saving) → Mitigation:
  `scheduling` is pure and dependency-free, and every scenario in
  `specs/tasks-and-scheduling/spec.md` becomes a table-driven unit test.
- **A mutable calendar can invalidate existing due dates** → This is specified behavior, not a
  defect: the task is flagged rather than moved. Risk is that the flag is derived, so a bug in the
  derivation silently hides the flag. Mitigation: explicit tests that add a closure day under an
  existing task and assert the flag and its Needs Attention entry.
- **No authentication in a build that models a real workflow** → Mitigation: the evaluation
  boundary requirements in `specs/evaluation-environment/spec.md` are treated as testable product
  behavior, including a refused production deployment path, and the synthetic-only rule is
  documented in the repository.
- **`Discovery package sent` completion is disabled in the UI only, if implemented carelessly** →
  Mitigation: the server refuses the completion command for that task type; the UI state is a
  convenience.
- **Duplicate and conflict responses disclose the matched record** → Duplicate blocks and
  stale-save conflicts name the record they matched, including inactive Households and
  `Discarded Draft` Referrals, which is required for the Platform User to act on them and is
  harmless under V1 equal access; this must be revisited when per-user visibility arrives.
- **Pilot-time abuse controls are deferred with authentication** → Rate limiting, field length
  bounds, and request size limits are not part of this slice; they belong with authentication
  before any operational pilot, and field length bounds in particular should be settled alongside
  the validation rules rather than retrofitted.
- **First-slice-shaped modules could become the wrong shape when conversion and appreciation
  arrive** → Mitigation: Household identity is already separated from Referral workflow, which is
  the boundary the later slices cross; outreach stage handling is deliberately concrete rather
  than a premature generalization.

## Migration Plan

- **Schema**: forward-only, ordered SQL migrations checked into the repository, applied by a
  repository script. No down-migrations are authored for this slice: the recovery path for an
  evaluation environment is reset to baseline, which is faster and safer than reversing a partially
  applied change.
- **Deploy**: build `apps/api` and `apps/web`, run migrations, run the seed/reset script to
  establish the known synthetic baseline. Evaluation environments only; the production deployment
  path stays disabled and refuses to run.
- **Rollback**: revert the change's commits, redeploy, and reset the environment to baseline.
  Because all data is synthetic and disposable, there is no data back-out obligation.
- **Data migration from spreadsheets**: none. Import is out of scope; the baseline is synthetic
  seed data modelled on the sanitized spreadsheets' structures without copying their rows.

## Observability

- **Business history** is Audit History, which is a product feature, not a log. It is the record of
  who changed what and when, and it is the primary observability surface for evaluation.
- **Technical logs**: structured server-side request logs with a correlation identifier, plus error
  logs for failed transactions. Because rejected saves, validation failures, and stale-save
  conflicts are deliberately *excluded* from Audit History, the technical log is where an evaluator
  and the implementer can see that they happened — this is the intended split, and the two must not
  be conflated.
- Log content is a requirement, not a convention: `specs/evaluation-environment/spec.md` —
  "Technical logs carry no business content" limits technical logs to record identifiers, error
  classes, and a correlation identifier, and forbids contact details, note or reason text, Client
  Numbers, and record field values. This is load-bearing precisely because the entries above route
  rejected saves and validation failures — the submissions most likely to carry contact details —
  into technical logs. Task 3.5 verifies it with a redaction test rather than by inspection.
- Security and authentication event logging is explicitly a later-slice prerequisite and is not
  implemented here.
- A health endpoint reports database reachability, so an evaluator can distinguish "audit writes
  are failing" (specified read-only degradation) from "the app is down".

## Verification

- **`scripts/verify.sh` does not exist in this repository yet.** `AGENTS.md` requires it before
  declaring implementation complete, so creating it is the first implementation checkpoint in
  `tasks.md`. It must run: TypeScript type-check, lint, unit tests, integration tests, and
  `openspec validate --all --strict`.
- **Unit tests, no I/O**: `scheduling` (Firm Business Day arithmetic, the Friday-of-week rule and
  its three-day floor, activation-day exclusion, after-hours and weekend activation, reminder time,
  earliest selectable reschedule date, daylight-saving boundaries) and the pure decision functions
  in `households`, `referrals`, and `referral-outreach` (validation aggregation, duplicate
  classification, disposition legality, dependency protection for attempts).
- **Integration tests against PostgreSQL**: every all-or-nothing transition, asserting both the
  success shape and the failure shape (nothing created, nothing audited, entered values returned).
  Retry-after-failure tests assert exactly-once outcomes. Concurrency tests assert the stale-save
  conflict and the duplicate unique-constraint guard.
- **Contract tests**: `packages/contracts` validators exercised against the API's actual responses,
  including the aggregated validation-error response.
- **End-to-end walkthroughs**: the evaluation-success list in `docs/product/journeys.md`
  ("Evaluation success") is the acceptance script — one automated walkthrough per bullet.
- **The first-follow-up scheduling branches are evidenced by unit tests, not by the walkthrough.**
  Activation uses server "now", so a single hands-on session can only exercise the branch the
  evaluation date happens to fall into: the Friday-of-week default, the shift to the preceding Firm
  Business Day, and the three-Firm-Business-Day floor cannot all be reached on one day. Branch
  correctness is therefore established by the table-driven unit tests in task 2.2, and the
  walkthrough demonstrates only the branch its date produces. The baseline seeds a firm calendar
  containing both a Friday closure day and a non-Friday closure day so that the Friday-shift branch
  can at least be inspected, and it may seed already-activated Referrals whose first follow-up tasks
  were positioned by different branches so an evaluator can see the results side by side; seeded
  results are illustrations, and the unit tests remain the evidence.
- **Boundary checks**: the two egress checks from D8 (dependency manifest, plus a source-level ban
  on `node:http`, `node:https`, `node:net`, `node:tls`, `node:dgram`, `node:child_process`, and
  global `fetch`); loopback-by-default binding, the refusal to bind a non-loopback interface without
  the named acknowledgement value, and the startup log of the bound interface; all three layers of
  the production guard and the reset guard, each asserted to refuse; a log redaction test; and a
  provenance check that no value, free-text fragment, or file originating in
  `agent_analysis_package/` appears in seed data, fixtures, tests, or application code, matching
  substrings of free-text columns rather than whole rows.
- All test data is synthetic and clearly marked. No real firm, client, or financial data is used in
  any test, fixture, or screenshot.

## Open Questions

These can be answered during implementation without changing the specs, the module boundaries, or
the task breakdown. The unresolved *product* decisions are in `proposal.md` — Unresolved Human
Decisions, and are not repeated here.

- The baseline's composition is **not** an open question here. It determines what the firm lead can
  demonstrate, so it is specified in product terms in `specs/evaluation-environment/spec.md` —
  "The synthetic baseline supports the whole evaluation demonstration". Implementation chooses
  exact names and counts, not which states must exist.
- Whether the version token in D5 is a row version counter or a last-modified token; both satisfy
  the specs.
- Which test runner and integration-test database strategy (per-test transaction rollback versus
  per-suite reset) the repository adopts, given no tooling exists yet.
