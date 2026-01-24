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
- Avoid `ThreadLocal` to prevent memory issues with millions of virtual threads
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

## Avoid

- Using `synchronized` with virtual threads
- ThreadLocal for millions of virtual threads
- CPU-bound work on virtual threads (use platform threads)
