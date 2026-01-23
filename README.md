# Tuyen's Claude Code

Claude Code plugin for Java Spring and React development. 12 commands, 8 agents.

## Installation

```bash
# Add from marketplace
/plugin add https://github.com/tuyenivt/tuyens-claude-code

# Install locally
/plugin install tuyens-claude-code
```

## Requirements

- Claude Code >= 2.0.0
- Backend: Java 21+, Spring Boot, JPA, Gradle
- Frontend: React, TypeScript

## Commands

### API (Backend)

| Command       | Purpose             |
| ------------- | ------------------- |
| `/api-new`    | Create API endpoint |
| `/api-review` | Review API code     |
| `/api-secure` | Security review     |
| `/api-test`   | Test strategy       |

### UI (Frontend)

| Command         | Purpose                |
| --------------- | ---------------------- |
| `/ui-component` | Create React component |
| `/ui-page`      | Create React page      |
| `/ui-review`    | Review React code      |
| `/ui-test`      | Test strategy          |

### General

| Command          | Purpose            |
| ---------------- | ------------------ |
| `/code-review`   | PR review          |
| `/code-cleanup`  | Refactoring plan   |
| `/docs-generate` | Generate docs      |
| `/perf-review`   | Performance review |

## Usage

```
/api-new
Resource: Order
Package: com.example.order
Operations: CRUD with pagination
```

```
/ui-component
Name: OrderList
Props: orders, onSelect
```

## Agents

| Agent                  | Focus                  |
| ---------------------- | ---------------------- |
| `backend-architect`    | Spring Boot, JPA, APIs |
| `frontend-architect`   | React, accessibility   |
| `security-engineer`    | OWASP, auth            |
| `performance-engineer` | Optimization           |
| `test-engineer`        | Testing strategy       |
| `refactoring-expert`   | Code cleanup           |
| `technical-writer`     | Documentation          |
| `tech-lead`            | Code review            |

## License

MIT
