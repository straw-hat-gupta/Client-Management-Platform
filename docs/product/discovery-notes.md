# Product Discovery Notes

## Document Status

- Status: Draft
- Source: Initial stakeholder interview
- Transcript status: Corrected and sanitized
- Source-reference method: Repository transcript physical line numbers
- Last extracted: 2026-08-16
- Human review status: Not reviewed
- Approved by: TBD

## Source Register

| Source ID | Source | Repository path | SHA-256 | Lines | Status |
|---|---|---|---|---:|---|
| I-001 | Initial stakeholder interview | `Client-platform-Interview.txt` | 52229924b9f4f41e889eebb4410e840b93263f02fbb97cae0c10b548ffeaaeae | 577 | Corrected and sanitized |

Line references are valid only for the transcript version identified by the
SHA-256 hash above. If the transcript changes, regenerate and review this
evidence ledger before relying on its references.

## Classification Guide

- Time orientation distinguishes current business practice from desired future practice; `Both`, `Not applicable`, and `Unclear` cover mixed, timeless, or ambiguous evidence.
- Evidence type distinguishes direct statements and explicit decisions from preferences, suggestions, examples, inferences, and contradictory evidence.
- Decision status records whether an authorized stakeholder confirmed a decision, favoured it tentatively, left it TBD, requires professional review, or made no decision.
- Follow-up priority is `Blocker` before an affected first OpenSpec proposal, `Important` before implementation, `Later` for later work, or `None` when no follow-up is needed.
- Source references use physical transcript lines, for example `I-001, L0120-L0138` or `I-001, L0087`.

## Evidence Index

