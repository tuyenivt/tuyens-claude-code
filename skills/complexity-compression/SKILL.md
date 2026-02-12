---
name: complexity-compression
description: Identify simplification opportunities to reduce cognitive load
metadata:
  category: governance
  tags: [simplification, cognitive-load, complexity, refactoring]
user-invocable: false
---

# Complexity Compression

## When to Use

- During code review to identify simplification opportunities
- After detecting AI-generated verbosity
- When a change increases cognitive load disproportionate to its value
- When reviewing code that is correct but harder to maintain than necessary

## Rules

- Simplification must preserve correctness and intent
- Measure complexity by how long it takes a new reader to understand
- Prefer removing code over refactoring code
- Every suggestion must include a concrete simpler alternative

## Pattern

### 1. Replace Conditional Chains with Data Structures

**Bad: Branching logic for static mapping**

```java
public String getStatusLabel(String status) {
    if ("PENDING".equals(status)) return "Pending Review";
    else if ("APPROVED".equals(status)) return "Approved";
    else if ("REJECTED".equals(status)) return "Rejected";
    else return "Unknown";
}
```

**Good: Map lookup**

```java
private static final Map<String, String> STATUS_LABELS = Map.of(
    "PENDING", "Pending Review",
    "APPROVED", "Approved",
    "REJECTED", "Rejected"
);

public String getStatusLabel(String status) {
    return STATUS_LABELS.getOrDefault(status, "Unknown");
}
```

### 2. Leverage Framework Features

**Bad: Manual pagination logic**

```java
public List<Order> getOrders(int page, int size) {
    var all = orderRepository.findAll();
    int start = page * size;
    int end = Math.min(start + size, all.size());
    return all.subList(start, end);
}
```

**Good: Spring Data pagination**

```java
public Page<Order> getOrders(Pageable pageable) {
    return orderRepository.findAll(pageable);
}
```

### 3. Use Language Features

**Bad: Verbose null handling**

```java
String name = null;
if (user != null && user.getProfile() != null) {
    name = user.getProfile().getName();
}
if (name == null) {
    name = "Anonymous";
}
```

**Good: Optional chaining or pattern matching**

```java
var name = Optional.ofNullable(user)
    .map(User::getProfile)
    .map(Profile::getName)
    .orElse("Anonymous");
```

### 4. Eliminate Dead Abstractions

If a class, interface, or method exists but:

- Has no callers outside tests
- Has a single implementation with no plans for extension
- Wraps another class without adding behavior

Suggest removal with specific evidence.

### 5. Consolidate Redundant Validation

**Bad: Same validation in multiple places**

```java
// In controller
if (request.email() == null || request.email().isBlank()) throw ...;
// In service
if (email == null || email.isBlank()) throw ...;
// In entity
@PrePersist void validate() { if (email == null) throw ...; }
```

**Good: Single validation point with Bean Validation**

```java
record CreateUserRequest(@NotBlank @Email String email) {}
```

### Output Format

When suggesting compression, always provide:

```
Location: file:line
Current: [brief description of current approach]
Simpler: [concrete alternative]
Reduction: [what gets removed or simplified]
```

## Avoid

- Compressing code that is intentionally explicit for safety
- Suggesting clever one-liners that reduce readability
- Removing error handling or validation in the name of simplicity
- Ignoring domain complexity that justifies code complexity
