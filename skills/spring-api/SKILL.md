---
name: spring-api
description: REST API design principles and HTTP best practices
metadata:
  category: backend
  tags: [rest, api-design, http, spring]
---

# Spring REST API

## When to Use

- Designing REST endpoints and API contracts
- Structuring request/response objects
- Implementing pagination and versioning

## Rules

- Use resource-based URI naming with plural nouns
- APIs must be stateless
- One responsibility per endpoint
- Use correct HTTP methods and proper status codes
- Never expose entity, always use DTO
- Validate all inputs
- Pagination mandatory for collection endpoints (support page, size, sort)
- Prefer backward-compatible evolution, version only if breaking
- Avoid `ResponseEntity` unless returning different status codes, response types, or headers within same method - `@RestController` + `@ResponseStatus` is cleaner

## Pattern

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

- Verb-based endpoints (use HTTP methods instead)
- Returning 200 for errors
- Exposing entity objects directly
- Missing pagination on collections
- Unnecessary `ResponseEntity` when `@ResponseStatus` suffices
