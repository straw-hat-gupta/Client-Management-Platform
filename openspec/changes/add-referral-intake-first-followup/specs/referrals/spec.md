## Purpose

The Referral is the continuing pre-client intake record for a Household. This capability owns the
Draft lifecycle, Referral Source attribution, the Referral Received Date, the atomic activation
transition to `In process`, the invariants an active Referral must keep satisfying, ownership, and
the `Discarded Draft` state.

## ADDED Requirements

### Requirement: Explicit Draft creation without autosave

A Draft Referral SHALL exist only after an explicit successful `Save Draft` action. The platform
SHALL NOT autosave business records in this slice. Leaving a form with unsaved changes SHALL warn
the Platform User. Every successful later save SHALL update the Draft and appear in Audit History.

#### Scenario: Draft is created only on explicit save

- **GIVEN** a Platform User has entered intake information into the `New Referral` form
- **WHEN** they have not yet activated `Save Draft`
- **THEN** no Referral, Household, or Household Member record exists

#### Scenario: Navigating away with unsaved changes warns

- **GIVEN** a `New Referral` form with unsaved changes
- **WHEN** the Platform User navigates away
- **THEN** the platform warns that the changes are unsaved

#### Scenario: Later Draft edits are audited

- **GIVEN** a saved Draft Referral
- **WHEN** the Platform User changes a value and saves successfully
- **THEN** the platform stores the change
- **AND** Audit History records the previous value, new value, actor, and timestamp

### Requirement: Minimum Draft content and missing-information visibility

A Draft Referral SHALL identify an existing Household or create a new Household with at least one
named Household Member. Contact information, Referral Source, Referral Received Date, and Referral
Owner MAY remain incomplete in `Draft`. A Draft SHALL create no task. The platform SHALL clearly
identify the information still required for activation.

#### Scenario: Draft saved with only a named member

- **GIVEN** a Platform User creating a new Household through Referral intake
- **WHEN** they supply one named Household Member and save the Draft
- **THEN** the platform creates the Household and a `Draft` Referral
- **AND** creates no task
- **AND** shows that contact information, Referral Source, Referral Received Date, and Referral Owner are still required for activation

#### Scenario: Draft without a Household is rejected

- **GIVEN** a `New Referral` form
- **WHEN** the Platform User saves without selecting an existing Household and without naming at least one Household Member
- **THEN** the platform rejects the save, preserves the entered values, and creates no record

#### Scenario: Draft may have no Referral Owner

- **GIVEN** a `Draft` Referral with no Referral Owner
- **WHEN** the Platform User saves it
- **THEN** the platform accepts the save
- **AND** the Referral appears in the `Needs attention` view as an unowned Draft Referral

### Requirement: Atomic creation of a new Household through Referral intake

When Referral intake creates a new Household, saving the Household, its Household Members, its
Referral Sources, and the Draft Referral SHALL be all-or-nothing. Failure SHALL create none of
those records, SHALL preserve the Platform User's entered form values for correction, and SHALL
permit retry. Standalone Pre-existing Client Household setup SHALL remain a separate intentional
action.

#### Scenario: Partial failure creates nothing

- **GIVEN** a Platform User saves a new Household, two members, one Referral Source, and a Draft Referral in one intake save
- **WHEN** any part of that save fails
- **THEN** the platform creates no Household, no Household Member, no Referral Source, and no Referral
- **AND** preserves the entered form values so the Platform User can correct and retry
- **AND** records nothing in Audit History for the failed save

#### Scenario: Retry after failure succeeds once

- **GIVEN** an intake save failed and created nothing
- **WHEN** the Platform User retries the same save successfully
- **THEN** the platform creates exactly one Household, its members, its sources, and one Draft Referral

### Requirement: Referral Source types and required attribution

A Referral MAY have multiple Referral Sources. All applicable sources SHALL receive equal
attribution and no source SHALL be primary. The platform SHALL support all six Referral Source
types and SHALL require each type's attribution before activation:

| Code | Source | Required attribution |
|---|---|---|
| C | Client | An existing client Household or client Household Member selected from the platform |
| E | Event | The specific Event name and date |
| M | Marketing | The campaign or channel name |
| SM | Social Media | The platform name |
| COI | Centre of Influence | An existing COI person selected from the platform |
| O | Other | A short source description |

A Client Referral Source SHALL be selected from existing client records; temporary names SHALL NOT
be allowed. A COI Referral Source SHALL be selected from existing COI person records; temporary
COI names and their resolution tasks are outside this slice.

#### Scenario: Marketing source requires a campaign or channel name

- **GIVEN** a Referral with a Marketing Referral Source and no campaign or channel name
- **WHEN** the Platform User attempts to activate the Referral
- **THEN** the platform rejects the activation, identifies the missing campaign or channel name, leaves the Referral `Draft`, and creates no task

