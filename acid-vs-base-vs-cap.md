# ACID vs BASE vs CAP

Understanding where ACID fits in the broader landscape of distributed systems.

## ACID vs BASE

| | ACID | BASE |
|---|------|------|
| **Stands for** | Atomicity, Consistency, Isolation, Durability | Basically Available, Soft state, Eventual consistency |
| **Philosophy** | Strong guarantees, correctness first | Availability first, consistency later |
| **Typical systems** | PostgreSQL, MySQL (InnoDB), Oracle, SQL Server | Cassandra, DynamoDB, MongoDB (in some modes), Riak |
| **Trade-off** | Slower, less available under load | Faster, more available, but reads may be stale |
| **Best for** | Financial systems, inventory, anything where correctness is critical | Social feeds, caches, recommendation systems |

**BASE** is essentially the opposite of ACID: it accepts temporary inconsistency in exchange for higher availability and performance.

## The CAP Theorem

The **CAP theorem** (Eric Brewer, 2000) states that a distributed system can only guarantee **two of three** properties at the same time:

- **C — Consistency:** every read gets the most recent write.
- **A — Availability:** every request gets a response (no failures).
- **P — Partition tolerance:** the system keeps working despite network partitions.

Since network partitions are inevitable in distributed systems, you really choose between:

- **CP** — Consistency + Partition tolerance (e.g., ZooKeeper, etcd, Cassandra in strong mode). Sacrifices availability during partitions.
- **AP** — Availability + Partition tolerance (e.g., DynamoDB, Cassandra in eventual mode). Sacrifices consistency during partitions.

## How ACID Relates to CAP

- ACID is a **single-node** guarantee. A single PostgreSQL instance can be fully ACID.
- CAP is about **distributed** systems. When you replicate ACID across nodes, you start trading off.
- Modern systems like Spanner, CockroachDB, and YugabyteDB aim to give you **distributed ACID** by using consensus protocols (Paxos, Raft) — but they still make CAP trade-offs under partitions.

## When to Use What

| Use case | Recommended model |
|----------|-------------------|
| Bank transfers, payments | ACID (single-node or distributed ACID) |
| E-commerce inventory | ACID (with SERIALIZABLE isolation) |
| Social media feed | BASE (eventual consistency is fine) |
| Shopping cart | BASE (temporary inconsistency OK) |
| Leader election, config store | CP (consistency is critical) |
| Global user directory | AP (availability is critical) |

## Key Takeaways

- ✅ ACID = strong correctness guarantees.
- ✅ BASE = availability and performance over strict consistency.
- ✅ CAP = a fundamental trade-off in distributed systems.
- ✅ Modern distributed databases try to give you ACID across nodes, but CAP still applies.
