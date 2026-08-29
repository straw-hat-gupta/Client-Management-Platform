## Purpose

Referral outreach records the work staff perform against an `In process` Referral. This capability
owns the Follow-up Call stage: contact attempts and their outcomes, the two-way conversation that
completes the stage, the `Skipped` stage outcome, NFAR closure and reopening, the correction paths
for mistaken activity, and creation of the `Discovery package sent` task that hands off to the next
slice.

## ADDED Requirements

### Requirement: Follow-up Call is the only outreach stage in this slice

Outreach SHALL begin at the Follow-up Call stage. The platform SHALL NOT allow work to progress
into `Discovery package received`, `First meeting`, or any later stage in this slice. The Follow-up
Call stage SHALL be completed only by a recorded two-way conversation, SHALL otherwise be
explicitly skipped, or SHALL end when the Referral receives an NFAR disposition. The same
follow-up task SHALL remain the Open Task until one of those outcomes occurs; an unsuccessful
attempt SHALL NOT create another task.

#### Scenario: Voicemail does not create a second task

- **GIVEN** an `In process` Referral with one Open follow-up task
- **WHEN** a Platform User records a `Voicemail` attempt
- **THEN** the same follow-up task remains Open
- **AND** no additional task is created

#### Scenario: Later stages are unavailable

- **GIVEN** a Referral whose `Discovery package sent` task exists
- **WHEN** a Platform User looks for `Discovery package received` or `First meeting` stage actions
- **THEN** no such actions exist in this slice

### Requirement: Recording a follow-up attempt

Each recorded attempt SHALL capture a non-future date and time in the Firm Time Zone — "non-future"
determined against server time resolved in the Firm Time Zone, never against a clock supplied by the
client — the Associate who made the attempt, an outcome of `No answer`, `Voicemail`, or `Two-way conversation`, an
optional note, and the Platform User and timestamp that entered it. The Associate who made the
attempt SHALL default to the Associate linked to the acting Platform User and MAY be changed when
recording on another Associate's behalf. An unsuccessful attempt SHALL also record the next due
date.

#### Scenario: Attempt is recorded with its defaults

- **GIVEN** an `In process` Referral with an Open follow-up task
- **WHEN** a Platform User records a `No answer` attempt without changing the caller
- **THEN** the attempt records the Associate linked to the acting Platform User as the caller
- **AND** records the entering Platform User and the entry timestamp separately
- **AND** appears in the Referral's activity history

#### Scenario: Attempt recorded on another Associate's behalf

- **GIVEN** the same Referral
- **WHEN** the Platform User records the attempt and selects a different synthetic Associate as the caller
- **THEN** the attempt records that Associate as the caller and the acting Platform User as the recorder

#### Scenario: Future attempt date and time is rejected

- **GIVEN** an attempt form
- **WHEN** the Platform User enters a date and time later than the current server time resolved in the Firm Time Zone
- **THEN** the platform rejects the attempt, records nothing, and preserves the entered values

#### Scenario: A client clock cannot widen the accepted range

- **GIVEN** a client whose local clock is ahead of server time
- **WHEN** it submits an attempt dated after the current server time in the Firm Time Zone
- **THEN** the platform rejects the attempt

### Requirement: Unsuccessful attempt keeps the task Open and requires a next due date

Recording `No answer` or `Voicemail` SHALL keep the follow-up task Open and SHALL require a future
next due date. That date SHALL default to three Firm Business Days after the attempt and MAY be
changed by the Platform User within the platform's rescheduling constraints. The previous due date
SHALL remain in Audit History.

#### Scenario: Next due date defaults to three Firm Business Days

- **GIVEN** an Open follow-up task
- **WHEN** a Platform User records a `Voicemail` attempt and accepts the offered next due date
- **THEN** the task's due date becomes the third Firm Business Day after the attempt at 5:00 p.m. Firm Time
- **AND** Audit History records the previous due date, the new due date, the actor, and the timestamp

#### Scenario: Next due date may be changed

- **GIVEN** the same attempt
- **WHEN** the Platform User selects a different valid future Firm Business Day
- **THEN** the platform accepts that date as the task's due date

#### Scenario: Missing next due date is rejected

- **GIVEN** an unsuccessful attempt being recorded
- **WHEN** the Platform User clears the next due date and saves
- **THEN** the platform rejects the save, states that a future next due date is required, and records no attempt

### Requirement: Two-way conversation

