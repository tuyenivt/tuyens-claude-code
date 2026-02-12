---
name: architecture-guardrail
description: Layer violation and boundary erosion detection for structural integrity
metadata:
  category: governance
  tags: [architecture, boundaries, coupling, layer-violations]
user-invocable: false
---

# Architecture Guardrail

## When to Use

- During code review to detect structural drift
- When changes introduce new dependencies between modules
- When code bypasses established abstractions or layers
- When reviewing changes to shared or core modules

## Rules

- Focus on structural integrity, not implementation style
- Flag violations only when they cross established boundaries
- Distinguish intentional refactoring from accidental drift
- One boundary violation is more important than ten style issues

## Pattern

### Layer Violations

Detect code that bypasses the established layering:

**Bad: Controller accessing repository directly**

```java
@RestController
public class OrderController {
    private final OrderRepository repository; // violation: skips service layer
}
```

**Good: Proper layering**

```java
@RestController
public class OrderController {
    private final OrderService service; // respects layer boundary
}
```

Common layer violations:

- Controller -> Repository (skipping service)
- Service -> Controller (reverse dependency)
- Domain entity in API response (skipping DTO)
- Business logic in controller (responsibility leak)
- Infrastructure concerns in domain layer

### Module Coupling

Detect new coupling between previously independent modules:

- Direct imports across module boundaries instead of through defined interfaces
- Shared mutable state between modules
- Circular dependencies (A -> B -> A)
- Feature module depending on another feature module's internals

### Boundary Erosion

Detect gradual weakening of established abstractions:

- Adding "just one more" public method to an internal class
- Exposing implementation details through return types
- Domain logic leaking into infrastructure layer
- Configuration values used directly instead of through abstraction

### Drift Detection

Compare change patterns against established conventions:

- New code that contradicts existing module structure
- Inconsistent package organization within the same module
- Mixed architectural styles (some modules use ports/adapters, new code does not)

### Good: Specific guardrail finding

```
[High] order-service/src/main/java/com/example/order/OrderController.java:45
- Issue: Controller directly queries PaymentRepository, bypassing PaymentService
- Impact: Creates hidden coupling between order and payment modules
- Drift: Existing pattern uses inter-service communication via PaymentClient
```

### Bad: Vague architectural concern

```
[Suggestion] The architecture could be improved.
```

## Avoid

- Flagging intentional architectural decisions as violations
- Enforcing a specific architecture style not established in the project
- Treating all coupling as equally harmful
- Missing the forest for the trees -- one structural violation matters more than many style issues
