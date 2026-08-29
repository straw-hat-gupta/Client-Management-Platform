# Product Journeys

## Document Status

- Status: Working draft
- Discovery sessions: 2026-08-17, 2026-08-20, 2026-08-23, 2026-08-27, and 2026-08-28
- Last updated: 2026-08-28
- Approval status: Approved for evaluation build; flow validation pending hands-on UI review
- Decision authority: The commissioning firm lead has final approval across product scope, workflows, permissions, professional-policy decisions, and release acceptance.
- Working authority: The current discovery participant may develop tentative decisions for firm-lead approval.
- Evidence boundary: These notes capture the collaborative discovery session and are not evidence extracted from source `I-001`.

## Tentative RMS-First Release

The proposed RMS-first release covers Referral intake, outreach, conversion, and the connected client-appreciation tasks. It includes minimal COI records for COI Referral Sources, minimal event-source records for Event Referral Sources, and basic appreciation-event handling required by RMS. Full COI relationship management, broader event cost and business-outcome tracking, and existing-client potential business remain separate later workflows.

The sanitized spreadsheets are current-state evidence only. Their columns and mixed value conventions do not automatically define future fields, states, or requirements.

## First Vertical Slice

The first slice is independently deployable only to an evaluation environment and covers Referral intake through the first follow-up stage:

The firm lead has approved creation of this evaluation build. Detailed flow acceptance remains pending hands-on validation of the initial UI and may result in revisions before any operational pilot.

