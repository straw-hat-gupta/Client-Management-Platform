# Product Journeys

## Document Status

- Status: Working draft
- Discovery sessions: 2026-08-17, 2026-08-20, and 2026-08-23
- Last updated: 2026-08-23
- Approval status: Pending firm-lead review
- Decision authority: The commissioning firm lead has final approval across product scope, workflows, permissions, professional-policy decisions, and release acceptance.
- Working authority: The current discovery participant may develop tentative decisions for firm-lead approval.
- Evidence boundary: These notes capture the collaborative discovery session and are not evidence extracted from source `I-001`.

## Tentative V1 Focus

The proposed first vertical slice is the RMS flow: Referral intake, outreach, conversion, and the connected client-appreciation workflow. Existing-client potential business belongs to the separate Pillar List workflow.

## RMS Journey

### Referral intake

A **Referral** is the canonical, continuing pre-client intake record for a **Household**, which may contain one or more people. A Household does not receive a new Referral merely because it returns through another source; the new source is added to its existing Referral. A Referral may have multiple equally attributed sources:

| Code | Source | Required attribution |
|---|---|---|
| C | Client | Require selection of an existing client; temporary names are not allowed |
| E | Event | Require the specific event name and date |
| M | Marketing | Require the campaign or channel name |
| SM | Social Media | Require the platform |
| COI | Centre of Influence | Select an existing centre or person, or enter a temporary name and create a mandatory task to link it later |
| O | Other | Require a short source description |

No source is primary. All applicable sources receive equal attribution.

Additional sources do not receive separate `Date received` values; the Referral retains its original overall date.

A temporary COI name does not prevent a Referral from entering `In process`. Staff choose the mandatory linking task's due date when entering the temporary name. RMS prevents that due date from falling after any applicable thank-you deadline.

Staff may save an incomplete Referral. Intake is complete when it has:

- At least one prospective client's name
- At least one contact method: phone number or email address
- At least one Referral Source
- Date received

`Date received` defaults to the current Alberta date. Staff may backdate it for delayed entry, but future dates are rejected.

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

After a held meeting, RMS keeps the Referral `In process` and automatically creates a decision-follow-up task due three Alberta business days later. If another meeting is needed instead, staff change that task to a meeting task and choose its planned date. When a decision follow-up occurs but the Household remains undecided, staff record the contact attempt on the same open task and move its due date. This may repeat as often as needed before a closing disposition is selected.

Completing `Discovery package sent` records the date sent, recipient Household member, and staff member who sent it. The workflow does not anticipate resending a discovery package.

Completing `Discovery package received` records the date received, staff member who recorded receipt, and whether the package is complete. An incomplete package does not complete the stage; its existing task remains open until the missing information arrives or the stage is skipped or outreach closes.

Completing or skipping an outreach stage automatically creates the task for the next stage in sequence. Every automatically created outreach task defaults to the current Referral Owner but may be reassigned. Each generated next-stage task defaults to three Alberta business days after the preceding stage is completed or skipped; users may change that timing. The initial follow-up task retains its separate Friday-preference rule.

The **Referral Status** values are:

- `In process`, while outreach remains active
- `Became client`, which closes outreach while signed documents or account opening progress toward the Client Start Date
- `NFAR — not interested in our services`
- `NFAR — no business opportunity`
- `NFAR — no response`
- `Merged`, an administrative inactive state for a duplicate Referral linked to its surviving Referral

The responsible staff member decides case by case when unsuccessful contact warrants `NFAR — no response`; there is no fixed attempt threshold.

`Open` describes an unfinished task. It is distinct from the Referral-level `In process` status.

Closing a Referral with an NFAR disposition automatically marks its current open task `Cancelled`; the NFAR disposition records why outreach stopped.

### Tasks, due dates, and reminders

Individual tasks may be assigned to staff other than the Referral Owner. The task assignee and Referral Owner both receive reminders and overdue notices.

For the proposed V1:

- Notifications appear inside the platform only; email is deferred.
- A task's default reminder appears at 9:00 a.m. Alberta time one Alberta business day before its due date.
- Users may change the default reminder for an individual task.
- A task is due at 5:00 p.m. Alberta time on its selected business day and is overdue immediately afterward.
- An Alberta business day is Monday through Friday excluding Alberta holidays.

Moving a Referral to `In process` automatically creates its first follow-up task and starts its scheduling clock; the earlier `Date received` does not set the due date. The preferred default due date is 5:00 p.m. Friday of the same calendar week, shifted to the nearest preceding Alberta business day when Friday is a holiday. If that target would allow fewer than three Alberta business days after activation, the task is instead due at 5:00 p.m. on the third Alberta business day after activation; it is not delayed to the following Friday. The activation day is excluded from the count. After-hours, weekend, or Alberta-holiday activation is treated as occurring on the next Alberta business day. Staff may perform the task earlier or move its due date.

Staff may move any task's due date, including after an unsuccessful call attempt. A change does not require a reason or an extra Referral Owner notification; the history retains who changed the due date and when.

### Audit history

