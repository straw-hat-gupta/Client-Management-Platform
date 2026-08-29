## Purpose

Tasks carry the firm's scheduled work and make missed follow-up visible. This capability owns task
status, due dates and due times in the Firm Time Zone, the Firm Business Day calendar, the default
scheduling rules for generated outreach tasks, the persistent reminder indicator, manual
rescheduling constraints, and the handling of a due date that a later calendar change makes
invalid.

## ADDED Requirements

### Requirement: Task statuses and derived Overdue

A task SHALL have exactly one of the statuses `Open`, `Completed`, or `Cancelled`. `Overdue` SHALL
be a derived due state for an Open Task past its due time and SHALL NOT be a selectable status. A
`Completed` or `Cancelled` task SHALL NOT be shown as Overdue. Completing a task SHALL record the
completion time and the acting Platform User.

#### Scenario: Open Task past its due time is Overdue

- **GIVEN** an Open Task due at 5:00 p.m. Firm Time today
- **WHEN** server time resolved in the Firm Time Zone passes 5:00 p.m.
- **THEN** the task is shown as Overdue with no grace period
- **AND** its status remains `Open`

#### Scenario: Overdue is not selectable

- **GIVEN** a task detail view
- **WHEN** a Platform User views the selectable statuses
- **THEN** only `Open`, `Completed`, and `Cancelled` are offered

#### Scenario: Cancelled task is not Overdue

- **GIVEN** a `Cancelled` task whose due date has passed
- **WHEN** a Platform User views the Tasks page
- **THEN** the task does not appear in the overdue section

### Requirement: Every task is linked to a business record

Every task SHALL remain linked to one Referral, Household, Event, or COI record. The platform SHALL
NOT create a task without such a link.

#### Scenario: Generated task carries its link

- **GIVEN** a Referral being activated
- **WHEN** the first follow-up task is created
- **THEN** the task is linked to that Referral and its Household

### Requirement: Firm Time Zone governs business dates and times

All business dates, task times, reminders, attempt timestamps, and displayed Audit History
timestamps SHALL use the `America/Edmonton` Firm Time Zone, including its daylight-saving rules,
regardless of the evaluator's computer settings. Date-only values SHALL remain firm-calendar dates.

#### Scenario: Evaluator time zone does not change business behavior

- **GIVEN** an evaluator whose computer is set to a different time zone
- **WHEN** they view a task due at 5:00 p.m. Firm Time
- **THEN** the platform shows and enforces 5:00 p.m. Firm Time, not the evaluator's local time

#### Scenario: Daylight-saving transition is handled

- **GIVEN** a due date that falls on a Firm Time Zone daylight-saving transition day
- **WHEN** the platform calculates the due time and reminder time
- **THEN** both resolve to 5:00 p.m. and 9:00 a.m. Firm Time on the correct firm-calendar dates

### Requirement: Firm Business Day calendar

A Firm Business Day SHALL be a Monday through Friday that is not excluded by the firm's maintained
holiday and closure calendar. Task scheduling, due-date defaults, reminder calculation, and
rescheduling SHALL use that calendar consistently. A calendar change SHALL affect future due-date
calculations only and SHALL NOT silently recalculate tasks that already exist.

#### Scenario: Weekend is not a Firm Business Day

- **GIVEN** the firm calendar
- **WHEN** the platform counts Firm Business Days
- **THEN** Saturdays and Sundays are excluded

#### Scenario: Calendar exclusion is honoured

- **GIVEN** a date marked as a firm closure day on the calendar
- **WHEN** the platform calculates a default due date
- **THEN** that date is not selected as a Firm Business Day

#### Scenario: Calendar change does not recalculate existing tasks

- **GIVEN** an existing Open Task with a due date
- **WHEN** a Platform User adds a closure day to the firm calendar
- **THEN** the existing task's due date is not silently changed
- **AND** Audit History records the calendar change with actor and timestamp

### Requirement: Task due time

A task SHALL be due at 5:00 p.m. Firm Time on its selected Firm Business Day and SHALL be Overdue
immediately afterward.

#### Scenario: Due time is 5:00 p.m. Firm Time

- **GIVEN** any task with a due date
- **WHEN** a Platform User views it
- **THEN** its due time is 5:00 p.m. Firm Time on that date

### Requirement: First follow-up task default due date

