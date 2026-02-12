---
name: complexity-review
description: Detect unnecessary complexity and provide concrete simpler alternatives
metadata:
  category: governance
  tags: [simplification, cognitive-load, over-engineering, verbosity, maintainability]
user-invocable: false
---

# Complexity Review

## When to Use

- During code review when complexity seems disproportionate to the problem
- When AI-generated or AI-assisted code is suspected of over-engineering
- When a PR feels larger than the feature warrants
- When reviewing code that is correct but harder to maintain than necessary

## Rules

- Judge by the ratio of complexity to value delivered
- Not all abstraction is bad -- flag only unjustified abstraction
- Three similar lines are better than a premature abstraction
- Simplification must preserve correctness and intent
- Measure complexity by how long it takes a new reader to understand
- Prefer removing code over refactoring code
- Every finding must include a concrete simpler alternative

## Pattern

### Phase 1: Detect Complexity Signals

**1. Interface-with-single-implementation**

```java
// Bad: unnecessary interface
public interface OrderProcessor { void process(Order order); }
public class DefaultOrderProcessor implements OrderProcessor { ... }

// Good: just the class, add interface when second implementation exists
public class OrderProcessor { void process(Order order) { ... } }
```

**2. Unnecessary factory/builder for simple construction**

```java
// Bad: factory for a simple object
public class NotificationFactory {
    public static Notification create(String type, String message) {
        return new Notification(type, message);
    }
}

// Good: direct construction
var notification = new Notification(type, message);
```

**3. Redundant mapping layers**

```java
// Bad: entity -> domain -> dto with no logic difference
record OrderEntity(...) {}
record OrderDomain(...) {} // identical fields, no domain logic
record OrderDto(...) {}    // identical fields again

// Good: entity -> dto (add domain model only when it carries behavior)
record OrderEntity(...) {}
record OrderDto(...) {}
```

**4. Excessive indirection**

```java
// Bad: wrapper that adds no value
public class DatabaseHealthChecker {
    private final DataSource dataSource;
    public boolean isHealthy() {
        return dataSource.getConnection() != null; // just delegate
    }
}

// Good: use the DataSource directly or use Spring Actuator health endpoint
```

**5. Boilerplate the framework already provides**

```java
// Bad: manual exception-to-response mapping in every controller
try { ... } catch (NotFoundException e) {
    return ResponseEntity.notFound().build();
}

// Good: @ControllerAdvice handles this globally
```

**6. Over-commented obvious code**

```java
// Bad: comments restating the code
// Get the user by ID
var user = userService.getById(id);
// Return the user
return user;

// Good: self-documenting, no comments needed
return userService.getById(id);
```

### Phase 2: Prescribe Simpler Alternatives

**7. Replace conditional chains with data structures**

```java
// Bad: branching logic for static mapping
public String getStatusLabel(String status) {
    if ("PENDING".equals(status)) return "Pending Review";
    else if ("APPROVED".equals(status)) return "Approved";
    else if ("REJECTED".equals(status)) return "Rejected";
    else return "Unknown";
}

// Good: Map lookup
private static final Map<String, String> STATUS_LABELS = Map.of(
    "PENDING", "Pending Review",
    "APPROVED", "Approved",
    "REJECTED", "Rejected"
);

public String getStatusLabel(String status) {
    return STATUS_LABELS.getOrDefault(status, "Unknown");
}
```

**8. Leverage framework features**

```java
// Bad: manual pagination logic
public List<Order> getOrders(int page, int size) {
    var all = orderRepository.findAll();
    int start = page * size;
    int end = Math.min(start + size, all.size());
    return all.subList(start, end);
}

// Good: Spring Data pagination
public Page<Order> getOrders(Pageable pageable) {
    return orderRepository.findAll(pageable);
}
```

**9. Use language features**

```java
// Bad: verbose null handling
String name = null;
if (user != null && user.getProfile() != null) {
    name = user.getProfile().getName();
}
if (name == null) {
    name = "Anonymous";
}

// Good: Optional chaining or pattern matching
var name = Optional.ofNullable(user)
    .map(User::getProfile)
    .map(Profile::getName)
    .orElse("Anonymous");
```

**10. Eliminate dead abstractions**

If a class, interface, or method exists but:

- Has no callers outside tests
- Has a single implementation with no plans for extension
- Wraps another class without adding behavior

Suggest removal with specific evidence.

**11. Consolidate redundant validation**

```java
// Bad: same validation in multiple places
// In controller
if (request.email() == null || request.email().isBlank()) throw ...;
// In service
if (email == null || email.isBlank()) throw ...;
// In entity
@PrePersist void validate() { if (email == null) throw ...; }

// Good: single validation point with Bean Validation
record CreateUserRequest(@NotBlank @Email String email) {}
```

### Severity Assessment

- **Blocker**: Abstraction that actively harms comprehension or introduces bugs
- **High**: Significant unnecessary complexity (full unused pattern, redundant layers)
- **Suggestion**: Minor verbosity that could be simplified

### Output Format

For each finding, provide:

```
Location: file:line
Signal: [which complexity signal from Phase 1 or Phase 2]
Current: [brief description of current approach]
Simpler: [concrete alternative]
Reduction: [what gets removed or simplified]
Severity: Blocker | High | Suggestion
```

## Avoid

- Flagging all abstractions as over-engineering
- Removing abstractions that serve a documented purpose
- Confusing conciseness with cleverness -- readable is better than terse
- Treating longer code as automatically worse -- sometimes explicit is clearer
- Suggesting clever one-liners that reduce readability
- Removing error handling or validation in the name of simplicity
- Ignoring domain complexity that justifies code complexity
- Compressing code that is intentionally explicit for safety
