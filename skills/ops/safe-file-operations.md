---
name: safe-file-operations
description: Cross-platform file operations using git and POSIX-compatible commands
category: ops
tags: [file-operations, git, cross-platform, bash, safety]
---

# Safe File Operations

## When to Use

- Removing, moving, or renaming files in a git repository
- Performing file operations that need to work across Windows (Git Bash), macOS, and Linux
- Ensuring file changes are tracked by git
- Avoiding platform-specific commands that may not be available

## Rules

### File Removal

- **Always prefer `git rm`** over `rm`, `del`, or `Remove-Item`
- `git rm` works cross-platform and stages the deletion automatically
- For untracked files, use `rm` (POSIX) - works in Git Bash on Windows

### File Moving/Renaming

- **Always prefer `git mv`** over `mv`, `move`, or `Move-Item`
- `git mv` preserves git history and stages the change automatically

### Directory Operations

- Use `mkdir -p` for creating nested directories (works in Git Bash)
- Never use `md`, `New-Item`, or Windows-specific commands

### Checking Before Deleting

- Use `git clean -n` (dry-run) to preview what would be deleted
- Use `git status` to review staged/unstaged changes before operations

## Pattern

❌ Bad - Platform-specific commands:

```bash
# Windows-only - won't work in Git Bash or Linux/macOS
del src/old-file.java
Remove-Item src/old-file.java
move src/old.java src/new.java
md -Force src/new-folder

# Dangerous - bypasses git tracking
rm src/tracked-file.java  # File disappears from working tree but git sees it as deleted
```

✅ Good - Git-native cross-platform commands:

```bash
# Remove tracked files (stages deletion automatically)
git rm src/old-file.java
git rm -r src/old-folder/

# Remove tracked files but keep in working directory
git rm --cached src/file-to-untrack.java

# Move/rename files (preserves history)
git mv src/OldName.java src/NewName.java
git mv src/old-folder/ src/new-folder/

# Preview what git clean would delete (safe dry-run)
git clean -n

# Remove untracked files (after reviewing with -n)
git clean -f

# Create directories (cross-platform)
mkdir -p src/main/java/com/example/service

# Remove untracked files only (POSIX - works in Git Bash)
rm src/temp-file.txt
rm -r src/temp-folder/
```

✅ Good - Safe deletion workflow:

```bash
# 1. Check status first
git status

# 2. For tracked files - use git rm
git rm src/deprecated-service.java

# 3. For untracked files - preview first, then remove
git clean -n  # See what would be deleted
git clean -f  # Actually delete (after confirming)

# 4. Verify the result
git status
```

## Command Reference

| Operation             | Use This         | NOT This                   |
| --------------------- | ---------------- | -------------------------- |
| Delete tracked file   | `git rm file`    | `rm`, `del`, `Remove-Item` |
| Delete untracked file | `rm file`        | `del`, `Remove-Item`       |
| Move/rename file      | `git mv old new` | `mv`, `move`, `Move-Item`  |
| Create directory      | `mkdir -p dir`   | `md`, `New-Item`           |
| Preview deletions     | `git clean -n`   | -                          |
| List files            | `ls -la`         | `dir`                      |
| Show file content     | `cat file`       | `type`, `Get-Content`      |

## Avoid

- `del`, `Remove-Item`, `move`, `Move-Item`, `md`, `New-Item` - Windows PowerShell only
- `rm` on tracked files without using `git rm` - loses git tracking
- `git clean -f` without first running `git clean -n` - may delete wanted files
- Any command that doesn't work in Git Bash (the common denominator for Windows devs)
- `dir` command - use `ls` instead for cross-platform compatibility
