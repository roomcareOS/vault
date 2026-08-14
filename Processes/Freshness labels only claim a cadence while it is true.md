---
tags: [process, yfarmx]
source: Jay's screenshot of the AI hub ticker, 14 August 2026
updated: 2026-08-14
---

# Freshness labels only claim a cadence while it is true

**The claim: a static page must never print a refresh promise ("refreshed every six hours", "LIVE", "updates every minute") as unconditional text. The promise is a claim about infrastructure, and it fails silently exactly when the infrastructure does.**

## What happened

From 11 to 14 August 2026 the GitHub Actions billing outage stopped both the six-hourly market refresh and the hourly rebuild. The AI hub spent three days reading "NASDAQ · 11 AUG 2026, 19:15 UTC · REFRESHED EVERY SIX HOURS" over three-day-old prices. Jay caught it from a screenshot. The homepage coin strip had the same flaw (a baked "LIVE" tag over a snapshot from 6 August) and so did the Crypto Systems explorer ("Live prices from CoinGecko, refreshed every few minutes" as static text).

## The rule, in three layers

1. **A cadence claim is printed conditionally at build time.** If the data's own stamp is older than one cadence (plus the rebuild interval), the label degrades to the stamp plus "refresh delayed". The stamp always shows; only the promise is withdrawn.
2. **The same check re-runs in the reader's browser.** During a full CI outage the *built page itself* is old, so a build-time check alone still lies. A few lines of client JS re-read the baked stamp against the clock and withdraw the claim.
3. **"Live" is only said once live figures are actually on screen.** The baked state names its snapshot and date; the live-feed script rewrites the label when (and only when) real figures land. In yfarmx this is the `[data-stock-stamp]` / `[data-coin-stamp]` contract in `src/scripts/live-prices.ts`.

## How to apply it elsewhere

Any surface in any estate property that shows fetched data with a freshness caption: caption = *source · data's own timestamp*, plus the cadence only under the conditions above. Grep candidates: "refreshed every", "updates every", "LIVE", "real-time".

## Related
- [[Map - Processes]]
- The repo record: `roomcareOS/yfarmx` `docs/status.md`, 14 August 2026 entry; the freshness registry is `docs/time-sensitive.md`.
