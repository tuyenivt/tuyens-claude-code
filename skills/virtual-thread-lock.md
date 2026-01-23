---
name: virtual-thread-lock
description: Virtual Thread-safe locking pattern for Java 21+
category: pattern
tags: [java, virtual-threads, concurrency]
---

# Virtual Thread-Safe Locking Pattern

## Problem

Java 21+ Virtual Threads are pinned (blocked) when using `synchronized` blocks, defeating their purpose for I/O-bound operations.

## Solution

Use `ReentrantLock` instead of `synchronized` for Virtual Thread compatibility.

## Pattern

### ❌ Bad - Pins Virtual Thread

```java
private final Object lock = new Object();

public void criticalSection() {
    synchronized (lock) {
        // Critical section
        // This PINS the virtual thread to a platform thread
    }
}
```

### ✅ Good - Virtual Thread Friendly

```java
import java.util.concurrent.locks.ReentrantLock;

private final ReentrantLock lock = new ReentrantLock();

public void criticalSection() {
    lock.lock();
    try {
        // Critical section
        // Virtual thread can be unmounted during blocking
    } finally {
        lock.unlock();
    }
}
```

## Spring Boot Configuration

```yaml
# application.yml - Enable virtual threads
spring:
  threads:
    virtual:
      enabled: true
```

## Additional Considerations

- **Connection Pool Sizing**: 10-40 connections (not 100+ like platform threads)
- **Avoid ThreadLocal**: Can cause memory issues with millions of virtual threads
- **I/O Operations**: Virtual threads excel here, use blocking calls freely
- **CPU-Bound**: Still better on platform threads

## When to Use

✅ Use `ReentrantLock` when:

- Protecting shared mutable state
- Using virtual threads (`spring.threads.virtual.enabled=true`)
- Need fine-grained control (tryLock, timed locks)

❌ Keep `synchronized` when:

- Not using virtual threads
- Very short critical sections (< 1μs)
- Method-level synchronization for simplicity
