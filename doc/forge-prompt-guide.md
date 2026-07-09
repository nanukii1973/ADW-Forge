# Forge Prompt Guide

## Purpose

This guide defines safe prompt patterns for using Forge with local agents.

Target model:

- qwen3:8b
- local-first
- short direct prompts
- tool usage must be controlled

Main rule:

```text
1 prompt = 1 tool = 1 action
```

Do not ask the agent to read, write, diff, commit, and push in one prompt.

---

## Session Types

### NEW SESSION

Use for tool tests and isolated actions.

Good for:

- git status
- list directory
- read one file
- write one file
- diff check

### MAIN

Use for planning and review only.

Good for:

- explain result
- propose next step
- review content
- decide whether to continue

Do not use tools in MAIN unless explicitly required.

### POWERSHELL

Use when safety or precision is required.

Good for:

- git commit
- git push
- fixing file encoding
- recovering from unwanted agent actions
- checking real repo state

---

## Core Safety Rules

```text
Read before Write
Diff before Commit
Commit before Push
Human review before Git history changes
```

Never trust a write operation until `git diff` has been reviewed.

If the agent uses an extra tool, stop immediately and check repo status.

---

## Prompt Style Rules

Use short prompts.

Use exact tool names.

Use exact paths.

Use only one action per prompt.

Avoid:

```text
Please read the project, update the docs, save memory, commit, and push.
```

Prefer:

```text
Use only git__git_status.

repo_path:
E:\Ai-Project\Projects\ADW-Forge

Do not use any other tool.
Reply with clean or changed only.
```

---

## Filesystem Prompt Patterns

### List Directory

Use in NEW SESSION.

```text
Use only filesystem__list_directory.

path:
E:\Ai-Project\Projects\ADW-Forge

Do not use any other tool.
Reply with a short list of files and folders only.
```

### Read File

Use in NEW SESSION.

```text
Use only filesystem__read_file.

path:
E:\Ai-Project\Projects\ADW-Forge\doc\progress.md

Read this file only.
Do not write files.
Do not create files.
Do not use any other tool.

Reply with:
- readable or not readable
- 3-line summary
```

### Write File

Use only after human review.

Use in NEW SESSION.

```text
Use only filesystem__write_file.

path:
E:\Ai-Project\Projects\ADW-Forge\doc\progress.md

Write this exact content to the file:

PASTE_FULL_REVIEWED_CONTENT_HERE

Do not create new files.
Do not edit other files.
Do not use any other tool.

Reply with write success or write failed only.
```

Important:

After every write operation, run a diff check before commit.

Known risk:

- The agent may write literal `\n` instead of real newlines.
- The agent may use relative paths.
- The agent may call extra tools.

---

## Git Prompt Patterns

### Git Status

Use in NEW SESSION.

```text
Use only git__git_status.

repo_path:
E:\Ai-Project\Projects\ADW-Forge

Do not use any other tool.
Reply with clean or changed only.
```

### Git Diff Unstaged

Use in NEW SESSION.

```text
Use only git__git_diff_unstaged.

repo_path:
E:\Ai-Project\Projects\ADW-Forge

Do not use any other tool.
Reply with:
- changed files
- short summary of changes
```

If the Git MCP diff tool times out, use PowerShell:

```powershell
cd E:\Ai-Project\Projects\ADW-Forge
git diff -- doc/progress.md
git status --short
```

### Commit

Use PowerShell until Git MCP commit is proven stable.

```powershell
cd E:\Ai-Project\Projects\ADW-Forge
git add doc/progress.md
git commit -m "Commit message here"
git status --short
```

### Push

Use PowerShell.

```powershell
cd E:\Ai-Project\Projects\ADW-Forge
git push
git log --oneline -3
```

---

## Memory Prompt Patterns

Memory tools must be tested separately.

Do not combine memory with filesystem or git until memory workflow is stable.

Safe pattern:

```text
Use only the memory tool.

Save this checkpoint:

CHECKPOINT_TEXT_HERE

Do not use filesystem.
Do not use git.
Do not use any other tool.

Reply with memory saved or memory failed only.
```

Before using memory in production workflow:

- test save
- test read
- verify memory file
- document tool name
- document failure behavior

---

## Recovery When Agent Uses Extra Tools

If the agent uses any tool not requested:

Stop.

Do not continue the workflow.

Check repo state immediately.

Use PowerShell:

```powershell
cd E:\Ai-Project\Projects\ADW-Forge
git status --short
```

If an unwanted file was created:

```powershell
Remove-Item "E:\Ai-Project\Projects\ADW-Forge\PATH_TO_FILE"
git status --short
```

If an existing file was changed:

```powershell
git diff -- PATH_TO_FILE
```

If the change is unwanted:

```powershell
git checkout -- PATH_TO_FILE
git status --short
```

If unsure:

Do not commit.

Review first.

---

## Human Review Checklist

Before writing:

```text
Is the target path correct?
Is the content reviewed?
Is this one file only?
Is this one tool only?
```

After writing:

```text
Did git status show only expected files?
Did git diff show correct changes?
Are newlines correct?
Is encoding correct?
Are there unwanted files?
```

Before commit:

```text
Working changes reviewed?
Only intended files staged?
Commit message clear?
```

Before push:

```text
Local commit passed?
Working tree clean?
Remote target correct?
```

---

## Safe Workflow

Standard safe workflow:

```text
1. git status
2. read file
3. propose change
4. human review
5. write file
6. git diff
7. human review
8. git add
9. git commit
10. git push
```

Never skip diff review.

Never commit unexpected changes.

Never push without a clean local check.

---

## Current Tested Tools

Tested and usable:

```text
git__git_status
git__git_diff_unstaged
filesystem__list_directory
filesystem__read_file
filesystem__write_file
```

Notes:

```text
git__git_diff_unstaged may timeout.
filesystem__write_file requires diff review.
Agent may call extra tools even when forbidden.
```

---

## Final Rule

When in doubt:

```text
Stop.
Check git status.
Review diff.
Continue only after human approval.
```
