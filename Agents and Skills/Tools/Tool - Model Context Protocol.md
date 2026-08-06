---
tags: [tool, yfarmx]
source: yfarmx/src/content/reference/ai/agents/model-context-protocol.md
updated: 2026-08-06
---

# Tool - Model Context Protocol

**What it is:** the open standard (originated by Anthropic, now at the Linux Foundation) that connects AI agents to outside systems — files, databases, tickets, search. Its own docs call it "a USB-C port for AI applications": build a connector once and every agent that speaks the protocol can use it.

**Why it matters to Jay:** it is the plumbing under all his agent work — [[Tool - Claude Code]], Cursor, Codex and Goose all reach external systems through it, so anything Jay exposes as an MCP server works everywhere at once. Its main open question is security: prompt injection through a connected tool. Complements [[Tool - Agent2Agent Protocol]] (agent-to-agent, where MCP is agent-to-tool).

**Published reference:** `yfarmx/src/content/reference/ai/agents/model-context-protocol.md` on [[YFarmX]].

[[Map - Agents and Skills]]
