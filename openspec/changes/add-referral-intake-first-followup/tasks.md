# Tasks — evaluation-only Referral intake and first follow-up

Each numbered group is one vertical checkpoint: it ends with behavior an evaluator can exercise and
evidence someone else can re-run. Every task states how it is verified. All test data is synthetic
and clearly marked; no real firm, client, or financial data is used anywhere.

Test evidence is named per behavior rather than mandating a global TDD cycle: pure calculation and
decision logic gets table-driven unit tests; every all-or-nothing transition gets an integration
test against PostgreSQL asserting both the success and the failure shape; each evaluation-success
bullet gets one end-to-end walkthrough.

## 1. Plan approval and foundation

- [ ] 1.1 Do not start implementation until a human records `PLAN APPROVED` in the draft PR; verify by linking the approving comment in the PR description before task 1.2 begins
- [ ] 1.2 Create the monorepo skeleton (`apps/web`, `apps/api`, `packages/contracts`, domain module directories per `design.md` D1) and verify `npm install` and a workspace-wide type-check succeed with no application behavior yet
- [ ] 1.3 Create `scripts/verify.sh` running type-check, lint, unit tests, integration tests, and `openspec validate --all --strict`; verify it exits non-zero when any step fails and zero on the empty skeleton (`AGENTS.md` requires this script and it does not exist in the repository today)
- [ ] 1.4 Add the PostgreSQL migration runner and the first empty migration; verify migrations apply and re-apply idempotently against a scratch database
- [ ] 1.5 Add module-boundary enforcement (lint rule or dependency check) asserting that domain modules import no HTTP, React, or database framework and that the dependency direction in `design.md` D1 has no cycles; verify the check fails on a deliberately introduced violation and passes on the skeleton
- [ ] 1.6 Add the boundary check asserting `apps/api` declares no outbound HTTP, mail, or third-party integration dependency (`specs/evaluation-environment/spec.md` — No upload, import, or external integration); verify it fails when such a dependency is added

## 2. Firm Time Zone and Firm Business Day scheduling

- [ ] 2.1 Implement the `scheduling` module: Firm Time Zone handling, Firm Business Day calendar lookup, business-day arithmetic, 5:00 p.m. due time, and 9:00 a.m. reminder time; verify with table-driven unit tests covering every scenario under "Firm Time Zone governs business dates and times", "Firm Business Day calendar", and "Task due time" in `specs/tasks-and-scheduling/spec.md`, including a daylight-saving transition date
- [ ] 2.2 Implement the first-follow-up default due-date rule (Friday of the same calendar week, shift to the nearest preceding Firm Business Day, three-Firm-Business-Day floor, activation-day exclusion, after-hours and non-business-day activation); verify with unit tests covering all five scenarios under "First follow-up task default due date"
- [ ] 2.3 Implement `earliestSelectableDueDate` and the default next-stage due date (three Firm Business Days); verify with unit tests covering "Manual rescheduling constraints" including the before/after 5:00 p.m. boundary
- [ ] 2.4 Verify the whole module has no database, HTTP, or React dependency by running its unit tests with no database available

## 3. Audit History and the transactional spine

- [ ] 3.1 Create the `audit_entry` table as append-only with the application role granted only `INSERT` and `SELECT`; verify an attempted `UPDATE` and `DELETE` are refused by the database
- [ ] 3.2 Implement the `audit` module interface (`recordChange`, `historyForRecord`) storing previous value, new value, acting Platform User, timestamp, and any required reason; verify with unit tests over entry content per `specs/audit-history/spec.md` — "Audit History entry content"
- [ ] 3.3 Implement the application-service transaction pattern from `design.md` D2 so a change and its audit entry share one transaction; verify with an integration test that forces the audit insert to fail and asserts the business change is not applied and can be retried to apply exactly once ("Audited changes are all-or-nothing")
- [ ] 3.4 Verify that record views, validation failures, and rejected stale saves produce no Audit History entry, with integration tests per "What Audit History does not record"

## 4. Identity, Associates, and the firm calendar

