# Safe Tool Policy / Read-only Mode

## Purpose

This document records the current safe tool policy for the local OpenClaw agent used in the ADW-Forge workflow.

The goal is to allow the local agent to inspect the repository safely while preventing it from changing files, memory, git history, sessions, browser state, or runtime state without human review.

## Current Mode

The local agent is currently treated as read-only for repository work.

Allowed use:

- Read repository files.
- List repository directories.
- Search files.
- Inspect file metadata.
- Run safe Git inspection tools such as status, log, diff, show, and branch.

Not allowed:

- Write or edit files.
- Run shell/process commands.
- Add, commit, reset, checkout, or create Git branches.
- Write or mutate memory.
- Spawn sessions or subagents.
- Use browser/playwright tools.
- Push changes.

## Current Safety Decision

The local agent may perform read-only repo inspection.

All file writes, memory writes, Git history changes, commits, and pushes must be done through PowerShell after human review.

## Known Issue

The local agent may still call extra read-only tools beyond the prompt.

This is lower risk after the read-only tool policy because dangerous tools are denied, but outputs must still be verified.

## Operating Rules

- One prompt = one intended action.
- Read before write.
- Diff before commit.
- Commit before push.
- Human review before Git history changes.
- Verify real files or Git state with PowerShell.
- Do not trust agent summaries for write, memory, or Git operations without checking the actual file or Git output.

## Current Verified Status

- Workspace routing points to the real repository.
- Repository path: E:\Ai-Project\Projects\ADW-Forge
- Built-in read can read doc/progress.md from the real repository.
- Dangerous tools are hidden from the local agent in read-only mode.
- Repository remained clean after read-only agent tests.
