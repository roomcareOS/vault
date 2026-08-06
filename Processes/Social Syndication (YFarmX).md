---
tags: [process, yfarmx]
source: yfarmx/docs/social-linkedin.md, yfarmx/docs/social-tiktok.md, yfarmx/docs/playbook.md, yfarmx/docs/social.md
updated: 2026-08-06
---

# Social Syndication (YFarmX)

How a published [[YFarmX]] article reaches X, LinkedIn and TikTok. Everything for X and LinkedIn is a queue file in the repo; a GitHub Action (an automated job that runs on a schedule or on changes) posts whatever is due. TikTok is manual.

## Timing (Jay's rule, final)

The moment an article is live and verified, it posts. **X and LinkedIn always fire together for the same article; the 5-minute stagger is between articles, never between platforms.** Three articles at 14:00 = six posts: both channels at 14:00, 14:05, 14:10. No waiting for a cron, no scheduling hours ahead.

## The channels

- **X:** entries in `data/social-queue.json`, posted by `post-social.mjs` on every queue change and every 15 minutes. Posts render as a **link card** (the hero with headline, whole card clickable) — never attach native media to an article link, with one exception: **reposting a URL X has already seen**, because X caches the card per URL for the day, so a repost must carry the picture as native media instead.
- **LinkedIn (company page):** always a hand-written entry in `data/buffer-queue.json`, posted through Buffer (a social scheduling service). The direct LinkedIn API path is documented but dormant — the repo holds no `LINKEDIN_*` secrets, so a nested `linkedin` queue target silently never fires *and* becomes a duplicate waiting to happen if credentials ever land. Buffer marks posts "posted" on acceptance, so a retry workflow re-checks LinkedIn's real status every 30 minutes and re-queues a confirmed failure exactly once.
- **TikTok (plus Reels/Shorts, same 1080x1920 file):** not connected to anything; upload the Remotion-rendered cut manually from `remotion/out/`, paste the description, pick the hook frame as cover ([[Video Production (YFarmX)]]). The bed under the voice is tonal and quiet by rule — no noise-based effects, nothing sampled, so no music licence attaches to our files and a trending sound laid on in the app has room to sit over it, or the bed can be muted entirely.

## Copy rules (the banned-word gate refuses violations at queue time)

**Everywhere:** British English, no em dash, no "matter"/"hype"/"quietly"/"noise", no "plainly"/"in plain English" family, no AI-sounding balanced framings ("bigger than it sounds, thinner than it seems"). Facts of the story, then stop; if a line could be deleted without losing information, delete it.

**X:** body ≤255 characters (a URL always counts as 23); hook, blank line, substance; the article link goes in the entry's `url` field and the poster puts it on its own line. **Never a bare domain in the body text** — X builds the preview card from the first domain it sees (a post once shipped carrying Crypto.com's card instead of ours). Name the parent company or write around it.

**LinkedIn:** one opening line that carries the story, then three to five bullets, one fact each, at most one signpost emoji per bullet, one short close with the link. Every sentence carries a number, a name, a quote, a date, or a conclusion drawn from one. Aim for ~500 characters, not 2,000.

**TikTok:** no clickable caption links exist, so the caption ends with the address in words ("Full story at yfarmx.com, link in bio") — the one sanctioned exception to the bare-domain rule. Only the first line shows before "more", so it must work as the whole post. Three to eight accurate hashtags beat twenty.

## The race the tooling absorbs

Both posters refuse to publish until the article URL and card image are actually fetchable (right behaviour, bit three times on timing). Discipline first: push the article, poll until page and hero return 200, then push the queues. The posters now also wait up to 10 minutes on their own, and a 15-minute cron is the backstop. A hold caused by our own mistake (missing file, malformed URL) fails immediately instead.

## Running the queues

