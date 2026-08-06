---
tags: [plan, cross]
source: built from repo evidence (roomcareOS/interviewprep, v1, myhomework, yfarmx, scenecast) + the Todoist board, 6 August 2026
updated: 2026-08-06
---

# The Mega Monetisation Plan
### Five businesses, twelve months, one person — September 2026 to August 2027

**Written 6 August 2026, revised the same day after an adversarial audit of every calculation.** Dollar figures converted at $1.27 to £1 (assumed rate — check on the day) and rounded. Every figure is either sourced (repo doc, named Todoist card) or marked *est.* An *est.* marked **[verify]** must be checked against a real source before money moves on it — the market-rates research pass (TikTok creator rates, Shop fees, drone day-rates, ad RPMs) did not complete and needs a follow-up session.

---

## 1. The headline, honestly

**Jay's goal: £200,000 profit across the five businesses in the twelve months to August 2027.**

**The evidence does not support that number, and this plan will not pretend otherwise.** What it does support:

- **Base case: a loss of roughly £5,000** with the OpenRouter budget staged — or **a loss of roughly £17,000** if the £1,400/month AI budget is spent flat from month one. Total base revenue is about £7,500 against costs of £13,000 (staged) to £25,000 (as budgeted).
- **Stretch case: £20,000–£25,000 profit**, if Intervooh's funnel converts near the optimistic end, the TikTok audiences arrive on schedule, and posters sell — with AI spend held to what revenue justifies.
- **What £200,000 would actually require:** roughly 1,000 paying Intervooh subscribers at £16/month sustained, versus a base model of ~135 mixed buyers across the whole year. Or a genuinely different play: commissioned drone services are the only high-ticket line on the board (£150–£400/job *est.* **[verify]**), and services are the one way a solo founder earns five figures in year one without an audience. RoomCare investment, if it lands, is **runway, not profit — it never counts toward the goal**.

The honest framing: **this is the year the five businesses earn their first real money and prove which one deserves the next year.** £200k is the year-two target chased from a proven base — and the December revision of this document, with three months of real numbers, is where that argument starts.

### One-screen summary (12 months to Aug 2027, cash in the window)

| Business | Revenue base | Revenue stretch | Direct costs (base) | Contribution (base / stretch) |
|---|---|---|---|---|
| **Intervooh** | £4,000 | £20,000–£25,000 | ~£1,300 | **+£2,700 / +£18,000–£22,000** |
| **MyHomework** | £1,000 | £3,000 | ~£1,300 | **−£300 / +£1,500** |
| **YFarmX** | £600 | £6,500 | £5,100 staged (£17,100 as budgeted) | **−£4,500 staged; −£16,500 budgeted / ≈ £0** |
| **Norwich Drones** | £1,050 (margin + creator fund) | £3,000 | ~£250 | **+£800 / +£2,900** |
| **RoomCare** | £1,000 | £5,000 | £4,500–£7,500 | **−£4,000 / −£1,000** (raise = runway, never profit) |
| **Scenecast** | £0 | £0 | £0 | Parked until it has an owner and a thesis |
| **Total** | **≈ £7,500** | **≈ £35,000–£40,000** | **≈ £13,000 staged / £25,000 budgeted** | **≈ −£5,000 staged, −£17,000 budgeted / ≈ +£20,000–£25,000** |

Norwich's revenue line is stated net of print costs and Shop fees (margin), because gross poster sales are not Jay's money; every other line is revenue. The maths behind every cell is in its section.

---

## 2. Intervooh — the engine (intervooh.com)

**Why it leads:** billing is fully built in code — Stripe Checkout, customer portal, webhook plan-sync (`src/lib/billing.ts`, `supabase/functions/billing/index.ts`). Everything blocking revenue is configuration, not code. Fastest revenue switch-on in the estate.

### Rung 0 — the gates (September). All carded, all p1:

1. **"Merge claude/new-session-2q7se5 and confirm the Vercel deploy"** — *this comes first.* The live 200 job pages currently publish the literal string `[object Object]` under a Preparation notes heading, are orphans reachable only from the sitemap, and declare the wrong canonical host. The branch carries the fix, 24 sector hubs, the footer that links them, and the SEO audit script. **The entire traffic model below assumes this merge; today's estate does not earn.**
2. **"Add the SUPABASE_SERVICE_ROLE_KEY function secret"** — without it the Stripe webhook throws and paid users are never marked Pro.
3. **"Run migration 0002 in the Supabase SQL editor"** — without it the £29 pass cannot grant its 28-day Pro window and AI budgets silently weaken. It degrades quietly: confirm it, never assume it.
4. **"Complete the Stripe setup so billing can go live"** — account, two products (**"Intervooh Everything" £16.00/month; "One Interview Pass" £29.00 one-off** — docs/SETUP-BILLING.md), webhook, four secrets. Rehearse in test mode first, as the doc says.
5. **"Set intervooh.com as the primary domain in Vercel"** — the apex 503s while all 230 sitemap URLs point at it.

Before the trial opens wide: the abuse cards (**global daily spend ceiling with free-tier kill switch**, **per-user cap: 3 trial interviews**, **per-IP cap**, **Turnstile CAPTCHA**, **email verification check**) and **"Register with the ICO as a data controller"** (£40–£60/yr).

### Rung 1 — the SEO funnel pays (November onwards)

Base model, **cash in the window**, all rates *est.* and deliberately below the first draft's (6% visit→signup and 8% signup→buy are top-quartile figures; cold informational SEO traffic typically converts at 1–3% — the base uses 4% and 6%):