- [ ] 4.1 Implement the `associate` record and the fixed `Evaluation User` development identity injected at the single API boundary point (`design.md` D8); verify with an integration test that every audited change records the development identity as actor
- [ ] 4.2 Implement Associate create, edit, and active status plus selectability rules; verify with tests covering "Synthetic Associates are selectable for work" in `specs/associates-and-identity/spec.md`
- [ ] 4.3 Implement the refusal to deactivate an Associate who owns a non-closed Referral or an Open Task, and success after reassignment; verify with integration tests covering all three scenarios under "An Associate owning work cannot be deactivated"
- [ ] 4.4 Implement firm holiday and closure calendar maintenance with Audit History, affecting future calculations only; verify with an integration test that an existing task's due date is not recalculated after a calendar change
- [ ] 4.5 Deliver the Associates page and verify by walkthrough that an evaluator can add a synthetic Associate and a closure day and see both in contextual Audit History

## 5. Households, members, and duplicate prevention

- [ ] 5.1 Implement Household and Household Member records, display-name suggestion and editing, and the optional mailing address; verify with tests covering "Household and Household Member identity" and "Household display name" in `specs/households/spec.md`
- [ ] 5.2 Implement Client Number uniqueness with a database unique constraint including inactive records; verify with an integration test asserting a concurrent duplicate insert is refused by the constraint, not only by the pre-check
- [ ] 5.3 Implement normalized phone/email comparison and the exact-match creation block with no override, including inactive records; verify with tests covering formatting-insensitive phone matching and case-insensitive email matching
- [ ] 5.4 Implement the exact-name possible-duplicate warning requiring explicit acceptance, retaining the accepting Platform User and timestamp; verify with tests for both accepted and not-accepted paths and the resulting Audit History entry
- [ ] 5.5 Implement shared contact information within one Household as a non-duplicate; verify with a test creating one Household whose two members share an email address
- [ ] 5.6 Implement inactive-Household reuse and reactivation; verify with an integration test that a matching inactive Household is reactivated with members and history intact while a `Discarded Draft` stays inactive
- [ ] 5.7 Implement Household optimistic concurrency (`design.md` D5); verify with a concurrency integration test asserting the stale save is rejected, nothing is overwritten, and nothing is audited
- [ ] 5.8 Implement Pre-existing Client Household setup with its required fields and the `Unknown` Client Start Date; verify with tests asserting no Referral and no client-appreciation task is created
- [ ] 5.9 Deliver the Households page and Household detail with active-by-default search; verify by walkthrough that an evaluator can create a Pre-existing Client Household, hit a duplicate block, accept a name-match warning, and open contextual Audit History

## 6. Minimal Event and COI reference records

- [ ] 6.1 Implement the minimal Event record (name and date) and the minimal COI person record (name plus at least one phone or email); verify with tests covering required-field rejection in `specs/reference-records/spec.md`
- [ ] 6.2 Implement reference-record duplicate blocking (exact Event name-and-date, exact COI phone or email, matching COI name) with no override and no merge, backed by constraints where the rule is an absolute block; verify with integration tests including the same Event name on a different date
- [ ] 6.3 Implement reactivation of a matching inactive Event or COI instead of recreation; verify with an integration test asserting no new record is created
- [ ] 6.4 Implement deactivation preserving existing Referral attribution and removing the record from new source selection, plus inactive search and reactivation; verify with tests covering "Deactivation preserves existing attribution"
- [ ] 6.5 Deliver the minimal Events and COIs pages; verify by walkthrough that an evaluator can create, deactivate, find, and reactivate a synthetic Event and COI

## 7. Draft Referral intake

- [ ] 7.1 Implement the Referral record, the six Referral Source types with their required attribution, and the Referral Relationship on Client sources; verify with tests covering the attribution table and the temporary-COI-name refusal in `specs/referrals/spec.md`
- [ ] 7.2 Implement `Save Draft` with no autosave, the unsaved-changes warning, and the minimum Draft content rule with missing-activation-information visibility; verify with tests plus a walkthrough asserting no record exists before the explicit save
- [ ] 7.3 Implement Referral Received Date defaulting, backdating, future-date rejection, and retention when a later source is added; verify with unit and integration tests for all three scenarios
- [ ] 7.4 Implement all-or-nothing creation of a new Household, its members, its sources, and the Draft Referral from one intake save, preserving entered values on failure; verify with an integration test that forces a mid-save failure and asserts nothing was created and retry creates exactly one of each
- [ ] 7.5 Implement the one-non-discarded-Referral-per-Household rule and the refusal to give a client Household its own Referral; verify with integration tests for both refusals
- [ ] 7.6 Deliver the `New Referral` single-form page and the Referrals list with its default `Draft` and `In process` filter; verify by walkthrough that an evaluator can search Households, create one inline, and save a Draft that names its missing activation information

## 8. Activation and first follow-up task

