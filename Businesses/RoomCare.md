---
tags: [business, roomcare]
source: [README.md, RoomCare-PRD.md, PROGRESS.md, docs/Security-and-Compliance.md]
updated: 2026-08-06
---

# RoomCare

**roomcare.uk** — the bedside operating system for independence, care requests and family connection in care homes. A resident controls their room, reaches their family and sends small requests to the care team by word, touch or gesture; requests arrive tidied and ordered on the team's screen with gentle time limits; family stay close between visits, always on the resident's terms.

**The non-clinical boundary in one line:** *Works alongside the nurse call, never in place of it* — RoomCare never diagnoses, never monitors anyone medically, and never stands between a person and urgent help; every decision that touches care is made by a person.

RoomCare Ltd, England and Wales, company no. 17317321. The richest operating doctrine in Jay's estate: [[Claude Operating Profile - RoomCare]] governs how it is built, [[Copy Rules (RoomCare)]] governs every word on screen, [[Decisions - RoomCare]] records why it is shaped the way it is.

## Three apps, one backend

| App | Who uses it | Where |
|---|---|---|
| **Resident** | The resident, in their room | Bedside or wall tablet, full screen, per-resident configurable interface |
| **Console** | The care team | Nurse-station wall screen, desktops, staff phones |
| **Family** | Relatives | Their own phones |

All three sit on **one Supabase backend in the London region** (a hosted Postgres database with built-in accounts, realtime and file storage — kept in London so resident data never leaves the UK/EU). This is the [[Supabase Stack Pattern]] Jay uses across [[MyHomework]] and [[Intervooh]] too, pushed hardest here: the database itself enforces who can see what (see [[RLS and Schema Change Process]]), the history of every request is append-only (can be added to, never edited — the audit trail for families and inspectors), and photo links die after an hour. An AI orchestration layer (server-side only, allowlisted intents, fully audited) handles the routine; people handle care.

The spine of the product is the **event-sourced request lifecycle**: every request is a stream of recorded events, and no screen ever says anything a real event has not made true. "Honest UX" is treated as a security property — misplaced reassurance is the most damaging failure in care.

## Status snapshot (6 August 2026)

- **All three apps built and working together.** v1 through v4 shipped between 10 and 24 July 2026: the faithful React port of the v0.2 prototype, the Console queue with SLA timers and emergency takeover, the Family app with the "nothing plays until chosen" shelf, then the truth-and-follow-through (v2) and one-honest-service-model (v3) releases per [[Decisions - RoomCare]].
- **The live database exists and the security proof passed** (12 July): Supabase project `roomcare-pilot` in London, full schema applied, all seven simulated attacks refused (anonymous reads blocked on 11 of 11 tables, forged writes refused, wrong-home staff sees zero rows, append-only history cannot be edited, bogus pairing codes rejected).
- **Sign-in, tablet pairing and the RoomCare mark** landed the same day: real Supabase accounts on Console and Family, managers mint one-time pairing codes for bedside tablets, vector logo everywhere.
- **A live demo page built for showing people** (24 July): Margaret's room, the console and Sarah's phone on one page, running the real apps against one shared demo world; screens glow when they receive something; every signal comes from real recorded events. Ten browser checks drive it end to end, on top of 125 unit tests and five end-to-end scenarios.
- **Next:** first real sign-in on the Console, pairing a tablet for Room 12, and the first live cup of tea end to end. Deploy and connection steps live in [[Deploy and Backend Runbooks (RoomCare)]].

## Related

- [[Map - Businesses]] · [[Home]]
- [[Claude Operating Profile - RoomCare]] · [[Todoist Doctrine]] (this repo files under the `RoomCare` project)
- [[UK Compliance Position (RoomCare)]] · [[Security Architecture (RoomCare)]] — the two halves of `docs/Security-and-Compliance.md`: what Jay can claim to buyers, and the engineering that earns it
- [[Copy Rules (RoomCare)]] · [[RLS and Schema Change Process]] · [[Supabase Stack Pattern]]
- [[Deploy and Backend Runbooks (RoomCare)]] · [[Decisions - RoomCare]]
- [[Mega Monetisation Plan]] — RoomCare's 12-month job: pilots and a fundable raise, not profit
