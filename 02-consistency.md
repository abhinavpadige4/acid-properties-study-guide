# C — Consistency

> **Definition:** A transaction must move the database from one **valid state** to another **valid state**. All rules — constraints, triggers, foreign keys, and application-level invariants — must hold before and after the transaction.

Consistency is the "big picture" property: it says the database must always obey its own rules.

## The Foreign Key Constraint Example

Consider an e-commerce database:

```sql
CREATE TABLE customers (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL
);

CREATE TABLE orders (
  id SERIAL PRIMARY KEY,
  customer_id INT NOT NULL,
  total NUMERIC,
  FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

The **rule** is: every order must belong to an existing customer.

### A Consistent Transaction

```sql
BEGIN;
  INSERT INTO customers (name) VALUES ('Carol');
  INSERT INTO orders (customer_id, total) VALUES (3, 49.99);
COMMIT;
```

Both rows are inserted, and the foreign key rule still holds. ✅

### An Inconsistent Transaction (rejected)

```sql
BEGIN;
  INSERT INTO orders (customer_id, total) VALUES (999, 49.99);
COMMIT;
-- ERROR: foreign key violation — customer 999 does not exist
```

The database **refuses** to commit because it would break the rule. The transaction is rolled back, and the database stays consistent.

## Other Consistency Rules

- **NOT NULL** — required fields can't be empty.
- **UNIQUE** — no duplicate values in a unique column.
- **CHECK** — custom conditions, e.g., `CHECK (balance >= 0)`.
- **Triggers** — code that runs on insert/update to enforce business rules.
- **Application invariants** — e.g., "total money in the system never changes."

## Consistency vs Atomicity

- **Atomicity** says: "the transaction is all-or-nothing."
- **Consistency** says: "the transaction must not break any rules."

They work together: atomicity provides the mechanism (rollback), and consistency defines the goal (valid state).

## Key Takeaways

- ✅ Consistency = the database always obeys its rules.
- ✅ Constraints (PK, FK, UNIQUE, CHECK) are enforced at commit time.
- ✅ If a transaction would break a rule, it's rolled back.
- ✅ Consistency is broader than constraints — it includes business logic too.

## Try It Yourself

```sql
-- This will fail: CHECK constraint violation
CREATE TABLE accounts (
  id INT PRIMARY KEY,
  balance NUMERIC CHECK (balance >= 0)
);

INSERT INTO accounts VALUES (1, -50);  -- ERROR
```

---

**Next:** [Isolation →](03-isolation.md)
