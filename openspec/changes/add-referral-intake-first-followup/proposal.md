# Evaluation-only Referral intake and first follow-up

## Why

Referral coordination currently lives in disconnected spreadsheets. Staff re-enter the same
identity and contact details, search several places for a Referral's owner, stage, and next
action, and risk missing follow-up entirely [E-001, E-002, E-029]. Discovery establishes that an
introduction already triggers an RMS entry [E-012] and that intake usually needs a name, a phone
number, and an email address [E-015]. That Referral intake plus the first follow-up call is the
right first slice is not established by those entries: it is the approved scope in
`docs/product/vision.md` "Delivery Shape" and `docs/product/journeys.md` "First Vertical Slice".

The firm lead has approved building this slice as an **evaluation-only** build so that the
flow can be exercised hands-on before any operational pilot. Flow details remain subject to
revision after that hands-on review; this proposal is not operational or production acceptance.

## User Outcome

Using only clearly synthetic data and a fixed development identity, an evaluator can:

- set up synthetic Associates and the Firm Business Day calendar, and set up the minimal
  Client, Event, and Centre of Influence (COI) reference records needed for Referral Source
  attribution;
- find an existing Household or create a Household with its members, and save a Referral as a
  `Draft` that names what is still missing;
- attribute any of the six Referral Source types, then activate the Referral to `In process`
  with exactly one correctly scheduled first follow-up task;
- record follow-up attempts (`No answer`, `Voicemail`, `Two-way conversation`), reschedule and
  reassign the same Open Task, skip the stage with a reason, or close the Referral with an NFAR
  disposition and reopen it later;
- see exactly one `Discovery package sent` task created as the handoff to the next slice, with
  its completion action disabled;
- correct mistakes — attempt correction, `Entered in error`, reversal of an incorrect Follow-up
  completion, `Discarded Draft` — without losing history; and
- read contextual Audit History on each affected record, and reset the environment to a known
  synthetic baseline.

Duplicate blocking, stale-save conflict rejection, and the disabled upload/external-integration/
production-deployment paths are part of that demonstration, not later cleanup.

## What Changes

- **New** Household records with members, editable display name, per-member Client Number
  uniqueness, contact information, and the standalone Pre-existing Client Household setup path.
- **New** Referral records with `Draft`, `In process`, three NFAR dispositions, and
  `Discarded Draft` statuses; the six Referral Source types with their required attribution; and
  Referral Received Date.
- **New** activation as one atomic business transition that validates all invariants together and
  creates exactly one first follow-up task, plus continuing `In process` invariants.
- **New** Follow-up Call outreach: attempts, next-due-date rules, two-way conversation results,
  `Skipped` stage outcome, NFAR closure and reopening, and correction/reversal paths.
- **New** tasks with statuses `Open`, `Completed`, `Cancelled`, derived `Overdue`, Firm Business
  Day scheduling, 5:00 p.m. Firm Time due time, the 9:00 a.m. persistent reminder indicator,
  manual rescheduling constraints, and the `Due on non-business day` flag.
- **New** minimal Event and COI reference records with inline creation from Referral intake,
  duplicate blocking, and deactivate/reactivate.
- **New** synthetic Associates and the fixed `Evaluation User` development identity, which is the
  non-overridable Audit History actor and the default call performer and default Tasks filter.
- **New** contextual Audit History recording previous value, new value, actor, and timestamp,
  written all-or-nothing with the change it audits.
- **New** evaluation-environment boundary: synthetic data only, isolated and non-public, no
  upload/import/email/external integration, production deployment disabled, and environment
  reset outside Platform User actions and Audit History.

No existing behavior is modified or removed: `openspec/specs/` is currently empty and nothing is
implemented. There are no **BREAKING** changes.

## In Scope

The pages exposed are limited to Referrals and Referral detail, Tasks, Households, minimal
Events, minimal COIs, synthetic Associates, and contextual `Audit history` actions.

- Household and Household Member identity, contact information, and duplicate rules
- Pre-existing Client Household setup so Client Referral Sources can be selected
- Draft Referral creation, editing, discovery, and `Discarded Draft`
- All six Referral Source types: Client (C), Event (E), Marketing (M), Social Media (SM), COI, Other (O)
- Referral activation, `In process` invariants, ownership change, and stale-save conflict rejection
- The Follow-up Call stage only, through completion, skip, or NFAR
- Creation of the `Discovery package sent` task as the boundary artifact
- Task statuses, Firm Business Day scheduling, reminders, and rescheduling
- Minimal Event and COI reference records used as Referral Sources
- Synthetic Associates, the `Evaluation User` development identity, and firm-calendar maintenance
- Contextual Audit History
- The evaluation-environment boundary and reset