#### Scenario: Multiple sources receive equal attribution

- **GIVEN** a Referral attributed to a Client source and an Event source
- **WHEN** the Platform User views the Referral
- **THEN** both sources are shown with equal attribution and neither is marked primary

#### Scenario: Temporary COI name is not accepted

- **GIVEN** a Platform User attributing a COI Referral Source
- **WHEN** they attempt to enter a temporary COI name instead of selecting an existing COI person
- **THEN** the platform does not accept a temporary name and requires selecting or inline-creating a COI person record

### Requirement: Referral Relationship on a Client Referral Source

Each Client Referral Source SHALL record its Referral Relationship to the prospective Household
using one of `Friend`, `Relative`, `Colleague`, `Service provider`, or `Other`. The Referral
Relationship SHALL be `N/A` when the Referral did not come through a client. The relationship
SHALL apply to the prospective Household collectively; per-member Referral participation is
outside this slice.

#### Scenario: Relationship is recorded per Client source

- **GIVEN** a Referral with two Client Referral Sources
- **WHEN** the Platform User records `Friend` for one and `Colleague` for the other
- **THEN** the platform retains each relationship against its own Client Referral Source

#### Scenario: Non-client Referral has no relationship

- **GIVEN** a Referral attributed only to a Social Media source
- **WHEN** the Platform User views its attribution
- **THEN** the Referral Relationship is `N/A`

### Requirement: Referral Received Date

The Referral Received Date SHALL be the calendar date in the Firm Time Zone on which the firm first
received the Referral. It SHALL default to the current date in the Firm Time Zone. The Platform
User MAY backdate it. The platform SHALL reject a future Referral Received Date. Adding a later
Referral Source SHALL NOT change the Referral's original Referral Received Date and SHALL NOT
create a second Referral Received Date.

#### Scenario: Future date is rejected

- **GIVEN** a Referral being saved or activated
- **WHEN** the Referral Received Date is later than the current date in the Firm Time Zone
- **THEN** the platform rejects the save, states that a future date is not accepted, and changes no record

#### Scenario: Backdating is accepted

- **GIVEN** a Referral entered after a delay
- **WHEN** the Platform User sets the Referral Received Date to an earlier date
- **THEN** the platform accepts it and records the change in Audit History

#### Scenario: A later source does not reset the date

- **GIVEN** an `In process` Referral with a Referral Received Date
- **WHEN** the Platform User adds an additional Referral Source
- **THEN** the Referral retains its original Referral Received Date
- **AND** Audit History records the added source with actor and timestamp

### Requirement: One non-discarded Referral per Household

A Household SHALL have at most one non-discarded Referral at a time. When a Household that already
has a Referral reaches the firm again, the platform SHALL offer adding a Referral Source or
reopening the existing Referral rather than creating a second Referral.

#### Scenario: Returning Household adds a source

- **GIVEN** a Household with an `In process` Referral
- **WHEN** the Platform User opens that Household to record a new introduction
- **THEN** the platform offers `Add Referral Source` rather than creating a second Referral

#### Scenario: Second Referral is refused

- **GIVEN** a Household with a non-discarded Referral
- **WHEN** creation of another Referral for that Household is attempted
- **THEN** the platform refuses and directs the Platform User to the existing Referral

### Requirement: A client Household cannot receive a Referral

A client Household MAY be selected as a Client Referral Source for a different prospective
Household. The platform SHALL NOT allow a client Household to itself receive a Referral. Potential
business from an existing client Household is outside this slice.

#### Scenario: Client Household is selectable as a source

- **GIVEN** a Pre-existing Client Household
- **WHEN** a Platform User attributes a Client Referral Source on a different prospective Household
- **THEN** the client Household is selectable

#### Scenario: Client Household cannot be the subject of a Referral

- **GIVEN** a client Household
- **WHEN** a Platform User attempts to create a Referral whose subject is that client Household
- **THEN** the platform refuses and states that existing-client potential business is outside this slice

### Requirement: Referral activation is one atomic transition

Activation SHALL validate the whole transition at once and SHALL require: at least one named
Household Member, at least one phone number or email address across the prospective Household, at
least one complete Referral Source with its source-specific attribution, a non-future Referral
Received Date, an active Referral Owner, and no blocking duplicate conflict. The Referral SHALL
move to `In process` only when exactly one first follow-up task and its due date are created
successfully. Failure SHALL leave the Referral `Draft`, preserve entered values, create no task,
and present all validation problems together. Retry SHALL create exactly one first follow-up task.

#### Scenario: Valid activation creates exactly one task

- **GIVEN** a `Draft` Referral satisfying every activation requirement
- **WHEN** the Platform User activates it
- **THEN** the Referral status becomes `In process`
- **AND** exactly one first follow-up task exists with a calculated due date
- **AND** Audit History records the activation with actor and timestamp

