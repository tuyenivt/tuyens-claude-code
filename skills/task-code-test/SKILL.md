---
name: task-code-test
description: Test strategy, scaffolds, and quality review for any framework
metadata:
  category: review
  tags: [testing, test-strategy, unit-test, integration-test]
  type: composite
---

# Code Test

## When to Use

- Test coverage evaluation
- Testing strategy design
- Test quality review
- Test pyramid balance assessment
- Generating test scaffolds

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

**Note:** `@MockBean` is deprecated since Spring Boot 3.4.0 and marked for removal. Prefer `@MockitoBean` from `org.springframework.test.context.bean.override.mockito.MockitoBean` (available from Spring Boot 3.2+, standard in 3.5+ and Spring Boot 4):

```java
@SpringBootTest
class ServiceIntegrationTest {
    @MockitoBean  // Preferred: Spring Boot 3.2+, 3.5, and Spring Boot 4
    ExternalService externalService;

    @Autowired
    OrderService orderService;

    @Test
    void should_process_order() {
        when(externalService.validate()).thenReturn(true);
        var result = orderService.process(new Order());
        assertThat(result.isSuccess()).isTrue();
    }
}
```

**Integration Test with Testcontainers:**

```java
// 1. Create reusable TestcontainersConfiguration (src/test/java)
@TestConfiguration(proxyBeanMethods = false)
public class TestcontainersConfiguration {

    @Bean
    @ServiceConnection
    PostgreSQLContainer<?> postgresContainer() {
        return new PostgreSQLContainer<>(DockerImageName.parse("postgres:16-alpine"));
    }

    @Bean
    @ServiceConnection
    GenericContainer<?> redisContainer() {
        return new GenericContainer<>(DockerImageName.parse("redis:7-alpine"))
            .withExposedPorts(6379);
    }
}

// 2. Use in tests via @Import
@SpringBootTest
@Import(TestcontainersConfiguration.class)
@Transactional
class OrderRepositoryTest {
    @Autowired OrderRepository repository;

    @Test
    void should_save_order() {
        var order = new Order("item", 100);
        var saved = repository.save(order);
        assertThat(saved.getId()).isNotNull();
    }
}

// 3. For custom containers not in TestcontainersConfiguration, use @Container directly
@SpringBootTest
@Testcontainers
class CustomContainerTest {
    @Container
    static KafkaContainer kafka = new KafkaContainer(DockerImageName.parse("confluentinc/cp-kafka:7.5.0"));
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
- [ ] Testcontainers: Use shared `TestcontainersConfiguration` class with `@Import` (avoid `@Container` per test class unless custom container needed)

## Frontend-Specific Checklist

- [ ] User interactions tested
- [ ] Loading/error states verified
- [ ] Accessibility assertions
- [ ] No implementation details tested

## Key Skills Reference

For detailed testing patterns, reference these skills:

- Use skill: `coding-standards` for test naming and structure
- Use skill: `spring-api` for API test patterns
- Use skill: `react-component-design` for component test strategies

## Principles

- Test behavior, not implementation
- Fast feedback is essential
- Tests are specifications
- Pyramid over ice cream cone

## Rules

- Every test must have a clear purpose
- Use Arrange-Act-Assert consistently
- No test interdependencies
- Do not aim for 100% coverage as a goal -- focus on business value
- Consider maintenance cost of each test

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

## Avoid

- Testing implementation details instead of behavior
- Flaky tests that depend on timing or external state
- Over-mocking (test nothing meaningful)
- Ignoring test maintenance cost
