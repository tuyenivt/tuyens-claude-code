# Tuyen's Claude Code

Claude Code plugin for Java 21+ / Spring Boot 3.5+ and React development. 9 commands, 10 agents, 21 skills.

## Installation

```bash
# Add marketplace
/plugin marketplace add tuyenivt/tuyens-claude-code

# Install
/plugin install tuyens-claude-code
```

## Requirements

- Claude Code >= 2.0.0
- Backend: Java 21+, Spring Boot 3.5+ (Spring Boot 4 best-effort), JPA, Gradle
- Frontend: React, TypeScript

## Key Features

- **Virtual Threads**: All commands enforce Virtual Thread compatibility (no `synchronized`)
- **Java 21+ Patterns**: Records for DTOs, pattern matching, sealed classes
- **Spring Boot 3.5+**: Jakarta EE 10 (EE 11 for Spring Boot 4), optimized connection pools (10-40)

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

| Agent                  | Focus                              |
| ---------------------- | ---------------------------------- |
| `spring-architect`     | Spring Boot, JPA, APIs             |
| `spring-performance`   | Spring Boot performance and tuning |
| `react-architect`      | React, accessibility               |
| `react-performance`    | React rendering optimization       |
| `security-engineer`    | OWASP, auth                        |
| `performance-engineer` | Optimization (multi-lang)          |
| `test-engineer`        | Testing strategy                   |
| `refactoring-expert`   | Code cleanup                       |
| `technical-writer`     | Documentation                      |
| `tech-lead`            | Code review (multi-lang)           |

## Skills (Reusable Patterns)

21 skills organized by category provide focused, reusable patterns that agents and commands reference.

### Backend (7)

| Skill                 | Purpose                             |
| --------------------- | ----------------------------------- |
| `async-processing`    | Async processing with Spring @Async |
| `exception-handling`  | Centralized exception handling      |
| `jpa-performance`     | JPA optimization and N+1 prevention |
| `spring-api`          | REST API best practices             |
| `transaction`         | Transaction management              |
| `virtual-thread-lock` | Virtual Thread-safe locking         |
| `websocket-spring`    | WebSocket and STOMP messaging       |

### Frontend (4)

| Skill                    | Purpose                          |
| ------------------------ | -------------------------------- |
| `react-component-design` | Component composition and SRP    |
| `react-memoization`      | useMemo and useCallback patterns |
| `react-state-management` | State scope and ownership        |
| `websocket-react`        | WebSocket hooks and STOMP client |

### Performance (3)

| Skill                  | Purpose                         |
| ---------------------- | ------------------------------- |
| `caching`              | Cache strategy and invalidation |
| `db-indexing`          | Database indexing strategy      |
| `payload-optimization` | API response optimization       |

### Ops (3)

| Skill                  | Purpose                                  |
| ---------------------- | ---------------------------------------- |
| `observability`        | Logging, metrics, tracing                |
| `resiliency`           | Timeout, retry, circuit breaker          |
| `safe-file-operations` | Cross-platform shell and file operations |

### Integration (2)

| Skill              | Purpose              |
| ------------------ | -------------------- |
| `idempotency`      | Idempotency patterns |
| `rest-integration` | REST client patterns |

### Governance (2)

| Skill              | Purpose                   |
| ------------------ | ------------------------- |
| `api-guidelines`   | API consistency standards |
| `coding-standards` | Code style and structure  |

**Benefits:**

- Single source of truth for patterns
- 86% reduction in duplicated content (~3,700 tokens saved)
- Easy maintenance (update once, affects all)
- Clear separation: agents/commands provide guidance, skills provide implementation patterns

## License

MIT
