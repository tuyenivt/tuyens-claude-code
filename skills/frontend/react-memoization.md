---
name: react-memoization
description: Performance optimization with useMemo and useCallback
category: frontend
tags: [react, performance, hooks, optimization]
---

# React Memoization

## When to Use

- Preventing unnecessary re-renders of expensive components
- Memoizing expensive computations
- Passing stable references to child components

## Rules

- Use `useMemo` for computations expensive (> 5ms)
- Use `useMemo` when value is a dependency in other hooks
- Use `useCallback` when passing to memoized child component
- Use `useCallback` when it's a dependency in other hooks
- Profile first before optimizing
- Always include all used values in dependency array
- Avoid premature optimization for cheap computations

## Pattern

❌ Bad - Recomputes on every render:

```jsx
function Parent({ data }) {
  const expensiveValue = computeExpensive(data); // Recalculates every render
  return <Child value={expensiveValue} />;
}
```

✅ Good - Memoized computation:

```jsx
function Parent({ data }) {
  const expensiveValue = useMemo(() => computeExpensive(data), [data]);
  return <Child value={expensiveValue} />;
}
```

## Avoid

- Premature optimization without profiling
- Memoizing cheap computations (< 1ms)
- Missing dependencies in dependency array
- Using inline objects/arrays (create new references)
