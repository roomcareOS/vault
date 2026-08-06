---
tags: [process, yfarmx]
source: yfarmx/docs/seo-go-live-checklist.md, yfarmx/docs/email-dns-setup.md, yfarmx/docs/time-sensitive.md
updated: 2026-08-06
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

## Links

[[YFarmX]] · [[Map - Processes]] · [[Article Pipeline (YFarmX)]] · [[Staging and Backups (YFarmX)]] · [[Decisions - YFarmX]]
