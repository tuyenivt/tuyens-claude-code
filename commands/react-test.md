---
description: Test strategy and scaffolds for React components
model: claude-sonnet-4-5
---

Evaluate and generate test strategy for React components.

## Context

$ARGUMENTS

## Testing Philosophy

Test behavior, not implementation. Tests should resemble user interaction.

## Test Patterns

### Component Test

```tsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";

describe("Component", () => {
  it("calls onClick when button clicked", async () => {
    const handleClick = vi.fn();
    render(<Component onClick={handleClick} />);

    await userEvent.click(screen.getByRole("button"));

    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### Hook Test

```tsx
import { renderHook, act } from "@testing-library/react";

it("increments counter", () => {
  const { result } = renderHook(() => useCounter());
  act(() => result.current.increment());
  expect(result.current.count).toBe(1);
});
```

## Query Priority

1. `getByRole` - Accessible
2. `getByLabelText` - Forms
3. `getByText` - Content
4. `getByTestId` - Last resort

## Checklist

- [ ] User interactions tested
- [ ] Loading/error states verified
- [ ] Accessibility assertions
- [ ] No implementation details tested

## Output

```markdown
## Assessment

**Coverage:** [X%]
**Quality:** [status]

## Gaps

| Component | Missing Tests |

## Recommended Tests

[Test cases to add]
```