- [ ] 8.1 Implement the `task` record with statuses `Open`, `Completed`, `Cancelled`, derived `Overdue`, and its mandatory link to a business record; verify with tests covering "Task statuses and derived Overdue" in `specs/tasks-and-scheduling/spec.md`
- [ ] 8.2 Implement activation as one atomic transition validating every invariant together and creating exactly one first follow-up task; verify with integration tests covering aggregated validation errors, the inactive-owner rejection, task-creation failure leaving the Referral `Draft`, and retry producing exactly one task and one audit entry
- [ ] 8.3 Implement the continuing `In process` invariants including atomic replacement and the refusal to return to `Draft`; verify with integration tests for last-contact-method removal, atomic replacement, and the `Draft` refusal
- [ ] 8.4 Implement Referral optimistic concurrency; verify with a concurrency integration test mirroring task 5.7
- [ ] 8.5 Implement Referral Owner change leaving the existing task assignee unchanged, the combined reassignment operation, and the new-owner default for later generated tasks; verify with integration tests for all three scenarios
- [ ] 8.6 Deliver Referral detail with summary, sources, outreach progress, tasks, activity history, and the `Audit history` action; verify by walkthrough that an evaluator can activate a Draft and see exactly one correctly scheduled first follow-up task

## 9. Tasks page, reminders, and rescheduling

- [ ] 9.1 Implement derived reminder indicator, overdue visibility, and the immediate-appearance rule when the reminder time has already passed (`design.md` D4); verify with tests covering every scenario under "Persistent reminder indicator" and the absence of dismiss and snooze
- [ ] 9.2 Implement reminder visibility for both the task assignee and the current Referral Owner; verify with an integration test where the two are different Associates
- [ ] 9.3 Implement manual rescheduling constraints and their Audit History; verify with tests covering past dates, non-business days, the 5:00 p.m. boundary, and the recorded previous and new dates
- [ ] 9.4 Implement the derived `Due on non-business day` flag and its Needs Attention entry, cleared only by a manual reschedule; verify with an integration test that adds a closure day under an existing task and asserts the flag appears, the due date is unchanged, and both changes are audited
- [ ] 9.5 Deliver the Tasks page with its four sections, ordering, filters, `Evaluation User` default filter, `Completed today` section, and no generic completion checkbox; verify by walkthrough against "Tasks page organization"

## 10. Follow-up Call attempts

- [ ] 10.1 Implement attempt recording with a non-future date and time, caller defaulting to the acting identity's Associate, separate recorder and entry timestamp, outcome, and optional note; verify with tests covering the default, the on-behalf-of case, and the future-date rejection in `specs/referral-outreach/spec.md`
- [ ] 10.2 Implement `No answer` and `Voicemail` keeping the same task Open with a required future next due date defaulting to three Firm Business Days; verify with integration tests including the missing-next-due-date rejection and the audited previous due date
- [ ] 10.3 Implement the `Two-way conversation` requiring at least one participating Prospect and a result, with `NFAR — no response` unavailable; verify with tests for the missing-participant rejection and the unavailable option
- [ ] 10.4 Verify by walkthrough that an evaluator can record repeated unsuccessful attempts, reschedule and reassign the same Open Task, and see every attempt retained

## 11. Stage completion, skip, and the handoff task

- [ ] 11.1 Implement `Continue outreach` completing the Follow-up Call stage and creating exactly one `Discovery package sent` task atomically; verify with integration tests covering success, forced failure leaving the stage Open with nothing recorded, and retry producing exactly one attempt and one task
- [ ] 11.2 Implement skipping the stage with a required reason, cancelling the unperformed task, and creating the next task in the same transaction; verify with integration tests including the missing-reason rejection and the Audit History cause
- [ ] 11.3 Implement the `Discovery package sent` task with its Referral Owner default, three-Firm-Business-Day due date, reassignment, rescheduling, reminders, and a server-side refusal of its completion command in addition to the disabled UI action; verify with an API-level test asserting the completion command is refused
- [ ] 11.4 Verify by walkthrough that both the completion path and the skip path produce exactly one `Discovery package sent` task clearly identified as the next-slice boundary

## 12. NFAR closure and reopening

