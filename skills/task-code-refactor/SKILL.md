---
name: task-code-refactor
description: Safe, incremental refactoring plan with risk assessment
metadata:
  category: review
  tags: [refactoring, code-quality, technical-debt]
  type: workflow
---

# Code Refactor

## When to Use

- Code smell identification and resolution
- Technical debt reduction
- Safe refactoring planning
- Code quality improvement

## Focus Areas

- **Smells**: Long methods, large classes, duplication, feature envy
- **Patterns**: Extract, move, inline refactorings
- **Safety**: Test coverage verification, incremental steps
- **Risk**: Impact analysis, rollback plans

## Code Smells

| Smell                    | Risk   |
| ------------------------ | ------ |
| Long Method (>20 lines)  | Medium |
| Large Class (>200 lines) | High   |
| Duplicate Code           | Medium |
| Feature Envy             | Low    |
| Long Parameter List      | Low    |

## Safe Refactoring Steps

1. Ensure tests exist
2. Commit current state
3. Apply ONE refactoring
4. Run tests
5. Commit
6. Repeat

## Common Refactorings

| Smell        | Refactoring      |
| ------------ | ---------------- |
| Long Method  | Extract Method   |
| Large Class  | Extract Class    |
| Duplication  | Extract, Pull Up |
| Feature Envy | Move Method      |

## Key Skills Reference

Apply these skills when performing refactorings:

- Use skill: `coding-standards` for style and structure guidelines
- Use skill: `virtual-thread-lock` for thread-safety during refactoring
- Use skill: `jpa-performance` for data access optimizations
- Use skill: `exception-handling` for error handling improvements

## Rules

- Never refactor without tests
- Never combine refactoring with feature changes
- One refactoring per commit
- Verify behavior preservation after each step
- Assess risks and prerequisites before starting

## Output

```markdown
## Current State

[Description of issues]

## Risk Assessment

| Factor        | Status         |
| ------------- | -------------- |
| Test Coverage | Safe/Warn/Risk |
| Complexity    | Safe/Warn/Risk |
| Dependencies  | Safe/Warn/Risk |

## Step-by-Step Plan

### Step 1: [Name]

- What: [change]
- Risk: Low/Medium/High
- Tests: [verify]

## Prerequisites

- [ ] Add tests for [area]
- [ ] Create branch

## Rollback

[How to revert]
```

## Avoid

- Refactoring without test coverage
- Combining refactoring with new features
- Large, multi-step changes in a single commit
- Refactoring code you don't understand
