# Tuyen's Claude Code

Claude Code plugin for Java 21+ / Spring Boot 4 and React development. 9 commands, 8 agents.

## Installation

```bash
# Add from marketplace
/plugin add https://github.com/tuyenivt/tuyens-claude-code

# Install locally
/plugin install tuyens-claude-code
```

## Requirements

- Claude Code >= 2.0.0
- Backend: Java 21+, Spring Boot 4, JPA, Gradle
- Frontend: React, TypeScript

## Key Features

- **Virtual Threads**: All commands enforce Virtual Thread compatibility (no `synchronized`)
- **Java 21+ Patterns**: Records for DTOs, pattern matching, sealed classes
- **Spring Boot 4**: Jakarta EE 11, optimized connection pools (10-40)

## Commands

### Framework-Specific Commands

| Command            | Purpose                     | Agent              |
| ------------------ | --------------------------- | ------------------ |
| `/spring-new`      | Create Spring Boot endpoint | `spring-architect` |
| `/react-component` | Create React component      | `react-architect`  |
| `/react-page`      | Create React page           | `react-architect`  |

### Framework-Aware Commands (Auto-detect)

| Command          | Purpose                     | Agent                  |
| ---------------- | --------------------------- | ---------------------- |
| `/code-review`   | Code review (any framework) | `tech-lead`            |
| `/code-secure`   | Security review             | `security-engineer`    |
| `/code-test`     | Test strategy               | `test-engineer`        |
| `/code-refactor` | Refactoring plan            | `refactoring-expert`   |
| `/perf-review`   | Performance review          | `performance-engineer` |
| `/docs-generate` | Generate documentation      | `technical-writer`     |

## Usage Examples

**Create Spring Boot endpoint:**

```
/spring-new
Resource: Order
Package: com.example.order
Operations: CRUD with pagination
```

**Create React component:**

```
/react-component
Name: OrderList
Props: orders, onSelect
```

**Review code (auto-detects framework):**

```
/code-review
[paste code or file path]
```

**Security review (works with any framework):**

```
/code-secure
[paste code or file path]
```

## Agents

| Agent                  | Focus                     |
| ---------------------- | ------------------------- |
| `spring-architect`     | Spring Boot, JPA, APIs    |
| `react-architect`      | React, accessibility      |
| `security-engineer`    | OWASP, auth               |
| `performance-engineer` | Optimization (multi-lang) |
| `test-engineer`        | Testing strategy          |
| `refactoring-expert`   | Code cleanup              |
| `technical-writer`     | Documentation             |
| `tech-lead`            | Code review (multi-lang)  |

## License

MIT
