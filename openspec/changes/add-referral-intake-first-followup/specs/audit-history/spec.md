## Purpose

Audit History preserves accountable history for every successful business and administrative change
so that staff can later establish what changed, who changed it, and when. This capability owns what
is and is not recorded, the content of each entry, how it is accessed, and its all-or-nothing
relationship with the change it audits.

## ADDED Requirements

### Requirement: Every successful change is recorded

Every successful change to a business or administrative record SHALL produce an Audit History
entry. Coverage SHALL be complete by default: a change is recorded unless it appears in the closed
exclusion list below. This includes, and is not limited to, Household and Household Member changes,
Referral changes, Referral Source changes, ownership changes, activation, task creation,
assignment, rescheduling, completion and cancellation, accepted duplicate warnings, outreach
activity and its corrections, dispositions and reopenings, Associate changes, and firm holiday and
closure calendar changes. An implementer SHALL NOT decide that a change is unimportant enough to
omit.

#### Scenario: An ownership change is recorded

- **GIVEN** an `In process` Referral
- **WHEN** a Platform User changes its Referral Owner and the save succeeds
- **THEN** Audit History contains an entry for that change

#### Scenario: An accepted duplicate warning is recorded

- **GIVEN** an exact Household Member name match
- **WHEN** a Platform User explicitly accepts the possible-duplicate warning and the Household is created
- **THEN** Audit History records the accepted warning with the accepting Platform User and the acceptance timestamp

#### Scenario: A firm-calendar change is recorded

- **GIVEN** the firm holiday and closure calendar
- **WHEN** the evaluation identity adds or removes a closure day
- **THEN** Audit History records the change with previous value, new value, actor, and timestamp

### Requirement: The exclusion list is closed and exhaustive

The following, and only the following, SHALL be excluded from Audit History:

1. Record views and other read-only access.
2. Unsaved input.
3. Validation failures.
4. Rejected stale-save conflicts.
5. Failed sign-in attempts.
6. Internal errors and technical failure detail.
7. The environment reset operation.

Nothing else SHALL be excluded. Adding an entry to this list SHALL require a recorded human
decision, not an implementation judgement. Technical failure details SHALL NOT be added to the
business record. Failed sign-ins and system failures belong in restricted technical and security
logs, which are outside this slice.

#### Scenario: A rejected stale save is not recorded

- **GIVEN** a save rejected because it was based on an outdated version
- **WHEN** a Platform User opens the record's Audit History
- **THEN** the rejected save does not appear

#### Scenario: A validation failure is not recorded

- **GIVEN** an activation attempt rejected because required information was missing
- **WHEN** a Platform User opens the Referral's Audit History
- **THEN** no activation entry and no technical failure detail appear

#### Scenario: Viewing a record is not recorded

- **GIVEN** a Household detail view
- **WHEN** a Platform User opens it without changing anything
- **THEN** Audit History gains no entry

### Requirement: Audit History entry content

Every Audit History entry SHALL contain the previous value, the new value, the Platform User who
made the change, and the timestamp of the change displayed in the Firm Time Zone. Where a change
requires a reason — discard, skip, correction, `Entered in error`, reversal, or reopening — the
entry SHALL retain that reason.

#### Scenario: Previous and new values are retained

- **GIVEN** a task rescheduled from one Firm Business Day to another
- **WHEN** a Platform User opens the task's Audit History
- **THEN** the entry shows the previous due date, the new due date, the acting Platform User, and the timestamp in the Firm Time Zone

#### Scenario: A required reason is retained

- **GIVEN** a `Draft` Referral discarded with a reason
- **WHEN** a Platform User opens its Audit History
- **THEN** the entry shows the reason together with the actor and timestamp

### Requirement: Contextual per-record access

Audit History SHALL be reached through an `Audit history` action on the applicable record detail
view and SHALL NOT be displayed inline in normal workflow views. Every authenticated Platform User
SHALL be able to open it. A global Audit History page SHALL NOT exist in this slice.

#### Scenario: History opens from the record

- **GIVEN** a Referral detail view
- **WHEN** a Platform User activates the `Audit history` action
- **THEN** the platform shows the history for that Referral

#### Scenario: Historical values stay out of daily views

- **GIVEN** a record whose values have changed several times
- **WHEN** a Platform User views it in normal workflow
- **THEN** only current values are shown

#### Scenario: No global audit page exists

- **GIVEN** the platform navigation
- **WHEN** a Platform User looks for a global Audit History page
- **THEN** no such page exists in this slice

### Requirement: Audited changes are all-or-nothing

Because every non-excluded change is recorded, every such change SHALL succeed together with its
history entry or SHALL fail entirely. The platform SHALL NOT accept an unaudited change for later
repair. The Platform User MAY retry. Read-only access MAY remain available while audit writes are
failing.

#### Scenario: Audit write failure rejects the change

- **GIVEN** a Platform User saving a change that requires an Audit History entry
- **WHEN** the Audit History entry cannot be written
- **THEN** the business change is not applied
- **AND** the Platform User receives an error and may retry

#### Scenario: Retry after an audit failure applies the change once

- **GIVEN** a change that failed because its audit entry could not be written
- **WHEN** the Platform User retries successfully
- **THEN** the change is applied exactly once with exactly one Audit History entry

#### Scenario: Reading remains possible during audit-write failure

- **GIVEN** audit writes are failing
- **WHEN** a Platform User opens a record
- **THEN** the platform may still show the record read-only

### Requirement: Error content when an audited write fails

An error returned when an audited write fails SHALL identify the failure class and state that the
Platform User may retry. It SHALL NOT contain database, query, stack, or configuration detail.

#### Scenario: Failure is explained without internal detail

- **GIVEN** a change whose Audit History entry cannot be written
- **WHEN** the Platform User receives the error
- **THEN** the error names the failure class and states that retry is possible
- **AND** it contains no database name, query text, stack trace, or configuration value

### Requirement: Audit History entries are immutable once written

An Audit History entry SHALL be immutable once written. The platform SHALL NOT provide any action
to delete or edit an entry, and the database role the application uses SHALL hold no `UPDATE` or
`DELETE` privilege on the audit store, so that an application defect cannot alter recorded history.
Corrections SHALL be recorded as further entries.

#### Scenario: No delete or edit action exists

- **GIVEN** an Audit History view
- **WHEN** a Platform User looks for a delete or edit action
- **THEN** no such action exists

#### Scenario: The application cannot alter a written entry

- **GIVEN** a written Audit History entry
- **WHEN** the application attempts an `UPDATE` or `DELETE` against the audit store
- **THEN** the database refuses the operation because the application role holds no such privilege

#### Scenario: A correction adds an entry

- **GIVEN** a value corrected after an earlier mistake
- **WHEN** the correction succeeds
- **THEN** Audit History contains both the original change and the correction
