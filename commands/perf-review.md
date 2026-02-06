---
description: Performance review for backend or frontend (Java 21+ / Spring Boot 4)
model: claude-opus-4-6
---

Identify performance issues in backend (Java 21+ / Spring Boot 4) or frontend (React).

## Code to Review

$ARGUMENTS

## Backend (Java 21+ / Spring Boot 4)

### Virtual Threads

- [ ] `synchronized` blocks causing thread pinning
- [ ] `ThreadLocal` with proper `remove()` cleanup (Java 25+: use `ScopedValue`)
- [ ] Connection pool sized for virtual threads (10-40)
- [ ] `spring.threads.virtual.enabled=true` configured

### Database Performance

- [ ] N+1 queries (use `@EntityGraph` or `JOIN FETCH`)
- [ ] Missing indexes on WHERE/ORDER BY columns
- [ ] Over-fetching (select only needed columns)
- [ ] No pagination for large datasets
- [ ] Connection pool sizing (HikariCP defaults)
- [ ] Query timeout configured
- [ ] Batch operations for bulk inserts/updates

### Caching Strategy

- [ ] Cache-aside pattern for read-heavy data
- [ ] Cache invalidation strategy defined
- [ ] TTL configured appropriately
- [ ] Cache key design (avoid collisions)
- [ ] Local cache vs distributed cache decision
- [ ] `@Cacheable` on appropriate service methods

### Stateless Design

- [ ] No server-side session state (use JWT/tokens)
- [ ] Externalized session if needed (Redis)
- [ ] No static mutable state
- [ ] Idempotent operations where possible

### Observability (Code-level)

- [ ] Structured logging (JSON format)
- [ ] Correlation ID propagation
- [ ] Key operation logging (entry/exit/errors)
- [ ] Micrometer metrics for custom operations
- [ ] `@Timed` on critical methods
- [ ] Health indicators for dependencies

### Memory & I/O

- [ ] Large objects in loops
- [ ] String concat in loops (use StringBuilder)
- [ ] Unbounded caches/collections
- [ ] Streaming for large files (not in memory)
- [ ] Timeouts on external calls
- [ ] Circuit breaker for external services

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

## Key Skills Reference

For detailed performance patterns, reference these skills:

**Backend Performance:**

- `virtual-thread-lock`, `jpa-performance`, `caching`, `db-indexing`, `async-processing`, `observability`

**Frontend Performance:**

- `react-memoization`, `react-component-design`, `payload-optimization`

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

## Database Issues

[N+1, missing indexes, connection pool]

## Caching Opportunities

[What to cache, invalidation strategy]

## Observability Gaps

[Missing logging, metrics, correlation IDs]

## Measurements Needed

| Issue | How to Measure |

## Recommendations

[Prioritized with trade-offs]
```
