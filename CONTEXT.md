# Client Management Platform

Shared business language for the firm's client-management workflows.

## Language

**Referral**:
The continuing pre-client intake record for a Household, regardless of how often or through how many sources the Household reaches the firm.
_Avoid_: Introduction, intake

**Prospect**:
A Household Member being introduced to the firm through a Referral. In the first slice every member of a Prospective Household is a Prospect; the Referral tracks their introduction and outreach.
_Avoid_: Referral, Referral Source

**Referral Source**:
One origin attributed to a Referral. A Referral may have multiple equally attributed sources; Client and Centre of Influence sources identify the specific originating party.
_Avoid_: Prospect

**Referral Relationship**:
The relationship between a referring client and the prospective Household collectively. Values are `Friend`, `Relative`, `Colleague`, `Service provider`, and `Other`; it is `N/A` when the Referral did not come through a client.
_Avoid_: inferring the meaning of legacy relationship codes

**Referral Received Date**:
The calendar date the firm first received a Referral, distinct from when it was entered or activated. It remains the Referral's original date when later sources are added.
_Avoid_: Date received, entry date

**Centre of Influence (COI)**:
An individual person who may be identified as a Referral Source. A COI is not an organization record in V1.
_Avoid_: Centre, COI organization

**Household**:
One or more people managed together from prospective Referral through the client relationship and client-appreciation activity. A Household may contain a single person and may be prospective, client, or former client; conversion does not create a replacement Household.
_Avoid_: Client group, family

**Household Member**:
An individual person within a Household, with their own name and contact information. A person belongs to only one active Household at a time; moving them preserves their earlier Household history.
_Avoid_: treating the Household and an individual client as interchangeable

**Client Number**:
The firm-assigned, platform-wide unique client identifier assigned to an individual Household Member after that person becomes a client. A member has at most one Client Number; it does not identify the Household as a whole.

**Pre-existing Client Household**:
A client Household whose relationship with the firm predates its entry into the platform. Its original Client Start Date may be unknown; recording it establishes source-reference data without treating it as newly converted or creating new-client appreciation tasks.

**Referral Owner**:
The staff member currently accountable for the Referral as a whole. A Referral has one current owner even when its tasks have other assignees.

**Associate**:
An individually identified internal staff member who may own workflows or receive task assignments. V1 records the Associate's name, work email, active status, and linked Platform User account. Associate identity does not imply a role or permission level.

**Platform User**:
An authenticated identity linked to one Associate and used to establish who is acting in the platform. In V1, every Platform User has equal access; authentication does not imply distinct roles or permissions.
_Avoid_: treating an editable Associate selection as the identity of the acting user

**Identity Administrator**:
The firm lead or another specifically designated person who may grant or revoke Platform User sign-in access and maintain the Firm Business Day calendar. This limited administrative authority is separate from V1's equal access to business records.

**Task Assignee**:
The staff member responsible for one task; this person may differ from the Referral Owner or Household Owner.

**Firm Business Day**:
A Monday-through-Friday day not excluded by the firm's maintained holiday and closure calendar. Task scheduling and reminders use this calendar consistently.
_Avoid_: Alberta Business Day

**Firm Time Zone**:
The `America/Edmonton` timezone used for business dates, task times, reminders, attempts, and displayed Audit History timestamps regardless of an evaluator's local timezone.
_Avoid_: Browser local time

**Draft**:
The pre-outreach Referral state used while minimum intake information or a Referral Owner is missing.

**Discarded Draft**:
An inactive state for a mistaken or abandoned Draft Referral. It is hidden from active work, retained for history, and cannot be restored; a later legitimate intake reuses the Household but creates a new Draft Referral.

**In Process**:
The active Referral status used while outreach continues.
_Avoid_: Final status

**Referral Status**:
The current lifecycle status of a Referral: `Draft`, `In process`, `Became client`, one of the three NFAR dispositions, or the administrative inactive states `Discarded Draft` and `Merged`.
_Avoid_: Final status

**Open Task**:
An unfinished task, including a follow-up-call task with unsuccessful attempts.
_Avoid_: In process

**Skipped Outreach Stage**:
An intentionally unperformed outreach stage with a required reason. Its task is Cancelled while the workflow advances to the next stage.
_Avoid_: Skipped Task

**Entered in Error**:
A retained activity that staff identify as wholly false rather than deleting. It remains visible in history and does not count as evidence for workflow decisions.
_Avoid_: Deleted activity

**NFAR**:
A closing Referral disposition qualified by one of three reasons: not interested in the firm's services, no business opportunity, or no response.

**Merged**:
An inactive Referral state indicating that a duplicate Referral was consolidated into, and remains linked to, a surviving Referral.

**Client Start Date**:
The date the Household's first member receives a Client Number and has their first account opened. It begins the Household's client relationship and creates its initial client-appreciation tasks; later member conversions do not restart those tasks.

**Client Appreciation**:
Task types and Event activity for new- and existing-client Households, including thank-you cards, service calls, and invitations. It is not a separate user-facing workflow record in V1.

**Household Owner**:
The staff member currently accountable for a client Household. Client-appreciation tasks default to this person but may have different assignees.

**Waiting for Next New-Client Event**:
The client-appreciation state used when a new-client Household is eligible for its first event but no event date exists yet; it is not a task and has no due date.

**Do Not Invite**:
A Household preference that excludes it from appreciation-event invitation selection until the preference is removed.

**Former Client**:
An inactive Household state used when the Household no longer has an active client relationship with the firm.

**Pillar List Workflow**:
The separate workflow for potential new business presented by an existing client Household.

**Audit History**:
The non-routine record of every RMS change, including previous and new values, who made the change, and when.
