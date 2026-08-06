---
tags: [decision, yfarmx]
source: yfarmx/docs/decisions.md, yfarmx/CLAUDE.md, yfarmx/docs/pivot-salvage.md
updated: 2026-08-06
---

# Decisions - YFarmX

The load-bearing calls from [[YFarmX]]'s decisions log (38 numbered entries, 16 July to 5 August 2026). A decision stands until a later one supersedes it. Grouped by theme, newest thinking first.

## The pivot (17 July 2026) — the big one

**YFarmX left WordPress and GoDaddy hosting for a static, git-based site.** The original 16-July plan was "stay on WordPress, replace the theme"; one day later that was overturned. Why: the Hermes pipeline is the heart of the project, and on a static architecture every article is a plain text file — Hermes writes the file, commits it, and the site deploys itself. No CMS, no plugins, no security patching, no hosting bill. The best pages on the old site were already hand-built HTML fighting the CMS, so WordPress was overhead, not value.

- **Stack: Astro + Cloudflare Pages** (typed content collections map 1:1 to the article schema; zero JavaScript by default; Cloudflare because the domain was already behind it, the free tier allows commercial use, and per-branch preview URLs suit Jay's phone reviews).
- **Nothing was wasted:** a salvage pass (docs/pivot-salvage.md) carried forward the design tokens, the working tool code, the Phase-0 audit (which became the migration bible: 427 posts, URL map, SEO and performance baselines), and every non-WordPress decision. The theme era was archived on a git branch, not deleted.
- The WordPress site stayed live and untouched until the new site fully replaced it; DNS cutover was a planned, reversible event.

## Publishing and safety

- **38 (5 Aug):** every change, articles included, goes through a private **staging site** first; the Promote workflow refuses to ship anything that is not byte-identical to what Jay reviewed. Daily backup tags plus weekly full-repo bundles. See [[Staging and Backups (YFarmX)]].
- **Audio (5 Aug):** house voice is `Aoede` on Gemini TTS with a one-line, adjective-free British brief — adjectives in the brief produced posh-nasal or cockney reads. Re-cutting published audio uses a `--suffix` flag, never a file rename (a rename once silently published stale audio). Free tier is 100 requests/day per model, the binding constraint. *Since parked: Jay wants an OpenRouter voice going forward.*
- **Byline:** Jay's real name (John Kamal) on posts and the editor bio — load-bearing for rankings and the AI-disclosure position.

## Design

- The palette is the cube: **AI = blue, Crypto = green, Quantum = violet, Security = red**; the KITT scanner is the kinetic signature, used sparingly.
- Design direction reset to **Jay's glass mockup** (light-first frosted glass, "Intelligence for an accelerating world"); an earlier dark editorial look was rejected.
- **Rebrand (26 Jul):** flat 2x2 puzzle cube mark (blue/red/green, Jay's saturated palette), 3D render retired; every derivative regenerates from one script, never hand-edited.
- **Trackers are no longer permanently dark (2 Aug):** they flip with the site theme; green carries money figures, red stays the alert colour.

## Money and risk

- **Budget:** plan to use the full £1,400/month; split across workstreams flexible. Per-workstream OpenRouter keys with limits; hard £15/day cap on the newsroom key only.
- **Crypto affiliate monetisation deferred** until a financial-promotions solicitor signs off in writing (UK rules restrict cryptoasset promotion). Launch monetisation uses labelled non-financial AI/quantum affiliates only.
- **Social: X plus both LinkedIn destinations** (company page and Jay's profile), routed via Buffer.
- **Repo lives under the roomcareOS GitHub organisation** (`roomcareOS/yfarmx`, private) but the businesses stay separated at repository level — no [[RoomCare]] material in YFarmX repos or vice versa.

## Architecture details that keep resurfacing

- **Live market data goes through one cached Pages Function** (a small server-side function on the host), not per-visitor API calls — keyless CoinGecko throttles Cloudflare's shared IPs. Superseded an earlier browser-direct approach.
- **Reference/explainer pages are their own content collection**, separate from dated news articles, routed under pillar sub-hubs.
- **CSP script security is pinned to build-time hashes**, not `unsafe-inline`; a brand-new inline script is blocked until the next build re-hashes, which is the guarantee wanted.
- **Jay's hand-built page code is canon** for tools, games and directories: his WordPress-era JS was harvested and runs near-verbatim in the new site.
- **Events policy:** listings verified against the organiser's own site, each record carrying a provenance URL; unverifiable editions dropped.

## Links

[[YFarmX]] · [[Map - Decisions]] · [[Article Pipeline (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Staging and Backups (YFarmX)]] · [[Social Syndication (YFarmX)]]
