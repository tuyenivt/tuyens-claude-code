---
name: rest-integration
description: REST client patterns and resilient integration
category: integration
tags: [rest, integration, resilience, clients]
---

# REST Integration

## When to Use

- Integrating with external REST APIs
- Calling remote services from backend
- Building resilient inter-service communication

## Rules

- Use typed client (WebClient/RestClient)
- Define timeout for all calls
- Always configure timeout (no defaults)
- Retry only on safe operations (GET, PUT, DELETE)
- Avoid infinite retries
- Map remote errors explicitly
- Never propagate raw HTTP errors
- Minimize payload size to avoid chatty calls
- Define fallback strategy for critical dependencies

## Pattern

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
- Raw HTTP exception propagation
- Retry on permanent failures (4xx)
- Chatty, oversized payloads