## Out of Scope

Each item below is deliberately excluded and must not be implemented by this change.

**The slice stops before:**

- Real authentication, Platform User provisioning, sign-in access grant/revoke, and security-event logging
- Use of any real firm, client, or financial data
- Executing the `Discovery package sent` task — its completion action is disabled
- Later outreach stages: `Discovery package received`, `First meeting`, decision follow-up
- Conversion (`Became client`), Client Start Date, and client appreciation (thank-you cards,
  three-month service call, appreciation events, invitations, responses, attendance, `Do Not Invite`)
- Reports, on-demand report generation, and CSV export of reports
- Household merging, the `Merged` Referral status, and merge reversal
- Spreadsheet import, import reversal, and import provenance
- Restoring a `Discarded Draft`
- Reopening a Referral into any stage later than Follow-up Call
- Temporary COI names and their mandatory resolution tasks
- Event management beyond source identity, and COI management beyond source identity
- Pillar List, Investment Transfer, Transfer Reconciliation, Insurance Business Monitor, Focus 10
- A global Audit History page
- `Needs attention` entries that depend on out-of-scope capabilities — `Became client` Referrals
  missing a Client Start Date, and unresolved temporary COI sources. The in-slice view carries only
  unowned `Draft` Referrals, active Referrals with no Open Task, and tasks flagged
  `Due on non-business day`
- Role-based access control and user-specific visibility [E-043]
- Automatic client email, push notifications, dismiss/snooze, and separate notification records [E-006, E-018]
- Associate profiles, teams, job titles, roles, permission configuration, and workload summaries
- Document storage
- Operational backup, retention, and migration of evaluation data
- Production deployment

**Journeys "Decisions Still Needed" — out of scope and not resolved here:**

- Firm-lead approval of broader RMS-release decisions outside this evaluation slice
- Professional review of equal-access permissions and privacy, compliance, retention, and
  security-governance policy
- Firm-lead and professional review of excluding `Do not invite` preference changes from Audit History
- Deadlines for both thank-you-card tasks
- Whether linked COI sources also receive a thank-you task
- Whether RMS should track and remind on pending account opening between `Became client` and the
  Client Start Date
- Firm-lead approval of the initial firm holiday and closure calendar and its authorized maintainer

## Capabilities

### New Capabilities

- `households`: Household and Household Member identity, display name, Client Number uniqueness,
  contact information, Pre-existing Client Household setup, duplicate blocking and name-match
  acceptance, and inactive-Household reuse and reactivation.
- `referrals`: Draft Referral lifecycle, the six Referral Source types and their required
  attribution, Referral Received Date, atomic activation, continuing `In process` invariants,
  stale-save conflict rejection, ownership change versus task assignee, and the `Discarded Draft`
  cascade to a newly created Household.
- `referral-outreach`: Follow-up Call attempts and outcomes, next-due-date rules, two-way
  conversation results, the `Skipped` stage outcome, atomic creation of exactly one
  `Discovery package sent` task with its completion action disabled, NFAR dispositions and
  closure, NFAR reopening, attempt correction and `Entered in error`, and reversal of an
  incorrect Follow-up completion.
- `tasks-and-scheduling`: Task statuses `Open`/`Completed`/`Cancelled`, derived `Overdue`, the
  first-follow-up default due date, the 5:00 p.m. Firm Time due time, the 9:00 a.m. persistent
  reminder indicator, manual rescheduling constraints, the Firm Business Day calendar, and the
  `Due on non-business day` flag with Needs Attention.
- `reference-records`: Minimal Event and COI records, inline creation from Referral intake,
  duplicate blocking, and deactivation/reactivation that preserves existing attribution.
- `associates-and-identity`: Synthetic Associates, the fixed `Evaluation User` development
  identity as default audit actor, default call performer, and default Tasks filter, firm-calendar
  maintenance, and blocked deactivation while work is owned.
- `audit-history`: What is and is not recorded, previous value plus new value plus actor plus
  timestamp, contextual per-record access, and all-or-nothing writing with the audited change.
- `evaluation-environment`: Synthetic-data-only boundary, isolated and non-public operation,
  disabled upload/import/email/external integration, disabled production deployment, and
  environment reset to a known baseline outside Platform User actions and Audit History.

### Modified Capabilities

