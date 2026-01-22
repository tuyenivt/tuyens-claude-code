# Tuyen's Claude Code

A Claude Code plugin for enterprise Java backend teams. Provides opinionated, review-focused slash commands for Spring Boot, JPA/Hibernate, and Java 21+ development.

## What This Plugin Does

This plugin standardizes how Java teams interact with Claude Code. It provides slash commands that:

- Review architecture, not generate it
- Identify problems, not fix them automatically
- Require human decisions at critical points
- Enforce governance and security policies

## What This Plugin Does NOT Do

This plugin will refuse to:

- Generate entire applications or modules
- Scaffold full classes automatically
- Make architectural or transactional decisions for you
- Guess when context is missing
- Store, infer, or expose secrets

## Installation

```bash
# Add from marketplace
/plugin marketplace add https://github.com/tuyenivt/tuyens-claude-code

# Install locally
/plugin install tuyens-claude-code
```

## Requirements

- Claude Code >= 1.0.0
- Target codebase: Java 21+, Spring Boot, JPA/Hibernate, Gradle

## Slash Commands

| Command                | Purpose                                             |
| ---------------------- | --------------------------------------------------- |
| `/architecture-review` | Review layering, coupling, and component boundaries |
| `/domain-model-review` | Analyze DDD alignment and aggregate design          |
| `/api-design-review`   | Audit REST API design and HTTP semantics            |
| `/jpa-review`          | Find N+1 queries, fetch issues, cascade problems    |
| `/transaction-review`  | Analyze transaction boundaries and isolation        |
| `/performance-review`  | Identify memory, concurrency, and JVM issues        |
| `/security-review`     | Find injection, auth, and data exposure risks       |
| `/test-strategy`       | Evaluate testing pyramid and coverage gaps          |

## Usage Examples

### Architecture Review

```
/architecture-review

Context: We have a Spring Boot monolith with 50+ services.
I want to review the payment processing module.

Files to review:
- src/main/java/com/example/payment/
```

### JPA Review

```
/jpa-review

Entity: Order.java with OrderItem collection
Problem: API response times degraded after adding order history feature
```

### Security Review

```
/security-review

Scope: User authentication and session management
Recent changes: Added OAuth2 integration
Compliance: SOC2, GDPR
```

## Command Output Structure

All commands produce structured output:

1. **Summary** - One-paragraph assessment
2. **Findings** - Categorized issues with severity
3. **Risk Checklist** - Binary yes/no risk indicators
4. **Trade-offs** - Options with pros/cons (no recommendations)
5. **Human Decisions Required** - Explicit list of choices only humans can make
6. **Next Steps** - Suggested follow-up actions (not implementations)

## Team Rules

### Always Required

- Provide file paths or paste code directly
- State the specific concern or goal
- Mention compliance requirements if applicable
- Review AI output before acting on it

### Never Allowed

- Sharing production credentials or secrets
- Asking Claude to implement fixes directly
- Bypassing human review markers
- Using output without team review for critical systems

## Governance Policies

This plugin enforces three policies:

1. **Usage Policy** - What Claude will and will not do
2. **Security Policy** - How sensitive data is handled
3. **Cost Policy** - Scope limits per request

Read the full policies in the `/policies` directory.

## Agents (Personas)

Commands use specialized agent personas:

| Agent                       | Expertise                                     |
| --------------------------- | --------------------------------------------- |
| `spring-architect`          | Spring Boot architecture, DI, modularity      |
| `jpa-specialist`            | Hibernate, entity mapping, query optimization |
| `java-performance-engineer` | JVM tuning, memory, concurrency               |
| `security-engineer`         | OWASP, authentication, authorization          |
| `test-engineer`             | Testing strategy, coverage, test design       |
| `tech-lead-reviewer`        | Holistic review, team standards               |

## License

MIT
