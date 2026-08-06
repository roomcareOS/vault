---
tags: [business, intervooh]
source: [README.md, docs/PRD.md, docs/DECISIONS.md, docs/BRAND-STICKERS.md]
updated: 2026-08-06
---

# Intervooh

**intervooh.com** — interview preparation for adults, UK-first. You tell it three things — **company, role, date** — and it builds a personalised, day-by-day preparation programme that fits the days you have left. Learn in Tuition mode, prove it in Mock mode (spoken practice interviews scored against a rubric), arrive ready. One line: *tell it the interview, get a programme; learn in tuition, prove it in mocks, arrive ready.*

Named Intervooh on 17 July 2026 (working title was "InterviewPrep"). Sibling product of [[MyHomework]] — same stack, same design language, same "coach, never crutch" philosophy, adapted for adults under interview pressure.

## The three refusals (product identity)

These are permanent, public positions — ethics and marketing wedge in one (see [[Marketing Claims Bank (Intervooh)]]):

1. **No fabrication** — it never invents experience for you; every story is built from what you actually did.
2. **No live whispering** — no real-time answer feeding during actual interviews. It prepares you; it doesn't impersonate you.
3. **No emotion-reading** — it scores behaviour you control (structure, specificity, relevance, evidence, delivery mechanics), never inferred feelings. This also keeps it clear of the EU AI Act's ban on emotion-inference in recruitment (see [[Research - Intervooh Evidence Base]]).

## Stack

React + Vite + TypeScript + Tailwind front end, built as an installable PWA (a web app you can add to your phone's home screen). The backend is Supabase (a hosted backend service) in the **London region so UK data stays in the UK**: accounts, a Postgres database with owner-only row-level security (each user can only ever read their own rows), and private CV storage. Edge functions (small server-side programs) do the sensitive work — the AI proxy to Google Gemini (the API key never touches the browser), Stripe billing, and account jobs. Hosted on Vercel. The deterministic programme engine is plain code, not AI — same inputs, same plan, every time. This is the proven [[Supabase Stack Pattern]] shared with [[MyHomework]].

A key architecture decision ([[Decisions - Intervooh]]): the app ships fully working in **local/device mode** — no account, no AI, everything in the browser — and accounts + AI light up only when environment variables are present.

## Business model

- **£16/month subscription** ("Intervooh Everything") — founder decision 18 July 2026, superseding the PRD's £19-pack / £12-month debate.
- **£29 one-off** "One Interview Pass".
- **Free tier**: setup, programme, questions-to-expect, and daily quick-fire practice — all served by the deterministic engine and [[Question Database (Intervooh)]], no AI cost.
- Plans are enforced **server-side** as AI spend budgets (free ≈ $0.25/day taste; pro ≈ $2.50/day), tunable without redeploying. Money flows Stripe Checkout → Revolut Business payout. Runbook: [[Billing Setup (Intervooh)]].
- Unit economics are comfortable: a full voice mock costs roughly 2–6p of AI; a heavily used 14-day programme ≈ £0.50–£1.00.

## Brand

Two-chairs logo (one navy chair, one mint — interviewer and candidate). Palette derives from it: mint `#4FB393`, ink navy `#1B2A44`, warm cream paper `#FAF9F5`. Calm, premium, adult register; British English; zero gamification gimmicks. Design language inherited from MyHomework v3. Spot-illustration method: [[Sticker Prompt Pack (Intervooh)]].

## Status snapshot (6 August 2026)

- **M0 built and deployed**: setup wizard (one question per screen), programme engine v1 (three shapes — standard 14+ days, short-runway 2–13 days, panic <48 hours), home timeline with block completion, the 200-job question database, data controls (export JSON / delete everything). Fully unit-tested; typecheck, lint and tests are release blockers.
- **M1 decisions taken**: Supabase London for accounts, Gemini via a single edge function, voice input at launch (browser speech recognition, typing always available), AI-scored voice mocks at launch.
- **Billing**: code fully wired; goes live the moment the Stripe secrets exist. Until then upgrade buttons say "Billing is not configured yet" and the app stays free.
- **Open founder decisions**: first audience wedge, technical drills depth, company packs, free tier size, report sharing, whether to admit 16–18 school leavers.
- Verification of changes is done end-to-end in a real browser via [[Skill - Verify (Intervooh)]].

## Related

- [[Map - Businesses]] · [[Home]]
- [[Decisions - Intervooh]] · [[Research - Intervooh Evidence Base]]
- [[Accounts and AI Switch-On (Intervooh)]] · [[Billing Setup (Intervooh)]]
- [[Marketing Claims Bank (Intervooh)]] · [[Question Database (Intervooh)]]
- Estate-wide: [[Todoist Doctrine]] · [[RLS and Schema Change Process]]
