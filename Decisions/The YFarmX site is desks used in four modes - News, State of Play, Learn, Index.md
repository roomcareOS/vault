---
tags: [decision, yfarmx]
source: Jay's whole-site UX/IA redesign report (16 Aug 2026, in chat) → yfarmx/docs/decisions.md entry 59
updated: 2026-08-16
---

# The YFarmX site is desks used in four modes - News, State of Play, Learn, Index

**The decision.** Everything on yfarmx.com outside Space is organised as three
subject desks (AI, Crypto, Quantum) plus one cross-frontier desk (Security),
each used in the same four modes: **News** (what changed), **State of Play**
(where things stand), **Learn** (how it works) and **Index** (what exactly
everything is). One reader promise: read → understand → track → explore.
Implemented 16 Aug 2026 on branch `claude/yfarmx-site-reorganization-zwxfge`
(awaiting Jay's review at the time of writing); repo decision 61 carries the
full change list.

**The rules a future session must not undo:**

- **The reference databases are called the AI Index, Crypto Index and Quantum
  Index in every label**, and nothing else. "Knowledge Hub", "Systems
  Explorer", "Model & Tool Index" and "Systems Index" are retired names; do
  not reintroduce them in navigation, cards, buttons or copy.
- **Their URLs stay where they are** (`/ai/knowledge/`, `/crypto/systems/`,
  `/quantum/knowledge/`), and the three trackers stay under `/tools/`. The
  report's own §23 caution: established search visibility outranks URL
  neatness. Do not "tidy" these URLs to match the labels without Jay's
  explicit instruction and a redirect plan (and mind `_redirects`' ~101-rule
  kill line — it has only ~4 slots left; new redirects go in Pages Functions).
- **State of Play lives at `/{desk}/state-of-play/`**; the desk hubs carry a
  three-card preview only. Do not grow the full accordion back into the hubs.
  The pages read the same `ai-hub.json` / `crypto-state.json` /
  `quantum-hub.json` files as before, so freshness passes need no new steps.
- **`/{desk}/news/` is the desk's chronological archive.** "AI home" and
  "AI news" are never again two labels for the same page.
- **Every desk keeps the same seven nav positions in the same order:**
  Overview · News · State of Play · Learn · Index · security tracker ·
  Events. Only the security wording flexes per desk.
- **Navigation navigates; the homepage editorialises.** The header mega
  panels carry one latest headline, not story stacks plus tracker figures.

**The condition that would reopen it:** Jay's word, or evidence from real
traffic that the four-mode model confuses readers (the report's premise is
that the old many-named products confused them).

## Links

[[YFarmX]] · [[Map - Decisions]] · [[Decisions - YFarmX]]
