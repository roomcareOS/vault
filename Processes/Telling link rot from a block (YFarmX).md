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

## Two refinements, both learned the hard way the same afternoon

**A 403 is never rot.** It is a refusal to serve, not a statement that the page
is absent, so it says nothing about whether the URL is good. An automated check
that treats "403 here, 200 at the host root" as rot will flag arbitrum.io,
openai.com, upbit.com and arbiscan.io, all of which are fine for a real reader.
Only a **404 or 410** is a candidate, and only then with the control test.

**A blocking host answers inconsistently, so sample more than once.**
defillama.com returned, across three runs inside an hour: 403 on everything;
then 200 at the root with 404 on a protocol page; then 403 on everything again,
including its own homepage and `/protocol/aave`, which certainly exists. Its
404 was one of its refusal modes, not a missing page. If a host's control
answers move between runs, treat every result from it as unusable and change
nothing.

## Finding where a page moved to, without a search budget

The site's own sitemap is the cheapest route and needs no search calls:

    curl -s https://<host>/robots.txt | grep -i sitemap
    curl -s https://<host>/sitemap_index.xml          # then the relevant child
    curl -s https://<host>/sitemap-blog.xml | grep -oiE 'https://[^<]*<keyword>[^<]*'

That is how the dead Zoom citation was repaired rather than deleted: the
post-quantum E2EE post had moved from `/en/blog/post-quantum-e2ee/` to
`/en/blog/guide-to-post-quantum-end-to-end-encryption/`, and `sitemap-blog.xml`
gave the new URL in one request. Wayback is blocked by egress policy here, so
the sitemap is the tool to reach for. Check the recovered page really carries
the claim: this one is dated 24 May 2024 and names Kyber 768, which is the
pre-standardisation name for ML-KEM-768.

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
