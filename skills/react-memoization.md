---
name: react-memoization
description: React performance optimization with useMemo and useCallback
category: pattern
tags: [react, performance, hooks]
---

# React Memoization Pattern

## Problem

React components re-render when props/state change. Expensive computations and callback recreations can cause unnecessary re-renders in child components.

## Solution

Use `useMemo` for expensive computations and `useCallback` for stable callback references.

## Patterns

### useMemo - Memoize Expensive Computations

#### ❌ Bad - Recomputes on Every Render

```tsx
function Component({ data }) {
  // Recalculates even if data hasn't changed
  const expensiveValue = computeExpensiveValue(data);

  return <div>{expensiveValue}</div>;
}
```

#### ✅ Good - Memoize Computation

```tsx
import { useMemo } from "react";

function Component({ data }) {
  // Only recomputes when data changes
  const expensiveValue = useMemo(() => {
    return computeExpensiveValue(data);
  }, [data]);

  return <div>{expensiveValue}</div>;
}
```

### useCallback - Stable Callback References

#### ❌ Bad - New Function Every Render

```tsx
function Parent() {
  const [count, setCount] = useState(0);

  // New function instance on every render
  // Causes Child to re-render even if count hasn't changed
  const handleClick = () => {
    doSomething(count);
  };

  return <Child onClick={handleClick} />;
}
```

#### ✅ Good - Memoize Callback

```tsx
import { useCallback } from "react";

function Parent() {
  const [count, setCount] = useState(0);

  // Same function reference unless count changes
  const handleClick = useCallback(() => {
    doSomething(count);
  }, [count]);

  return <Child onClick={handleClick} />;
}
```

## When to Use

### ✅ Use useMemo when:

- Computation is expensive (> 5ms)
- Value is used as a dependency in other hooks
- Passing computed value to memoized child component
- Filtering/sorting large arrays

### ✅ Use useCallback when:

- Passing callback to memoized child component
- Callback is a dependency in other hooks (useEffect, useMemo)
- Callback is used in event listeners that need cleanup

### ❌ Don't use when:

- Computation is cheap (< 1ms)
- Component always re-renders anyway
- Premature optimization (profile first!)

## Code Splitting Pattern

```tsx
import { lazy, Suspense } from "react";

// Lazy load heavy components
const HeavyComponent = lazy(() => import("./HeavyComponent"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

## Virtual Scrolling for Large Lists

```tsx
import { FixedSizeList } from "react-window";

function LargeList({ items }) {
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={50}
      width="100%"
    >
      {({ index, style }) => <div style={style}>{items[index]}</div>}
    </FixedSizeList>
  );
}
```

## Best Practices

1. **Profile before optimizing** - Use React DevTools Profiler
2. **Avoid inline objects/arrays** - They create new references
3. **Memoize child components** - Use `React.memo()` with callbacks
4. **Dependencies matter** - Always include all used values in deps array
5. **Bundle size** - Use code splitting for routes and heavy components
