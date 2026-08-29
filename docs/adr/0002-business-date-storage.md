---
Status: proposed
---

# Business date-only values are stored as calendar dates; times of day are applied in the Firm Time Zone at comparison and display time

Points in time (attempt timestamps, audit timestamps, completion times) are stored as UTC instants.
Date-only business values (Referral Received Date, task due date, Event date, Client Start Date,
firm calendar entries) are stored as calendar dates with no time zone. The 5:00 p.m. due time and the
9:00 a.m. reminder time are applied by the `scheduling` module in the IANA `America/Edmonton` zone at
the moment of comparison or display, so daylight saving is handled by the zone database rather than
by arithmetic.

## Considered Options

- **Store due dates as timestamps already resolved to 5:00 p.m. local.** Rejected: a later
  firm-calendar or policy change to the due time would require rewriting stored data, and it invites
  accidental time-zone drift across daylight-saving boundaries.
- **Store all values as UTC instants, including date-only business values.** Rejected: a calendar
  date is not a point in time, and forcing one into an instant makes the stored value depend on the
  zone that was in effect when it was written.

## Consequences

- Every read path that compares "now" to a due date must go through `scheduling`. Comparing a stored
  date against a raw clock anywhere else is a bug, not a shortcut.