#### Scenario: All validation problems are presented together

- **GIVEN** a `Draft` Referral missing contact information and a Referral Owner and carrying a future Referral Received Date
- **WHEN** the Platform User attempts activation
- **THEN** the platform presents all three problems together
- **AND** the Referral remains `Draft`, entered values are preserved, and no task is created

#### Scenario: Task-creation failure leaves the Referral Draft

- **GIVEN** a `Draft` Referral satisfying every activation requirement
- **WHEN** first-task creation or due-date calculation fails
- **THEN** the Referral remains `Draft`
- **AND** no task exists
- **AND** the Platform User receives an error
- **AND** Audit History records no activation and no technical failure detail

#### Scenario: Retry after a failed activation does not duplicate

- **GIVEN** an activation attempt failed and left the Referral `Draft`
- **WHEN** the Platform User retries activation successfully
- **THEN** the Referral becomes `In process` with exactly one first follow-up task
- **AND** Audit History records only the successful activation

#### Scenario: Inactive Referral Owner blocks activation

- **GIVEN** a `Draft` Referral whose selected Referral Owner is an inactive Associate
- **WHEN** the Platform User attempts activation
- **THEN** the platform rejects the activation, states that an active Referral Owner is required, and creates no task

### Requirement: Continuing `In process` invariants

An `In process` Referral SHALL continue to satisfy its activation invariants. A save MAY replace
required values atomically. The platform SHALL reject a save that would remove the Referral's last
contact method, remove its last complete Referral Source, leave it without a Referral Owner, assign
an inactive Associate as Referral Owner, or future-date its Referral Received Date. An `In process`
Referral SHALL NOT move back to `Draft` to permit incomplete data.

#### Scenario: Last contact method cannot be removed

- **GIVEN** an `In process` Referral whose prospective Household has exactly one email address and no phone number
- **WHEN** a Platform User saves a change removing that email address
- **THEN** the platform rejects the save and states that at least one contact method is required

#### Scenario: Contact method may be replaced atomically

- **GIVEN** the same Referral
- **WHEN** a Platform User saves one change that removes the email address and adds a phone number
- **THEN** the platform accepts the save
- **AND** Audit History records both the previous and new values

#### Scenario: Return to Draft is refused

- **GIVEN** an `In process` Referral
- **WHEN** a Platform User attempts to move it back to `Draft`
- **THEN** the platform refuses and states that an active Referral does not return to `Draft`

### Requirement: Referral stale-save conflict rejection

The platform SHALL reject a save based on an outdated Referral version rather than overwriting a
newer save, SHALL tell the Platform User to reload the current record and reapply the intended
change, and SHALL record only successful saves in Audit History.

#### Scenario: Outdated Referral save is rejected

- **GIVEN** two Platform Users opened the same Referral and one has already saved a change
- **WHEN** the other saves based on the version loaded before that change
- **THEN** the platform rejects the save without overwriting the newer values
- **AND** tells the Platform User to reload and reapply the intended change
- **AND** records nothing in Audit History for the rejected save

### Requirement: Referral ownership change does not reassign existing tasks

A Referral SHALL have exactly one current Referral Owner while active. Changing the Referral Owner
SHALL NOT silently change the assignee of an existing task. The reassignment action SHALL show the
existing task assignee and SHALL permit changing both in one operation. Future generated tasks
SHALL default to the new Referral Owner. Reminders SHALL continue to reach both the task assignee
and the current Referral Owner. Any Platform User MAY assign or reassign a Referral.

#### Scenario: Existing task keeps its assignee

- **GIVEN** an `In process` Referral whose first follow-up task is assigned to Associate A while Associate B is the Referral Owner
- **WHEN** a Platform User changes the Referral Owner to Associate C without also reassigning the task
- **THEN** the first follow-up task remains assigned to Associate A
- **AND** Audit History records the ownership change with previous and new values, actor, and timestamp

#### Scenario: Owner and assignee changed in one operation

- **GIVEN** the same Referral
- **WHEN** the Platform User changes the Referral Owner and the task assignee in one reassignment operation
- **THEN** both changes take effect together
- **AND** Audit History records both

#### Scenario: Later generated tasks default to the new owner

- **GIVEN** a Referral whose Referral Owner was changed to Associate C
- **WHEN** the platform later generates the `Discovery package sent` task
- **THEN** that task is assigned to Associate C by default

### Requirement: Discarded Draft

Any Platform User MAY move a mistaken or abandoned `Draft` Referral to `Discarded Draft`. The
action SHALL require extra confirmation and a reason. The record SHALL leave active lists by
default, SHALL create no task, SHALL preserve its Household and Referral Sources, and SHALL record
the actor, reason, and timestamp. A `Discarded Draft` SHALL NOT be restorable in this slice and
SHALL NOT be permanently deleted. Only a `Draft` Referral SHALL be discardable.

