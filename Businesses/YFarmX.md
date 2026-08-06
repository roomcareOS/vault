---
tags: [business, yfarmx]
source: yfarmx/CLAUDE.md, yfarmx/README.md, yfarmx/docs/master-plan.md, yfarmx/docs/status.md, yfarmx/docs/audit.md, yfarmx/docs/pivot-salvage.md
updated: 2026-08-06
---

# YFarmX

**What it is:** an automated frontier-technology newsroom at **yfarmx.com**, covering AI, crypto/blockchain and quantum computing. Run by Jay (John Kamal) as YFarmX Ltd, solo, directing AI builds from a Chromebook and phone. The aim: the most readable, accessible frontier-tech news site on the web.

**The model in one line:** *the repo is the newsroom.* A static site built with Astro (a static site generator: the site is plain files, no database, no CMS to log into) hosted on Cloudflare Pages. Every article is a Markdown text file committed to git; publishing is a git push; deploys are automatic. If it is not committed, it does not exist.

## Targets

- **~5 well-sourced articles a day**, each with audio and social syndication
- **90 to 95% automation** through the [[Hermes Newsroom Pipeline (YFarmX)]], reached only on a sustained quality bar (~month 6). Everything human-reviewed until then
- **£1,400/month OpenRouter budget** (OpenRouter is a broker that routes work to many AI models), tracked from day one. Guardrails: per-workstream API keys with limits; a hard £15/day cap on the automated newsroom key only, so automation can never run away silently; interactive work is never blocked
- **Monetise properly by month 12**: labelled AI/quantum affiliates first (crypto affiliates deferred pending solicitor sign-off), newsletter from month 3, display ads once traffic justifies, premium tier only if earned

## Editorial position

Accuracy is the product. Primary and official sources only (regulators, filings, the company's own announcements), never rival tech outlets. Minimum two independent sources per news claim, no invented quotes, sources listed on every article, British English throughout. The full working rules live in [[Claude Operating Profile - YFarmX]] and the day-to-day loop in [[Article Pipeline (YFarmX)]].

## History in three beats

1. **WordPress era (2022 to July 2026).** 427 published posts on a GoDaddy-hosted WordPress site. A read-only audit (docs/audit.md, 16 July 2026) found it fixable but creaking: broken category system feeding Google dead links, mobile Lighthouse 53/100, no backup under our control, business logic living unversioned in the database, PHP 7.4 (end-of-life).
2. **The pivot (17 July 2026).** WordPress abandoned for the static git-based architecture; nothing wasted (docs/pivot-salvage.md records what carried forward: design tokens, tool code, the audit as migration bible, all standing decisions). Why and how: [[Decisions - YFarmX]].
3. **The static newsroom (now).** Site rebuilt, articles migrated with URLs preserved, tools and glossary carried over, robotics vertical launched 30 July, staging-and-backup safety net live 5 August.

**Content scale:** ~736 published content files live in the repo (526 news articles plus 210 reference/explainer pages). They are deliberately **not** mirrored into this vault; the repo is their home.

## Infrastructure notes

- Repo: `roomcareOS/yfarmx` (private, under the [[RoomCare]] GitHub organisation; the two businesses stay separated at repository level)
- Hosting: Cloudflare Pages, free tier, domain registration stays at GoDaddy, DNS at Cloudflare
- Key dates on the plan: EU AI transparency rules from 2 Aug 2026 (documented human review keeps the site inside the editorial exemption); cheap scanning models retire ~Oct 2026 (config swap)

## Status snapshot (5 August 2026, from docs/status.md)

- **Staging and daily backups are LIVE** (decision 38). Every change, articles included, goes to a private staging site for Jay's review before promotion to live. See [[Staging and Backups (YFarmX)]]. **Outstanding: Jay's one dashboard toggle** (Cloudflare Pages "Access policy") — until it is on, staging is unindexable but not truly private.
- **Audio is parked, not finished.** Jay's call: move to an OpenRouter voice going forward; the Gemini TTS work stops where it is. Next session should evaluate OpenRouter's speech offering.
- Three articles published and socialised on 5 Aug (AISI agent-behaviour, OpenRouter Ori, SpaceX/NVIDIA Starmind). Starmind is the Space desk's first live news article.
- Robotics vertical launched 30 July ([[Robotics Launch Checklist (YFarmX)]]); glossary at 3,050 terms across three desks; all Security Desk trackers redesigned and theme-aware.
- **Time-sensitive flags:** DMARC email policy due to tighten ~16 August (`p=none` → `p=quarantine`), and DKIM is still outstanding — see [[Ops Runbooks (YFarmX)]]. A true off-site backup copy needs a credential from Jay (all current backup layers live on GitHub/Cloudflare).

## Links

[[Home]] · [[Map - Businesses]] · [[Map - Processes]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Article Pipeline (YFarmX)]] · [[Decisions - YFarmX]] · [[Social Syndication (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Staging and Backups (YFarmX)]] · [[Ops Runbooks (YFarmX)]] · [[App and Store Distribution (YFarmX)]] · [[Space Hub Build (YFarmX)]] · [[Decisions - Space Hub (YFarmX)]] · [[Todoist Doctrine]]
