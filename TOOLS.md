# TOOLS.md

## Forge Tool Plan

Forge will use tools through OpenClaw and MCP.

## Core Tools

### Filesystem
Purpose:
- Read project files
- Write project files
- Organize workspace

Access:
- E:\Ai-Project only at first

### Memory
Purpose:
- Remember project decisions
- Track progress
- Store useful context

### Browser / Playwright
Purpose:
- Open local dashboards
- Test web apps
- Automate browser tasks

### Docker
Purpose:
- Check container status
- Read logs
- Restart selected project containers

Safety:
- Ask before stopping/removing containers
- Ask before deleting volumes
- Ask before exposing ports

### Web Search
Purpose:
- Search docs
- Check bugs
- Verify latest setup instructions

Preferred:
- Use local/self-hosted search when possible
- Use API/cloud search only when needed

### Vision
Purpose:
- Analyze screenshots
- Read UI errors
- Understand diagrams/images

Model:
- qwen3-vl:4b

## Playwright MCP Checkpoint

Status: Working

Verified:
- `openclaw mcp doctor`
  - playwright: ok
- `openclaw mcp probe playwright --json`
  - 23 tools
- Dashboard Chat successfully called:
  - `playwright__browser_navigate`
  - opened `http://example.com`
  - returned Page Title: `Example Domain`

Notes:
- Do not install global Playwright unless needed.
- Playwright MCP entry path:
  `E:\Ai-Project\mcp\playwright\node_modules\@playwright\mcp\cli.js`
- Current config uses:
  `--browser chrome`
  `--headless`
- Dashboard Agent may still call Web Fetch/Web Search unnecessarily; this is agent behavior, not MCP failure.

## OpenClaw Tool Policy Checkpoint

Status: Working

Verified:
- Playwright MCP works from Dashboard Chat.
- `playwright__browser_navigate` opened `http://example.com`.
- Page Title returned: `Example Domain`.
- Built-in Web Fetch and Web Search are disabled for local model.
- Built-in browser tool is disabled for local model.
- Security audit critical count is now 0.

Current tool policy:
- Local model should prefer MCP tools.
- Web search is intentionally not configured yet.
- Browser automation uses Playwright MCP, not OpenClaw built-in browser.

Playwright config:
- Browser: `msedge`
- Mode: `--headless`
- Isolation: `--isolated`

Cleanup:
- Playwright Edge processes can be closed with:
  `taskkill /IM msedge.exe /F`