---
name: api-guidelines
description: API consistency, compatibility, documentation standards, and Spring Boot REST endpoint design
metadata:
  category: governance
  tags: [api, governance, standards, documentation, rest, api-design, http, spring]
user-invocable: false
---

# API Guidelines

## When to Use

- Defining organizational API standards across multiple services
- Ensuring API consistency and contract documentation
- Establishing deprecation and versioning policy
- Designing REST endpoints and API contracts
- Structuring request/response objects
- Implementing pagination and versioning

## Rules

- Consistent naming conventions across all services
- Standardized error response format across all services
- Backward compatibility by default
- Deprecation before removal for API changes
- OpenAPI/Swagger documentation mandatory for all public APIs
- Include example request and response in docs
- Each API must have clear ownership
- Version only when breaking changes are unavoidable
- Use resource-based URI naming with plural nouns
- APIs must be stateless
- One responsibility per endpoint
- Use correct HTTP methods and proper status codes
- Never expose entity, always use DTO
- Validate all inputs
- Pagination mandatory for collection endpoints (support page, size, sort)
- Avoid `ResponseEntity` unless returning different status codes, response types, or headers within same method - `@RestController` + `@ResponseStatus` is cleaner

## Pattern

### Cross-Service Consistency

Every API must document:

1. **Ownership** -- which team owns and maintains this API
2. **Contract** -- OpenAPI spec with example request/response
3. **Deprecation policy** -- minimum notice period before removal
4. **Error format** -- consistent error response shape across all services

### Good: Documented, owned API

```java
@GetMapping("/users/{id}")
@Operation(summary = "Get user by ID",
    description = "Retrieves user details",
    tags = "users")
@ApiResponse(responseCode = "200", description = "User found")
@ApiResponse(responseCode = "404", description = "User not found")
public UserDTO getUser(@PathVariable Long id) {
    return userService.getUser(id);
}
```

### Bad: Undocumented, no ownership

```java
@GetMapping("/get_user/{userId}")
public User getuser(@PathVariable Long userId) {
    return userRepository.findById(userId).orElseThrow();
}
// No OpenAPI docs, unclear ownership
```

### Spring Boot REST Endpoint Design

Bad - Action-based URIs, exposing entity:

```java
@PostMapping("/getUser/{id}")
public User getUser(@PathVariable Long id) {
    return userRepository.findById(id).orElseThrow();
}
```

Good - Resource-based, DTO response, no ResponseEntity needed:

```java
@GetMapping("/users/{id}")
public UserDTO getUser(@PathVariable Long id) {
    return userService.getUser(id);
}

@PostMapping("/users")
@ResponseStatus(HttpStatus.CREATED)
public UserDTO create(@Valid @RequestBody CreateUserRequest request) {
    return userService.create(request);
}
```

ResponseEntity OK - Different outcomes require different responses:

```java
@GetMapping("/users/{id}")
public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
    return userService.findById(id)
        .map(ResponseEntity::ok)
        .orElse(ResponseEntity.notFound().build());
}
```

## Avoid

- Inconsistent endpoint naming across services
- Missing or outdated API documentation
- Breaking changes without versioning and deprecation notice
- Unclear API ownership
- Verb-based endpoints (use HTTP methods instead)
- Returning 200 for errors
- Exposing entity objects directly
- Missing pagination on collections
- Unnecessary `ResponseEntity` when `@ResponseStatus` suffices
