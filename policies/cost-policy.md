# Cost Policy

## Purpose

This policy defines scope limits and usage patterns to ensure efficient use of AI resources. These guidelines help teams get maximum value while controlling costs and maintaining quality.

---

## Core Cost Principles

1. **Focused requests** - One concern per command invocation
2. **Minimal context** - Share only code relevant to the question
3. **Slash commands first** - Use structured commands over freeform chat
4. **Iterative refinement** - Start narrow, expand if needed
5. **Human judgment** - Don't use AI when human expertise is faster

---

## Scope Limits Per Request

### Recommended Scope Sizes

| Command                | Recommended Scope            | Too Broad          |
| ---------------------- | ---------------------------- | ------------------ |
| `/architecture-review` | 1 module, 5-10 classes       | Entire monolith    |
| `/domain-model-review` | 1 aggregate, 3-5 entities    | All domain objects |
| `/api-design-review`   | 1 controller, 5-10 endpoints | All APIs           |
| `/jpa-review`          | 1-3 related entities         | All entities       |
| `/transaction-review`  | 1 service class              | All services       |
| `/performance-review`  | 1 hot path, 3-5 methods      | Entire codebase    |
| `/security-review`     | 1 feature area               | All security       |
| `/test-strategy`       | 1 module's tests             | All tests          |

### Signs Your Request Is Too Broad

- Asking to review "everything"
- Including more than 10 files
- Combining multiple concerns (security AND performance AND architecture)
- Request context exceeds reasonable prompt size
- You can't articulate a specific question

---

## Efficient Usage Patterns

### DO: Use Slash Commands

Slash commands are optimized for specific tasks:

```bash
# Good - Specific command, focused scope
/jpa-review src/main/java/com/example/order/Order.java

# Good - Clear concern
/performance-review --concern "slow checkout flow"
```

### DON'T: Freeform Broad Requests

```bash
# Bad - Too vague
"Review my code"

# Bad - Multiple concerns
"Check my code for performance, security, and architecture issues"

# Bad - No scope
"Are there any problems with my application?"
```

---

## When NOT to Use AI

### Use Human Expertise Instead

| Scenario                 | Better Approach       |
| ------------------------ | --------------------- |
| Quick syntax question    | IDE or documentation  |
| Team convention question | Ask a teammate        |
| Simple refactoring       | IDE refactoring tools |
| Known bug fix            | Debug directly        |
| Business logic question  | Domain expert         |

### Use AI When

- Reviewing unfamiliar patterns
- Getting a second opinion on design
- Learning best practices
- Identifying issues you might miss
- Getting structured analysis

---

## Request Optimization

### Before Invoking a Command

1. **Define the question** - What specific question do you want answered?
2. **Identify the scope** - What code is relevant to the question?
3. **Remove noise** - Strip out unrelated code
4. **Redact secrets** - Remove credentials and sensitive data
5. **Choose one command** - Pick the most relevant command

### During the Review

1. **Start narrow** - Begin with smallest relevant scope
2. **Iterate if needed** - Expand scope based on findings
3. **Ask follow-ups** - Clarify specific points
4. **Don't repeat** - Don't re-ask the same question

---

## Batching Strategy

### Efficient Batching

When reviewing multiple concerns:

```bash
# Good - Sequential focused reviews
/architecture-review src/main/java/com/example/order/
# Review output, then:
/jpa-review src/main/java/com/example/order/entities/
# Review output, then:
/test-strategy src/test/java/com/example/order/
```

### Inefficient Batching

```bash
# Bad - Trying to do everything at once
/architecture-review AND /jpa-review AND /security-review on the entire codebase
```

---

## Cost Indicators

### Signs You're Using AI Efficiently

- Commands complete quickly with actionable output
- Findings are specific and relevant
- Human review time is productive
- Follow-up questions are clarifications, not restarts

### Signs You're Wasting Resources

- Commands timing out or truncating
- Output is generic and not actionable
- Repeated requests for the same concern
- Human must heavily filter irrelevant findings

---

## Team Usage Guidelines

### Establish Team Norms

1. **Review threshold** - When should AI review be used?
2. **Scope guidelines** - What's an appropriate request size?
3. **Sharing findings** - How are AI insights shared?
4. **Learning capture** - How do we learn from AI feedback?

### Track Value, Not Volume

Measure AI assistance by:

- Issues caught before production
- Time saved in code review
- Learning and skill development
- Quality improvement

Not by:

- Number of commands run
- Lines of code reviewed
- Hours spent with AI

---

## Resource Budgeting

### Per-Developer Guidelines

Suggested limits (adjust for your team):

| Period     | Suggested Limit | Notes                                   |
| ---------- | --------------- | --------------------------------------- |
| Per day    | 10-20 commands  | More for learning, less for experienced |
| Per PR     | 2-5 commands    | Focus on highest-risk areas             |
| Per sprint | Review patterns | Ensure value, not habit                 |

### Team Review

Periodically assess:

- Are we getting value from AI assistance?
- Are requests appropriately scoped?
- Could we answer this faster ourselves?
- Are we learning or just offloading?

---

## Refusal for Scope

Claude WILL refuse requests that:

- Ask to review "the entire codebase"
- Combine more than one command's concern
- Provide no specific code to review
- Cannot articulate a specific question
- Request generation instead of review

Claude WILL instead:

- Suggest appropriate scope
- Recommend which command to use
- Ask clarifying questions
- Propose a focused starting point

---

## Version

Policy Version: 1.0.0
Last Updated: 202601
