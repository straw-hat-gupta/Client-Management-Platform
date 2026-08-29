# Product Vision

## Document Status

- Status: Working draft
- Last updated: 2026-08-28
- Approval status: Approved for evaluation build; flow validation pending hands-on UI review
- Decision authority: The commissioning firm lead has final approval.
- Current planning focus: RMS-first release and its first vertical slice

## Evidence Base

This vision is grounded in:

- The source-traceable interview evidence in [discovery-notes.md](./discovery-notes.md)
- Tentative decisions from collaborative discovery sessions held on 2026-08-17, 2026-08-20, 2026-08-23, and 2026-08-27
- Sanitized `RMS_clean.csv`, `COI_clean.csv`, and `Events_clean.csv` files, used only as evidence of current fields and working practices

Current spreadsheet structure does not silently define future platform behavior. Interview evidence and collaborative-session decisions remain subject to firm-lead approval and appropriate professional review.

## Vision

Create a secure internal client-management platform that gives the firm one understandable place to coordinate relationship work, see what requires attention, preserve accountable history, and produce reliable operational information.

The platform should replace disconnected spreadsheet-driven coordination incrementally. The first delivery focus is RMS because referral follow-up and the resulting client-appreciation tasks form a coherent slice with immediate operational value.

## Problem Context

The current process distributes related information and workflow checkpoints across spreadsheets. Staff repeat entry, search multiple places for context, manually determine next actions, and risk missing follow-up or maintaining conflicting values [E-001, E-002, E-024, E-029].

The sanitized spreadsheets reinforce that the present records mix several concerns in single rows: identity, referral source, ownership, workflow milestones, status, next action, appreciation activity, and reporting values. They also use dates, booleans, blanks, `N/A`, and status markers inconsistently. The future platform should preserve supported business meaning without copying those inconsistencies as product rules.

## Intended Users and Authority

All firm team members are intended internal users [E-004]. Each V1 user signs in through an individually identifiable account linked to an Associate. Anonymous access is not supported. The working V1 model gives every authenticated platform user equal access to business records and actions, including Audit History; role-based business restrictions and user-specific visibility are deferred. Granting or revoking sign-in access is the exception and is restricted to the firm lead or a designated identity administrator. Professional review of privacy, compliance, retention, and security governance remains required.

The commissioning firm lead owns final decisions for product scope, workflow rules, permissions, professional-policy decisions, and release acceptance. Collaborative-session decisions remain tentative until that approval occurs.

## Desired Outcomes

- Make every active Referral, responsible owner, current stage, open task, due date, and next action easy to find.
- Reduce missed work through task creation, reminders, overdue visibility, and explicit workflow transitions.
- Separate Household identity, Referral history, task responsibility, and client-appreciation work while keeping their relationships visible.
- Preserve correction, reassignment, status, merge, export, and other material history for audit access.
- Support timely search, operational work lists, management reporting, and CSV export.
- Replace spreadsheets one independently deployable workflow at a time rather than recreating every spreadsheet simultaneously.

These outcomes are directional. Quantitative baselines, targets, evaluation periods, and acceptance measures remain TBD [E-003, E-007, E-011].

For the first draft, practical success means the firm can stop adding new RMS work to the spreadsheet, manage active Referrals and client tasks in the platform, and reliably find each record's current owner, status, next task, and contextual Audit History. Quantitative performance measures may be established after the firm has operating experience with the platform.

## Product Principles

### Human-controlled workflow

The platform may create tasks, reminders, summaries, and prompts, but staff make relationship decisions. It does not infer dispositions, automatically declare `NFAR — no response`, or send automatic client email in the RMS-first slice.

### Explicit state and ownership

Draft, active, closed, merged, converted, former-client, and waiting states should be distinguishable. Overall workflow ownership and individual task assignment are separate and historically attributable.

### Preserve meaning, not spreadsheet shape

Spreadsheet columns are discovery evidence. The platform should use clear domain records and workflow concepts rather than reproduce ambiguous blanks, mixed data types, page-specific columns, or overloaded status labels.

### Auditability without cluttering daily work

Daily RMS views show current values and work. Audit History separately preserves previous and new values, actor, and timestamp, subject to approved professional policy and documented exceptions.

### Incremental delivery

Each planned change should provide one usable vertical slice with an understandable disable or rollback path. Later workflows should not be pulled into RMS merely because a current spreadsheet mentions them.

