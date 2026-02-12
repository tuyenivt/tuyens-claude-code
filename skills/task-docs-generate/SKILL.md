---
name: task-docs-generate
description: Generate clear, maintainable documentation (README, API docs, ADRs)
metadata:
  category: review
  tags: [documentation, readme, api-docs, adr]
  type: composite
---

# Documentation Generator

## When to Use

- Documentation creation (README, API docs, ADR)
- Documentation review and improvement
- Code comments and JavaDoc
- Runbooks and guides

## Focus Areas

- **Audience**: Who is reading, what they need
- **Purpose**: What readers will accomplish
- **Examples**: Working, tested examples
- **Maintenance**: Can it be kept updated

## Documentation Types

### README

```markdown
# Project Name

Brief description.

## Quick Start

\`\`\`bash
./gradlew bootRun # or npm start
\`\`\`

## Prerequisites

- Java 21+ / Node 18+

## Installation

Step-by-step.

## Usage

Examples.

## Configuration

| Property | Description | Default |
```

### ADR

```markdown
# ADR-NNN: Title

## Status

Proposed | Accepted | Deprecated

## Context

What is the issue?

## Decision

What did we decide?

## Consequences

Positive and negative results.
```

### API Docs

```markdown
# Endpoint Name

**Method:** GET/POST
**Path:** /api/v1/resource

## Request

| Param | Type | Required |

## Response

\`\`\`json
{ "id": 1 }
\`\`\`

## Errors

| Status | Description |
```

## Key Skills Reference

For documentation patterns related to specific domains:

- Use skill: `spring-api` for REST API documentation patterns
- Use skill: `api-guidelines` for API consistency and documentation standards
- Use skill: `coding-standards` for code comment guidelines

## Principles

- Audience first
- Show, don't tell
- Simple words, short sentences
- Examples must work and be tested

## Rules

- Identify audience and purpose before writing
- Structure information clearly with progressive disclosure
- Provide working examples for every concept
- Keep documentation maintainable and close to the code

## Checklist

- [ ] Audience is clear
- [ ] Purpose stated
- [ ] Examples work
- [ ] No jargon without explanation
- [ ] Can be maintained alongside code changes

## Avoid

- Documentation without seeing the code first
- Marketing language in technical docs
- Documenting obvious code (self-documenting is better)
- Stale examples that no longer work
