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
- Use `ScopedValue` instead of `ThreadLocal` for request context propagation
- `ThreadLocal` causes memory issues with millions of virtual threads and doesn't inherit across virtual thread boundaries
- Use blocking I/O calls freely in virtual threads (they excel here)
- Keep `synchronized` for CPU-bound or very short critical sections (< 1μs)

## Pattern

❌ Bad - Pins virtual thread to platform thread:

```java
private final Object lock = new Object();
public void criticalSection() {
    synchronized (lock) {
        // Virtual thread PINNED to platform thread
    }
}
```

✅ Good - Virtual thread friendly:

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

## ScopedValue Pattern

❌ Bad - ThreadLocal doesn't work well with virtual threads:

```java
// Memory leak with millions of virtual threads
// Values don't inherit across virtual thread boundaries
private static final ThreadLocal<String> correlationId = new ThreadLocal<>();

public void handleRequest(String id) {
    correlationId.set(id);
    try {
        processAsync(); // ThreadLocal won't propagate!
    } finally {
        correlationId.remove();
    }
}
```

✅ Good - ScopedValue for virtual threads (Java 21+):

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

## Avoid

- Using `synchronized` with virtual threads
- `ThreadLocal` for context propagation (use `ScopedValue` instead)
- CPU-bound work on virtual threads (use platform threads)
