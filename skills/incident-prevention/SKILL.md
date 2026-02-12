---
name: incident-prevention
description: Systemic prevention strategies derived from incident root cause analysis
metadata:
  category: ops
  tags: [incident, prevention, guardrails, reliability]
user-invocable: false
---

# Incident Prevention

## When to Use

- After root cause analysis is complete and containment is in place
- During incident retrospective to propose systemic improvements
- When identifying patterns across multiple related incidents

## Rules

- Prevention must address the class of failure, not just the specific instance
- Every recommendation must be actionable and scoped
- Prioritize by blast radius reduction potential
- Include both technical and process improvements

## Pattern

### Prevention Categories

| Category               | Focus                                                       |
| ---------------------- | ----------------------------------------------------------- |
| Architecture guardrail | Boundary enforcement, isolation, bulkheading                |
| Observability          | Missing signals that would have caught this earlier         |
| Testing                | Test scenario that would have prevented this in CI          |
| Deployment safety      | Canary, feature flags, rollback automation                  |
| Capacity planning      | Resource limits, pool sizing, rate limiting                 |
| Dependency management  | Timeout budgets, circuit breakers, fallback paths           |
| Process improvement    | Review checklist update, runbook addition, on-call training |
| Chaos engineering      | Fault injection scenario to validate resilience             |

### Recommendation Structure

For each recommendation:

1. **What** -- specific change to make
2. **Why** -- which failure class it prevents
3. **Priority** -- immediate / next sprint / quarterly
4. **Blast radius reduction** -- how much damage this prevents next time

### Good: Specific prevention tied to failure class

```
1. Add connection pool circuit breaker (Priority: immediate)
   What: Configure HikariCP leak detection (30s) and add circuit breaker on payment-gateway client
   Why: Prevents connection pool exhaustion from slow downstream dependencies
   Blast radius reduction: Isolates payment failures from order and cart flows

2. Add timeout budget enforcement (Priority: next sprint)
   What: Set total request timeout = sum(downstream timeouts) + 20% buffer, fail fast when budget exceeded
   Why: Prevents retry amplification from consuming full connection pool
   Blast radius reduction: Bounds cascading latency at the integration boundary

3. Add connection pool saturation alert (Priority: immediate)
   What: Alert when HikariCP active connections > 80% for 2 minutes
   Why: Provides early warning before pool exhaustion causes user-facing errors
   Blast radius reduction: Enables containment before cascading failure
```

### Bad: Vague improvement suggestion

```
We should improve our monitoring and add better error handling.
```

## Avoid

- Recommendations that only fix the specific instance, not the failure class
- Unbounded improvement wishlists without prioritization
- Ignoring process and human factors (runbooks, training, review checklists)
- Proposing architectural rewrites when targeted fixes suffice
