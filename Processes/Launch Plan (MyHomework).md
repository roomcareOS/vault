---
tags: [process, myhomework]
source: myhomework/docs/LAUNCH-PLAN.md
updated: 2026-08-06
---

The path from family beta to a paid product for [[MyHomework]], in order. **Each stage has a gate: finish it before spending money or attention on the next.**

## Stage 0 — before anyone outside the family (the non-negotiables)

Housekeeping (an afternoon):
- [ ] Delete test accounts (`testpilot`, `testpilot2`, `livetest`) in Supabase → Authentication → Users.
- [ ] Delete the `claude-deploy` token in Vercel once building is done.
- [ ] Move the Gemini key to a billing-enabled tier (still pennies per session) so the per-minute limit stops biting.

The name (start early — takes weeks):
- [ ] UK trademark search on "myHomework" (classes 9/41/42, ~£200) + IP solicitor view. A well-known US app of the same name has been on both app stores for a decade. **This is the single biggest business risk on the list.** Outcomes: keep the name for a UK web app (risky on app stores) or rebrand while the audience is tiny. Decision context in [[Decisions - MyHomework]].

Legal pack (~a week, mostly templates):
- [ ] Privacy notice written for both a 13-year-old and their parent, reflecting what is actually true (nickname-only, London data, photos private, voice deleted, no ads, no tracking).
- [ ] Terms of service (parent is the contracting party for under-18s).
- [ ] DPIA (data protection impact assessment) — [[Children's Data and Safety Checklist (MyHomework)]] is 80% of the raw material.
- [ ] Register with the ICO as a data controller (£40–60/year).
- [ ] Privacy + Terms links in the footer, plus a contact email for data requests.

Safety proof (built; needs a written record):
- [ ] Run the jailbreak battery against the live coach after each big prompt change and **keep the transcript** — the answer to a school asking "how do you know it won't write the essay?".

**Gate: all four boxes done. Nothing here needs code.**

## Stage 1 — school-gate beta

Goal: 20–50 real students at the daughter's school, zero pounds spent. Her class first; then a one-page letter for the head of year (what it does, what it never does, QR code — it already runs on their Chromebooks in the browser).

Watch four numbers weekly (all visible in Supabase):
- Week-2 return rate (target: half).
- Tasks saved per active student per week (target: 3+).
- Coach sessions ending with the task done (target: 60%+).
- AI cost per active student per week (under 5p on flash-lite).

Collect student/parent quotes with written parent permission — three good quotes replace a marketing budget.

**Gate: week-2 retention around half, and at least one teacher who'd say so publicly.**

## Stage 2 — marketing that fits a one-person company

The story writes itself: *a dad built the anti-cheating homework app with his 13-year-old daughter, and it refuses to do your homework for you.* In order: UK parenting/education press (The Times education, TES, Schools Week, Mumsnet) → TikTok/Shorts of the coach charmingly refusing (the refusal IS the content; post as the founder, not a brand) → local parent groups, honestly → SEO later → Product Hunt once there are testimonials. **Do NOT pay for ads before retention is proven.**

## Stage 3 — the app stores

The PWA is genuinely enough for the whole beta (iPhone: Share → Add to Home Screen; push works). When store presence is worth it: Google Play first (easy TWA wrap, one-off $25, Families policies apply); Apple App Store only after revenue (Capacitor wrapper, $99/year, and Apple's rules mean **parents subscribe on the website; the iOS app is just a place to sign in** — in-app digital subscriptions must use Apple's cut and you may not link out to your own checkout).

## Stage 4 — the paid model

Stripe Checkout + Billing on the website, **parent pays** (parent's email lives in Stripe, linked to the student's nickname by a join code — the first real email in the system). A `subscriptions` table updated by a Stripe webhook; entitlement checked server-side in the `ai` function (free = capped sessions, paid = full daily cap). Beta families get a founding-family price. Test £9/month or £79/year, sibling discount. Use Stripe Tax from day one (UK digital VAT applies from the first pound). Honest metric: AI cost per paying family per month (should be under 50p). Build order: join code → Checkout + webhook → entitlement check → receipts; roughly one build session.

## The order, compressed

1. Tidy accounts + tokens + paid AI key (afternoon).
2. Trademark search (start now).
3. Legal pack + ICO registration (a week).
4. Class beta → school beta; watch the four numbers.
5. Founder-story press + TikTok when retention holds.
6. Play Store wrap; App Store after revenue.
7. Stripe parent subscriptions when beta ends.

## Links

- [[MyHomework]]
- [[Map - Processes]]
- [[Children's Data and Safety Checklist (MyHomework)]]