- The build runs locally or in an isolated private evaluation environment, is not publicly exposed, accepts no file uploads or imports, has no external communication integrations, and cannot be deployed as production. The prohibition on real firm/client data is documented without an in-product warning.
- Its synthetic data may be reset to a known baseline through an environment operation outside normal Platform User actions and Audit History. No operational backup, retention, or migration promise applies to evaluation data.
- A clearly marked development identity operates the slice; real authentication and Platform User provisioning are not included.
- The development identity is linked to a synthetic `Evaluation User` Associate used as the default Audit History actor, call performer, and Tasks filter. Other synthetic Associates remain selectable for assignments and on-behalf-of activity entry.
- Synthetic Associates are visible and selectable as Referral Owners, task assignees, and activity participants.
- The evaluation identity maintains the holiday and closure calendar used to exercise Firm Business Day calculations; administrative permission enforcement is deferred.
- An Associate who owns a Referral or open task cannot be deactivated until that work is reassigned.
- The evaluator finds an existing Household or creates a Household and its members.
- The user creates and saves a Draft Referral.
- The user may attribute any of the six Referral Source types. Client, Event, and COI sources use minimal existing-client Household/member, Event name/date, and COI person reference records.
- Staff may manually establish Pre-existing Client Households for Client Referral Source selection without triggering new-client appreciation tasks.
- Referral intake may create a minimal Event inline from its name and date or a minimal COI person inline from their name plus at least one phone number or email. Temporary COI names and resolution tasks remain deferred to a later slice.
- Once minimum intake information and a Referral Owner are present, the user moves the Referral to `In process`.
- Activation automatically creates the first follow-up task using the agreed default scheduling rule.
- Staff record every follow-up attempt; voicemail or no answer leaves the same task Open and permits rescheduling or reassignment.
- A two-way conversation completes the Follow-up Call stage. Staff may instead skip it with a required reason.
- Successful contact or skipping creates the `Discovery package sent` task as the handoff to the next slice.
- Completing the follow-up as NFAR records the selected NFAR disposition, completes the task, and closes the Referral. Setting NFAR from Referral detail without performing the task cancels the task instead.
- An NFAR Referral may be reopened into the Follow-up Call stage with a required reason and an additional Referral Source when applicable. The same task reopens with prior attempts and dates, moves to the current Referral Owner, and receives a new default due date three Firm Business Days after reopening. Reopening into later stages remains outside the slice.
- `NFAR — no response` requires at least one recorded contact attempt but no fixed attempt count. The responsible staff member decides case by case when to stop, and prior attempts remain visible.
- Recording voicemail or no answer keeps the task Open and requires a future next due date. The date defaults to three Firm Business Days after the attempt but may be changed; the previous due date remains in Audit History.
- Each attempt records a non-future date and time, the Associate who made it, outcome (`No answer`, `Voicemail`, or `Two-way conversation`), an optional note, and the user and timestamp that entered it. The caller defaults to the signed-in user but may be changed when recording on another Associate's behalf; unsuccessful attempts also record the next due date.
- A two-way conversation requires at least one participating Prospect from the Household and a result: `Continue outreach`, `NFAR — not interested in our services`, or `NFAR — no business opportunity`. Continuing completes the task and creates the `Discovery package sent` task; `NFAR — no response` is unavailable because contact occurred. A note remains optional.
- Completing or skipping Follow-up Call and creating exactly one `Discovery package sent` task is atomic. Failure leaves Follow-up Call Open, preserves entered values, and permits retry without duplicate activities or tasks. NFAR instead closes the Referral atomically without creating a next task.
- `Skipped` is the Follow-up Call stage outcome and requires a reason. Its unperformed task becomes `Cancelled`; it does not introduce a `Skipped` Task status. Audit History identifies intentional stage skipping as the cancellation cause.
- The generated `Discovery package sent` task appears in Tasks and Referral detail with assignee, owner, due date, reminder, and Audit History. It may be reassigned or rescheduled, but its completion action is disabled and identified as the boundary to the next evaluation slice.
- The Referral and task appear in the Referrals and Tasks pages and expose contextual Audit History.
- Exact phone, email, and Client Number conflicts block duplicate creation. Exact name matches require explicit acceptance, with the user and timestamp retained.
- A client Household may be selected as another Household's Client Referral Source but cannot itself receive a Referral; that potential business belongs to the deferred Pillar List workflow.
- Activation is atomic: if first-task creation or due-date calculation fails, the Referral remains Draft, the user receives an error, and retry creates exactly one first follow-up task. Audit History records only the successful activation; technical failure details are not added to the business record.
- A save based on an outdated Referral or Household version is rejected rather than overwriting a newer save. The user is told to reload and reapply the intended change; only successful saves appear in Audit History.
- When Referral intake creates a new Household, saving the Household, members, sources, and Draft Referral is all-or-nothing. Failure creates none of them, keeps the user's entered form values available for correction, and permits retry. Pre-existing Client Household setup remains a separate action.
- Draft creation requires an explicit `Save Draft` action; V1 does not autosave business records. Navigating away with unsaved changes warns the user, and every successful later save updates the Draft and appears in Audit History.
- A Draft must select an existing Household or create a new Household with at least one named member. Contact information, Referral Source, Referral Received Date, and Referral Owner may remain incomplete. The Draft creates no task and clearly identifies the information still required for activation.
- Activation validates all required information together: a named Household Member, at least one phone number or email across the prospective Household, a complete Referral Source with its source-specific fields, a non-future Referral Received Date, an active Referral Owner, and no blocking duplicate conflict. Failure leaves the Referral Draft, preserves entered values, creates no task, and shows all validation problems together.
- An `In process` Referral must continue satisfying those invariants. A save may replace required values atomically but cannot remove its last contact method or complete source, leave it ownerless or assigned to an inactive owner, or future-date its Referral Received Date. It does not move back to Draft to permit incomplete data.
- Changing the Referral Owner leaves the existing first-task assignee unchanged. The reassignment action shows that assignee and permits changing both in one operation; future generated tasks default to the new owner, and reminders continue to both the task assignee and current Referral Owner.
- Contextual Audit History records successful Household/member, Referral, source, ownership, activation, task, duplicate-warning, Associate, and firm-calendar changes under the development identity. It excludes record views, unsaved input, validation failures, stale-save conflicts, and internal errors; authentication and security-event logging remain prerequisites for an operational pilot.
- A business or administrative change requiring Audit History succeeds with its history entry or fails entirely. The user may retry; the platform does not accept an unaudited change for later repair, although read-only access may remain available during an audit-write failure.
- Business dates and times use the `America/Edmonton` Firm Time Zone, including daylight-saving changes, regardless of evaluator computer settings. Date-only values remain firm-calendar dates.
- At 9:00 a.m. Firm Time one Firm Business Day before due, a task appears in the navigation indicator and reminder section. The indicator persists until completion, cancellation, or rescheduling and becomes overdue visibility after the due time. Dismiss, snooze, email, push, and separate notification records are outside the slice.
- Manual rescheduling accepts only Firm Business Days that have not passed. Today is selectable before 5:00 p.m. Firm Time; after that, the earliest date is the next Firm Business Day. No reason is required, and Audit History retains the previous date, new date, actor, and timestamp.
- If task creation or rescheduling occurs after its calculated reminder time has passed, the reminder indicator appears immediately upon the successful save.
- A later firm-calendar change does not silently move existing tasks. A task now due on an excluded date is flagged `Due on non-business day`, appears in Needs Attention, and remains until staff manually reschedule it; both the calendar and task changes remain in Audit History.
- Attempt corrections require a reason and preserve previous and new values, actor, reason, and timestamp. A wholly false attempt is marked `Entered in error` rather than deleted and does not satisfy the attempt requirement for `NFAR — no response`. Correction does not silently change related task or Referral state.
- An attempt cannot be invalidated when it is the only evidence supporting `NFAR — no response` or the two-way conversation that completed Follow-up Call while its Discovery-package handoff remains active. Staff must first explicitly reverse the dependent transition; downstream state is never rewritten silently.
- Reversing an incorrect Follow-up completion requires extra confirmation and a correction reason. It cancels the unexecuted `Discovery package sent` task, reopens the original Follow-up task with earlier valid attempts, assigns it to the current Referral Owner with a due date three Firm Business Days after reversal, marks the incorrect conversation `Entered in error`, and retains every transition in Audit History.
- Any Platform User may move a mistaken or abandoned Draft to `Discarded Draft` with extra confirmation and a required reason. The record leaves active lists by default, creates no task, preserves its Household and sources, and records actor, reason, and timestamp. It cannot be restored in the first slice and is not permanently deleted.
- If that Draft was the only relationship for a newly created Prospective Household, the Household also becomes inactive and leaves active lists while its members and history remain retained. An existing Household or one with other linked relationships remains unchanged. The discard updates the applicable records together or neither.
- A later legitimate intake for those Prospects reuses and reactivates the existing Household but creates a new Draft Referral. The earlier Discarded Draft remains inactive, and only one non-discarded Referral may exist for the Household at a time.
- Event and COI records created inline remain active when a Draft is discarded because they are independent, reusable Referral Sources. A mistaken Event or COI is deactivated separately.
- Deactivation preserves existing Referral Source links and excludes the Event or COI from new source selection by default. Users may include inactive references in search and reactivate them; deactivation and reactivation remain in contextual Audit History.
- An exact Event name-and-date match or exact COI phone/email match blocks creation and directs staff to the existing record. A matching COI name also blocks creation until staff select the existing person or provide a distinct phone or email. The first slice has no duplicate override or reference-record merge.
- Duplicate checks include inactive records. A matching inactive Household is reused and reactivated, while a matching inactive Event or COI is reactivated rather than recreated. Discarded Drafts remain inactive under their separate rule.
- Every member of a Prospective Household is a Prospect on its active Referral in this slice. Call participation remains selectable per member, and a Client-source relationship applies to the prospective Household collectively. Per-member Referral participation is deferred.

