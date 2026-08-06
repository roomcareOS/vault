---
tags: [decision, yfarmx]
source: yfarmx/docs/space/DECISIONS.md, yfarmx/docs/space/OUT_OF_SCOPE.md, yfarmx/docs/space/BUDGET.md, yfarmx/docs/space/RECON.md
updated: 2026-08-06
---

# Decisions - Space Hub (YFarmX)

Non-obvious choices from the [[YFarmX]] Space section build, each with its reason. The build itself is described in [[Space Hub Build (YFarmX)]].

## Scope and control

- **The scope lock was translated, not followed literally** (29 Jul). Word for word it forbade every file the spec asked for (no `space/` directory exists); the translation — "the Space set only, nothing shared with the main site" — was proposed to Jay rather than assumed.
- **`SPACE_PUBLIC` stays false; only Jay flips it.** One line launches the section into sitemap, RSS, nav and search. The out-of-scope register records the exact diff, unapplied.
- **Space documents live at `docs/space/`,** following the repo's existing convention, since the spec's `space/_docs/` location cannot exist.
- **An append-only out-of-scope register** records every forbidden change with the smallest diff that would have satisfied it — entries are appended, never rewritten, so the record of what was wanted stays intact.
- **The shared `Turntable.astro` component is never edited** — it also serves the Robotics vertical. The new models were rendered to its existing naming convention so they drop in with zero risk; any future GLB viewer gets a Space-only component instead.
- **three.js added on Jay's explicit instruction (2 Aug),** resolving the dependency hold; satellite.js still absent, only needed for live orbit propagation.

## Honesty

- **Class archetypes, never named spacecraft.** A procedural model captioned "Perseverance" would be a guess presented as fact in a section that promises no invented data. Every model carries provenance `illustration`.
- **Scale is measured, not declared.** A geometry helper builds boxes at half the requested size; fixing it would break a hand-tuned library in one commit, so the defect stays, the manifest publishes script-measured spans as authoritative, and no scale-comparison view may be built until the library is rebuilt. Publish what is true, mark what is not verified, document the trap.
- **Opinion desk stays empty until Jay writes it; News fills itself from the other desks.**

## Craft (won by looking, not reasoning)

- **Python scripts are the source; no `.blend` binaries committed** — a git newsroom should not carry blobs it cannot review. Blender itself is a gitignored symlink.
- **Sun lamps, not area lights** — a sun has no distance falloff, so one rig lights a 0.3m cubesat and a 60m station identically. Found by looking at a render after wrongly blaming two other causes first.
- **AgX tonemapping, not Filmic** — Filmic drained lit solar arrays to white.
- **Framing is measured across all eight angles,** not fitted to a bounding sphere, which wastes the frame on anything wide and flat (most spacecraft).
- **WebP straight out of Blender; renders run one at a time** — keeps a `package.json` change out of the render path, and Cycles already saturates every core.

## Performance (measurements beat rules)

- **No WebM sibling for the hero video, against the spec.** x264 at CRF 36 (542KB) beat AV1 (693KB+) and VP9 (2,178KB) on this footage — shipping WebM would send more bytes for a worse picture. Re-test if the source is ever regenerated.
- **The denoise-then-encode trick:** AI-generated footage carries grain that wrecks compression; a moderate denoise unlocked a 79% cut with no visible damage, checked frame against frame.
- **Video rendition chosen in JavaScript, not `<source media>`** — Chromium fetched both files with media queries; one assignment cannot. With JS off, the poster stands alone, which was already the no-JS state.
- **GLB masters are never served to readers** — 600–900KB each would breach the 3D budget alone; a future viewer needs decimated versions.

[[Map - Decisions]] · [[Decisions - YFarmX]] · [[Map - Processes]]
