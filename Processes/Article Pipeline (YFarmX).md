---
tags: [process, yfarmx]
source: yfarmx/docs/playbook.md, yfarmx/docs/publishing-plan.md
updated: 2026-08-06
---

# Article Pipeline (YFarmX)

How an article actually gets onto [[YFarmX]] today, distilled from the newsroom playbook. Golden rule: **the repo is the newsroom** — every article is a Markdown file, every deploy a git push, no CMS.

## The five-step loop (Jay's order of operations, do not improvise)

1. **Research.** Primary sources only, every URL opened before it is cited (tip-off screenshots have given confident URLs that 404).
2. **Deliver the draft to Jay in chat: the full article text written out in the reply, plus the image prompts** (one 16:9 hero, one portrait 3:4 infographic, each self-contained). A file path or "it's committed as a draft" is not a delivery.
3. **Jay comes back** with the generated images and his text edits.
4. **Apply the edits, then make the audio** — in that order, so the MP3 reads the final approved words. An article is not finished until it has audio; never ask whether he wants it.
5. **Publish:** flip `draft: false`, push to **staging**, Jay checks the rendered page at staging.yfarmx.pages.dev, then the Promote workflow ships it live ([[Staging and Backups (YFarmX)]]). Only after promotion do the social waves fire ([[Social Syndication (YFarmX)]]).

"Article" always means the complete package: text, hero, figures, audio, social. Jay should never have to ask for any of it.

## Two publishing doors, one mechanism

- **Pipeline door:** [[Hermes Newsroom Pipeline (YFarmX)]] POSTs JSON to `/api/publish` with `draft: true`; the piece enters the review queue.
- **Jay's door:** the phone-friendly Publishing Desk form at `/desk/` — paste article, SEO fields, image, category, submit. Both doors end in the same thing: one atomic git commit of the article file plus media, which triggers the automatic build and deploy (~2–3 minutes).

**Review queue = the draft flag.** `draft: true` commits the file but the build ignores it; approving is flipping the flag. **Scheduling = a future date** in the front matter; an hourly rebuild timer means a scheduled article appears within the hour after its time passes. Times are UTC (UK summer time is UTC+1).

## What the build enforces (a failed build means fix the copy, not force it)

`npm run build` runs, in order: banned-word lint → Astro build (schema-checked front matter, so a malformed article fails rather than ships) → glossary auto-linking → mobile table wrapping → search index → CSP hashes (a security header pinned to the exact scripts shipped). One banned-word list lives in `scripts/lib/banned-words.mjs` and binds articles, reference pages AND social copy at the same moment — the lists diverged once and bad posts got out.

## Rules that exist because they were paid for

- **Two pushes, article first, then social**, with a wait until the page and hero image both return 200 — social posters scrape the live URL, and queueing both in one push starts the poster mid-deploy.
- **Never change a published media file's bytes under an unchanged URL.** `/media/*` is cached for hours; fixing an image, video or audio file means a new filename (`-v2`) and updating the article.
- **Never promise a future update in copy** ("we will update this piece…"). What is unknown is stated as fact and left there.
- **In brief box is brief:** summary two sentences, 45 words max; four key points max. Enforced at build time for articles from 28 July 2026.
- **Glossary links: right sense or no link.** Words used in their ordinary English sense must not auto-link to a desk definition ("street address" vs crypto address); collisions go in the linker's stop list with the reason written beside them.
- Every article links the first mention of any subject that has a reference page — those pages are the SEO backbone; articles are how they earn authority.

## Freshness

Everything editorial except dated news articles goes stale ("current flagship / latest / price" language). The registry of which pages rot, with review tiers (fast/medium/slow) and per-page `updated:` stamps, is `docs/time-sensitive.md`; a weekly pass re-verifies against primary sources. Never bump an `updated:` date without genuinely revising the page — fake freshness hurts.

## Links

[[YFarmX]] · [[Map - Processes]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Staging and Backups (YFarmX)]] · [[Claude Operating Profile - YFarmX]]