A `Two-way conversation` attempt SHALL identify at least one participating Prospect from the
prospective Household and SHALL require a result of `Continue outreach`,
`NFAR — not interested in our services`, or `NFAR — no business opportunity`. A note SHALL remain
optional. `NFAR — no response` SHALL NOT be selectable for a completed conversation because
contact occurred.

#### Scenario: Conversation without a participant is rejected

- **GIVEN** a `Two-way conversation` attempt being recorded
- **WHEN** the Platform User selects no participating Prospect
- **THEN** the platform rejects the save, states that at least one participating Prospect is required, and records no attempt

#### Scenario: `NFAR — no response` is unavailable after contact

- **GIVEN** a `Two-way conversation` attempt being recorded
- **WHEN** the Platform User opens the result options
- **THEN** `NFAR — no response` is not offered

#### Scenario: Participation is recorded per member

- **GIVEN** a prospective Household with two members
- **WHEN** the Platform User records a conversation with one of them
- **THEN** the attempt identifies that member as the participating Prospect

### Requirement: Completing Follow-up Call and creating the handoff task is atomic

Completing the Follow-up Call stage through `Continue outreach`, or skipping it, SHALL complete or
cancel its task and create exactly one `Discovery package sent` task in one atomic operation.
Failure SHALL leave the Follow-up Call task Open, SHALL preserve entered values, and SHALL permit
retry without creating duplicate activities or tasks.

#### Scenario: Continue outreach completes the stage and creates one task

- **GIVEN** an `In process` Referral with an Open follow-up task
- **WHEN** a Platform User records a `Two-way conversation` with the result `Continue outreach`
- **THEN** the follow-up task becomes `Completed`
- **AND** exactly one `Discovery package sent` task is created
- **AND** the Referral remains `In process`
- **AND** Audit History records the stage completion and the task creation

#### Scenario: Failure leaves the stage open

- **GIVEN** the same operation
- **WHEN** any part of completing the task or creating the `Discovery package sent` task fails
- **THEN** the follow-up task remains Open
- **AND** no `Discovery package sent` task exists
- **AND** no attempt or stage completion is recorded
- **AND** the Platform User's entered values are preserved for retry

#### Scenario: Retry does not duplicate

- **GIVEN** a failed completion that changed nothing
- **WHEN** the Platform User retries successfully
- **THEN** exactly one attempt is recorded and exactly one `Discovery package sent` task exists

### Requirement: Skipping the Follow-up Call stage

`Skipped` SHALL be an outreach stage outcome requiring a reason, and SHALL NOT be a Task status.
Skipping the Follow-up Call stage SHALL mark its unperformed task `Cancelled` and SHALL create the
`Discovery package sent` task in the same atomic operation. Audit History SHALL identify
intentional stage skipping as the cause of the cancellation and SHALL retain the reason.

#### Scenario: Skip cancels the task and creates the next one

- **GIVEN** an `In process` Referral with an Open follow-up task
- **WHEN** a Platform User skips the Follow-up Call stage with a reason
- **THEN** the follow-up task becomes `Cancelled`
- **AND** exactly one `Discovery package sent` task is created
- **AND** Audit History records the skip reason, actor, timestamp, and intentional stage skipping as the cancellation cause

#### Scenario: Skip without a reason is rejected

- **GIVEN** the skip action
- **WHEN** the Platform User confirms without supplying a reason
- **THEN** the platform rejects the action and the follow-up task remains Open

#### Scenario: `Skipped` is not a Task status

- **GIVEN** any task in the platform
- **WHEN** a Platform User views the available Task statuses
- **THEN** only `Open`, `Completed`, and `Cancelled` are present

### Requirement: The `Discovery package sent` task is the slice boundary

The generated `Discovery package sent` task SHALL appear in the Tasks page and on Referral detail
with its assignee, the Referral Owner, its due date, and its reminder. It SHALL default to the
current Referral Owner as assignee and to a due date three Firm Business Days after the Follow-up
Call stage was completed or skipped. It MAY be reassigned and rescheduled and SHALL appear in
Audit History. Its completion action SHALL be disabled and SHALL be clearly identified as the
boundary to the next evaluation slice.

#### Scenario: Completion is disabled and labelled

- **GIVEN** a `Discovery package sent` task
- **WHEN** a Platform User opens it
- **THEN** the completion action is disabled
- **AND** the platform states that executing this task is the boundary to the next evaluation slice

#### Scenario: The task may still be reassigned and rescheduled

