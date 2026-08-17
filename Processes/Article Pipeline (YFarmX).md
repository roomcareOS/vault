---
tags: [process, yfarmx]
source: yfarmx/docs/playbook.md, yfarmx/docs/publishing-plan.md
updated: 2026-08-17
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

**The body itself carries media (Jay, 17 Aug 2026, decision 65): one compact animation and at least two screenshots or images breaking up the text.** Primary-source screenshots first — the notice, the filing, the product screen, captured ourselves (the 15 August crypto batch carried six; the DeepSeek Harness page nine). The animation is a short silent loop sized like a figure, not a video player. Unbroken text is an incomplete article: the two 17 August pieces shipped without any and prompted the rule.

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
- **In brief box is brief AND easyish to follow:** summary two sentences, 45 words max; four key points max, enforced at build time for articles from 28 July 2026. **Restated 9 Aug 2026 because the length rule alone did not stop it:** the BIP-110 brief shipped carrying `0x20006000`, "bit 4 unset" and "mandatory signalling", all accurate and all closed to anyone outside the subject. The brief is the layman's door; the body keeps the hex and the block heights. Test before shipping: could somebody who does not read this desk finish the brief and say what happened and why it counts? Not a simplification and never patronising, which is the older half of the same rule. Forward-looking only, the archive stands.
- **Glossary links: right sense or no link.** Words used in their ordinary English sense must not auto-link to a desk definition ("street address" vs crypto address); collisions go in the linker's stop list with the reason written beside them. **Since 8 Aug 2026 the double check is enforced by the build:** a link that crosses desks (a crypto term on a space page) must be in the linker's reviewed-pairs file or the build fails and prints the sentence; a session reads it and either records the pairing (`GLOSSARY_ACCEPT_CROSSDESK=1`) or stops/scopes the term. Never set the accept flag without reading the sentences — the flag IS the review. Playbook §4 and yfarmx decision 51 carry the detail; the trigger was the DEX "slippage" tooltip landing on schedule slippage.
- Every article links the first mention of any subject that has a reference page — those pages are the SEO backbone; articles are how they earn authority.
- **No closing "what this story is not" section (Jay, 9 Aug 2026, banning it on the third attempt).** The caveats section had survived two earlier rules by renaming itself: "what is not known", "what is not settled", "what is established, and what is not". Same tic each time, the piece stopping to discuss its own sourcing instead of ending on the story. The honesty goes in the sentence that makes the claim, where a reader needs it: attribute in place ("Back's answer was flat: no blocks after the first 2"), label confidence in four words ("a US assessment", "the company's own figure"), and let the `sources` list be the audit trail it already is. End on the consequence, the open question or the best quote. Related and older: never explain the methodology on the page (8 Aug), and never build the piece around what was NOT said (28 July). Nine legacy articles still carry the old pattern and are left alone.

## Freshness

Everything editorial except dated news articles goes stale ("current flagship / latest / price" language). The registry of which pages rot, with review tiers (fast/medium/slow) and per-page `updated:` stamps, is `docs/time-sensitive.md`; a weekly pass re-verifies against primary sources. Never bump an `updated:` date without genuinely revising the page — fake freshness hurts.

## Links

[[YFarmX]] · [[Map - Processes]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Staging and Backups (YFarmX)]] · [[Claude Operating Profile - YFarmX]]