| In-year totals (Nov–Aug) | Base | How derived |
|---|---|---|
| Visits | ≈ 56,000 | ramp from 2,000/month (Nov) to 8,000/month (Aug) after the merge + domain fix |
| Signups at 4% *(est.)* | ≈ 2,260 | free, no-card setup |
| Buyers at 6% of signups *(est.)* | ≈ 135 | mix *est.*: 60% take the £29 pass, 40% take £16/month staying ~2.5 months ("cancel the day you get the job" is the site's own copy) |
| **Cash in the window** | **≈ £4,000** | 81 passes × £29 ≈ £2,350, plus 54 subscribers × £16 × ~2 billed months average in-window ≈ £1,730 |

Run-rate milestones: ≈ £250/month cash by February, ≈ £550–£650/month by August. **Stretch ≈ £20,000–£25,000**: traffic roughly triples (short-video marketing once the five brand films have voices — blocked on the Google AI Studio cards) *and* conversion hits the 6%/8% optimistic band. The honest error bar is wide: four stacked estimated rates mean base-case cash could land anywhere from ~£1,500 to ~£8,000.

**Costs scale with the funnel — modelled, not capped away.** Trial AI ≈ $0.18/trial user (3 interviews × ~$0.06 cheap-tier): base ≈ 2,260 users ≈ $410 ≈ £320/year; stretch ≈ £1,450. The $55/month cap card is a floor for launch, not a ceiling for the year — **lift the cap as signups grow, because the per-user cap is what bounds unit cost**. Infra ~£40/month, Stripe ~3% effective, Pro-user AI £0.50–£1.00 per heavy programme against £16 (PRD §10).

### Rung 2 — App Store presence (May)
Capacitor wrap for discovery only; **Stripe Checkout on the website, never in-app** (board policy), so Apple's commission never touches margin. Two honest caveats: Apple's developer account is $99/yr, and **a thin wrapper whose only purchase path is external web billing carries real App Store review risk** (minimum-functionality and external-purchase rules) — treat May's submission as an experiment that can bounce, not a certainty.

### Rung 3 — B2B careers services
Deferred in the PRD. No revenue planned from it this year.

**Kill test:** if traffic is under 1,000 visits/month by January despite the merge and domain fix, the SEO thesis needs marketing spend or content expansion before the model can be believed.

---

## 3. MyHomework — small, honest, second (myhomework.app)

**The model:** free during beta, then **about £9/month, set up and paid by a parent** — the promise already on the pricing pages. The parent pays; the child never sees a checkout. That single design keeps children's-payment risk out of the product.

### Rung 0 — the gates (September–December)

- **"Unblock Vercel deployments for the myhomework project"** (p1, Inbox) — **the plan's first draft missed this and it gates everything**: the last three deploys were rejected, the live site is frozen at 18 July, and the fix involves a decision Jay has explicitly deferred (the project sits in the RoomCare Hobby team; moving it to personal scope is free but briefly takes the domain down). **No beta, no paid launch, nothing ships until this clears.**
- **"Add a per-user AI cap to myhomework.app before the free trial opens"** — 50 questions/trial user at ~$0.002/question (cheap tier); cost scales at ~$0.10 per fully-used trial, so even 1,000 trial users ≈ £80. The cap protects against abuse, not against success.
- **"Write an automated jailbreak regression suite for the coach"** (p1) — the evidence a school or parent will ask for.
- **"Wire the saved booklet photos into the coach's context"** (p1) — the product must do what the My materials copy promises.
- **"Draft the privacy notice and DPIA for solicitor review"** (£500–£1,000 *est.*) before the beta opens beyond invited families.
- **New gate this revision: a safeguarding escalation route for the consumer product.** "Coach conversations are never visible to parents" is the right privacy stance, but the consumer app needs a defined route for when a child discloses harm to the coach — the schools version has one planned; the parents version currently has none. Design it before open beta; it will also be asked about in every school conversation. *(Carded 6 Aug.)*
- Trademark: **"Get a UK trademark search on the myHomework name"** — see Risks; run before any store submission or paid marketing.

### Rung 1 — parent subscriptions (paid launch March)

Build order per the existing card: join-code parent link → Stripe Checkout on the web (never in-app) → webhook to a subscriptions table → entitlement check in the ai function. **Stripe Tax from the start** (UK digital VAT applies from the first pound).

Base maths *(est.)*: paid from March, 10 parents growing to 30 by August — average ~18 across 6 paying months: 18 × £9 × 6 ≈ **£1,000**. Stretch: the class beta converts and testimonials land — 10 growing to 100 by August averages ~55 parents: 55 × £9 × 6 ≈ **£3,000**. Base direct costs ≈ £1,300 (solicitor £750 mid, trial AI ~£100, ICO £52, infra share ~£400), so the base contribution is slightly negative — this year MyHomework buys evidence, not profit.

### Rung 2 — schools
Procurement plus safeguarding review will not pay inside 12 months. Any school pilot is evidence, not income.

**Targets:** deploys unblocked in September; invited-family beta October; paid switch-on March (its own push month); 10 paying parents by end of March, 30 by August.

---

## 4. YFarmX — audience first, and the truth about the AI budget (yfarmx.com)

**Where it truly is:** live, publishing daily — and real traffic is ~350 genuine visits/week once the bot flood is excluded (board card "Decide how to handle the automated traffic hitting /search/"). One logged TikTok post. Podcast built, not public. **There is no monetisable audience yet.** Jay's instinct — audience first, then monetise — is right, and this plan keeps it.

### The uncomfortable number first

The planned OpenRouter budget is **£1,400/month = £16,800/year, before a pound of YFarmX revenue exists**. At base-case growth YFarmX cannot earn back a tenth of that this year. **Stage it**: £200–£400/month for the TikTok run, heroes and the daily scrape, raised only when a revenue rung switches on. Staged ≈ £4,800/year — an £12,000 swing, the single largest controllable number in this whole document. Sequencing (both carded): **rotate the pasted OpenRouter key first**, then **turn off auto top-up**, then buy the first month by hand; ask the accountant the **VAT reverse-charge question** before the first invoice (it changes what £1,400 buys by ~20%).

### The ladder

**Rung 0 — audience (Sep–Feb, the long rung).**
- **TikTok:** 60 cuts briefed, two a day for 30 days. Blocked on three cards: the plan review, the voice approval, and the Google AI Studio credits (every Gemini call currently returns 429 — no voice can render). **Realistic start: within ~2 weeks of those clearing — target September, not mid-August as first drafted.**
- **Podcast:** built, three episodes, feed validating; blocked on the R2 credential/binding and cover-approval cards. Public in October.
- **SEO/site:** compounding already; the coin pages and hubs are YFarmX's version of Intervooh's estate.

**Rung 1 — programmatic ads (at ~50,000 real pageviews/month; realistically Q3).** Display RPM £3–£6 *(est.)* **[verify]** → 20k pageviews ≈ £60–£120/month. Base-case year ≈ **£600**. Ads on today's traffic would earn ~£5/month and cost credibility. **Compliance note: on a crypto-heavy site the ad network itself can serve non-compliant crypto promotions to UK users — block the crypto/investment category in the ad platform from day one** (Jay's FCA duty does not stop at his own affiliate links).