Real authentication, Platform User access administration, real firm/client data, Associate profiles, teams, job titles, roles, permission configuration, and workload summaries are excluded. The slice also stops before executing the `Discovery package sent` task, progressing through later outreach stages, conversion, client-appreciation tasks, Event management beyond source identity, COI management beyond source identity, on-demand reports, Household merging, and spreadsheet import. Those capabilities belong to later slices within the RMS-first release.

The evaluation environment uses clearly synthetic seed records modeled on the sanitized spreadsheets' structures and value patterns. Sanitized rows are not copied into fixtures, and the build must not receive real firm/client data.

The slice exposes only Referrals and Referral detail, Tasks, Households, minimal Events, minimal COIs, synthetic Associates, and contextual `Audit history` actions. Reports, appreciation, full Event/COI management, import, and a global Audit page are not present.

A Pre-existing Client Household requires a display name and at least one Household Member. Every member identified as a client requires a name and a platform-wide unique Client Number. Staff record the original Client Start Date when known or explicitly mark it `Unknown`; phone, email, mailing address, and notes are optional. This setup path creates neither a Referral nor client-appreciation tasks.

### Evaluation success

The slice is successful when an evaluator can demonstrate, using only synthetic data:

- Associate selection and Firm Business Day calendar setup
- Minimal Client, Event, and COI source-reference setup
- Draft creation, editing, discarding, search, and missing-field visibility
- Valid activation with exactly one correctly scheduled first follow-up task
- Unsuccessful attempts and rescheduling of the same task
- A two-way conversation and creation of the Discovery-package handoff task
- Follow-up Call skipping with a reason
- Every valid NFAR closure path and reopening into Follow-up Call
- Duplicate blocking, stale-save conflicts, ownership and task reassignment, and contextual Audit History
- Reset of the synthetic evaluation environment
- Disabled upload, external-integration, and production-deployment paths, with documented synthetic-only use

## RMS Page Responsibilities

The proposed RMS-first page model separates durable records from workflows:

| Page | Responsibility |
|---|---|
| Referrals | Referral discovery, lists, stages, statuses, sources, and ownership |
| Referral detail | Outreach workflow, activities, tasks, meetings, and disposition |
| Households | Household and person discovery, the V1 client list, relationship state, and duplicate handling |
| Household detail | Durable Household identity, contact information, and linked workflow history |
| Events | RMS appreciation events, invitee selection, invitations, responses, and attendance |
| COI Directory | COI people, expertise, contact information, and Referral Source selection |
| Associates | Staff records, active status, linked Platform User accounts, and current assignments |
| Tasks | Personal and firm-wide task assignment, scheduling, reminders, and status |
| Reports | Access to and generation of specific requested reports |

Referral detail and Household detail are separate but linked. Household owns durable identity and contact information; Referral owns the pre-client workflow. This avoids mixing identity, workflow, and appreciation into one spreadsheet-shaped record.

The Referrals page is the default landing page. A persistent global search in the main navigation searches Households and members, Referrals, COIs, Events, and tasks.

### Household model

A Household is created when its Referral is entered and continues through prospective, client, and former-client states; conversion does not create a replacement record or move its contact information. A Household may contain one person or multiple people. Each Household member has an individual name and contact information.

