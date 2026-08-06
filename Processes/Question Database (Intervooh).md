---
tags: [process, intervooh]
source: docs/QUESTION-DB.md
updated: 2026-08-06
---

# Question Database (Intervooh)

The 200-job interview question database inside [[Intervooh]]: how it's designed, how questions get picked, and how it's maintained. Built 17 July 2026 against live web research; evidence base in [[Research - Intervooh Evidence Base]].

## What it is

A structured database of the **200 most-recruited job titles across 24 sectors** (UK-first, with US/EU coverage). Each job carries its own competency model, typical interview stages, 2026 format notes and a role-specific question bank — layered over per-sector banks and a universal core bank. It powers three product behaviours **with no AI call at all** (this is what makes the free tier essentially free to run):

1. **Role matching in set-up** — typing a role searches titles + aliases; a match pulls in competencies and pre-selects typical stages.
2. **"The questions to expect"** — a deterministic top-20 for the specific interview, ranked by stages, region, era and the pasted advert.
3. **Quick-fire practice** — three questions a day from that list, rotated by date, answered out loud against coach tips.

At M1+ the same data feeds the AI layer as context, so "company + city + job title + date" is enough for tailored coaching.

## The data shape (enough to reason about)

- Every **question**: text, type (behavioural / technical / situational / motivational / strengths / case / values), optional competency link, optional stage/region pins, an era tag (`classic` or `"2026"` for AI-use questions), and a one-line coach tip.
- Every **job**: 4–6 competencies (exactly one marked `core` — the programme engine schedules it early and repeats it), 2–4 typical stages, 8–10 role-specific questions, regional prep notes, and the sources actually used.

## Selection pipeline (deterministic — same inputs, same list, every time)

1. **Hard filters**: drop questions pinned to stages the user doesn't face or non-matching regions.
2. **Scoring**: specificity (job +3 / sector +2 / core +1), stage-pinned +2, era-2026 +1, advert mentions the competency +2.
3. **Coverage guarantees**: every competency ≥1 question, ≥1 motivational, ≥2 era-2026.
4. Top 20, stable-ordered. Daily practice picks 3 of the 20, rotating with the date.

## Honesty rules (load-bearing)

- Official frameworks named only where real: Civil Service Success Profiles behaviours, the six NHS Constitution values, safeguarding in teaching/care, trade certifications.
- **No question claims to be "asked at <Company>"** — questions are *commonly asked* for the role; company-specific tailoring is the AI layer's job, grounded in the user's own pasted material.
- Source URLs stored per job and per sector; British English, coach-voice tips throughout.

## Maintenance

- `npm run build:data` validates everything (enums, id uniqueness, competency references, company-attribution lint) and regenerates the search index; **CI runs it as a release blocker**.
- Trimming or adding jobs/sectors is a data edit, no code changes — which is why the "pick 5 role families" launch decision was superseded ([[Decisions - Intervooh]]).
- Refresh cadence: era-2026 questions half-yearly; sector format notes yearly; sources re-checked whenever a claim graduates into marketing copy ([[Marketing Claims Bank (Intervooh)]]).

## Related

- [[Map - Processes]] · [[Intervooh]]
- Browser checks of the role-matching and questions-to-expect flows: [[Skill - Verify (Intervooh)]]