**Rung 2 — affiliate (Q3–Q4, hard legal gate).** Non-crypto only: AI tools, VPNs, hardware, courses. **Crypto affiliate links are regulated financial promotions — see Risks before placing a single exchange link.** Base £0; stretch £2,000–£4,000.

**Rung 3 — sponsorship and newsletter (Q4 at the earliest).** £100–£300/slot *(est.)* **[verify]** once TikTok is at 10k and the podcast has an audience. Stretch only.

**Stretch total ≈ £6,500** (affiliate £2–4k + ads ~£1.5k + a few sponsored slots — the components' own ceiling; the first draft's £8,000 exceeded its own parts). Costs staged ≈ £4,800 + ~£300 Google AI = £5,100; the honest stretch outcome is **break-even**, and that would be a genuine win for year one of an audience business.

**Kill test:** if TikTok is under 5,000 followers after the full 60-cut run plus two months, change the format before spending more render budget.

---

## 5. Norwich Drones — small money, fast, low effort

**Today:** 2,500 TikTok followers, target 10,000; norwichdrones.com not built. The Seedance-week loops card and the planning card both exist.

**Rung 0 — the run to 10k (Aug–Feb).** 4–5 posts/week from existing aerials plus the Seedance loops. Cheapest audience-build on the board; 10k by ~February is plausible *(est.)* if cadence holds.

**Rung 1 — creator programme (at 10k).** UK creator-reward rates *(est.)* **[verify — the market research pass did not complete]**: roughly £0.20–£1.00 per 1,000 qualified views → **£40–£300/month on good months, likely nearer the bottom; plan ≈ £150 total**. A badge, not a business — and check the exact eligibility thresholds in-app before promising anything against it.

**Rung 2 — posters via TikTok Shop (Shop live March; first cash April; five selling months).** Stated properly this time, gross and margin separated:
- Sell at £18. Print-on-demand cost £5–£8 delivered *(est.)*. TikTok Shop referral fee ≈ 9% (~£1.60) — **UK fees have risen; verify the current category rate before pricing**.
- **Margin ≈ £8–£11/unit; plan at ~£9.**
- Base: 20 units/month × 5 months = 100 units → gross £1,800, **margin ≈ £900**. Stretch: ~300 units over the window → gross £5,400, **margin ≈ £2,700**.
- Needs: Shop seller approval, 3–5 designs from the best-performing aerials, a print partner (no stock risk).

