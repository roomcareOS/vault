---
tags: [process, yfarmx]
source: Agent connector surface build, 19 September 2026, the day after Meta opened Muse to third-party connectors
updated: 2026-09-19
---

# The agent API shapes are a contract held by the build (YFarmX)

YFarmX serves a public, read-only JSON layer for AI agents (Meta Muse, Claude, ChatGPT and their kin): `/data/v1/` for news, articles, glossary, events and state of play, `/openapi.yaml` describing it, and the connector brief at `/connectors/muse.md` that an agent reads when a user says "connect YFarmX". The manual is `docs/agent-api.md` in the yfarmx repo; this note carries only what a session must never get wrong.

## The rules a future session must hold

- **v1 shapes change by addition only.** Renaming, removing or retyping a field that has shipped breaks the connectors agents have already built, silently, in other people's products. A breaking change ships as `/data/v2/` with v1 kept serving. `scripts/verify-agent-api.mjs` runs as the last step of every build and fails on any break, so treat that gate failing as "you were about to break someone's agent", never as an obstacle.
- **A new endpoint lands in five places in one commit:** the route under `src/pages/data/v1/`, the manifest (`index.json.ts`), `openapi.yaml.ts`, new checks in `verify-agent-api.mjs`, and the MCP tool map in `functions/api/mcp.js` when a tool should carry it. The briefs pick it up automatically: all four (`/connectors/muse.md`, `claude.md`, `openai.md`, `grok.md`) render from ONE shared body in `src/lib/connector-brief.ts`, so edit that file and never the vendor files.
- **The namespace is `/data/v1/`, never `/api/`.** robots.txt disallows `/api/` for every AI crawler group and Pages Functions own that prefix.
- **Model identity stays behind `/api/model-compare`.** Jay's 16 September ruling (no file downloads of the bench) still stands; the agent surface links the compare page and serves none of those files.
- **The API is static build output**, generated from the same collections as the pages, so it can never disagree with the site. Freshness is the last deploy, and every payload says so in `generatedAt`.
- **Every payload opens with the provenance envelope** (product, version, generatedAt, source, license, licenseUrl, attribution, citeAs). The tracker downloads keep CC BY 4.0; everything newer carries quote-with-attribution terms.
- `/openapi.yaml` serves through an exact-name valve in `functions/_probe-guard.js` (yaml is otherwise a blocked extension). Widen that valve only by exact basename, knowingly.
- **The MCP server is `/api/mcp`, a Pages Function, live only.** Stateless MCP over streamable HTTP; seven read-only tools that fetch the public JSON, so MCP can never disagree with the site. The deploy token is Pages-scoped (a separate Worker fails with authentication error 10000), so keep it a function; the staging script's named function list deliberately leaves it off.

Business: [[YFarmX]]. Live on yfarmx.com since 19 September 2026 (deployment 6808c818), with Jay's ten-image art pack wired into /agents/ and /connectors/.
