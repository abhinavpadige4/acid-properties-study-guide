# I — Isolation

> **Definition:** Concurrent transactions must not interfere with each other. Each transaction should behave as if it's the only one running on the database.

Isolation is what makes multi-user databases safe. Without it, two users acting at the same time could produce wrong results.

## The Concurrent Ticket Booking Example

Imagine a concert with **1 ticket left**. Two users, Alice and Bob, try to buy it at the same time.

### Without Isolation (bad — "lost update" / double-sell)

```
Time  Transaction A (Alice)          Transaction B (Bob)
────  ─────────────────────          ─────────────────────
t1    SELECT tickets WHERE id=1      SELECT tickets WHERE id=1
      → sees 1 ticket                  → sees 1 ticket
t2    UPDATE tickets SET qty=0       UPDATE tickets SET qty=0
t3    COMMIT                         COMMIT
```

Both users "saw" 1 ticket and both bought it. **The concert is oversold.**

### With Isolation (good)

The database serializes the two transactions so they don't see each other's uncommitted changes:

```
Time  Transaction A (Alice)          Transaction B (Bob)
────  ─────────────────────          ─────────────────────
t1    BEGIN
t2    SELECT tickets WHERE id=1      BEGIN
      → sees 1 ticket                SELECT tickets WHERE id=1
                                     → BLOCKED (row locked)
t3    UPDATE tickets SET qty=0
t4    COMMIT                         → unblocked, sees 0 tickets
t5                                     UPDATE tickets SET qty=0
t6                                     COMMIT (or rollback)
```

Only one user gets the ticket. ✅

## Isolation Levels (SQL Standard)

Databases let you trade off isolation for performance. From weakest to strongest:

| Level | Dirty Read | Non-repeatable Read | Phantom Read |
|-------|:----------:|:-------------------:|:------------:|
| **READ UNCOMMITTED** | ✅ | ✅ | ✅ |
| **READ COMMITTED** (PostgreSQL default) | ❌ | ✅ | ✅ |
| **REPEATABLE READ** (MySQL InnoDB default) | ❌ | ❌ | ✅* |
| **SERIALIZABLE** | ❌ | ❌ | ❌ |

\* PostgreSQL's REPEATABLE READ actually prevents phantoms too.

### What are those anomalies?

- **Dirty read** — reading data another transaction hasn't committed yet.
- **Non-repeatable read** — reading the same row twice and getting different values.
- **Phantom read** — a new row appears (or disappears) between two reads of the same range.

## How Databases Implement Isolation

- **Locking** — row-level or table-level locks prevent concurrent writes.
- **Multi-Version Concurrency Control (MVCC)** — each transaction sees a snapshot of the data (used by PostgreSQL).
- **Serializable Snapshot Isolation (SSI)** — detects conflicts and rolls back one transaction.

## Key Takeaways

- ✅ Isolation prevents concurrent transactions from corrupting each other's results.
- ✅ Higher isolation = safer but slower (more locking / conflict detection).
- ✅ PostgreSQL defaults to READ COMMITTED; use SERIALIZABLE for strict correctness.
- ✅ The ticket-booking example is the classic "lost update" problem.

## Try It Yourself

```sql
-- In PostgreSQL, force strict isolation
BEGIN ISOLATION LEVEL SERIALIZABLE;
  SELECT * FROM tickets WHERE id = 1;
  UPDATE tickets SET qty = qty - 1 WHERE id = 1 AND qty > 0;
COMMIT;
-- If two such transactions conflict, one will be rolled back with a serialization error.
```

---

**Next:** [Durability →](04-durability.md)
