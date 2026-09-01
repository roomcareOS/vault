---
tags: [process, yfarmx, security]
source: Cloudflare hardening pass, 1 September 2026, plus the live verification that followed it
updated: 2026-09-01
---

# Edge Security Lives at the Cloudflare Zone (YFarmX)

[[Pipeline Security Rules (YFarmX)]] covers the machinery that publishes the site. This note covers the layer in front of it, because on 1 September 2026 half of [[YFarmX]]'s security moved to the Cloudflare zone, where **nothing in the repository can see it**. A session reading only the repo will now draw wrong conclusions about what the site enforces, and can break the live site while believing it is hardening it.

## What now lives at the zone rather than in the repo

Added 1 September 2026, all on the `yfarmx.com` zone: minimum TLS 1.2, edge HSTS, Certificate Transparency alerts, the Cloudflare Free Managed WAF ruleset, a rate limit of ten `POST /api/*` per ten seconds per IP, DNSSEC signing, DMARC at `p=quarantine`, and **a Content-Security-Policy applied to every HTML page by a Response Header Transform Rule**.

None of that is in `public/_headers`. None of it appears in a build. It is edited at Cloudflare → yfarmx.com → Rules, SSL/TLS, Security and DNS. A Pages-scoped deploy token cannot read or change any of it, so a session cannot check it with the token it deploys with; it has to fetch the live headers instead.

## The rule that matters most: two CSP headers intersect, they do not merge

If a page is served with two `Content-Security-Policy` headers, a resource must satisfy **both**. The effective policy is the intersection, so adding a second policy can only ever make things stricter. **You cannot loosen a CSP by adding another one.**

This is live and dangerous for YFarmX right now:

- the zone CSP allows `img-src` from `media.yfarmx.com` and three legacy WordPress image hosts
- the repo CSP in `public/_headers` allows `img-src 'self' data:` and nothing else

The repo one is not currently serving, because it is around 5,700 characters (92 script hashes) and **Cloudflare Pages silently drops a response header over roughly 2,000 characters**. So the danger is latent. The moment somebody "fixes" the repo CSP so it fits and starts serving, the intersection blocks every R2 image and all audio, site-wide, with no error anywhere except the browser console.

Pick one owner for the CSP and delete the other. Never run both.

## The zone CSP must enumerate every origin the browser calls, and the repo already knew them

The 1 September rule shipped with `connect-src 'self' https://cloudflareinsights.com https://media.yfarmx.com` and nothing else. The site calls seven further origins directly from the browser, so all seven were blocked the moment it was enforced. Broken: `/verify/tectonic/` (whose entire purpose is reading balances from public nodes in the reader's own browser), `/tools/gas-fee-checker/`, `/arcade/who-wants-to-be-a-whole-bitcoiner/`, and the Web3Forms email notification on every form.

Two things caused it, and both are avoidable:

**The verification tested representative pages, not the ones that make cross-origin calls.** The homepage, search, an article, the Space section, the tracker and the newsletter form all passed with zero violations, because every one of them is same-origin or hits `/api/*`. The pages that do talk to other origins are the tools, the arcade and the verify page, and none was opened.

**The origin list already existed.** `public/_headers` carries a `connect-src` maintained precisely for this, and copying it across would have been correct on the first attempt.

### How to find the real list

Grep the **built output**, not the source. Source greps produce false positives (a coin id of `binancecoin` matches a grep for `binance`) and miss anything a bundler rewrites:

```
grep -rhoE 'fetch\("https://[^"]+"' dist --include="*.js" --include="*.html"
```

Then confirm in a browser. If the session's network cannot reach the live host, serve `dist` locally and inject the live policy as a header on HTML responses; that reproduces the exact page against the exact policy. Several of these fetches only fire on a click, so the page must be driven, not merely loaded.

## `yfarmx.pages.dev` bypasses every one of these protections

The zone rules attach to `yfarmx.com`. `yfarmx.pages.dev` is a different hostname in a different zone, and on 1 September it was serving a complete, current copy of the site with no CSP, no WAF and no rate limit, plus a duplicate of all 1,546 pages for search engines.

`functions/_middleware.js` now 301s the bare production host to `yfarmx.com`. It deliberately leaves `staging.yfarmx.pages.dev` and the `<hash>.yfarmx.pages.dev` previews alone, because the staging review flow depends on them and the R2 CORS policy names staging explicitly.

**Any new protection added at the zone needs the same question asked: does pages.dev get it too?** The answer is always no.

## How to check what the site actually enforces

Never grep `_headers` or the build and conclude anything. Dump the live headers:

```
curl -sI https://yfarmx.com/ | grep -i content-security-policy
```

Grepping the built file for the `__SCRIPT_HASHES__` placeholder is a **false pass**: it returns nothing whether substitution worked or the whole header was dropped. Ask the live site.

## Related

- [[Pipeline Security Rules (YFarmX)]] — the workflow and repository layer, where the 8 August pass found the real risk
- [[Staging and Backups (YFarmX)]] — why the staging and preview hosts must keep working
- [[YFarmX]]
- [[Map - Processes]]
