---
tags: [process, yfarmx]
source: yfarmx/docs/image-style.md, yfarmx/docs/brand-marketing-prompts.md, yfarmx/docs/playbook.md
updated: 2026-08-08
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

## Restyling real imagery — when accuracy demands the actual object (added 8 Aug 2026)

For real hardware (spacecraft, rovers, stations — the Space missions desk was the first run), do **not** generate a lookalike: research the real photograph, verify its licence, then restyle it image-to-image so the object stays untouched and only the background becomes the house dark starfield. Tooling: `scripts/make-image.mjs --input <source> --model google/gemini-2.5-flash-image` (Nano Banana 2, Jay's instruction for restyles — these images carry no text, so the Pro-only-where-words-appear rule is not in play).

**Licence chain of title, checked per image — non-negotiable on a commercial site:**

- **NASA/JPL** — public domain; credit anyway (`Image: NASA/JPL-Caltech, restyled`).
- **esa.int downloads are barred** — ESA's "Standard Licence" prohibits commercial use. Use Wikimedia copies under CC BY-SA, or ESA images separately released CC BY-SA 3.0 IGO (those allow commercial use with attribution).
- **CC BY-NC is unusable, full stop** — we are a commercial site.
- **Press-kit terms can forbid modification** (Vast/Haven-1 does, beyond formatting): use the image unmodified, credit verbatim as the terms require (`Images courtesy of Vast`), no restyle.
- **No cleanly licensed source → ship without an image** (the Chang'e-7 precedent). Never pass off a generation as the real craft.

**Model quirk:** Nano Banana 2 ignores the aspect-ratio line on image-to-image (7 of 10 outputs kept the source aspect). Do not re-roll — fix deterministically with sharp: letterbox the output onto a blurred, darkened copy of itself at the target frame (1200×675 for the mission cards).

The credit string ships with the image (for the missions desk it lives in `space-missions.json` as `imageCredit` and renders on the card next to `last verified`). Restyled images say `restyled` in the credit; unmodified ones do not.

## The OpenAI image model, and the cheap door (9 Aug 2026)

Jay asked to try ChatGPT's image model in place of Nano Banana Pro (*"On OpenRouter, can you use chat GPT image too instead of NanoBanana Pro?"*). Verdict from the open weights hero and its infographic: **`openai/gpt-5.4-image-2` renders text and logos better than Nano Banana Pro.** Twenty real marks in one collage, every one accurate (including `moz://a` and Anthropic's backslash wordmark), and every word spelled right first time, so `patch_image.py` was not needed once. It is now the first choice where a hero or an infographic carries words.

**The door you knock on changes the price by 6.5x.** OpenRouter has two: the chat-completions route the script always used, and `/api/v1/images/generations`. Measured on the identical prompt and model, same day:

| Route | Aspect | Cost |
| --- | --- | --- |
| chat completions | ignored `16:9`, returned 1024x1024 twice | $0.2376 |
| images endpoint | honoured `aspect_ratio`, returned 1536x864 | $0.0363 |

The chat route has no aspect parameter at all for this model (`supported_parameters` confirms it), and no amount of prompt wording fixes it: two attempts saying "wide landscape banner, do not produce a square image" both came back square. So `scripts/make-image.mjs` gained `--route`, defaulting to **images** whenever there is no `--input`. **Restyles must stay on `--route chat`** because only that route accepts a source image, which is the one thing the cheap door cannot do. Worth re-testing whether the images endpoint fixes Nano Banana 2's aspect-ratio quirk on text-to-image too; the sharp letterbox workaround may be obsolete for everything except restyles.

## Quoting a post from X when you cannot screenshot it (9 Aug 2026)

x.com resets the connection on a headless browser, so a session cannot take a screenshot of a post however the proxy is configured. Two things do work and are enough: **X's own oEmbed endpoint**, `https://publish.twitter.com/oembed?url=<post url>&omit_script=1`, returns the real text, author name and date as JSON; and plain `curl` on the post URL puts the text in the page's `<title>`. Post ids can be scraped off a profile with `curl https://x.com/<handle> | strings | grep -o 'status/[0-9]\{18,20\}'`.

From that, render a **house quote card**: the verbatim text, the handle, the date and the post id on each quote, so a reader can check every line. Two rules on it. Never dress the card as X's interface, because our own rendering presented as an authentic screenshot is a fabricated record. And never generate a tweet image from a model: a real person's words in a made picture is the same offence with worse accuracy. When Jay supplies a genuine screenshot he took himself, that is a real record and can be used as one.

This is also a sourcing route, not just an illustration one. The playbook bars citing rival outlets, and the outlets were wrong twice on the BIP-110 story: they attributed "misguided and unusually careless" jointly to Adam Back and Mark Erhardt when the review thread shows the words are Erhardt's and Back is not in it, and they implied Chris Guida's proof-of-work code was primed when his own post says no activation deadline has been set. **The principals' own posts are primary, free, and more accurate than the write-ups of them.**

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
