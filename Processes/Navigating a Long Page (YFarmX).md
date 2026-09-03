---
tags: [process, yfarmx, design]
source: the 2 September 2026 rework of the /space hub, and the verification pass that followed it
updated: 2026-09-03
---

# Navigating a Long Page (YFarmX)

Jay's ask on 2 September 2026 was *"make /space better formatted, better designed UX wise, improve the navigation and clarity within /space"*. The hub was 9,200px on a phone with no way to see what was on it. What follows is what the rework and its verification pass actually taught, kept because every one of these will recur on the next long page.

## One word cannot be both the taxonomy and the signpost

Four consecutive sections on the hub were kickered **"Explore ·"** — the solar system, missions and machines, three ways in, the intelligence desks. "Explore" is a real group in the site's taxonomy, so each was defensible on its own, and together they made the page unreadable: the one word meant to teach the model was the word that destroyed it.

**A section label has to be unique on the page it appears on**, whatever the taxonomy calls it. If two sections would carry the same kicker, the page has two sections that should be one, or a label that is doing taxonomy work instead of navigation work.

## A kicker is not a heading

The kicker was each section's ONLY heading, at 0.66rem mono caps — the same token as the card meta beneath it (`ORBIT · 8 AUG`). So a section start read *smaller* than the card titles inside it, and the newsletter sign-up box carried the largest heading on the page below the H1.

Every section now takes a numbered kicker **and** a real heading in the display face. Check it by measuring, not by eye: the section H2 should be the second-largest text after the H1, and no card title should approach it.

## `scroll-padding-top` and `scroll-margin-top` STACK

The hub added `scroll-margin-top: 7rem` to its sections so an anchor jump would clear the masthead and the sticky index. `global.css` already set `scroll-padding-top: 8.5rem` on the scrollport. The two **add**: 136px + 112px = 248px, so every chip tap landed a third of a screen low with the previous section's tail showing under the strip.

The site-wide `scroll-padding-top` was already correct. The per-section margin was the bug. **Check for an existing scroll-padding on the scrollport before adding scroll-margin to a target.**

## A page can be wider than every element in it

`/space/launches/` scrolled sideways on a 390px phone, at 442px. No element's bounding box exceeded the viewport. No pseudo-element. No transform. Every hunt for an over-wide box came back empty.

The cause was a `<select>` in a grid track. **A `<select>` will not shrink below its longest option**, and in a grid it forces the track to its own minimum. The fix is `min-width: 0` on the control plus `minmax(0, 1fr)` tracks.

The general lesson: when nothing's *rectangle* overflows, look for an element whose **intrinsic minimum** is larger than the space given it — selects, long unbroken strings, `white-space: nowrap`, and anything with a min-content wider than its column. The diagnostic that finds it is comparing `scrollWidth` to `clientWidth` per element, not comparing bounding boxes to the viewport:

```js
for (const el of document.querySelectorAll('body *')) {
  const cs = getComputedStyle(el)
  if (el.scrollWidth > el.clientWidth + 2 && cs.overflowX === 'visible') console.log(el, el.scrollWidth, el.clientWidth)
}
```

## A heading must not outrank its own content

The missions rail was headed **"Live missions, nearest milestone first"**. The fourth card on it is Gaia, whose own record says *"Spacecraft retired; archive programme active"* and *"Sun-Earth L2 (retired to solar orbit)"*. The heading asserted something the card beneath it denied, in the same viewport.

**Before writing a heading over generated content, read what the content can actually contain.** A heading over a data-driven list is a claim about every row the list can produce, not about the rows you happened to see.

## Tap targets and the contrast of the quiet layer

The Space header's icons were 35px and the two-world switcher 26px, both under the 44px thumb floor, and the new index chips shipped at 32px before the verification pass caught them. Separately `--space-dim`, the token carrying **every date and verification stamp in the Space world**, was `#5a616d`: 3.2:1 on the panel colour, under AA for text.

The quiet layer is the one that fails these checks, because it is quiet by design and nobody looks at it. Audit the tokens that carry metadata, not just the ones carrying headlines.

## Related

- [[Space Hub Build (YFarmX)]] — how the section was built and what its identity rules are
- [[YFarmX]]
- [[Map - Processes]]
