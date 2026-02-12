# Tuyen's Claude Code

Claude Code plugin for Java 21+ / Spring Boot 3.5+ and React development. 10 agents, 30 skills (9 composite + 21 atomic).

## Installation

```bash
# Add marketplace
/plugin marketplace add tuyenivt/tuyens-claude-code

# Install
/plugin install tuyens-claude-code
```

## Optional: Share Skills Between Claude Code and Codex

Claude Code and Codex use the same `agentskills.io` format. You can create a symbolic link so Codex reuses the skills managed by Claude Code.
This is useful because Codex does not automatically update skills, while Claude Code does.

```bash
# Unix (Linux/macOS)
ln -s "$HOME/.claude/plugins/marketplaces/tuyens-claude-code/skills" "$HOME/.codex/skills/tuyens-claude-code-skills"

# Windows
mklink /J "%USERPROFILE%\.codex\skills\tuyens-claude-code-skills" "%USERPROFILE%\.claude\plugins\marketplaces\tuyens-claude-code\skills"
```

## Requirements

- Claude Code >= 2.0.0
- Backend: Java 21+, Spring Boot 3.5+ (Spring Boot 4 best-effort), JPA, Gradle
- Frontend: React, TypeScript

## Key Features

- **Virtual Threads**: All skills enforce Virtual Thread compatibility (no `synchronized`)
- **Java 21+ Patterns**: Records for DTOs, pattern matching, sealed classes
- **Spring Boot 3.5+**: Jakarta EE 10 (EE 11 for Spring Boot 4), optimized connection pools (10-40)

## Composite Skills (Task Workflows)

Composite skills (`task-*`) orchestrate multiple atomic skills into task-oriented workflows. They are invoked as slash commands.

### Framework-Specific

| Skill                   | Purpose                     | Agent              |
| ----------------------- | --------------------------- | ------------------ |
| `task-spring-new`       | Create Spring Boot endpoint | `spring-architect` |
| `task-react-component`  | Create React component      | `react-architect`  |
| `task-react-page`       | Create React page           | `react-architect`  |

### Framework-Aware (Auto-detect)

| Skill                 | Purpose                     | Agent                  |
| --------------------- | --------------------------- | ---------------------- |
| `task-code-review`    | Code review (any framework) | `tech-lead`            |
| `task-code-secure`    | Security review             | `security-engineer`    |
| `task-code-test`      | Test strategy               | `test-engineer`        |
| `task-code-refactor`  | Refactoring plan            | `refactoring-expert`   |
| `task-perf-review`    | Performance review          | `performance-engineer` |
| `task-docs-generate`  | Generate documentation      | `technical-writer`     |

## Usage Examples

**Create Spring Boot endpoint:**

```
/task-spring-new
Resource: Order
Package: com.example.order
Operations: CRUD with pagination
```

**Create React component:**

```
/task-react-component
Name: OrderList
Props: orders, onSelect
```

**Review code (auto-detects framework):**

```
/task-code-review
[paste code or file path]
```

**Security review (works with any framework):**

```
/task-code-secure
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

## Atomic Skills (Reusable Patterns)

21 atomic skills organized by category provide focused, reusable patterns. These are referenced by composite skills and agents -- they are not intended for independent use.

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
- Clear separation: composite skills orchestrate workflows, atomic skills provide implementation patterns

## License

MIT
