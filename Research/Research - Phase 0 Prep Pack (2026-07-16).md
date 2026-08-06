---
tags: [research, yfarmx]
source: yfarmx/docs/research/2026-07-16-phase0-prep-pack.md
updated: 2026-08-06
---

# Research - Phase 0 Prep Pack (2026-07-16)

**What was researched:** every question in the [[YFarmX]] mission brief that could be answered before touching the website or needing credentials — seven topics (social posting, audio, budget, GoDaddy hosting, crypto affiliate law, AI disclosure, rebuild approach). Fourteen agents ran; every price and claim was re-checked by an independent fact-checking pass instructed to disprove the figures.

## Key findings

- **Money is not the constraint.** The whole newsroom (writing, editing, images, audio, social) costs roughly **£50–75/month** at 5 articles a day against a £1,400 budget — 20 to 25 times headroom. Realistic cost per article: £0.15–0.25.
- **Audio ~16p per article** via Gemini TTS on OpenRouter (~£24/month), with a near-free fallback (Kokoro 82M, ~£0.50/month, six genuine British voices) wired in from day one.
- **Social posting via Buffer, £0–9/month.** X killed its free API (Feb 2026: $0.20 per post containing a link); LinkedIn company-page API needs months of approval. Buffer absorbs both problems. Avoid Zapier for X (you pay X's fees on top).
- **The one legal red flag:** affiliate links to crypto exchanges from a UK site sit inside FCA financial-promotion rules — breaching them is a **criminal offence** (up to two years). Launch with AI/quantum affiliates only; no crypto money links until a solicitor signs off in writing. Writing about crypto factually stays fine.
- **EU AI Act Article 50 applies from 2 August 2026.** A genuine, documented human review of each piece before publishing keeps the site inside the editorial exemption — the review must be real and recorded, a rubber stamp does not count.
- **Two pages the site must publish:** a "How we use AI" page and a corrections page (dated notes, never silent edits). Specific per-article disclosure notes maintain reader trust; bare "made with AI" labels actively reduce it (~42% of readers trust a piece less). The disclosure gets injected automatically by the pipeline so it can never be forgotten.
- **Proposed £15/day hard cap** on the automated key, enforced at OpenRouter account level plus a software check — this became the standing rail in the [[Hermes Newsroom Pipeline (YFarmX)]].

## What Jay should remember

- The GoDaddy and WordPress-rebuild sections were **overtaken one day later** by the pivot to the static git-based site ([[Decisions - YFarmX]]). The durable parts — budget maths, the FCA red flag, the EU AI Act reading, the disclosure and corrections policy, the daily cap — all carried forward into the [[Article Pipeline (YFarmX)]].
- Price durability: Sonnet's intro price ends 31 August 2026; the cheap scanning models retire ~October 2026. Model names live in config, never hard-coded — that rule started here.
- Real inference spend is 2–5% of the £1,400. The open question the pack raises: is the rest savings, or does it fund marketing, legal, better tooling, or bigger ambition?

[[Map - Research]] · [[Map - Businesses]]
