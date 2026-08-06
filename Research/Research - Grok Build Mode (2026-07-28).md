---
tags: [research, yfarmx]
source: yfarmx/docs/research/2026-07-28-grok-build-mode.md
updated: 2026-08-06
---

# Research - Grok Build Mode (2026-07-28)

**What was researched:** a tip-off (Jay's screenshot of a Benji Taylor post) that xAI had launched an app builder for Grok. Treated per the playbook as a pointer, not a source. `x.ai` blocked the newsroom container entirely (Cloudflare 403), so the announcement page is cited nowhere — everything in the piece was rebuilt from xAI's own documentation at `docs.x.ai`, with screenshots as evidence.

## Key findings (all verified against docs.x.ai)

- **The privacy story is the lead.** Build mode chats and sessions are **retained** to allow publishing across sessions; Private Chats are **not available** in Build mode; if the "improve the model" toggle is on, build chats, sessions and data from created apps **may train the model**.
- The Grok Build CLI with zero-data-retention keeps "no trace or code data"; without it, code-data retention can be switched off in settings. Sandbox is **off by default**.
- Grok 4.5: $2 per 1M input / $6 per 1M output, knowledge cutoff 1 Feb 2026.
- Grok Build is deliberately **Claude Code-compatible**: it reads `CLAUDE.md`, skills, plugins, MCPs (Model Context Protocol — the standard plug between agents and tools) and hooks, accepts Claude Code flag aliases, and imports Claude Code sessions.
- **Enterprise trap:** Claude Code's `disableBypassPermissionsMode` setting is NOT honoured by Grok's always-approve mode — Grok needs its own `requirements.toml`, honoured only from root-owned paths.

## What Jay should remember

- Everything that existed only in the announcement or secondary write-ups (launch date, `grok.me` publishing, SuperGrok exclusivity) was **deliberately left out**. If Jay pastes the announcement text, those claims can go in. The pointer-not-source rule held under pressure.
- **Container workaround worth keeping:** Chromium cannot reach the internet directly there; screenshots were taken by intercepting browser requests with `page.route` and fulfilling them from a Node `fetch`. The script lives at `scratchpad/shot2.mjs` — rebuild it, do not rediscover it.
- If [[Tool - Claude Code]] and Grok's CLI ever run side by side on a repo, the permission-bypass gap above is a real safety difference, not a detail.

[[YFarmX]] · [[Map - Research]] · [[Article Pipeline (YFarmX)]]
