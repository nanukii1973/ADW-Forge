---
## 2026-07-09 - Git MCP Checkpoint
- Git MCP registered in OpenClaw config.
- openclaw mcp doctor: git ok.
- openclaw mcp probe git: 12 tools.
- Dashboard tests passed: git__git_status, git__git_log, git__git_diff_unstaged.
- Repo: E:\Ai-Project\Projects\ADW-Forge
- Branch: main
- Remote: origin/main
- Working tree: clean

---
## 2026-07-09 - Project Automation Workflow Test
- Step 1 git__git_status passed.
- Step 2 filesystem__list_directory passed.
- Step 3 filesystem__read_file passed.
- Step 4 filesystem__write_file tested, but newline escaping required review before commit.
- Confirmed safe workflow pattern: test in NEW SESSION, commit only after review.

---
## 2026-07-10 - Memory MCP JSONL Persistence Checkpoint
- Memory MCP persistence fixed by switching memory store to JSONL format.
- MEMORY_FILE_PATH now points to E:\Ai-Project\.forge\memory\mcp-memory.jsonl.
- Existing memory checkpoints migrated into JSONL lines.
- memory__read_graph passed.
- memory__create_entities passed.
- memory__search_nodes passed.
- Verified persistence by checking mcp-memory.jsonl directly.
- Known issue: local agent may still call extra tools or ignore reply format.
- Safe rule remains: 1 prompt = 1 tool = 1 action, then verify real file or git state.

---
## 2026-07-10 - Use Case 1.4 Full Safe Workflow Dry Run Checkpoint
- Repo status verified by PowerShell.
- Branch: main.
- Working tree: clean.
- Latest commit verified: 6ff6cf6 Document Memory MCP JSONL persistence checkpoint.
- progress.md verified by reading the real repository file directly.
- Memory JSONL persistence verified by reading E:\Ai-Project\.forge\memory\mcp-memory.jsonl directly.
- Memory file contains project checkpoints for filesystem MCP, memory MCP, git MCP, and JSONL persistence test.
- Issue found: local agent read/write tools still resolve paths under C:\Users\YiCD\.openclaw\workspace instead of the real ADW-Forge repo.
- Safety decision: do not allow local agent write/create/commit actions until filesystem routing is fixed or verified.
- Continue safe workflow using PowerShell for real repo verification and Git operations.

---
## 2026-07-10 - OpenClaw Workspace Routing Checkpoint
- agents.defaults.workspace set to E:\Ai-Project\Projects\ADW-Forge.
- Gateway restarted and workspace routing verified.
- Built-in read tool can now read doc/progress.md from the real ADW-Forge repository.
- Issue remains: local agent may still call extra tools beyond the prompt.
- OpenClaw runtime/bootstrap files may appear in the repo workspace.
- Added .gitignore rules for SOUL.md, USER.md, HEARTBEAT.md, IDENTITY.md, openclaw-workspace-state.json, and openclaw-schema.tmp.json.
- Commit pushed: 2fc09a8 Ignore OpenClaw runtime workspace files.
- Safe rule remains: local agent may read repo files, but write/commit/push should still be done by PowerShell after human review.
