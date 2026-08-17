---
tags: [process, yfarmx, images]
source: The Index pages overhaul, 17 August 2026
updated: 2026-08-17
---

# Cutout objects from Nano Banana 2 via chroma key (YFarmX)

**What this is for.** Photoreal transparent-background objects — floating hero
props, animation components, collage pieces — generated in-session without any
background-removal service. First used for the Index pages' floating objects
(robot head, Bitcoin coin, cryostat chandelier and six others), which shipped
on 17 August 2026 and live in `public/media/index-art/`.

**The recipe.**

1. **Prompt for a chroma ground, not transparency.** Ask for "a single
   photorealistic object … perfectly centred and fully inside the frame with
   generous margin … isolated on a SOLID FLAT PURE GREEN chroma-key
   background, hex 00FF00, with nothing else in frame and no cast shadow on
   the background". Square 1:1. Nano Banana 2 (`gemini-3.1-flash-image`)
   follows this reliably and all nine Index objects keyed on the first
   generation.
2. **Keep the object text-free.** NB2 garbles rendered text, and a cutout
   needs none: say "Render NO text, no letters, no words and no numbers
   anywhere in the image". Emblems that are marks, not words (the Bitcoin B,
   the Ethereum diamond), come out clean. The prompt still carries the house
   type clause so `check-prompt.mjs` passes it, phrased conditionally ("if
   any lettering were to appear…").
3. **Key on green dominance, not green colour.** For each pixel take
   `dom = G - max(R, B)` and ramp alpha from opaque at dom ≤ 24 to
   transparent at dom ≥ 90. Keying on the raw green channel eats golds and
   yellows; dominance leaves them alone.
4. **Despill.** Where dom > 0, clamp `G = min(G, max(R, B) + 6)`. Without
   this every edge wears a green fringe.
5. **Green objects need border-connected keying.** The vault-wheel cutout
   has a genuinely green glow ring; global keying would erase it. Flood-fill
   the green-dominant region from the frame edges only and key just that
   connected region — enclosed greens survive. (The reverse trap: an object
   with see-through gaps, like the armillary sphere, needs GLOBAL keying so
   its enclosed background windows go transparent too. Pick per object.)
6. **Finish.** Feather the matte (0.8px gaussian on alpha), trim to the alpha
   bbox, pad ~12px, resize to ≤ 720px, save as WebP with alpha
   (quality 88). Review every cutout on BOTH a dark and a light swatch —
   the site has two themes and a bad matte only shows on one of them.

**Placement pattern that worked.** Three objects per hero in fixed slots
(large bottom-left, medium right, small blurred top-right for depth), CSS
idle-drift keyframes at different durations, pointer parallax scaled by a
per-object depth factor, and everything behind `prefers-reduced-motion`.
Implementation: `src/components/IndexHero.astro` in the yfarmx repo.

**Related.** [[Every generated image sets its text in square monospaced extrabold]] ·
[[Screenshotting external sites from a Claude session (YFarmX)]] ·
[[Image Style and Prompt Libraries (YFarmX)]]
