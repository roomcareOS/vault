---
tags: [map, cross]
updated: 2026-08-08
---

# Map - Processes

Every repeatable process in the estate, grouped by business. Cross-estate patterns first — they carry the most weight. Start at [[Home]].

## Cross-estate

- [[Supabase Stack Pattern]] — the one backend recipe ([[RoomCare]], [[MyHomework]], [[Intervooh]]): London region, row-level security as the security model, edge functions for secrets.
- [[RLS and Schema Change Process]] — the two verbatim rules for touching any database; policies and a failing-read test ship with every migration.
- [[Working Across Devices (Claude Sessions)]] — cloud sessions by default, Remote Control when working locally; why chats never sync between devices and why that loses nothing.

## YFarmX

- [[Article Pipeline (YFarmX)]] — the five-step human-in-the-loop publishing loop; draft delivered in chat, never improvised.
- [[Social Syndication (YFarmX)]] — X and LinkedIn fire together per article from queue files; TikTok is manual. Running the queues and switching a channel on live here too.
- [[Social Platforms (YFarmX)]] — the wider map: dormant broadcast mirrors, feed syndication, and the standing decision never to automate Reddit.
- [[Video Production (YFarmX)]] — the Remotion cut pipeline: output spec and safe area, which renderer is which, the standing imagery and copy rules, the light theme, and the render traps not to "fix".
- [[Audio and Voice Production (YFarmX)]] — the house voice and its fixed settings, `make-audio.py`, the daily quota, the fallback, and the rule that the voice never changes without Jay.
- [[Podcast - YFarmX Briefings]] — live since 6 Aug: the engraved five-character cast takes one article apart per episode, measured into a self-hosted feed; the show page is progressive enhancement over native audio. Directory submissions still pending, in a fixed order.
- [[Image Style and Prompt Libraries (YFarmX)]] — the cyber-dossier house look and the index of self-contained prompt packs.
- [[Staging and Backups (YFarmX)]] — staging branch Jay reviews before anything reaches live, plus daily backups.
- [[Ops Runbooks (YFarmX)]] — SEO go-live, email anti-spoofing DNS, page-freshness registry: the account-level clicks only Jay can do.
- [[Pipeline Security Rules (YFarmX)]] — where the real risk sits: secrets on steps not jobs, `permissions:` on every workflow, a lost `git push` that quietly double-posts, why `_headers` never reaches a Function, and localStorage as an input.
- [[Robotics Launch Checklist (YFarmX)]] — what launching a site vertical actually needs, learned by shipping one.
- [[Space Hub Build (YFarmX)]] — the flag-gated Space section, and the disciplined agent-project method worth copying.
- [[App and Store Distribution (YFarmX)]] — the site is the app: the PWA layer, the Google Play route (organisation account, TWA, keep the signing key), Apple parked for phase 2.

## Intervooh

- [[Billing Setup (Intervooh)]] — switch on Stripe; the app stays free and working until the secrets exist.
- [[Accounts and AI Switch-On (Intervooh)]] — Supabase accounts plus the Gemini AI layer, about 20 minutes, no secrets in the browser.
- [[Question Database (Intervooh)]] — 200 job titles across 24 sectors; powers the free tier with no AI call at all.
- [[Marketing Claims Bank (Intervooh)]] — every public claim traces to an evidence row or is pure product description.
- [[Sticker Prompt Pack (Intervooh)]] — 20 brand spot illustrations from one pasted base style.

## MyHomework

- [[Launch Plan (MyHomework)]] — family beta to paid product, one gate at a time.
- [[Children's Data and Safety Checklist (MyHomework)]] — the ICO Children's Code turned into a build checklist.

## RoomCare

- [[UK Compliance Position (RoomCare)]] — the regulatory answer for managers, procurement and investors: GDPR, Cyber Essentials, the non-clinical position, the honest gap list and the door paragraph.
- [[Security Architecture (RoomCare)]] — the principles and threat model that earn those claims; policy lives in the database, the record is append-only, the nurse call is never touched.
- [[Copy Rules (RoomCare)]] — the word-level law for every UI string, enforced by the build itself.
- [[Deploy and Backend Runbooks (RoomCare)]] — putting RoomCare online and wiring its backend, in redoable steps.

## Scenecast

- [[Project Practices (Scenecast)]] — release gate, contribution rules and security posture for a public codebase.

## Starting a new process note

- [[Template - Process Note]] — the shape a new process note takes.
