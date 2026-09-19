---
tags: [in-progress, yfarmx]
source: Session of 19 September 2026, from Jay's Muse connector brief and the two links he sent
updated: 2026-09-19
---

# Agent connector surface awaiting Jay (YFarmX)

Zuckerberg opened Muse to third-party connectors on 18 September 2026 ("You bring the API") and Muse already reads yfarmx.com. The connector surface is on protected staging as deployment 3f7ecbca, gate verified 401, awaiting Jay's review: `/data/v1/` (news, articles, glossary shards, events, state of play), `/openapi.yaml`, the brief at `/connectors/muse.md`, and the human pages `/agents/` and `/connectors/`.

- **Branch:** `claude/nice-clarke-yhwhqm`, merged to `staging`; the working record is `docs/agent-api.md` and the decision entry of 19 September in `docs/decisions.md`.
- **Blocking:** Jay's review on staging, then the promote. Agents cannot reach staging (password gate), so the Muse end-to-end test only works after the live deploy. The Pages 20,000-file cap was hit twice tonight and eased by the other session's 640w variant trim (build now 19,269 of 20,000); the durable fix (the R2 media rule, or a plan check) is a p1 card in the Todoist Inbox and the 19 September decision entries carry the numbers.
- **After live:** verify the URLs serve from yfarmx.com, then the X and LinkedIn announcement (draft handed to Jay in the session; the 18 September Muse launch is the 7-day news hook).
- **Held for later, deliberately:** a hosted MCP server on a Cloudflare Worker (about a day), "ask your agent" hints on article pages, any paid tier or sponsorship line, and exposing the model bench (Jay's call).

When this promotes, fold the durable rules into [[The agent API shapes are a contract held by the build (YFarmX)]] and delete this note.
