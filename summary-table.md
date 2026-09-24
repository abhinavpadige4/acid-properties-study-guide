# ACID Summary Table

A quick reference for all four properties.

| Property | Question it answers | Example | Mechanism |
|----------|---------------------|---------|-----------|
| **Atomicity** | "Did the transaction fully happen, or not at all?" | Bank transfer: Alice −$100, Bob +$100 | Transaction log, undo logs, rollback |
| **Consistency** | "Are all database rules still satisfied?" | Foreign key: every order has a valid customer | Constraints, triggers, CHECK, PK/FK |
| **Isolation** | "Do concurrent transactions interfere?" | Ticket booking: only one user gets the last ticket | Locks, MVCC, SERIALIZABLE |
| **Durability** | "Will committed data survive a crash?" | Write-ahead log: committed updates are replayed on restart | WAL, checkpoints, redo logs |

## How They Work Together

```
        ┌─────────────────────────────────────────┐
        │              TRANSACTION                 │
        │                                         │
        │  BEGIN                                  │
        │    ├─ Atomicity: all-or-nothing         │
        │    ├─ Consistency: rules must hold      │
        │    ├─ Isolation: no interference        │
        │    └─ (work happens)                    │
        │  COMMIT                                 │
        │    └─ Durability: changes are permanent │
        └─────────────────────────────────────────┘
```

## Common Interview Questions

1. **What happens if a transaction fails halfway?**
   → Atomicity rolls it back; the database stays consistent.

2. **What's the difference between READ COMMITTED and SERIALIZABLE?**
   → READ COMMITTED allows non-repeatable and phantom reads; SERIALIZABLE prevents all anomalies.

3. **How does a database recover after a crash?**
   → It replays the WAL: redo committed transactions, undo uncommitted ones.

4. **Can a database be ACID-compliant and still be slow?**
   → Yes. Higher isolation and durability come with performance costs (locking, disk I/O).

5. **What's the opposite of ACID?**
   → BASE (Basically Available, Soft state, Eventual consistency) — used in distributed NoSQL systems.
