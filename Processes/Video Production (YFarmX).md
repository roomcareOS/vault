---
tags: [process, yfarmx]
source: yfarmx/docs/video.md, yfarmx/docs/video-light-theme.md
updated: 2026-08-06
---

# Video Production (YFarmX)

How a [[YFarmX]] article becomes a vertical cut for TikTok, Reels and Shorts. It is a repeatable renderer, not a hand-edit: point it at an article and it turns that article's own assets into a 1080x1920 film with voiceover, burned-in captions and original sound. Posting the finished file is [[Social Syndication (YFarmX)]].

## Output spec (fixed)

- **1080x1920, 30fps, H.264 video + AAC audio**, roughly 35 to 47 seconds.
- **Burned-in captions**, because most of the audience watches on mute.
- **Safe area:** everything readable between y=120 and y=1600. The top 8px is the progress rail; the band below y=1600 is left to the app's own buttons. Vertical throughout, so the same file fits Reels and Shorts with no re-cut.
- **Heroes are 16:9 and are never cropped to fit.** Cropping one to 9:16 throws away two thirds of a collage built to be read, so the `hook` scene keeps the plate's own shape and fills the rest of the frame with a blurred, darkened copy of itself. Do not "fix" this by cropping.
- **Where files land:** renders to `remotion/out/<name>.mp4`; hand-made cut-outs are committed at `remotion/public/cutouts/<cut>-<object>.png`; fonts are derived into `scripts/video/fonts/`.

## Which renderer

Three directories exist and they all obey the rules below.

- **`remotion/`** — Remotion (a renderer that composes each frame in a real browser with React). The intended survivor: it gets proper text layout, real CSS filters and spring physics instead of hand-drawn graphics, and a scene is a component you can preview live.
- **`scripts/video/`** — the original, composing frames in PIL (the Python imaging library). Still works; single-threaded and slower.
- **`scripts/video/remotion/`** — **a duplicate awaiting Jay's call.** On 30 July 2026 two sessions were given the same ask for two different articles and each built a Remotion pipeline without knowing about the other. The repo should end up with one. The plan is to keep `remotion/` as the host (it landed first, and its `cut.json` data format is the better one), port over the `versus` and `ledger` scenes, the banned-words gate and the figure fitting, then delete this directory. **Not done, because which host survives is Jay's decision.**

Sound is shared whatever the renderer: the voice and the sound design always come from `scripts/video/`, so every cut sounds identical.

## Commands

```
# PIL renderer
pip install imageio-ffmpeg numpy fonttools brotli gtts pillow
python3 scripts/video/getfonts.py            # woff2 -> ttf, from node_modules
python3 scripts/video/build.py zcash microsoft att
```

```
# Remotion
cd scripts/video/remotion
npm install
node prepare.mjs                             # stage, gate, speak, mix, write timeline
npx remotion render src/index.ts vertical out/<name>.mp4
npx remotion studio                          # live preview
```

- Python dependencies are deliberately not in `package.json` — only this step needs them. `imageio-ffmpeg` ships its own static ffmpeg binary, so nothing is installed system-wide.
- `prepare.mjs` rebuilds everything derived (staged fonts and images, the grain tile, the copy gate, the spoken lines, the mix, the timeline), so a fresh clone needs no manual copying. Only source is committed.
- Render is about **four minutes per video** either way. Remotion runs headless, with no network access during the render, at `--concurrency=4`.
- Remotion ships a full ffmpeg build, so no system install is needed — but **its filter set is whitelisted**: `atempo` and `volume` are in, `highpass` and `acompressor` are not, so those two stages of the approved voice chain run in numpy instead. Same shape, different execution.

## The standing rules (Jay, 30 July 2026 — every renderer)

1. **The logo is the cube, and it is never redrawn.** Load `public/logo-cube.svg` or `public/logo-cube.png` — an isometric 2x2x2 cube, blue top face, red left, green right. Jay's master is `public/media/brand/yfarmx-cube-master.png`. Both outros used to *draw* a flat four-square shape that was not our logo.
2. **No noise. The sound is tonal.** A broadband noise floor under a voice reads as sea or static. Everything is built from sines instead: a low two-chord pad moving every eight seconds, soft struck notes for data points, a pitch-drop thud for a beat, a tonal swell for a rise. The old whoosh, riser and bed-air layers are noise and are retired; a whoosh has no tonal equivalent, so it is simply gone. Serious business bulletin, not sound effects. **This is measurable** — the old bed put 36% of its energy above 6kHz, the tonal bed puts 0.06%, and a finished mix reads 0.15% to 0.34% in the gaps between lines. If a future cut sounds like surf, measure the bed before redesigning anything.
3. **Write it for someone who has never heard the jargon, and write it as one story.** Not a list of facts: a beginning (what changed), a middle (why it was worth doing, with proof), an end (what it means next). The plain phrase is the label and the trade word goes underneath it, small, so a viewer learns it without needing it first — THE TOOLS / *harness*, WHAT IT SEES / *context*. Say what a thing is before quoting its number. **Open with the news** — who, what, when — and hold any twist to the last line, where the viewer has the context to feel it.
4. **Show the receipts.** A claim on a card is an assertion; the same claim on the company's own page is evidence. `scripts/video/shots.mjs` captures those pages and can scroll to a phrase before the shutter, so the frame lands on the sentence the scene is about. **Capture at about 620 CSS px wide, not 900** — a 900px page squeezed into the plate sets type too small to read on a phone.

