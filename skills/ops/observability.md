---
name: observability
description: Structured logging, metrics, and distributed tracing
category: ops
tags: [logging, metrics, tracing, monitoring]
---

# Observability

## When to Use

- Debugging production issues
- Monitoring application health and performance
- Understanding distributed system behavior

## Rules

- Use structured logs (JSON) with consistent fields
- Include correlation ID in all logs for request tracing
- Never log sensitive data (passwords, tokens, PII)
- Implement RED metrics for APIs: Rate, Errors, Duration
- Track key business metrics
- Enable distributed tracing and propagate trace context
- Alert on symptoms, not causes

## Pattern

❌ Bad - Unstructured logging, missing context:

```java
logger.info("User processed: " + userId);
logger.error("Database error: " + e.getMessage());
```

✅ Good - Structured with context:

```java
logger.info("User processed", Map.of(
    "userId", user.getId(),
    "correlationId", MDC.get("correlationId"),
    "duration", stopwatch.elapsed(TimeUnit.MS)
));
logger.error("Database operation failed",
    Map.of("operation", "findUser", "error", e.getClass().getName()),
    e);
```

## Avoid

- Logging sensitive data
- Unstructured logs without context
- Logging too much or too little information
- Missing correlation IDs