| ID | Finding | Category | Time orientation | Decision status | Priority | Source lines |
|---|---|---|---|---|---|---|
| [E-001](#e-001-disconnected-spreadsheets-create-duplicate-work) | Disconnected spreadsheets create duplicate work | Problem | Current state | Not a decision | Important | I-001, L0006-L0019 |
| [E-002](#e-002-information-and-workflow-status-are-hard-to-find) | Information and workflow status are hard to find | Pain point | Current state | Not a decision | Important | I-001, L0025-L0038 |
| [E-003](#e-003-efficiency-and-reliable-record-production-drive-the-effort) | Efficiency and reliable record production drive the effort | Outcome | Desired future state | Not a decision | Important | I-001, L0014-L0015; L0043-L0051 |
| [E-004](#e-004-all-team-members-are-expected-users) | All team members are expected users | User | Desired future state | Tentative | Important | I-001, L0055-L0064 |
| [E-005](#e-005-the-platform-is-expected-to-summarize-and-recommend-work) | The platform is expected to summarize and recommend work | Desired workflow | Desired future state | Tentative | Important | I-001, L0065-L0074 |
| [E-006](#e-006-automatic-client-email-is-not-wanted-for-now) | Automatic client email is not wanted for now | Non-goal | Desired future state | Tentative | Later | I-001, L0075-L0082 |
| [E-007](#e-007-client-value-is-associated-with-faster-more-accurate-follow-up) | Client value is associated with faster, more accurate follow-up | Outcome | Desired future state | Not a decision | Important | I-001, L0083-L0094; L0143-L0150 |
| [E-008](#e-008-first-release-workflow-priorities-are-internally-inconsistent) | First-release workflow priorities are internally inconsistent | Scope | Desired future state | TBD | Blocker | I-001, L0098-L0129 |
| [E-009](#e-009-search-and-timely-reporting-are-important) | Search and timely reporting are important | Reporting requirement | Desired future state | Tentative | Important | I-001, L0126-L0134 |
| [E-010](#e-010-nccp-and-mm-are-not-expected-to-be-replaced-or-integrated) | NCCP and M&M are not expected to be replaced or integrated | Non-goal | Desired future state | Tentative | Later | I-001, L0137-L0142 |
| [E-011](#e-011-success-and-failure-are-not-yet-measurable) | Success and failure are not yet measurable | Success measure | Desired future state | TBD | Important | I-001, L0049-L0054; L0143-L0153 |
| [E-012](#e-012-an-introduction-triggers-entry-in-rms) | An introduction triggers entry in RMS | Current workflow | Current state | Not a decision | None | I-001, L0159-L0167; L0184-L0201 |
| [E-013](#e-013-work-progresses-from-rms-to-pillar-and-specialized-tracking) | Work progresses from RMS to Pillar and specialized tracking | Current workflow | Current state | Not a decision | Important | I-001, L0163-L0173 |
| [E-014](#e-014-spreadsheets-have-different-team-owners-and-a-report-consumer) | Spreadsheets have different team owners and a report consumer | Handoff | Current state | Not a decision | Important | I-001, L0176-L0189 |
| [E-015](#e-015-referral-intake-usually-needs-name-phone-and-email) | Referral intake usually needs name, phone, and email | Data definition | Current state | Not a decision | Important | I-001, L0190-L0198; L0279-L0284 |
| [E-016](#e-016-calls-meetings-and-signing-dates-are-recorded-but-emails-are-not) | Calls, meetings, and signing dates are recorded, but emails are not | Current workflow | Current state | Not a decision | Important | I-001, L0204-L0221 |
| [E-017](#e-017-weekly-review-and-calendar-copying-drive-next-actions) | Weekly review and calendar copying drive next actions | Current workflow | Current state | Not a decision | Important | I-001, L0206-L0208; L0224-L0233 |
| [E-018](#e-018-reminders-are-preferred-in-email-and-in-the-platform) | Reminders are preferred in email and in the platform | Desired workflow | Desired future state | Tentative | Important | I-001, L0234-L0239 |
| [E-019](#e-019-current-workflow-terminology-is-limited) | Current workflow terminology is limited | Status or lifecycle | Current state | Not a decision | Important | I-001, L0240-L0268 |
| [E-020](#e-020-missing-details-are-uncommon-and-repeat-pillar-entries-can-be-valid) | Missing details are uncommon, and repeat Pillar entries can be valid | Exception | Current state | Not a decision | Important | I-001, L0273-L0288 |
| [E-021](#e-021-client-decisions-and-transfers-can-change-or-fail) | Client decisions and transfers can change or fail | Exception | Current state | Not a decision | Important | I-001, L0292-L0302 |
| [E-022](#e-022-transfer-tracking-distinguishes-initiated-received-and-invested) | Transfer tracking distinguishes initiated, received, and invested | Status or lifecycle | Current state | Not a decision | Important | I-001, L0296-L0302; L0324-L0329 |
| [E-023](#e-023-tasks-have-assignees-and-can-be-reassigned) | Tasks have assignees and can be reassigned | Permission | Current state | Not a decision | Important | I-001, L0304-L0316 |
| [E-024](#e-024-amounts-can-conflict-between-sheets) | Amounts can conflict between sheets | Risk | Current state | Not a decision | Important | I-001, L0317-L0323 |
| [E-025](#e-025-pillar-tracks-evaluated-potential-business-and-follow-up-activity) | Pillar tracks evaluated potential business and follow-up activity | Data definition | Current state | Not a decision | None | I-001, L0320-L0341 |
| [E-026](#e-026-investment-transfer-begins-after-agreement-and-signed-documents) | Investment transfer begins after agreement and signed documents | Status or lifecycle | Current state | Not a decision | Important | I-001, L0324-L0341 |
| [E-027](#e-027-transfer-reconciliation-is-client-specific-and-has-additional-status-detail) | Transfer reconciliation is client-specific and has additional status detail | Reporting requirement | Current state | Not a decision | Important | I-001, L0328-L0353 |
| [E-028](#e-028-an-insurance-business-monitor-is-desired-but-not-prioritized) | An insurance business monitor is desired but not prioritized | Desired workflow | Desired future state | Tentative | Important | I-001, L0354-L0364 |
| [E-029](#e-029-manual-errors-and-missed-updates-can-cause-lost-follow-up-and-business) | Manual errors and missed updates can cause lost follow-up and business | Failure and recovery | Current state | Not a decision | Important | I-001, L0365-L0380 |
| [E-030](#e-030-change-history-is-an-unresolved-interviewer-proposal) | Change history is an unresolved interviewer proposal | Audit requirement | Desired future state | TBD | Important | I-001, L0381-L0385 |
| [E-031](#e-031-stopped-work-should-be-preserved-and-closed-work-can-reopen) | Stopped work should be preserved, and closed work can reopen | Business rule | Desired future state | Confirmed | Important | I-001, L0386-L0394 |
| [E-032](#e-032-concurrent-updates-are-resolved-manually) | Concurrent updates are resolved manually | Exception | Current state | Not a decision | Important | I-001, L0395-L0401 |
| [E-033](#e-033-every-task-should-remind-and-overdue-work-has-no-grace-period) | Every task should remind, and overdue work has no grace period | Business rule | Desired future state | Confirmed | Important | I-001, L0402-L0420 |
| [E-034](#e-034-client-and-prospective-client-have-distinct-current-definitions) | Client and prospective client have distinct current definitions | Data definition | Current state | Not a decision | Important | I-001, L0424-L0436 |
| [E-035](#e-035-record-grouping-varies-by-workflow-and-family-composition) | Record grouping varies by workflow and family composition | Business rule | Current state | Not a decision | Blocker | I-001, L0437-L0476 |
| [E-036](#e-036-focus-10-is-a-report-of-near-term-potential-business) | Focus 10 is a report of near-term potential business | Reporting requirement | Both | Tentative | Important | I-001, L0482-L0490 |
| [E-037](#e-037-opportunity-means-estimated-investable-assets) | Opportunity means estimated investable assets | Data definition | Current state | Not a decision | Important | I-001, L0499-L0500 |
| [E-038](#e-038-records-are-linked-by-name-and-sometimes-client-number) | Records are linked by name and sometimes client number | Data source | Current state | Not a decision | Blocker | I-001, L0501-L0512 |
| [E-039](#e-039-pillar-values-and-activity-dates-change-frequently) | Pillar values and activity dates change frequently | Status or lifecycle | Current state | Not a decision | Important | I-001, L0525-L0527 |
| [E-040](#e-040-authoritative-data-is-only-partly-identified) | Authoritative data is only partly identified | Data source | Current state | Not a decision | Blocker | I-001, L0528-L0537 |
| [E-041](#e-041-rms-continues-after-client-conversion-for-appreciation-activities) | RMS continues after client conversion for appreciation activities | Current workflow | Current state | Not a decision | Important | I-001, L0538-L0550 |
| [E-042](#e-042-spreadsheets-are-to-be-replaced-but-import-scope-is-not-confirmed) | Spreadsheets are to be replaced, but import scope is not confirmed | Scope | Desired future state | Confirmed | Blocker | I-001, L0551-L0554 |
| [E-043](#e-043-all-users-are-preferred-to-have-equal-access-for-now) | All users are preferred to have equal access for now | Permission | Desired future state | Tentative | Blocker | I-001, L0555-L0558 |
| [E-044](#e-044-document-storage-remains-undecided) | Document storage remains undecided | Scope | Desired future state | TBD | Later | I-001, L0559-L0561 |
| [E-045](#e-045-year-end-carry-forward-and-retention-need-professional-review) | Year-end carry-forward and retention need professional review | Professional review required | Both | Professional review required | Important | I-001, L0562-L0572 |
| [E-046](#e-046-additional-process-details-may-emerge-during-use) | Additional process details may emerge during use | Open question | Desired future state | TBD | Later | I-001, L0573-L0577 |

## Detailed Evidence

### E-001: Disconnected spreadsheets create duplicate work

- Source: I-001, L0006-L0019
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Problem
- Secondary categories: [Pain point, Current workflow]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The office maintains separate, unconnected spreadsheets, causing the same information to be entered multiple times and updates to be missed.
- Business significance: Repeated and inconsistent maintenance is the central problem motivating the platform.
- Related terms: [Spreadsheet, Redundancy]
- Related evidence: [E-002, E-003]
- Follow-up question: Which specific fields are duplicated across which spreadsheets?

### E-002: Information and workflow status are hard to find

- Source: I-001, L0025-L0038
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Pain point
- Secondary categories: [Current workflow, Reporting requirement]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Staff must open multiple spreadsheets to find details, and workflow status is similarly difficult to access.
- Business significance: Poor discoverability consumes staff time and may conceal work needing attention.
- Related terms: [Spreadsheet, Workflow status]
- Related evidence: [E-001, E-017]
- Follow-up question: Which information and status questions are most frequent?

### E-003: Efficiency and reliable record production drive the effort

- Source: I-001, L0014-L0015; L0043-L0051
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Outcome
- Secondary categories: [Problem, Success measure]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder seeks a smoother, more efficient daily routine and considers failure to produce wanted records a project failure.
- Business significance: These outcomes frame value but do not yet define measurable acceptance criteria.
- Related terms: [Efficiency, Records]
- Related evidence: [E-011]
- Follow-up question: Which records must be producible for the first release to be considered useful?

### E-004: All team members are expected users

- Source: I-001, L0055-L0064
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: User
- Secondary categories: [Scope, Outcome]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Tentative
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder expects all team members, including reception, to use the platform; clients are affected through smoother follow-up but are not identified as direct users.
- Business significance: The breadth of internal users affects workflow discovery and access decisions.
- Related terms: [Team member, Client]
- Related evidence: [E-043]
- Follow-up question: What business responsibilities does each user group perform in each workflow?

### E-005: The platform is expected to summarize and recommend work

- Source: I-001, L0065-L0074
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Desired workflow
- Secondary categories: [Reporting requirement, Handoff]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Tentative
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder agreed that the platform should calculate or summarize information, create tasks, and recommend actions so team work is not missed.
- Business significance: The requested assistance extends beyond presenting stored information, but its boundaries are undefined.
- Related terms: [Task, Recommendation, Summary]
- Related evidence: [E-018, E-033]
- Follow-up question: What calculations, summaries, and recommendations are needed first, and who approves their definitions?

### E-006: Automatic client email is not wanted for now

- Source: I-001, L0075-L0082
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Non-goal
- Secondary categories: [Scope, Desired workflow]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Tentative
- Follow-up priority: Later
- Decision owner: TBD
- Finding: When automatic action was illustrated as emailing clients, the stakeholder rejected it for now while leaving open a much later possibility.
- Business significance: Current discovery should not treat automatic client communication as accepted scope.
- Related terms: [Client email, Automation]
- Related evidence: [E-018]
- Follow-up question: None.

### E-007: Client value is associated with faster, more accurate follow-up

- Source: I-001, L0083-L0094; L0143-L0150
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Outcome
- Secondary categories: [Success measure, Pain point]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Desired client value includes faster and more accurate follow-up, smoother onboarding and business booking, and less duplicate internal entry.
- Business significance: Client value is linked indirectly to office workflow improvement rather than repeated client information requests.
- Related terms: [Follow-up, Onboarding, Accuracy]
- Related evidence: [E-011, E-029]
- Follow-up question: How will faster, more accurate follow-up be measured without using client-identifying data?

### E-008: First-release workflow priorities are internally inconsistent

- Source: I-001, L0098-L0129
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Scope
- Secondary categories: [Contradiction, Open question]
- Time orientation: Desired future state
- Evidence type: Contradictory evidence
- Decision status: TBD
- Follow-up priority: Blocker
- Decision owner: TBD
- Finding: Referral is consistently important, but transfers and audit history appear both among immediate priorities and among items that may be excluded; reporting and search are also described as first-release priorities.
- Business significance: An affected first proposal cannot rely on a stable release boundary from this discussion.
- Related terms: [Referral, Transfer, Audit history, Reporting]
- Related evidence: [E-009, E-030, E-042]
- Follow-up question: What is the ordered, approved first-release scope and what is explicitly deferred?

### E-009: Search and timely reporting are important

- Source: I-001, L0126-L0134
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Reporting requirement
- Secondary categories: [Desired workflow, Scope]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Tentative
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder described search as important for creating the right reports in a timely fashion, while distinguishing these reports from Salesforce reports.
- Business significance: Reporting needs discovery separate from existing Salesforce reporting.
- Related terms: [Search, Report, Salesforce]
- Related evidence: [E-027, E-036]
- Follow-up question: Which named reports, filters, recipients, and timing are needed first?

### E-010: NCCP and M&M are not expected to be replaced or integrated

- Source: I-001, L0137-L0142
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Non-goal
- Secondary categories: [Scope, Constraint]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: Tentative
- Follow-up priority: Later
- Decision owner: TBD
- Finding: The stakeholder said NCCP and M&M do not participate in the spreadsheet workflow and are not really expected to be replaced or integrated.
- Business significance: These systems appear outside the discussed platform boundary, subject to confirmation.
- Related terms: [NCCP, M&M]
- Related evidence: [E-042]
- Follow-up question: Confirm whether both systems are explicit non-goals for the first release.

### E-011: Success and failure are not yet measurable

- Source: I-001, L0049-L0054; L0143-L0153
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Success measure
- Secondary categories: [Outcome, Risk]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: TBD
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Success is described as more client value and failure as inability to produce wanted records or staff finding the platform too time-consuming, but no baseline or target is given.
- Business significance: The project lacks measurable evidence for evaluating adoption and outcome improvement.
- Related terms: [Client value, Adoption, Records]
- Related evidence: [E-003, E-007]
- Follow-up question: What observable measures, baselines, targets, and review period will define success?

### E-012: An introduction triggers entry in RMS

- Source: I-001, L0159-L0167; L0184-L0201
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Current workflow
- Secondary categories: [Handoff, Data definition]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: None
- Decision owner: TBD
- Finding: A referral or introduction starts the process, and the first spreadsheet action is entering the person in RMS before booking a first meeting.
- Business significance: This establishes the described current workflow trigger and first recorded action.
- Related terms: [Introduction, Referral, RMS]
- Related evidence: [E-013, E-015]
- Follow-up question: None.

### E-013: Work progresses from RMS to Pillar and specialized tracking

- Source: I-001, L0163-L0173
- Speaker: SPEAKER_00
- Primary category: Current workflow
- Secondary categories: [Status or lifecycle, Handoff]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The described flow moves an introduction through RMS and meetings into Pillar, then adds confirmed work to investment-transfer or insurance-transfer spreadsheets; Pillar also receives new business from existing clients.
- Business significance: This is the central cross-spreadsheet workflow, but its exact transitions and exceptions remain incomplete.
- Related terms: [RMS, Pillar, Investment transfer, Insurance transfer]
- Related evidence: [E-025, E-026, E-028, E-041]
- Follow-up question: What exact event, required information, and owner govern each transition?

### E-014: Spreadsheets have different team owners and a report consumer

- Source: I-001, L0176-L0189
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Handoff
- Secondary categories: [User, Current workflow]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: RMS is managed by one team member, Pillar by two team members, reports are needed by SPEAKER_00, and anyone could be trained to start the process.
- Business significance: Spreadsheet ownership, task responsibility, report consumption, and system permissions cannot be assumed to be the same.
- Related terms: [RMS, Pillar, Report]
- Related evidence: [E-004, E-023, E-043]
- Follow-up question: Which business role owns each workflow stage, handoff, and report?

### E-015: Referral intake usually needs name, phone, and email

- Source: I-001, L0190-L0198; L0279-L0284
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Data definition
- Secondary categories: [Current workflow, Exception]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Name, phone number, and email are described as intake information; a missing phone number is unlikely but possible, while later work may include assets, statements, meeting information, and application activity.
- Business significance: The current minimum and progressively collected data are not fully distinguished.
- Related terms: [Referral, Phone number, Email]
- Related evidence: [E-020, E-034]
- Follow-up question: Which fields are required at each stage, and what happens when a required field is unavailable?

### E-016: Calls, meetings, and signing dates are recorded, but emails are not

- Source: I-001, L0204-L0221
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Current workflow
- Secondary categories: [Data definition, Audit requirement]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Staff are supposed to record phone-call dates in RMS, meeting dates, and document-signing dates, while emails are not recorded in spreadsheets and recording them was rejected as too much.
- Business significance: The activity history has an explicit current boundary that should not be silently expanded.
- Related terms: [Phone call, Meeting, Document signing, Email]
- Related evidence: [E-030]
- Follow-up question: Which activity details beyond dates are currently recorded and need to remain available?

### E-017: Weekly review and calendar copying drive next actions

- Source: I-001, L0206-L0208; L0224-L0233
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Current workflow
- Secondary categories: [Pain point, Handoff]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Staff inspect spreadsheets weekly and copy instructions or next actions into calendars; Pillar is updated weekly with new, previous, and next activity.
- Business significance: Next-action awareness currently depends on manual review and duplicate transfer to another tool.
- Related terms: [Weekly review, Calendar, Next activity]
- Related evidence: [E-002, E-018, E-033]
- Follow-up question: Who performs each weekly review, and what happens when it is missed?

### E-018: Reminders are preferred in email and in the platform

- Source: I-001, L0234-L0239
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Desired workflow
- Secondary categories: [Desired workflow, Reporting requirement]
- Time orientation: Desired future state
- Evidence type: Preference
- Decision status: Tentative
- Follow-up priority: Important
- Decision owner: TBD
- Finding: When asked how attention should be surfaced, the stakeholder preferred reminders both by email and within the platform.
- Business significance: The preference identifies channels but not recipients, timing, or failure handling.
- Related terms: [Reminder, Email]
- Related evidence: [E-033]
- Follow-up question: Which reminders go to whom, when, and through which channel?

### E-019: Current workflow terminology is limited

- Source: I-001, L0240-L0268
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Status or lifecycle
- Secondary categories: [Data definition, Open question]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Current spreadsheets do not use the interviewer’s proposed general lifecycle statuses; common terms include “no further action required,” APNA, and BNA.
- Business significance: A shared lifecycle vocabulary is not yet established and interviewer examples were not accepted as existing terms.
- Related terms: [No further action required, APNA, BNA]
- Related evidence: [E-039]
- Follow-up question: Define each current term, its entry and exit conditions, and whether it is a status or an instruction.

### E-020: Missing details are uncommon, and repeat Pillar entries can be valid

- Source: I-001, L0273-L0288
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Exception
- Secondary categories: [Data definition, Business rule]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Partial referral information is possible but described as unlikely, and the same client may validly have multiple Pillar entries for different potential business.
- Business significance: Missing-data and duplicate-handling rules must distinguish errors from legitimate repeated opportunities.
- Related terms: [Missing information, Duplicate, Pillar]
- Related evidence: [E-015, E-037]
- Follow-up question: What identifies one distinct opportunity, and how are accidental duplicates corrected?

### E-021: Client decisions and transfers can change or fail

- Source: I-001, L0292-L0302
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Exception
- Secondary categories: [Status or lifecycle, Failure and recovery]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Clients change their minds, transfers are sometimes rejected, and initiated, received, and invested amounts or events are distinct.
- Business significance: The workflow is non-linear and needs clarified exception outcomes rather than assuming every opportunity completes.
- Related terms: [Transfer rejection, Initiated, Received, Invested]
- Related evidence: [E-022, E-031]
- Follow-up question: What states and next actions follow a changed decision or rejected transfer?

### E-022: Transfer tracking distinguishes initiated, received, and invested

- Source: I-001, L0296-L0302; L0324-L0329
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Status or lifecycle
- Secondary categories: [Current workflow, Data definition]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Transfer tracking distinguishes what was initiated, what was received, and what was invested, with dates recorded for these events.
- Business significance: These are separate business events rather than interchangeable descriptions of completion.
- Related terms: [Transfer, Initiated, Received, Invested]
- Related evidence: [E-026, E-027]
- Follow-up question: Define permitted transitions, required fields, reversibility, and failure handling for each transfer stage.

### E-023: Tasks have assignees and can be reassigned

- Source: I-001, L0304-L0316
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Permission
- Secondary categories: [Handoff, Current workflow]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Spreadsheets record who needs to perform a follow-up, and the same task can be moved to a different person.
- Business significance: Assignment and reassignment are current workflow concepts, but authority and history are unstated.
- Related terms: [Task, Assignee, Reassignment]
- Related evidence: [E-014, E-030, E-043]
- Follow-up question: Who may assign or reassign work, what notice is required, and must prior assignments remain visible?

### E-024: Amounts can conflict between sheets

- Source: I-001, L0317-L0323
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Risk
- Secondary categories: [Data source, Current workflow]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder first denied conflicts, then acknowledged that Pillar and actual transfer amounts may differ because they represent different concepts.
- Business significance: Apparent conflict may be legitimate evolution, but the concepts and reconciliation process must be explicit.
- Related terms: [Pillar amount, Transfer amount]
- Related evidence: [E-025, E-026, E-040]
- Follow-up question: Which amount does each sheet represent, and when is a difference an error rather than a valid change?

### E-025: Pillar tracks evaluated potential business and follow-up activity

- Source: I-001, L0320-L0341
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Data definition
- Secondary categories: [Current workflow, Status or lifecycle]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: None
- Decision owner: TBD
- Finding: Pillar records an evaluation of potential business and activities used to pursue it before the client has agreed and signed documents.
- Business significance: A Pillar opportunity is an estimate, not a completed or agreed transfer.
- Related terms: [Pillar, Opportunity, Evaluation]
- Related evidence: [E-026, E-037]
- Follow-up question: None.

### E-026: Investment transfer begins after agreement and signed documents

- Source: I-001, L0324-L0341
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Status or lifecycle
- Secondary categories: [Current workflow, Handoff]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The investment-transfer sheet is used after a client agrees and signs documents, then tracks transfer dates.
- Business significance: Agreement and signing appear to separate evaluated opportunity from initiated transfer, but the exact transition remains incomplete.
- Related terms: [Investment transfer, Agreement, Signed documents]
- Related evidence: [E-022, E-025]
- Follow-up question: Is agreement, signing, initiation, or another event the authoritative entry condition?

### E-027: Transfer reconciliation is client-specific and has additional status detail

- Source: I-001, L0328-L0353
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Reporting requirement
- Secondary categories: [Current workflow, Status or lifecycle]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Investment transfer aggregates clients, while transfer reconciliation is client-specific, used in meetings, and includes additional legends such as in process, received but not invested, invested, and received but not invested by decision.
- Business significance: The two artifacts overlap but serve different reporting purposes and levels of detail.
- Related terms: [Investment transfer, Transfer reconciliation, Legend]
- Related evidence: [E-009, E-022]
- Follow-up question: Inventory all reconciliation fields, legend meanings, users, update cadence, and authoritative values.

### E-028: An insurance business monitor is desired but not prioritized

- Source: I-001, L0354-L0364
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Desired workflow
- Secondary categories: [Scope, Status or lifecycle]
- Time orientation: Desired future state
- Evidence type: Preference
- Decision status: Tentative
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder would value an insurance business monitor that expands identified insurance needs into application-status tracking after documents are signed.
- Business significance: A desired insurance workflow is described, but its release priority and complete stages are not settled.
- Related terms: [Insurance business monitor, Application]
- Related evidence: [E-008, E-013]
- Follow-up question: Is insurance monitoring in the first release, and what stages, fields, and owners does it include?

### E-029: Manual errors and missed updates can cause lost follow-up and business

- Source: I-001, L0365-L0380
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Failure and recovery
- Secondary categories: [Pain point, Risk]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Common problems include wrong amounts, missed assigned tasks, and missed updates; weekly manual review detects them, and consequences include missed follow-up and unbooked business.
- Business significance: Error detection is delayed and manual, with direct workflow and business impact.
- Related terms: [Input error, Manual review, Missed follow-up]
- Related evidence: [E-017, E-030, E-032]
- Follow-up question: How are detected errors corrected, approved, and communicated today?

### E-030: Change history is an unresolved interviewer proposal

- Source: I-001, L0381-L0385
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Audit requirement
- Secondary categories: [Suggestion, Contradiction]
- Time orientation: Desired future state
- Evidence type: Contradictory evidence
- Decision status: TBD
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder said retaining a wrong amount was not wanted for now, after which the interviewer proposed broad user and change tracking without obtaining explicit stakeholder acceptance.
- Business significance: No audit-history scope can be treated as approved from this exchange.
- Related terms: [Audit history, Change history]
- Related evidence: [E-008, E-023]
- Follow-up question: Which actions and prior values, if any, must be attributable and reviewable?

### E-031: Stopped work should be preserved, and closed work can reopen

- Source: I-001, L0386-L0394
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Business rule
- Secondary categories: [Failure and recovery, Status or lifecycle]
- Time orientation: Desired future state
- Evidence type: Explicit decision
- Decision status: Confirmed
- Follow-up priority: Important
- Decision owner: SPEAKER_00
- Finding: The stakeholder explicitly said work stopped midway should be preserved and completed or no-further-action work should sometimes be reopenable.
- Business significance: Terminal-looking states are not necessarily destructive or irreversible.
- Related terms: [Preserve, Reopen, No further action]
- Related evidence: [E-019, E-021]
- Follow-up question: Who may reopen work, under what conditions, and what prior state and history must remain visible?

### E-032: Concurrent updates are resolved manually

- Source: I-001, L0395-L0401
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Exception
- Secondary categories: [Failure and recovery, Current workflow]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Different employees may update the same information, and the described response is manual review followed by keeping the correct value.
- Business significance: The current conflict-resolution process lacks stated authority, evidence, and recovery rules.
- Related terms: [Concurrent update, Manual review]
- Related evidence: [E-024, E-029]
- Follow-up question: Who decides which value is correct, using what source, and should discarded values remain reviewable?

### E-033: Every task should remind, and overdue work has no grace period

- Source: I-001, L0402-L0420
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Business rule
- Secondary categories: [Desired workflow, Status or lifecycle]
- Time orientation: Desired future state
- Evidence type: Explicit decision
- Decision status: Confirmed
- Follow-up priority: Important
- Decision owner: SPEAKER_00
- Finding: The stakeholder said every task or step should have a reminder, users should be able to change follow-up dates, and overdue work has no grace period.
- Business significance: The supported rule establishes due-date behaviour but not escalation recipients or repeat timing.
- Related terms: [Reminder, Follow-up date, Overdue]
- Related evidence: [E-018, E-023]
- Follow-up question: What does escalation mean, who receives it, and how often does it repeat?

### E-034: Client and prospective client have distinct current definitions

- Source: I-001, L0424-L0436
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Data definition
- Secondary categories: [Status or lifecycle, User]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: A client is described as someone who has opened accounts and received a client number; a prospective client is an introduction from an existing client, event, colleague, or another source.
- Business significance: The terms rely on business events and identifiers that need authoritative confirmation.
- Related terms: [Client, Prospective client, Client number]
- Related evidence: [E-015, E-038, E-040]
- Follow-up question: Confirm exact entry and exit conditions for client and prospective-client status.

### E-035: Record grouping varies by workflow and family composition

- Source: I-001, L0437-L0476
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Business rule
- Secondary categories: [Data definition, Current workflow]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Blocker
- Decision owner: TBD
- Finding: RMS and Pillar commonly group couples, adult children may have separate potential business, minors remain grouped, and investment transfers are tracked separately by individual.
- Business significance: Person, couple, family, account, opportunity, and transfer are not interchangeable record boundaries.
- Related terms: [Household, Couple, Adult child, Minor, Account]
- Related evidence: [E-027, E-038]
- Follow-up question: Define grouping and identity rules for every workflow, including non-couple households and changes in family relationships.

### E-036: Focus 10 is a report of near-term potential business

- Source: I-001, L0482-L0490
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Reporting requirement
- Secondary categories: [Desired workflow, Data definition]
- Time orientation: Both
- Evidence type: Direct statement
- Decision status: Tentative
- Follow-up priority: Important
- Decision owner: TBD
- Finding: The stakeholder wants to identify ten potential businesses expected to close in the next three months and report them as Focus 10; entries may represent an individual or couple.
- Business significance: A named report has a partial selection rule but unresolved grouping and maintenance rules.
- Related terms: [Focus 10, Pillar, Potential business]
- Related evidence: [E-009, E-035, E-037]
- Follow-up question: Who selects Focus 10, how often is it refreshed, and what happens when an item closes or slips?

### E-037: Opportunity means estimated investable assets

- Source: I-001, L0499-L0500
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Data definition
- Secondary categories: [Current workflow, Reporting requirement]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: An opportunity in Pillar is described as the estimated amount of assets that could be invested.
- Business significance: This definition is investment-specific and does not establish whether other business types use the same concept.
- Related terms: [Opportunity, Estimated assets, Pillar]
- Related evidence: [E-020, E-025, E-036]
- Follow-up question: Does opportunity also cover insurance or other business, and what identifies a distinct opportunity?

### E-038: Records are linked by name and sometimes client number

- Source: I-001, L0501-L0512
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Data source
- Secondary categories: [Data definition, Risk]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Blocker
- Decision owner: TBD
- Finding: The same person commonly appears across RMS, Pillar, transfer, insurance, and Focus 10, currently matched by name and, after conversion, sometimes by client number.
- Business significance: Current cross-sheet matching is not a complete identity or duplicate-resolution rule.
- Related terms: [Name, Client number, Record linkage]
- Related evidence: [E-020, E-034, E-035, E-040]
- Follow-up question: What authoritative identifiers and manual resolution process distinguish people with similar, changed, or missing names?

### E-039: Pillar values and activity dates change frequently

- Source: I-001, L0525-L0527
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Status or lifecycle
- Secondary categories: [Current workflow, Data definition]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Next-activity information and dates change frequently, and Pillar distinguishes potential, in-progress, and actual values.
- Business significance: Frequently changing operational and value fields need clear meanings and transition evidence.
- Related terms: [Next activity, Potential, In progress, Actual]
- Related evidence: [E-019, E-024, E-040]
- Follow-up question: Define each value, its update trigger, and whether prior values matter.

### E-040: Authoritative data is only partly identified

- Source: I-001, L0528-L0537
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Data source
- Secondary categories: [Data definition, Open question]
- Time orientation: Current state
- Evidence type: Direct statement
- Decision status: Not a decision
- Follow-up priority: Blocker
- Decision owner: TBD
- Finding: In response to a source-of-truth question, the stakeholder identified actual closed business in Pillar and the new client number in RMS, but did not establish authority for other data.
- Business significance: Current spreadsheet use alone does not establish a complete source-of-truth map.
- Related terms: [Source of truth, Actual closed business, Client number]
- Related evidence: [E-024, E-034, E-038]
- Follow-up question: For every shared field, which source is authoritative and who may correct it?

### E-041: RMS continues after client conversion for appreciation activities

- Source: I-001, L0538-L0550
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Current workflow
- Secondary categories: [Status or lifecycle, Contradiction]
- Time orientation: Current state
- Evidence type: Contradictory evidence
- Decision status: Not a decision
- Follow-up priority: Important
- Decision owner: TBD
- Finding: Later in the interview, RMS is described as continuing after client conversion for a thank-you card, a three-month call, and appreciation events, contrary to an earlier statement that RMS stops at conversion.
- Business significance: The RMS lifecycle boundary is unresolved and affects current-workflow understanding.
- Related terms: [RMS, Client conversion, Appreciation]
- Related evidence: [E-013]
- Follow-up question: When does an RMS record become inactive, and which post-conversion activities belong to it?

### E-042: Spreadsheets are to be replaced, but import scope is not confirmed

- Source: I-001, L0551-L0554
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Scope
- Secondary categories: [Desired workflow, Data source]
- Time orientation: Desired future state
- Evidence type: Explicit decision
- Decision status: Confirmed
- Follow-up priority: Blocker
- Decision owner: SPEAKER_00
- Finding: The stakeholder explicitly agreed that the platform should entirely replace the spreadsheets; the interviewer’s further statement that import is only an initial transfer was not explicitly accepted.
- Business significance: Replacement is confirmed, while migration scope and whether any spreadsheet remains operational are not.
- Related terms: [Spreadsheet replacement, Import]
- Related evidence: [E-008, E-010, E-040]
- Follow-up question: Which spreadsheets and historical records are included, and is import strictly one-time?

### E-043: All users are preferred to have equal access for now

- Source: I-001, L0555-L0558
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Permission
- Secondary categories: [User, Scope]
- Time orientation: Desired future state
- Evidence type: Preference
- Decision status: Tentative
- Follow-up priority: Blocker
- Decision owner: SPEAKER_00
- Finding: When asked about roles, the stakeholder said everyone is equal “for now”; no system roles or permission boundaries were defined.
- Business significance: A provisional preference is insufficient to establish access to all client and workflow information. Professional review may also be required before finalizing access boundaries.
- Related terms: [Access, Role, Team member]
- Related evidence: [E-004, E-014, E-023]
- Follow-up question: Which actions and information may each business responsibility view, create, change, assign, approve, export, or delete?

### E-044: Document storage remains undecided

- Source: I-001, L0559-L0561
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Scope
- Secondary categories: [Data source, Open question]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: TBD
- Follow-up priority: Later
- Decision owner: SPEAKER_00
- Finding: Asked whether documents should be stored and linked to clients, the stakeholder said they might think about it.
- Business significance: Document storage is an unresolved idea, not accepted scope.
- Related terms: [Document storage, Client]
- Related evidence: [E-008]
- Follow-up question: Is document storage in scope; if so, which document types, purposes, owners, and professional constraints apply?

### E-045: Year-end carry-forward and retention need professional review

- Source: I-001, L0562-L0572
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Professional review required
- Secondary categories: [Current workflow, Status or lifecycle]
- Time orientation: Both
- Evidence type: Direct statement
- Decision status: Professional review required
- Follow-up priority: Important
- Decision owner: Appropriate Canadian privacy, legal, and compliance professionals
- Finding: Current spreadsheets start a new year, carry active or pending RMS and Pillar work forward, and do not carry prior-year booked Pillar business forward; this describes practice but does not establish lawful retention or deletion rules.
- Business significance: Retention, archival, and deletion cannot be inferred from annual spreadsheet rollover. Professional review required.
- Related terms: [Retention, Year-end, Carry-forward]
- Related evidence: [E-031, E-040]
- Follow-up question: What retention, archival, deletion, and legal-hold rules apply to each information category and record state?

### E-046: Additional process details may emerge during use

- Source: I-001, L0573-L0577
- Speaker: SPEAKER_00 and SPEAKER_01
- Primary category: Open question
- Secondary categories: [Risk, Scope]
- Time orientation: Desired future state
- Evidence type: Direct statement
- Decision status: TBD
- Follow-up priority: Later
- Decision owner: TBD
- Finding: The stakeholder did not identify another topic but expected additional process details or incorrect assumptions might emerge once use begins.
- Business significance: The interview is not represented as a complete account of every process detail.
- Related terms: [Assumption, Process detail]
- Related evidence: [E-008, E-011]
- Follow-up question: What review and validation activities will expose missing process details before affected proposals are approved?

## Current-State Workflow Summary

### Referral and relationship workflow

- Actors: Any trained team member may start the process; RMS and Pillar have different team ownership [E-012, E-014] (I-001, L0176-L0189).
- Trigger: An introduction or referral [E-012] (I-001, L0159-L0167).
- Current steps: Enter the referral in RMS, book a first meeting, identify potential business in Pillar, and continue into specialized tracking when appropriate [E-012, E-013] (I-001, L0159-L0173).
- Systems or documents used: RMS, Pillar, calendars, and later investment- or insurance-related spreadsheets [E-013, E-017] (I-001, L0163-L0173; L0224-L0233).
- Handoffs: RMS and Pillar have different team owners; task assignees may change [E-014, E-023] (I-001, L0176-L0183; L0304-L0316).
- Known problems: Duplicate entry, disconnected updates, poor information access, and missed tasks [E-001, E-002, E-029] (I-001, L0006-L0019; L0025-L0038; L0365-L0380).
- Exceptions: Referrals may lack a phone number, the same client may have multiple opportunities, and stopped work may later resume [E-020, E-031] (I-001, L0279-L0288; L0386-L0394).
- Supporting evidence: [E-001, E-002, E-012, E-013, E-014, E-017, E-020, E-023, E-029, E-031].
- Missing information: Exact stage transitions, ownership, required fields, correction steps, and the final RMS boundary remain unresolved [E-013, E-015, E-041] (I-001, L0163-L0173; L0190-L0198; L0538-L0550).

### Investment opportunity and transfer workflow

- Actors: Pillar is managed by two team members, tasks have changeable assignees, and client-specific reconciliation is shown in meetings [E-014, E-023, E-027] (I-001, L0176-L0183; L0304-L0316; L0328-L0353).
- Trigger: Identification of estimated potential business in Pillar [E-025, E-037] (I-001, L0320-L0341; L0499-L0500).
- Current steps: Evaluate potential business and pursue activities in Pillar; after agreement and signing, use investment transfer to track initiated, received, and invested events; use transfer reconciliation for client-specific detail [E-022, E-025, E-026, E-027] (I-001, L0296-L0302; L0320-L0353).
- Systems or documents used: Pillar, investment transfer, transfer reconciliation, and Focus 10 [E-025, E-027, E-036] (I-001, L0320-L0353; L0482-L0490).
- Handoffs: The exact handoff from evaluated opportunity to agreed or initiated transfer is not consistently defined [E-026] (I-001, L0324-L0341).
- Known problems: Estimated and actual amounts may appear to conflict, input amounts can be wrong, and updates may be missed [E-024, E-029] (I-001, L0317-L0323; L0365-L0380).
- Exceptions: Clients change their minds, transfers may be rejected, and received funds may not be invested [E-021, E-027] (I-001, L0292-L0302; L0350-L0353).
- Supporting evidence: [E-021, E-022, E-024, E-025, E-026, E-027, E-036, E-037].
- Missing information: Complete state definitions, transition authority, required data, correction rules, and authoritative values [E-022, E-024, E-026, E-040] (I-001, L0296-L0302; L0317-L0329; L0528-L0537).

### Insurance business workflow

- Actors: Team ownership is not identified [E-028] (I-001, L0354-L0364).
- Trigger: Identification of an insurance need in Pillar [E-028] (I-001, L0354-L0364).
- Current steps: Record the identified need in Pillar, discuss it with the client, and track application status after signing [E-028] (I-001, L0354-L0364).
- Systems or documents used: Pillar and the insurance business monitor [E-028] (I-001, L0354-L0364).
- Handoffs: The transcript implies movement from identification to application tracking but does not name an owner or exact handoff [E-028] (I-001, L0354-L0364).
- Known problems: None specifically stated for this workflow.
- Exceptions: None specifically stated for this workflow.
- Supporting evidence: [E-028].
- Missing information: Release priority, stages, fields, actors, assignments, outcomes, and exceptions [E-028] (I-001, L0354-L0364).

### Ongoing RMS and annual rollover workflow

- Actors: RMS is managed by one team member, while the exact performer of appreciation tasks is unstated [E-014, E-041] (I-001, L0176-L0183; L0538-L0550).
- Trigger: Client conversion does not necessarily end RMS activity [E-041] (I-001, L0538-L0550).
- Current steps: Add the client number, send a thank-you card, call after three months, support appreciation events, and carry active introductions into the next year [E-041, E-045] (I-001, L0538-L0550; L0562-L0572).
- Systems or documents used: RMS [E-041, E-045] (I-001, L0538-L0550; L0562-L0572).
- Handoffs: Not described.
- Known problems: The stated RMS end point conflicts within the interview [E-013, E-041] (I-001, L0170-L0173; L0538-L0550).
- Exceptions: Pending work carries forward, while prior-year actual booked Pillar business does not [E-045] (I-001, L0562-L0572).
- Supporting evidence: [E-013, E-041, E-045].
- Missing information: Authoritative lifecycle boundaries and professionally reviewed retention, archival, and deletion rules [E-041, E-045] (I-001, L0538-L0550; L0562-L0572).

## Desired-Future-State Summary

- Explicitly requested behaviour: Preserve work that stops midway and allow some completed or no-further-action work to reopen [E-031] (I-001, L0386-L0394).
- Explicitly requested behaviour: Give every task a reminder, allow follow-up dates to change, and treat work as overdue when due without a grace period [E-033] (I-001, L0402-L0420).
- Explicitly requested behaviour: Replace the current spreadsheets with the platform [E-042] (I-001, L0551-L0552).
- Stakeholder preference: Provide reminders in email and in the platform [E-018] (I-001, L0234-L0239).
- Stakeholder preference: Summarize information, create tasks, and recommend actions [E-005] (I-001, L0065-L0074).
- Stakeholder preference: Support an insurance business monitor and Focus 10 reporting [E-028, E-036] (I-001, L0354-L0364; L0482-L0490).
- Interviewer suggestion: Add broad user and change history; the stakeholder did not accept it and had just rejected preserving a wrong input for now [E-030] (I-001, L0381-L0385).
- Interviewer suggestion: Treat import as an initial-only transfer; the stakeholder explicitly accepted spreadsheet replacement but not this additional limitation [E-042] (I-001, L0551-L0554).
- Unresolved idea: Give all users equal access “for now”; permission boundaries remain undefined [E-043] (I-001, L0555-L0558).
- Unresolved idea: Store documents linked to clients [E-044] (I-001, L0559-L0561).

## Confirmed Decisions

- E-031 — Preserve stopped work and allow some closed work to reopen (I-001, L0386-L0394).
- E-033 — Remind for every task, permit due-date changes, and use no overdue grace period (I-001, L0402-L0420).
- E-042 — Replace the current spreadsheets with the platform (I-001, L0551-L0552).

## Tentative Decisions and Preferences

- E-004 — All team members are expected to use the platform. Source: I-001, L0055-L0064. Decision owner: TBD. Required follow-up: Map business responsibilities and direct users.
- E-005 — The platform should summarize, create tasks, and recommend actions. Source: I-001, L0065-L0074. Decision owner: TBD. Required follow-up: Define approved calculations and recommendations.
- E-006 — Automatic client email is excluded for now. Source: I-001, L0075-L0082. Decision owner: TBD. Required follow-up: None for the first release.
- E-009 — Search and timely reports are important. Source: I-001, L0126-L0134. Decision owner: TBD. Required follow-up: Identify named reports and filters.
- E-010 — NCCP and M&M are tentatively outside replacement and integration scope. Source: I-001, L0137-L0142. Decision owner: TBD. Required follow-up: Confirm explicit non-goals.
- E-018 — Reminders are preferred in email and in the platform. Source: I-001, L0234-L0239. Decision owner: TBD. Required follow-up: Define recipients and delivery behaviour.
- E-028 — An insurance business monitor is desired. Source: I-001, L0354-L0364. Decision owner: TBD. Required follow-up: Decide release priority and workflow coverage.
- E-036 — Focus 10 reporting is desired. Source: I-001, L0482-L0490. Decision owner: TBD. Required follow-up: Define selection and refresh rules.
- E-043 — Users are preferred to have equal access for now. Source: I-001, L0555-L0558. Decision owner: SPEAKER_00. Required follow-up: Establish reviewed permission boundaries.

## Contradictions

### C-001: First-release workflow priority conflict

- Evidence A: Transfers are repeated as part of the immediate top-three workflows.
- Source lines: I-001, L0103-L0117
- Evidence B: Investment transfer or audit history may be excluded from the first version, while search and reporting are also said to be important for it.
- Source lines: I-001, L0124-L0129
- Nature of conflict: The discussion does not establish a consistent ordered first-release scope.
- Affected workflow: Referral, transfer, audit, search, and reporting
- Required decision: Approve an ordered first-release scope and explicit non-goals.
- Proposed decision owner: TBD
- Priority: Blocker

### C-002: Change-history intent conflict

- Evidence A: The stakeholder does not want a record of the wrong input for now.
- Source lines: I-001, L0381-L0383
- Evidence B: The interviewer immediately says broad change and user history will be added, without explicit stakeholder agreement.
- Source lines: I-001, L0384-L0385
- Nature of conflict: Stakeholder intent and the interviewer’s proposed implementation direction are not reconciled.
- Affected workflow: Error correction and audit history
- Required decision: Define whether prior values and user actions must remain visible, and obtain any required professional review.
- Proposed decision owner: TBD
- Priority: Important

### C-003: RMS lifecycle boundary conflict

- Evidence A: RMS is said to stop when an introduction becomes a new client.
- Source lines: I-001, L0170-L0173
- Evidence B: RMS is later said not to stop at conversion and to continue through thank-you, three-month call, and appreciation activities.
- Source lines: I-001, L0538-L0550
- Nature of conflict: The end point and purpose of RMS are described differently.
- Affected workflow: Referral, client conversion, and ongoing relationship activity
- Required decision: Confirm the RMS lifecycle, inactive condition, and post-conversion activities.
- Proposed decision owner: TBD
- Priority: Important

## Open Questions

### Product ownership

- Who has final authority to approve product scope and workflow rules? Related evidence: [E-008, E-046]. Supporting lines: I-001, L0098-L0129; L0573-L0577. Priority: Blocker. Suggested decision owner: TBD. Blocks: The first OpenSpec proposal.

### Users and roles

- What business responsibilities does each user group perform, independent of job title or system role? Related evidence: [E-004, E-014]. Supporting lines: I-001, L0055-L0064; L0176-L0189. Priority: Important. Suggested decision owner: TBD. Blocks: A users-and-workflow discovery document or affected proposal.

### Workflow

- What event, required information, owner, and failure behaviour govern each RMS-to-Pillar-to-specialized-tracking transition? Related evidence: [E-013, E-015, E-026, E-028]. Supporting lines: I-001, L0163-L0173; L0190-L0198; L0324-L0341; L0354-L0364. Priority: Blocker. Suggested decision owner: TBD. Blocks: The first affected workflow proposal.
- When does RMS become inactive, and which post-conversion activities remain part of it? Related evidence: [E-013, E-041]. Supporting lines: I-001, L0170-L0173; L0538-L0550. Priority: Important. Suggested decision owner: TBD. Blocks: An RMS lifecycle proposal.

### Business rules

- What states, next actions, required data, reversibility, and evidence apply when a client changes their mind or a transfer is rejected? Related evidence: [E-021, E-022, E-031]. Supporting lines: I-001, L0292-L0302; L0386-L0394. Priority: Important. Suggested decision owner: TBD. Blocks: A transfer workflow proposal.
- Who may reopen completed or no-further-action work, under what conditions, and what history remains? Related evidence: [E-031]. Supporting lines: I-001, L0386-L0394. Priority: Important. Suggested decision owner: TBD. Blocks: A lifecycle and recovery proposal.

### Permissions

- Which business responsibilities may view, create, change, assign, approve, export, or delete each information type? Related evidence: [E-004, E-014, E-023, E-043]. Supporting lines: I-001, L0055-L0064; L0176-L0189; L0304-L0316; L0555-L0558. Priority: Blocker. Suggested decision owner: TBD. Blocks: The first proposal involving access or client information.

### Data and sources of truth

- What uniquely identifies a person, couple, family, account, opportunity, and transfer, and how are ambiguous matches resolved? Related evidence: [E-020, E-035, E-038]. Supporting lines: I-001, L0279-L0288; L0437-L0476; L0501-L0512. Priority: Blocker. Suggested decision owner: TBD. Blocks: The first proposal involving cross-workflow records or import.
- Which source and owner are authoritative for every shared field, and how are corrections reconciled? Related evidence: [E-024, E-032, E-040]. Supporting lines: I-001, L0317-L0323; L0395-L0401; L0528-L0537. Priority: Blocker. Suggested decision owner: TBD. Blocks: The first data migration or shared-record proposal.
- Which spreadsheets and historical periods are included in replacement, and is import one-time? Related evidence: [E-042]. Supporting lines: I-001, L0551-L0554. Priority: Important. Suggested decision owner: TBD. Blocks: An import or spreadsheet-retirement proposal.

### Statuses and lifecycle

- What do APNA, BNA, no further action, potential, in progress, and actual mean, and what events move work among them? Related evidence: [E-019, E-039]. Supporting lines: I-001, L0257-L0268; L0525-L0527. Priority: Important. Suggested decision owner: TBD. Blocks: A task or Pillar lifecycle proposal.
- What is the complete transfer state model, including received-but-not-invested outcomes? Related evidence: [E-022, E-027]. Supporting lines: I-001, L0296-L0302; L0328-L0353. Priority: Important. Suggested decision owner: TBD. Blocks: A transfer proposal.

### Reporting

- Which reports, filters, recipients, timing, and source values are needed in the first release? Related evidence: [E-009, E-027, E-036]. Supporting lines: I-001, L0126-L0134; L0328-L0353; L0482-L0490. Priority: Important. Suggested decision owner: TBD. Blocks: A reporting proposal.
- Who selects and refreshes Focus 10, and how are closed or delayed items handled? Related evidence: [E-036]. Supporting lines: I-001, L0482-L0490. Priority: Later. Suggested decision owner: TBD. Blocks: A Focus 10 proposal.

### Audit and history

- Which actions and prior values must be attributable or reviewable, and by whom? Related evidence: [E-023, E-030, E-032]. Supporting lines: I-001, L0304-L0316; L0381-L0401. Priority: Important. Suggested decision owner: TBD. Blocks: An audit-history or error-correction proposal.

### Scope and priorities

- What is the approved ordered first-release scope, including referrals, transfers, insurance, search, reporting, audit, and explicit non-goals? Related evidence: [E-008, E-009, E-010, E-028, E-042, E-044]. Supporting lines: I-001, L0098-L0142; L0354-L0364; L0551-L0561. Priority: Blocker. Suggested decision owner: TBD. Blocks: The first OpenSpec proposal.

### Success measures

- What baselines, targets, and evaluation period define improved efficiency, follow-up, record production, and adoption? Related evidence: [E-003, E-007, E-011]. Supporting lines: I-001, L0014-L0015; L0043-L0054; L0083-L0094; L0143-L0153. Priority: Important. Suggested decision owner: TBD. Blocks: A success-measure or product-review document.

### Operations and recovery

- How are wrong or concurrent values corrected, who approves the result, and what evidence must remain? Related evidence: [E-024, E-029, E-032]. Supporting lines: I-001, L0317-L0323; L0365-L0380; L0395-L0401. Priority: Important. Suggested decision owner: TBD. Blocks: An error-correction or recovery proposal.
- What happens when reminder email delivery fails or a responsible person is unavailable? Related evidence: [E-018, E-033]. Supporting lines: I-001, L0234-L0239; L0402-L0420. Priority: Later. Suggested decision owner: TBD. Blocks: A reminder-operations proposal.

## Professional-Review Questions

### Privacy

- What retention, deletion, and access obligations apply to prospect and client information across active and prior-year records? Related evidence: [E-043, E-045]. Supporting lines: I-001, L0555-L0558; L0562-L0572. Priority: Important. Suggested decision owner: Appropriate Canadian privacy professional.

### Compliance

- What recordkeeping obligations apply to annual rollover, prior-year booked business, pending opportunities, and recorded client-contact activity? Related evidence: [E-016, E-045]. Supporting lines: I-001, L0204-L0221; L0562-L0572. Priority: Important. Suggested decision owner: Appropriate Canadian wealth-management compliance professional.

### Retention

- What approved retention, archival, deletion, and legal-hold schedule applies to each information category and lifecycle state? Related evidence: [E-045]. Supporting lines: I-001, L0562-L0572. Priority: Important. Suggested decision owner: Appropriate Canadian privacy, legal, and compliance professionals.

### Suitability

No transcript-supported professional-review question was identified.

### Tax

No transcript-supported professional-review question was identified.

### Legal

- Does the described annual practice of not carrying prior-year actual booked Pillar business forward satisfy applicable record-preservation obligations? Related evidence: [E-045]. Supporting lines: I-001, L0562-L0572. Priority: Important. Suggested decision owner: Appropriate Canadian legal and compliance professionals.

### Security governance

- What approved access boundaries are required before adopting the tentative preference that all users be equal? Related evidence: [E-004, E-043]. Supporting lines: I-001, L0055-L0064; L0555-L0558. Priority: Blocker. Suggested decision owner: Appropriate security-governance and privacy professionals.

## Coverage Gaps

### Final decision authority

- Why the transcript is insufficient: SPEAKER_00 makes several choices, but the interview does not establish final authority for product, business-rule, or professional decisions.
- Related evidence IDs: [E-008, E-046]
- Supporting lines: I-001, L0098-L0129; L0573-L0577
- Suggested follow-up question: Who has final decision authority for scope, workflow rules, permissions, and acceptance?
- Priority: Blocker
- Suggested decision owner: TBD

### User roles and access boundaries

- Why the transcript is insufficient: It identifies all team members as users and says they are equal for now, but does not map responsibilities or access boundaries.
- Related evidence IDs: [E-004, E-014, E-043]
- Supporting lines: I-001, L0055-L0064; L0176-L0189; L0555-L0558
- Suggested follow-up question: Map each business responsibility to permitted actions and information.
- Priority: Blocker
- Suggested decision owner: TBD

### Core domain definitions

- Why the transcript is insufficient: Some terms are defined, but person, household, couple, account, opportunity, and workflow grouping rules remain incomplete.
- Related evidence IDs: [E-034, E-035, E-037, E-038]
- Supporting lines: I-001, L0424-L0476; L0499-L0512
- Suggested follow-up question: Approve a business glossary with examples and counterexamples for each core term.
- Priority: Blocker
- Suggested decision owner: TBD

### Sources of truth

- Why the transcript is insufficient: Only actual closed business and client number receive partial answers to the source-of-truth question.
- Related evidence IDs: [E-024, E-038, E-040]
- Supporting lines: I-001, L0317-L0323; L0501-L0512; L0528-L0537
- Suggested follow-up question: Identify the authoritative source, owner, and correction process for every shared field.
- Priority: Blocker
- Suggested decision owner: TBD

### Required reports

- Why the transcript is insufficient: Reporting and Focus 10 are important, but report inventory, definitions, recipients, timing, and acceptance criteria are absent.
- Related evidence IDs: [E-009, E-027, E-036]
- Supporting lines: I-001, L0126-L0134; L0328-L0353; L0482-L0490
- Suggested follow-up question: Provide a prioritized report catalogue with purpose, fields, filters, source values, recipient, and cadence.
- Priority: Important
- Suggested decision owner: TBD

### Error-correction processes

- Why the transcript is insufficient: Weekly manual detection and manual resolution are described, but authority, correction steps, notification, reversibility, and history are not.
- Related evidence IDs: [E-029, E-030, E-032]
- Supporting lines: I-001, L0365-L0401
- Suggested follow-up question: Walk through one synthetic wrong-value correction from detection through approval and closure.
- Priority: Important
- Suggested decision owner: TBD

### Duplicate handling

- Why the transcript is insufficient: Repeat Pillar entries can be valid and names link records, but no accidental-duplicate detection or resolution rule is given.
- Related evidence IDs: [E-020, E-035, E-038]
- Supporting lines: I-001, L0279-L0288; L0437-L0476; L0501-L0512
- Suggested follow-up question: How are legitimate repeat opportunities distinguished from duplicate people or entries?
- Priority: Blocker
- Suggested decision owner: TBD

### Audit expectations

- Why the transcript is insufficient: The stakeholder rejects preserving a wrong input for now, while the interviewer proposes broad history without explicit acceptance.
- Related evidence IDs: [E-030]
- Supporting lines: I-001, L0381-L0385
- Suggested follow-up question: Which events and prior values must be attributable and reviewable, subject to professional confirmation?
- Priority: Important
- Suggested decision owner: TBD

### Retention requirements

- Why the transcript is insufficient: Annual spreadsheet carry-forward practice is described, but no professionally approved retention, archival, deletion, or hold rules are established.
- Related evidence IDs: [E-045]
- Supporting lines: I-001, L0562-L0572
- Suggested follow-up question: Obtain a professionally approved schedule by record category and lifecycle state.
- Priority: Important
- Suggested decision owner: Appropriate Canadian privacy, legal, and compliance professionals

### Success metrics

- Why the transcript is insufficient: Desired value and failure conditions are qualitative, with no baselines, targets, dates, or measurement owner.
- Related evidence IDs: [E-003, E-007, E-011]
- Supporting lines: I-001, L0014-L0015; L0043-L0054; L0083-L0094; L0143-L0153
- Suggested follow-up question: What synthetic or operational measures will establish improvement, and over what period?
- Priority: Important
- Suggested decision owner: TBD

### V1 priorities and non-goals

- Why the transcript is insufficient: Immediate priorities conflict with possible exclusions, and several suggested capabilities lack explicit acceptance.
- Related evidence IDs: [E-006, E-008, E-009, E-010, E-028, E-030, E-042, E-044]
- Supporting lines: I-001, L0075-L0082; L0098-L0142; L0354-L0364; L0381-L0385; L0551-L0561
- Suggested follow-up question: Approve an ordered V1 list and a separate explicit non-goal list.
- Priority: Blocker
- Suggested decision owner: TBD

### Failure and recovery expectations

- Why the transcript is insufficient: Missed work, rejected transfers, stopped work, and concurrent edits are mentioned, but complete recovery paths and operational ownership are absent.
- Related evidence IDs: [E-021, E-029, E-031, E-032]
- Supporting lines: I-001, L0292-L0302; L0365-L0401
- Suggested follow-up question: For each failure scenario, define detection, responsible actor, recovery steps, evidence, and closure.
- Priority: Important
- Suggested decision owner: TBD
