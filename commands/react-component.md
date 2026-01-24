---
description: Create new React component with TypeScript
model: claude-sonnet-4-5
---

Create a new React component with TypeScript following best practices.

## Requirements

$ARGUMENTS

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

## Checklist

- TypeScript strict mode compatible
- Props interface exported
- Memoization where beneficial
- Accessible (ARIA, semantic HTML)
- Loading and error states
- TODO for customization
