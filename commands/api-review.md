---
description: Review Spring Boot API code for architecture, design, and data access
model: claude-sonnet-4-5
---

Review Spring Boot API code for architecture, design patterns, and data access issues.

## Code to Review

$ARGUMENTS

## Review Checklist

### Architecture

- [ ] Controller → Service → Repository separation
- [ ] No business logic in controllers
- [ ] Single responsibility per class

### API Design

- [ ] RESTful URLs, correct HTTP methods
- [ ] Proper status codes
- [ ] Input validation with @Valid
- [ ] Consistent error format

### JPA & Data

- [ ] No N+1 queries
- [ ] Appropriate fetch strategies
- [ ] Proper cascade settings
- [ ] Indexes on queried columns

### Transactions

- [ ] @Transactional on service (not controller)
- [ ] readOnly=true for reads
- [ ] Appropriate isolation levels

## Output

```markdown
## Summary

[One paragraph assessment]

## Findings

### Critical | High | Medium | Low

- **Location:** [file:line]
- **Issue:** [description]
- **Impact:** [why it matters]

## Recommendations

[Prioritized improvements with trade-offs]
```
