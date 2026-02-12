---
name: engineering-governance
description: Engineering process and governance improvement patterns for incident prevention
metadata:
  category: governance
  tags: [governance, process, review, deployment, testing, incident]
user-invocable: false
---

# Engineering Governance

## When to Use

- During postmortem to recommend process improvements
- When identifying governance gaps that contributed to a production failure
- When strengthening engineering processes after a pattern of related incidents
- When evaluating deployment, review, or testing process effectiveness

## Rules

- Process changes must be proportional to the risk they mitigate
- Every recommendation must be actionable and assignable
- Prefer structural enforcement over behavioral expectations
- Balance safety with development velocity -- do not over-process low-risk areas
- Prioritize by blast radius reduction potential

## Pattern

### Governance Dimensions

| Dimension             | Focus                                                              |
| --------------------- | ------------------------------------------------------------------ |
| Review process        | Checklist updates, reviewer assignment, risk-based review triggers |
| Testing strategy      | Missing test types, coverage gaps, chaos experiment design         |
| Deployment safety     | Canary, feature flags, progressive rollout, rollback criteria      |
| Design governance     | When to require design docs, ADRs, architecture review             |
| Operational readiness | Runbook updates, on-call training, incident response playbooks     |
| Knowledge sharing     | Post-incident learnings distribution, pattern documentation        |

### Recommendation Structure

For each recommendation:

1. **What** -- specific process change
2. **Why** -- which failure class or gap it addresses
3. **Trigger** -- when this process activates (change scope, risk level, module affected)
4. **Owner** -- who is responsible for implementation (role, not individual)
5. **Priority** -- immediate / next sprint / quarterly

### Good: Specific governance improvement with trigger

```
What: Require design doc for changes that modify transaction boundaries or add new async flows
Why: Transaction boundary changes have caused 3 incidents in the last quarter; review alone is insufficient for assessing distributed state impact
Trigger: PR modifies @Transactional annotations, adds new event publishers, or changes message consumer logic
Owner: Tech lead for affected module
Priority: next sprint
```

### Bad: Vague process suggestion

```
We should have better design reviews for important changes.
```

### Design Doc Triggers

Recommend mandatory design documentation when changes involve:

- Transaction boundary modifications across services
- New async flows or event-driven communication
- Data model changes affecting multiple consumers
- New external dependency integration
- Changes to authentication or authorization model
- Infrastructure topology changes

### Chaos Experiment Design

For each proposed chaos experiment:

- **Hypothesis** -- what failure mode is being validated
- **Injection** -- specific fault to introduce (latency, error, resource limit)
- **Expected behavior** -- how the system should respond (fallback, graceful degradation)
- **Blast radius control** -- how to limit experiment impact

## Avoid

- Process changes disproportionate to the risk (heavyweight process for low-risk areas)
- Behavioral expectations without structural support ("be more careful" is not a process)
- Governance changes that only apply to the specific incident, not the failure class
- Adding process without removing or streamlining existing overhead
- Ignoring the cost of process on development velocity