The first draft prioritizes getting a basic, usable spreadsheet replacement operating quickly so the firm can iterate from real workflow experience. Fine-grained interface behaviour and uncommon edge cases remain deferred unless they are necessary for data integrity, security, auditability, or the core RMS flow.

## Delivery Shape

The **RMS-first release** is the complete product direction described below. It is not one implementation change. Delivery should be divided into independently deployable vertical slices.

The first vertical slice is an **evaluation-only Referral intake and first follow-up** build. Using clearly synthetic data and a clearly marked development identity, an evaluator can find or create a Household, save a Referral as Draft, attribute any of the six Referral Source types, activate the Referral as `In process`, and work the first follow-up task through attempts, successful contact, an explicit skip, or an NFAR disposition. Successful contact or a skip creates the `Discovery package sent` task as the handoff to the next slice. The Referral and tasks appear in their respective lists and detail views with contextual Audit History.

The firm lead has approved creating this evaluation build. The flow details remain subject to validation and revision after the firm lead can exercise the initial UI; this is not operational or production acceptance.

Because the slice has no real authentication, it runs only locally or in an isolated private evaluation environment, is never exposed publicly or to an untrusted network, and uses no real firm/client data. It has no file upload, import, email, messaging, or other external integration, and production deployment remains disabled until authentication and security logging are added. The data restriction is documented rather than presented as an in-product warning.

The fixed development identity is linked to a clearly synthetic `Evaluation User` Associate. That identity appears as the default audit actor, call performer, and Tasks filter while evaluators may assign work or record activity for other synthetic Associates.

Evaluation data is disposable and may be reset to a known synthetic baseline through an environment operation rather than a Platform User feature. Evaluation data carries no backup, retention, or migration promise; operational backup and recovery must be established before real client use.

The slice is successful when an evaluator can demonstrate synthetic Associate/calendar setup; minimal Client, Event, and COI source setup; Draft creation, editing, discarding, and discovery; valid activation with exactly one scheduled task; unsuccessful and successful follow-up paths; stage skipping; NFAR closure and reopening; duplicate and stale-edit protection; reassignment; contextual Audit History; and environment reset, while upload, external integration, and production deployment remain disabled.

Its page surface is limited to Referrals and detail, Tasks, Households, minimal Events, minimal COIs, synthetic Associates, and contextual Audit History. Reports, appreciation, full Event/COI management, import, and a global Audit page remain absent.

Associate scope in the evaluation slice is limited to synthetic staff records that can be selected as owners, assignees, or activity participants. The evaluation identity may configure the firm holiday and closure calendar needed to exercise Firm Business Day calculations. Real sign-in, access provisioning and revocation, administrative permission enforcement, profiles, teams, job titles, roles, and workload summaries remain deferred.

All Referral Source types work in the slice. Client, Event, and COI attribution use only minimal reference records: existing client Household/member identity, Event name and date, and COI person identity. Broader client, Event, and COI workflows remain deferred.

From Referral intake, staff may create a minimal Event from its name and date, create a minimal COI person from their name plus phone or email, or open the Pre-existing Client Household setup path. The broader release's temporary-COI name and resolution-task behaviour is deferred from this slice.

The slice exercises manual creation of synthetic Pre-existing Client Households so Client Referral Sources can be selected. Synthetic seed data may follow the sanitized spreadsheets' structures and value patterns, but sanitized spreadsheet rows are not copied into fixtures and an import pipeline remains outside the slice.

Duplicate prevention is part of the slice rather than a later cleanup capability: exact phone, email, and Client Number conflicts block duplicate creation, while an exact name match requires explicit acceptance whose user and timestamp are retained. Household merging remains deferred.

A client Household may be selected as a Client Referral Source for a different prospective Household, but it cannot itself receive a Referral. Existing-client potential business remains in the deferred Pillar List workflow.

Referral activation is one atomic business transition: the Referral moves to `In process` only when exactly one first follow-up task and its due date are created successfully. A failed attempt leaves the Referral Draft and may be retried without creating duplicate tasks.

Concurrent edits do not silently overwrite newer saves. A user saving an outdated Referral or Household version receives a conflict, reloads the current record, and reapplies the intended change; Audit History contains only successful saves.

Creating a new Household through Referral intake is atomic with saving its members, sources, and Draft Referral. Failure creates none of those records and preserves the user's entered form values for correction and retry. Standalone setup of a Pre-existing Client Household remains a separate intentional action.

