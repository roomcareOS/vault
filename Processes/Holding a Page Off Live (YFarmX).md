---
tags: [process, yfarmx, deploys]
source: the 18 September 2026 near-miss on /ai/models/jev/, caught reading dist before a live deploy
updated: 2026-09-18
---

# Holding a Page Off Live (YFarmX)

When Jay holds a page back, **set a flag. Do not rely on `--allow-drop`.**

`scripts/deploy-live.sh --allow-drop=/some-page/` removes a page from **one deploy and nothing more.** It is the named exception that lets a single deploy delete a live page past `predeploy-check` ([[A Deploy Replaces the Whole Site (YFarmX)]]). It says nothing about the next deploy, and the build keeps emitting the page.

## The near miss

On the morning of 18 September 2026 Jay held the Jev decision desk off live: the TypeSafe Jev dataset record goes live, `/ai/models/jev/` comes off. The removal ran with `--allow-drop=/ai/models/jev/` and was verified at the origin as 404, exactly as instructed.

Every build afterwards still emitted the page, at 155,769 bytes, **with a sitemap entry.** `predeploy-check` guards removals, not additions, so nothing objected. The next live deploy from any branch would have republished it.

It was caught that evening, by one `ls dist/ai/models/jev` before a live deploy. Nothing else would have caught it: the page is a `src/pages/` route rather than a content entry, so `scripts/staging-queue.mjs` did not list it under PUBLISHES either.

## The shape to use instead

Four parts, already proven by the competitions section:

1. **A switch** in `src/lib/flags.mjs`, with the instruction and its date in the comment.
2. **A holdback script**, `scripts/<thing>-holdback.mjs`, that deletes what the build emitted. A plain `.astro` route has no `getStaticPaths` to empty, so deleting the output is the equivalent of not building it. Wire it into the `build` chain in `package.json`.
3. **A sitemap filter** in `astro.config.mjs`, so the URL is never submitted.
4. **A `predeploy-check` refusal** for any live dist still carrying the path.

Nothing under `src/` is touched, so flipping the flag to true relaunches the page whole. Live examples: `COMPETITIONS_PUBLIC`, `JEV_DESK_PUBLIC`, `MODEL_DATA_FILES_PUBLIC`.

## The check worth keeping

**Read `dist` before a live deploy, not just the staging queue.** The queue lists content pages that would newly publish. It cannot see a page route, a held section, or anything the build emits from `src/pages/`.

```
ls dist/<held-path>            # should not exist
grep -c '<held-path>' dist/sitemap-0.xml   # should be 0
```

Both take seconds, and between them they are the difference between a holdback and a hope.

## Related

- [[A Deploy Replaces the Whole Site (YFarmX)]] — where `--allow-drop` comes from, and what it is actually for
- [[Overwriting a Stale Pages Asset (YFarmX)]] — what happened next on the same URL
- [[Pipeline Security Rules (YFarmX)]]
- [[YFarmX]]
- [[Map - Processes]]