RMS retains an **Audit History** for every change, including the previous value, new value, user, and timestamp. Historical values are not displayed in the normal RMS workflow; they are available separately when an audit is needed, and every platform user may access them. Changes to a Household's `Do not invite` preference are the current working exception and are not retained in Audit History. This describes required business access to history and does not choose a logging or storage implementation.

Users do not permanently delete Referrals, Households, tasks, activities, or meeting notes. They correct, cancel, or make records inactive while Audit History remains available. Any professionally required retention, erasure, or legal-hold behavior may supersede this working rule and remains subject to review.

Any platform user may merge Households later found to be duplicates. The user chooses the surviving Referral. The other Referral becomes `Merged`, remains linked to the survivor, and retains its complete Audit History. Its open tasks are cancelled rather than moved to the surviving Referral. Its Referral Sources are copied to the surviving Referral for future attribution and reporting, with identical sources deduplicated to one entry. When a person has conflicting names or contact details, the user performing the merge selects the current value while both originals remain in Audit History.

Any platform user may reverse a mistaken merge. Reversal restores the pre-merge Households, Referral states, sources, and selected person/contact values, and reopens tasks that the merge cancelled. Each reopened task is assigned to its restored Referral's current owner with a new due date three Alberta business days after reversal; prior assignees and due dates remain in Audit History. The merge and reversal also remain in Audit History.

### V1 work visibility

The proposed V1 makes these work lists available:

- Draft Referrals awaiting information or an owner
- Active Referrals grouped by owner and outreach stage
- Tasks assigned to the current user
- Non-overdue tasks due today or within the next five Alberta business days, with the horizon adjustable by the user
- All overdue tasks
- `Became client` Referrals awaiting a Client Start Date
- New-client appreciation tasks
- Existing-client recurring appreciation activities
- Recently closed NFAR Referrals

V1 search covers Household and person names, phone numbers, email addresses, client numbers, Referral Source details, Referral Owners, task assignees, Referral Statuses, outreach stages, meeting notes, and activity notes. Name and note matching is partial and case-insensitive; email matching is case-insensitive; phone matching ignores formatting; and client numbers support exact and prefix matching. Search defaults to active records only; users may enable inactive and closed records, and every result clearly shows its status.

The proposed V1 includes these management reports:

- Referral pipeline by stage, status, owner, and age
- Referral sources and conversion outcomes
- Staff task workload, due work, and overdue work
- Audit and change activity

Every platform user may access these reports. Each report supports a user-selected date range, filters for Referral Owner, Referral Source, Referral Status, and outreach stage, plus an option to include inactive, closed, and merged Referrals.

V1 reports may be exported as CSV. PDF and print-ready formats are deferred. Each export is recorded with the user, timestamp, report, date range, and filters used.

The source-and-conversion report gives one full Referral and conversion count to every linked source. Source totals may therefore exceed the number of distinct Referrals, and the report must make that non-additive counting clear.

Report date ranges use the relevant business event: Referral `Date received` for pipeline and source reporting, the date Referral Status changed to `Became client` for conversion outcomes, Client Start Date for completed conversion, task due date for task reporting, and change timestamp for Audit History reporting.

Referral age begins on `Date received`, including time spent in `Draft`. Active Referral age runs through the current date; closed Referral age stops on the date it moved to `Became client`, an NFAR disposition, or `Merged`.

### Conversion and client appreciation

Staff may select `Became client` once documents are signed or account opening is underway. Referral outreach then closes, but client appreciation waits for the Client Start Date.

If account opening fails or the Household withdraws before the Client Start Date, staff move the Referral from `Became client` to the applicable existing NFAR disposition. No client-appreciation workflow has begun at that point.

Current understanding is that pending account opening is not tracked, but this has not been confirmed. Future RMS handling remains undecided.

When the Household receives its first opened account and client number:

1. That date becomes the **Client Start Date**.
2. A separate client-appreciation workflow begins for the Household.

The former Referral Owner becomes the default **Appreciation Owner**. Any platform user may reassign appreciation ownership independently of the closed Referral, and prior owners remain in Audit History.

Automatically created appreciation tasks default to the current Appreciation Owner but may be reassigned individually without changing appreciation ownership. Reminders and overdue notices go to both the task assignee and Appreciation Owner.

The workflow includes these required activities, which may occur in different orders:

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

If no new-client event has been scheduled, the Household remains `Waiting for next new-client event`. This is a workflow state, not a task, and has no due date. Creating an event date creates the Household's invitation task with a default due date one calendar month before the event; staff may change that date. If the calculated date is a weekend or Alberta holiday, the due date moves to the preceding Alberta business day. If the event is created less than one calendar month away, the invitation task is due at 5:00 p.m. on the next Alberta business day.

Completing an invitation task records the event, invitation date, invited Household members, staff member who sent it, and an optional note. RMS also allows later recording of whether the Household accepted, declined, attended, or did not attend; those outcomes do not control the new-to-existing transition.

Staff select which Households an event will invite. They may select all eligible Households or filter by new-client versus existing-client status, Appreciation Owner, Client Start Date range, last invitation date, last attendance date, never previously invited, Referral Source, or Household/person name. Event creation does not automatically invite everyone.