Activating a Referral SHALL start the scheduling clock; the Referral Received Date SHALL NOT set the
due date. The preferred default due date SHALL be 5:00 p.m. Firm Time on the Friday of the same
calendar week as activation, shifted to the nearest preceding Firm Business Day when that Friday is
excluded by the firm calendar. When that target would allow fewer than three Firm Business Days
after activation, the task SHALL instead be due at 5:00 p.m. Firm Time on the third Firm Business
Day after activation and SHALL NOT be delayed to the following Friday. The activation day SHALL be
excluded from the count. Activation after 5:00 p.m. Firm Time or on a day that is not a Firm
Business Day SHALL be treated as occurring on the next Firm Business Day. Staff MAY perform the
task earlier or move its due date.

#### Scenario: Friday of the same week is used

- **GIVEN** a Referral activated on a Monday that is a Firm Business Day, with that week's Friday a Firm Business Day
- **WHEN** the first follow-up task is created
- **THEN** it is due at 5:00 p.m. Firm Time on that Friday

#### Scenario: Excluded Friday shifts to the preceding Firm Business Day

- **GIVEN** a Referral activated on a Monday where that week's Friday is a firm closure day and Thursday is a Firm Business Day
- **WHEN** the first follow-up task is created
- **THEN** it is due at 5:00 p.m. Firm Time on that Thursday

#### Scenario: Three-Firm-Business-Day floor applies

- **GIVEN** a Referral activated on a Wednesday that is a Firm Business Day
- **WHEN** the Friday of the same calendar week would leave fewer than three Firm Business Days after the activation day
- **THEN** the task is due at 5:00 p.m. Firm Time on the third Firm Business Day after activation
- **AND** it is not delayed to the following Friday

#### Scenario: After-hours activation counts from the next Firm Business Day

- **GIVEN** a Referral activated at 6:00 p.m. Firm Time on a Firm Business Day
- **WHEN** the first follow-up task is created
- **THEN** the scheduling calculation treats the activation as occurring on the next Firm Business Day

#### Scenario: Weekend activation counts from the next Firm Business Day

- **GIVEN** a Referral activated on a Sunday
- **WHEN** the first follow-up task is created
- **THEN** the scheduling calculation treats the activation as occurring on the following Monday, or on the next Firm Business Day if Monday is excluded

### Requirement: Persistent reminder indicator

A reminder SHALL appear at 9:00 a.m. Firm Time one Firm Business Day before a task's due date. It
SHALL be a persistent derived indicator shown in the navigation indicator and the Tasks page
reminder section. It SHALL persist until the task is completed, cancelled, or rescheduled and
SHALL become overdue visibility after the due time. Dismiss, snooze, email, push, and separate
notification records SHALL NOT exist in this slice. If task creation or rescheduling occurs after
the calculated reminder time has already passed, the reminder indicator SHALL appear immediately
when the save succeeds.

#### Scenario: Reminder appears one Firm Business Day before due

- **GIVEN** an Open Task due at 5:00 p.m. Firm Time on a Firm Business Day
- **WHEN** the Firm Time Zone clock reaches 9:00 a.m. on the preceding Firm Business Day
- **THEN** the reminder indicator appears for that task

#### Scenario: Reminder appears immediately when its time has passed

- **GIVEN** a task being created or rescheduled at 2:00 p.m. Firm Time for a due date whose 9:00 a.m. reminder time has already passed
- **WHEN** the save succeeds
- **THEN** the reminder indicator appears immediately

#### Scenario: Reminder persists until the task changes

- **GIVEN** a task whose reminder indicator is showing
- **WHEN** a Platform User views the task without completing, cancelling, or rescheduling it
- **THEN** the indicator remains

#### Scenario: No dismiss or snooze is offered

- **GIVEN** a reminder indicator
- **WHEN** a Platform User looks for dismiss or snooze
- **THEN** neither action exists

#### Scenario: Rescheduling clears and recalculates the indicator

- **GIVEN** a task whose reminder indicator is showing
- **WHEN** a Platform User reschedules it to a later valid Firm Business Day
- **THEN** the indicator is recalculated from the new due date

### Requirement: Reminders reach both the task assignee and the record owner

A task's reminder and overdue visibility SHALL reach both the task assignee and the current
Referral Owner when those are different Associates.

#### Scenario: Owner and assignee both see the reminder

- **GIVEN** a task assigned to Associate A on a Referral owned by Associate B
- **WHEN** the reminder time is reached
- **THEN** the reminder indicator is visible for both Associate A and Associate B

### Requirement: Manual rescheduling constraints

Manual rescheduling SHALL accept only Firm Business Days that have not passed. The current day
SHALL be selectable before 5:00 p.m. Firm Time; after that time the earliest selectable date SHALL
be the next Firm Business Day. Both "has not passed" and the 5:00 p.m. boundary SHALL be evaluated
against server time resolved in the Firm Time Zone, never against a clock supplied by the client,
and the server SHALL enforce the constraint regardless of what the web app offered. No reason SHALL be required. Audit History SHALL retain the previous
date, the new date, the actor, and the timestamp.

