---
tags: [process, intervooh]
source: docs/SETUP-AI-ACCOUNTS.md
updated: 2026-08-06
---

# Accounts and AI Switch-On (Intervooh)

Ops runbook: turn on the two things in [[Intervooh]] that need external services. The app ships fully working in local/device mode with no accounts and no AI ([[Decisions - Intervooh]]); this switches on:

- **User accounts** (sign in, cloud-synced data, CV storage) → Supabase
- **The AI layer** (tuition coach + scored mocks) → a Google Gemini API key, used only server-side by a Supabase edge function (a small server-side program)

Two free-to-start accounts needed: **Supabase** and **Google AI Studio**. About 20 minutes. Nothing here puts a secret in the browser.

## Part A — Supabase (accounts + database)

- [ ] Create a project at supabase.com, **London region (eu-west-2)** — keeps UK data in the UK.
- [ ] SQL Editor → paste and run `supabase/migrations/0001_init.sql` (creates tables, owner-only row-level security, CV storage bucket, daily-usage counter).
- [ ] Copy the **Project URL** and the **publishable key** — both safe to expose in the browser; row-level security is what protects the data. Never use the secret/service-role key in the front end.
- [ ] In Vercel → Environment Variables, add `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`; redeploy. Accounts now work (auth emails run on Supabase's shared sender; add own SMTP later for branded emails).

## Part B — Google Gemini (the AI brain)

- [ ] aistudio.google.com → create an API key. Pay-as-you-go (pennies per coaching session); enabling billing raises rate limits. The key goes into Supabase, **never** into the browser app or Vercel env.

## Part C — deploy the AI edge function

One file: `supabase/functions/ai/index.ts`. It needs exactly one secret, then deploying. It authenticates each caller with their own login token, so no service-role key is required.

- [ ] Set the secret in the Supabase dashboard: key `GEMINI_API_KEY` (the function also accepts `GEMINI_INTERVOOH_API`, `GOOGLE_API_KEY`, or any secret whose value is a Google AI Studio key).
- [ ] Deploy, one of three ways:
  1. **GitHub Action (recommended, no terminal)** — the repo ships `.github/workflows/deploy-supabase-functions.yml`; add repo secrets `SUPABASE_ACCESS_TOKEN` and `SUPABASE_PROJECT_ID`, then run the "Deploy AI function" workflow. Redeploys automatically on future changes.
  2. **Supabase dashboard editor** — Edge Functions → deploy via editor, name it `ai`, paste the file.
  3. **CLI** — `npx supabase login` → `npx supabase link --project-ref <ref>` → `npx supabase functions deploy ai`. (Use `npx`, not a global install.)

The React app calls this function with the signed-in user's token; the function checks the daily cap, calls Gemini with the key kept server-side, runs the fabrication and emotion-inference guards, and returns coaching/scoring.

## What each key can and can't do

| Value | Where it lives | Risk if leaked |
|---|---|---|
| Supabase URL / publishable key | Browser (public by design) | None — row-level security blocks access to others' rows |
| `GEMINI_API_KEY` | Supabase secret (server only) | Someone could spend the Gemini credit — never in Vercel env or front end |

## Tuning (optional Supabase secrets)

- `AI_MODEL` — default `gemini-3.6-flash`; `gemini-flash-latest` always tracks Google's current release (avoids model-retirement errors). Overloads auto-retry and fall back across models.
- `AI_DAILY_CAP` — max AI calls per user per day (default 60).

## Verify it works

1. Sign up in the app, confirm email, sign in.
2. Open an interview → AI story coach → it greets you with a question.
3. Run a scored mock → answer 3 questions → scored report appears.

Diagnostics: "sign in" shown = signed out; "not switched on" = Vercel env vars missing; "AI is not configured" = the Gemini key secret is missing or unreadable.

## Related

- [[Map - Processes]] · [[Intervooh]] · [[Supabase Stack Pattern]]
- Next runbook in sequence: [[Billing Setup (Intervooh)]]
- End-to-end checking after changes: [[Skill - Verify (Intervooh)]]
