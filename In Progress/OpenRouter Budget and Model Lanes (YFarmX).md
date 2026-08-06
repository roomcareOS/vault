---
tags: [in-progress, yfarmx]
source: yfarmx@claude/open-router-budget-plan-nu1s99:docs/openrouter-budget.md, yfarmx@claude/open-router-budget-plan-nu1s99:docs/monthly-plan.md
updated: 2026-08-06
---

# OpenRouter Budget and Model Lanes (YFarmX)

**Status: unmerged.** This lives only on the branch `claude/open-router-budget-plan-nu1s99` in `roomcareOS/yfarmx`, which is **316 commits behind main** and has never been merged. The open Todoist card ("Resolve the OpenRouter plan branch", p1) is a straight decision: cherry-pick the useful parts onto a fresh branch off main, or close the branch and keep only this note. **The thinking below is worth keeping; the numbers are not current.** Everything was written on 28 July 2026 against that day's OpenRouter price list, and the model market moves weekly — treat every dollar figure as a shape, not a quote, and re-read the live prices before acting on any of it.

**The one number to hold in view.** £1,400 a month is £16,800 a year, and that makes it the single biggest cost line anywhere in Jay's estate — larger than every other subscription, tool and service combined. [[Mega Monetisation Plan]] recommends **staging it at £200–400/month** instead, raised only when a revenue rung actually switches on, which saves roughly £12,000 across the year. This branch plans how to spend £1,400 well; it does not argue that £1,400 should be spent. Those are separate questions and the monetisation plan answers the second one.

## What the £1,400 is actually meant to buy

Two things come off the top before a single word is generated.

| | |
|---|---|
| Cash out | **£1,400** |
| Less VAT at 20% (possibly reclaimable — see below) | −£233 |
| Less OpenRouter's card top-up fee, 5.5% | −£61 |
| **Credits landing in the account** | **≈£1,106** |
| In dollars, at the 25 July rate of $1.332 to the pound | **≈$1,473** |

Split as Jay asked: **$1,052 operating** (about $34.60 a day, for everything routine) and **$421 reserve**, spent deliberately in week four on one named thing rather than dribbled away. Roughly a fifth of the cash never becomes usable credit at all, which is the first argument for staging.

**The VAT question is the biggest free win on the page.** OpenRouter is a US company, and cross-border digital services bought by a UK VAT-registered business are normally handled by the *reverse charge* (the supplier charges no VAT; the business accounts for it on its own return and reclaims it in the same breath, so the net cost is nothing). If that applies, the same £1,400 buys **$1,768 instead of $1,473 — 20% more, every month, for one email to the accountant.** Two things to confirm, and it is the accountant's call, not this note's: is the company VAT-registered, and does a real OpenRouter invoice show VAT charged or a reverse-charge note. [[Mega Monetisation Plan]] adds the sting in the tail: reverse-charged purchases from abroad count towards the ~£90k registration threshold as deemed supplies, and at £16,800/year OpenRouter alone is a fifth of it.

## The branch contains two different plans, and the second one supersedes the first

This is the most important thing to understand before cherry-picking anything, because the two files disagree and both are dated 28 July.

`openrouter-budget.md` assumes **everything runs on credits** — article text, code, images, video, the lot. It splits the operating pot into seven lanes led by Newsroom ($260/month) and site build ($250/month), and concludes that £1,400 buys "roughly 3–4 hours a day of Opus-class building, or 9–11 hours if most of it runs on Sonnet".

`monthly-plan.md` then measures what was actually happening and rewrites the shape. Over eleven days and four projects — 6,715 API calls, 2.789 billion tokens, a **97.7% cache hit rate** — the Claude Max subscription was found to be carrying about **$6,400/month of usage at API rates, for a $200 flat fee. Thirty-two times what it costs.** So text and reasoning are free at the margin and belong on the subscription; the credits should buy only what the subscription physically cannot do.

| | Original lanes (`openrouter-budget.md`) | Revised lanes (`monthly-plan.md`) |
|---|---|---|
| Biggest lane | Newsroom, $260 | **Video / studio, $380** |
| Article text and code | On credits | **On the Claude Max subscription, $0 at the margin** |
| Cost per published article | ~$1.35 | **~$0.21** (media only) |
| Newsroom lane | $260 | $90 (heroes, audio, discovery crons) |
| Bulk work | Scattered | Its own "workers" lane, $120 |
| Hermes | Not a lane | Lane F, $130 — demoted to a side project |

**Use the revised shape and treat the original as background.** The revised lanes are: newsroom media $90, studio/video $380, workers $120, free-trial AI $100, research $180, Hermes/RoomCare $130, ops $45 — $1,045 total, about $34 a day, with $421 of reserve held back and starting at zero.

