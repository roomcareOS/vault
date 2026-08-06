---
tags: [process, roomcare]
source: [docs/Deploying-To-Vercel.md, docs/Connect-The-Backend.md, docs/RoomCare-Setup-Guide.md, supabase/functions/README.md, README.md]
updated: 2026-08-06
---

# Deploy and Backend Runbooks (RoomCare)

The merged runbooks for putting [[RoomCare]] online and wiring its backend. Written for Jay in plain English; every step is redoable and nothing in them can damage anything. The stack itself is the [[Supabase Stack Pattern]].

## Running locally

```
pnpm install
pnpm dev:resident   # http://localhost:5173
pnpm dev:console    # http://localhost:5174
pnpm dev:family     # http://localhost:5175
```

With no Supabase keys configured every app runs in **demo mode**: fully working with made-up data (Margaret in Room 12, Emma and Aisha on shift), safe to show anyone. Checks: `pnpm typecheck && pnpm lint && pnpm test` plus `node scripts/check-copy.mjs` for [[Copy Rules (RoomCare)]].

## Runbook 1: Vercel (hosting), once per app

- [ ] Vercel → Add New → Project → import the `v1` repo.
- [ ] Set **Root Directory** to `apps/resident` (then repeat the import for `apps/console` and `apps/family`). Leave everything else; the repo carries the build settings.
- [ ] Deploy. Each app gets its own link; after this, every push to main goes live in about a minute and every branch gets a preview link.
- [ ] Later, custom domains per project: `room.roomcare.uk`, `console.roomcare.uk`, `family.roomcare.uk` (one DNS record each, pasted into cPanel).

## Runbook 2: connect the backend (about 30 minutes)

- [ ] **Create the Supabase project**: name `roomcare-pilot`, **region London (eu-west-2)** so resident data stays in the UK. Save the generated database password in a password manager, never in a chat.
- [ ] **Copy two values** from Settings → API: the Project URL and the anon public key. These are safe to share with the apps; they only work through the security rules. **Never share the `service_role` key with anything or anyone** — it bypasses all security.
- [ ] **Apply the database**: the migrations in `supabase/migrations/` (`0001_init.sql`, `0002_service.sql`) create every table, the RLS policies and the guarded functions. Then run the security proof per [[RLS and Schema Change Process]] before any real data enters.
- [ ] **Give the apps the keys**: in each of the three Vercel projects, add environment variables `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, and `VITE_ROOMCARE_MODE=pilot`, then redeploy. While `VITE_ROOMCARE_MODE` is missing or `demo`, the apps keep the safe made-up data, so this can be done gradually.
- [ ] **First people and first room**: create the home and rooms (prepared SQL), first staff sign-ins (Supabase Auth, linked to the home), pair a bedside tablet with a one-time code (burned on first use), send a family invite.
- [ ] **Prove it end to end** on two devices: ask for a cup of tea on the tablet, watch it appear on the Console, press Accept, watch the tablet update.

## Runbook 3: edge functions (server-side programs on Supabase)

Two Deno functions do what the database cannot: rate limiting, CORS locked to the app origins, and later the model call whose key never leaves the server.

- `provision-device` — exchanges a manager-created pairing code for the bedside tablet's device token.
- `tidy-intent` — the orchestrator boundary: verifies the device token, then turns an utterance into one allowlisted intent. Deterministic today; the model call slots in behind the same guards.

Deploy with `supabase functions deploy <name>` after `supabase login` and `supabase link`. Secrets are set once per project with `supabase secrets set` — the names are `ALLOWED_ORIGINS` and (when the model call ships) `ANTHROPIC_API_KEY`. Secrets live only there: never in an app bundle, never in git. Neither function uses the service-role key, raw utterances are never logged, and the deterministic intent gate mirrors `packages/core/src/intents.ts` (core is the source of truth; change both in the same commit).

## Costs (as written for Jay, July 2026)

Roughly £0 while demoing on free tiers. Once a home depends on it: Supabase Pro (~$25/month, no pausing, daily backups) plus Vercel Pro (~$20/month, required once commercial) — about £35 to £40 a month.

## If something goes wrong

Copy the exact error message and paste it to Claude Code with a line about which step you were on. Every step can be redone.

## Related

- [[Map - Processes]] · [[RoomCare]]
- [[Supabase Stack Pattern]] · [[RLS and Schema Change Process]]
- [[Claude Operating Profile - RoomCare]] · [[Copy Rules (RoomCare)]]
