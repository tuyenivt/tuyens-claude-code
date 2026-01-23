---
description: Test strategy and scaffolds for any framework
model: claude-sonnet-4-5
---

Evaluate and generate test strategy for your codebase (framework-aware).

## Context

$ARGUMENTS

## Testing Pyramid

```
    /\      E2E (few)
   /--\     Integration (some)
  /----\    Unit (many)
```

## Framework-Specific Patterns

### Backend (Java/Spring Boot)

**Unit Test:**

```java
@ExtendWith(MockitoExtension.class)
class ServiceTest {
    @Mock Repository repo;
    @InjectMocks Service service;

    @Test
    void should_X_when_Y() {
        // Given - Arrange
        // When - Act
        // Then - Assert
    }
}
```

**Integration Test:**

```java
@SpringBootTest
@AutoConfigureMockMvc
@Transactional
class ControllerTest {
    @Autowired MockMvc mockMvc;

    @Test
    void should_return_200() throws Exception {
        mockMvc.perform(get("/api/v1/resource"))
            .andExpect(status().isOk());
    }
}
```

### Frontend (React)

**Component Test:**

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

**Hook Test:**

```tsx
import { renderHook, act } from "@testing-library/react";

it("increments counter", () => {
  const { result } = renderHook(() => useCounter());
  act(() => result.current.increment());
  expect(result.current.count).toBe(1);
});
```

### React Query Priority

1. `getByRole` - Accessible
2. `getByLabelText` - Forms
3. `getByText` - Content
4. `getByTestId` - Last resort

## Universal Checklist

- [ ] Clear test names (describe behavior)
- [ ] Arrange-Act-Assert pattern
- [ ] Edge cases covered
- [ ] Error paths tested
- [ ] No test interdependencies
- [ ] Fast feedback (tests run quickly)

## Backend-Specific Checklist

- [ ] Integration tests use transactions
- [ ] No flaky tests (proper cleanup)
- [ ] Database tests isolated

## Frontend-Specific Checklist

- [ ] User interactions tested
- [ ] Loading/error states verified
- [ ] Accessibility assertions
- [ ] No implementation details tested

## Output

```markdown
## Assessment

**Coverage:** [X%]
**Pyramid Balance:** [status]

## Gaps

| Area | Missing Tests | Priority |

## Recommended Tests

[Test cases to add]
```