Every Household has a display name suggested from its members and editable by staff. Each member may have a name, optional preferred name, phone numbers, email addresses, and—once assigned—one individual Client Number. A Client Number belongs to that member rather than the Household. The Household may also have an optional mailing address; it is not required to activate a Referral.

Each Client Number is assigned directly by the firm and must be unique across the platform. A member has at most one Client Number. A person may belong to only one active Household at a time; moving a person to another Household preserves their earlier membership in Audit History.

The Households page is the V1 client list as well as the place to find prospective and former-client Households. A separate Clients page is deferred because it would duplicate the same relationship record in the first draft.

The Households list shows Household name, member names, relationship state, Client Numbers, primary contact information, current Referral Owner or Household Owner as applicable, and the next task. Users may filter the list by Household Owner. Household detail is organized into members and contact information, relationship state and Client Start Date, linked Referral, client-appreciation tasks, and event history, with an `Audit history` button where applicable.

The Household's Client Start Date is the date its first member receives a Client Number and has their first account opened. Later members may receive their own Client Numbers, but those later conversions do not recreate the Household's initial appreciation tasks.

### Associates page

An Associate records name, work email, active or inactive status, and the linked Platform User account. Creating an Associate business record is distinct from granting an authenticated account access to the platform.

All Platform Users have equal access to business records in V1. Granting or revoking sign-in access is the limited security-administration exception and is restricted to the firm lead or a designated identity administrator. Deactivating an Associate also disables their linked Platform User's sign-in access.

Identity Administrators also maintain the firm holiday and closure calendar. Calendar changes remain in Audit History and affect future due-date calculations only; they do not silently recalculate tasks that already exist.

Before an Associate can be made inactive, their open tasks and owned Referrals, client Households, and Events must be reassigned. Their earlier assignments and actions remain in Audit History.

### Events page

One Event record may support Referral Source attribution, client appreciation, or both; the platform does not create duplicate records for those purposes. An Event used only as a Referral Source may omit capacity, invitees, responses, and attendance. Event statuses are `Draft`, `Scheduled`, `Completed`, and `Cancelled`. A Draft event does not create invitation tasks until it is scheduled.

The Events list shows event name, date and time, audience or purpose, location, event owner, status, capacity, accepted-attendee count, and open invitation-task count. Event detail contains basic information, capacity, optional invitee selection, optional invitation responses and attendance, related Referrals, tasks, notes, and an `Audit history` action where applicable.

### COI Directory

A COI is an individual person rather than an organization record. The COI Directory stores name, optional organization affiliation, title or profession, expertise, phone, email, active status, and notes. Users may create, edit, deactivate, search, and select COIs as Referral Sources. COI follow-up dates, action workflows, and broader relationship management remain deferred.

### Tasks page

V1 requires each **Platform User** to sign in through an individually identifiable account linked to an **Associate**. Anonymous access is not supported. An Associate is the staff record used for ownership and assignment; the linked Platform User establishes who is acting.

V1 has no role-based access control. Every authenticated Platform User has the same visibility and actions, may view any Associate's work, and may assign or reassign work. Individual identity remains required for ownership, reminders, and Audit History. User-specific restrictions may be added in a later slice without changing the meaning of existing Associate assignments or actor history.

The Tasks page initially shows tasks assigned to the Associate linked to the signed-in Platform User and can be switched to another Associate or all firm tasks. It uses separate sections for:

- Overdue tasks
- Tasks due today
- Non-overdue tasks due within the next five Firm Business Days
- Tasks completed today

Tasks are ordered by due time, with the longest-overdue task first in the overdue section and the earliest due task first in the current and upcoming sections.

Each task row shows the task type, related Household or other record, related Referral or Event when applicable, due date and time, task assignee, Referral Owner or Household Owner as applicable, current workflow stage or status, and a clear overdue indicator.

Selecting a task opens its task-specific form in the context of its related Referral, Household, Event, or COI. The Tasks page does not provide a generic completion checkbox that could bypass required activity details.

Tasks completed during the current day remain available in a collapsed `Completed today` section until the end of that day.

The Tasks page supports filters for assignee, record owner, task type, status, due-date range, overdue state, and related record. Staff may create manual tasks from a Referral, Household, Event, or COI in addition to automatically generated tasks. Every V1 task must remain linked to one of those records; context-free standalone tasks are deferred.

Manual tasks require a title, related record, assignee, due date, reminder, and optional notes. Task statuses are `Open`, `Completed`, and `Cancelled`. `Overdue` is a derived due state for an Open task past its due time, not a manually selected lifecycle status. Completing a manual task records the completion time and acting user automatically and permits an optional completion note.

In-platform reminders appear through the Tasks page and a navigation notification indicator. V1 does not add a separate Notifications page.

### Reports page

The Reports page provides access to the specific reports supported by V1. A user selects a report and its parameters and requests generation when needed; reports are not automatically scheduled or delivered.

