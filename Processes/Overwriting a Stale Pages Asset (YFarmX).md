---
tags: [process, yfarmx, cloudflare, caching, diagnostics]
source: /ai/models/jev/ serving 200 from one region for hours after removal, 18 September 2026
updated: 2026-09-18
---

# Overwriting a Stale Pages Asset (YFarmX)

A page removed from the site kept answering **200 from one Cloudflare region** while every other region answered 404. Purge Everything did not clear it. Nothing in the zone was responsible. This note is how that was identified and what actually fixed it.

## The diagnostic, in two steps

**Step one: add a query string.**

```
curl -s -o /dev/null -w '%{http_code}\n' "https://yfarmx.com/<path>/"
curl -s -o /dev/null -w '%{http_code}\n' "https://yfarmx.com/<path>/?cb=$RANDOM"
```

200 then 404, from the same region, means **a cache key, not the origin.** A query string changes the key, misses, and reaches the origin. This is the same ten-second comparison that diagnosed the 1 September deletions ([[A Deploy Replaces the Whole Site (YFarmX)]]); it answers a second question too.

**Step two: ask the deployment directly.** A Pages deployment has its own hostname, which bypasses the zone completely:

```
https://<deployment-id>.yfarmx.pages.dev/<path>
```

List deployments and their ids with the Pages API (`/accounts/<id>/pages/projects/yfarmx/deployments`); the production one carries the `yfarmx.com` alias. All three deployments tested answered 404. **So no deployment held the page, and the zone was serving something none of them contained.**

## What it was not, and why that took a while

Jay ran **Purge Everything** and the object was back within about thirty seconds. A full dashboard sweep found nothing in the zone to blame:

| Checked | Found |
| --- | --- |
| Cache Reserve | Off, never activated |
| Tiered Cache | Off |
| Cache Rules | One rule, `http.host eq "yfarmx.com"`, 2 minute edge TTL |
| Response Header Transform Rules | One, disabled, unrelated |
| Snippets | Unavailable on the plan |
| Workers Routes | None |
| Page Rules | None used |
| WAF custom rules | Four, none touching the path |

The giveaway was in the response headers. The 200 carried:

```
x-robots-tag: noindex
cache-control: public, s-maxage=604800
```

**That pair is what Cloudflare Pages emits for a PREVIEW deployment**, and the repo sets neither for an article path. A second tell: the 404s came back as `cf-cache-status: HIT` with `cache-control: no-store`, which should never be cached at all.

## The conclusion

The copy sat in **Pages' own asset cache, not the zone cache.** A zone purge does not reach it, which is why Purge Everything changed nothing and why the dashboard had nothing to show. Two caches, two owners, one hostname.

## The fix: overwrite, do not purge

**You cannot delete from there. You can give Pages a real response for the path, and it replaces what it is holding.** One `_redirects` rule did it:

```
/ai/models/jev* /ai/models/typesafe/jev-1.13/ 301
```

Verified 20 of 20 from the region that had been serving the stale copy, and it sends a reader following an old link to the live record rather than an error. Jay picked the destination: *"i just want /ai/models/typesafe/jev-1.13/"*.

Two practical notes:

- **Mind the `_redirects` cap.** This deployment silently stops applying rules past roughly the hundredth, and `verify-seo` fails above 95. A splat covering the bare path, the trailing slash and anything beneath costs one rule rather than two.
- **A redirect is not a holdback.** Keep the flag as well, or the page can rebuild underneath the redirect. See [[Holding a Page Off Live (YFarmX)]].

## Related

- [[A Deploy Replaces the Whole Site (YFarmX)]] — the `?cb=` diagnostic, and the cache hiding a deletion
- [[Edge Security Lives at the Cloudflare Zone (YFarmX)]] — the settings the repo cannot see
- [[Holding a Page Off Live (YFarmX)]] — why the page was being removed in the first place
- [[YFarmX]]
- [[Map - Processes]]