- **GIVEN** a `Discovery package sent` task
- **WHEN** a Platform User changes its assignee and its due date to a valid future Firm Business Day
- **THEN** the platform accepts both changes
- **AND** Audit History records previous and new values, actor, and timestamp

#### Scenario: Default due date is three Firm Business Days after the stage outcome

- **GIVEN** a Follow-up Call stage completed or skipped on a Firm Business Day
- **WHEN** the `Discovery package sent` task is created
- **THEN** its due date is 5:00 p.m. Firm Time on the third Firm Business Day after that day

### Requirement: NFAR disposition closes the Referral

An NFAR disposition SHALL be one of `NFAR — not interested in our services`,
`NFAR — no business opportunity`, or `NFAR — no response`. Completing the current task as NFAR
SHALL record the selected disposition, mark that task `Completed`, and close the Referral. Setting
NFAR from Referral detail without performing the current task SHALL close the Referral and mark the
still-unperformed task `Cancelled`. Either path SHALL be atomic and SHALL create no next task.

#### Scenario: NFAR through the task completes it

- **GIVEN** an `In process` Referral with an Open follow-up task
- **WHEN** a Platform User records a `Two-way conversation` with the result `NFAR — no business opportunity`
- **THEN** the follow-up task becomes `Completed`
- **AND** the Referral Status becomes `NFAR — no business opportunity`
- **AND** no `Discovery package sent` task is created
- **AND** Audit History records the disposition, actor, and timestamp

#### Scenario: NFAR from Referral detail cancels the task

- **GIVEN** an `In process` Referral with an Open follow-up task
- **WHEN** a Platform User sets `NFAR — not interested in our services` from Referral detail without performing the task
- **THEN** the task becomes `Cancelled`
- **AND** the Referral Status becomes `NFAR — not interested in our services`
- **AND** no next task is created

#### Scenario: NFAR closure failure changes nothing

- **GIVEN** an NFAR closure in progress
- **WHEN** any part of the closure fails
- **THEN** the Referral remains `In process`, the task keeps its previous status, and Audit History records nothing for the failed attempt

### Requirement: `NFAR — no response` requires a recorded attempt

`NFAR — no response` SHALL require at least one recorded contact attempt that is not marked
`Entered in error`. The platform SHALL NOT impose a fixed number of attempts; the responsible staff
member decides case by case when to stop. Prior attempts SHALL remain visible after closure.

#### Scenario: No attempts blocks the disposition

- **GIVEN** an `In process` Referral with no recorded attempt
- **WHEN** a Platform User selects `NFAR — no response`
- **THEN** the platform rejects the disposition and states that at least one recorded contact attempt is required

#### Scenario: One attempt is sufficient

- **GIVEN** an `In process` Referral with exactly one `No answer` attempt
- **WHEN** a Platform User selects `NFAR — no response`
- **THEN** the platform accepts the disposition and closes the Referral

#### Scenario: An `Entered in error` attempt does not satisfy the requirement

- **GIVEN** an `In process` Referral whose only attempt is marked `Entered in error`
- **WHEN** a Platform User selects `NFAR — no response`
- **THEN** the platform rejects the disposition

### Requirement: Reopening an NFAR Referral into Follow-up Call

An NFAR Referral MAY be reopened into the Follow-up Call stage with a required reason and an
additional Referral Source when applicable. Reopening SHALL reopen the same task with its earlier
attempts and dates rather than creating a replacement, SHALL assign it to the current Referral
Owner, and SHALL give it a new default due date three Firm Business Days after reopening.
Reopening into any later stage SHALL be outside this slice.

#### Scenario: Reopening restores the same task

- **GIVEN** a Referral closed as `NFAR — no response` whose follow-up task was `Completed`
- **WHEN** a Platform User reopens it into the Follow-up Call stage with a reason
- **THEN** the Referral Status becomes `In process`
- **AND** the same follow-up task becomes Open with its earlier attempts and dates retained
- **AND** it is assigned to the current Referral Owner with a due date on the third Firm Business Day after reopening
- **AND** Audit History records the reopening reason, previous and new values, actor, and timestamp

#### Scenario: Reopening without a reason is rejected

- **GIVEN** an NFAR Referral
- **WHEN** a Platform User attempts to reopen it without a reason
- **THEN** the platform rejects the action and the Referral remains closed

#### Scenario: Reopening into a later stage is unavailable

- **GIVEN** an NFAR Referral
- **WHEN** a Platform User opens the reopening action
- **THEN** only the Follow-up Call stage is available

### Requirement: Correcting a follow-up attempt