## Imagery (Jay, 5 August 2026)

**Every scene carries a picture.** A counter or a statement on bare paper is not acceptable. Each scene takes two layers, wired from the cut's own props: a `wash` (the article's hero, blown up and bleached back into the sheet, so the frame has a subject without competing with the type) and `cutouts` (one or two objects lifted out of that same artwork, sitting proud of the page with a hard print shadow and a slow drift).

**A cut-out is a whole object with no background.** Slicing fixed fractions out of the hero shears through objects and leaves each one on a rectangle of paper. The method that works:

1. Crop **wide** of a whole object, with margin — never let a bound cross the object.
2. Remove the background with `rembg` (u2net) — `pip install rembg onnxruntime`.
3. Trim to the result's own alpha bounding box, so no transparent margin survives.
4. Save to `remotion/public/cutouts/` and **commit it**. These are hand-chosen crops, not a build product.

**Where the picture comes from matters more than how it looks.** A viewer reads any image under a sentence as evidence for it, so an unrelated image is a false caption. Use the article's own artwork, our own published page, the specific document being cited, or a drawn background plate. **Do not shop in `public/media`** — it holds 638 images and almost none of them are about the story in front of you. Drawn plates follow the same rules as the prompt library ([[Image Style and Prompt Libraries (YFarmX)]]): no type, calm through the middle, interest in the upper half, one accent; drawn brighter than they look on screen, because the backdrop darkens them again behind the type.

## Copy in a video

Everything from [[Article Pipeline (YFarmX)]] applies — primary sources only, no invented quotes, the banned-words list, British English. Four rules bite only here:

- **The copy is gated in code.** `prepare.mjs` runs `scripts/lib/banned-words.mjs` over every spoken line and every caption and refuses to build on a hit. Video copy was the last path where the house list depended on whoever was typing.
- **Every spoken line needs a caption.** The build fails on a mismatch.
- **The spoken line and the caption may differ, and often should.** A voice reading "three million, four hundred and twenty eight thousand, one hundred and forty three" wastes six seconds; the caption shows `3,428,143` while the voice says the sentence. Set the pairing in `CAPTION_FIX`.
- **Numbers on screen must be the real ones.** Counters animate up from a lower value, and the figure they land on is the article's figure, which is the primary source's figure.

## Timing

**Timing follows the voice, not a stopwatch.** Every line is spoken first, measured with ffmpeg, and the scene that owns it is stretched to fit. That is why captions are frame-accurate with nobody syncing them by hand, and why editing a line automatically re-times the video around it. Durations come from `timeline.json`; no scene invents a length.

## Scenes

| Scene | What it is for |
|---|---|
| `hook` | the hero letterboxed over a blurred blow-up of itself, headline stamping in a word at a time |
| `bignum` | the one figure the story turns on, counting up to its true value |
| `statement` | three lines of display type, revealed word by word |
| `versus` | two panels, one figure each, where the distance between two numbers *is* the story |
| `bars` | label, animated bar, value, with one row flagged as the odd one out |
| `ledger` | a sequence of values as bars, log-scaled and labelled as such |
| `figure` | the article's own infographic, framed as a dossier plate and drifting |
| `screenshot` | a published page itself, scrolling in a browser plate |
| `outro` | the cube, the wordmark, the headline and the URL |

`versus` and `ledger` currently live only in the duplicate directory and are part of what gets ported. Both generalise: any piece with a "what you are shown versus what is true" pair, or an escalating sequence, can use them.

## The light theme

Jay's brief: *"not dark gloomyness. only light energetic clean vibe."* The reference is the hero look in [[Image Style and Prompt Libraries (YFarmX)]] — off-white halftone newsprint, high-contrast subject, a few real accent colours, corner registration squares. The hexes below are final.

