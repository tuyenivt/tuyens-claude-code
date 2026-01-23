---
description: Generate documentation - README, API docs, ADRs
model: claude-sonnet-4-5
---

Generate clear, maintainable documentation.

## Requirements

$ARGUMENTS

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

## Checklist

- Audience is clear
- Purpose stated
- Examples work
- No jargon without explanation