None. `openspec/specs/` is empty; this change introduces the first behavior for this platform.

## Impact

- **New code**: `apps/web` (React) for the seven in-scope pages, `apps/api` (Express) for the
  REST surface, `packages/contracts` for shared request and response contracts, and domain
  modules for Household, Referral, outreach, scheduling, reference records, identity, and audit.
- **New persistence**: PostgreSQL schema for the records above, created by forward migrations.
- **No external systems**: no email, messaging, file upload, import, or third-party integration
  is introduced.
- **Deployment**: evaluation environments only. The production deployment path stays disabled.
- Detailed module boundaries, data ownership, trust boundaries, and verification approach are in
  `design.md`.

## Dependencies

- **Approved product direction** — `docs/product/vision.md` "Delivery Shape" and
  `docs/product/journeys.md` "First Vertical Slice" are the approved scope for this change.
- **Glossary** — `CONTEXT.md` supplies the binding vocabulary. This change introduces no synonyms.
- **Firm Business Day calendar** — Firm Business Day scheduling requires a maintained holiday and
  closure calendar. The firm lead has not yet approved the initial calendar or its authorized
  maintainer, so the slice ships with a clearly synthetic evaluation calendar that the evaluation
  identity maintains.
- **Runtime** — TypeScript monorepo, React, Express, PostgreSQL, `America/Edmonton` time-zone data.
  No new service, queue, cache, or framework is required.
- **Hands-on UI validation by the firm lead** is the accepted mechanism for revising flow details
  after this change is built. It is a follow-on activity, not a precondition for implementation.

## Data Sensitivity

- The evaluation build **must never receive real firm, client, or financial data**. Every record,
  fixture, seed, screenshot, and test input is synthetic and clearly marked as synthetic.
- Synthetic seed data may follow the sanitized spreadsheets' structures and value patterns, but
  sanitized spreadsheet rows are **not** copied into fixtures.
- The build runs locally or in an isolated private evaluation environment and is never exposed
  publicly or to an untrusted network.
- The data restriction is documented in the repository rather than presented as an in-product
  warning.
- There is no real authentication, so the build carries no credential material and no production
  secrets.
- Evaluation data carries no backup, retention, or migration promise. Operational backup and
  recovery, and professionally approved retention, privacy, compliance, and security governance,
  must be established before any real client use.

## Rollback and Disable Path

- The slice is one independently deployable vertical slice targeting evaluation environments only.
  Rolling it back removes an evaluation build; no operational workflow depends on it.
- **Disable**: the evaluation environment can be shut down or made unreachable at any time. No
  external system, scheduled job, or outbound communication continues to run.
- **Reset**: environment reset restores a known synthetic baseline. It is an environment operation
  outside Platform User actions and is not recorded in Audit History.
- **Code rollback**: revert the change's commits and redeploy the evaluation environment. Because
  the data is synthetic and disposable, no data migration back-out is required; the environment is
  reset to baseline instead of down-migrated.
- **Production deployment remains disabled** for the life of this slice, so there is no production
  rollback surface to manage.

## Unresolved Human Decisions

Each entry below needs a human decision. None blocks implementation of the behavior specified in
this change. Flow acceptance itself is not listed here: hands-on UI validation by the firm lead is
recorded under Dependencies as the mechanism for revising flow details, not as a decision to be
taken before building.

1. **Initial firm holiday and closure calendar and its authorized maintainer** — not yet approved
   by the firm lead. The slice uses a synthetic evaluation calendar maintained by the evaluation
   identity. *Blocks*: any operational pilot, because every due date and reminder in the platform
   is computed from this calendar. *Either way*: approving a real calendar changes seeded data and
   the dates an evaluator sees, not any specified behavior; naming a maintainer other than the
   evaluation identity introduces the first administrative permission boundary, which this slice
   deliberately does not enforce.
2. **Per-task reminder override** — `docs/product/journeys.md` describes a changeable default
   reminder at the RMS-release level, while both documents describe the first slice's reminder as
   a persistent derived indicator. This change specifies only the derived indicator. *Blocks*:
   nothing in this slice. *Either way*: keeping the derived indicator leaves reminders computed
   with nothing stored; allowing an override means a reminder time becomes stored per-task state
   that must be audited, rescheduled, and recalculated with the task, which is a materially larger
   change than it appears.
3. **Professional review of equal-access permissions, privacy, compliance, retention, and security
   governance** [E-043, C-002]. *Blocks*: any operational pilot and any use of real data.
   *Either way*: confirming equal access leaves V1 as specified; requiring restrictions introduces
   role-based access control, which changes who may act but not what the actions mean.
