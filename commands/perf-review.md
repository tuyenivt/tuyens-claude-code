---
description: Performance review for backend or frontend (Java 21+ / Spring Boot 4)
model: claude-sonnet-4-5
---

Identify performance issues in backend (Java 21+ / Spring Boot 4) or frontend (React).

## Code to Review

$ARGUMENTS

## Backend (Java 21+ / Spring Boot 4)

### Virtual Threads
- [ ] `synchronized` blocks causing thread pinning
- [ ] ThreadLocal misuse with virtual threads
- [ ] Connection pool sized for virtual threads (10-40)
- [ ] `spring.threads.virtual.enabled=true` configured

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

### GC Tuning
- [ ] G1 vs ZGC for workload
- [ ] Appropriate heap sizing

### Startup
- [ ] Class Data Sharing (CDS) enabled
- [ ] Lazy initialization where appropriate

### I/O
- [ ] Blocking operations on virtual threads (OK)
- [ ] Timeouts configured
- [ ] Large files streaming (not in memory)

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

## Virtual Thread Compatibility
[Thread pinning issues, synchronized blocks]

## Measurements Needed
| Issue | How to Measure |

## Recommendations
[Prioritized with trade-offs]
```
