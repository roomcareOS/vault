---
tags: [process, yfarmx]
source: yfarmx/docs/space/RECON.md, yfarmx/docs/space/MODELS.md, yfarmx/docs/space/BUDGET.md, yfarmx/docs/space/IMAGE-PROMPTS.md, yfarmx/docs/space/IMAGE-PROMPTS-26.md, yfarmx/docs/space/HUB-COMPLETION.md, yfarmx/docs/space/OUT_OF_SCOPE.md
updated: 2026-08-08
---

# Space Hub Build (YFarmX)

The Space section of [[YFarmX]]: a hub, sixteen topic desks (News, Launches, Missions, Satellites, Orbit, Moon, Mars, Science, Robotics, Business, Security, Policy, Defence, Analysis, Opinion, Data) plus an About page, all under `/space/`. **LAUNCHED 7 August 2026** — Jay flipped `SPACE_PUBLIC` by voice instruction that evening (it was his call alone, and remained so; see [[Decisions - Space Hub (YFarmX)]]). The world now carries its own display face (Chakra Petch, loaded only on space pages), decode-on-view headings, a two-world masthead switcher, per-desk banner art on all sixteen desks, and a sister-title band on the main homepage.

## The working method (this is the reusable part)

The build ran as a disciplined agent project, and the shape is worth copying for any future section:

1. **Phase 0 is read-only recon.** Inspect everything, change nothing, write it up (`RECON.md`) — including where the spec and the code disagree, stated plainly, because each disagreement changes the size of a task.
2. **Translate the scope lock, on the record.** The spec assumed a `space/` directory that does not exist; taken literally it forbade every file it asked for. The recon proposed an editable "Space set" and an off-limits list, and asked Jay to confirm rather than deciding silently.
3. **Keep four standing documents:** `RECON.md` (what exists), `DECISIONS.md` (one line per non-obvious choice, with the reason, newest last), `OUT_OF_SCOPE.md` (an append-only register: every request the lock forbids, with the smallest diff that would have satisfied it), `BUDGET.md` (performance numbers **measured, not estimated**, with the method stated and dated blocks added whenever the section changes shape).
4. **End every phase with numbered questions for Jay.** Nothing invasive happens without an answer.

## Honesty rules that shape the whole section

- **No invented data, anywhere** — the About page promises it. Every 3D model is a *class archetype* ("communications satellite"), never captioned with a mission name; every manifest entry carries provenance `illustration`.
- The launch countdown renders only for confirmed windows (a date-only window gets no clock).
- Empty desks render an honest empty state, not fake content.
- **Opinion stays empty until Jay writes it** — signed argument; the house byline is not a person with a view. News needs no article of its own; it renders the whole Space wire.
- Data files carry per-record `sources` and `lastVerified`; "NEVER add a launch without primary sources" is written into the file itself.

## The 3D model library

Sixteen Blender-built class archetypes. **The Python scripts are the source** (`scripts/space3d/`); no binary `.blend` files are committed, so a git-based newsroom can diff and review its own artwork. Each model outputs 8 turntable frames (WebP), a poster, a GLB master (never served to readers — too heavy) and a metadata sidecar folded into a manifest. Scale is **measured by script, not typed by hand**, after a geometry-helper defect made hand-typed figures untrustworthy.

Adding a model, as a checklist:
- [ ] Copy `models/sat_comms.py` (the reference); define `SPEC` and `build()`
- [ ] Keep to the material palette; model in metres; colour only as functional accents
- [ ] Iterate with `--preview` (one small fast frame) until it reads from every angle
- [ ] Run `build_all.py` to render and fold it into the manifest

## Performance discipline

Measure with a stated method, then attack the biggest number first. The case study: the hub loaded 3,558KB on a phone against a 2MB budget, and one hero video was 2,529KB of it. A moderate denoise before re-encoding cut it 79% with no visible damage (checked frame against frame); a phone rendition took it to 353KB. Modern codecs were **tested and lost** on this footage, so the spec's WebM rule was deliberately not followed — measurements beat rules. By 2 August the hub's initial mobile transfer was **595KB**, with the 3D viewer loading lazily and costing no GPU while idle.

## Artwork rules (26 prompts written and waiting)

Two rules override every image prompt:
1. **No invented data in artwork.** Every figure in an infographic must be one the article carries and the source states. `[FROM ARTICLE]` placeholders must be filled from the reporting, never by the image tool. (The cautionary case: a shipped infographic carried a rating no source stated.)
2. **No decorative chart shapes.** An unlabelled bar chart is filler that looks like evidence.

And an ordering rule: **write the article before the hero prompt** — art that predates the reporting invents the picture, and the copy then gets written to match it.

## Launch blockers (as recorded 30 July – 2 August 2026)

- [ ] Articles: ten desks empty (four drafts written: Science, Security, Mars, Moon)
- [ ] Art: no article hero exists; six desks share banners; no image key in the build environment
- [ ] Audio: none — the playbook treats audio as part of the article
- [ ] Jay's review: drafts stay `draft: true` until Jay reads and edits them (the only stop in the loop, same as the [[Article Pipeline (YFarmX)]])
- [ ] CLS (layout shift) 0.0588 against a target of zero — untraced; font swap is the hypothesis, not the finding

The honest summary from the completion doc: the machinery works; what the section lacks is journalism, and the next unit of work is articles, not features. (The [[YFarmX]] status snapshot records the Space desk's first live news article on 5 August.) **Post-launch note, 7 Aug:** the shared-banner art blocker is resolved (every desk owns its banner); empty desks still render their honest empty states, and articles remain the real work.

## Mission imagery (8 Aug 2026)

Eleven of the twelve missions on the Missions desk now carry real imagery of the actual craft, restyled onto the house starfield (Chang'e-7 ships imageless — no cleanly licensed source exists; Haven-1 is unmodified because Vast's press terms forbid alteration). The class-archetype rule above still governs the **3D models**; photographs of the real, named craft are a different category and are captioned and credited as such. The sourcing, licensing and restyle workflow is recorded in [[Image Style and Prompt Libraries (YFarmX)]] — read it before adding any mission or hardware image.

## Traps the 7 August adversarial review caught (worth remembering)

Twenty-four confirmed findings, all fixed before the production push. Three generalise beyond this build:

- **A failed dynamic `import()` is memoised by the module map** — retrying the same specifier rejects instantly from cache without touching the network, so an in-code retry of a chunk import is largely theatre. Design the fallback (here, the sprite drag) rather than the retry.
- **`@fontsource` imports in a layout leak beyond it.** The `[slug]` article shell imports both world layouts statically, so a font CSS import in SpaceBase became a render-blocking stylesheet on 500-plus article pages that never draw the face. A hand-written `@font-face` over a self-hosted woff2 in the world's stylesheet costs nothing until an element computes to the family.
- **Treat a superseded async load as benign, never as the machinery failing.** A picker click during the viewer's boot rejected boot's own load with 'superseded', and one catch-all marked WebGL dead for the rest of the page view. Separate "the machinery broke" from "a newer request overtook this one" in every loader.

[[Map - Processes]] · [[Decisions - Space Hub (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Robotics Launch Checklist (YFarmX)]]
