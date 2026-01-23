---
description: Code review for pull requests (framework-aware)
model: claude-sonnet-4-5
---

Code review for pull requests and code changes. Adapts to framework context.

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

## Framework-Specific Checks

**Java/Spring:**

- [ ] Using Records for DTOs (not classes)
- [ ] Pattern matching where applicable (Java 21+)
- [ ] No `synchronized` blocks (Virtual Thread compatibility)
- [ ] Constructor injection (not field `@Autowired`)

**React/TypeScript:**

- [ ] Props interfaces exported
- [ ] Proper hooks dependencies
- [ ] Accessibility (ARIA, semantic HTML)
- [ ] Memoization only where beneficial

**General (any language):**

- [ ] Clear naming and structure
- [ ] Error handling appropriate
- [ ] Tests cover key scenarios
- [ ] Documentation for complex logic

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