**The governing idea.** The dark theme carried energy with *emitted light* — glows, bloom, white flashes, radial haloes. None of that exists on paper. The light theme carries the same energy with **ink**: hard-edged blocks of solid colour, hard offset shadows (a printing plate sitting proud of the page), high value contrast, and speed. **Every `box-shadow: 0 0 Npx accent` in the codebase is a glow and must die**, replaced by a solid border, a hard offset shadow, or a scale/position move. Second rule: **the plate is paper, the type is ink, the accent is a spot colour** — a solid block or solid type, never a wash, never a 0.2-alpha tint standing in for a glow, never a gradient fading to transparent.

### Tokens (`remotion/src/theme.ts`)

| Role | Values |
|---|---|
| Paper | `BG0 #f4f1ea` the plate · `BG1 #ffffff` raised (cards, caption band) · `BG2 #eae5db` recessed (bar troughs, rails, browser chrome) · `LINE #d8d2c6` hairline · `INK #12161b` |
| Ink ramp | `TX1 #12161b` headline/body · `TX2 #48525c` secondary · `TX3 #5f6870` tertiary |
| Desk accents (ink) | ai `#1b4dd8` · crypto `#00734e` · quantum `#6d28d9` · security `#c2261d` |
| POP (fill only) | `#2f6bff` · `#00a870` · `#8b5cf6` · `#e23a2e`, reached via `popOf(accent)` |
| Registration triad | red `#c2261d` · blue `#1b4dd8` · green `#00734e`, fixed and desk-independent |
| Shadows | `PRINT(dy, a)` hard un-blurred offset · `LIFT(dy, blur, a)` soft lift |

Scene signatures do not change: they keep taking the ink accent, and call `popOf()` where they need a fill.

### The contrast rules that generated the spec

Contrast ratios are the WCAG legibility measure; **4.5:1 is the readable floor** and every figure was computed, not estimated.

- **Every ink and accent clears 4.5:1 on all three paper surfaces.** `TX3` on `BG2` is the tightest at 4.52, which is exactly why `BG2` is `#eae5db` and not the darker `#e6e1d6` first tried (4.35, failed).
- **POP is fill-only and fails as text on purpose** (2.72 to 3.99) — those colours exist to be looked at, not read. If a POP fill ever has to carry type, the type is `TX1`, **never white**. White on the four *ink* accents is 5.85 to 7.10 and is fine.
- **No faded ink.** `TX1` at 45% over the paper composites to 2.91:1. This is the single most common dark-theme habit that breaks on paper — it worked on black because faded *white* stays bright. Use a solid `TX2` or `TX3`.
- **No `rgba(accent, ≤0.5)` for anything readable.** The AI blue at 35% composites to 1.75:1. Accent type is the solid ink accent at opacity 1.
- **Carry on/off states with weight of ink or a hard switch** (`TX3` → `TX1`, hairline → solid accent), never with opacity. Binary reads better on light than a fade.
- **The red is `#c2261d`, a vermilion press red, not `#ff3a3a`.** The old red is 3.15:1, and at full saturation on off-white it fluoresces — it is the one colour that makes a clean editorial frame look like a warning banner, and it fights the newsprint red in the heroes. Settled, do not re-litigate: `#b91c1c` is safer but goes maroon; `#7c3aed` and `#047857` both fail inside a bar trough on `BG2`.

### Standard substitutions

The spec works through the codebase file by file, but every line is one of these moves:

- glow → a hard offset `PRINT` shadow, a solid border, or a spread ring (rings read on paper; blurs do not)
- white flash → the `Snap` ink-block wipe
- white grid, track or rule on white → `BG2` with an inset `LINE` hairline
- dark scrim → a **paper bleach** (`rgba(244,241,234,0.74)`); black vignette → a burn-out to paper at the edges, so the frame gets brighter towards the corners
- grain in overlay blend → **multiply**, so the turbulence reads as paper fibre
- CRT scan lines → a **halftone dot screen** (6px radial dots, plus a second pass offset 3px for the rosette). The single biggest "it looks like our heroes" win in the whole spec.
- the travelling white scan line → an **ink roller sweep**: the same band in ink, transient and 180px tall
- corner brackets stay, and the three registration squares are added inside them
- **the one legitimately dark element is the `Swap` socket** (`INK`). A socket is a hole, and a hole in paper is dark; it is the only place the eye reads recess.

### The caption band (opacity 1.0, always)

