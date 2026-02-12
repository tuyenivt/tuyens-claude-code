---
name: task-code-review
description: Code review for pull requests (framework-aware)
metadata:
  category: review
  tags: [code-review, pull-request, quality]
  type: workflow
---

# Code Review

## When to Use

- Pull request reviews
- Code change reviews
- Pre-merge quality checks

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
- [ ] Constructor injection - use `@RequiredArgsConstructor` if Lombok available
- [ ] `@Slf4j` for logging if Lombok available
- [ ] Avoid `ResponseEntity` unless multiple status codes/response types in same method
- [ ] Use `var` when type is obvious (constructors, literals, factory methods)

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

## Key Skills Reference

When providing detailed feedback, reference these skills:

**Backend:**

- Use skill: `api-guidelines` for API design and REST endpoint patterns
- Use skill: `concurrency-model` for thread-safety
- Use skill: `jpa-performance` for data access
- Use skill: `exception-handling` for error handling
- Use skill: `transaction` for transaction management

**Frontend:**

- Use skill: `react-component-design` for component patterns
- Use skill: `react-state-management` for state patterns
- Use skill: `react-memoization` for optimization

**Cross-cutting:**

- Use skill: `coding-standards` for style guidelines
- Use skill: `api-guidelines` for API consistency
- Use skill: `observability` for logging and metrics
- Use skill: `resiliency` for fault tolerance

## Feedback Labels

| Label        | Meaning       | Required |
| ------------ | ------------- | -------- |
| [Blocker]    | Must fix      | Yes      |
| [Suggestion] | Would improve | No       |
| [Question]   | Need clarity  | Clarify  |
| [Nitpick]    | Minor         | No       |
| [Praise]     | Done well     | -        |

## Rules

- Review the whole change, not just individual files
- Check for consistency with existing codebase patterns
- Provide actionable feedback with examples
- Acknowledge good work alongside issues

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

## Avoid

- Nitpicking style when no standard exists
- Blocking on personal preference
- Reviewing without understanding context
