---
tags: [decision, roomcare]
source: [docs/V2-Review.md, docs/V3-Plan.md]
updated: 2026-08-06
---

# Decisions - RoomCare

The direction-setting decisions for [[RoomCare]], distilled from two commissioned outside reviews (Jay had other agents drive and critique the whole product) and the triage of their findings. The pattern itself is a decision worth keeping: **build, commission an honest outside review, triage it in writing (applied / deferred / declined with reasons), so future sessions know why.**

## Decision: v2 is the "truth and follow-through" release (10 July 2026)

The v1 review's verdict: the bones are good (calm brand, kind copy, the request timeline as central metaphor, the honoured "nothing plays until chosen" shelf promise), but the failures are failures of **honesty and follow-through** — the app sometimes told Margaret things that were not true. That framing set the product's north star:

- **No invented names, no timer-driven progress.** A staff name appears only after a real person acted; until then, "Waiting for the team". The emergency bar advances only on real Console events. In a demo a simulated timeline is a simulation; in a real home it is a lie with safety consequences.
- **An emergency must never look like a cup of tea**: full-width burgundy takeover on the Console, never sorts below anything.
- **Forgiveness everywhere**: 10-second Undo on staff actions, soft cancel for residents ("I'm alright now, thank you"), take-back for unopened family photos.
- Delete the pretend staff member from any build a home could see.

## Decision: one honest service model (v3, 10 July 2026)

Everything became **event-sourced**: a request is a stream of recorded events; what each screen shows is computed from those events in one shared place; an undo is itself a recorded correction; nothing is ever quietly rewritten. Copy generators live in shared code so no app invents its own story. Safety findings applied at the same time: the speech fallback can never fire an action the resident did not choose, Emergency during privacy keeps privacy on, "Stop" halts a moving bed at once, duplicate asks offer a choice instead of silently duplicating, and out-of-scope eye-tracking teasers were removed. A build flag (`demo | pilot | production`) structurally separates demo behaviour from real-home builds.

## Deferred to the Supabase phase (needs a real server)

Offline outbox with idempotent sync, staff auth and roles, consent enforced by RLS with history, device health telemetry, shared audited handover storage, video calls, multi-resident family accounts, real insights, performance work. All recorded so nothing is mistaken for forgotten.

## Flagged for Jay, deliberately not applied

- **Information-architecture rebuilds** (replacing the resident tile rail, a "My work" phone Console, restructured Family app): thoughtful, but the PRD names the v0.2 interface as the approved benchmark and the rail is per-resident configurable by design. Changing the fundamental shape of the apps is a founder decision; the event model would support it without rework if Jay chooses.
- **"Tap to talk" as the default**: possible later as a profile option; hold-to-talk stays per the approved reference.
- **A window-open request type**: sensible, cheap once per-home safety rules exist at the device phase; not added so the request set stays matched to the PRD.

## Where Jay's side disagreed with the reviewer

- **"No open requests. Lovely." stays.** The empty queue is a staff surface in a calm moment; the warmth is the voice. Playful copy remains banned near health and emergency, and the codebase enforces that ([[Copy Rules (RoomCare)]]).
- **Seeded demo content stays in demo mode** because in demo mode the seeded content *is* the product; it is gated behind the mode flag, never removed.

## Related

- [[Map - Decisions]] · [[RoomCare]]
- [[Claude Operating Profile - RoomCare]] · [[Copy Rules (RoomCare)]]
- [[RLS and Schema Change Process]] · [[Supabase Stack Pattern]]
