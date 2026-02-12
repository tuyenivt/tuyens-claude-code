---
name: release-safety
description: Rollout, rollback, and deployment risk patterns
metadata:
  category: ops
  tags: [deployment, rollout, rollback, canary, feature-flags]
user-invocable: false
---

# Release Safety

## When to Use

- During architecture design to plan deployment strategy
- When designing rollback capability for a new feature
- When evaluating deployment risk for schema changes or data migrations
- When planning feature flag strategies for progressive rollout

## Rules

- Every deployment must have a rollback plan before it ships
- Database migrations must be backward compatible with the previous code version
- Feature flags for high-risk changes -- decouple deploy from release
- Canary or progressive rollout for changes with wide blast radius
- Monitor error rate and latency during rollout; define automatic rollback triggers

## Pattern

### Rollout Strategies

| Strategy       | Use When                               | Rollback Speed | Risk |
| -------------- | -------------------------------------- | -------------- | ---- |
| Feature flag   | High-risk logic, gradual user exposure | Instant        | Low  |
| Canary         | Infrastructure or performance changes  | Fast (minutes) | Low  |
| Blue-green     | Full environment swap needed           | Fast (minutes) | Med  |
| Rolling update | Low-risk, stateless services           | Moderate       | Med  |
| Big bang       | Never (avoid)                          | Slow           | High |

### Database Migration Safety

Deploy order for additive changes:

1. Deploy new code that handles both old and new schema
2. Run migration (add column, add table)
3. Deploy code that uses new schema
4. Remove old schema support in next release

Deploy order for destructive changes:

1. Deploy code that stops using old column
2. Verify no reads/writes to old column in production
3. Run migration to drop column

**Rule**: Code must work with both old and new schema during rolling deployment.

### Feature Flag Design

- **Kill switch** -- disable feature instantly without deploy
- **Percentage rollout** -- expose to N% of users, increase gradually
- **User targeting** -- enable for internal users or beta cohort first
- **Time-boxed** -- flags must have an expiry; remove after full rollout

### Good: Specific rollout plan with rollback trigger

```
Rollout: Canary to 5% of traffic for 30 minutes
Monitor: p99 latency < 500ms, error rate < 0.1%
Rollback trigger: error rate > 0.5% or p99 > 2s for 5 minutes
Rollback action: revert to previous deployment version
DB migration: additive only (new nullable column), backward compatible
Feature flag: ENABLE_NEW_PRICING gated for internal users first, then 10% -> 50% -> 100% over 3 days
```

### Bad: No rollback plan

```
Deploy to all instances. If something breaks, we will fix it.
```

## Avoid

- Deploying without a rollback plan
- Schema migrations that break the previous code version
- Big-bang deployments for high-blast-radius changes
- Feature flags without expiry dates (flag debt)
- Rollback plans that require data migration to undo
