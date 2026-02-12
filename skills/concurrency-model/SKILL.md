---
name: concurrency-model
description: Concurrency risk assessment and Virtual Thread compatibility
metadata:
  category: architecture
  tags: [concurrency, virtual-threads, thread-safety, contention, locking]
user-invocable: false
---

# Concurrency Model

## When to Use

- During architecture design to assess concurrency risks per component
- When designing shared state access patterns
- When evaluating Virtual Thread compatibility for new components
- When identifying contention points that limit throughput

## Rules

- Default to no shared mutable state -- if state must be shared, justify it
- Never use `synchronized` blocks -- use `ReentrantLock` for Virtual Thread compatibility
- Connection pool sizes 10-40 for Virtual Thread environments (not 100+)
- Identify contention points early -- they become bottlenecks under load
- Distinguish read contention from write contention

## Pattern

### Concurrency Risk Assessment

For each component that manages state, evaluate:

1. **Shared state** -- what mutable state is accessed by multiple threads
2. **Access pattern** -- read-heavy, write-heavy, or mixed
3. **Contention profile** -- how often do concurrent accesses conflict
4. **Lock strategy** -- how conflicts are resolved (lock, CAS, queue, partition)
5. **Virtual Thread safety** -- does the locking mechanism support Virtual Threads

### Risk Classification

| Risk Level | Criteria                                                    |
| ---------- | ----------------------------------------------------------- |
| Low        | No shared mutable state, or read-only shared state          |
| Medium     | Shared state with low contention, proper locking in place   |
| High       | Shared state with high contention, or blocking I/O in lock  |
| Critical   | Shared state with synchronized blocks under Virtual Threads |

### Virtual Thread Compatibility

Patterns that cause Virtual Thread pinning (avoid):

- `synchronized` blocks or methods
- Native methods holding monitors
- Blocking I/O inside `synchronized` scope

Safe alternatives:

- `ReentrantLock` for mutual exclusion
- `StampedLock` for read-write scenarios
- `ConcurrentHashMap` for concurrent map access
- Atomic classes (`AtomicInteger`, `AtomicReference`) for simple state
- Immutable objects and records for shared data

### Good: Specific concurrency assessment

```
Component: InventoryService
Shared State: stock count per SKU (in-memory cache + DB)
Access Pattern: read-heavy (100:1 read:write), write bursts during flash sales
Contention: High during flash sales (many concurrent decrements on same SKU)
Lock Strategy: ReentrantLock per SKU (partitioned locking), DB optimistic locking for persistence
Virtual Thread Safety: Yes (ReentrantLock, no synchronized)
Risk: Medium (partitioned locking limits blast radius; DB optimistic lock handles write conflicts)
```

### Bad: No concurrency consideration

```
InventoryService updates stock counts. It uses a database.
```

### Contention Reduction Strategies

| Strategy                | Use When                                   | Trade-off                       |
| ----------------------- | ------------------------------------------ | ------------------------------- |
| Partitioned locking     | High write contention on identifiable keys | Memory for lock objects         |
| Optimistic locking      | Low conflict rate, DB-backed state         | Retry cost on conflict          |
| CQRS (read/write split) | Read-heavy, staleness acceptable           | Eventual consistency            |
| Event sourcing          | Audit trail needed, append-only natural    | Query complexity                |
| Lock-free (CAS)         | Simple atomic operations                   | Complex for multi-field updates |

## Avoid

- Using `synchronized` in any new code (Virtual Thread incompatible)
- Assuming low contention without traffic analysis
- Global locks when partitioned locks suffice
- Blocking I/O while holding locks
- Ignoring connection pool sizing for Virtual Thread environments
- Shared mutable state without documented justification
