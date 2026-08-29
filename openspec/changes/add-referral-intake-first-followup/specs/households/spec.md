## Purpose

The Household is the durable relationship record for one or more people managed together, from
prospective intake through the client relationship. This capability owns Household and Household
Member identity, contact information, Client Number assignment, duplicate prevention, and the
manual setup of Pre-existing Client Households used as Client Referral Sources.

## ADDED Requirements

### Requirement: Household and Household Member identity

The platform SHALL represent a Household as one durable record containing one or more Household
Members. Each Household Member SHALL have their own name and MAY have a preferred name, phone
numbers, and email addresses. The Household SHALL have an optional mailing address. A Household
SHALL NOT be required to contain more than one Household Member.

#### Scenario: Creating a Household with a single member

- **GIVEN** a Platform User is creating a Household
- **WHEN** they supply one Household Member with a name and save
- **THEN** the platform creates the Household with that single Household Member
- **AND** the Household is available for selection and search

#### Scenario: A Household Member is required

- **GIVEN** a Platform User is creating a Household
- **WHEN** they save with no named Household Member
- **THEN** the platform rejects the save, states that at least one named Household Member is required, and creates no Household

#### Scenario: Mailing address is optional

- **GIVEN** a Household with named members and no mailing address
- **WHEN** the Platform User saves it
- **THEN** the platform accepts the save and does not require a mailing address at any later point in this slice

### Requirement: Household display name

Every Household SHALL have a display name. The platform SHALL suggest the display name from its
members' names and SHALL allow a Platform User to edit it. An edited display name SHALL be
retained and SHALL NOT be overwritten by later member changes.

#### Scenario: Display name is suggested from members

- **GIVEN** a Platform User is creating a Household with named members
- **WHEN** the Household is saved without an explicitly entered display name
- **THEN** the platform stores a display name suggested from those members' names

#### Scenario: Edited display name survives a member change

- **GIVEN** a Household whose display name was explicitly edited by a Platform User
- **WHEN** a member's name is later changed
- **THEN** the platform retains the edited display name
- **AND** Audit History records the member name change with its previous and new values

### Requirement: Client Number uniqueness

A Client Number SHALL belong to an individual Household Member, not to the Household. A Household
Member SHALL have at most one Client Number. Each Client Number SHALL be unique across the
platform. The platform SHALL reject any save that would assign a Client Number already held by
another Household Member.

#### Scenario: Duplicate Client Number is blocked

- **GIVEN** an existing Household Member already holds Client Number `SYN-1001`
- **WHEN** a Platform User saves another Household Member with Client Number `SYN-1001`
- **THEN** the platform rejects the save, identifies the conflicting existing Household Member, and creates or changes no record
- **AND** the rejected save does not appear in Audit History

#### Scenario: A member cannot hold two Client Numbers

- **GIVEN** a Household Member already holds a Client Number
- **WHEN** a Platform User attempts to assign a second Client Number to that same member
- **THEN** the platform rejects the change and states that a member has at most one Client Number

#### Scenario: Client Number conflict check includes inactive records

- **GIVEN** an inactive Household contains a member holding Client Number `SYN-1002`
- **WHEN** a Platform User saves a new Household Member with Client Number `SYN-1002`
- **THEN** the platform rejects the save and directs the Platform User to the existing record

### Requirement: Duplicate blocking on exact phone or email

The platform SHALL block creation of a second Household when an exact phone number or exact email
address matches contact information already recorded on a different existing Household. The block
SHALL have no override. The platform SHALL direct the Platform User to the existing Household and
its Referral. Duplicate checks SHALL include inactive Households.

Matching SHALL use these normalizations, so that two values match only when their normalized forms
are identical:

- **Phone**: remove every character that is not a digit; then, when the result is 11 digits
  beginning with `1`, remove that leading `1`; compare the resulting digit strings for exact
  equality. Values whose normalized forms differ in length do not match.
- **Email**: trim surrounding whitespace and compare case-insensitively.

#### Scenario: Exact phone match blocks creation across formatting

- **GIVEN** an existing Household records the phone number `+1-555-555-0100`, which normalizes to `5555550100`
- **WHEN** a Platform User attempts to create a different Household using `(555) 555-0100`, which normalizes to the same `5555550100`
- **THEN** the platform blocks creation, directs the Platform User to the existing Household and its Referral, and offers no override
- **AND** no new Household, member, Referral Source, or Referral is created

#### Scenario: Different normalized phone numbers do not match

- **GIVEN** an existing Household records the phone number `+1-555-555-0100`, which normalizes to `5555550100`
- **WHEN** a Platform User creates a different Household using `555-0100`, which normalizes to `5550100`
- **THEN** the platform does not treat the two as a match and does not block creation

#### Scenario: Exact email match blocks creation

- **GIVEN** an existing Household records the email address `synthetic.prospect@example.invalid`
- **WHEN** a Platform User attempts to create a different Household using `Synthetic.Prospect@Example.Invalid`
- **THEN** the platform blocks creation and directs the Platform User to the existing Household

#### Scenario: The Platform User corrects the conflict instead of overriding

- **GIVEN** creation was blocked by an exact phone match
- **WHEN** the Platform User corrects the contact information to a non-conflicting value and saves again
- **THEN** the platform accepts the save and creates the Household

### Requirement: Shared contact information inside one Household is not a duplicate

Household Members of the same Household MAY intentionally share a phone number or email address.
The platform SHALL NOT treat contact information shared within a single Household as a duplicate
conflict.

#### Scenario: Two members of one Household share an email address

- **GIVEN** a Platform User is creating one Household with two named members
- **WHEN** both members are given the email address `synthetic.household@example.invalid`
- **THEN** the platform accepts the save without a duplicate block or duplicate warning

