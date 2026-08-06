---
tags: [process, yfarmx]
source: yfarmx/docs/social-platforms.md, yfarmx/docs/social.md
updated: 2026-08-06
---

# Social Platforms (YFarmX)

Where a [[YFarmX]] article can go beyond X, LinkedIn and TikTok: what is already built and waiting on a key, what is applied for, what is deliberately switched off, and what is a dead end. The posting mechanics live in [[Social Syndication (YFarmX)]]; this note is the territory.

**Provenance:** every claim was checked against official documentation on 25 July 2026 by a research pass with adversarial verification (twelve agents, official docs only). Platform rules move — re-verify before leaning on this after a few months.

## The broadcast mirrors — built in, dormant until keyed

The poster already carries four mirrors alongside X and LinkedIn. Each is dormant until its secret exists in GitHub → Settings → Secrets and variables → Actions. **Add the keys and the next due post fans out automatically; no code changes needed.** Mirrors fire only *after* X posts successfully, so the link-card quality gate holds everywhere, and each records its own status under `broadcast.<platform>` in the queue entry.

| Platform | Effort | What Jay does | Secret names |
|---|---|---|---|
| **Telegram** channel | 10 min, phone-friendly, zero risk | Create a public channel. Message @BotFather → `/newbot` → copy the token. Channel → Administrators → add the bot → enable **Post Messages**. | `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHANNEL` |
| **Discord** | 5 min, phone-friendly, zero risk | Channel → Edit Channel → Integrations → Webhooks → New Webhook. **The URL is the secret.** No bot account, no review. Posts arrive as a rich embed card. | `DISCORD_WEBHOOK_URL` |
| **Bluesky** | 5 min, phone-friendly, low risk | Settings → App Passwords → create one. **Never the main password.** The poster uploads the hero as the card thumbnail. | `BSKY_HANDLE`, `BSKY_APP_PASSWORD` |
| **Mastodon** | 15 min, one-time token dance, low risk | Pick an instance (a tech instance beats mastodon.social), mark the account as a bot out of courtesy, then Preferences → Development → New application → scope `write:statuses`. | `MASTODON_INSTANCE`, `MASTODON_TOKEN` |

All four are free and need no review, and all four build their own link previews. Bluesky's app passwords are officially "legacy" with OAuth as the successor, but no shutdown date exists and the documentation still blesses them for bots.

## Buffer's spare slots

Buffer's free plan allows **three channels**, plus an API key at 3,000 requests a month. **LinkedIn occupies one of them, so two are spare.** Buffer can carry Facebook Page, Instagram, Threads, TikTok, X, Pinterest, Google Business, Mastodon, YouTube and Bluesky — each connected by clicking in the Buffer app, after which the queue simply gains those services per post.

- **Recommended use of the two: Facebook Page, then Threads.** Beyond three channels it is about $5 per channel per month.
- **Threads** could also be done directly (free official API, no app review for your own account), but it is more setup than Buffer — only worth it if the slots run out.
- **Instagram: captions carry no clickable links.** Treat it as brand presence, not traffic. Skip for now.

*Correction against the source doc: `social-platforms.md` states that all three Buffer slots are free because LinkedIn uses the direct API. That was already out of date when written — LinkedIn went live through Buffer on 22 July 2026 and the direct API path is dormant. Two spare slots, not three.*

## Feed-based distribution (apply once, then automatic forever)

- **Flipboard** — free Publisher Account; a human reviews the RSS feed against their guidelines. Our feed was upgraded on 25 July 2026 to their current spec (300+ character excerpts, jpg enclosure at 500px or more, a creator field, 20+ items). Apply at **flipboard.com/publisher_signup** — **not** `/publishers`, which lands on a paid promotion pitch. Review takes days and approval is discretionary, but the site meets every stated criterion.
- **Google News** — **nothing to do.** Manual submission was abolished in April 2024; inclusion is automatic for policy-compliant sites and we already ship the news sitemap. The only lever is what we already do: real bylines, real sources, human review.
- **MSN Partner Hub (Microsoft Start)** — free with an ad revenue share and it ingests our RSS, but it is **invitation-only with no application queue**. Parked. If an invite ever arrives, the setup needs the Chromebook (mobile is unsupported) and a dedicated Microsoft account.
- **Apple News** — **closed to new publishers**; they are not accepting unsolicited applications. Parked, re-check quarterly. Anyone offering to "get you in" is running a scam.
- **SmartNews** — the application requires building their bespoke feed format first, and they warn they may never reply. Parked.
- **NewsBreak** — a US local-news product. Poor fit, skip.

## Reddit: deliberately NOT automated

Read this before asking. Reddit's API works and is free at our volume, but its spam policy names **our exact use case** as the canonical violation — "programming a bot that continuously promotes specific products or services within a community or across many communities". The Responsible Builder Policy (updated 25 July 2026) adds that apps must not post identical or substantially similar content across subreddits, that automated accounts must register for a public "App" label, and that API access now requires an approval ticket.

Enforcement reaches **accounts, bots, domains *or* subreddits** — meaning **yfarmx.com itself can be domain-banned sitewide**, killing every Reddit referral and manual share forever.

The compliant play is Jay sharing individual pieces by hand from his personal account, occasionally, with a sentence of context, while genuinely taking part in those subreddits. Five links a day from a bot is how the domain dies. **This is a policy decision, not a technical gap — do not "helpfully" build it in a future session.**

## Cross-posting whole articles (optional, parked)

Full-body syndication with a canonical link back, so the original still wins in search.

- **DEV.to** — free API key, canonical links supported and encouraged. The natural home for the AI and agent explainers, but it needs the full body; stub links get moderated. Build when wanted.
- **Tumblr** — free link posts, needs a one-time authorisation flow. Marginal audience, parked.
- **Hashnode** — the API went paid-only on 13 May 2026 and the endpoint is mid-migration. Parked.
- **Medium** — the publishing API is discontinued. No automated route.
- **Hacker News** — manual only by design. **Substack** is the newsletter conversation, not a syndication target.

## Dead ends, recorded so nobody rediscovers them

- **Facebook Groups: no API since 22 April 2024.** Post via the Page through Buffer, and share into groups by hand. Any tool claiming otherwise is driving a logged-in browser against Meta's terms.
- **Third-party RSS blasters (dlvr.it, Zapier, IFTTT):** they work, but a third party would then hold every account credential, their free tiers cap at roughly our daily volume with no headroom, and we already have a direct poster. Wrong architecture for us.

## Related

[[YFarmX]] · [[Map - Processes]] · [[Home]] · [[Social Syndication (YFarmX)]] · [[Article Pipeline (YFarmX)]] · [[Video Production (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]]