The band has to be legible over a hero photo, over a bordered screenshot, and over an infographic that is 90% white paper. A translucent plate solves the first two and fails the third — and the burned-in caption *is* the copy. So the band is a **fully opaque white card**: `background: BG1` (never `rgba`), `border: 3px solid accent`, `boxShadow: 0 10px 0 rgba(18,22,27,0.16), 0 0 0 1px rgba(18,22,27,0.10)`, `color: TX1`, `fontWeight: 700`, `borderRadius: 22`.

Why each piece: opaque white makes 18.16:1 a guarantee rather than a hope, since nothing behind the band can change it. Over a white infographic the plate has no luminance edge, so the accent ring *is* the edge. The hard un-blurred shadow draws a crisp dark line under the card — a blurred one would smear into white and vanish. The 1px ink halo outside the border separates the band from a frame that happens to be the same hue as the desk accent. Weight goes 650 → 700 because dark-on-light type appears thinner than light-on-dark at the same weight.

### Energy without gloom

There is no headroom above white, so energy comes from hard edges, spot colour, offset and speed.

- **`Snap`** — a skewed bar of vivid POP crossing the full width in about four frames, over a 10% ink multiply. Loud, over in 130ms, and the light-field equivalent of a strobe. Fires wherever the dark theme fired a white flash.
- **Highlight-marker sweep** — POP at 0.22 alpha in `mixBlendMode: multiply`, wiping 0 → 100% width behind an emphasised line over 0.28s. Multiply is essential: it stains the paper like a marker pen instead of washing over the ink. There is no dark-theme equivalent, so use it freely.
- **Misregistration snap** — draw the type twice, the offset copy in POP at 0.55 alpha, snapping into register over five frames. Reads as a colour plate landing on a press.
- **Hard shadows that move** — animate the offset with the element (`PRINT(4 + 8 * k)`). A growing hard shadow reads as an object leaping off the page; it works by shape, not luminance, which is why it survives on white.
- **POP fills only ever appear in motion** — bar fills, the Kitt block, the wipe, the progress rail, an underline growing with a count. Static furniture uses the ink accent. The effect is that the only fully saturated colour on screen is the thing that is moving.
- **Registration squares blink on every cut**, red then blue then green, two frames apart. Tiny, but it puts a beat of three real colours in the corners of every scene.
- **Speed up, because slow reads as static on white:** Kitt period 2.4 → 1.8 and width 340 → 150; scan-line speed 520 → 760; word stagger 0.085 → 0.07 in `Hook` and 0.1 → 0.085 in `Statement`. Type should land like keys on a typewriter. Nothing else about the timing model changes.

### Order, and what counts as done

Implement in this order: `theme.ts` tokens → the hard-coded background hex in `Video.tsx` → `Chrome.tsx` (this fixes most of every frame) → the caption band → the media plate → scenes, `Hook` first → the energy moves last, once the flat look is right.

**Not done until:** one still per scene kind is rendered at 1080x1920 and viewed at phone size; nothing has disappeared into the paper; the caption band is checked over the `figure` and `screenshot` frames specifically (they are the failure case); and `grep -rn "255,255,255" remotion/src/` returns nothing but the `Snap` block's own alpha and the grain filter.

## Traps, each of which cost a render

- **Do not hand-roll font loading — use `@remotion/fonts`.** A hand-written `FontFace` loader behind a `delayRender` handle killed two passes, at frame 82 and frame 636: on a tab Remotion opened part-way through a long render the load hung without resolving *or* rejecting, so the handle could never clear, and the timeout blamed the font loader rather than the cause. `remotion/` uses `loadFont` with `staticFile` and rendered 1268 frames without trouble, so fetching a font at render time is not itself the problem.
- **SVG `feTurbulence` grain is a five-fold render tax** at this canvas size. A 256px fixed-seed tile, offset per frame at 5.5% opacity, reads the same.
- **Fit big figures to the column; never hand-set a size.** A fixed 176px silently clipped `$19,208,785` to `$19,208,78` and pushed the currency suffix clean off the canvas. The size is now derived from the widest value the counter can land on.

## Sandbox limits (do not "fix" these)

- **Chromium here cannot decode H.264.** A `<video>` element sits at `readyState 0` with `videoWidth 0`, and that tells you nothing about real browsers, which decode it in hardware. Verify layout by the poster frame and the element's geometry, and trust the standard libx264 yuv420p `+faststart` recipe — it is what every mp4 already playing on the site uses. Do not re-encode a video because the local check will not play it.
- **Chromium cannot browse out**, but that no longer limits screenshots: `shots.mjs` intercepts every request and fulfils it from Node's fetch, which the proxy does serve, so a third party's own announcement page captures fine.

## Related

[[YFarmX]] · [[Map - Processes]] · [[Home]] · [[Article Pipeline (YFarmX)]] · [[Audio and Voice Production (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]]
