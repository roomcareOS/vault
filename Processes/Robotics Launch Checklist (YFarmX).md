---
tags: [process, yfarmx]
source: yfarmx/docs/robotics-launch-checklist.md
updated: 2026-08-06
---

# Robotics Launch Checklist (YFarmX)

The robotics vertical on [[YFarmX]] **launched 30 July 2026** (live in the nav, sitemap, search, RSS and llms.txt). This note keeps the durable part: the checklist of what launching a new site vertical actually needs, learned by doing it. It is the template if a Space desk or any other vertical launches next.

## The launch-gate checklist (what "ready" means)

1. **Content, not code, is the blocker.** Every sub-desk needs at least one sourced story; an empty desk shows an honest empty state but looks unsettled. Three or four per desk looks lived-in.
2. **Explainers before launch.** A pillar with no evergreen reference pages has nothing to rank on between news cycles — the largest single job (robotics shipped with twelve).
3. **Hero art generated AND wired.** Prompts existing in a docs file is half the job; the templates need the banner slots too. Never launch on placeholders.
4. **Every claim re-verified against the makers' own pages** just before launch (all ten featured robot platforms were re-read verbatim against primary sources on launch day).
5. **The parent hub must actually link in** — a vertical only reachable from a flag-gated dropdown is invisible.
6. **Cross-links into hidden sections need gating** (robotics linked into the then-hidden Space world; a live page must not link a noindexed one).
7. **An RSS route has to be built, not assumed** — flipping the visibility flag does not conjure a feed that was never coded.
8. **llms.txt line added** (the file that tells AI answer engines what the site holds).
9. **The freshness registry gets the new pages** ([[Ops Runbooks (YFarmX)]]): the hub and desks at the medium tier, the platform data file at fast.
10. Then, and only then, flip the visibility flag (`ROBOTICS_PUBLIC`).

## The one open item

The stratospheric drone article (`world-mobile-launches-stratospheric-connectivity`) sits in `category: crypto`, so it never reaches the drones desk. It is genuinely a crypto-and-telecoms story about a drone network, so recategorising is Jay's judgement call, not an obvious fix. Its URL would not change either way.

## Links

[[YFarmX]] · [[Map - Processes]] · [[Article Pipeline (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Decisions - YFarmX]] · [[Space Hub Build (YFarmX)]]