One subtlety in the revision that is easy to miss and worth more than it looks: **moving bulk work onto cheap credits protects the subscription, not just the wallet.** The whole 32× economy rests on that 97.7% cache hit rate, and the cache lives an hour on a subscription but only five minutes once usage spills onto pay-as-you-go credits. At a 50% hit rate the same work would cost nearly five times as much. So a daily scrape run by Claude Code itself burns the weekly allowance Jay needs for writing; the same scrape on a cheap model costs pennies and leaves the allowance intact.

## The lane caps are scheduling guards, not spending guards

This is the conceptual heart of the plan and the part most worth keeping regardless of what happens to the branch.

The design gives each lane its own OpenRouter key, and each key a hard `limit` with a `limit_reset` of daily, weekly or monthly, set through OpenRouter's management-keys settings. It looks like spending control. **It is not.** The only cast-iron control is one setting:

> **Auto top-up OFF. Buy the credits once a month, by hand.**

An account with no auto top-up cannot spend money that is not in it. Once that is true, the bill *cannot* grow — so overspend stops being a financial risk and becomes a scheduling one, which is a far easier problem. Every per-key cap exists to stop the month running dry on the 19th, not to stop the bill growing, because the bill has already been made incapable of growing. That reframing is what makes the three rules that follow enforceable:

1. **Never raise a cap to unblock a job mid-day.** The cap did its work. The job either waits for midnight UTC or comes out of reserve, consciously.
2. **The `ops` key never gets a human's attention.** If routine automation hits its small daily cap, something is looping. Investigate; do not raise.
3. **The reserve key starts at zero every month** and is raised deliberately, with a named purpose, in week four. A reserve that is permanently available is not a reserve, it is just a bigger budget.

**Why some caps are weekly.** Video and research are bursty — an ad gets made in one afternoon, not in twenty-dollar slices — so a daily cap there just blocks the work on the day it happens. Automation runs every day by definition, so it gets a daily cap and a runaway loop is contained within hours rather than a week.

Two related sequencing points from [[Mega Monetisation Plan]], both already carded: **rotate the OpenRouter key that was pasted into a chat first**, then turn auto top-up off, then buy the first month by hand. Keys live in host secrets and environment variables only — never in the repo, never echoed into a log.

## Model choice per job

The branch's own summary of where money is won or lost, with the caveat that the price card behind it is from 28 July and the model names have almost certainly moved on.

- **Opus 5 — the expensive one, used deliberately.** Article drafting (the writing is the product), architecture decisions, anything touching the publish path, adversarial review passes, and debugging that has already beaten a cheaper model.
- **The workhorse tier (Sonnet-class) — the default** for routine component building, CSS, tests, SEO fields, summaries, social copy, refactors and doc updates. The single most-quoted comparison in the branch: the same three-hour feature build costs about **$10 on Opus and $4 on Sonnet**.
- **A different model for the editor pass.** The playbook requires the editor to be a different model from the drafter, because a model is a poor judge of its own prose. The branch nominates Gemini 3.1 Pro in one file and Kimi K3 in another; the principle matters more than the name.
- **Flash-tier models for reading.** Source verification, long-document triage and the AI inside the free trials. The rule stated plainly: **do not send Opus to read.** Reading, scanning and summarising are cheap-tier jobs; Opus is for judgement.
- **The scanner.** Feed sweeps, dedupe and first-pass filtering ran at about **$0.01 a day** for the whole newsroom — free in all but name, so story discovery should never be a budget conversation.
- **Images: use the quality dial.** Candidates at medium, final at high — roughly $0.03 against $0.13 on `gpt-image-2`. The premium option (Nano Banana Pro, ~$0.134) was mandated by the house image style because it alone renders dense text and logos reliably, but the branch flags this as **untested since the pivot** and asks for one dense-text hero run side by side before the whole pipeline commits.
- **Video: draft cheap, finish expensive.** A 30-second ad built as four 8-second shots — three cheap drafts each, one good final each — came to **$8.00**, against **$76.80** going straight to the top-tier model for takes that mostly get thrown away. With 1,366 approved stills already in the repo, animating a still that already looks right usually beats rolling the dice on text-to-video.
- **Voice.** gTTS was free and in place; a cheap audio model was costed at about **$0.016 an article**, roughly $2.50 a month at five a day, with **~$8 to backfill all 507 existing articles**. The branch itself says test one article and check the billed amount before committing, because the price gap to the full-fat audio model was 27× and a gap that large deserves verification. **Note this has since been overtaken by events** — the voice work moved to Gemini TTS on 5 August and is now parked pending an OpenRouter voice evaluation, so see [[Audio and Voice Production (YFarmX)]] rather than this branch.

