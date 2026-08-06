---
tags: [skill, myhomework]
source: myhomework/docs/HOW-WE-TEACH.md, myhomework/PRD.md (§6)
updated: 2026-08-06
---

How the [[MyHomework]] AI coach teaches. This is the product's core skill: the pedagogy (teaching method) is enforced behaviour, not a style choice.

## The one rule everything hangs on

**The coach never does the homework.** Not the essay, not the answer, not the summary. It guides; the student does every real step.

Why it's non-negotiable: a randomised trial (PNAS, Bastani et al.) found students given plain GPT-4 did 48% better on practice then **17% worse on the exam** once the AI was gone; a hints-only guardrailed tutor improved practice (+127%) with **no exam harm**. Answer-giving AI creates the feeling of learning while quietly removing the learning. Full evidence trail in [[Research - MyHomework]].

## The method, step by step

1. **Say it back.** Session opens with "tell me in your own words what it's asking" — misreading the task is the most common failure.
2. **Small, concrete, do-it-yourself steps.** Short, structured, feedback-rich work beats long slogs (EEF: task quality matters more than time spent).
3. **Attempt before hint.** The coach must see the student's try before helping further — feedback tied to an attempt is where the learning happens.
4. **The hint ladder.** A stuck student never gets a flat refusal and never the answer; one rung more help per request: rebuild understanding → conceptual hint → specific hint → walk the method together in micro-steps, the student performing each one. **The final step is always taken by the student.**
5. **React like a teacher.** Name one genuinely strong thing, ask one question about what's missing, never rewrite the student's work.
6. **Deepen deliberately.** AI tutors under-reach the intended thinking depth 38–50% of the time, so difficulty steps up each cycle (recall → explain → connect → argue) and depth is tracked as a metric.
7. **Pictures when they beat words**; **close with reflection** ("what do you know now that you didn't an hour ago?"). Sessions cap at 45 min with a break nudge at 25 — more time is not more learning.
8. **Stuck is fine.** After genuine struggle: this is a good one to ask your teacher tomorrow — the app saves exactly where the student got stuck. Honesty beats fake omniscience.

## How "guide, never solve" is enforced (layered, all server-side)

1. Core pedagogy prompt + per-subject playbooks (Maths, English, Computer Science first), versioned in the repo.
2. The backend, not the model, owns the session step machine — each AI call is scoped to the current step, so the model can't "helpfully" jump to the solution.
3. An independent answer-leak filter checks every reply before the student sees it; leaks are regenerated.
4. Depth monitor scores transcripts against the intended thinking level.
5. Refusal UX = the hint ladder — never a lecture, never a stonewall; wording co-designed with the 13-year-old so it lands as fair.
6. Tone bible: warm, funny, genuinely clever; never cheesy AI-speak, never patronising (decided in [[Decisions - MyHomework]]).
7. Jailbreak regression suite: a fixed battery of "give me the answer" tricks runs on every prompt change; any leaked deliverable is a release blocker.

Honesty clause: a determined student can always open another AI in another tab. The bet is making the guided path pleasant and fast enough that doing it properly beats cheating — stated openly to parents.

## Safety inside a session

Distress signals end pedagogy mode: fixed signposting to Childline (0800 1111) and a trusted adult, per the process in [[Children's Data and Safety Checklist (MyHomework)]]. The model never learns who the student is. The coach admits it can be wrong; "ask your teacher tomorrow" is an encouraged outcome.

## Live vs coming (18 July 2026)

Live: never-solve rule + leak filter, say-it-back opening, hint ladder + attempt-before-hint, warm voice, diagrams, deadline-aware planning, Childline signposting (prompt-level). Coming: spaced revision prompts (2 & 7 days), push deadline ladder, self-set goals + forgiving streaks, depth monitor, curriculum-aware coaching from uploaded textbooks, automated jailbreak suite.

## Links

- [[MyHomework]]
- [[Map - Agents and Skills]]
- [[Research - MyHomework]]
