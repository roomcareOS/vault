---
tags: [skill, intervooh]
source: .claude/skills/verify/SKILL.md
updated: 2026-08-06
---

# Skill - Verify (Intervooh)

A Claude Code skill in the [[Intervooh]] repo (`.claude/skills/verify/`) that **builds, launches and drives the app end-to-end in a real browser** to verify a change at its real surface — the UI a user actually touches, not just the unit tests.

## What it does

1. **Build + launch**: `npm run build` (typecheck + production build), then `npm run preview` serves the built app locally with SPA fallback (single-page-app routing, so deep links work).
2. **Drive it**: launches Chromium via Playwright (a browser-automation library) and clicks through the app the way a user would, asserting what appears on screen.
3. **Fails on any console error** — the skill collects browser console and page errors and treats any as a failure; the app should be error-free.

## When it runs

Whenever an agent (or Jay) wants proof a change actually works in the product — after edits to the wizard, engine, question database or screens — rather than trusting that the unit tests passing means the UI is fine. It complements, never replaces, the release-blocker checks (typecheck, lint, tests, data validation).

## The flows it drives

- **Setup wizard end-to-end**: company → role → family → date → stages → minutes → advert → "three things you're proud of" → review → "Build my programme".
- **Programme shapes**: an interview 21 days out must produce the "Standard programme", 5 days the "Short-runway" shape, 1 day the "Final-48-hours plan" — the three deterministic engine shapes.
- **State survives reload**: block completion ticks persist across a page reload; deep links to an interview reload correctly; unknown routes land on Home.
- **Settings**: JSON export fires a download; "Delete all data" returns Home to the empty state.
- **Jobs database flow**: role suggestions come from the generated index, a match shows "Matched: <title>", and the "questions to expect" + three-question practice flow render — the browser-side proof of [[Question Database (Intervooh)]].
- **Dark mode and mobile**: dark colour scheme gives the right background; a 390×844 phone viewport must show no horizontal overflow.

## Gotchas it encodes (institutional knowledge)

- Wizard controls are plain buttons/checkboxes found by accessible name — the selectors are documented so scripts don't guess.
- The wizard draft persists per browser tab; clear it (or use a fresh browser context) between runs.
- Interview dates must be between today and one year out.
- The engine is deterministic and clock-free — assertion dates must be computed from the calendar, not hard-coded.

## Related

- [[Map - Agents and Skills]] · [[Intervooh]]
- Same verify-at-the-surface discipline as the other products on the [[Supabase Stack Pattern]] stack, e.g. [[MyHomework]]
- Runs after ops changes too: [[Accounts and AI Switch-On (Intervooh)]]
