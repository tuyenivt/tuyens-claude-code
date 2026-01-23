# Tuyen's Claude Code

Claude Code plugin for Java 21+ / Spring Boot 4 and React development. 12 commands, 8 agents.

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

### Spring Boot (Backend)

| Command          | Purpose             |
| ---------------- | ------------------- |
| `/spring-new`    | Create API endpoint |
| `/spring-review` | Review API code     |
| `/spring-secure` | Security review     |
| `/spring-test`   | Test strategy       |

### React (Frontend)

| Command            | Purpose                |
| ------------------ | ---------------------- |
| `/react-component` | Create React component |
| `/react-page`      | Create React page      |
| `/react-review`    | Review React code      |
| `/react-test`      | Test strategy          |

### General

| Command          | Purpose            |
| ---------------- | ------------------ |
| `/code-review`   | PR review          |
| `/code-cleanup`  | Refactoring plan   |
| `/docs-generate` | Generate docs      |
| `/perf-review`   | Performance review |

## Usage

```
/spring-new
Resource: Order
Package: com.example.order
Operations: CRUD with pagination
```

```
/react-component
Name: OrderList
Props: orders, onSelect
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
