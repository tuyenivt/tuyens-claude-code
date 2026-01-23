---
description: Review React code for patterns, performance, accessibility
model: claude-sonnet-4-5
---

Review React code for best practices, performance, and accessibility.

## Code to Review

$ARGUMENTS

## Review Checklist

### Component Design

- [ ] Single responsibility
- [ ] Props clearly typed
- [ ] Logic in custom hooks
- [ ] Appropriate size (<200 lines)

### Performance

- [ ] useMemo/useCallback where needed
- [ ] No expensive calculations in render
- [ ] Stable keys for lists
- [ ] Large lists virtualized

### Hooks

- [ ] Correct dependencies in useEffect
- [ ] Cleanup functions present
- [ ] No stale closures
- [ ] Custom hooks for reuse

### Accessibility

- [ ] Semantic HTML
- [ ] ARIA attributes
- [ ] Keyboard navigation
- [ ] Focus management

### TypeScript

- [ ] No `any` types
- [ ] Props properly typed
- [ ] Event handlers typed

## Output

```markdown
## Summary

[One paragraph assessment]

## Findings

### Critical | High | Medium | Low

- **Location:** [file:line]
- **Issue:** [description]
- **Fix:** [suggestion]

## Recommendations

[Prioritized improvements]
```