A Household marked `Do not invite` is excluded from event selection, including select-all actions. Staff must remove the preference before inviting the Household; changing this preference is not retained in Audit History.

Creating an appreciation event requires an event name, date and time, intended audience (`New clients`, `Existing clients`, or `Both`), location or virtual-meeting details, and staff owner. Capacity and notes are optional.

Event capacity counts individual expected attendees rather than Households. Staff record the expected attendee count when accepting an invitation. When capacity is set, RMS warns staff if accepted attendance exceeds it but does not block further acceptance. Declined invitations automatically release reserved capacity.

An accepted response may include guests who are not Household members. RMS records the number of guests, not their names, and includes them in capacity.

Each invitation task defaults to the selected Household's Appreciation Owner, not the event owner. The event owner remains accountable for the overall event.

Cancelling an event preserves it with status `Cancelled`, cancels all open invitation tasks, and preserves completed invitations and responses. A Household whose first new-client event was cancelled returns to `Waiting for next new-client event` until invited to a replacement event. RMS creates a notification task for every Household already invited. Each notification task defaults to the Household's Appreciation Owner, is due at 5:00 p.m. on the next Alberta business day, and follows the normal reminder, reassignment, and Audit History rules.

Rescheduling an event preserves the original date in Audit History and recalculates open invitation-task due dates from the new event date. RMS creates a next-Alberta-business-day update-notification task for every Household already invited; existing accepted or declined responses remain until staff update them.

### Reopening and correction

An NFAR Referral may be reopened, including when the Household returns through a new source. The user adds the source when applicable, provides a reopening reason, and manually selects the outreach stage from which work resumes; earlier disposition and activity history remain visible. If that stage has a task cancelled by the NFAR closure, RMS reopens the same task with its earlier attempts and dates rather than creating a replacement. The reopened task is assigned to the current Referral Owner and receives a new default due date three Alberta business days after reopening; its prior assignees and due dates remain in history.

A correctly converted Household's later potential business belongs in the Pillar List workflow, not a reopened Referral. Reopening `Became client` is reserved for correcting an erroneous conversion. That correction:

- Requires a reason
- Preserves completed appreciation tasks as history
- Cancels incomplete appreciation tasks
- Returns the Household to prospective status

## Decisions Still Needed

- Firm-lead approval of every working decision above
- Appropriate professional review of equal-access permissions and relevant privacy, compliance, retention, and security-governance policy
- Firm-lead and professional review of excluding `Do not invite` preference changes from Audit History
- Deadlines for both thank-you-card tasks
- Whether linked Centre of Influence sources also receive a thank-you task
- Whether RMS should track and remind on pending account opening between `Became client` and the Client Start Date
- The authoritative Alberta holiday calendar
- Remaining RMS fields, validation, reporting, correction, and recovery details

## Migration Working Decisions

Spreadsheet import is proposed as a reusable platform capability rather than a one-time operation. The import operator selects and supplies the records, including any desired history cutoff, and the platform processes everything in that curated input rather than applying its own lookback period. Import behavior is deterministic and tailored to each supported spreadsheet type. V1 supports only the RMS spreadsheet; Pillar List, Investment Transfer, Transfer Reconciliation, Insurance Business Monitor, and Focus 10 imports are deferred to their respective workflow slices.

Any platform user may run or reverse an RMS import, consistent with V1 equal access.

Import uses fixed, reviewable mapping and validation rules: the same input produces the same result, and the platform does not guess or invent missing values. Ambiguous or invalid rows are rejected with specific errors so staff can correct the source data or mapping and rerun the import.

V1 accepts one approved RMS spreadsheet layout with fixed column mappings. Files with missing or unexpected columns are rejected; users do not create arbitrary mappings during import.

Rerunning an import adds only genuinely new rows. Rows that were already imported successfully do not create duplicates and are not updated by the rerun; previously rejected rows may be added after correction.

If a rerun contains changed data for a row already imported successfully, the platform does not update that record. It reports the conflict, and staff correct the existing platform record manually under normal Audit History rules.

Imports allow partial success: every valid row is imported, while invalid, ambiguous, duplicate, or conflicting rows are rejected or skipped individually. The platform produces a row-by-row result report.

Before writing records, import shows a validation preview with spreadsheet type and mapping, total rows, rows ready to import, duplicate or previously imported rows, invalid or ambiguous rows with errors, and conflicts with existing records. The user must explicitly confirm before valid rows are imported.

Any platform user may reverse an import batch with extra confirmation. Records created by that batch become inactive rather than being permanently deleted, and tasks created by it are cancelled. Records edited after import are flagged for individual review rather than reversed automatically. The import and reversal remain in Audit History.

Every imported record retains provenance identifying its import batch, spreadsheet type, original filename, source row number, source-file SHA-256 hash, importing user, and timestamp.

The platform does not retain the original uploaded spreadsheet after processing. It retains the file hash, provenance, validation preview, and result report, subject to professionally approved retention policy.