### Requirement: Exact name match requires explicit acceptance

An exact Household Member name match against an existing Household SHALL produce a possible
duplicate warning rather than a block. The platform SHALL require the Platform User to explicitly
accept that warning before creating a separate Household. The platform SHALL retain which Platform
User accepted the warning and when. No written reason SHALL be required.

#### Scenario: Name match warns and is accepted

- **GIVEN** an existing Household contains a member named `Jordan Sample`
- **WHEN** a Platform User creates a different Household with a member named `Jordan Sample` and explicitly accepts the possible-duplicate warning
- **THEN** the platform creates the separate Household
- **AND** Audit History records the accepted duplicate warning with the accepting Platform User and the acceptance timestamp

#### Scenario: Name match not accepted

- **GIVEN** the possible-duplicate warning is shown
- **WHEN** the Platform User does not explicitly accept it
- **THEN** the platform does not create the Household, retains the entered values for correction, and records nothing in Audit History

### Requirement: Inactive Household reuse and reactivation

Duplicate checks SHALL include inactive Households. When intake matches an inactive Household, the
platform SHALL reuse and reactivate that Household rather than creating a new one, preserving its
members and history. A `Discarded Draft` Referral SHALL remain inactive and SHALL NOT be
reactivated by this rule.

#### Scenario: Matching inactive Household is reused

- **GIVEN** a Household became inactive when its only Draft Referral was discarded
- **WHEN** a later legitimate intake matches that Household by exact phone number
- **THEN** the platform reuses and reactivates the existing Household with its members and history intact
- **AND** the earlier `Discarded Draft` Referral remains inactive
- **AND** Audit History records the reactivation with actor and timestamp

### Requirement: Pre-existing Client Household setup

The platform SHALL provide a standalone action to manually establish a Pre-existing Client
Household so that it can be selected as a Client Referral Source. Setup SHALL require a display
name and at least one Household Member. Every member identified as a client SHALL require a name
and a platform-wide unique Client Number. The Platform User SHALL record the original Client Start
Date when known or explicitly mark it `Unknown`. Phone, email, mailing address, and notes SHALL be
optional. This path SHALL create neither a Referral nor any client-appreciation task.

#### Scenario: Pre-existing Client Household is created

- **GIVEN** a Platform User opens Pre-existing Client Household setup
- **WHEN** they supply a display name, one member with a name and a unique Client Number, and mark the Client Start Date `Unknown`, then save
- **THEN** the platform creates the client Household
- **AND** creates no Referral and no client-appreciation task
- **AND** the Household is selectable as a Client Referral Source

#### Scenario: Client member without a Client Number is rejected

- **GIVEN** a Platform User is establishing a Pre-existing Client Household
- **WHEN** a member identified as a client is saved without a Client Number
- **THEN** the platform rejects the save, states that a client member requires a Client Number, and creates no record

#### Scenario: Client Start Date must be known or explicitly unknown

- **GIVEN** a Platform User is establishing a Pre-existing Client Household
- **WHEN** they neither record an original Client Start Date nor mark it `Unknown`
- **THEN** the platform rejects the save and states that one of the two is required

### Requirement: Household relationship state and discovery

The Households page SHALL serve as the client list as well as the place to find prospective
Households. The list SHALL show Household display name, member names, relationship state, Client
Numbers, primary contact information, and the next Open Task. Search SHALL default to active
records only and SHALL allow the Platform User to include inactive records, with every result
clearly showing its status.

#### Scenario: Inactive Households are excluded by default

- **GIVEN** an inactive Household and an active Household both match a search term
- **WHEN** a Platform User searches without enabling inactive records
- **THEN** only the active Household appears

#### Scenario: Inactive Households are shown on request

- **GIVEN** the same search term
- **WHEN** the Platform User enables inactive records
- **THEN** both Households appear and each result clearly shows its status

### Requirement: Household stale-save conflict rejection

The platform SHALL reject a save based on an outdated Household version rather than overwriting a
newer save. The platform SHALL tell the Platform User to reload the current record and reapply the
intended change. Only successful saves SHALL appear in Audit History.

#### Scenario: Outdated Household save is rejected

- **GIVEN** two Platform Users opened the same Household and one of them has already saved a change
- **WHEN** the other Platform User saves based on the version they loaded before that change
- **THEN** the platform rejects the save without overwriting the newer values
- **AND** tells the Platform User to reload and reapply the intended change
- **AND** records nothing in Audit History for the rejected save

#### Scenario: Reapplied change succeeds

- **GIVEN** a save was rejected as stale
- **WHEN** the Platform User reloads the current Household and saves the intended change again
- **THEN** the platform accepts the save and records it in Audit History with previous value, new value, actor, and timestamp

### Requirement: Equal access and no permanent deletion of Households

Every authenticated Platform User SHALL have equal access to view and change Household records in
this slice. The platform SHALL NOT provide permanent deletion of a Household or Household Member;
records are corrected or made inactive while Audit History remains available.

#### Scenario: Any Platform User may edit a Household

- **GIVEN** a Household owned by work assigned to another Associate
- **WHEN** any authenticated Platform User edits its contact information
- **THEN** the platform accepts the change and records the actor in Audit History

#### Scenario: No delete action is offered

- **GIVEN** a Household detail view
- **WHEN** a Platform User looks for a permanent delete action
- **THEN** no such action exists

### Requirement: A Household Member belongs to one active Household

The platform SHALL NOT allow the same Household Member record to belong to more than one active
Household at a time.

#### Scenario: Member cannot be added to a second active Household

- **GIVEN** a Household Member belongs to an active Household
- **WHEN** a Platform User attempts to add that same member record to another active Household
- **THEN** the platform rejects the change and states that a member belongs to one active Household at a time