#### Scenario: Past date is rejected

- **GIVEN** a rescheduling action
- **WHEN** a Platform User selects a date earlier than the current Firm Time Zone date
- **THEN** the platform rejects it and the due date is unchanged

#### Scenario: Non-business day is rejected

- **GIVEN** a rescheduling action
- **WHEN** a Platform User selects a Saturday or a firm closure day
- **THEN** the platform rejects it and the due date is unchanged

#### Scenario: Today is selectable before 5:00 p.m.

- **GIVEN** the current time is 3:00 p.m. Firm Time on a Firm Business Day
- **WHEN** a Platform User reschedules a task to today
- **THEN** the platform accepts it

#### Scenario: Today is not selectable after 5:00 p.m.

- **GIVEN** the current server time is 5:30 p.m. Firm Time
- **WHEN** a Platform User opens the rescheduling action
- **THEN** the earliest selectable date is the next Firm Business Day

#### Scenario: The server rejects a date the client should not have offered

- **GIVEN** the current server time is 5:30 p.m. Firm Time
- **WHEN** a request arrives rescheduling a task to today
- **THEN** the server rejects it and the due date is unchanged

#### Scenario: Rescheduling requires no reason but is audited

- **GIVEN** a valid rescheduling
- **WHEN** the Platform User saves without entering a reason
- **THEN** the platform accepts the change
- **AND** Audit History records the previous date, the new date, the actor, and the timestamp

### Requirement: `Due on non-business day` flag and Needs Attention

When a later firm-calendar change makes an existing task's due date a non-business day, the
platform SHALL NOT move the task silently. It SHALL flag the task `Due on non-business day`, SHALL
show it in Needs Attention, and SHALL keep the flag until a Platform User manually selects a valid
date. Both the calendar change and the later task change SHALL remain in Audit History.

#### Scenario: Calendar change flags an existing task

- **GIVEN** an Open Task due on a date that is currently a Firm Business Day
- **WHEN** a Platform User adds that date to the firm calendar as a closure day
- **THEN** the task is flagged `Due on non-business day` and appears in Needs Attention
- **AND** its due date is not changed

#### Scenario: Flag clears on manual reschedule

- **GIVEN** a task flagged `Due on non-business day`
- **WHEN** a Platform User reschedules it to a valid Firm Business Day
- **THEN** the flag is removed and the task leaves Needs Attention
- **AND** Audit History records the previous date, the new date, the actor, and the timestamp

#### Scenario: Flag persists until manually resolved

- **GIVEN** a task flagged `Due on non-business day`
- **WHEN** no Platform User reschedules it
- **THEN** the flag and its Needs Attention entry remain

### Requirement: Tasks page organization

The Tasks page SHALL initially show tasks assigned to the Associate linked to the acting Platform
User and SHALL allow switching to another Associate or to all firm tasks. It SHALL use separate
sections for overdue tasks, tasks due today, non-overdue tasks due within the next five Firm
Business Days, and tasks completed today. Tasks SHALL be ordered by due time, with the
longest-overdue task first in the overdue section and the earliest due task first in the current
and upcoming sections. Each row SHALL show the task type, the related record, the related Referral
when applicable, the due date and time, the task assignee, the Referral Owner when applicable, the
current workflow stage or status, and a clear overdue indicator. The page SHALL support filters for
assignee, record owner, task type, status, due-date range, overdue state, and related record.
Selecting a task SHALL open its task-specific form in the context of its related record. The Tasks
page SHALL NOT provide a generic completion checkbox that could bypass required activity details.

#### Scenario: Default filter is the acting identity's Associate

- **GIVEN** tasks assigned to several synthetic Associates
- **WHEN** a Platform User opens the Tasks page without changing filters
- **THEN** only tasks assigned to the Associate linked to the acting Platform User are shown

#### Scenario: Overdue ordering

- **GIVEN** three overdue tasks with different due times
- **WHEN** a Platform User views the overdue section
- **THEN** the longest-overdue task appears first

#### Scenario: Completed today remains visible

- **GIVEN** a task completed earlier today
- **WHEN** a Platform User views the Tasks page
- **THEN** the task appears in a collapsed `Completed today` section until the end of that Firm Time Zone day

#### Scenario: No generic completion checkbox

- **GIVEN** a follow-up task row on the Tasks page
- **WHEN** a Platform User attempts to complete it directly from the row
- **THEN** no generic completion checkbox exists and the platform opens the task-specific form instead
