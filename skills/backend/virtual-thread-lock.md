---
name: virtual-thread-lock
description: Virtual Thread-safe locking with ReentrantLock
category: backend
tags: [concurrency, virtual-threads, java-21, performance]
---

# Virtual Thread-Safe Locking

## When to Use

- Protecting shared mutable state in virtual thread environments
- Using Java 21+ with `spring.threads.virtual.enabled=true`
- Need fine-grained control (tryLock, timed locks)

## Rules

- Use `ReentrantLock` instead of `synchronized` for virtual threads
- Virtual threads are pinned (blocked) by `synchronized`, defeating their purpose
- Connection pool sizing: 10-40 connections (not 100+ like platform threads)
- **Java 21-24**: Use `ThreadLocal` carefully (always call `remove()` in finally block)
- **Java 25+**: Use `ScopedValue` instead of `ThreadLocal` for context propagation
- `ThreadLocal` causes memory issues with millions of virtual threads; `ScopedValue` (stable in Java 25) solves this
- Use blocking I/O calls freely in virtual threads (they excel here)
- Keep `synchronized` for CPU-bound or very short critical sections (< 1μs)

## Pattern

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

## Context Propagation Pattern

### Java 21-24: ThreadLocal (with caution)

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

### Java 25+: ScopedValue (recommended)

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

## Avoid

- Using `synchronized` with virtual threads
- `ThreadLocal` without `remove()` in finally block (Java 21-24)
- `ThreadLocal` when `ScopedValue` is available (Java 25+)
- CPU-bound work on virtual threads (use platform threads)
