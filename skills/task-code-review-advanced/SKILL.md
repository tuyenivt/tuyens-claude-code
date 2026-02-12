---
name: task-code-review-advanced
description: Staff-level system-aware code review with risk assessment
metadata:
  category: review
  tags: [code-review, pull-request, risk-assessment, architecture, ai-quality]
  type: workflow
---

# Code Review -- Staff Edition

## Purpose

Staff-level code review that prioritizes system risk over style:

- **Risk-based evaluation** -- assess blast radius and cross-module impact before line-level feedback
- **Architecture boundary protection** -- detect coupling, layer violations, and structural drift early
- **Maintainability control** -- catch over-abstraction, verbosity inflation, and premature generalization
- **High-signal findings only** -- no nitpicking, no trivial formatting comments

This skill focuses on the 80/20: high-impact issues that affect system integrity.

## When to Use

- Pull request reviews
- Code change reviews before merge
- Post-AI-generation quality gate
- Architecture drift detection
- Pre-merge risk assessment

## Scope

Before starting the review, ask the user which scope they need:

| Scope      | What runs                                                                      |
| ---------- | ------------------------------------------------------------------------------ |
| Core       | Phases A-E only (risk, correctness, architecture, AI quality, maintainability) |
| + Perf     | Core + delegate to skill: `task-perf-review`                                   |
| + Security | Core + delegate to skill: `task-code-secure`                                   |
| Full       | Core + Performance + Security                                                  |

