---
name: spring-api
description: REST API design principles and HTTP best practices
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

## Pattern

❌ Bad - Action-based URIs, exposing entity:

```java
@PostMapping("/getUser/{id}")
public User getUser(@PathVariable Long id) {
    return userRepository.findById(id).orElseThrow();
}
```

✅ Good - Resource-based, DTO response:

```java
@GetMapping("/users/{id}")
public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
    return ResponseEntity.ok(userService.getUser(id));
}
```

## Avoid

- Verb-based endpoints (use HTTP methods instead)
- Returning 200 for errors
- Exposing entity objects directly
- Missing pagination on collections