### Referrals page

The Referrals page is the default landing page. It shows Household or prospective-client name, all Referral Sources, Referral Received Date, Referral age, current outreach stage, Referral Status, Referral Owner, and the next open task with its due date. It supports filters for Referral Owner, Referral Status, outreach stage, Referral Source, Referral Received Date range, and task due state. It initially shows Draft and `In process` Referrals; users may include converted, NFAR, and Merged records.

A `Needs attention` view surfaces unowned Draft Referrals, `Became client` Referrals missing a Client Start Date, unresolved temporary COI sources, and active Referrals with no open next task. An active Referral without an open task is labelled `No next task` rather than being silently omitted.

The page provides the primary `New Referral` action. An existing Household page instead offers `Add Referral Source` or `Reopen Referral` when applicable, avoiding creation of a second Referral for a returning Household.

Referral detail is organized into summary, Household and contact information, Referral Sources, outreach progress, tasks, and activity history, with an `Audit history` button.

`New Referral` uses one form rather than a multi-step wizard. Staff first search existing Households and may create a new Household and its members within the same form when no match exists. The form includes an optional plain-text Referral context note.

Referral detail begins with a summary of Household, Referral Received Date, Referral Owner, Referral Status, current outreach stage, Referral age, Referral Sources, and next open task. The Referrals list is for finding and filtering records; workflow changes are made from Referral detail.

The legacy RMS `Relation` field represents the relationship between a referring client and the prospect, or `N/A` when the Referral was not received through a client. Its labels are `Friend` (`F`), `Relative` (`R`), `Colleague` (`C`), `Service provider` (`S`, such as a COI relationship), and `Other` (`O`). Each linked Client Referral Source has its own relationship to the prospect.

Proper role and permission boundaries are deferred and require professional review before later adoption.

## RMS Journey

### Referral intake

A **Referral** is the canonical, continuing pre-client intake record for a **Household**, which may contain one or more people. A Household does not receive a new Referral merely because it returns through another source; the new source is added to its existing Referral. A Referral may have multiple equally attributed sources:

| Code | Source | Required attribution |
|---|---|---|
| C | Client | Require selection of an existing client; temporary names are not allowed |
| E | Event | Require the specific event name and date |
| M | Marketing | Require the campaign or channel name |
| SM | Social Media | Require the platform |
| COI | Centre of Influence | Select an existing COI person, or enter a temporary name and create a mandatory task to link it later |
| O | Other | Require a short source description |

No source is primary. All applicable sources receive equal attribution.

Additional sources do not receive separate Referral Received Dates; the Referral retains its original date.

A temporary COI name does not prevent a Referral from entering `In process`. Staff choose the mandatory linking task's due date when entering the temporary name. RMS prevents that due date from falling after any applicable thank-you deadline.

Staff may save an incomplete Referral. Intake is complete when it has:

- At least one prospective client's name
- At least one contact method: phone number or email address
- At least one Referral Source
- Referral Received Date

Referral Received Date defaults to the current date in the Firm Time Zone. Staff may backdate it for delayed entry, but future dates are rejected.

People in the same Household may intentionally share phone numbers or email addresses; shared contact information within that Household is not a duplicate.

An incomplete Referral remains in `Draft`, creates no follow-up task, and may temporarily have no owner. Moving it to `In process` requires complete intake information and a Referral Owner.

Each active Referral has one current **Referral Owner**. The firm lead currently assigns the initial owner, but the proposed V1 has no role-based access control, so any platform user may assign or reassign a Referral. Past assignments remain visible.

RMS blocks creation of a second Referral when an exact phone number or email address matches contact information on an existing Household and directs the user to the existing Referral. The block has no override; staff must correct the contact information or Household membership before creating a separate Referral. An exact name match produces a possible-duplicate warning instead; the user must explicitly accept that warning before creating a separate Referral. RMS retains who accepted the warning and when, but no written reason is required.

### Referral outreach

Outreach follows this ordered sequence:

1. Follow-up call
2. Discovery package sent
3. Discovery package received
4. First meeting

Stages may be skipped, but a skipped stage must be explicitly marked `Skipped` with a reason. Repeat activities are retained; for example, every attempted follow-up call is recorded.

The follow-up-call stage is completed only by a two-way conversation with the prospective client. Leaving a voicemail is recorded as an unsuccessful attempt on the existing call task; it does not complete that task or create another task. The same task remains open until the stage is completed, skipped, or the Referral receives a closing disposition.

The first-meeting stage is completed only when the meeting is held. A cancellation or no-show is recorded on the existing meeting task, which remains open for rescheduling. Completing a held meeting records the meeting date, Household attendees, firm attendees, and brief notes. Referral disposition may remain undecided for several days after the meeting.

