## Purpose

Associates are the internal staff records used for ownership, assignment, and activity
participation. This capability owns synthetic Associate records, the fixed `Evaluation User`
development identity that operates the evaluation slice, firm-calendar maintenance, and the rule
that an Associate cannot be deactivated while they still own work.

## ADDED Requirements

### Requirement: Associate record

An Associate SHALL record a name, a work email address, an active or inactive status, and the
linked Platform User account. Associate identity SHALL NOT imply a role or a permission level.
Creating an Associate business record SHALL be distinct from granting an authenticated account
access to the platform.

#### Scenario: Synthetic Associate is created

- **GIVEN** a Platform User creating a synthetic Associate
- **WHEN** they supply a name, a work email address, and active status and save
- **THEN** the platform creates the Associate
- **AND** Audit History records the creation with actor and timestamp

#### Scenario: Associate record grants no access

- **GIVEN** a newly created Associate
- **WHEN** a Platform User views it
- **THEN** the Associate carries no role or permission level, and creating it grants no sign-in access

### Requirement: Synthetic Associates are selectable for work

Active Associates SHALL be selectable as Referral Owners, task assignees, and activity
participants. Inactive Associates SHALL NOT be selectable for new ownership, new assignment, or new
activity, while their earlier assignments and recorded actions SHALL remain visible.

#### Scenario: Active Associate is selectable

- **GIVEN** an active synthetic Associate
- **WHEN** a Platform User selects a Referral Owner or a task assignee
- **THEN** that Associate is available for selection

#### Scenario: Inactive Associate is not selectable for new work

- **GIVEN** an inactive Associate
- **WHEN** a Platform User selects a Referral Owner or a task assignee
- **THEN** that Associate is not offered
- **AND** their earlier assignments and recorded actions remain visible in history

### Requirement: The fixed `Evaluation User` development identity

The slice SHALL be operated by one fixed, clearly marked development identity linked to a synthetic
`Evaluation User` Associate. The Audit History actor SHALL be that authenticated Platform User
identity as established at the request boundary. It SHALL NOT be selectable, defaultable, or
overridable by any request field, header, or body value, and no editable Associate selection SHALL
stand in for the identity of the acting user. That identity SHALL also be the default Associate
recorded as having performed a call and the default Tasks page filter; those two are defaults the
Platform User may change, while the actor is not.

In this slice the requirement is satisfied by a fixed identity that no request field, header, or
body value can influence; it is not evidence that an authentication mechanism has been built or
tested. It becomes an authenticated-identity requirement when authentication arrives, without
changing the meaning of any actor already recorded in Audit History. Other synthetic
Associates SHALL remain selectable for assignments and for recording activity on another
Associate's behalf. Real authentication and Platform User provisioning SHALL NOT be part of this
slice.

#### Scenario: The actor comes from the request boundary

- **GIVEN** the evaluation build
- **WHEN** any Platform User action is recorded in Audit History
- **THEN** the acting Platform User is the fixed development identity established at the request boundary

#### Scenario: A request cannot choose its own actor

- **GIVEN** a request that carries an actor, user, or on-behalf-of value in its body, query string, or headers
- **WHEN** the platform processes it
- **THEN** the request is rejected or the supplied value is ignored
- **AND** the recorded actor is still the identity established at the request boundary

#### Scenario: Changing the call performer does not change the actor

- **GIVEN** a follow-up attempt recorded on another Associate's behalf
- **WHEN** the attempt is saved
- **THEN** the selected Associate is recorded as the caller
- **AND** the Audit History actor remains the acting Platform User

#### Scenario: Development identity is the default call performer

- **GIVEN** a follow-up attempt being recorded
- **WHEN** the Platform User does not change the caller
- **THEN** the `Evaluation User` Associate is recorded as having made the attempt

#### Scenario: Development identity is the default Tasks filter

- **GIVEN** tasks assigned to several synthetic Associates
- **WHEN** a Platform User opens the Tasks page
- **THEN** it initially shows tasks assigned to the `Evaluation User` Associate

#### Scenario: No sign-in provisioning exists

- **GIVEN** the Associates page
- **WHEN** a Platform User looks for actions to grant or revoke sign-in access
- **THEN** no such actions exist in this slice

### Requirement: Equal access for every Platform User

Every authenticated Platform User SHALL have the same visibility and the same actions across
business records in this slice. There SHALL be no role-based access control and no user-specific
visibility. Individual identity SHALL remain required for ownership, reminders, and Audit History.
This requirement, and every other "every authenticated Platform User" requirement in this change,
states V1 access **policy** and is not evidence that an access-control mechanism has been built or
tested; when authentication arrives, equal access becomes an explicit allow-all decision taken at a
real authorization checkpoint.

#### Scenario: Any Platform User may view any Associate's work

- **GIVEN** tasks and Referrals owned by several synthetic Associates
- **WHEN** a Platform User switches the Tasks page to another Associate or to all firm tasks
- **THEN** the platform shows that work without restriction

### Requirement: Firm holiday and closure calendar maintenance

The evaluation identity SHALL be able to maintain the firm holiday and closure calendar used for
Firm Business Day calculations. Calendar maintenance SHALL be reached from the Associates surface,
which is the page surface that holds it in this slice. Calendar changes SHALL remain in Audit History and SHALL affect
future due-date calculations only. Administrative permission enforcement SHALL NOT be part of this
slice.

#### Scenario: Calendar maintenance is reachable

- **GIVEN** the evaluation build's Associates surface
- **WHEN** a Platform User looks for firm holiday and closure calendar maintenance
- **THEN** it is reachable from there

#### Scenario: Calendar day is added

- **GIVEN** the firm calendar
- **WHEN** the evaluation identity adds a closure day and saves
- **THEN** the platform stores the closure day
- **AND** Audit History records the previous value, the new value, the actor, and the timestamp

#### Scenario: Calendar change affects future calculations only

- **GIVEN** an existing Open Task with a due date
- **WHEN** the evaluation identity changes the calendar
- **THEN** future due-date calculations use the new calendar
- **AND** the existing task's due date is not recalculated

### Requirement: An Associate owning work cannot be deactivated

The platform SHALL refuse to make an Associate inactive while they are the Referral Owner of a
non-closed Referral or the assignee of an Open Task. The Platform User SHALL reassign that work
first. The Associate's earlier assignments and actions SHALL remain in Audit History. When real
authentication arrives, revocation of a Platform User's sign-in access SHALL be independent of this
rule and SHALL never be blocked by owned work.

#### Scenario: Deactivation is refused while work is owned

- **GIVEN** an Associate who owns an `In process` Referral
- **WHEN** a Platform User attempts to make that Associate inactive
- **THEN** the platform refuses, identifies the owned work that must be reassigned, and leaves the Associate active

#### Scenario: Deactivation is refused while an Open Task is assigned

- **GIVEN** an Associate assigned an Open Task
- **WHEN** a Platform User attempts to make that Associate inactive
- **THEN** the platform refuses and identifies the Open Task

#### Scenario: Deactivation succeeds after reassignment

- **GIVEN** an Associate whose owned Referrals and Open Tasks have all been reassigned
- **WHEN** a Platform User makes that Associate inactive
- **THEN** the platform accepts the change
- **AND** Audit History records the deactivation with actor and timestamp
- **AND** the Associate's earlier assignments and recorded actions remain visible
