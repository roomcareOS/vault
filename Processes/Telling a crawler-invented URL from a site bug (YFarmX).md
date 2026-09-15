---
tags: [process, yfarmx, seo, aieo]
source: The `<page>/null` investigation, 15 September 2026
updated: 2026-09-15
---

# Telling a crawler-invented URL from a site bug (YFarmX)

**Why this exists.** On 15 September 2026 Cloudflare showed hundreds of requests
to paths shaped `<real page>/null`: `/news/null`, `/contact/null`,
`/ai/models/compare/null`, article slugs, sub-hub pages. That shape looks
exactly like a template interpolating a null variable into an `href` or `src`,
and the brief read as a site bug to hunt. It was not ours. **A crawler took URLs
it had already fetched and appended a segment of its own.** Hours go into
hunting a phantom unless the checks below are run first, and they take minutes.

## The control test: does the string exist in anything we serve?

The parallel to [[Telling link rot from a block (YFarmX)]] is exact. One odd
observation proves nothing; the control test separates the two explanations.

Run all five. Any single pass is weak, and together they are conclusive.

1. **Served HTML.** `curl` the worst-hit page live and grep for the token. Our
   `/news/` and `/contact/` contained zero occurrences of `null`, in the build
   and from the edge.
2. **Rendered DOM.** Load the page in headless Chromium and inspect every
   element for `href`, `src`, `srcset`, `imagesrcset`, `action`, `poster`,
   `data-src`, `content` and `formaction` holding the token. Twenty page types
   were checked and all were clean. This catches anything JavaScript injects,
   which a `curl` never sees.
3. **Network during load.** Log every request the page makes and match the
   suspect path. None appeared.
4. **The scripts that page actually loads.** Pull each `<script src>` from the
   live HTML and grep for URL assignment: `.href =`, `.src =`,
   `setAttribute('href'`. Both worst-hit pages had **zero** of them, so neither
   page has code that could build a URL at runtime. This is the quickest check
   and it was the decisive one.
5. **Every URL-building site in the codebase.** `grep` for those three patterns
   across `src/` and `public/js/`, then read each hit. Ours were all guarded:
   `safePath()` in `palette.js` requires a leading `/`, `SubHubIndex` and
   `crypto/systems` fall back to `''`, the article audio player reads
   `?? ''`, and the compare page's `el.href` is an object URL for a download.

**Astro omits an attribute whose value is null** rather than printing the
string, so the static output cannot carry one. Only client JavaScript can, which
is why check 4 settles it so fast.

## The positive evidence that a crawler built it

Three signs, and they are what to look for next time:

- **Every junk path is a real page plus the extra segment.** Eight out of eight
  sampled base paths served 200 while the same path with `/null` served 404. A
  crawler walking real URLs produces exactly that; a template bug does not care
  whether the base path exists.
- **The same agent fetches both.** meta-externalagent fetched `/news/` 143
  times and `/news/null` 63 times in one day.
- **The traffic has no reader shape.** 100% United States, every request a GET,
  flat at 26 to 93 an hour across all 24 hours with no daily curve. Real
  readers of a UK site do not look like that.

## A status code that follows the client, not the path

The 504s in the same data looked like our origin hanging. They were one broken
client: `nginx-ssl early hints` made **9,504 requests in 24 hours and every one
returned 504**, which is 11% of zone traffic and not a single success.

The test that settled it, and the reusable lesson: **check whether that client
also fails on a page that works for everyone else.** It returned 504 on
`/offline/`, which serves 200 to every other client. So the status belonged to
the client. Confirming from our side, `/news/null` returns a 404 in 0.3 to 1.0
seconds over HTTP/1.0, HTTP/1.1 and HTTP/2, the same as any other 404, and
every 504 in the window carried request protocol `UNK` while no HTTP/2 request
timed out.

A 504 concentrated in one user agent is a client to block, never an origin to
tune.

## Querying the zone (free plan, and what it refuses)

Zone tag `a3ffe5f239e47a718e0d0320b06a03ec`, `httpRequestsAdaptiveGroups`,
token from the `CLOUDFLARE_API_TOKEN` environment variable and never written
down. Group by `{clientRequestPath edgeResponseStatus userAgent}` and filter
with `clientRequestPath_like: "%null%"`.

- **Available:** `clientRequestPath`, `edgeResponseStatus`, `userAgent`,
  `clientCountryName`, `clientRequestHTTPProtocol`,
  `clientRequestHTTPMethodName`, `datetimeHour`.
- **Refused on this plan:** `clientAsn`, `clientRefererHost`, `refererHost`.
  The referer would have been the fastest answer of all, so plan around its
  absence rather than discovering it mid-investigation.

`clientRequestHTTPProtocol` earned its place here: `UNK` is the marker of a
client that is not speaking ordinary HTTP, and it separated the 504 cluster
from everything else in one query.

## What to do about it

Nothing in the code, because there is nothing to fix. The requests stop when a
rule blocks the client. Two were drafted for Jay on 15 September and neither is
applied without his say: meta-externalagent restricted to the prefixes
`robots.txt` already disallows, and the non-completing early-hints client
blocked outright. See the repo record for the exact expressions.

Where a crawler ignores `robots.txt`, the WAF rule should enforce **exactly the
published `Disallow` list and nothing more**, so it carries out the policy that
already exists instead of inventing a second one. Keep `/logo-cube.png`,
`/js/palette.js`, `/social-card*` and the favicons reachable: blocking them
breaks the page render bots see and breaks link previews. Link previews come
from `facebookexternalhit`, a different agent from `meta-externalagent`, so a
rule naming the crawler leaves them alone.

## See also

- [[Telling link rot from a block (YFarmX)]]
- [[Edge Security Lives at the Cloudflare Zone (YFarmX)]]
- Repo record: `docs/crawler-traffic-2026-09-15.md` in `roomcareOS/yfarmx`,
  carrying the measured numbers and both rule expressions.