**Three habits worth more than any model swap:** let the cache work (long sessions on stable context are ten times cheaper per input token, so restarting a session to keep things tidy throws real money away); do not send Opus to read; draft cheap and finish expensive, which is true for video, images and code alike. And `/clear` between unrelated tasks buys back weekly allowance — one measured session grew its context 13.4× and the last block cost 5.2× the first for exactly the same number of calls.

## Which figures will have moved since 28 July

Stated plainly, because the branch is now over a week stale and prices move faster than that:

- **Every dollar-per-million-tokens figure in the price card**, and therefore every per-article, per-session and per-ad cost derived from it. The branch itself says to re-read the live model list on the 1st of each month and restate the card.
- **Sonnet-class introductory pricing was flagged to end 31 August 2026**, and cheap scanning models to retire around October — so the cheapest lines are the least durable ones.
- **One video model's API was flagged to sunset on 24 September 2026.** Nothing should be built on it.
- **The exchange rate.** $1.332 was the 25 July figure; at $1.28 the pot is $1,416 and at $1.38 it is $1,526. The branch says plan at $1.33 and treat any upside as reserve, never as operating budget.
- **The model names themselves.** They live in config, which is the point, but a cherry-pick that copies the names across without checking them will fail on the first call.

## Scripts the plan depends on

Four small scripts, and no orchestration layer needed for any of it. Claude Code (or a scheduled job) runs a script; the script calls OpenRouter.

| Script | State on the branch | What it does |
|---|---|---|
| `scripts/ask-model.mjs` | **To write — everything else depends on it** | Calls any model by name through OpenRouter's chat endpoint, so trying a new release is a command-line argument rather than a project |
| `scripts/make-image.mjs` | Exists; **needs repointing** at OpenRouter's images endpoint | Hero images, with the house art direction baked into the script where it cannot drift |
| `scripts/make-video.mjs` | **To write** | Submit a video job, poll until it completes, download the result |
| `scripts/make-audio.py` | Exists | Article audio (since superseded — see the voice note above) |

**Each script takes a `--key` argument naming which OpenRouter key to bill**, which is what makes the lane caps hold whether the caller is Claude Code, a cron job or Jay at the keyboard. That one flag is the mechanism the whole guardrail design rests on.

Unattended work stays on GitHub Actions, which the repo already uses for the hourly rebuild and the six-hourly market refresh: a scheduled action calls `ask-model.mjs`, writes a data file, commits, and the site rebuilds. No VPS, no daemon, no orchestrator. The branch is explicit that **the daily X scrape belongs there** rather than in an interactive session or in Hermes.

## The monthly rhythm, and the numbers that catch drift early

Weeks one to three, operate inside the caps and let them do the thinking. Week four, spend the reserve on one written-down purpose — a real voice on every article, hero images for the legacy archive, a bake-off against a new model release, a new tool, or a genuine experiment that might not work. Unspent reserve is not wasted; credits sit in the account and a quiet month funds a bigger experiment the next one.

At month end, five numbers go into the repo's status file: marginal cost per article, cost per finished ad, cost per trial user, credits spent against the $1,052 plan, and **the share of spend on Opus-class models**. That last one is the early-warning light — if it climbs past 40%, the model policy has drifted, and it shows up about a fortnight before the money does.

## What to do with this branch

The material worth carrying forward is the *thinking*, not the tables: auto top-up off as the only real guarantee, per-key lanes as scheduling guards, `--key` on every script, draft-cheap-finish-expensive, per-user caps on trial AI so a thousand signups degrade gracefully instead of detonating, and the measured case that the flat-fee subscription should carry all text and reasoning. The tables need re-costing from live prices before anything is acted on, and the whole budget needs reconciling against the staged £200–400/month recommendation in [[Mega Monetisation Plan]] before the first credits are bought.

*One other file rode along on this branch: `docs/status-archive.md`, 894 lines of status entries from before 28 July. It exists only because the live status file is loaded at the start of every working session and had grown to about 39,000 tokens of opening context — the split was a context-cost saving, nothing was deleted, and it needs no write-up here.*

## Related

[[YFarmX]] · [[Map - In Progress]] · [[Hermes Orchestration Plan (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Mega Monetisation Plan]] · [[Article Pipeline (YFarmX)]] · [[Audio and Voice Production (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Video Production (YFarmX)]] · [[Decisions - YFarmX]] · [[Home]]
