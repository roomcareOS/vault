---
tags: [process, yfarmx]
source: yfarmx/docs/seo-go-live-checklist.md, yfarmx/docs/email-dns-setup.md, yfarmx/docs/time-sensitive.md
updated: 2026-10-15
---

# Ops Runbooks (YFarmX)

Three thin but load-bearing [[YFarmX]] runbooks merged: the SEO go-live checklist, the email anti-spoofing DNS setup, and the page-freshness registry. These are mostly account-level clicks only Jay can do; Claude does the code side.

## 1. SEO and analytics go-live (Jay's hand-steps, in order)

**Preview stage (done as needed):**
- **Web3Forms key** — makes the contact form live: request a key against contact@yfarmx.com, paste it into the session; it is designed to be public.
- **Cloudflare Web Analytics token** (cookieless traffic stats): add the site in the dashboard, choose the *manual snippet* option (the site's security policy expects the exact official script), paste the token in.

**At DNS cutover:**
- **Remove the `X-Robots-Tag: noindex` line** in `public/_headers` — the deliberate lock keeping search engines out while WordPress was still live. One line, Claude does it, Jay says the word.
- **Google Search Console:** re-verify ownership by **DNS record** (the old Site Kit meta-tag verification dies with WordPress); submit `sitemap-index.xml`; request indexing; watch Pages/Coverage for a fortnight.
- **Bing Webmaster Tools:** "Import from Google Search Console" does it in one click. Bing feeds ChatGPT Search and Copilot, so this also feeds the AI assistants.

Everything else (titles, meta, canonicals, JSON-LD structured data, sitemap, AI-crawler-welcoming robots.txt, llms.txt, 301 redirects for every old WordPress URL family) is already handled in code.

## 2. Email anti-spoofing DNS (SPF / DKIM / DMARC)

Three DNS records that prove mail "from @yfarmx.com" is really us. DNS lives at Cloudflare since cutover; GoDaddy holds only the domain registration. Mailbox is Microsoft 365, provisioned through GoDaddy's reseller — which is why values read "secureserver" and that is correct, not a mistake.

- **SPF (who may send): DONE.** `v=spf1 include:secureserver.net -all` — looks wrong next to a Microsoft MX but the GoDaddy chain already ends at Microsoft. Do not "fix" it, and never add a second SPF record; a domain gets exactly one.
- **DKIM (cryptographic signature): OUTSTANDING.** The keys can only come from Microsoft (admin.microsoft.com → Defender → Email authentication → DKIM), whatever dashboard you start in; GoDaddy's own DKIM toggle belongs to a different mail product. Add the two CNAMEs in Cloudflare (grey cloud), wait, then flip Enable.
- **DMARC (what to do with fakes): DONE at monitor-only** (`p=none`, reports to contact@yfarmx.com). **Tighten to `p=quarantine` around 16 August 2026**, later `p=reject` — but read a report first and prefer finishing DKIM first.

Standing warnings: every mail/verification record is **DNS-only (grey cloud), never proxied**; do not delete `_acme-challenge` (it renews the site's HTTPS certificate, not a stale leftover); keep `msoid` and `email`.

## 3. The freshness registry (docs/time-sensitive.md)

**Time-sensitive = every editorial page that describes the present** — almost the whole site, excluding news articles (dated dispatches, corrected but never "updated to today") and ~10 static utility/legal pages. Three review tiers: **fast** (daily: live markets, "current flagship" model pages, incident logs, events), **medium** (weekly: landscapes, tool round-ups, hub state-of-play strips), **slow** (monthly spot-check: pure-concept explainers, glossary). Each page carries a visible `updated:` stamp; refresh the underlying JSON data files and the pages follow. The registry is the single source of truth for "refresh the time-sensitive pages" — a weekly pass runs against it (see [[Article Pipeline (YFarmX)]] on honest freshness).

## 4. Live systems: ask them, do not read them (8 Aug 2026)

Three weeks of form submissions were silently lost — plausibly reader mail,
corrections and news tips as well as waitlist signups — because a check on
18 July confirmed the contact form *rendered* (`data-armed="yes"`) and called
it live. Rendering is not delivering. The cause was the Cloudflare Pages
`GITHUB_TOKEN` expiring: it returns 401, which also stops `/api/publish`, so
the Publishing Desk and Hermes could not commit an article and nothing said
so.

**The rule: a write path must be able to answer "could you do your job right
now", and something must ask it.** `/api/subscribe` reports whether it is
armed and takes `dry:true` to exercise everything but the write, reporting
what the upstream actually answers. Build the same self-report into anything
that writes, and prefer a live probe over reading the code.

**Cloudflare platform traps, each of which hid a fault rather than causing
one:**

- **A Cache API key must be same-origin.** Cloudflare refuses a key on any
  other hostname and *throws*. An `*.internal` key inside `waitUntil` throws
  invisibly, which is how the price endpoints' last-known-good fallback was
  stated as a guarantee for weeks while never storing anything.
- **Never return 502 from a Function.** The edge treats it as a gateway
  failure and replaces the body with its own error page, so the reason never
  reaches the caller. Use 500 for application errors.
- **Miniflare accepts what production rejects.** `wrangler pages dev` cannot
  prove a Function works; only the deployed URL can.
- **Changing a Pages environment variable needs a redeploy** before Functions
  see it.

### Freshness release lessons, 15 October 2026

- Inventory all non-article references, but distinguish changed pages, verified unchanged claims and blocked source responses. A successful URL request is not a factual recheck. Keep historical benchmark and observation dates.
- Use the same dated market snapshot in reference copy, cards and state-of-play data. Separate release dates, disclosure dates and patch dates. Treat a retirement date qualified as “as early as” as an earliest possible date.
- Bounty feed totals must use public programmes only. Preserve the last successful source date when a platform fetch fails. A new incident or vulnerability row does not make the entire historical register freshly verified.
- New references also affect their parent hubs, latest-release lists, search, model feeds, public API guides and catalogue counts. Run cross-page consistency checks after composing the release.
- When disk is tight, immutable verified images/audio can be hard-linked into an isolated build, with final hash checks. A postprocessor must not overwrite a linked baseline asset. Keep the live snapshot until preservation and origin proof finish.
- Pagefind output can retain obsolete files between builds. Decode the active metadata dependency lists; preserve the complete previous live index and the new index, and remove only unreachable older index files. Count the whole deployment tree against the hosting limit before upload.
- The media release gate can take several minutes without intermediate output. Wait for its result; lack of output alone does not establish a stalled deployment. Preserve the deployment lock through the upload and full sharing-card proof.

These are operating lessons from `docs/research/reference-refresh-2026-10-15/` in the site repository. Earlier account-level SEO and email entries above retain their original historical status and need an account check before being treated as current.

### Homepage model selection, 15 October 2026

Jay wants the homepage to feature significant frontier models or standout releases, rather than automatically promoting every specialist model page. Use the explicit editorial selection in `src/lib/homepage-releases.ts` for both Latest and Release watch. Review the reader interest before adding a model; vendor identity alone is not a selection rule. The full model catalogue and Newswire keep their broader coverage.

The homepage's More news section contains six articles. Its duplicate exclusions must use the same filtered release pool as Latest. Verify all category filters and the default layout at desktop and phone widths after changing the selection. A model refresh does not by itself authorise changing this homepage policy.

## Links

[[YFarmX]] · [[Map - Processes]] · [[Article Pipeline (YFarmX)]] · [[Staging and Backups (YFarmX)]] · [[Decisions - YFarmX]]
