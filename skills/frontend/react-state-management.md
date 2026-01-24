---
name: react-state-management
description: State scope and ownership patterns
category: frontend
tags: [react, state, context, hooks]
---

# React State Management

## When to Use

- Managing component state, global state, and server state
- Determining appropriate state scope and ownership
- Avoiding duplication and sync issues

## Rules

- Default to local state for UI-only state
- Keep state close to where it's used
- Use context only for cross-feature state
- Keep global state minimal and normalized
- Treat server state as a cache, never duplicate locally
- Clear ownership of state
- Avoid derived state unless necessary

## Pattern

❌ Bad - Global state for everything, duplicated server state:

```jsx
const globalStore = {
  user: userData, // Duplicated from server
  isLoading: true, // UI state in global
  formInput: "", // Local UI in global
};
```

✅ Good - Appropriate scope for each:

```jsx
// Server state: React Query/SWR handles caching
const { data: user } = useQuery(["user"], fetchUser);

// Global: Cross-feature, normalized
const { theme } = useContext(ThemeContext);

// Local: UI-only state
const [isOpen, setIsOpen] = useState(false);
```

## Avoid

- Storing server data in global state
- Mixing UI state with app state
- Over-using context for everything
- Derived state without clear reason
