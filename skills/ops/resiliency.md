---
name: resiliency
description: Timeout, retry, circuit breaker, and fallback patterns
category: ops
tags: [resilience, timeouts, retries, circuit-breaker]
---

# Resiliency

## When to Use

- Building applications that handle failures gracefully
- Protecting against cascading failures
- Managing external service dependencies

## Rules

- Mandatory timeout for all external calls
- Retry only on transient failures (not 4xx)
- Retry duration < timeout to prevent cascading
- Use circuit breaker to protect critical dependencies
- Fail fast when service is unhealthy
- Implement graceful degradation with fallback responses
- Isolate resource usage with bulkhead pattern

## Pattern

Bad - No timeout, unlimited retries:

```java
RestTemplate template = new RestTemplate();
String response = template.getForObject("http://external-api/data", String.class);
```

Good - Timeout, retry, circuit breaker:

```java
@CircuitBreaker(name = "externalApi", fallbackMethod = "fallback")
@Retry(name = "externalApi", fallbackMethod = "fallback")
@Timeout(name = "externalApi")
public String fetchData() {
    return restTemplate.getForObject(
        "http://external-api/data",
        String.class
    );
}

public String fallback(Exception e) {
    return "{}"; // Default response
}
```

## Avoid

- Missing timeouts on external calls
- Retry on permanent failures (4xx)
- No circuit breakers for critical dependencies
- Unbounded retry attempts
