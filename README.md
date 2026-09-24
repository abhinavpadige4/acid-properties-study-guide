# ACID Properties in Databases — A Beginner's Study Guide

> **Goal:** Understand the four guarantees that make relational databases trustworthy, using simple, real-world examples.

ACID is a set of four properties — **Atomicity, Consistency, Isolation, Durability** — that guarantee reliable processing of database transactions. Even when the system crashes, two users act at the same time, or a query fails halfway, an ACID-compliant database keeps your data in a valid, predictable state.

The concept was formalized in the 1980s by researchers including **Jim Gray** and **Theo Härder**, and it remains the gold standard for relational DBMSs like PostgreSQL, MySQL (InnoDB), Oracle, and SQL Server.

---

## Table of Contents

1. [What is a Transaction?](#what-is-a-transaction)
2. [The Four ACID Properties](#the-four-acid-properties)
   - [A — Atomicity](01-atomicity.md)
   - [C — Consistency](02-consistency.md)
   - [I — Isolation](03-isolation.md)
   - [D — Durability](04-durability.md)
3. [Summary Table](summary-table.md)
4. [Visual Diagram (Mermaid)](diagrams/acid-diagram.md)
5. [ACID vs BASE vs CAP](acid-vs-base-vs-cap.md)
6. [Review Checklist & Calendar Event](review-checklist.md)
7. [References](references.md)

---

## What is a Transaction?

A **transaction** is a logical unit of work — one or more database operations that must succeed or fail *together*.

```sql
-- A bank transfer is a single transaction
BEGIN;
  UPDATE accounts SET balance = balance - 100 WHERE id = 'alice';
  UPDATE accounts SET balance = balance + 100 WHERE id = 'bob';
COMMIT;
```

If either `UPDATE` fails, the whole transfer is rolled back — Alice's money is never lost.

---

## The Four ACID Properties

| Letter | Property | One-line definition |
|--------|----------|---------------------|
| **A** | **Atomicity** | All-or-nothing: a transaction either fully completes or leaves no trace. |
| **C** | **Consistency** | A transaction moves the database from one valid state to another valid state. |
| **I** | **Isolation** | Concurrent transactions don't interfere with each other. |
| **D** | **Durability** | Once committed, data survives crashes and power loss. |

Each property has its own deep dive with a concrete example:

- **[Atomicity →](01-atomicity.md)** — Bank transfer example
- **[Consistency →](02-consistency.md)** — Foreign key constraint example
- **[Isolation →](03-isolation.md)** — Concurrent ticket booking example
- **[Durability →](04-durability.md)** — Write-ahead log example

---

## Quick Mental Model

Imagine a **bank vault** that only opens when all four locks click:

- 🔒 **Atomicity** — the vault either opens fully or not at all (no half-open doors).
- 🔒 **Consistency** — the vault's rules (e.g., "total money in the system is conserved") always hold.
- 🔒 **Isolation** — two people can't peek into each other's transactions while they're in progress.
- 🔒 **Durability** — once the vault closes, the contents are safe even if the building burns down.

---

## Next Steps

1. Read each property's deep-dive page.
2. Skim the [summary table](summary-table.md) and [diagram](diagrams/acid-diagram.md).
3. Check off the [review checklist](review-checklist.md) before your review session.
4. See [references](references.md) for authoritative sources.

---

*Study guide prepared for a beginner audience. All examples are simplified for clarity.*
