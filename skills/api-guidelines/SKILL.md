---
name: api-guidelines
description: API consistency, compatibility, and documentation standards
metadata:
  category: governance
  tags: [api, governance, standards, documentation]
---

# API Guidelines

## When to Use

- Defining organizational API standards
- Ensuring API consistency across services
- Documenting API contracts and expectations

## Rules

- Consistent naming conventions across all services
- Standardized error response format
- Backward compatibility by default
- Deprecation before removal for API changes
- OpenAPI/Swagger documentation mandatory
- Include example request and response in docs
- Each API must have clear ownership
- Version only when breaking changes are unavoidable

## Pattern

Bad - Inconsistent naming, missing docs:

```java
@GetMapping("/get_user/{userId}")
public User getuser(@PathVariable Long userId) {
    return userRepository.findById(userId).orElseThrow();
}
// No OpenAPI docs, unclear ownership
```

Good - Consistent, documented, owned:

```java
@GetMapping("/users/{id}")
@Operation(summary = "Get user by ID",
    description = "Retrieves user details",
    tags = "users")
@ApiResponse(responseCode = "200", description = "User found")
@ApiResponse(responseCode = "404", description = "User not found")
public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
    return ResponseEntity.ok(userService.getUser(id));
}
```

## Avoid

- Inconsistent endpoint naming
- Missing or outdated documentation
- Breaking changes without versioning
- Unclear API ownership
