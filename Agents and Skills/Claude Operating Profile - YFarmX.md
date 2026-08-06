---
tags: [agent, yfarmx]
source: yfarmx/CLAUDE.md
updated: 2026-08-06
---

# Claude Operating Profile - YFarmX

How Claude is briefed to work on [[YFarmX]]. The repo's CLAUDE.md (the instruction file Claude Code loads every session) is the pivot-era mission brief; this distils its standing rules.

## Who it works with

Jay: founder, solo operator, **not a developer**, experienced at directing AI builds. Works from a 1366x768 Chromebook and a phone, often by voice-to-text, so transcription quirks are interpreted charitably and anything genuinely ambiguous gets confirmed. Plain explanations, small verifiable steps, screenshots for anything visual.

## Every session, in order

1. Read `docs/playbook.md` (the operating manual), then `docs/status.md` (where things stand). End by updating `docs/status.md`; log any call made in `docs/decisions.md`.
2. One meaningful change per commit, plain factual messages.
3. Ambiguous → ask Jay. Risky or hard to reverse → stop and explain first.
4. Never assume credentials; ask for exactly the one needed, by name, when needed.

## Hard constraints (verbatim spirit, never violated)

1. **Existing posts migrate; they are not lost.** URLs preserved, mapped 301 redirects for anything that must change, migration verified by sample and count.
2. **No destructive change** without a verified backup and Jay's explicit confirmation.
3. **DNS cutover is a planned, reversible event** (checklist, low TTLs, rollback plan, Jay's go-ahead).
4. **Secrets never touch the repo** — environment variables and host secrets only, never echoed. Names are fine; values never.
5. **British English throughout**, en-GB dates.
6. **No unsourced claims in published content.** Accuracy is the product.

## Quality gates (stand for humans and for [[Hermes Newsroom Pipeline (YFarmX)]] alike)

- Minimum **two independent sources** per news claim
- **No invented quotes**, ever
- **Sources listed** on every article
- Corrections policy page, documented AI-disclosure position
- Cost logged per article; agreed daily spend cap; cheap models for bulk work, strong models for writing and editing

## The phases, compressed to a timeline

- **P1** — Salvage and scaffold: pivot-salvage doc, archive branch, Astro + Cloudflare Pages chosen, design system, templates for sign-off
- **P2** (weeks 2–4) — Migration: WordPress export, conversion to front matter (the metadata block atop each article file), images into the repo, URL map plus redirects, automated verification
- **P3** (weeks 3–6) — Build-out: all templates, tools, glossary, search, RSS, sitemap, publishing form and review queue
- **P4** — Cutover: staging checks, DNS switch with rollback, Search Console watched a fortnight, then GoDaddy hosting cancelled
- **P5** (month 2–3 on) — The automated newsroom: Hermes end to end, publish step writes files and commits
- **P6** (months 3–4) — Audio on every article, backfill top posts
- **P7** (months 4–8) — Interactive tools as islands (small self-contained interactive widgets on otherwise static pages)
- **P8** (months 3–12) — Monetisation in sequence, monthly report of traffic, revenue and pipeline cost against the £1,400

## Links

[[YFarmX]] · [[Map - Agents and Skills]] · [[Article Pipeline (YFarmX)]] · [[Decisions - YFarmX]] · [[Staging and Backups (YFarmX)]] · [[Todoist Doctrine]] · [[Tool - Claude Code]]
