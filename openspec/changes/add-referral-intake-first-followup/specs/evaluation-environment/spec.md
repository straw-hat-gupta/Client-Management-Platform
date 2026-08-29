## Purpose

This slice is an evaluation-only build. This capability owns the boundary that keeps it safe to
operate: synthetic data only, isolated and non-public operation, no external integration, no
production deployment, and a reset that restores a known synthetic baseline.

## ADDED Requirements

### Requirement: Synthetic data only

The evaluation build SHALL be used only with clearly synthetic data. Real firm, client, or
financial data SHALL NOT be entered into it, and SHALL NOT appear in fixtures, seed data, logs,
screenshots, or tests. No value, free-text fragment, or file originating in `agent_analysis_package/`
SHALL appear in fixtures, seed data, tests, or application code; only structural patterns — column
shapes, value formats, and cardinality — MAY be reused. The restriction is at the level of
individual values, not whole rows, because the sanitized files preserve original free-text wording.
This restriction SHALL be documented in the repository rather than presented as an in-product
warning.

#### Scenario: Seed data is clearly synthetic

- **GIVEN** a freshly reset evaluation environment
- **WHEN** an evaluator views the seeded Households, Associates, Events, and COIs
- **THEN** every record is clearly marked as synthetic

#### Scenario: No in-product data warning is shown

- **GIVEN** the evaluation build
- **WHEN** an evaluator uses any page
- **THEN** no in-product warning about real data is displayed, and the restriction is documented in the repository instead

#### Scenario: No sanitized value or fragment is present

- **GIVEN** the repository's fixtures, seed data, tests, and application code
- **WHEN** they are checked against the contents of `agent_analysis_package/`
- **THEN** they contain no copied row, no copied field value, and no copied free-text fragment

#### Scenario: The sanitized files are never read by the build

- **GIVEN** the application and test code
- **WHEN** its file reads and imports are inspected
- **THEN** none of them resolves to a path under `agent_analysis_package/`

### Requirement: Isolated, non-public operation

The evaluation build SHALL run only locally or in an isolated private evaluation environment. It
SHALL NOT be exposed publicly or to an untrusted network. It has no real authentication, so it
SHALL NOT be treated as access-controlled.

The platform SHALL enforce this rather than rely on operator discipline: the API SHALL bind a
loopback interface by default, SHALL refuse to start when configured to bind a non-loopback
interface unless an explicitly named acknowledgement value is set, and SHALL log the interface it
bound at startup.

#### Scenario: The build is not publicly reachable

- **GIVEN** a deployed evaluation environment
- **WHEN** its reachability is checked from outside the isolated network
- **THEN** it is not reachable

#### Scenario: Default bind is loopback

- **GIVEN** an evaluation environment with no interface explicitly configured
- **WHEN** the API starts
- **THEN** it binds a loopback interface
- **AND** it logs the bound interface

#### Scenario: Non-loopback bind is refused without acknowledgement

- **GIVEN** the API configured to bind a non-loopback interface
- **WHEN** it starts without the explicitly named acknowledgement value
- **THEN** it refuses to start and names the isolation requirement as the reason

#### Scenario: Acknowledged non-loopback bind starts and is logged

- **GIVEN** the API configured to bind a non-loopback interface with the acknowledgement value set
- **WHEN** it starts
- **THEN** it starts and logs the non-loopback interface it bound

### Requirement: No upload, import, or external integration

The evaluation build SHALL provide no file upload, no spreadsheet import, no email, no messaging,
and no other external communication or third-party integration. No import capability SHALL be
enabled in any environment holding real data without the provenance behavior described in
`docs/product/journeys.md` "Deferred Migration Notes".

#### Scenario: No upload or import action exists

- **GIVEN** any page in the evaluation build
- **WHEN** an evaluator looks for a file upload or spreadsheet import action
- **THEN** no such action exists

#### Scenario: No outbound communication occurs

- **GIVEN** a task reminder becoming due
- **WHEN** the reminder indicator appears
- **THEN** the platform sends no email, message, or push notification

### Requirement: Production deployment is disabled

Production deployment of this slice SHALL remain disabled. It SHALL remain disabled until real
authentication and security logging are added, a completed security threat model exists, and
professionally approved privacy, compliance, retention, and security governance are in place.

The refusal SHALL NOT depend on the repository's deployment script alone, because an ordinary
build pointed at a different `DATABASE_URL` would bypass it. Three independent layers SHALL apply:

1. The deployment path refuses to run and names the evaluation-only boundary.
2. The API refuses to start unless an explicit evaluation-mode marker is present in its
   configuration.