After a held meeting, RMS keeps the Referral `In process` and automatically creates a decision-follow-up task due three Firm Business Days later. If another meeting is needed instead, staff change that task to a meeting task and choose its planned date. When a decision follow-up occurs but the Household remains undecided, staff record the contact attempt on the same open task and move its due date. This may repeat as often as needed before a closing disposition is selected.

Completing `Discovery package sent` records the date sent, recipient Household member, and staff member who sent it. The workflow does not anticipate resending a discovery package.

Completing `Discovery package received` records the date received, staff member who recorded receipt, and whether the package is complete. An incomplete package does not complete the stage; its existing task remains open until the missing information arrives or the stage is skipped or outreach closes.

Completing or skipping an outreach stage automatically creates the task for the next stage in sequence. Every automatically created outreach task defaults to the current Referral Owner but may be reassigned. Each generated next-stage task defaults to three Firm Business Days after the preceding stage is completed or skipped; users may change that timing. The initial follow-up task retains its separate Friday-preference rule.

The **Referral Status** values are:

- `In process`, while outreach remains active
- `Became client`, which closes outreach while signed documents or account opening progress toward the Client Start Date
- `NFAR — not interested in our services`
- `NFAR — no business opportunity`
- `NFAR — no response`
- `Merged`, an administrative inactive state for a duplicate Referral linked to its surviving Referral

The responsible staff member decides case by case when unsuccessful contact warrants `NFAR — no response`; there is no fixed attempt threshold.

`Open` describes an unfinished task. It is distinct from the Referral-level `In process` status.

Completing the current task as NFAR records the selected disposition, marks that task `Completed`, and closes the Referral. Setting NFAR from Referral detail without performing the current task closes the Referral and marks the still-unperformed task `Cancelled`. The disposition records why outreach stopped in either path.

### Tasks, due dates, and reminders

Individual tasks may be assigned to staff other than the Referral Owner. The task assignee and Referral Owner both receive reminders and overdue notices.

For the proposed V1:

- Notifications appear inside the platform only; email is deferred.
- A task's default reminder appears at 9:00 a.m. Firm Time one Firm Business Day before its due date.
- Users may change the default reminder for an individual task.
- A task is due at 5:00 p.m. Firm Time on its selected Firm Business Day and is overdue immediately afterward.
- A Firm Business Day is Monday through Friday excluding dates on the firm's maintained holiday and closure calendar.

Moving a Referral to `In process` automatically creates its first follow-up task and starts its scheduling clock; the earlier Referral Received Date does not set the due date. The preferred default due date is 5:00 p.m. Friday of the same calendar week, shifted to the nearest preceding Firm Business Day when Friday is excluded by the firm calendar. If that target would allow fewer than three Firm Business Days after activation, the task is instead due at 5:00 p.m. on the third Firm Business Day after activation; it is not delayed to the following Friday. The activation day is excluded from the count. Activation after hours or outside a Firm Business Day is treated as occurring on the next Firm Business Day. Staff may perform the task earlier or move its due date.

Staff may move any task's due date, including after an unsuccessful call attempt. A change does not require a reason or an extra Referral Owner notification; the history retains who changed the due date and when.

### Audit history

RMS retains an **Audit History** for every change, including the previous value, new value, user, and timestamp. Historical values are not displayed in the normal RMS workflow; they are available separately when an audit is needed, and every platform user may access them. Changes to a Household's `Do not invite` preference are the current working exception and are not retained in Audit History. This describes required business access to history and does not choose a logging or storage implementation.

Applicable record detail pages provide an `Audit history` button rather than displaying Audit History inline. The button opens the history for that record and provides a CSV download option. A separate global Audit History page is deferred from the first draft.

Users do not permanently delete Referrals, Households, tasks, activities, or meeting notes. They correct, cancel, or make records inactive while Audit History remains available. Any professionally required retention, erasure, or legal-hold behavior may supersede this working rule and remains subject to review.

Any platform user may merge Households later found to be duplicates. The user chooses the surviving Referral. The other Referral becomes `Merged`, remains linked to the survivor, and retains its complete Audit History. Its open tasks are cancelled rather than moved to the surviving Referral. Its Referral Sources are copied to the surviving Referral for future attribution and reporting, with identical sources deduplicated to one entry. When a person has conflicting names or contact details, the user performing the merge selects the current value while both originals remain in Audit History.

Any platform user may reverse a mistaken merge. Reversal restores the pre-merge Households, Referral states, sources, and selected person/contact values, and reopens tasks that the merge cancelled. Each reopened task is assigned to its restored Referral's current owner with a new due date three Firm Business Days after reversal; prior assignees and due dates remain in Audit History. The merge and reversal also remain in Audit History.

### V1 task and record visibility

The proposed V1 makes these work lists available:

- Draft Referrals awaiting information or an owner
- Active Referrals grouped by owner and outreach stage
- Tasks assigned to the current user
- Non-overdue tasks due today or within the next five Firm Business Days, with the horizon adjustable by the user
- All overdue tasks
- `Became client` Referrals awaiting a Client Start Date
- New-client appreciation tasks
- Existing-client Event invitation tasks and activity
- Recently closed NFAR Referrals

