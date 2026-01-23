---
description: Review Spring Boot 4 API code for architecture, design, and data access
model: claude-sonnet-4-5
---

Review Spring Boot 4 API code for architecture, design patterns, and data access issues.

## Code to Review

$ARGUMENTS

## Review Checklist

### Architecture
- [ ] Controller → Service → Repository separation
- [ ] No business logic in controllers
- [ ] Single responsibility per class
- [ ] Jakarta EE 11 compliance (Spring Boot 4)

### API Design
- [ ] RESTful URLs, correct HTTP methods
- [ ] Proper status codes
- [ ] Input validation with @Valid (Jakarta Validation)
- [ ] Consistent error format

### JPA & Data (Spring Boot 4)
- [ ] No N+1 queries
- [ ] Appropriate fetch strategies
- [ ] Proper cascade settings
- [ ] Indexes on queried columns

### Transactions
- [ ] @Transactional on service (not controller)
- [ ] readOnly=true for reads
- [ ] Appropriate isolation levels

### Virtual Threads Compatibility
- [ ] No `synchronized` blocks (causes thread pinning)
- [ ] Use `ReentrantLock` for thread safety
- [ ] Connection pool sized appropriately (10-40)
- [ ] No ThreadLocal abuse

### Java 21+ Features
- [ ] Records for DTOs
- [ ] Pattern matching where applicable
- [ ] Sealed classes for type hierarchies (if applicable)

## Output

```markdown
## Summary
[One paragraph assessment]

## Findings
### Critical | High | Medium | Low
- **Location:** [file:line]
- **Issue:** [description]
- **Impact:** [why it matters]

## Virtual Thread Issues
[Any synchronized blocks or thread pinning concerns]

## Recommendations
[Prioritized improvements with trade-offs]
```
