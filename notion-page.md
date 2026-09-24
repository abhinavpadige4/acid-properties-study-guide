# 📚 ACID Properties — Notion Page

> A Notion-style study page for ACID properties. Copy this content into Notion and use the toggle headings and checkable bullets as designed.

---

## 🎯 What is ACID?

ACID is a set of four properties that guarantee reliable processing of database transactions. Even when the system crashes, two users act at the same time, or a query fails halfway, an ACID-compliant database keeps your data in a valid, predictable state.

**Formalized by:** Jim Gray, Theo Härder (1980s)
**Used by:** PostgreSQL, MySQL (InnoDB), Oracle, SQL Server

---

## 🔵 A — Atomicity

> **All-or-nothing.** A transaction either fully completes or leaves no trace.

### 🏦 Example: Bank Transfer

Alice sends $100 to Bob. Two steps must happen together:
1. Subtract $100 from Alice's balance.
2. Add $100 to Bob's balance.

If step 1 succeeds but step 2 fails, Alice loses $100 out of thin air. Atomicity prevents this by rolling back the entire transaction on failure.

```sql
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 'alice';
  UPDATE accounts SET balance = balance + 100 WHERE id = 'bob';
COMMIT;
```

### 📌 Key Takeaways

- [ ] A transaction is a single logical unit — it can't be partially applied.
- [ ] `BEGIN ... COMMIT` marks the transaction boundaries.
- [ ] `ROLLBACK` (automatic or manual) undoes all changes.
- [ ] Atomicity is what makes multi-step operations like transfers, orders, and inventory updates safe.

### ⚙️ Mechanism

- Transaction log / write-ahead log (WAL)
- Undo logs
- Rollback mechanism

---

## 🟢 C — Consistency

> **Valid state to valid state.** A transaction must not break any database rules.

### 🔗 Example: Foreign Key Constraint

Every order must belong to an existing customer.

```sql
CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  customer_id INT NOT NULL,
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

If a transaction tries to insert an order for a non-existent customer, the database **refuses to commit** and rolls back.

### 📌 Key Takeaways

- [ ] Consistency = the database always obeys its rules.
- [ ] Constraints (PK, FK, UNIQUE, CHECK) are enforced at commit time.
- [ ] If a transaction would break a rule, it's rolled back.
- [ ] Consistency is broader than constraints — it includes business logic too.

### ⚙️ Mechanism

- Primary keys, foreign keys, unique constraints
- CHECK constraints
- Triggers
- Application-level invariants

---

## 🟡 I — Isolation

> **No interference.** Concurrent transactions behave as if they're the only ones running.

### 🎟️ Example: Concurrent Ticket Booking

A concert has 1 ticket left. Alice and Bob try to buy it at the same time.

**Without isolation:** both see 1 ticket, both buy it → oversold!
**With isolation:** one transaction blocks the other → only one user gets the ticket.

### 📌 Key Takeaways

- [ ] Isolation prevents concurrent transactions from corrupting each other's results.
- [ ] Higher isolation = safer but slower (more locking / conflict detection).
- [ ] PostgreSQL defaults to READ COMMITTED; use SERIALIZABLE for strict correctness.
- [ ] The ticket-booking example is the classic "lost update" problem.

### ⚙️ Isolation Levels (weakest → strongest)

| Level | Dirty Read | Non-repeatable | Phantom |
|-------|:----------:|:--------------:|:-------:|
| READ UNCOMMITTED | ✅ | ✅ | ✅ |
| READ COMMITTED | ❌ | ✅ | ✅ |
| REPEATABLE READ | ❌ | ❌ | ✅ |
| SERIALIZABLE | ❌ | ❌ | ❌ |

### ⚙️ Mechanism

- Row-level / table-level locks
- Multi-Version Concurrency Control (MVCC)
- Serializable Snapshot Isolation (SSI)

---

## 🔴 D — Durability

> **No take-backs.** Once committed, data survives crashes and power loss.

### 📝 Example: Write-Ahead Log (WAL)

1. Before applying a change, the DB writes it to a log file on disk.
2. Only after the log is safely on disk does the DB apply the change.
3. If the system crashes, on restart the DB **replays the log** to recover committed changes.

### 📌 Key Takeaways

- [ ] After `COMMIT`, data is safe from crashes.
- [ ] The write-ahead log is the core mechanism.
- [ ] Recovery replays the log on restart.
- [ ] For real-world safety, add backups and replication on top of durability.

### ⚙️ Mechanism

- Write-ahead log (WAL)
- Checkpoints
- Redo logs

---

## 📊 Summary Table

| Property | Question | Example | Mechanism |
|----------|----------|---------|-----------|
| **Atomicity** | All-or-nothing? | Bank transfer | Undo logs, rollback |
| **Consistency** | Rules hold? | Foreign keys | Constraints, triggers |
| **Isolation** | No interference? | Ticket booking | Locks, MVCC |
| **Durability** | Survives crash? | Write-ahead log | WAL, checkpoints |

---

## 🧠 Mental Model: The Bank Vault

Imagine a bank vault that only opens when all four locks click:

- 🔒 **Atomicity** — the vault either opens fully or not at all.
- 🔒 **Consistency** — the vault's rules always hold.
- 🔒 **Isolation** — two people can't peek into each other's transactions.
- 🔒 **Durability** — once closed, contents are safe even if the building burns down.

---

## 📅 Review Session

**When:** 2026-09-25 at 10:00 AM UTC
**Duration:** 30 minutes
**Reminder:** 10 minutes before

See [review-checklist.md](review-checklist.md) for the full checklist and an importable `.ics` calendar file.

---

## 📚 References

- [PostgreSQL — Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [MySQL 8.0 — InnoDB Transaction Model](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-model.html)
- [GeeksforGeeks — ACID Properties in DBMS](https://www.geeksforgeeks.org/acid-properties-in-dbms/)
- [YouTube — ACID Properties Explained](https://www.youtube.com/watch?v=5FkY0uYcG6I)

Full list in [references.md](references.md).
