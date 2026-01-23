---
description: General code review for pull requests
model: claude-sonnet-4-5
---

Code review for pull requests and code changes.

## Code to Review

$ARGUMENTS

## Review Checklist

### Correctness

- [ ] Does what it's supposed to do
- [ ] Edge cases handled
- [ ] Error paths considered

### Readability

- [ ] Self-documenting code
- [ ] Clear naming
- [ ] Appropriate complexity

### Maintainability

- [ ] Changes are focused
- [ ] Follows existing patterns
- [ ] No unnecessary coupling

### Testing

- [ ] Appropriate coverage
- [ ] Edge cases tested

## Feedback Labels

| Label        | Meaning       | Required |
| ------------ | ------------- | -------- |
| [Blocker]    | Must fix      | Yes      |
| [Suggestion] | Would improve | No       |
| [Question]   | Need clarity  | Clarify  |
| [Nitpick]    | Minor         | No       |
| [Praise]     | Done well     | -        |

## Output

```markdown
## Summary

**Assessment:** Approve | Request Changes | Discuss

**Done Well:**

- [Positive 1]

## Findings

### [Blocker] Location

- Issue: [description]
- Why: [impact]

### [Suggestion] Location

- [improvement idea]

## Key Takeaways

[Summary]
```
