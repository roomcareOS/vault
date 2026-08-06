---
tags: [decision, myhomework]
source: myhomework/QUESTIONS.md
updated: 2026-08-06
---

The 15 launch questions for [[MyHomework]], answered by Jay (by voice) on 16 July 2026 and folded into the PRD. This is the decision log.

1. **Name** → MyHomework, on the owned domain myhomework.app. Caveat: a US "myHomework" planner app already exists — trademark check before public launch (see [[Launch Plan (MyHomework)]], where this is flagged as the single biggest business risk).
2. **First audience** → private beta: daughter + invited friends → possibly her school → public.
3. **First subjects** → Maths, English, Computer Science (each needs its own coaching playbook).
4. **Under-13s** → blocked at launch (13+ can consent for themselves under UK law; a parental-consent flow is extra build for later).
5. **Parent visibility** → full transparency: a linked parent sees what the student sees, read-only. "It's supposed to help the student; nothing to hide." The student is always told what's visible.
6. **School system** → Google Classroom (school-managed Chromebook). Import wanted but staged to M3.5 — gated on Google OAuth verification and the school admin's policy; manual capture stays the guaranteed path.
7. **Voice notes** → transcribe to text, delete the recording (most private option; no keep-audio in v1).
8. **Notifications** → yes: sort-your-inbox nudge ~17:00 after school; reminder 3 days before a deadline if the task isn't done.
9. **Gamification** → streaks with 3 free freeze days per week; self-set goals asked at the start and changeable. No leaderboards (see [[Research - MyHomework]] for why).
10. **Coach personality** → warm, funny, really smart; sophisticated wit; human; brilliant at explaining hard topics. Never cheesy AI.
11. **Strictness** → never gives the answer; every plea for the answer moves one rung deeper on the hint ladder (understand → hint → stronger hint → guided micro-steps). Spelled out in [[Teaching Approach (MyHomework)]].
12. **Budget** → founder covers beta, hard cap £25/month to start. Long-term price aim ≤ £5/month, possibly a £10–20 heavy-use tier — decide with real data.
13. **Devices** → her iPhone 16 (installed PWA at home) + school Chromebook (plain browser tab at school).
14. **Homework format** → set digitally via Classroom, done digitally or in exercise books — so text/screenshot/photo capture, photo-of-book still supported.
15. **Success in 3 months** → homework happens without nagging; she can easily see what's due; when stuck she presses Start and the coach guides her. (This is the metric the product optimises.)

Still open after this round: trademark/brand check; final model choice (bench on her real homework); pricing tiers; whether her school's Google admin permits Classroom access.

## Links

- [[MyHomework]]
- [[Map - Decisions]]
