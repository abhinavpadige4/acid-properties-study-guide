# A — Atomicity

> **Definition:** A transaction is **all-or-nothing**. Either every operation in the transaction completes successfully, or none of them do — as if the transaction never happened.

## The Bank Transfer Example

Suppose Alice wants to send $100 to Bob. The database must do two things:

1. Subtract $100 from Alice's balance.
2. Add $100 to Bob's balance.

These two steps must happen **together**. If step 1 succeeds but step 2 fails (say, the server crashes right after Alice's balance is updated), Alice loses $100 out of thin air. That's a disaster.

### Without Atomicity (bad)

```sql
UPDATE accounts SET balance = balance - 100 WHERE id = 'alice';
-- 💥 CRASH HERE — Bob never receives the money
UPDATE accounts SET balance = balance + 100 WHERE id = 'bob';
```

Result: Alice is charged, Bob is not credited. **$100 vanished.**

### With Atomicity (good)

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 'alice';
  UPDATE accounts SET balance = balance + 100 WHERE id = 'bob';
COMMIT;
```

If anything goes wrong between `BEGIN` and `COMMIT`, the database **rolls back** every change made by that transaction. Alice's balance is restored, and it's as if the transfer never happened.

## How Databases Implement Atomicity

- **Transaction log / write-ahead log (WAL):** every change is recorded before it's applied.
- **Undo logs:** enough information to reverse each change.
- **Rollback mechanism:** on failure, the DB replays undo logs in reverse order.

## Key Takeaways

- ✅ A transaction is a single logical unit — it can't be partially applied.
- ✅ `BEGIN ... COMMIT` marks the transaction boundaries.
- ✅ `ROLLBACK` (automatic or manual) undoes all changes.
- ✅ Atomicity is what makes multi-step operations like transfers, orders, and inventory updates safe.

## Try It Yourself

```sql
-- Simulate a failure mid-transaction
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 'alice';
  SELECT 1/0;  -- force an error
COMMIT;
-- The UPDATE is rolled back automatically.
```

---

**Next:** [Consistency →](02-consistency.md)