V1 search covers Household and person names, phone numbers, email addresses, client numbers, Referral Source details, Referral Owners, task assignees, Referral Statuses, outreach stages, meeting notes, and activity notes. Name and note matching is partial and case-insensitive; email matching is case-insensitive; phone matching ignores formatting; and client numbers support exact and prefix matching. Search defaults to active records only; users may enable inactive and closed records, and every result clearly shows its status.

The Reports page provides these reports on demand:

- Referral pipeline by stage, status, owner, and age
- Referral sources and conversion outcomes
- Staff task workload, due work, and overdue work

Every platform user may access these reports. Each report supports applicable filters, including a user-selected date range, Referral Owner, Referral Source, Referral Status, outreach stage, and an option to include inactive, closed, and merged Referrals. Results appear on screen and may be downloaded as CSV.

PDF, print-ready formats, scheduled delivery, and a global audit report are deferred. Each CSV export is recorded with the user, timestamp, report, date range, and filters used.

The source-and-conversion report gives one full Referral and conversion count to every linked source. Source totals may therefore exceed the number of distinct Referrals, and the report must make that non-additive counting clear.

Report date ranges use the relevant business event: Referral Received Date for pipeline and source reporting, the date Referral Status changed to `Became client` for conversion outcomes, Client Start Date for completed conversion, and task due date for task reporting.

Referral age begins on Referral Received Date, including time spent in `Draft`. Active Referral age runs through the current date; closed Referral age stops on the date it moved to `Became client`, an NFAR disposition, or `Merged`.

### Conversion and client appreciation

Staff may select `Became client` once documents are signed or account opening is underway. Referral outreach then closes, but client appreciation waits for the Client Start Date.

If account opening fails or the Household withdraws before the Client Start Date, staff move the Referral from `Became client` to the applicable existing NFAR disposition. No client-appreciation tasks have been created at that point.

Current understanding is that pending account opening is not tracked, but this has not been confirmed. Future RMS handling remains undecided.

When the Household receives its first opened account and client number:

1. That date becomes the **Client Start Date**.
2. The Household becomes a client Household and its initial client-appreciation tasks are created.

The former Referral Owner becomes the default **Household Owner**. Any platform user may reassign client Household ownership independently of the closed Referral, and prior owners remain in Audit History.

Automatically created client-appreciation tasks default to the current Household Owner but may be reassigned individually without changing Household ownership. Reminders and overdue notices go to both the task assignee and Household Owner.

The initial client-appreciation tasks cover these required activities, which may occur in different orders:

1. One-time thank-you card to the new-client Household
2. One-time thank-you card to every linked Client referrer
3. One-time service call due three calendar months after the Client Start Date
4. Invitation to the Household's first new-client appreciation event

Each linked Client referrer receives a separate thank-you-card task. Completing any thank-you-card task records the date sent, recipient, staff member who sent it, and an optional note.

The three-month service-call task is completed only by a two-way conversation. Voicemail and unsuccessful attempts remain on the same open task, whose due date staff may move. Completion records the date, Household participants, firm participant, and brief notes.

The Household becomes an existing-client Household when invited to its first new-client appreciation event; attendance is not required. Existing-client appreciation events then recur. One event may serve new and existing clients at the same time.

Existing-client appreciation has no fixed periodic task or cadence. Staff create events as needed, select eligible existing-client Households, and RMS creates invitation tasks only for those selected.

Any platform user may mark a Household `Former client`. The action requires an effective date, reason, optional note, and extra confirmation; it cancels open appreciation tasks and event invitations, excludes the Household from future event selection, and preserves prior records and Audit History.

Any platform user may manually reactivate a Former Client Household as an existing-client Household. Reactivation requires an effective date, reason, optional note, and extra confirmation. Previously cancelled appreciation tasks remain cancelled; staff may select the reactivated Household for any future appreciation event.

Any unfinished new-client thank-you-card or service-call tasks remain open and mandatory after the Household becomes an existing-client Household.

If no new-client event has been scheduled, the Household remains `Waiting for next new-client event`. This is a workflow state, not a task, and has no due date. Creating an event date creates the Household's invitation task with a default due date one calendar month before the event; staff may change that date. If the calculated date is not a Firm Business Day, the due date moves to the preceding Firm Business Day. If the event is created less than one calendar month away, the invitation task is due at 5:00 p.m. on the next Firm Business Day.

Completing an invitation task records the event, invitation date, invited Household members, staff member who sent it, and an optional note. RMS also allows later recording of whether the Household accepted, declined, attended, or did not attend; those outcomes do not control the new-to-existing transition.

Staff select which Households an event will invite. They may select all eligible Households or filter by new-client versus existing-client status, Household Owner, Client Start Date range, last invitation date, last attendance date, never previously invited, Referral Source, or Household/person name. Event creation does not automatically invite everyone.

