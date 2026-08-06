---
tags: [process, cross]
source: [RoomCare-PRD.md, docs/Security-and-Compliance.md, docs/Connect-The-Backend.md, supabase/functions/README.md]
updated: 2026-08-06
---

# Supabase Stack Pattern

Jay's one proven backend recipe, used across [[RoomCare]], [[MyHomework]] and [[Intervooh]]. Learn it once, and every product's backend reads the same way. Supabase is a hosted backend service: a Postgres database with accounts, realtime, file storage and small server-side programs built in.

## The pattern

1. **Supabase, London region (eu-west-2), always.** UK user data stays in the UK/EU. For RoomCare this is a hard rule (resident data never processed outside the UK/EU); for the apps it is the same instinct applied to students and job-seekers.
2. **Postgres with row-level security IS the security model.** The database itself decides which rows each actor may read or write; authorisation never lives only in app code, so a tampered or buggy client gains nothing. RoomCare scopes by home, resident consent and room; MyHomework and Intervooh scope owner-only (each user reads only their own rows). Every schema change follows [[RLS and Schema Change Process]]: policies plus a failing-read test ship with the migration, and append-only tables get INSERT-only policies.
3. **Edge functions for anything sensitive or server-only.** Small server-side programs hold the work a browser must never do: the AI model call (the API key never touches the client — Anthropic for RoomCare's orchestrator, Gemini for the apps), billing, invites, device pairing, rate limiting and CORS. Guarded database functions re-validate every write server-side.
4. **Realtime channels for live state.** RoomCare's request queue and room state stream over Supabase channels with optimistic UI and reconciliation; no refresh button anywhere.
5. **Secrets only in environment variables and function secrets.** Only the anon public key ships in an app bundle, and it grants nothing by itself; the service-role key is never used in application paths and never shared. Never commit a `.env`.
6. **Demo/local mode as the default.** With no keys configured the app runs fully on made-up or on-device data; real accounts and AI light up only when the environment variables exist (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, plus a mode flag). Missing configuration means demo data, never accidental live data — fail closed.
7. **Migrations as reviewed files in version control**, applied per environment; the infrastructure is reproducible from the repo. Hosting is GitHub → Vercel: push to main, live in about a minute, preview links on every branch.

## Why this stack (Jay's one-breath reasoning, from the RoomCare PRD)

One language everywhere, instant web deploys, RLS makes the consent model enforceable in the database instead of in promises, London residency, generous free tiers, and a clean path from web app to store app without a rewrite.

## Where each business pushes it furthest

- [[RoomCare]]: the deep end — device tokens stored hashed, append-only event history, 60-minute signed URLs for private media, a seven-attack live security proof before real data. Runbooks: [[Deploy and Backend Runbooks (RoomCare)]].
- [[MyHomework]] and [[Intervooh]]: owner-only RLS, a single edge-function AI proxy with server-side spend budgets, Stripe billing behind the same wall.

## Related

- [[Map - Processes]] · [[Home]]
- [[RLS and Schema Change Process]] · [[Deploy and Backend Runbooks (RoomCare)]]
- [[RoomCare]] · [[MyHomework]] · [[Intervooh]]
