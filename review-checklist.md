# Review Checklist & Calendar Event

Use this checklist to confirm you've understood ACID before your review session.

## ✅ Key Takeaways (check each one)

- [ ] **Atomicity** — A transaction is all-or-nothing. If any step fails, the whole transaction is rolled back.
- [ ] **Consistency** — A transaction moves the database from one valid state to another valid state. Constraints and rules are always enforced.
- [ ] **Isolation** — Concurrent transactions don't interfere with each other. Isolation levels trade off safety for performance.
- [ ] **Durability** — Once committed, data survives crashes. The write-ahead log is the core mechanism.
- [ ] **Transaction boundaries** — `BEGIN ... COMMIT` (or `ROLLBACK`) defines a transaction.
- [ ] **Isolation levels** — READ UNCOMMITTED < READ COMMITTED < REPEATABLE READ < SERIALIZABLE.
- [ ] **ACID vs BASE** — ACID = strong correctness; BASE = availability first.
- [ ] **CAP theorem** — A distributed system can only guarantee 2 of 3: Consistency, Availability, Partition tolerance.

## 📅 Calendar Event

**Title:** Review ACID Properties Study Guide

**Date & Time:** 2026-09-25 at 10:00 AM UTC

**Duration:** 30 minutes

**Reminder:** 10 minutes before

**Description:**
```
Review the ACID properties study guide:
- https://github.com/abhinavpadige4/acid-properties-study-guide

Agenda:
1. Re-read the summary table (5 min)
2. Walk through each property's example (15 min)
3. Answer the interview questions in summary-table.md (10 min)
```

### iCalendar (.ics) — import into any calendar app

```ics
BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//ACID Study Guide//Review//EN
BEGIN:VEVENT
DTSTART:20260925T100000Z
DTEND:20260925T103000Z
DTSTAMP:20260924T000000Z
UID:acid-review-20260925@study-guide
SUMMARY:Review ACID Properties Study Guide
DESCRIPTION:Review the ACID properties study guide at https://github.com/abhinavpadige4/acid-properties-study-guide\n\nAgenda:\n1. Re-read the summary table (5 min)\n2. Walk through each property's example (15 min)\n3. Answer the interview questions in summary-table.md (10 min)
LOCATION:Online
BEGIN:VALARM
TRIGGER:-PT10M
ACTION:DISPLAY
DESCRIPTION:Review ACID Properties in 10 minutes
END:VALARM
END:VEVENT
END:VCALENDAR
```

Save the block above as `acid-review.ics` and import it into Google Calendar, Apple Calendar, Outlook, or any other calendar app.

## 🎯 Self-Test Questions

1. What happens if a bank transfer crashes after Alice's balance is updated but before Bob's?
2. Why does a foreign key constraint enforce consistency?
3. What's the difference between a dirty read and a phantom read?
4. Why is the write-ahead log written *before* the data file?
5. Can a database be both ACID and BASE? Explain.

<details>
<summary><strong>Answers</strong></summary>

1. Atomicity rolls back the transaction — Alice's balance is restored, and Bob is never credited.
2. A foreign key ensures referential integrity: every order must point to an existing customer. If a transaction would violate this, it's rolled back.
3. A dirty read sees uncommitted data from another transaction. A phantom read sees new rows appear (or disappear) between two reads of the same range.
4. Because if the system crashes, the log is the source of truth for recovery. If we wrote the data file first and crashed before the log, we'd have no record of what happened.
5. Not strictly — they're opposite philosophies. But a system can offer ACID for some operations and BASE for others (e.g., a database with tunable isolation levels).

</details>
