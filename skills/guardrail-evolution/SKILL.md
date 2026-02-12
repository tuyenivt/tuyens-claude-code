---
name: guardrail-evolution
description: Evolve engineering guardrails based on incident learnings
metadata:
  category: governance
  tags: [guardrails, incident, evolution, enforcement, prevention]
user-invocable: false
---

# Guardrail Evolution

## When to Use

- During postmortem to propose new or strengthened guardrails
- When existing guardrails proved insufficient during a production failure
- When converting incident lessons into enforceable constraints
- When reviewing guardrail effectiveness across multiple incidents

## Rules

- Every guardrail must be enforceable (automated or structurally embedded)
- Guardrails must target failure classes, not specific incidents
- Prefer automated enforcement over manual process
- Each guardrail must state what it prevents and how it is checked
- Do not propose guardrails that cannot be verified

## Pattern

### Guardrail Structure

For each proposed guardrail:

| Field                   | Description                                              |
| ----------------------- | -------------------------------------------------------- |
| Rule                    | Specific, enforceable constraint                         |
| Scope                   | Where it applies (code review, CI, deployment, runtime)  |
| Enforcement mechanism   | How it is checked (lint rule, CI gate, alert, checklist) |
| Failure class prevented | Category of failure this guards against                  |
| Priority                | immediate / next sprint / quarterly                      |

### Enforcement Tiers

| Tier       | Mechanism                  | Reliability |
| ---------- | -------------------------- | ----------- |
| Automated  | CI gate, lint rule, policy | High        |
| Structural | Architecture constraint    | High        |
| Monitored  | Alert, dashboard, SLO      | Medium      |
| Manual     | Checklist, review policy   | Low         |

Prefer higher-tier enforcement. If only manual enforcement is possible, pair it with a plan to automate.

### Good: Enforceable guardrail with clear scope

```
Rule: All external service calls must have a circuit breaker configured
Scope: Code review + CI
Enforcement: Custom ArchUnit test that verifies @CircuitBreaker on all REST client methods
Failure class prevented: Cascading failure from external dependency timeout
Priority: immediate
```

### Bad: Vague unenforceable suggestion

```
Rule: Be more careful with external service calls
```

### Evolution Triggers

When proposing guardrail changes, categorize the change:

- **New guardrail** -- no existing rule covers this failure class
- **Strengthen existing** -- existing guardrail was too weak or narrow
- **Automate existing** -- manual guardrail needs automated enforcement
- **Broaden scope** -- guardrail exists but did not apply to the affected area

## Avoid

- Guardrails that cannot be verified or enforced
- Rules so broad they generate false positives and get ignored
- Manual-only enforcement for critical failure classes
- Guardrails that address only the specific incident, not the failure class
- Adding guardrails without removing or consolidating obsolete ones
