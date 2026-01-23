---
description: Performance review for backend or frontend (Java 25 + Spring Boot 4)
model: claude-sonnet-4-5
---

Identify performance issues in backend (Java 25/Spring Boot 4) or frontend (React).

## Code to Review

$ARGUMENTS

## Backend (Java 25 / Spring Boot 4)

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

### Memory (Java 25)
- [ ] Large objects in loops
- [ ] String concat in loops
- [ ] Unbounded caches
- [ ] Memory leaks
- [ ] Compact Object Headers enabled (JEP 519)

### GC Tuning (Java 25)
- [ ] G1 vs Shenandoah for workload
- [ ] Generational Shenandoah for low-latency
- [ ] Appropriate heap sizing

### Startup (Java 25)
- [ ] AOT compilation considered (JEP 514/515)
- [ ] Class Data Sharing (CDS) enabled

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
