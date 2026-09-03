---
tags: [process, yfarmx, images]
source: The Gemini 3.8 Flash and Claude Fable 5.1 heroes, 3 September 2026
updated: 2026-09-03
---

# Fitting supplied art to the 16:9 hero (YFarmX)

**What this is for.** Jay's generated art keeps arriving at **1168x784** (ratio
1.49), and every hero slot on the site is a hard 16:9. This is how to make the
two meet without losing anything and without shipping invented text.

## The trap: the page crops for you, badly

`src/components/ReferencePage.astro` and `src/components/ReferenceHero.astro`
both set `aspect-ratio: 16 / 9` with `object-fit: cover`. So a 1.49 image is
centre-cropped **by the browser**, and nobody sees it happen at authoring time.

On a 1168x784 card that is **127px of height gone**, about 63px off each end.
On the two 3 September heroes that clipped the top of the headline and the
chart's own bar labels. Check before assuming a crop is survivable:

- all 127px off the top → clips the headline
- all 127px off the bottom → clips the bottom row of labels
- split evenly → clips both

When a card is designed edge to edge, **there is no crop that works**. Extend
the canvas sideways instead. 784 x 16/9 = 1394, so it is +113px each side, then
scale to the house 1600x900. Rule 4b (30 August: less cluttered, more clear
space) means the added ground is a gain, not a compromise.

## Adobe generative expand works, and invents text

`image_generative_expand` (Adobe MCP; init with `adobe_mandatory_init` first,
upload via `asset_initialize_file_upload` → PUT the block → `asset_finalize_file_upload`)
continues newsprint texture, torn red blocks and crop marks convincingly.

**It also invents garbled glyphs in the strips it generates** — strings like
"28002 ⑈7" and "H4 1209\4" that read as real annotation at a glance. That
breaks [[Ban invented figures in image prompts]]. On the Gemini card it did it
on **all four seeds tried**, so re-rolling is not a reliable fix.

**Always inspect the generated strips at zoom.** At full size the fault is
invisible. Extract just the new strips into one contact sheet rather than
reading whole images:

```js
// left and right strips of each candidate, side by side
sharp(f).extract({left: 0,       top: 0, width: 150, height: H})
sharp(f).extract({left: W - 150, top: 0, width: 150, height: H})
```

## The fix: rebuild the strips from the image's own texture

Keep the generated output for the **shape** of the torn blocks, synthesise
every pixel of the strips from clean texture sampled out of the same image.

1. **Find a clean tile of each material** by measuring, not by eye. A plain
   paper patch (mean 222.2, sd 7.5, min 140 — no dark marks) and a patch that
   is **100% red** (`(r - max(g,b)) > 38` for every pixel, sd 6.9). Print
   mean/sd/min/non-matching-pixel-% for several candidate boxes and pick.
2. **Mirror-tile** both so there is no repeat seam:
   `tx = x % (2*w); if (tx >= w) tx = 2*w - 1 - tx`.
3. **Mask from the generated output, blurred then thresholded.** Box-blur the
   redness mask at radius 7 so a thin dark glyph stroke cannot punch a hole in
   a block, threshold at 0.5, then blur at radius 1 for a ~2px feather. A soft
   ramp straight off the blur leaves a pale band along the torn edge.
4. **Match the red per row** against the original's own edge columns, smoothed
   vertically (±9 rows) or the sampling noise reads as horizontal banding. One
   constant red is wrong: these cards carry a lighter top-left block (193,71,58)
   and a darker bottom-right one (172,63,52).
5. **Texture, do not flat-fill.** `red = tile(x,y) - tileMean + rowTone`. A flat
   per-row colour reads as a vector fill against real riso grain, obviously.

**No cross-seam feather.** Strips 100% synthetic, original 100% untouched. A
feather blends generated junk back across the boundary — that is how the first
attempt reintroduced a dotted line of invented glyphs at x≈108. Paper tone comes
from the same image, so measure the column means across the seam and confirm
there is no step to hide.

## Do not mirror the original inward

Tempting, and wrong: mirroring the adjacent original into the strip brings real
texture but also mirrors real content. On the Gemini card it would have thrown
a reversed "AI / 2026" into the red block.

## Afterwards

`.jpg` twins for the social cards are generated into `dist` by
`scripts/og-jpegs.mjs` on every build (X does not render WebP), and the
`-480w/-800w/-1200w.webp` variants by `scripts/responsive-hero.mjs`. Neither is
committed. Place only the 1600x900 `.webp` in
`public/media/header-images/` and let the build do the rest.

## See also

- [[Every generated image sets its text in square monospaced extrabold]]
- [[Ban invented figures in image prompts]] — the rule the expand breaks
- [[Image Style and Prompt Libraries (YFarmX)]]
