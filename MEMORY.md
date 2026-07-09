# MEMORY.md

## Forge Project Memory

This file records important decisions, checkpoints, and lessons learned.

## Project Identity

Project name:
Forge

Goal:
Build a local-first AI partner running on this machine through OpenClaw.

## Current Checkpoint

- Windows 10 x64
- Docker Desktop working
- Ollama working
- OpenClaw Gateway local working
- OpenClaw Dashboard working
- Dashboard Chat working
- Memory Plugin enabled
- Local agent can respond

## Important Decisions

- Use OpenClaw as the main orchestration layer
- Use Ollama local models by default
- Use qwen3:8b as the default general model
- Use qwen2.5-coder:7b for coding
- Use qwen3-vl:4b for vision/image understanding
- Build step by step
- Do not overload the user with too many new ideas

## Safety Rules

- Ask before destructive changes
- Ask before deleting files
- Ask before removing Docker containers or volumes
- Ask before changing system-wide configuration

## Progress Log

### 2026-07-09

Created initial project brain files:

- FORGE.md
- AGENTS.md
- TOOLS.md
- MEMORY.md

### 2026-07-09 - MCP Filesystem Connected

- Registered filesystem MCP in OpenClaw config
- MCP doctor result: filesystem ok
- MCP probe result: filesystem 14 tools
- Dashboard Chat successfully used Filesystem List Directory on E:\Ai-Project
- Forge can now inspect the local workspace through MCP
- Web Search is still disabled and will be configured later

### 2026-07-09 - MCP Memory Connected

- Registered memory MCP in OpenClaw config
- MCP doctor result: memory ok
- MCP probe result: memory 9 tools, resources
- Dashboard Chat successfully saved entity to MCP memory
- Memory file created at E:\Ai-Project\.forge\memory\mcp-memory.json
- Forge now has working MCP filesystem and MCP memory