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
public class OrderController {
    private final OrderService orderService;

    public OrderController(OrderService orderService) {
        this.orderService = orderService;
    }

    @PostMapping
    public ResponseEntity<OrderDTO> create(OrderRequest req) {
        return ResponseEntity.ok(orderService.create(req));
    }
}
```

## Avoid

- Field or setter injection
- Business logic in controllers
- Mutable DTOs
- Magic numbers and constants
- God classes or methods
