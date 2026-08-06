---
tags: [agent, roomcare]
source: [CLAUDE.md]
updated: 2026-08-06
---

# Claude Operating Profile - RoomCare

How Claude Code is directed on the [[RoomCare]] repo (`CLAUDE.md`). This is the fullest operating profile in the estate and the template for how Jay works with an AI builder anywhere.

## Working style with Jay

Jay is not a developer and usually works by voice from a phone. So Claude must:

- Explain every decision in one or two plain-English sentences **before** doing it. No jargon without a translation in brackets.
- Work in **small, verified steps**. After each step: what changed, how it was verified, and the exact copy-pasteable command or URL Jay needs, one at a time.
- **Never assume Jay ran something.** Ask for the output if a step depends on it.
- On an ambiguous instruction: pick the safest reading, say what was assumed, offer the alternative.
- If Jay references a file or screenshot that did not actually arrive: stop and say so. Never guess at missing content.

## Hard rules (non-negotiable)

1. **The orchestrator (intent service)** — the AI orchestration layer — has an internal codename that must NEVER appear in code, comments, UI strings, file names, commit messages, logs, error messages or any documentation that could ship. In code it is called `orchestrator` or `intent-service`, nothing else.
2. Always "resident", never "patient"; British English everywhere. Full banned-word list in [[Copy Rules (RoomCare)]].
3. **Non-clinical boundary**: nothing that diagnoses, advises on health or monitors anyone medically. Health-flagged input routes to the nurse path with fixed, calm wording. Reject scope creep even when asked casually; point to PRD §12 and ask Jay to confirm in writing.
4. Burgundy `#7A2F3C` is reserved for the emergency/nurse-call path only.
5. **Emergency and privacy behaviours may never be weakened by a refactor** (emergency always available; privacy mode pauses the microphone). Add a regression test for each before touching related code.
6. Secrets only in environment variables; the Anthropic API key lives in Supabase Edge Function secrets, never in any app bundle; never commit a `.env`.
7. Data lives in the Supabase **London** region. Flag loudly before adding any third-party service that would process resident data outside the UK/EU.
8. Every schema change ships with row-level security and a failing-read test; append-only tables get INSERT-only policies — see [[RLS and Schema Change Process]].

## Engineering conventions

- pnpm monorepo: `apps/resident`, `apps/console`, `apps/family`, `packages/ui`, `packages/core`, `packages/config`. Shared logic (intents, SLA maths, consent checks, types) belongs in `packages/core` with unit tests.
- TypeScript strict (no `any` without an explaining comment); React + Vite + Tailwind; design tokens live in `packages/ui/tokens.ts` — never hard-code brand hex values in components.
- Accessibility is a feature: keyboard and screen-reader paths for everything, visible focus, reduced motion honoured, touch targets at least 44px on the resident app.
- Realtime via Supabase channels; optimistic UI with reconciliation; the resident app is a PWA (a web app that installs and works offline) with an offline outbox for requests.
- Tests: Vitest for `packages/core`, Playwright for the PRD acceptance flows. `pnpm typecheck && pnpm lint && pnpm test` must pass before anything is called done, and Claude must say it ran them.
- Conventional commits; small PR-sized changes even when committing straight to main.
- On finishing a feature, update `PROGRESS.md`: what shipped, how to see it, what is next. Jay reads that file to keep his bearings.

## Definition of done (any feature)

1. Behaviour matches the PRD section, quoted by number.
2. Types, lint and unit tests pass; a Playwright test covers the happy path.
3. Copy obeys [[Copy Rules (RoomCare)]] — grep for the banned words before finishing.
4. RLS verified if data was touched.
5. Deployed preview URL given to Jay with a one-line "what to tap to see it".

## Task tracking

Governed by the estate-wide [[Todoist Doctrine]]: this repo files into the `RoomCare` project; anything needing Jay goes to Inbox with full context; the board is read at session start and moved as states change.

## Related

- [[Map - Agents and Skills]] · [[RoomCare]]
- [[Copy Rules (RoomCare)]] · [[RLS and Schema Change Process]] · [[Todoist Doctrine]]
- [[Supabase Stack Pattern]] · [[Deploy and Backend Runbooks (RoomCare)]]
- [[Tool - Claude Code]] — the tool this profile directs.