A Draft exists only after an explicit successful `Save Draft` action; the first slice does not autosave business records. Leaving a form with unsaved changes produces a warning, and successful subsequent saves appear in Audit History.

A Draft must identify an existing Household or create a new Household with at least one named member. Contact information, Referral Source, Referral Received Date, and Referral Owner may remain incomplete until activation; the Draft creates no task and identifies its missing activation information.

Activation validates the complete transition at once: a named Household Member, at least one phone number or email, a complete source with its source-specific information, a non-future Referral Received Date, an active Referral Owner, and no blocking duplicate conflict. Failure leaves the Referral Draft, preserves entered values, creates no task, and presents all validation problems together.

An `In process` Referral must continue satisfying those activation invariants. Staff may replace required values atomically but cannot remove its last contact method or complete source, leave it ownerless or assigned to an inactive owner, or future-date its Referral Received Date. Active Referrals do not move back to Draft to permit incomplete data.

Changing a Referral Owner does not silently change the assignee of an existing task. The user may reassign that task in the same operation; otherwise it keeps its assignee, while future generated tasks default to the new owner and reminders continue to both responsible people.

For the later operational pilot, sign-in access may be revoked immediately even while an Associate owns work. Revocation preserves the Associate and assignments but flags that work for reassignment; the Associate cannot be marked inactive until reassignment is complete.

Contextual Audit History contains successful business and administrative changes, not record views, unsaved input, rejected saves, login failures, or internal errors. Failed logins and system failures belong in restricted technical/security logs.

Any change requiring Audit History succeeds with its history entry or fails entirely. The platform does not accept an unaudited change for later repair; read-only access may remain available when audit writes fail.

All business date and time behaviour uses the `America/Edmonton` Firm Time Zone, including daylight-saving rules, regardless of evaluator computer settings. Date-only fields remain firm-calendar dates.

First-slice reminders are persistent derived task indicators rather than dismissible notifications. They appear at 9:00 a.m. Firm Time one Firm Business Day before due, remain until completion, cancellation, or rescheduling, and move to overdue visibility after the due time; email, push, dismiss, snooze, and separate notification records are deferred.

Manual rescheduling accepts only Firm Business Days that have not passed. The current day is selectable before 5:00 p.m. Firm Time; afterward the earliest date is the next Firm Business Day. No reason is required, while prior date, new date, actor, and timestamp remain in Audit History.

If task creation or rescheduling occurs after the calculated reminder time has already passed, the reminder indicator appears immediately when the save succeeds.

When a later firm-calendar change makes an existing due date a closure day, the task is not moved silently. It is flagged `Due on non-business day`, appears in Needs Attention, and remains until staff manually select a valid date; both changes remain in Audit History.

Staff may correct a follow-up attempt only with a required correction reason and retained old/new values. A wholly false attempt is marked `Entered in error`, remains in history, and does not count toward `NFAR — no response`; attempt correction does not silently change related task or Referral state.

An attempt cannot be invalidated while a later state transition depends on it. Staff must first explicitly reverse the dependent NFAR or Follow-up completion; the platform never silently rewrites downstream state during activity correction.

Reversing an incorrect Follow-up completion requires confirmation and a reason, cancels the unexecuted Discovery-package task, reopens the original Follow-up task under the current Referral Owner with a three-Firm-Business-Day default, and marks the false conversation `Entered in error`; every transition remains in Audit History.

An NFAR Referral may be reopened into the Follow-up Call stage with a required reason. The same task, attempts, and dates are retained; the task moves to the current Referral Owner with a new default due date three Firm Business Days after reopening. Later-stage reopening remains outside the slice.

`NFAR — no response` requires at least one recorded contact attempt but no fixed number of attempts. The responsible staff member decides case by case when to stop, and the attempts and disposition remain in history.

Recording voicemail or no answer keeps the follow-up task Open and requires its next future due date. That date defaults to three Firm Business Days after the attempt but may be changed; the previous due date remains in Audit History.

Each attempt records its non-future date and time, the Associate who made it, its outcome, an optional note, and the user and timestamp that entered it. The caller defaults to the signed-in user but may differ from the recorder; unsuccessful attempts also record the next due date.

A two-way conversation identifies at least one participating Prospect and results in continued outreach, `NFAR — not interested in our services`, or `NFAR — no business opportunity`. Continued outreach creates the Discovery-package task; `NFAR — no response` is unavailable for a completed conversation.

