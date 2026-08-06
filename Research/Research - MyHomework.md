---
tags: [research, myhomework]
source: myhomework/docs/RESEARCH.md
updated: 2026-08-06
---

The verified research behind every design choice in [[MyHomework]]. From a deep research pass (107 agents, adversarially fact-checked) plus a second pass on competitors and AI prices, dated 16 July 2026. Where the evidence is weak, the doc says so.

## The single most important finding

A randomised controlled trial (PNAS, Bastani et al., high-school maths): plain GPT-4 access made students 48% better on practice, then **17% worse on the exam** once the AI was removed. A **guardrailed hints-only tutor** made practice 127% better with the exam harm gone entirely. This is the strongest causal evidence that "guide, never solve" — the heart of [[Teaching Approach (MyHomework)]] — is a safety property, not a style choice.

## Does homework help, and what kind?

- EEF (the UK's what-works body): homework adds ~+5 months progress per year at secondary — but the EEF rates its own evidence security lowest tier, so directional only.
- Three solid design rules: task quality beats volume (impact *falls* as hours rise); homework linked to classwork works better ("what did you do in class on this?"); feedback while working raises impact.
- Spaced practice is robust (maths meta-analysis g = 0.28); retrieval quizzing is solid across subjects but not proven for maths — so quizzing ships for fact-heavy subjects, as an experiment for maths.

## Do AI tutors work?

- Pre-LLM intelligent tutoring systems: the largest meta-analysis (Ma et al. 2014, 14,321 participants) found step-based tutors statistically indistinguishable from one-on-one human tutoring, including as homework aid. Caveats: effects shrink on standardised tests, and it all predates modern AI.
- Keeping chats on-topic is easy; **depth is the hard part** — a 2025 K-12 deployment found 38–50% of tutor conversations under-reached the intended thinking depth. Hence attempt-before-hint and the depth monitor.
- Honest limit: **whether an LLM Socratic tutor improves learning for unsupervised 11–16-year-olds is unproven. It is the product's central bet** — instrumented so it can be measured.

## Gamification and teen trust

- Gamification helps modestly, through **autonomy** and **relatedness**, barely through competence. Leaderboards backfire for 13–17s (stress, exclusion; one study saw motivation *decrease*). So: self-set goals, forgiving streaks, no comparisons — as decided in [[Decisions - MyHomework]].
- Young teens can't reliably tell honest system messages from manipulation (2024 ACM study) — one pushy prompt and the whole app reads as a scam. Every prompt says why, offers a real "no", never fakes urgency.
- Teens receive a median of 237 notifications a day; batching into ~3 predictable daily moments beat both continuous pings and none. Duolingo data: reminder wording wears out (rotate it); ignored reminders should switch themselves off.

## The law (UK)

- 13+ can consent for themselves (DPA 2018 s.9); the ICO Children's Code applies in full and dictates the architecture — data minimisation, high-privacy defaults, no nudge techniques. Operationalised in [[Children's Data and Safety Checklist (MyHomework)]].
- Ofcom position: a one-to-one chatbot with no live web search and no user-to-user sharing is outside Online Safety Act Part 3.

## Competitors (verified July 2026)

- **Khanmigo**: closest design precedent (Socratic, refuses answers, moderates everything) — but not purchasable by UK families, no capture-and-plan loop.
- **ChatGPT Study Mode**: scaffolds but is a toggle — one tap back to answers. Its optionality is exactly our gap.
- **Photomath / Brainly**: answer-first, "the cheating apps". **Snapchat My AI**: wrote an essay in a press test; the ICO enforcement that followed forced Snap's risk assessment to name homework misuse by 13–17s — direct DPIA precedent.
- **Duolingo**: engagement benchmark and manipulation cautionary tale (worst dark-pattern score in education); copy the streak freezes and self-retiring reminders, reject the guilt. **Quizlet**: never paywall the core loop after the fact. **MyStudyLife**: nearest planner, no tutor.
- UK schools set homework via Satchel One, Class Charts, Google Classroom; only Classroom has a student-facing API (gated by Google verification + school admins). **Fast capture is the only universal integration.**
- Position confirmed: nobody in the UK combines instant capture of real homework with an enforced guide-don't-solve coach.

## Costs (read 16 July 2026 — re-verify before publishing)

- One 30-minute guided session: ~1–2p on budget vision models (gpt-5-nano, Gemini 2.5 Flash-Lite, Llama 4 Scout), 4–6p mid-tier; prompt caching cuts 30–58% and is a build requirement.
- One capture (voice note + photo) under 1p; vision models read handwriting at 1–2% character error, so no separate OCR service.
- A 10–15 student beta: a few pounds a month. **Quality, not cost, picks the model.**
- Self-hosting (the Ollama question): a capable rented GPU is ~£200–300+/month regardless of use; break-even ~10,000–28,000 sessions/month, and the server bills through 13 weeks of school holidays. Decision: per-token API now, hosted open models as the middle path, self-host only at scale. Ollama stays for free local dev.

## Safeguarding practice

Khanmigo's model (moderate every chat, tell children what adults see, alert on flags, cap daily time) is matched. Character.AI is the boundary marker: task-scoped tutor, never open-ended companion. DfE product-safety standards (Jan 2026) specify the tiered response the app mirrors; NSPCC practice shapes the fixed wording; Childline 0800 1111 is the signpost. Moderation: OpenAI Moderation API (free) primary, Llama Guard 4 fallback; avoid Perspective API (sunsets end of 2026).

## Known limits

EEF figures are low-security evidence; the tutoring meta-analysis predates LLMs; three findings rest on preprints; the dark-patterns study covers ages 11–12, not 13. Stated honestly so decisions can be revisited.

## Links

- [[MyHomework]]
- [[Map - Research]]