3. Both API startup and the migration runner refuse a database that does not carry an
   evaluation-environment marker row, which the reset operation seeds.

Lifting any of these refusals SHALL require a code change; no configuration value alone SHALL
enable production operation.

#### Scenario: The production deployment path refuses to run

- **GIVEN** the repository's deployment configuration
- **WHEN** a production deployment is attempted
- **THEN** it is refused and identifies the evaluation-only boundary as the reason

#### Scenario: Startup without the evaluation-mode marker is refused

- **GIVEN** the API configured without the explicit evaluation-mode marker
- **WHEN** it starts
- **THEN** it refuses to start and names the evaluation-only boundary as the reason

#### Scenario: An unmarked database is refused

- **GIVEN** a database that carries no evaluation-environment marker row
- **WHEN** the API starts or the migration runner runs against it
- **THEN** both refuse and name the missing marker as the reason

#### Scenario: Configuration alone cannot enable production operation

- **GIVEN** any combination of configuration and environment values
- **WHEN** an operator attempts to lift the refusals without changing code
- **THEN** the refusals remain in force

### Requirement: Environment reset to a known synthetic baseline

The evaluation environment SHALL be resettable to a known synthetic baseline. Reset SHALL be an
environment operation performed outside normal Platform User actions, SHALL NOT be exposed as a
Platform User feature in the application, and SHALL NOT be recorded in Audit History.

Because reset is destructive and takes its target from configuration, it SHALL refuse to run unless
the target database carries the evaluation-environment marker row and evaluation-mode configuration
is present. Reset SHALL leave an out-of-band operator record of when it ran and against which
target; before any operational pilot, no comparably destructive environment operation may exist
without a restricted security-log entry.

#### Scenario: Reset restores the baseline

- **GIVEN** an evaluation environment containing work created during evaluation
- **WHEN** the environment reset operation is run
- **THEN** the environment contains exactly the known synthetic baseline records
- **AND** an out-of-band operator record identifies when it ran and against which target

#### Scenario: Reset refuses an unmarked target

- **GIVEN** a target database that carries no evaluation-environment marker row
- **WHEN** the reset operation is run against it
- **THEN** it refuses and destroys nothing

#### Scenario: Reset refuses without evaluation-mode configuration

- **GIVEN** a reset invoked with evaluation-mode configuration absent
- **WHEN** it runs
- **THEN** it refuses and destroys nothing

#### Scenario: Reset is not a Platform User feature

- **GIVEN** any page in the evaluation build
- **WHEN** a Platform User looks for an environment reset action
- **THEN** no such action exists in the application

#### Scenario: Reset is not audited

- **GIVEN** a completed environment reset
- **WHEN** a Platform User opens any record's Audit History
- **THEN** the reset does not appear as an Audit History entry

### Requirement: No operational data promise

Evaluation data SHALL carry no backup, retention, or migration promise. Operational backup and
recovery SHALL be established before any real client use. Evaluation data SHALL NOT be promoted,
restored, or migrated into any environment used with real data; such an environment starts empty.

#### Scenario: Evaluation data is disposable

- **GIVEN** an evaluation environment
- **WHEN** it is reset or destroyed
- **THEN** no backup, retention, or migration obligation applies to the data it held

### Requirement: Technical logs carry no business content

Technical request and error logs SHALL carry only record identifiers, error classes, and a
correlation identifier. They SHALL NOT carry contact details, note or reason text, Client Numbers,
or record field values. This applies to the rejected saves, validation failures, and stale-save
conflicts that are deliberately excluded from Audit History and therefore appear only in technical
logs, which are the submissions most likely to contain contact details.

#### Scenario: A rejected save is logged without its content

- **GIVEN** a save rejected by validation whose payload contained a phone number, an email address, a Client Number, and free-text note and reason values
- **WHEN** the rejection is logged
- **THEN** the log entry identifies the record, the error class, and the correlation identifier
- **AND** none of those submitted values appears anywhere in the log output

### Requirement: Limited page surface

The evaluation build SHALL expose only the Referrals page, Referral detail, the Tasks page, the
Households page and Household detail, minimal Events, minimal COIs, synthetic Associates, and
contextual `Audit history` actions. Reports, client appreciation, full Event and COI management,
import, and a global Audit History page SHALL NOT be present.

#### Scenario: Out-of-scope pages are absent

- **GIVEN** the evaluation build's navigation
- **WHEN** an evaluator looks for Reports, appreciation, import, or a global Audit History page
- **THEN** none of them exist
