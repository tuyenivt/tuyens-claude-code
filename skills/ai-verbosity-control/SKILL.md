---
name: ai-verbosity-control
description: Detect AI-generated over-engineering, verbosity, and premature abstraction
metadata:
  category: governance
  tags: [ai-quality, verbosity, over-engineering, maintainability]
user-invocable: false
---

# AI Verbosity Control

## When to Use

- During code review when AI-generated or AI-assisted code is suspected
- When a PR feels larger than the feature warrants
- When code complexity seems disproportionate to the problem
- When reviewing code with unusually high abstraction density

## Rules

- Judge by the ratio of complexity to value delivered
- Not all abstraction is bad -- flag only unjustified abstraction
- Three similar lines are better than a premature abstraction
- The best code is code you do not have to write

## Pattern

### Signals of AI-Generated Verbosity

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

### Severity Assessment

- **Blocker**: Abstraction that actively harms comprehension or introduces bugs
- **High**: Significant unnecessary complexity (full unused pattern, redundant layers)
- **Suggestion**: Minor verbosity that could be simplified

## Avoid

- Flagging all abstractions as over-engineering
- Removing abstractions that serve a documented purpose
- Confusing conciseness with cleverness -- readable is better than terse
- Treating longer code as automatically worse -- sometimes explicit is clearer