| File | Job |
|---|---|
| `data/social-queue.json` | the X queue. Each entry can also carry a nested LinkedIn block — that is the dormant direct-API path, not Buffer. |
| `data/buffer-queue.json` | the LinkedIn queue, posted through Buffer |
| `scripts/queue-post.mjs` | adds a scheduled entry from the command line, and refuses copy that trips the banned-words list |
| `scripts/post-social.mjs` | posts every due entry to each platform still `pending`, then marks it `posted`. Zero dependencies; X authenticates with OAuth 1.0a. |
| `scripts/social-due.mjs` | a cheap gate answering "is anything due" using the standard library alone, so the workflow can run often without paying to install dependencies each time. Answers "yes" on any error. |
| `.github/workflows/social.yml` | runs the poster on every queue change and every 15 minutes, retries once after 3 minutes if a post was still waiting on its deploy, and commits the results back |

Schedule an X post, then commit the queue file:

```
node scripts/queue-post.mjs --text "X copy" \
  --url "https://yfarmx.com/slug/" --at "2026-07-21T09:00:00Z"
```

Omit `--at` to go out on the next run. **Times are UTC** — British Summer Time is UTC+1, so 09:00 UK is `08:00:00Z`. The script's `--linkedin`, `--also-linkedin` and `--no-x` flags write into the **nested LinkedIn block, which is the dormant path** and will never fire; a LinkedIn post is a hand-written entry in the Buffer queue.

**Statuses, per platform:** `pending` (waiting) · `posted` (done, carries an id) · `failed` (gave up after three tries, reason in `error`). Top-level `hold` freezes a whole entry, `skip` means "not to X" (LinkedIn only), and `example` is an ignored sample.

**The waiting knobs:** the X poster waits for the article URL to serve an `og:image`, the Buffer poster waits for the card image to serve the exact bytes of the committed file. Both default to 40 tries at 15 seconds — the ten minutes described above — and both return on the first successful probe, so an already-live asset costs one request.

## Switching a channel on

A platform stays dormant until its secrets exist; missing credentials leave that platform's posts `pending` while the run still exits cleanly, so one channel never blocks another. Secret **names** only below — the values live in GitHub → Settings → Secrets and variables → Actions.

- **X:** at developer.x.com set the app's permissions to **Read and Write**, *then* **regenerate** the access token and secret. Regenerating before the permission change is why posting 403s. Add `X_API_KEY`, `X_API_SECRET`, `X_ACCESS_TOKEN`, `X_ACCESS_SECRET`. The poster trims whitespace on these, so a stray newline from a mobile paste will not break authentication.
- **Buffer (which is how LinkedIn posts):** `BUFFER_API` is a Buffer *personal* API key, from Settings → API → Personal Keys, and it must have both **`accountRead`** and **`postsWrite`** ticked — a missing `accountRead` gives `FORBIDDEN`, a stale or rotated key gives `401 UNAUTHENTICATED`. The free plan allows **one key at a time**, so delete the old one before making a new one. Pin the YFarmX.com page's Buffer channel id on every queue entry (the value is already in `data/buffer-queue.json`); posting then needs only `postsWrite` and never touches the channels query. `DRY=1 node scripts/buffer-post.mjs` lists the channels without posting. Buffer is a LinkedIn-approved partner, so no LinkedIn developer review is needed, and it works on the free plan.
- **The LinkedIn image card is not automatic.** Give the entry a `link` object (url, title, description, image) and the poster attaches it as a Buffer link asset, so LinkedIn renders the rich preview with the hero. Text alone posts as a plain update with **no image** — and when there is a `link`, keep the raw URL out of the text, because the card already carries it.

Everywhere else a YFarmX article could go — the Telegram, Discord, Bluesky and Mastodon mirrors already built into the poster, the feed-syndication applications, and the deliberate decision never to automate Reddit — is [[Social Platforms (YFarmX)]].

## Links

[[YFarmX]] · [[Map - Processes]] · [[Article Pipeline (YFarmX)]] · [[Social Platforms (YFarmX)]] · [[Video Production (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]]
