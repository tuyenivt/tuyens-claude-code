---
name: task-pr-prepare
description: Prepare a pull request for review. Generates conventional commit messages, PR title and description with summary of changes, testing notes, and migration notes. Validates code compiles, tests pass, and no obvious issues exist before submitting.
metadata:
  category: review
  tags: [pull-request, commit, pr-description, workflow]
  type: workflow
agent: tech-lead
---

# PR Prepare

## When to Use

- Before submitting a pull request
- When you need well-structured commit messages
- When preparing a PR description with context for reviewers
- Pre-submit validation of code quality

## Workflow

### Step 1 — Analyze Changes

- Run: `git diff --stat` to see changed files
- Run: `git diff` to see actual changes
- Categorize changes: feature, bugfix, refactor, chore, docs, test
- Identify the scope: which module/package is affected

### Step 2 — Generate Commit Message(s)

Follow Conventional Commits format:

```
type(scope): description

Body: what and why (not how — the code shows how)

Footer: BREAKING CHANGE if applicable, ticket reference
```

**Types:** `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `perf`, `ci`

**Scope:** module or package name

Good:

```
feat(order): add payment integration

Integrate Stripe payment gateway for order checkout.
Supports credit card and bank transfer methods.

Refs: PROJ-1234
```

Bad:

```
update code
```

**Splitting large changes** — suggest multiple logical commits when:

- Refactoring is mixed with feature work
- Migration is bundled with application code
- Test additions are combined with implementation

### Step 3 — Generate PR Description

Template:

```markdown
## What

{1-2 sentence summary of what this PR does}

## Why

{Business context — why is this change needed}

## How

{Brief technical approach — key design decisions}

## Changes

- {file-level or component-level change summary}

## Testing

- {What tests were added/modified}
- {Manual testing steps if applicable}

## Migration Notes

- DB migrations included? Yes/No
- Configuration changes? Yes/No
- Breaking API changes? Yes/No

## Checklist

- [ ] Tests pass locally
- [ ] No new warnings
- [ ] API documentation updated (if applicable)
- [ ] Migration is backward-compatible
```

### Step 4 — Pre-Submit Validation

- Run: `./gradlew build` (compile + test)
- Use skill: `coding-standards` — check for style violations
- Use skill: `architecture-guardrail` — check for layer violations
- Check: no `TODO`/`FIXME` in new code (or explain why)
- Check: no `System.out.println` or debug logging left in
- Check: no `@Disabled` tests that should be enabled

### Step 5 — Output

Present:

1. Suggested commit message(s)
2. PR title (concise, under 72 characters)
3. PR description (ready to paste)
4. Any pre-submit warnings found
5. Suggested reviewers based on changed packages (if team structure is known)

## Key Skills Reference

**Quality gates:**

- Use skill: `coding-standards` for style and naming conventions
- Use skill: `architecture-guardrail` for layer and dependency violations

**Context for better descriptions:**

- Use skill: `api-guidelines` for API change documentation
- Use skill: `exception-handling` for error handling changes
- Use skill: `transaction` for transaction boundary changes
- Use skill: `jpa-performance` for data access changes

## Rules

- Always analyze the full diff before generating anything
- Commit messages follow Conventional Commits strictly
- PR description must include What, Why, and How sections
- Run build validation before declaring ready for review
- Never skip pre-submit checks — surface warnings, don't hide them

## Checklist

- [ ] Changes categorized (feature/bugfix/refactor/chore/docs/test)
- [ ] Commit message(s) follow Conventional Commits format
- [ ] PR title is concise and descriptive
- [ ] PR description covers What/Why/How/Changes/Testing/Migration
- [ ] Build passes (`./gradlew build`)
- [ ] No style or architecture violations
- [ ] No leftover debug code (`System.out.println`, `TODO`, `FIXME`)
- [ ] No accidentally disabled tests

## Avoid

- Generating commit messages without reading the actual diff
- Vague PR descriptions ("various improvements")
- Skipping pre-submit validation to save time
- Combining unrelated changes into a single commit
- Writing commit bodies that describe "how" instead of "why"
