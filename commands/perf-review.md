---
description: Performance review for backend or frontend
model: claude-sonnet-4-5
---

Identify performance issues in backend (Java) or frontend (React).

## Code to Review

$ARGUMENTS

## Backend (Java/Spring)

### Database

- [ ] N+1 queries
- [ ] Missing indexes
- [ ] Over-fetching
- [ ] No pagination

### Memory

- [ ] Large objects in loops
- [ ] String concat in loops
- [ ] Unbounded caches
- [ ] Memory leaks

### I/O

- [ ] Blocking operations
- [ ] No timeouts
- [ ] Large files in memory

## Frontend (React)

### Rendering

- [ ] Unnecessary re-renders
- [ ] Missing memoization
- [ ] Inline objects in JSX
- [ ] Heavy computations in render

### Data

- [ ] Over-fetching
- [ ] No caching
- [ ] Waterfall requests

### Assets

- [ ] Unoptimized images
- [ ] No lazy loading
- [ ] Large bundle

## Principle

> Measure first. No optimization without profiling.

## Output

```markdown
## Summary

[Performance assessment]

## Findings

### Critical | High | Medium

- **Location:** [file:line]
- **Issue:** [description]
- **Impact:** [expected improvement]

## Measurements Needed

| Issue | How to Measure |

## Recommendations

[Prioritized with trade-offs]
```