4. **Erasure obligations versus an append-only Audit History** — this is the sharpest item on the
   list and is called out separately because it is structurally expensive to reverse. This change
   specifies Audit History entries as immutable once written, enforced by withholding `UPDATE` and
   `DELETE` privileges from the application's database role, and specifies that records are never
   permanently deleted. If professional review establishes an erasure or right-to-be-forgotten
   obligation, those two properties conflict with it directly. *Blocks*: any use of real personal
   data. *Either way*: confirming the append-only model leaves the design as specified; an erasure
   obligation requires a deletion or redaction path into the audit store, which means revisiting
   the database-privilege control, ADR-0001, and the never-deleted rule together — not a
   configuration change.
5. **Audit History scope confirmation** — discovery contains an unreconciled contradiction between
   the stakeholder's stated preference for not recording wrong input and the proposed broad change
   history [E-030, C-002]. The approved product direction specifies broad Audit History, and this
   change has since hardened it: coverage is complete by default with a closed, exhaustive
   seven-item exclusion list. *Blocks*: nothing in this slice. *Either way*: adopting the
   stakeholder's narrower preference now means reopening a closed exclusion list and recording a
   human decision for each addition, not flipping a setting — which is the point of closing the
   list.
6. **Referral Relationship requiredness** — `docs/product/journeys.md` enumerates the relationship
   values for a Client Referral Source and `CONTEXT.md` defines the term, but the
   required-attribution table lists only "selection of an existing client". This change specifies
   that a Referral Relationship is recorded per Client Referral Source, defaults to `N/A`, and does
   **not** form part of a Referral Source's completeness, so it never blocks activation. Whether an
   explicit relationship value should become required for activation is deferred.
7. **Audit History CSV download** — `docs/product/journeys.md` describes a CSV download option on
   the contextual `Audit history` action at the RMS-release level, while the slice's page surface
   excludes reports and CSV export. This change specifies contextual Audit History access without a
   CSV download. *Blocks*: nothing. *Either way*: adding it is a small addition to an existing
   view, but it is the first export path in the platform and therefore the first place export
   provenance and any retention rule would have to apply.
8. **Manual task creation** — the RMS-release Tasks page allows creating manual tasks from a
   Referral, Household, Event, or COI. Neither document's first-slice scope lists manual task
   creation, so this change specifies only generated tasks plus the linkage invariant that every
   task belongs to one of those records. Whether manual task creation is exercised in this slice
   is deferred.
9. **Global navigation search narrowed to Households and Referrals** — `docs/product/journeys.md`
   describes a persistent global search in the main navigation covering Households and members,
   Referrals, COIs, Events, and tasks. This change specifies search over Households and Referrals
   only, because COIs, Events, and tasks are present in this slice only as minimal reference and
   generated records. *Blocks*: nothing. *Either way*: keeping the narrowing means an evaluator
   finds COIs, Events, and tasks through their own pages; restoring the full scope makes search the
   primary navigation surface, which changes the shape of the main navigation rather than adding a
   filter.
10. **Marking evaluator-entered records, or warning in-product** — ESCALATED, not resolved here.
   Only seeded records are required to be clearly synthetic, so a record an evaluator types
   carries no marker, and the approved product decision is that the synthetic-only rule is
   documented rather than shown in-product (`docs/product/vision.md` line 84,
   `docs/product/journeys.md` line 25). Nothing in the build therefore deters someone from typing
   real client data into it. This is an approved-decision-versus-data-protection conflict and
   needs a human decision between two options: (a) every record in the evaluation environment,
   including evaluator-entered records, carries an evaluation marker; or (b) an unobtrusive
   environment banner is accepted as distinct from the "real data warning" the firm declined.
   This change picks neither.
11. **Who owns the pilot precondition list** — this change names several preconditions for leaving
   evaluation: real authentication and security logging, a completed security threat model
   (`docs/security/threat-model.md` exists but is empty), operational backup and recovery, and the
   professional review in items 3 and 4. What needs a human decision is not the contents of any one
   of them but who owns that list, keeps it current, and confirms it is satisfied before production
   deployment is enabled. This change does not author any of those artifacts.

### Recorded, not a decision for this slice

- **Evaluation success measures** — quantitative baselines, targets, evaluation periods, and
  acceptance measures remain to be established [E-003, E-007, E-011]. This is a post-pilot note:
  the firm expects to set these after operating experience, so nothing in this slice waits on it.