**Rung 3 — commissioned drone work (Q3, demand-led — and the plan's wildcard).** The only high-ticket line anywhere on the board: £150–£400/job *(est.)* **[verify local rates]** for estate agents, roof surveys, events. **Commercial operation needs CAA compliance (operator ID, likely A2 CofC or GVC, insurance) — verify before quoting a single job.** Not in the base numbers because it spends Jay's scarcest resource, time — but if the £200k ambition needs a lever this year, five-figure services income is the only credible one on this page. Build norwichdrones.com as a one-page enquiry form in Q3 and let enquiries prove demand.

**Targets:** loops in Seedance week (carded, by 13 Aug); 5k followers by November; 10k by February; Shop live March; first poster cash April.

---

## 6. RoomCare — the truth about the big one (roomcare.uk)

**Jay's framing, held to:** the serious project, the VC target. **Investment is runway, not profit; it never counts toward the £200k.**

**Where it truly is:** well-built, security-serious, pre-revenue, no real customer. Willow Grange is seed data. No tablet has been paired to a real room; the first live end-to-end request has not happened. No pricing exists anywhere in the repo — the model below is constructed, not quoted.

### Pilot revenue, constructed

Per-room per-month SaaS: £10–£20/room/month (a 40-bed home at £15/room ≈ £7,200/yr), bracketed by care-tech comparators at £1–£3/bed/week *(est.)* **[verify]**. **First invoice:** a 3-month paid pilot at £500–£1,500 flat for 5–15 rooms in an *independent* home (owner-manager decides, signs in 4–8 weeks), conversion terms pre-agreed. A free pilot with a signed letter of intent is worth more to the raise than the fee. **Base: one paid pilot ≈ £1,000. Stretch: conversion plus a second pilot ≈ £5,000.** Councils run 9–18 months and cannot pay in this window; do not chase them for revenue.

### Before the first paid pound (all carded, plus three gaps)

- Product last-mile: **pair the Room 12 tablet**, **run the first live tea request**, **migration 0003**, then **re-run the live security proof**.
- Compliance: **ICO registration** (hard gate before pilot data), **Cyber Essentials** (~£300–£600). **CE Plus (£1,500–£2,500) is honestly a fundraising cost, not a pilot requirement for one independent home** — buy it for the raise, and it doubles as the first pen test. **CSO sign-off (£1,000–£1,500, Jay's 6 Aug call)** — strengthens both procurement and diligence; note that clinical-safety questions also arrive via insurers and CQC-registered managers, not only council commissioners.
- Not yet carded, must be drafted before an invoice: pilot agreement, pricing sheet, per-home DPIA, data-processing agreement — **and a consent approach for residents who lack capacity** (Mental Capacity Act best-interests process; resident data is special-category data and the current consent model assumes a resident who can consent).
- Infra: Vercel Pro + Supabase Pro ~£35–£40/month from pilot; tablets ~£100–£200/room; PI/PL + cyber insurance ~£500–£1,000/yr *(est.)*.

### The raise (mid-2027)

Target £150k–£500k. **SEIS covers the first £250,000 (lifetime company cap); anything beyond is EIS** — file SEIS advance assurance early, it is the standard sweetener for UK angels and it is not yet in any repo doc. Show: 1–3 live pilots with usage evidence from the Insights data the product already collects, one paying or converting home, the diligence pack (Security-and-Compliance.md is genuinely strong collateral), TAM (~16–17k UK care homes *(est.)* **[verify]**), per-room unit economics. The demo page *is* the pitch meeting.

**Targets:** last-mile proof September; compliance purchases October; five independent-home approaches November; one pilot signed by December–January; live by February; conversion conversation and any pilot #2 in June (not stacked into May); seed conversations from June.

---

## 7. Scenecast — one line

No revenue thesis, no card, no owner this year. £0 in the model. Its value in this window is reputational (a public, well-run open-source project supports the raise narrative). Park it.

---

## 8. The quarterly roadmap — one person, one launch-grade push per month

The first draft broke its own rule twice; this version does not. Everything not named "push" in a month must be maintenance-weight.

**Q1 — switch the engine on**
- **September — push: Intervooh live.** The merge card, the three billing cards, domain fix, test-mode rehearsal, first paid customer. Also that month (small, sequenced): rotate the OpenRouter key → auto top-up off → first staged credits; unblock MyHomework Vercel (a dashboard conversation, not a build); TikTok run starts when its three blockers clear.
- **October — push: YFarmX Briefings podcast launch.** RoomCare compliance purchases (clicks, not a push). MyHomework invited-family beta opens.
- **November — push: RoomCare pilot outreach.** Five independent homes, demo page in hand. Intervooh funnel tuning only.

**Q2 — second engine, first pilot**
- **December — push: MyHomework subscriptions built and tested** (quiet outreach month). RoomCare negotiation continues.
- **January — push: RoomCare pilot go-live.** DPIA, hardware, Pro tiers on. Intervooh passes £250/month cash.
- **February — push: Norwich Drones Shop launch** (10k push, seller approval, designs, print partner). *MyHomework paid launch moves out of this month — first real payments from real parents is launch-grade and gets March to itself.*

**Q3 — widen what works**
- **March — push: MyHomework paid launch** to beta families then public, with testimonials.
- **April — push: YFarmX monetisation switch-on** (ads if the traffic gate is met, first non-crypto affiliates, sponsorship kit). Posters selling.
- **May — push: Intervooh App Store submission** (accepting the review-risk caveat). RoomCare pilot #2 and conversion talks explicitly deferred to June.

**Q4 — evidence and the raise**
- **June — push: RoomCare** — conversion conversation, pilot #2 if it is there, seed materials, SEIS advance assurance filed.
- **July — push: double down on the year's best revenue line** (by now the numbers say which).
- **August — review against this document** and write the year-two plan from a proven base.

---

## 9. The full cost table (12 months, base case)

| Line | Amount | Notes / source |
|---|---|---|
| OpenRouter (YFarmX) | **£4,800 staged (recommended)** / £16,800 as budgeted | Jay's stated budget; stage it. Rotate the pasted key first; auto top-up off; accountant's VAT answer before first invoice |
| Intervooh trial AI | ~£320 (scales ~$0.18/trial user; stretch ≈ £1,450) | per-user cap card; cap lifts with growth |
| Intervooh Pro-user AI + infra + Stripe | ~£800 | infra ~£40/mo, Stripe ~3% effective, Pro AI per PRD §10 |
| MyHomework trial AI | ~£100 (scales ~$0.10/fully-used trial) | per-user cap card |
| MyHomework solicitor (privacy notice + DPIA) | £500–£1,000 *(est.)* | carded |
| Google AI Studio (voices + coach) | ~£200–£400 *(est.)* | the two Inbox cards; also unblocks three businesses' films |
| Supabase Pro + Vercel Pro (three commercial apps) | ~£800–£1,000 | ~£35–40/month per live app |
| ICO fees (RoomCare, Intervooh, MyHomework) | ~£130–£180 total | carded |
| RoomCare: Cyber Essentials + CE Plus | £300–£600 + £1,500–£2,500 | CE Plus = raise collateral + first pen test |
| RoomCare: CSO sign-off | £1,000–£1,500 | Jay's 6 Aug decision, carded |
| RoomCare: pilot tablets + insurance | £1,000–£2,000 + £500–£1,000 *(est.)* | 10 rooms; PI/PL + cyber |
| Trademark searches (both app brands) | ~£200–£400 search; UKIPO filing £170+ if pursued | both carded |
| Apple developer account | ~£78/yr ($99) when the store app ships | web billing keeps commission at £0 |
| Poster print + Shop fees | netted out of Norwich's margin line | ~£5–8/unit + ~9% fee **[verify]** |
| Domains, R2, Cloudflare, misc | ~£200–£400 *(est.)* | existing estate |

**VAT notes.** Registration triggers at ~£90k taxable turnover — but **the threshold is per entity, not per app: if the five lines run through one company (or Jay as one sole trader), their turnover aggregates, and reverse-charged purchases from abroad (OpenRouter) count toward it as deemed supplies**. At £16,800/year, OpenRouter alone is a fifth of the threshold — the accountant card is not optional homework. Stripe Tax on from the start for both subscription apps.

**Jay's time is the unpriced line.** The rhythm below implies 15–25 hours/week on top of the daily newsroom. A launch-grade push is a half-day block, and a launch month usually needs two or three of them, not one. If a month's push does not fit, the push slips — the rule is the rule precisely so that slipping one thing does not slip five.

---

## 10. Risks and mitigations

| Risk | Reality | Mitigation |
|---|---|---|
| **FCA cryptoasset promotions (YFarmX)** | Crypto promotions to UK consumers are regulated financial promotions; unapproved inducement breaches s21 FSMA — a criminal offence | Ads + **non-crypto** affiliate only. No exchange links without paid legal advice and an FCA-authorised approver. **Also block crypto/investment categories in the ad network** — the risk includes ads served *to* the site, not just links placed *by* it |
| **myHomework trademark collision** | A US "myHomework Student Planner" has traded 15+ years with store presence | Search card runs **before** store submission or paid marketing. Decide early: clear, coexist, or rebrand while it is cheap |
| **Children's data (MyHomework)** | Under-16 users; ICO Children's Code; payments must never touch the child | Parent pays via join-code (built into the design); solicitor-reviewed notice + DPIA before open beta; **consumer safeguarding escalation route now carded**; age-assurance thinking, not just parent-pays billing |
| **Care procurement + clinical safety (RoomCare)** | Independents 4–8 weeks; groups 6–12 months; councils 9–18 months. Clinical-safety questions also come from insurers and CQC managers. Resident data is special-category; capacity/consent needs a Mental Capacity Act answer | Sell only to independent single-site homes this year. CSO engaged per Jay's decision. Capacity/consent process drafted with the DPIA before any real resident uses it |
| **App Store review (Intervooh, May)** | A thin wrapper with external-web-only billing can be rejected on minimum-functionality / purchase rules | Treat the submission as an experiment; the web product is the business either way |
| **TikTok dependence (YFarmX + Norwich)** | Reach can halve overnight; creator-fund income is pocket money | Multi-home every asset (Shorts/Reels at near-zero marginal cost); the podcast and sites are owned channels; email capture from Q2; never build revenue plans on creator-fund income |
| **AI spend abuse** | The varg.ai case: 118 farmed accounts, $590 in 48 hours | Ceilings, per-IP caps, alerts, CAPTCHA — all carded, all in **before** trials open wide |
| **One person, five businesses** | The real constraint; slippage compounds | One push per month, kill tests per ladder, the weekly rhythm. Ties go to Intervooh — best evidence-to-revenue ratio on the board |

---

## 11. The weekly operating rhythm

**Daily (45–60 min on top of the newsroom):** the two scheduled TikTok posts (batch-rendered); one Norwich post from the queue; one 30-second glance at Stripe and the AI-spend alerts.

**Weekly:**
- **Monday — money.** Stripe, signups, trial→paid, both TikTok counts, bot-filtered traffic, AI spend vs caps. One scoreboard note, 15 minutes. This is where the plan stays honest.
- **Tuesday — the month's push.** The half-day block. In a launch month, expect a second such block midweek.
- **Wednesday — RoomCare.** One sustained block: outreach in autumn, the pilot in winter, evidence and raise materials from spring.
- **Thursday — batch production.** Next week's cuts for both channels, podcast, poster candidates.
- **Friday — clicks and board hygiene.** Clear the Inbox "Dashboard clicks" and "Money, keys & accounts" cards; re-triage; name next week's push.
- **Weekend — nothing scheduled.** Slack is what keeps a one-person system alive.

**Monthly (first Monday):** score against §8, run every kill test, pick the next push. A business that fails its kill test twice running gives its time to whichever line is earning. **The plan serves the goal, not the portfolio.**

---

## 12. What would have to change for £200,000

So the target is not just declared impossible: the three levers, in order of credibility —

1. **Stage the OpenRouter budget** (worth £12,000 of the gap on its own — the largest controllable number here).
2. **Make Intervooh subscription-first and buy growth once unit economics are proven.** ~1,000 sustained £16/month subscribers is the only *product* path to the number; billing is genuinely days from live, and every pound of proven funnel maths justifies acquisition spend the current plan doesn't dare include.
3. **Commissioned drone services.** High-ticket, no audience required, gated only by CAA compliance and Jay's calendar. The one credible five-figure line for year one.

Pulling all three hard, a defensible ceiling this year is **£30,000–£50,000 profit**. £200,000 is the year-two number, argued from December's real data.

---

## Related notes in this vault

- The businesses: [[Intervooh]] · [[MyHomework]] · [[YFarmX]] · [[Norwich Drones]] · [[RoomCare]] · [[Scenecast]]
- Gates named above: [[Billing Setup (Intervooh)]] · [[Accounts and AI Switch-On (Intervooh)]] · [[Launch Plan (MyHomework)]] · [[Children's Data and Safety Checklist (MyHomework)]] · [[UK Compliance Position (RoomCare)]] · [[Security Architecture (RoomCare)]]
- How the work runs: [[Article Pipeline (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Todoist Doctrine]] — Todoist owns the live state; this plan owns the reasoning behind it
- [[Map - Businesses]] · [[Map - Processes]] · [[Home]]

---

*Revised 6 Aug 2026 after adversarial audit: headline recomputed on one cash basis; Intervooh cumulative reconciled with its own funnel; AI-cost caps modelled as scaling, not fixed; two board blockers added as gates (the Intervooh SEO merge, the MyHomework deploy block); TikTok start reset behind its real blockers; SEIS capped correctly at £250k; February and May de-stacked; Norwich restated gross vs margin; MyHomework stretch re-derived. Re-run against real numbers quarterly — the December revision will know more than this one.*
