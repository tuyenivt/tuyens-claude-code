---
description: Test strategy and scaffolds for Spring Boot APIs
model: claude-sonnet-4-5
---

Evaluate and generate test strategy for Spring Boot APIs.

## Context

$ARGUMENTS

## Testing Pyramid

```
    /\      E2E (few)
   /--\     Integration (some)
  /----\    Unit (many)
```

## Test Patterns

### Unit Test

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

### Integration Test

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

## Checklist

- [ ] Clear test names (should_X_when_Y)
- [ ] Arrange-Act-Assert pattern
- [ ] Edge cases covered
- [ ] Error paths tested
- [ ] No test interdependencies

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
