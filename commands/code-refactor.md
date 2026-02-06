---
description: Refactoring plan with risk assessment
model: claude-opus-4-6
---

Create a safe, incremental refactoring plan.

## Code to Refactor

$ARGUMENTS

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

## Output

```markdown
## Current State

[Description of issues]

## Risk Assessment

| Factor        | Status   |
| ------------- | -------- |
| Test Coverage | ✅/⚠️/❌ |
| Complexity    | ✅/⚠️/❌ |
| Dependencies  | ✅/⚠️/❌ |

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
