## Purpose

This slice is an evaluation-only build. This capability owns the boundary that keeps it safe to
operate: synthetic data only, isolated and non-public operation, no external integration, no
production deployment, and a reset that restores a known synthetic baseline.

## ADDED Requirements

### Requirement: Synthetic data only

The evaluation build SHALL be used only with clearly synthetic data. Real firm, client, or
financial data SHALL NOT be entered into it, and SHALL NOT appear in fixtures, seed data, logs,
screenshots, or tests. Synthetic seed data MAY follow the sanitized spreadsheets' structures and
value patterns, but sanitized spreadsheet rows SHALL NOT be copied into fixtures. This restriction
SHALL be documented in the repository rather than presented as an in-product warning.

#### Scenario: Seed data is clearly synthetic

- **GIVEN** a freshly reset evaluation environment
- **WHEN** an evaluator views the seeded Households, Associates, Events, and COIs
- **THEN** every record is clearly marked as synthetic

#### Scenario: No in-product data warning is shown

- **GIVEN** the evaluation build
- **WHEN** an evaluator uses any page
- **THEN** no in-product warning about real data is displayed, and the restriction is documented in the repository instead

#### Scenario: Sanitized spreadsheet rows are not present

- **GIVEN** the repository's fixtures and seed data
- **WHEN** they are inspected
- **THEN** they contain no rows copied from the sanitized spreadsheets

### Requirement: Isolated, non-public operation

The evaluation build SHALL run only locally or in an isolated private evaluation environment. It
SHALL NOT be exposed publicly or to an untrusted network. It has no real authentication, so it
SHALL NOT be treated as access-controlled.

#### Scenario: The build is not publicly reachable

- **GIVEN** a deployed evaluation environment
- **WHEN** its reachability is checked from outside the isolated network
- **THEN** it is not reachable

### Requirement: No upload, import, or external integration

The evaluation build SHALL provide no file upload, no spreadsheet import, no email, no messaging,
and no other external communication or third-party integration.

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
authentication and security logging are added, together with professionally approved privacy,
compliance, retention, and security governance.

#### Scenario: The production deployment path refuses to run

- **GIVEN** the repository's deployment configuration
- **WHEN** a production deployment is attempted
- **THEN** it is refused and identifies the evaluation-only boundary as the reason

### Requirement: Environment reset to a known synthetic baseline

The evaluation environment SHALL be resettable to a known synthetic baseline. Reset SHALL be an
environment operation performed outside normal Platform User actions, SHALL NOT be exposed as a
Platform User feature in the application, and SHALL NOT be recorded in Audit History.

#### Scenario: Reset restores the baseline

- **GIVEN** an evaluation environment containing work created during evaluation
- **WHEN** the environment reset operation is run
- **THEN** the environment contains exactly the known synthetic baseline records

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
recovery SHALL be established before any real client use.

#### Scenario: Evaluation data is disposable

- **GIVEN** an evaluation environment
- **WHEN** it is reset or destroyed
- **THEN** no backup, retention, or migration obligation applies to the data it held

### Requirement: Limited page surface

The evaluation build SHALL expose only the Referrals page, Referral detail, the Tasks page, the
Households page and Household detail, minimal Events, minimal COIs, synthetic Associates, and
contextual `Audit history` actions. Reports, client appreciation, full Event and COI management,
import, and a global Audit History page SHALL NOT be present.

#### Scenario: Out-of-scope pages are absent

- **GIVEN** the evaluation build's navigation
- **WHEN** an evaluator looks for Reports, appreciation, import, or a global Audit History page
- **THEN** none of them exist
