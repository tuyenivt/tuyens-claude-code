---
name: task-react-component
description: Create new React component with TypeScript following best practices
metadata:
  category: frontend
  tags: [react, typescript, component, accessibility]
  type: workflow
---

# React Component

## When to Use

- Creating new React components with TypeScript
- Scaffolding component files with types, styles, and tests
- Applying component best practices

## Implementation

Generate complete component with:

1. **Component file** - Functional component with TypeScript
2. **Types** - Props interface exported
3. **Styles** - CSS modules or Tailwind
4. **Test file** - Basic test with Testing Library

## Pattern

```tsx
import { type FC } from "react";

interface ComponentProps {
  title: string;
  onClick?: () => void;
  children?: React.ReactNode;
}

export const Component: FC<ComponentProps> = ({ title, onClick, children }) => {
  return (
    <div className="component">
      <h2>{title}</h2>
      {children}
      {onClick && <button onClick={onClick}>Action</button>}
    </div>
  );
};
```

## Key Skills Reference

For detailed patterns, reference these skills:

- Use skill: `react-component-design` for composition patterns and props design
- Use skill: `react-state-management` for managing component state
- Use skill: `react-memoization` for optimization (only when beneficial)

## Rules

- TypeScript strict mode compatible
- Props interface exported
- Accessible (ARIA attributes, semantic HTML)
- Handle loading and error states
- Memoize only when beneficial (not by default)

## Checklist

- [ ] TypeScript strict mode compatible
- [ ] Props interface exported
- [ ] Memoization where beneficial
- [ ] Accessible (ARIA, semantic HTML)
- [ ] Loading and error states
- [ ] TODO for customization

## Avoid

- Inline styles (use CSS modules or Tailwind)
- Over-memoization without profiling
- Missing accessibility attributes
- Props interfaces not exported
