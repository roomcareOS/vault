---
tags: [process, yfarmx]
source: yfarmx src/pages/[slug].astro; Jay's player instructions 15-16 Aug 2026
updated: 2026-08-16
---

# The article audio player (YFarmX)

**What it is:** the panel at the top of every [[YFarmX]] article that plays the audio. Since 15 August 2026 it carries **two products** — the single-voice read (the default) and the podcast episode (the option) — and since 16 August it is built to Jay's own design.

## The layout is the part that matters

Jay supplied a mockup and then said the important thing about it: *"it was more the positioning that was vital, the extras are optional as appropriate."* So the arrangement is fixed and the decoration is not:

- **Left half, the transport:** a large play button with a green halo, a small waveform glyph beside the label, the seek bar, and the clock split to the two ends beneath it (elapsed left, duration right).
- **Right half, the choice:** the **Article | Podcast** pill pair on top, the speed chip and **All podcasts** beneath. Divided from the transport by a rule.
- **Article is always LEFT, Podcast always RIGHT**, positions fixed. The lit half IS the answer to "what am I listening to"; the unlit half breathes gently to show the choice exists.
- **Every control is spelled out in full, on a phone too.** "ART" and "POD" saved a few pixels and cost the meaning. On a phone the two halves stack and the rule turns horizontal; all three controls still share one row, the space coming out of padding and letter-spacing, never out of the words.

## Rules that must hold

- **A solid brand fill needs a different green in each theme.** The light palette's `--brand` is already dark enough to carry white text; the dark palette needs its bright `--brand-t`. That is what the local `--pill-on` variable is for, with `--brand-ink` as the matching text colour. Never hard-code a green here.
- **Put responsive overrides BELOW the base rules.** A media query adds no specificity, so a later base rule silently beats an earlier `@media` one. This cost a debugging round on 16 August: the phone layout looked unchanged through several rebuilds because `.aud-right`'s base definition sat after its own media query.
- **Glow with static box-shadows, animate transform and opacity only.** Jay's condition on the breathing highlight was *"that it doesn't slow the page"*. A static shadow costs one paint; transform and opacity composite on the GPU. Never animate box-shadow or filter here, and always honour `prefers-reduced-motion`.
- **The seek bar is drawn by hand**, not left to `accent-color`, so the played portion, the remainder and the handle all match the design. The script paints `--pct` on every `timeupdate`; anything that resets the source must reset it too.
- **The floating dock shares the same `<audio>` element and the same remembered rate.** Any change to the panel's controls must be checked against the dock, which drives the same state from a different set of buttons.
- **Verify against the deployed site, not the local build.** And pick a check that can only pass on the new version: `>Article<` appears in the old markup too, so it "confirmed" a deploy that had not happened. Absence of the old abbreviation spans was the discriminating test.

## Related

[[Podcast - YFarmX Briefings]] · [[Audio and Voice Production (YFarmX)]] · [[YFarmX]] · [[Map - Processes]]
