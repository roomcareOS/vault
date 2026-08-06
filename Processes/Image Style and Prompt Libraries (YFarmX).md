---
tags: [process, yfarmx]
source: yfarmx/docs/image-style.md, yfarmx/docs/brand-marketing-prompts.md, yfarmx/docs/playbook.md
updated: 2026-08-06
---

# Image Style and Prompt Libraries (YFarmX)

The house art direction for [[YFarmX]], plus an index of the prompt libraries kept in the repo. Jay generates the images himself from pasted prompts, so every prompt ships **self-contained**: shape, full style, every object and logo described inside one block, nothing referring to "the house style" or "the previous prompt".

## The hero look (news articles and explainers)

A **dense, layered magazine-collage in a cyber-dossier style** — a high-end investigative-tech cover, not a tidy studio photo. The one-line test: *bright, packed with real objects and logos, a huge headline built into the art, black-and-white with selective colour pops, and it obviously symbolises THIS story.*

- **Bright, never dark or moody:** off-white halftone newsprint background, torn-paper edges, red/blue/green registration squares in the corners.
- **High-contrast black-and-white photography with colour as a spotlight** — the subject's real brand colours plus the desk accent. No neon, no rainbow.
- **The big headline names what the article is ABOUT** (a cover names its subject); the sharp secondary fact goes elsewhere in the scene.
- **Desk accents:** AI blue, Crypto green, Quantum purple, Security red. Red is not the default.
- **Always the real logos** (Jay's hard rule: never write "no logos" into a prompt) — and every logo eyeballed against the company's official site before shipping, because generators invent marks. Known traps recorded: Anthropic's mark is the wordmark plus "AI" monogram, the orange sunburst is Claude's.
- **BANNED since 30 July 2026: dark rounded HUD/data panels and red rubber stamps.** Both were house furniture and both are barred (the panels made every image identical; the stamps did a headline's job). Figures go into the scene instead: a torn spec sheet, a paper chart, a machine plate, an evidence tag. *Note: the template and worked example still inside docs/image-style.md pre-date this ban; the ban wins.*
- **Space and robotics desks are the exception:** dark field, few superb objects with room to breathe, one or two words of headline, red-green-blue mixed. Not the collage.
- **Infographics are PORTRAIT 3:4** so labels stay legible on a phone. Heroes are 16:9.
- **Never ship a placeholder or stand-in hero** — write the prompt, put it in front of Jay, wait for the real art.
- **Patch wrong text in a generated image, do not flag it** (`scripts/patch_image.py` repaints text on its tilted surface); regenerate only if the whole composition is wrong.
- Model: always Nano Banana Pro (`gemini-3-pro-image`) where words or figures appear — the cheaper model garbles text.

## The brand look (marketing, app store, banners — NOT article heroes)

Our own identity is the opposite register: clean, premium, confident. The flat 2x2 puzzle-cube mark (blue #015ee5 / red #f51e13 / green #01832c, never 3D), the "YFarmX" wordmark in Inter 800, two approved straplines only ("Intelligence for an accelerating world", "the frontier of compute"), generous negative space. The dossier collage is reserved for news.

## Prompt-library index (the prompts live in the repo; do not duplicate here)

| File (in `yfarmx/docs/`) | What is inside |
|---|---|
| `brand-marketing-prompts.md` | 30 paste-ready brand/marketing prompts (app icon, splash, store graphics, banners, print) in the clean brand register |
| `explainer-hero-prompts.md` | Heroes for 20 reference/explainer pages, colour assigned by desk |
| `coin-hero-prompts.md` | 46 evergreen coin-page heroes — no prices or ranks, only facts that do not move; colour varies per coin's real brand |
| `video-background-prompts.md` | 30 quiet 9:16 background plates for Remotion article videos (stills; Remotion adds the motion) |
| `opus-5-image-prompts.md` | Hero + infographic pair for the Claude Opus 5 article, with the Anthropic/Claude logo trap spelled out |
| `image-prompts-articles-2026-07-30.md` / `-08-01.md` / `-08-05.md` | Per-date article prompt batches (hero + portrait infographic per article), post-ban versions |
| `image-prompts-qwen3-8-max.md` | Hero for the Qwen3.8-Max model page (only the hero needed) |
| `image-prompts-remaining.md` | Nine reference pages still on placeholders — single unified scenes, not panel grids |
| `image-prompts-robotics-explainers.md` | The 12 robotics explainer heroes, 16:9 |
| `image-prompts-robotics-space.md` | Robotics sub-hub and (hidden) Space world art |
| `image-prompts-subhubs.md` | 22 sub-hub banners: Robotics (6) and Space (16) |
| `flow-prompt-pack.md` | Google Flow (Veo) hero-animation prompts — living stills only, image-to-video from the actual header file, text and logos frozen, one or two things moving |

## Links

[[YFarmX]] · [[Map - Processes]] · [[Article Pipeline (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Decisions - YFarmX]] · [[Robotics Launch Checklist (YFarmX)]] · [[Sticker Prompt Pack (Intervooh)]]
