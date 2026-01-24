---
name: react-component-design
description: Component composition and single responsibility principle
category: frontend
tags: [react, components, design, composition]
---

# React Component Design

## When to Use

- Creating reusable, maintainable React components
- Organizing component hierarchy
- Reducing code duplication and prop drilling

## Rules

- Keep components small and focused
- One responsibility per component
- Prefer composition over inheritance
- Explicit and typed props
- Avoid excessive prop drilling (use context for deeply nested props)
- Deterministic render output, no side effects during render
- Extract reusable components early
- Avoid copy-paste components

## Pattern

❌ Bad - Multiple responsibilities, prop drilling:

```jsx
function UserCard({ user, onUpdate, onDelete, theme, notifications }) {
  return (
    <div className={theme}>
      <h1>{user.name}</h1>
      <button onClick={() => onUpdate(user)}>Edit</button>
      <button onClick={() => onDelete(user.id)}>Delete</button>
      <Notifications data={notifications} />
    </div>
  );
}
```

✅ Good - Separated concerns:

```jsx
function UserCard({ user, onEdit, onDelete }) {
  return (
    <article>
      <h1>{user.name}</h1>
      <UserActions onEdit={onEdit} onDelete={onDelete} userId={user.id} />
    </article>
  );
}
```

## Avoid

- God components with multiple responsibilities
- Deep prop drilling (use context instead)
- Render side effects or complex logic during render