A Household marked `Do not invite` is excluded from event selection, including select-all actions. Staff must remove the preference before inviting the Household; changing this preference is not retained in Audit History.

Creating an appreciation event requires an event name, date and time, intended audience (`New clients`, `Existing clients`, or `Both`), location or virtual-meeting details, and staff owner. Capacity and notes are optional.

Event capacity counts individual expected attendees rather than Households. Staff record the expected attendee count when accepting an invitation. When capacity is set, RMS warns staff if accepted attendance exceeds it but does not block further acceptance. Declined invitations automatically release reserved capacity.

An accepted response may include guests who are not Household members. RMS records the number of guests, not their names, and includes them in capacity.

Each invitation task defaults to the selected Household's Household Owner, not the event owner. The event owner remains accountable for the overall event.

Cancelling an event preserves it with status `Cancelled`, cancels all open invitation tasks, and preserves completed invitations and responses. A Household whose first new-client event was cancelled returns to `Waiting for next new-client event` until invited to a replacement event. RMS creates a notification task for every Household already invited. Each notification task defaults to the Household's Household Owner, is due at 5:00 p.m. on the next Firm Business Day, and follows the normal reminder, reassignment, and Audit History rules.

Rescheduling an event preserves the original date in Audit History and recalculates open invitation-task due dates from the new event date. RMS creates a next-Firm-Business-Day update-notification task for every Household already invited; existing accepted or declined responses remain until staff update them.

### Reopening and correction

An NFAR Referral may be reopened, including when the Household returns through a new source. The user adds the source when applicable, provides a reopening reason, and manually selects the outreach stage from which work resumes; earlier disposition and activity history remain visible. If that stage has a task cancelled by the NFAR closure, RMS reopens the same task with its earlier attempts and dates rather than creating a replacement. The reopened task is assigned to the current Referral Owner and receives a new default due date three Firm Business Days after reopening; its prior assignees and due dates remain in history.

A correctly converted Household's later potential business belongs in the Pillar List workflow, not a reopened Referral. Reopening `Became client` is reserved for correcting an erroneous conversion. That correction:

- Requires a reason
- Preserves completed appreciation tasks as history
- Cancels incomplete appreciation tasks
- Returns the Household to prospective status

## Decisions Still Needed

- Firm-lead approval of broader RMS-release decisions outside the approved evaluation slice
- Appropriate professional review of equal-access permissions and relevant privacy, compliance, retention, and security-governance policy
- Firm-lead and professional review of excluding `Do not invite` preference changes from Audit History
- Deadlines for both thank-you-card tasks
- Whether linked Centre of Influence sources also receive a thank-you task
- Whether RMS should track and remind on pending account opening between `Became client` and the Client Start Date
- Firm-lead approval of the initial firm holiday and closure calendar and its authorized maintainer

## Deferred Migration Notes

The following previously captured decisions are retained for later migration planning, but spreadsheet import is not part of the current RMS page-and-workflow definition. When import planning resumes, the first supported import will cover only the RMS spreadsheet; Pillar List, Investment Transfer, Transfer Reconciliation, Insurance Business Monitor, and Focus 10 imports remain deferred to their respective workflow slices.

Any platform user may run or reverse an RMS import, consistent with the initial equal-access model.

Import uses fixed, reviewable mapping and validation rules: the same input produces the same result, and the platform does not guess or invent missing values. Ambiguous or invalid rows are rejected with specific errors so staff can correct the source data or mapping and rerun the import.

The first supported import accepts one approved RMS spreadsheet layout with fixed column mappings. Files with missing or unexpected columns are rejected; users do not create arbitrary mappings during import.

Rerunning an import adds only genuinely new rows. Rows that were already imported successfully do not create duplicates and are not updated by the rerun; previously rejected rows may be added after correction.

If a rerun contains changed data for a row already imported successfully, the platform does not update that record. It reports the conflict, and staff correct the existing platform record manually under normal Audit History rules.

Imports allow partial success: every valid row is imported, while invalid, ambiguous, duplicate, or conflicting rows are rejected or skipped individually. The platform produces a row-by-row result report.

Before writing records, import shows a validation preview with spreadsheet type and mapping, total rows, rows ready to import, duplicate or previously imported rows, invalid or ambiguous rows with errors, and conflicts with existing records. The user must explicitly confirm before valid rows are imported.

Any platform user may reverse an import batch with extra confirmation. Records created by that batch become inactive rather than being permanently deleted, and tasks created by it are cancelled. Records edited after import are flagged for individual review rather than reversed automatically. The import and reversal remain in Audit History.

Every imported record retains provenance identifying its import batch, spreadsheet type, original filename, source row number, source-file SHA-256 hash, importing user, and timestamp.

The platform does not retain the original uploaded spreadsheet after processing. It retains the file hash, provenance, validation preview, and result report, subject to professionally approved retention policy.
