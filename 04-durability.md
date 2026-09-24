# D — Durability

> **Definition:** Once a transaction is **committed**, its effects are permanent — they survive crashes, power loss, and even disk failures (with replication).

Durability is the "no take-backs" guarantee. After `COMMIT`, the data is safe.

## The Write-Ahead Log (WAL) Example

The classic way databases achieve durability is the **Write-Ahead Log**:

1. Before applying a change to the main data file, the database writes the change to a **log file** on disk.
2. Only after the log entry is safely on disk does the database apply the change to the data file.
3. If the system crashes, on restart the database **replays the log** to recover any changes that were committed but not yet written to the data file.

### Timeline of a Committed Transaction

```
1. Client sends: UPDATE accounts SET balance = 500 WHERE id = 1;
2. DB writes log entry: "UPDATE accounts id=1 balance=500"  → flushed to disk
3. DB applies the change to the in-memory buffer
4. DB writes COMMIT record to the log → flushed to disk
5. DB acknowledges COMMIT to the client
6. (Later) DB writes the updated page to the data file
```

If the server crashes at step 4, on restart the DB sees the COMMIT record and knows the update is durable — it will re-apply it.

If the server crashes at step 2 (before COMMIT), the log entry is discarded and the update is rolled back.

## Why "Write-Ahead"?

The log is written **before** the data file. This guarantees that if we ever need to recover, we have a complete record of what happened.

## Durability vs Atomicity

- **Atomicity** uses the log to **undo** uncommitted changes.
- **Durability** uses the log to **redo** committed changes after a crash.

Same mechanism, opposite directions.

## What Durability Does NOT Guarantee

- A single disk failure can still lose data (unless you have backups or replication).
- Durability is about **committed** transactions only — uncommitted work is not durable.

## Key Takeaways

- ✅ After `COMMIT`, data is safe from crashes.
- ✅ The write-ahead log is the core mechanism.
- ✅ Recovery replays the log on restart.
- ✅ For real-world safety, add backups and replication on top of durability.

## Try It Yourself

```sql
-- In PostgreSQL, check the WAL location and status
SELECT pg_current_wal_lsn();

-- Force a checkpoint (writes dirty pages to disk)
CHECKPOINT;
```

---

**Next:** [Summary Table →](summary-table.md)