#### Scenario: Draft is discarded with confirmation and reason

- **GIVEN** a `Draft` Referral
- **WHEN** a Platform User discards it with extra confirmation and a reason
- **THEN** the Referral status becomes `Discarded Draft` and it leaves active lists
- **AND** its Household and Referral Sources are preserved
- **AND** Audit History records the actor, reason, and timestamp

#### Scenario: Discard without a reason is rejected

- **GIVEN** the discard confirmation
- **WHEN** the Platform User confirms without supplying a reason
- **THEN** the platform rejects the action and the Referral remains `Draft`

#### Scenario: Discarded Draft cannot be restored

- **GIVEN** a `Discarded Draft` Referral
- **WHEN** a Platform User looks for a restore action
- **THEN** no restore action exists and the record is not permanently deleted

#### Scenario: An `In process` Referral cannot be discarded

- **GIVEN** an `In process` Referral
- **WHEN** a Platform User attempts to discard it
- **THEN** the platform refuses, because only a `Draft` Referral may be discarded

### Requirement: Discard cascade to a newly created Household

When the discarded Draft was the only relationship for a Household newly created through that
intake, the Household SHALL also become inactive and leave active lists while its members and
history are retained. A pre-existing Household, or a Household with other linked relationships,
SHALL remain unchanged. The discard SHALL update the applicable records together or neither.

#### Scenario: Newly created Household becomes inactive

- **GIVEN** a `Draft` Referral that created its Household during intake and is that Household's only relationship
- **WHEN** the Platform User discards the Draft
- **THEN** both the Referral and the Household become inactive together and leave active lists
- **AND** the Household's members and history are retained
- **AND** Audit History records both changes

#### Scenario: Existing Household is unchanged

- **GIVEN** a `Draft` Referral attached to a Household that existed before this intake
- **WHEN** the Platform User discards the Draft
- **THEN** the Referral becomes `Discarded Draft` and the Household remains active

#### Scenario: Cascade failure changes nothing

- **GIVEN** a discard that must update both the Referral and its newly created Household
- **WHEN** either update fails
- **THEN** neither record changes and the Platform User receives an error

### Requirement: Later intake after a discard

A later legitimate intake for the same Prospects SHALL reuse and reactivate the existing Household
and SHALL create a new `Draft` Referral. The earlier `Discarded Draft` SHALL remain inactive.

#### Scenario: New Draft on a reactivated Household

- **GIVEN** an inactive Household whose only Referral is a `Discarded Draft`
- **WHEN** a Platform User enters a later legitimate intake matching that Household
- **THEN** the platform reactivates the Household and creates a new `Draft` Referral
- **AND** the earlier `Discarded Draft` remains inactive
- **AND** the Household then has exactly one non-discarded Referral

### Requirement: Referral discovery and Needs attention

The Referrals page SHALL be the default landing page. It SHALL show the Household or prospective
client name, all Referral Sources, the Referral Received Date, Referral age, the current outreach
stage, the Referral Status, the Referral Owner, and the next Open Task with its due date. It SHALL
initially show `Draft` and `In process` Referrals and SHALL allow including NFAR and
`Discarded Draft` records. It SHALL support filters for Referral Owner, Referral Status, outreach
stage, Referral Source, Referral Received Date range, and task due state. A `Needs attention` view
SHALL surface unowned `Draft` Referrals and active Referrals with no Open Task, labelling the
latter `No next task`.

#### Scenario: Closed and discarded Referrals are excluded by default

- **GIVEN** `Draft`, `In process`, NFAR, and `Discarded Draft` Referrals exist
- **WHEN** a Platform User opens the Referrals page without changing filters
- **THEN** only the `Draft` and `In process` Referrals appear

#### Scenario: Active Referral with no Open Task is labelled

- **GIVEN** an `In process` Referral whose only task was cancelled
- **WHEN** a Platform User opens the `Needs attention` view
- **THEN** that Referral appears labelled `No next task` rather than being omitted

### Requirement: Equal access and no permanent deletion of Referrals

Every authenticated Platform User SHALL have equal access to view and change Referral records in
this slice. The platform SHALL NOT provide permanent deletion of a Referral, Referral Source, or
recorded activity.

#### Scenario: Any Platform User may act on any Referral

- **GIVEN** a Referral owned by one Associate
- **WHEN** a different Platform User changes its Referral Owner
- **THEN** the platform accepts the change and records the acting Platform User in Audit History

#### Scenario: No delete action is offered

- **GIVEN** a Referral detail view
- **WHEN** a Platform User looks for a permanent delete action
- **THEN** no such action exists
