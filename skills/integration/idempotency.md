---
name: idempotency
description: Idempotency key pattern for safe retries
category: integration
tags: [idempotency, retries, integration, safety]
---

# Idempotency

## When to Use

- POST operations with side effects
- Retryable operations in unreliable networks
- Event consumption and message processing

## Rules

- Idempotent operations must return same result on repeated calls
- Store idempotency state safely (database)
- Use idempotency keys to track processed requests
- Expire idempotency keys with appropriate TTL
- Support natural business keys when applicable
- Use deduplication table for at-most-once semantics

## Pattern

❌ Bad - Retries cause duplicate operations:

```java
@PostMapping("/orders")
public Order createOrder(OrderRequest req) {
    return orderRepository.save(new Order(req));
    // Retry of same request = duplicate order
}
```

✅ Good - Idempotency key ensures safety:

```java
@PostMapping("/orders")
public Order createOrder(
    @RequestHeader("Idempotency-Key") String key,
    OrderRequest req) {
    return idempotencyService.executeOnce(key, () ->
        orderRepository.save(new Order(req))
    );
}
```

## Avoid

- Relying on client retries without server-side protection
- Ignoring idempotency for POST operations
- Missing natural business keys where applicable
- Infinite idempotency key retention
