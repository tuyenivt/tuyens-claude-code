# Review Output Contract

## Purpose

This template defines the required structure for all review command outputs. Consistent structure enables:

- Predictable consumption by teams
- Automation and tooling integration
- Quality assurance
- Knowledge capture

---

## Required Sections

Every review output MUST include these sections in this order:

### 1. Summary

```markdown
## Summary

[One paragraph executive summary: what was reviewed, overall assessment, and key takeaway. Maximum 4-5 sentences. No bullet points.]
```

**Rules:**

- Plain prose, no formatting
- State the bottom line upfront
- Mention scope explicitly
- Acknowledge limitations if relevant

---

### 2. Scope Reviewed

```markdown
## Scope Reviewed

**Files/Components:** [List what was analyzed]
**Excluded:** [Note what was not analyzed, if relevant]
**Context Provided:** [Summarize user-provided context]
```

**Rules:**

- Be explicit about boundaries
- Note excluded areas to prevent assumptions
- Reference user-provided context

---

### 3. Findings

```markdown
## Findings

### Critical

[Issues requiring immediate attention. Empty section if none.]

### High

[Significant issues to address soon. Empty section if none.]

### Medium

[Issues to address in normal course. Empty section if none.]

### Low

[Minor improvements. Empty section if none.]
```

**Finding Format:**

```markdown
**[Finding Title]**

- **Location:** [File:line or component]
- **Issue:** [What the problem is]
- **Impact:** [Why it matters]
- **Evidence:** [Code snippet or reference]
```

**Rules:**

- Always include all four severity levels (even if empty)
- Each finding has location, issue, and impact
- Use evidence when possible
- Don't recommend fixes in findings section

---

### 4. Risk Assessment

```markdown
## Risk Assessment

| Risk Factor | Status   | Notes     |
| ----------- | -------- | --------- |
| [Factor 1]  | ✅/⚠️/❌ | [Context] |
| [Factor 2]  | ✅/⚠️/❌ | [Context] |
```

**Rules:**

- Use command-specific risk checklist
- Include all risk factors from the command
- Provide brief context for each assessment

---

### 5. Trade-off Analysis

```markdown
## Trade-off Analysis

### [Finding Name]

**Current State:** [Description]

**Option A: [Name]**

- Effort: [Low/Medium/High]
- Risk: [Low/Medium/High]
- Pros: [Bullet list]
- Cons: [Bullet list]

**Option B: [Name]**

- Effort: [Low/Medium/High]
- Risk: [Low/Medium/High]
- Pros: [Bullet list]
- Cons: [Bullet list]

**Human Decision Required:** [What must be decided]
```

**Rules:**

- Include for top 2-3 significant findings
- Always present at least two options
- Never recommend one option over another
- Always include "Human Decision Required"

---

### 6. Human Decisions Required

```markdown
## Human Decisions Required

The following decisions require human judgment and cannot be made by AI:

1. **[Decision]** - [Context and why human input needed]
2. **[Decision]** - [Context and why human input needed]
3. **[Decision]** - [Context and why human input needed]
```

**Rules:**

- Explicit list of all decisions requiring human input
- Brief context for each
- This section is NEVER empty

---

### 7. Suggested Next Steps

```markdown
## Suggested Next Steps

1. [Action item - verb phrase]
2. [Action item - verb phrase]
3. [Action item - verb phrase]
```

**Rules:**

- Actions, not implementations
- Prioritized order
- Measurable when possible
- 3-5 items maximum

---

## Prohibited in Output

Outputs MUST NOT contain:

- [ ] Generated code (more than 1-2 line examples)
- [ ] Specific recommendations without trade-offs
- [ ] Decisions made on behalf of the team
- [ ] Guarantees or certifications
- [ ] Secrets, credentials, or sensitive data
- [ ] Implementation details
- [ ] Tool or vendor recommendations without being asked

---

## Output Quality Checklist

Before completing output, verify:

- [ ] All required sections present
- [ ] Findings categorized by severity
- [ ] Trade-offs presented without recommendation
- [ ] Human decisions explicitly marked
- [ ] No code generation beyond examples
- [ ] Scope limitations acknowledged
- [ ] Uncertainty appropriately expressed

---

## Example Minimal Output

```markdown
## Summary

Reviewed the Order entity and related JPA mappings. Found one critical N+1 query pattern in order item loading that will impact performance at scale. Overall mapping quality is good with minor improvements possible.

## Scope Reviewed

**Files/Components:** Order.java, OrderItem.java, OrderRepository.java
**Excluded:** OrderService transaction management (separate review recommended)
**Context Provided:** Performance degradation reported after adding order history feature

## Findings

### Critical

**N+1 Query in Order Item Loading**

- **Location:** Order.java:45
- **Issue:** @OneToMany items loaded lazily but accessed in loop in OrderMapper
- **Impact:** 1 + N queries per order list request; at 100 orders = 101 queries
- **Evidence:** `@OneToMany(fetch = FetchType.LAZY)` without corresponding JOIN FETCH

### High

[None]

### Medium

**Missing @BatchSize on OrderItem Collection**

- **Location:** Order.java:45
- **Issue:** No batch size hint for collection loading
- **Impact:** If N+1 fix not applied, batch loading would reduce to 1 + ceil(N/batchSize)

### Low

[None]

## Risk Assessment

| Risk Factor                    | Status | Notes                      |
| ------------------------------ | ------ | -------------------------- |
| Production N+1 causing latency | ❌     | Confirmed pattern          |
| Memory exhaustion              | ⚠️     | Possible with large orders |
| Data integrity                 | ✅     | No concerns                |

## Trade-off Analysis

### N+1 Query in Order Item Loading

**Current State:** Lazy loading without fetch optimization

**Option A: JOIN FETCH in Repository**

- Effort: Low
- Risk: Low
- Pros: Simple fix, single query
- Cons: Always loads items even when not needed

**Option B: Entity Graph**

- Effort: Medium
- Risk: Low
- Pros: Flexible, reusable
- Cons: More configuration

**Human Decision Required:** Whether to always load items or use conditional fetching

## Human Decisions Required

1. **Fetch strategy** - Should items always be loaded with orders, or only in specific use cases?
2. **Performance budget** - What's acceptable query count per operation?

## Suggested Next Steps

1. Measure current query count with Hibernate statistics
2. Decide on fetch strategy based on usage patterns
3. Implement chosen fix in development
4. Load test with production-like data volumes
```

---

## Versioning

Contract Version: 1.0.0  
Compatible Commands: All /commands/\*
