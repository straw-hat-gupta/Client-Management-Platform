## Purpose

Minimal Event and Centre of Influence (COI) records exist so that Event and COI Referral Sources
can identify a specific originating party. This capability owns their creation from Referral
intake, their duplicate rules, and their deactivation and reactivation. It deliberately excludes
broader Event and COI management.

## ADDED Requirements

### Requirement: Minimal Event reference record

An Event reference record SHALL require an Event name and an Event date and SHALL require nothing
else in this slice. It SHALL be selectable as an Event Referral Source. Event capacity, invitees,
invitations, responses, attendance, statuses, and cost SHALL NOT be part of this slice.

#### Scenario: Event is created from name and date

- **GIVEN** a Platform User creating an Event reference record
- **WHEN** they supply an Event name and an Event date and save
- **THEN** the platform creates the Event record and makes it selectable as an Event Referral Source
- **AND** Audit History records the creation with actor and timestamp

#### Scenario: Event without a date is rejected

- **GIVEN** the Event creation form
- **WHEN** the Platform User supplies a name but no date
- **THEN** the platform rejects the save, states that an Event date is required, and creates no record

### Requirement: Minimal COI reference record

A COI SHALL be an individual person, not an organization record. A COI reference record SHALL
require a name and at least one phone number or email address. It SHALL be selectable as a COI
Referral Source.

#### Scenario: COI is created from name and one contact method

- **GIVEN** a Platform User creating a COI person record
- **WHEN** they supply a name and one email address and save
- **THEN** the platform creates the COI record and makes it selectable as a COI Referral Source

#### Scenario: COI without a contact method is rejected

- **GIVEN** the COI creation form
- **WHEN** the Platform User supplies only a name
- **THEN** the platform rejects the save, states that at least one phone number or email address is required, and creates no record

#### Scenario: An organization cannot be recorded as a COI

- **GIVEN** the COI creation form
- **WHEN** a Platform User looks for an organization-record option
- **THEN** no such option exists; a COI is an individual person

### Requirement: Inline creation from Referral intake

Referral intake SHALL allow creating a minimal Event from its name and date, and a minimal COI
person from their name plus at least one phone number or email address, without leaving intake.
Inline creation SHALL apply the same required fields and the same duplicate rules as standalone
creation.

#### Scenario: Event created inline during intake

- **GIVEN** a Platform User attributing an Event Referral Source during Referral intake and finding no matching Event
- **WHEN** they create the Event inline from its name and date
- **THEN** the platform creates the Event record and attributes it to the Referral in the same intake save

#### Scenario: Inline creation obeys the duplicate rules

- **GIVEN** an existing COI with the email address `synthetic.coi@example.invalid`
- **WHEN** a Platform User attempts to create a COI inline using that same email address
- **THEN** the platform blocks creation and directs the Platform User to the existing COI record

### Requirement: Duplicate blocking for reference records

The platform SHALL block creation of a duplicate reference record and SHALL provide no override and
no reference-record merge. An exact Event name-and-date match SHALL block Event creation. An exact
COI phone number or email address match SHALL block COI creation. A matching COI name SHALL block
creation until the Platform User selects the existing person or provides a distinct phone number or
email address. In every case the platform SHALL direct the Platform User to the existing record.
Duplicate checks SHALL include inactive records.

#### Scenario: Exact Event name and date blocks creation

- **GIVEN** an existing Event named `Synthetic Client Seminar` dated 2026-05-14
- **WHEN** a Platform User attempts to create another Event with the same name and date
- **THEN** the platform blocks creation and directs the Platform User to the existing Event

#### Scenario: Same Event name on a different date is allowed

- **GIVEN** the same existing Event
- **WHEN** a Platform User creates an Event with that name and a different date
- **THEN** the platform accepts the creation

#### Scenario: COI name match blocks until resolved

- **GIVEN** an existing COI named `Alex Sample`
- **WHEN** a Platform User attempts to create another COI named `Alex Sample`
- **THEN** the platform blocks creation
- **AND** the Platform User may either select the existing COI or supply a distinct phone number or email address to proceed

#### Scenario: No override exists

- **GIVEN** a blocked duplicate reference-record creation
- **WHEN** the Platform User looks for an override or a merge action
- **THEN** neither exists

### Requirement: Inactive reference records are reactivated rather than recreated

When a duplicate check matches an inactive Event or COI, the platform SHALL reactivate that record
rather than creating a new one. Reactivation SHALL be recorded in Audit History.

#### Scenario: Inactive COI is reactivated

- **GIVEN** an inactive COI with the phone number `+1-555-0142`
- **WHEN** a Platform User attempts to create a COI with that phone number
- **THEN** the platform reactivates the existing COI rather than creating a new record
- **AND** Audit History records the reactivation with actor and timestamp

### Requirement: Deactivation preserves existing attribution

Deactivating an Event or COI SHALL preserve every existing Referral Source link to it and SHALL
remove it from new source selection by default. Platform Users SHALL be able to include inactive
reference records in search and to reactivate them. Both deactivation and reactivation SHALL remain
in contextual Audit History.

#### Scenario: Existing attribution survives deactivation

- **GIVEN** a COI attributed as the Referral Source of an `In process` Referral
- **WHEN** a Platform User deactivates that COI
- **THEN** the Referral keeps its COI Referral Source attribution
- **AND** the COI no longer appears in new source selection by default

#### Scenario: Inactive reference records can be found and reactivated

- **GIVEN** an inactive Event
- **WHEN** a Platform User searches with inactive records included and reactivates it
- **THEN** the Event becomes active and selectable again
- **AND** Audit History records both transitions with actor and timestamp

### Requirement: Inline reference records survive a discarded Draft

An Event or COI created inline during Referral intake SHALL remain active when that Draft Referral
is later discarded, because reference records are independent and reusable. A mistaken Event or COI
SHALL be deactivated separately.

#### Scenario: Discarding a Draft leaves its inline COI active

- **GIVEN** a `Draft` Referral that created a COI inline during intake
- **WHEN** a Platform User discards that Draft
- **THEN** the COI remains active and selectable
- **AND** the Platform User may deactivate it separately if it was a mistake
