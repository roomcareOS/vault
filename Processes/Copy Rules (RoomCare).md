---
tags: [process, roomcare]
source: [CLAUDE.md, RoomCare-PRD.md]
updated: 2026-08-06
---

# Copy Rules (RoomCare)

The word-level law for every [[RoomCare]] UI string, error message and notification (PRD §13, CLAUDE.md rule 3). Enforced automatically: `node scripts/check-copy.mjs` fails the build if banned wording appears, and the definition of done in [[Claude Operating Profile - RoomCare]] requires a grep before any feature is finished.

## The rules (exact wording is load-bearing)

- **British English.** Warm, plain, concrete.
- Always **"resident"**, never "patient" (the adjective patient is fine).
- **No em dashes.**
- **Never the word "matters"** — sole exception: the phrase **"moments that really matter"**.
- **No buzzwords:** seamless, revolutionise, empower, cutting-edge, game-changing, leveraging.
- **No antithesis constructions:** no ", not X", no "instead of", no "rather than". The one sanctioned antithesis is the fixed line:

  > **"Works alongside the nurse call, never in place of it."**

- AI is described **at most once per surface**, as the "agentic AI orchestration layer" handling pre-agreed paths. The internal codename of the orchestrator never appears anywhere (see [[Claude Operating Profile - RoomCare]]).
- **Health-adjacent strings are calm and never playful.** Playful copy is banned anywhere near health or emergency (the warm "No open requests. Lovely." on an empty staff queue survives because it is neither — see [[Decisions - RoomCare]]).

## Colour rule that travels with the copy

Burgundy **`#7A2F3C` is reserved for the emergency/nurse-call path only.** No other element may wear it, in any theme. Emergency must also stay the loudest element in every theme, including high contrast.

## Why so strict

The audience is residents who may be anxious or disoriented, and their families. The screen must never change its story, overpromise, or sound like marketing. Truthful, calm copy is treated as a safety property, the same as the honest event timelines.

## Related

- [[Map - Processes]] · [[RoomCare]]
- [[Claude Operating Profile - RoomCare]] · [[Decisions - RoomCare]]
- The same instinct applied to marketing copy: [[Marketing Claims Bank (Intervooh)]]
