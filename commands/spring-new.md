---
description: Create new Spring Boot 4 API endpoint with controller, service, repository
model: claude-sonnet-4-5
---

Create a new Spring Boot 4 REST API endpoint following modern best practices.

## Requirements

$ARGUMENTS

## Implementation

Generate complete, production-ready code for Spring Boot 4 / Java 21+:

1. **Controller** - REST endpoints with `@Valid`, proper status codes
2. **Service** - Business logic with `@Transactional`
3. **Repository** - Spring Data JPA with custom queries if needed
4. **DTOs** - Request/Response records with validation
5. **Entity** - JPA entity if needed

## Modern Patterns (Java 21+ / Spring Boot 4)

```java
@RestController
@RequestMapping("/api/v1/{resources}")
@RequiredArgsConstructor
public class ResourceController {
    private final ResourceService service;

    @GetMapping("/{id}")
    public ResponseEntity<ResourceResponse> getById(@PathVariable Long id) {
        return service.findById(id)
            .map(ResponseEntity::ok)
            .orElseThrow(() -> new ResourceNotFoundException(id));
    }
}

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class ResourceService {
    private final ResourceRepository repository;

    // For thread-safety, use skill: virtual-thread-lock
}

// Java 21+ Records for DTOs
public record CreateRequest(
    @NotBlank String name,
    @NotNull @Positive Long amount
) {}

public record ResourceResponse(
    Long id,
    String name,
    Instant createdAt
) {}
```

## Virtual Threads Considerations

For Virtual Thread-safe locking patterns, use skill: `virtual-thread-lock`

Key points:

- Enable in `application.yml`: `spring.threads.virtual.enabled: true`
- Connection pool: 10-40 connections (not 100+)
- Virtual threads excel at I/O-bound operations

## Checklist

- [ ] Constructor injection (no @Autowired fields)
- [ ] Validation on request DTOs (Jakarta Validation)
- [ ] Proper HTTP status codes (200, 201, 204, 400, 404)
- [ ] Pagination for list endpoints
- [ ] Entity not exposed in responses (use DTOs)
- [ ] No `synchronized` blocks (Virtual Thread compatible)
- [ ] Records for immutable DTOs