Default: **Core** (if the user doesn't specify, run Core only).

If the user invokes with an explicit scope argument (e.g., `/task-code-review-advanced +perf`), skip the question and use that scope directly.

## Inputs

| Input                     | Required | Description                                    |
| ------------------------- | -------- | ---------------------------------------------- |
| Code diff or file content | Yes      | The changes under review                       |
| PR description            | No       | Context on intent and scope                    |
| Architectural assumptions | No       | Module boundaries, ownership, layering rules   |
| Module context            | No       | What the changed module does, its dependencies |

## Rules

- Review the whole change as a system impact, not file-by-file in isolation
- Lead with risk assessment before line-level findings
- Never comment on trivial formatting or style where no project standard exists
- Never block on personal preference
- If risk is low, state so explicitly and keep the review short
- Optimize for daily repeated use -- be concise
- Only delegate to skill: `task-perf-review` or skill: `task-code-secure` when the user-selected scope includes them
- Default to Core scope -- do not run performance or security sub-workflows unless explicitly requested

## Review Model

### A. PR Risk Snapshot

**Run first. This frames the entire review.**

Use skill: `pr-risk-analysis` to evaluate cross-cutting risk signals.
Use skill: `blast-radius-analysis` to assess failure propagation scope.

Output risk level and blast radius before proceeding to findings.

### B. Correctness and Safety

Check only high-impact items:

- Logical correctness -- does the code do what the PR claims?
- Error handling completeness -- are failure paths handled or silently swallowed?
- Edge cases that affect state integrity (data corruption, partial writes)
- Backward compatibility -- breaking changes to public APIs or data contracts
- Unsafe shared state mutation (especially with Virtual Threads)
- Transaction boundary correctness

Use skill: `resiliency` for fault tolerance patterns.
Use skill: `exception-handling` for error handling patterns.
Use skill: `api-guidelines` for API contract consistency.
Use skill: `transaction` for transaction scope issues.

Do not comment on trivial style.

### C. Architecture Guardrails

Use skill: `architecture-guardrail` to detect:

- Layer violations (controller calling repository directly, etc.)
- New coupling between previously independent modules
- Circular dependency risk
- Bypassing established abstractions
- Boundary erosion (domain logic leaking into controllers, etc.)
- Unintended architectural drift from established patterns

Focus on long-term structural integrity.

### D. AI-Generated Code Quality Control

Use skill: `ai-verbosity-control` to detect verbosity and over-engineering.
Use skill: `complexity-compression` to identify simplification opportunities.

Key signals:

- Over-abstraction (interfaces with single implementation, unnecessary factories)
- Premature generalization (generic solutions for one use case)
- Redundant mapping layers that add indirection without value
- Boilerplate that a framework already provides
- Pattern inflation without architectural justification

Focus on maintainability erosion, not micro-optimizations.

### E. Maintainability and Clarity

Check only high-impact clarity issues:

- Naming that obscures intent or misleads
- Mixed responsibilities in a single class/function
- Large unreviewable chunks that should be split
- Complex logic without contextual explanation

Use skill: `coding-standards` for structural guidelines.
Use skill: `observability` for logging/tracing gaps in critical paths only.

## Framework-Specific Signals

**Java 21+ / Spring Boot 3.5+:**

- Records for DTOs (not mutable classes)
- No `synchronized` blocks (Virtual Thread compatibility)
- Constructor injection only
- `@Transactional(readOnly = true)` by default
- `@MockitoBean` for tests (not deprecated `@MockBean`)

**React / TypeScript:**

- Proper hooks dependency arrays
- Memoization only where measured benefit exists
- Accessibility (ARIA, semantic HTML)
- State colocation -- state lives where it is used

**General:**

- Error handling proportional to risk
- Tests cover key behavioral scenarios
- No secrets or credentials in code

## Output

```markdown
## Summary

**Assessment:** Approve | Request Changes | Discuss
**Risk Level:** Low | Medium | High | Critical
**Blast Radius:** Narrow | Moderate | Wide

## High-Impact Findings

### [Blocker] file:line

- Issue:
- Impact:
- System Risk:

### [High] file:line

- Issue:
- Impact:

### [Suggestion] file:line

- Improvement:

## Architecture Notes

- Boundary impact:
- Coupling change:
- Drift detected:

## Maintainability Notes

- Over-engineering detected:
- Simplification opportunities:

## Key Takeaways

- 2-4 concise bullets summarizing systemic impact.
```

### Output Constraints

- Do NOT list trivial formatting issues
- If risk is low, say so and keep findings minimal
- Findings ordered by severity: Blocker > High > Suggestion
- Omit empty sections
- No [Nitpick] or [Praise] labels -- focus on actionable findings only
- Optimize for token efficiency

## Avoid

- Nitpicking style when no project standard exists
- Blocking on personal preference
- Reviewing without understanding module context
- Running performance or security sub-workflows when user requested Core scope only
- Duplicating deep performance analysis inline (delegate to skill: `task-perf-review` when in scope)
- Duplicating deep security analysis inline (delegate to skill: `task-code-secure` when in scope)
- Commenting on every file -- focus on systemic issues

## Key Skills Reference

### Existing Skills Reused

**Correctness and Safety:**

- Use skill: `resiliency` for fault tolerance patterns
- Use skill: `exception-handling` for error handling patterns
- Use skill: `api-guidelines` for API contract consistency
- Use skill: `transaction` for transaction scope

**Architecture and Quality:**

- Use skill: `coding-standards` for structural guidelines
- Use skill: `observability` for logging/tracing (lightweight)

**Framework-Specific (as needed):**

- Use skill: `spring-api` for REST API design
- Use skill: `virtual-thread-lock` for thread-safety
- Use skill: `jpa-performance` for data access
- Use skill: `react-component-design` for component patterns
- Use skill: `react-state-management` for state patterns
- Use skill: `react-memoization` for optimization

**Delegated Reviews (scope-dependent):**

- Use skill: `task-perf-review` for deep performance analysis (when scope includes +Perf or Full)
- Use skill: `task-code-secure` for deep security analysis (when scope includes +Security or Full)

### Sub-Skills Defined for This Workflow

| Skill                    | Purpose                                               |
| ------------------------ | ----------------------------------------------------- |
| `pr-risk-analysis`       | Lightweight heuristic PR risk classification          |
| `blast-radius-analysis`  | Failure propagation and change impact scope           |
| `architecture-guardrail` | Layer violation and boundary erosion detection        |
| `ai-verbosity-control`   | AI-generated over-engineering and verbosity detection |
| `complexity-compression` | Simplification opportunity identification             |