- [ ] 12.1 Implement the three NFAR dispositions, closure through the task, and closure from Referral detail cancelling the unperformed task, each atomic and creating no next task; verify with integration tests for both paths and for a forced closure failure changing nothing
- [ ] 12.2 Implement the `NFAR — no response` attempt requirement, excluding attempts marked `Entered in error`; verify with tests for zero attempts, one attempt, and an `Entered in error` attempt
- [ ] 12.3 Implement reopening into the Follow-up Call stage with a required reason and an optional additional Referral Source, reopening the same task with prior attempts, reassigning to the current Referral Owner, and setting a due date three Firm Business Days after reopening; verify with integration tests including the missing-reason rejection and the absence of later-stage options
- [ ] 12.4 Verify by walkthrough that an evaluator can exercise every valid NFAR closure path and reopen the Referral

## 13. Correction, reversal, and discard

- [ ] 13.1 Implement attempt correction requiring a reason and leaving task and Referral state untouched; verify with integration tests asserting unchanged workflow state and a complete audit entry
- [ ] 13.2 Implement `Entered in error` marking that retains and visibly flags the attempt without deleting it; verify with tests asserting continued visibility and exclusion from workflow evidence
- [ ] 13.3 Implement the refusal to invalidate an attempt that a later transition depends on; verify with integration tests for the NFAR-supporting attempt and for the conversation supporting an active handoff
- [ ] 13.4 Implement reversal of an incorrect Follow-up completion as one atomic operation (cancel the handoff task, reopen the original task with earlier valid attempts, assign to the current Referral Owner with a three-Firm-Business-Day due date, mark the conversation `Entered in error`); verify with an integration test asserting all effects together and a forced-failure test asserting none of them
- [ ] 13.5 Implement `Discarded Draft` with extra confirmation and a required reason, no restore action, and no permanent deletion; verify with tests including the refusal to discard an `In process` Referral
- [ ] 13.6 Implement the discard cascade to a Household created by that intake, all-or-nothing, leaving pre-existing Households unchanged and inline Events and COIs active; verify with integration tests for all three cases plus a forced cascade failure changing nothing
- [ ] 13.7 Implement the later-intake path that reactivates the Household and creates a new Draft Referral; verify with an integration test asserting the earlier `Discarded Draft` stays inactive and exactly one non-discarded Referral results

## 14. Contextual Audit History surface

- [ ] 14.1 Deliver the `Audit history` action on Household, Referral, task, Associate, Event, and COI detail views with no inline history in workflow views and no global audit page; verify by walkthrough against `specs/audit-history/spec.md` — "Contextual per-record access"
- [ ] 14.2 Verify by integration test that every change type listed under "What Audit History records" produces an entry with previous value, new value, actor, timestamp, and any required reason
- [ ] 14.3 Implement read-only availability while audit writes are failing and verify with an integration test that records remain viewable while writes are rejected

## 15. Evaluation environment boundary

- [ ] 15.1 Author the synthetic baseline seed data modelled on the sanitized spreadsheets' structures without copying their rows, and the reset script that runs outside the application; verify by running reset and asserting the environment matches the baseline exactly
- [ ] 15.2 Verify by test that reset is not reachable as a Platform User action and produces no Audit History entry
- [ ] 15.3 Implement the production deployment guard that refuses to run and names the evaluation-only boundary; verify by executing the guarded path and asserting the refusal
- [ ] 15.4 Add the fixture check asserting no rows copied from the sanitized spreadsheets appear in seed data or tests; verify it fails on a deliberately introduced copied row
- [ ] 15.5 Document the synthetic-data-only, isolated, non-public operating rule in the repository (not as an in-product warning); verify the documentation exists and that no in-product data warning is rendered
- [ ] 15.6 Verify by walkthrough that upload, import, email, and external-integration actions are absent from every page

## 16. Verification, review, and archive

- [ ] 16.1 Run `./scripts/verify.sh` and verify every check passes; report which checks ran, their results, any skipped checks, and residual risk, per `AGENTS.md`
- [ ] 16.2 Run one end-to-end walkthrough per bullet of the "Evaluation success" list in `docs/product/journeys.md` and verify each is demonstrable using only synthetic data
- [ ] 16.3 Run `openspec validate --all --strict` and verify it passes with no findings
- [ ] 16.4 Confirm the two proposed ADRs in `design.md` (audit-in-transaction, date storage) are either written and accepted under `docs/adr/` or explicitly declined by a human; verify the decision is recorded in the PR
- [ ] 16.5 Obtain independent final review from the agent that did not author the code, with fresh context and read-only access; verify every accepted finding is fixed by the original author and re-verified
- [ ] 16.6 Archive the change and inspect the resulting canonical spec diff under `openspec/specs/` before merge; verify the diff contains only the behavior specified here
