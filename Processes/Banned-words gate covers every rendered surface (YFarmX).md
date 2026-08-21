---
tags: [process, yfarmx, copy, lint]
source: 21 August 2026 whole-site audit and fix pass
updated: 2026-08-21
---

# Banned-words gate covers every rendered surface (YFarmX)

**The rule: if a string can reach a reader, the gate reads it.** From
21 August 2026, `scripts/lint-content.mjs` scans four surfaces against the one
list in `scripts/lib/banned-words.mjs`:

1. The Markdown corpus (articles + reference), as before.
2. The rendered-data JSON under `src/data/` (events, glossary, exploit log,
   hub state, trackers, space boards) — machine-baked files
   (`desk-markets`, `market-pulse`, `audio-manifest`, `brand-logos`) skipped.
3. The arcade game HTML in `src/games/`.
4. String literals and template text in `src/pages`, `src/components`,
   `src/layouts`, `src/lib` and `public/js`. Code identifiers, URLs, paths,
   comments and letterless placeholder strings ("—" score readouts) never
   reach the rules; prose-shaped strings do.

## Why

The 21 August audit found every live banned-word leak sitting exactly where
the old gate never looked: five em dashes on the events boards, seventeen
hits across the glossary and exploit log, "In simple terms" in a quiz
question, eleven podcast feed titles built by an em-dash template, an
aria-label carrying the "X by X" tic, and an author-page title template. The
gate that "enforced the list on every build" read only `src/content`.

## Subject-sense allowances

`ALLOWED_SUBJECT_PHRASES` in `banned-words.mjs` blanks named collocations
before matching, following the "noise as physics" and "HYPE the ticker"
precedents: `condensed matter`, `matter and energy`, `light and matter`,
`quantum hype cycle` (a glossary term name). Keep the list to named
collocations, never bare words, and record each addition here and in the
module comment. Flagged to Jay 21 Aug for a standing ruling on the physics
"matter" senses.

## The ratchet still refuses desk-era tolerance

`--write-baseline` carries forward only `migrated: true` files' debt. The new
surfaces have no such flag, so a hit in data, a game or a template can never
be baselined — it gets fixed. The 21 Aug refreeze emptied the stale
reference-page entries (all eighteen described violations that no longer
existed).

## Related same-day machinery

- The Crypto Index snapshot (`crypto-systems.json`) joined the six-hourly
  markets refresh (`scripts/refresh-markets.mjs`), name-guarded by pinned
  CoinGecko ids so a renamed ticker can never write a wrong figure;
  `--systems-only` exists for manual catch-ups on a branch.
- RSS enclosure byte sizes are patched into `dist/rss.xml` by
  `scripts/og-jpegs.mjs` after it writes the real files — never predicted.
- Tool names ruled by Jay (21 Aug): **Gas Fee Checker**, **Crypto Exploit
  Tracker**, **Glossary**. Grep out any resurfacing "Gas Console", "Attack
  Log" or "the Codex".
