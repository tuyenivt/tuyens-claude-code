# Tuyen's Claude Code

Claude Code plugin for Java 21+ / Spring Boot 3.5+ and React development. 11 agents, 45 skills (12 workflow + 33 atomic).

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

## Workflow Skills

Workflow skills (`task-*`) orchestrate multiple atomic skills into task-oriented workflows. They are invoked as slash commands.

### Framework-Specific

| Skill                  | Purpose                     | Agent              |
| ---------------------- | --------------------------- | ------------------ |
| `task-spring-new`      | Create Spring Boot endpoint | `spring-architect` |
| `task-react-component` | Create React component      | `react-architect`  |
| `task-react-page`      | Create React page           | `react-architect`  |

### Framework-Aware (Auto-detect)

| Skill                       | Purpose                                                                    | Agent                  |
| --------------------------- | -------------------------------------------------------------------------- | ---------------------- |
| `task-code-review`          | Code review (any framework)                                                | `tech-lead`            |
| `task-code-review-advanced` | Staff-level review with risk assessment (scope: core/+perf/+security/full) | `tech-lead`            |
| `task-code-secure`          | Security review                                                            | `security-engineer`    |
| `task-code-test`            | Test strategy                                                              | `test-engineer`        |
| `task-code-refactor`        | Refactoring plan                                                           | `refactoring-expert`   |
| `task-perf-review`          | Performance review                                                         | `performance-engineer` |
| `task-docs-generate`        | Generate documentation                                                     | `technical-writer`     |
| `task-root-cause`           | Incident root cause analysis                                               | `reliability-engineer` |
| `task-postmortem`           | Post-incident postmortem and prevention                                    | `reliability-engineer` |

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

**Review code (basic, auto-detects framework):**

```
/task-code-review
[paste code or file path]
```

**Staff-level review (risk assessment, architecture guardrails, quality):**

```
/task-code-review-advanced
[paste code or file path]
```

Scope options -- asks interactively if not specified:

```
/task-code-review-advanced +perf      # Core + performance review
/task-code-review-advanced +security  # Core + security review
/task-code-review-advanced full       # Core + performance + security
```

**Incident root cause analysis:**

```
/task-root-cause
[paste stack trace, logs, or error message]
```

**Post-incident postmortem:**

```
/task-postmortem
[paste incident summary and root cause analysis output]
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
| `reliability-engineer` | Incident analysis and prevention   |

## Atomic Skills (Reusable Patterns)

33 atomic skills organized by category provide focused, reusable patterns. These are hidden from the slash menu (`user-invocable: false`) and referenced only by workflow skills and agents.

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

### Ops (7)

Note: 4 of these are sub-skills defined by `task-root-cause`.

| Skill                          | Purpose                                                 |
| ------------------------------ | ------------------------------------------------------- |
| `observability`                | Logging, metrics, tracing                               |
| `resiliency`                   | Timeout, retry, circuit breaker                         |
| `safe-file-operations`         | Cross-platform shell and file operations                |
| `failure-classification`       | Classify failures by type, mechanism, and system layer  |
| `failure-propagation-analysis` | Trace cascading failure paths across system boundaries  |
| `root-cause-hypothesis`        | Generate ranked hypotheses with confidence and evidence |
| `incident-prevention`          | Systemic prevention strategies tied to failure classes  |

### Integration (2)

| Skill              | Purpose              |
| ------------------ | -------------------- |
| `idempotency`      | Idempotency patterns |
| `rest-integration` | REST client patterns |

### Governance (10)

| Skill                    | Purpose                                                 |
| ------------------------ | ------------------------------------------------------- |
| `api-guidelines`         | API consistency standards                               |
| `coding-standards`       | Code style and structure                                |
| `pr-risk-analysis`       | Lightweight heuristic PR risk classification            |
| `blast-radius-analysis`  | Failure propagation and change impact scope             |
| `architecture-guardrail` | Layer violation and boundary erosion detection          |
| `ai-verbosity-control`   | AI-generated over-engineering and verbosity detection   |
| `complexity-compression` | Simplification opportunity identification               |
| `review-gap-analysis`    | Analyze why review processes missed a failure           |
| `guardrail-evolution`    | Evolve guardrails based on incident learnings           |
| `engineering-governance` | Engineering process and governance improvement patterns |

**Benefits:**

- Single source of truth for patterns
- Reduced duplicated content through skill composition
- Easy maintenance (update once, affects all)
- Clear separation: workflow skills orchestrate multi-step tasks, atomic skills provide implementation patterns

## License

MIT