Correcting a recorded attempt SHALL require a correction reason and SHALL preserve the previous
value, new value, actor, reason, and timestamp. A correction SHALL NOT silently change the related
task's or Referral's state.

#### Scenario: Correction requires a reason

- **GIVEN** a recorded attempt
- **WHEN** a Platform User corrects its note or outcome detail without supplying a reason
- **THEN** the platform rejects the correction and the attempt is unchanged

#### Scenario: Correction leaves workflow state untouched

- **GIVEN** an `In process` Referral with an Open follow-up task and a corrected attempt
- **WHEN** the correction is saved with a reason
- **THEN** the task status, due date, and Referral Status are unchanged
- **AND** Audit History records the previous value, new value, actor, reason, and timestamp

### Requirement: Marking an attempt `Entered in error`

A wholly false attempt SHALL be marked `Entered in error` rather than deleted. It SHALL remain
visible in the Referral's history and SHALL NOT count as evidence for any workflow decision,
including the attempt requirement for `NFAR — no response`.

#### Scenario: Entered in error remains visible

- **GIVEN** an attempt that never occurred
- **WHEN** a Platform User marks it `Entered in error` with a reason
- **THEN** the attempt remains visible in history clearly marked `Entered in error`
- **AND** it is not deleted
- **AND** Audit History records the change with actor, reason, and timestamp

### Requirement: A depended-upon attempt cannot be invalidated first

The platform SHALL refuse to mark an attempt `Entered in error` while a later state transition
depends on it — specifically when it is the only attempt supporting a `NFAR — no response`
disposition, or the two-way conversation that completed the Follow-up Call stage while its
`Discovery package sent` handoff remains active. The Platform User SHALL first explicitly reverse
the dependent transition. The platform SHALL NOT silently rewrite downstream state.

#### Scenario: Attempt supporting NFAR is protected

- **GIVEN** a Referral closed as `NFAR — no response` supported by exactly one attempt
- **WHEN** a Platform User attempts to mark that attempt `Entered in error`
- **THEN** the platform refuses and states that the dependent NFAR disposition must be reversed first
- **AND** neither the attempt nor the Referral Status changes

#### Scenario: Conversation supporting an active handoff is protected

- **GIVEN** a Follow-up Call completed by a two-way conversation whose `Discovery package sent` task is still active
- **WHEN** a Platform User attempts to mark that conversation `Entered in error`
- **THEN** the platform refuses and states that the Follow-up completion must be reversed first

### Requirement: Reversing an incorrect Follow-up completion

Reversing an incorrect Follow-up Call completion SHALL require extra confirmation and a correction
reason. The reversal SHALL cancel the unexecuted `Discovery package sent` task, reopen the original
Follow-up task retaining its earlier valid attempts, assign that task to the current Referral
Owner with a due date three Firm Business Days after the reversal, and mark the incorrect
conversation `Entered in error`. Every transition SHALL remain in Audit History. The reversal SHALL
apply as a whole or not at all.

#### Scenario: Reversal restores the Follow-up Call stage

- **GIVEN** a Follow-up Call completed by an incorrect two-way conversation with an active `Discovery package sent` task
- **WHEN** a Platform User reverses the completion with extra confirmation and a reason
- **THEN** the `Discovery package sent` task becomes `Cancelled`
- **AND** the original follow-up task becomes Open with its earlier valid attempts retained
- **AND** that task is assigned to the current Referral Owner with a due date on the third Firm Business Day after the reversal
- **AND** the incorrect conversation is marked `Entered in error`
- **AND** Audit History records every transition with actor, reason, and timestamp

#### Scenario: Reversal without confirmation or reason is rejected

- **GIVEN** the reversal action
- **WHEN** the Platform User omits the extra confirmation or the correction reason
- **THEN** the platform rejects the reversal and no record changes

#### Scenario: Partial reversal is not accepted

- **GIVEN** a reversal in progress
- **WHEN** any part of it fails
- **THEN** no part of the reversal takes effect and the Platform User receives an error

### Requirement: Equal access to outreach actions

Every authenticated Platform User SHALL be able to record attempts, complete or skip the Follow-up
Call stage, set and reverse dispositions, and reassign outreach tasks, regardless of who owns the
Referral. Every such action SHALL identify the acting Platform User in Audit History.

#### Scenario: A non-owner records an attempt

- **GIVEN** a Referral owned by one Associate
- **WHEN** a different Platform User records a follow-up attempt
- **THEN** the platform accepts the attempt and records the acting Platform User as its recorder
