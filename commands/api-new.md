---
description: Create new Spring Boot API endpoint with controller, service, repository
model: claude-sonnet-4-5
---

Create a new Spring Boot REST API endpoint following best practices.

## Requirements

$ARGUMENTS

## Implementation

Generate complete, production-ready code:

1. **Controller** - REST endpoints with `@Valid`, proper status codes
2. **Service** - Business logic with `@Transactional`
3. **Repository** - Spring Data JPA with custom queries if needed
4. **DTOs** - Request/Response records with validation
5. **Entity** - JPA entity if needed

## Patterns

```java
@RestController
@RequestMapping("/api/v1/{resources}")
@RequiredArgsConstructor
public class ResourceController {
    private final ResourceService service;
}

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class ResourceService {
    private final ResourceRepository repository;
}

public record CreateRequest(@NotBlank String name) {}
public record Response(Long id, String name) {}
```

## Checklist

- Constructor injection (no @Autowired fields)
- Validation on request DTOs
- Proper HTTP status codes (200, 201, 204, 400, 404)
- Pagination for list endpoints
- Entity not exposed in responses
- TODO comments for business logic
