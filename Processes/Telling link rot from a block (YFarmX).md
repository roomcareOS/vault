---
tags: [process, yfarmx, sourcing]
source: The risk-tracker and events refresh, 3 September 2026
updated: 2026-09-03
---

# Telling link rot from a block (YFarmX)

**Why this exists.** House rule 3 says open every URL before citing it, and the
trackers count as copy. But a session sits behind an egress proxy and a lot of
sites refuse it. A non-200 from here is **not** evidence a link is dead, and
acting on one means "fixing" a citation that was fine, or dropping a real
source.

## The rule: always test a second URL on the same host

A single failing URL tells you nothing. Fetch something else on the same
origin, ideally its homepage plus a page you are certain exists.

- **zoom.com** — the tracker's post-quantum E2EE citation returned 404. The
  homepage, `/en/blog/` and `/en/trust/security/` all returned 200. Different
  behaviour on the same host, so the 404 is **real rot**.
- **defillama.com** — a protocol page returned 404 on one sweep and 403 on the
  next. Its own homepage and `defillama.com/protocol/aave` both returned 403
  too. Uniform refusal across the host, so it **blocks this session** and the
  earlier 404 proved nothing. Left untouched.

Same symptom, opposite conclusion, and only the control test separates them.

## Known-blocked, not rotten

Seen refusing a session while being perfectly alive for real readers:
`etherscan.io` and the other block explorers, `defillama.com`, `x.com` for
unauthenticated HTML, `openai.com`, `ft.com`, `bloomberg.com`, `medium.com`,
`arstechnica`, exchange sites (Upbit, Bybit, Binance), and Cloudflare-fronted
conference sites. A 403 from any of these is the proxy, not the publisher.

## Reaching the content anyway

- **DefiLlama**: `defillama.com` is 403 but **`api.llama.fi/hacks` is not**.
  The hacks database is the settled loss figure for a crypto incident and it is
  fetchable. This is the primary to reach for when a protocol has published no
  number of its own.
- **X / Twitter**: `cdn.syndication.twimg.com/tweet-result?id=<id>&token=a`
  returns the post as JSON, including the author handle and full text, with no
  login. Use it to CONFIRM a post, then cite the ordinary `x.com/<handle>/status/<id>`
  URL. It also distinguishes a deleted post: the response is a tombstone reading
  "This Post was deleted by the Post author", which is how a $9.3m figure was
  kept out of the exploit tracker.
- **Wayback / CDX**: blocked by egress policy here. Do not plan around it.
- **NVD**: cite `nvd.nist.gov/vuln/detail/CVE-...`, not
  `services.nvd.nist.gov/rest/json/...`. The API is for checking, the detail
  page is for a reader.

## When you cannot find a replacement

Do not swap a dead citation for a live page on the same site that does not
support the claim. That converts visible rot into an invisible fake citation,
which is worse. Leave it and say so.

## See also

- [[Verify before you correct published copy]]
- [[A Deploy Replaces the Whole Site (YFarmX)]]