Completing or skipping Follow-up Call and creating exactly one `Discovery package sent` task is atomic and retry-safe. Failure leaves Follow-up Call Open and preserves entered form values; NFAR instead closes the Referral atomically without creating a next task.

`Skipped` is an outreach-stage outcome with a required reason, not a Task status. Skipping Follow-up Call marks its unperformed task `Cancelled` and creates the next task, with Audit History preserving why.

The generated `Discovery package sent` task is visible, assignable, reschedulable, and included in reminders and Audit History. Its completion action is disabled and clearly identified as the boundary to the next evaluation slice.

Mistaken or abandoned Drafts are not deleted. Staff may place them in the `Discarded Draft` state with confirmation and a reason; they leave active lists while retaining their data and Audit History, and cannot be restored in the first slice.

When a discarded Draft was the only relationship for a newly created Prospective Household, that Household also becomes inactive and leaves active lists. Existing Households and Households with other linked relationships remain unchanged; the discard updates the applicable records together or neither.

A later legitimate intake for the same Prospects reuses and reactivates that Household but creates a new Draft Referral. The earlier Discarded Draft remains inactive, and the Household may have only one non-discarded Referral at a time.

Minimal Event and COI records created during intake remain active when a Draft is discarded because they are independent, reusable Referral Sources. Staff deactivate a mistaken Event or COI separately.

Deactivation preserves existing Referral attribution but removes the Event or COI from new source selection by default. Users may search inactive references and reactivate them; both transitions remain in contextual Audit History.

The slice does not permit duplicate Event or COI references. An exact Event name-and-date match or exact COI phone/email match blocks creation and directs staff to the existing record. A matching COI name also blocks creation until staff select the existing person or provide a distinct phone or email; there is no override or merge capability.

Duplicate checks include inactive records. Matching inactive Households are reused and reactivated; matching inactive Events or COIs are reactivated rather than recreated. Discarded Drafts remain inactive under their separate rule.

Every member of a Prospective Household is treated as a Prospect on its active Referral in the first slice. Call participation is recorded per member, while a Client-source relationship applies to the prospective Household collectively; per-member Referral participation remains deferred.

The first slice stops before real authentication and access administration, use of real firm/client data, executing the `Discovery package sent` task, later outreach stages, conversion, client-appreciation tasks, Event management beyond source identity, COI management beyond source identity, reports, merging, and imports.

## Capability Direction

### RMS-first release

The RMS-first direction includes:

- Households and people
- Referral intake and sources
- Referral and client-Household ownership, plus task assignment
- Ordered but skippable outreach stages
- Repeat contact attempts, discovery packages, meetings, and dispositions
- Conversion and Client Start Date
- New-client and existing-client appreciation tasks
- Basic appreciation-event creation, invitation, response, attendance, cancellation, and rescheduling
- Minimal COI records required to identify COI Referral Sources, including expertise
- Minimal event-source records required to identify Event Referral Sources
- Task lists, search, requested reports, CSV export, and contextual Audit History

Detailed RMS behavior is maintained in [journeys.md](./journeys.md).

### Later workflow slices

- Full COI relationship management, including COI follow-up and action tracking
- Broader event management, including cost and potential/booked business outcomes
- Pillar List and existing-client opportunity management
- Investment Transfer and Transfer Reconciliation
- Insurance Business Monitor
- Focus 10
- Other workflows approved after RMS

## RMS-First Boundaries

The minimal COI and event capabilities exist only to support RMS source attribution and client appreciation. A V1 COI is an individual person, not an organization record. These capabilities do not bring the broader COI or event-management spreadsheets into the first slice.

V1 uses the Household as the continuing relationship record from prospect through client or former-client status. The Households page also serves as the initial client list; a separate Clients page and a conversion-time replacement record are not needed for the first draft.

The following remain deferred or unresolved:

- Automatic client email
- Role-based access control and user-specific visibility; V1 still requires individual authentication and applies one equal-access policy to every user
- Full COI, event-outcome, Pillar, transfer, insurance, and Focus 10 workflows
- Document storage
- Detailed spreadsheet-import planning
- Professionally approved retention, privacy, compliance, and security-governance behavior

## Readiness for the Next Planning Stage

The first evaluation slice is approved to proceed into OpenSpec planning. Its proposal must preserve the synthetic-only, isolated, non-production boundary and identify hands-on UI validation as the mechanism for revising flow details. Broader RMS-release decisions and operational authentication, privacy, compliance, retention, security governance, backup, and recovery remain outside this approval.
