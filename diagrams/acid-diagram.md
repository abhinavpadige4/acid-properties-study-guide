# ACID Visual Diagram

Rendered with [Mermaid](https://mermaid.js.org/). GitHub renders Mermaid natively in Markdown.

## The Four Properties at a Glance

```mermaid
mindmap
  root((ACID))
    Atomicity
      All-or-nothing
      Bank transfer
      Rollback on failure
    Consistency
      Valid state to valid state
      Foreign keys
      CHECK constraints
    Isolation
      No interference
      Ticket booking
      Locks and MVCC
    Durability
      Survives crashes
      Write-ahead log
      Redo on recovery
```

## Transaction Lifecycle

```mermaid
sequenceDiagram
    participant Client
    participant DB
    participant WAL as Write-Ahead Log
    participant Disk

    Client->>DB: BEGIN
    DB->>WAL: write start record
    Client->>DB: UPDATE ...
    DB->>WAL: write change record (flush)
    DB->>Disk: apply change (later)
    Client->>DB: COMMIT
    DB->>WAL: write COMMIT record (flush)
    DB-->>Client: OK (durable!)
```

## Isolation Levels (Trade-off)

```mermaid
flowchart LR
    A[READ UNCOMMITTED] -->|stricter| B[READ COMMITTED]
    B -->|stricter| C[REPEATABLE READ]
    C -->|stricter| D[SERIALIZABLE]

    A -.- A1[fastest, most anomalies]
    D -.- D1[safest, slowest]
```

## How Each Property Is Enforced

```mermaid
flowchart TB
    T[Transaction] --> A[Atomicity]
    T --> C[Consistency]
    T --> I[Isolation]
    T --> D[Durability]

    A --> A1[Undo logs]
    A --> A2[Rollback]

    C --> C1[Constraints]
    C --> C2[Triggers]
    C --> C3[CHECK / FK / PK]

    I --> I1[Row locks]
    I --> I2[MVCC]
    I --> I3[SSI]

    D --> D1[Write-ahead log]
    D --> D2[Checkpoints]
    D --> D3[Redo on recovery]
```

## The Bank Vault Analogy

```mermaid
flowchart LR
    V((Bank Vault))
    V --> L1[🔒 Atomicity<br/>all-or-nothing]
    V --> L2[🔒 Consistency<br/>rules hold]
    V --> L3[🔒 Isolation<br/>no peeking]
    V --> L4[🔒 Durability<br/>crash-proof]
```
