---
tags: [business, myhomework]
source: myhomework/README.md, myhomework/PRD.md, myhomework/PROGRESS.md
updated: 2026-08-06
---

**MyHomework** (myhomework.app) — a homework app for students aged 13+, built by Jay with his 13-year-old daughter as co-designer (she holds veto power over anything cringe). One sentence: **capture homework in seconds, plan automatically, and be coached through the work — never given the answers.**

## How it works

- **Capture**: photo, voice note or a few typed words — lock screen to "saved" in under 15 seconds, works offline. Voice notes are transcribed then the audio is deleted.
- **Plan**: a nudge after school (default 17:00) sorts the inbox — the AI guesses subject and size, asks only what it can't infer, sets a spaced work schedule and reminders.
- **Coach**: pressing Start opens a guided session where an AI coach walks the student through the task step by step but **never does the work for them** — the full method is in [[Teaching Approach (MyHomework)]].
- **Remember**: a deadline reminder ladder, spaced revision prompts, and a Sunday "week ahead" view.

What it refuses to be: an answer machine, a surveillance tool (a linked parent sees a read-only mirror the student knows about), or an attention trap (no leaderboards, no guilt streaks, no feeds — ever). The evidence behind every choice is in [[Research - MyHomework]]; the launch decisions in [[Decisions - MyHomework]].

## Stack

Vite + React + TypeScript + Tailwind as an installable PWA (progressive web app — a website that installs like an app), hosted free on Vercel at myhomework.app. Backend is Supabase in the **London region**: Postgres database, auth (pseudonymous nickname + password/passkey accounts, no email needed), private photo storage, and one Edge Function (`ai`) that holds the only AI key and applies the pedagogy rules server-side. **Owner-only Row Level Security** (database rules meaning a student can only ever read their own rows) on every table, proven live and re-runnable via `scripts/rls-smoke.sh`. A model adapter keeps the AI provider swappable — currently Gemini Flash-Lite on a free key; sessions cost 1–6p on budget vision models. Same shape as [[Supabase Stack Pattern]].

## Money

Beta free, founder-covered (£25/month AI cap). Public launch target ≤ £5/month, parent pays; whatever is free at launch stays free (the Quizlet lesson — never paywall the core loop after the fact). Path to revenue in [[Launch Plan (MyHomework)]]; children's-data obligations in [[Children's Data and Safety Checklist (MyHomework)]].

## Status snapshot (2026-08-06)

From PROGRESS.md, newest entries (18 July 2026):

- **Live at myhomework.app** — domain verified on Vercel, database live, coach live and passing adversarial "give me the answer" tests in production.
- All three capture modes work (photo with EXIF stripping, voice transcribe-and-delete, text); Sort flow, subject pages with "My materials" curriculum uploads, Home/Today/Save/Me structure, settings, 10 avatar characters.
- Public site now pre-rendered to real HTML (was invisible to crawlers); 13 crawlable pages; positioned as "MyHomework AI" pending the trademark check.
- Help ladder visible in-session ("I'm stuck" button — deliberately no "give me the answer" option); accessibility settings (easier reading, calm mode, read aloud); age/exam-board awareness; working delete-my-data.
- Guardrails verified live: off-topic and harmful-task refusals land exactly as scripted; "draw that as a picture" diagrams via Google's image model (~3p per image).
- Known limit: free Gemini key rate-caps at busy moments (honest "swamped, try again" handling shipped). Next: paid AI key, jailbreak regression suite, push reminders, streaks/goals.

## Links

- [[Map - Businesses]]
- [[Home]]
- Estate-wide: [[Todoist Doctrine]] · [[RLS and Schema Change Process]]
