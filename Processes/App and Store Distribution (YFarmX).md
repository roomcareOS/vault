---
tags: [process, yfarmx]
source: yfarmx/docs/app.md
updated: 2026-08-06
---

# App and Store Distribution (YFarmX)

**The site IS the app.** yfarmx.com is a full progressive web app (PWA — a website a phone can install like an app): app icon, full-screen window, offline reading, and the whole site inside it — every article, hub, tool, the glossary and the audio. There is no second codebase: publish an article and it is in the app the moment the site deploys. For a one-person newsroom that is the only architecture that stays healthy; a separate native app would rot the week attention moved elsewhere.

## The PWA layer (shipped 26 July 2026)

- Manifest, 192/512 icons (plus maskable variants from the cube logo), a branded offline page, and a service worker (`public/sw.js`) with three baked-in rules: **never serve a stale page as if it were current** (pages are network-first; a news app must not show old news as fresh), **fail open** (anything unrecognised — forms, `/api/`, `/desk`, audio streams, third parties — goes straight to the network), and **respect the phone's disk** (60 pages / 140 assets cached, oldest trimmed).
- `sw.js` and the manifest ship `no-cache`, so a deploy reaches installed apps on their next visit.
- **The build gates the app layer** (`scripts/verify-seo.mjs`): if the manifest, worker, offline page, icons or store screenshots go missing, the build fails — the app cannot be dropped by accident.
- Installing today, no store needed: Android Chrome ⋮ → *Add to Home screen*; iPhone Safari Share → *Add to Home Screen*. Same app the stores will ship.

## Google Play (first store, $25 once)

1. **Register the developer account as an Organisation, not personal.** Personal accounts must run a 12-tester, 14-day closed test before production; organisations are exempt. Needs a free **D-U-N-S number** for YFarmX Ltd — can take days to weeks, so start it first.
2. **Package with pwabuilder.com** (Trusted Web Activity, package ID `com.yfarmx.app`). The downloaded ZIP holds the signed `.aab` upload bundle and the signing key — **keep the whole ZIP safely**; that key signs every future update.
3. **Prove domain ownership**: its `assetlinks.json` goes in the repo at `public/.well-known/`. Without it the installed app shows a browser address bar; with it, full-screen.
4. **Listing**: assets ready in `store-assets/play/`; name "YFarmX: Frontier Tech News", News & Magazines, free. Data safety declares **no personal data collected** (cookieless Cloudflare counts, no accounts, no ads SDK) and **no ads** — both true, and the ads declaration must be revisited if AdSense ever arrives.
5. **Content updates never need a new release** — the app shows the live site. A new `.aab` only if the wrapper itself changes (icon, name, splash).

## Apple App Store (phase 2, honestly)

£/$99 **every year**, and Apple rejects thin website wrappers (guideline 4.2). The passing route is a small native shell (Capacitor) with genuinely native features — push breaking-news alerts, downloaded articles with background audio, native share — real work, worth doing once Android traffic justifies the fee. Building iOS needs a Mac the Chromebook is not; a GitHub Actions macOS runner plus fastlane solves that from the repo when phase 2 starts. Until then iPhone readers install from Safari and get the identical app, minus only the badge.

## Maintenance

- `public/sw.js` carries a `VERSION` — bump it when worker **logic** changes, never for content; old caches clean themselves up.
- App misbehaving after a deploy: hard-refresh once (the worker updates on next navigation), then DevTools → Application → Service workers.

## Links

[[YFarmX]] · [[Map - Processes]] · [[Ops Runbooks (YFarmX)]] · [[Decisions - YFarmX]] · [[Claude Operating Profile - YFarmX]]
