---
name: coding-standards
description: Code style, structure, and maintainability guidelines
category: governance
tags: [code-quality, standards, structure, readability]
---

# Coding Standards

## When to Use

- Maintaining consistent code style across teams
- Ensuring codebase readability and maintainability
- Enforcing architectural patterns

## Rules

- Constructor injection only (no field or setter injection)
- Immutable DTO where possible
- Clear, descriptive naming for classes, methods, variables
- Follow Controller → Service → Repository layer structure
- No cross-layer access (direct repository calls in controllers)
- Keep methods small and focused
- Single responsibility per class
- Avoid god classes with multiple responsibilities
- No magic numbers, use named constants
- Use enums for fixed value sets

### Java Style

- Use `var` for local variables when type is obvious from right-hand side (constructors, literals, factory methods)
- Avoid `var` when type is unclear: null assignments, generic type inference (e.g., `TypeReference<>`), ambiguous method returns
- If project uses Lombok: prefer `@Slf4j` for logging, `@RequiredArgsConstructor` for dependency injection

## Pattern

❌ Bad - Mixed concerns, field injection, magic numbers:

```java
@RestController
public class OrderController {
    @Autowired UserService userService;

    @PostMapping
    public void create(Order order) {
        if (order.getAmount() > 5000) { // Magic number
            notify(order); // Business logic in controller
        }
        userService.save(order); // Cross-layer access
    }
}
```

✅ Good - Layered, constructor injection, clear intent:

```java
@RestController
@RequiredArgsConstructor  // Lombok - if project supports
@Slf4j                    // Lombok - if project supports
public class OrderController {
    private final OrderService orderService;

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public OrderDTO create(@Valid @RequestBody OrderRequest req) {
        var order = orderService.create(req);  // var OK - type obvious
        log.info("Order created: {}", order.id());
        return order;
    }
}
```

✅ Good - `var` usage:

```java
var user = new User("name");           // OK - constructor
var list = List.of(1, 2, 3);           // OK - factory method
var count = 10;                        // OK - literal

// Avoid var in these cases:
String value = null;                   // null - type unclear
Map<String, List<Order>> map = mapper.readValue(json, new TypeReference<>() {});  // TypeReference - type inference
```

## Avoid

- Field or setter injection
- Business logic in controllers
- Mutable DTOs
- Magic numbers and constants
- God classes or methods
- `var` with null, generic type inference, or unclear return types
- Manual logger declaration when Lombok `@Slf4j` is available
