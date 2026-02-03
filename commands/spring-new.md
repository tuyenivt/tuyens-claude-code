---
description: Create new Spring Boot 4 API endpoint with controller, service, repository
model: claude-opus-4-5
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
@RequiredArgsConstructor  // Lombok for DI
@Slf4j                    // Lombok for logging
public class ResourceController {
    private final ResourceService service;

    @GetMapping("/{id}")
    public ResourceResponse getById(@PathVariable Long id) {
        // No ResponseEntity needed - @RestController handles serialization
        return service.getById(id);
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)  // Use @ResponseStatus instead of ResponseEntity
    public ResourceResponse create(@Valid @RequestBody CreateRequest request) {
        var response = service.create(request);  // Use var when type is obvious
        log.info("Created resource: {}", response.id());
        return response;
    }

    // ResponseEntity OK when returning different status codes in same method
    @GetMapping("/{id}/optional")
    public ResponseEntity<ResourceResponse> findById(@PathVariable Long id) {
        return service.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
}

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
@Slf4j
public class ResourceService {
    private final ResourceRepository repository;

    public ResourceResponse getById(Long id) {
        var entity = repository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException(id));
        return toResponse(entity);
    }

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

## Key Skills Reference

For detailed patterns, reference these skills:

- Use skill: `spring-api` for REST API design principles
- Use skill: `virtual-thread-lock` for Virtual Thread-safe locking patterns
- Use skill: `jpa-performance` for optimal data access patterns
- Use skill: `exception-handling` for centralized error handling
- Use skill: `transaction` for transaction management

**Virtual Threads Configuration:**

- Enable in `application.yml`: `spring.threads.virtual.enabled: true`
- Connection pool: 10-40 connections (not 100+)
- Virtual threads excel at I/O-bound operations

## Checklist

- [ ] Constructor injection (no @Autowired fields) - use `@RequiredArgsConstructor` if Lombok available
- [ ] Validation on request DTOs (Jakarta Validation)
- [ ] Proper HTTP status codes - use `@ResponseStatus` instead of `ResponseEntity` when possible
- [ ] Pagination for list endpoints
- [ ] Entity not exposed in responses (use DTOs)
- [ ] No `synchronized` blocks (Virtual Thread compatible)
- [ ] Records for immutable DTOs
- [ ] Use `var` for local variables when type is obvious
- [ ] Use `@Slf4j` for logging if Lombok available
