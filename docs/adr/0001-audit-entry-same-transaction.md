---
Status: accepted
---

# Audit History entries are written in the same database transaction as the change they audit

Every command the specs describe as all-or-nothing runs inside a single PostgreSQL transaction that
also inserts its Audit History rows. If the audit insert fails, the transaction rolls back and the
business change does not happen. This is what makes `docs/product/vision.md`'s requirement — that a
change needing Audit History "succeeds with its history entry or fails entirely" — true by
construction rather than by convention.

Immutability is enforced at the database, not only in the module interface: the application connects
as a **non-owning** role, distinct from the migration and reset role, holding `INSERT` and `SELECT`
only on the audit store, with no `UPDATE`, `DELETE`, `TRUNCATE`, or DDL privilege. Non-owning is the
load-bearing part — in PostgreSQL a table's owner retains implicit rights over it regardless of
explicit grants, so grants alone would not enforce immutability against an owning role.

## Considered Options

- **Transactional outbox with an asynchronous drain.** Rejected: an outbox accepts an unaudited
  change and repairs it later, which is exactly the state `docs/product/vision.md` rules out.
- **An application-level "audit first, then change" sequence.** Rejected: it can leave a recorded
  change that never happened, which is a worse failure than a missing entry because the record
  reads as authoritative.

## Consequences

- The audit store becomes a hard dependency of every write. When it is unavailable, the platform
  accepts no business change at all.
- Audit growth is unbounded: entries are append-only and nothing prunes them, so storage grows
  monotonically with use.
- The audit store is the sole accountability record while operational backup and recovery are
  deferred (see `proposal.md` — Data Sensitivity, which makes backup and recovery a precondition for
  real client use), so losing it loses all accountability history with no restore path.

Open tension: `proposal.md` Unresolved Human Decision 4 records that if professional review
establishes an erasure or right-to-be-forgotten obligation, it conflicts directly with the
append-only model, and this ADR would have to be revisited together with the never-deleted rule.
