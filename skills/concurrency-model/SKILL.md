---
name: concurrency-model
description: Concurrency risk assessment, Virtual Thread-safe locking, and context propagation
metadata:
  category: architecture
  tags: [concurrency, virtual-threads, thread-safety, contention, locking, java-21]
user-invocable: false
---

# Concurrency Model

## When to Use

- During architecture design to assess concurrency risks per component
- When designing shared state access patterns
- When evaluating Virtual Thread compatibility for new components
- When identifying contention points that limit throughput
- When protecting shared mutable state in virtual thread environments
- When choosing context propagation strategy (ThreadLocal vs ScopedValue)

## Rules

- Default to no shared mutable state -- if state must be shared, justify it
- Never use `synchronized` blocks -- use `ReentrantLock` for Virtual Thread compatibility
- Connection pool sizes 10-40 for Virtual Thread environments (not 100+)
- Identify contention points early -- they become bottlenecks under load
- Distinguish read contention from write contention
- **Java 21-24**: Use `ThreadLocal` carefully (always call `remove()` in finally block)
- **Java 25+**: Use `ScopedValue` instead of `ThreadLocal` for context propagation
- Use blocking I/O calls freely in virtual threads (they excel here)
- Keep `synchronized` only for CPU-bound or very short critical sections (< 1 microsecond)

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

### Virtual Thread-Safe Locking

Bad - Pins virtual thread to platform thread:

```java
private final Object lock = new Object();
public void criticalSection() {
    synchronized (lock) {
        // Virtual thread PINNED to platform thread
    }
}
```

Good - Virtual thread friendly:

```java
import java.util.concurrent.locks.ReentrantLock;
private final ReentrantLock lock = new ReentrantLock();
public void criticalSection() {
    lock.lock();
    try {
        // Virtual thread can be unmounted during blocking
    } finally {
        lock.unlock();
    }
}
```

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

### Context Propagation

#### Java 21-24: ThreadLocal (with caution)

```java
// Always remove in finally to prevent memory leaks
private static final ThreadLocal<String> CORRELATION_ID = new ThreadLocal<>();

public void handleRequest(String id) {
    CORRELATION_ID.set(id);
    try {
        process();
    } finally {
        CORRELATION_ID.remove(); // CRITICAL: always remove!
    }
}
```

#### Java 25+: ScopedValue (recommended)

```java
// Immutable, automatically inherited by child virtual threads
// No memory leak, bounded lifetime
private static final ScopedValue<String> CORRELATION_ID = ScopedValue.newInstance();

public void handleRequest(String id) {
    ScopedValue.runWhere(CORRELATION_ID, id, () -> {
        processAsync(); // ScopedValue automatically inherited!
    });
}

// Access anywhere in the call chain
public void processAsync() {
    String id = CORRELATION_ID.get(); // Works in virtual threads
}
```

| Aspect      | ThreadLocal (Java 21-24) | ScopedValue (Java 25+) |
| ----------- | ------------------------ | ---------------------- |
| Inheritance | Manual                   | Automatic to child VTs |
| Memory      | Risk of leaks            | Bounded by scope       |
| Mutability  | Mutable                  | Immutable              |
| Cleanup     | Manual `remove()`        | Automatic              |

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

| Strategy            | Use When                                   | Trade-off                       |
| ------------------- | ------------------------------------------ | ------------------------------- |
| Partitioned locking | High write contention on identifiable keys | Memory for lock objects         |
| Optimistic locking  | Low conflict rate, DB-backed state         | Retry cost on conflict          |
| Lock-free (CAS)     | Simple atomic operations                   | Complex for multi-field updates |

Use skill: `data-consistency-modeling` for CQRS, event sourcing, and consistency strategy decisions.

## Avoid

- Using `synchronized` in any new code (Virtual Thread incompatible)
- `ThreadLocal` without `remove()` in finally block (Java 21-24)
- `ThreadLocal` when `ScopedValue` is available (Java 25+)
- CPU-bound work on virtual threads (use platform threads)
- Assuming low contention without traffic analysis
- Global locks when partitioned locks suffice
- Blocking I/O while holding locks
- Ignoring connection pool sizing for Virtual Thread environments
- Shared mutable state without documented justification
