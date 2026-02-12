---
name: resiliency
description: Timeout, retry, circuit breaker, fallback patterns, and resilient REST client integration
metadata:
  category: ops
  tags: [resilience, timeouts, retries, circuit-breaker, rest, integration, clients]
user-invocable: false
---

# Resiliency

## When to Use

- Building applications that handle failures gracefully
- Protecting against cascading failures
- Managing external service dependencies
- Integrating with external REST APIs
- Calling remote services from backend
- Building resilient inter-service communication

## Rules

- Mandatory timeout for all external calls
- Retry only on transient failures (not 4xx)
- Retry duration < timeout to prevent cascading
- Use circuit breaker to protect critical dependencies
- Fail fast when service is unhealthy
- Implement graceful degradation with fallback responses
- Isolate resource usage with bulkhead pattern
- Use typed client (WebClient/RestClient)
- Map remote errors explicitly -- never propagate raw HTTP errors
- Minimize payload size to avoid chatty calls
- Retry only on safe operations (GET, PUT, DELETE)

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

### REST Client Integration

Bad - No timeout, raw error propagation:

```java
RestTemplate template = new RestTemplate();
User user = template.getForObject(
    "http://user-service/users/{id}",
    User.class, id
); // No timeout, throws raw exception
```

Good - Typed client, timeout, error mapping:

```java
@Service
public class UserServiceClient {
    public Optional<UserDTO> getUser(Long id) {
        return userWebClient.get()
            .uri("/users/{id}", id)
            .retrieve()
            .bodyToMono(UserDTO.class)
            .timeout(Duration.ofSeconds(5))
            .onErrorMap(this::mapError)
            .blockOptional();
    }

    private RuntimeException mapError(Throwable e) {
        return new UserServiceException("Failed to fetch user", e);
    }
}
```

## Avoid

- Missing timeouts on external calls
- Retry on permanent failures (4xx)
- No circuit breakers for critical dependencies
- Unbounded retry attempts
- Raw HTTP exception propagation
- Chatty, oversized payloads
