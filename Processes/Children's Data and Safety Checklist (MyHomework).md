---
tags: [process, myhomework]
source: myhomework/SECURITY-COMPLIANCE.md
updated: 2026-08-06
---

The children's-data and safety build checklist for [[MyHomework]]. Regulatory frame: UK GDPR + Data Protection Act 2018, the ICO Children's Code (the UK rulebook for services children use), PECR (cookies/notifications). It is a **build checklist, not a legal essay** — solicitor review before public launch.

The one-paragraph version: build for children, so the strictest rules apply by default. Hold as little data as possible (nickname, not name), default every setting to high privacy, never profile or track for advertising, keep all data in the UK/EU, send the AI provider only homework content (never the student's identity), moderate everything the AI says both ways, and give students a real delete button. **The Children's Code is not a hurdle for this product; it is a description of the product.**

## What we hold vs refuse

- Hold (minimum, London region): nickname + avatar, age band, password/passkey credential, optional recovery email (own table, never joined to content), school subjects (not school), homework captures, task metadata, session transcripts, push token, 90-day pseudonymous telemetry.
- Refuse to hold: real name, date of birth, school name, address, geolocation, contacts, ad identifiers, tracking cookies. Sign-up works with zero PII (personally identifiable information).
- Data that sneaks in anyway: photos may show a printed name (crop encouraged, EXIF including GPS stripped, private-to-account); voice is personal data (transcribe then delete audio — decided); students may type personal things to the coach (safeguarding + retention handle it, excluded from analytics).

## Standing rules (design-time, always on)

- [ ] 13+ self-consent; under-13s blocked with a friendly "not yet" screen (see [[Decisions - MyHomework]]).
- [ ] Every setting defaults to highest privacy; each optional element (notifications, parent link, recovery email) is a separate consent switch.
- [ ] No behavioural profiling, no geolocation, no nudges toward more data or less privacy (banned-pattern list tested in review).
- [ ] Parent link is an openly stated read-only mirror with an in-app indicator — no covert monitoring, ever.
- [ ] AI calls are **identity-blind**: the provider gets homework content keyed by a random per-session ID, never the account, nickname or email. Provider must contractually not train on the data; UK/EU endpoints preferred.
- [ ] Moderation both directions (student→model and model→student). Primary: OpenAI Moderation API (free, text+images); fallback: Llama Guard 4. Do NOT build on Google's Perspective API (shuts down 31 Dec 2026).
- [ ] Safeguarding: on distress signals the coach stops teaching and shows a **fixed, pre-written** screen signposting Childline 0800 1111, Samaritans 116 123 and a trusted adult — it never counsels or keeps a crisis chat going. Tiered response per DfE standards (signpost → flag → alert); flagged events reviewed within 48h. The coach is a task-scoped tutor, never an open-ended companion (the Character.AI boundary).
- [ ] Row Level Security on every table, CI-tested (a test user must FAIL to read another's task); AI key server-side only; signed expiring photo URLs; no public buckets; no third-party scripts.
- [ ] Retention runs itself: voice audio deleted on transcription; photos 90 days after task completion; transcripts 30 days verbatim then summarised; self-serve delete-my-account effective immediately.
- [ ] No user-to-user features at all — keeps the app outside Online Safety Act Part 3. Three PRD-level gates that would change that: content sharing between users, community/bot features, live web search for the coach. Re-analyse before shipping any of them.

## Pre-launch checklist (before public beta)

- [ ] DPIA completed and signed off — must explicitly name homework/essay misuse by 13–17s (the Snap My AI enforcement lesson), self-harm disclosure handling, pseudonymity limits, provider data flows.
- [ ] Child-readable privacy notice (reading age ≤ 12) + full notice, user-tested with the co-designer.
- [ ] Terms of service (UK-only, 13+, education-support product, not counselling).
- [ ] Provider DPAs executed; no-training and retention terms pinned; transfer addenda where needed.
- [ ] ICO registration fee paid.
- [ ] RLS test suite green; wrong-account read red-team checked.
- [ ] Jailbreak/answer-leak regression suite green (release blocker — see [[Teaching Approach (MyHomework)]]).
- [ ] Safeguarding responses reviewed against current NSPCC/DfE guidance.
- [ ] Retention jobs implemented and tested.
- [ ] Breach runbook: detect → contain → assess → notify ICO within 72h if reportable → notify users.
- [ ] Solicitor review of this document, the notice and the ToS.

Feeds Stage 0 of [[Launch Plan (MyHomework)]]. Evidence base: [[Research - MyHomework]] (law and safeguarding parts).

## Links

- [[MyHomework]]
- [[Map - Processes]]
- [[Supabase Stack Pattern]] — where the data actually sits (London region, row-level security).
