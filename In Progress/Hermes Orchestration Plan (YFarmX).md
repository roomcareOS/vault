---
tags: [in-progress, yfarmx]
source: yfarmx@claude/open-router-budget-plan-nu1s99:docs/hermes-orchestration.md
updated: 2026-08-06
---

# Hermes Orchestration Plan (YFarmX)

**Status: unmerged, and partly overtaken by events.** This plan exists only on the branch `claude/open-router-budget-plan-nu1s99` in `roomcareOS/yfarmx`, which is **316 commits behind main** and has never been merged. The open Todoist card ("Resolve the OpenRouter plan branch", p1) asks for a decision: cherry-pick what still holds onto a fresh branch off main, or close it.

**The stale bit, up front, because it is load-bearing.** The plan was written on 28 July 2026 and it says flatly "no Nous Portal" — the whole design routes everything through OpenRouter on one key, and explicitly tells Jay not to buy a Nous Portal subscription. **Hermes is already running on Nous Portal.** So the section arguing against it is describing a decision that has since gone the other way, and anything downstream of "one key, one budget, one set of caps" needs re-checking rather than trusting. The rest of the document — what the orchestrator is for, which model does which job, how profiles map to budget lanes, what stays in scripts — is largely unaffected, but read it knowing the wallet question was settled differently.

**A second thing to know before reading:** the same branch contradicts itself about whether this plan should happen at all. `hermes-orchestration.md` opens with "Decision: Hermes is the orchestration layer, Jay's call, 28 July". The companion `monthly-plan.md`, dated the same day, says **"Hermes is now a side project"** — not the main workflow, given its own small budget lane (~$130/month) and its own job, which is [[RoomCare]] rather than the newsroom. The later view is that Claude Code orchestrates when Jay is at the keyboard, GitHub Actions handles anything on a clock, and Hermes becomes a place to experiment with messaging front-ends and always-on agents without touching the thing that pays. **Two orchestrators is worse than one** is the branch's own closing warning, and it applies to its own two files.

## Naming: these are two different Hermeses

Worth stating plainly because the branch does not. This plan is about **Nous Research's Hermes Agent** — an open-source, self-hosted agent that runs as a daemon (a program that stays running in the background) on a machine of your own; see [[Tool - Hermes Agent (published reference)]]. YFarmX's own automation pipeline is *also* called Hermes ([[Hermes Newsroom Pipeline (YFarmX)]]) and was named independently. The proposal here is to have the first one drive the second.

## What the orchestration layer is meant to do

The pitch is one specific scene, and it is the thing nothing else in the estate can do: **Jay sends a WhatsApp voice note from anywhere; the agent picks up the story, browses the primary sources, drafts to house style, runs the quality gates, commits, and replies on WhatsApp with the preview link.** A newsroom he can run from his pocket.

The plan's central correction is that an earlier assessment had ruled Hermes out for being unable to run the repo's quality gates, and that was simply wrong. It can run shell commands and touch the filesystem, so `npm run build`, the content linter, the SEO verifier and the link checker all work exactly as they do in a Claude Code session — they are just commands. On top of that it has native git and a bundled GitHub skill (branch, commit, push, open a pull request from one prompt), real browsing rather than simple page-fetching, persistent memory that grows across sessions, built-in scheduled jobs, subagents for parallel work, and connections to twenty-odd messaging platforms sharing one memory.

So the quality machinery survives the change of driver. That was the main objection, and it does not stand.

## How it relates to the newsroom pipeline that already exists

It does not replace it — it would *drive* it. The stage list in [[Hermes Newsroom Pipeline (YFarmX)]] stays exactly as written; this plan only decides who runs each stage.

| Stage | Who runs it, under this plan |
|---|---|
| Story discovery, source verification | Hermes' own browser, on a cheap model |
| Draft | The main model — the writing is the product |
| Editor pass | **A different model, deliberately** — the playbook requires the editor never to be the drafter |
| Banned words and content lint | The repo's existing script, via shell |
| Hero image, audio, social queue | The repo's existing scripts, via shell |
| Publish | The same `/api/publish` door Jay's phone form uses, as a draft |
| Review and approve | **Jay, on WhatsApp** — the point of the whole exercise |
| Build gates | `npm run build`, via shell |

**The "via shell" rows are the important ones.** Each of those scripts encodes a rule Jay has already paid to learn the hard way — the X card that needed a JPEG twin, the redirects file that caps out around 101 rules, an overflow bug, some arithmetic that was wrong once. They live in scripts precisely so they cannot be forgotten by a model having an off day. Hermes calling them is the design, not a limitation.

The constraint that does not move: **nothing publishes to the live site unreviewed** until the written quality bar is met, whoever or whatever is orchestrating. That matches where the pipeline actually stands today — the plumbing exists, the day-to-day articles still run human-in-the-loop through the [[Article Pipeline (YFarmX)]], and automation only rises as the quality bar is earned.

## The configuration choice that saves the most money

Hermes runs a capable main model as the driver of the agent loop, and separately runs eleven small auxiliary tasks — compressing long conversations, reading images, extracting web pages, generating titles, routing tool calls, reviewing its own skill usage, triaging work. **Those eleven can each be pointed at a different model, and leaving them on automatic runs all of them on the expensive main model.** The plan's estimate: on a busy day that is the difference between roughly £8 and roughly £25, for about ten minutes of configuration.

The shape of the recommendation, rather than the specific model names, which have moved since 28 July:

- **Main model: capable but not the most expensive thing available.** It drives every tool call, so its price is multiplied by everything. The dearest models are invoked deliberately for specific jobs, never left driving the loop — one of them costs five times the main model's output rate and would have applied that multiple to every step.
- **Highest-volume auxiliary tasks (compression, vision) go on the cheapest capable tier.** These run constantly and are the single biggest lever.
- **Trivial tasks (titles, profile descriptions, routing) go on the cheapest models available** — they are effectively free.
- **Tasks where a bad answer costs something (approvals, work decomposition, spec quality) keep a mid-tier model.** Reliability is worth paying for in the few places a mistake propagates.
- **A fallback chain** of two or three models, so one provider having a bad hour does not stop the newsroom.
- **`data_collection: deny` in the routing settings.** This one matters beyond cost: it keeps the newsroom's sources and unpublished drafts out of providers' training sets.
- **A floor of at least 64,000 tokens of context on any model**, because the system prompt and sixty-plus tool descriptions consume a great deal before the conversation even starts. This rules out some cheap small models that otherwise look attractive.

The plan is honest that Hermes ships tagged releases and moves fast, so the exact configuration keys should be confirmed from the running version rather than hand-copied from the document. Given the version drift since 28 July, that advice now applies with force.

## Profiles are the budget lanes

The neat structural idea, and the one most worth preserving. Hermes profiles each carry their own configuration, skills, scheduled jobs and connections — so **one profile per budget lane, one OpenRouter key per profile**, and every cap in [[OpenRouter Budget and Model Lanes (YFarmX)]] keeps working unchanged: newsroom, site build, products, studio, research, ops. The lane names and cap figures in this file are the *original* lane table, which the companion monthly plan later revised, so take the mapping idea and not the numbers.

The guarantee underneath is the same one as in the budget note, and it is the only cast-iron one: **auto top-up off, credits bought by hand once a month.** Hermes cannot spend money that is not in the account, whatever it decides to do.

**One risk specific to this architecture, which the plan flags and is right to.** An agent with scheduled jobs, subagents and persistent memory can generate spend **while Jay is asleep** — that is genuinely different from an interactive session, where a human is present for every step. The small daily cap on the ops key exists to be boring: if routine automation ever hits it, something is looping, and the answer is to investigate rather than raise it.

## What it would actually take

1. **A machine that is always on**, because Hermes is a daemon. A small VPS at roughly £5–10/month is the only unavoidable new cost; the desktop is where profiles get built and debugged, the VPS is where it lives so scheduled jobs and messaging keep running when the desktop is off.
2. **The repo cloned on that box**, with a deploy key or a fine-grained access token limited to contents read/write on the one repository.
3. **Node and Python** for the existing scripts.
4. **Secrets in the agent's own environment file, never in the repo.** Hermes excludes credentials, memories, sessions, logs and its workspace from anything it shares, so nothing private leaks if a profile is ever passed around.
5. **One messaging platform to start — WhatsApp**, since that is how Jay actually works. Anything else earns its place later.

The order of work the plan proposes is sensible and survives the staleness: install pinned to a tagged release, prove the main model runs, do the auxiliary-model configuration (biggest saving, smallest effort), clone the repo and have it run the build and lint gates and report back, connect WhatsApp and send a voice note, then one article end to end as a draft with the cost logged, and only then split into profiles and per-lane keys once real usage shows the shape.

## What I would carry forward

The durable parts: profiles mapping one-to-one onto budget lanes with a key each; auxiliary models configured rather than left on automatic; the editor model deliberately different from the drafter; the repo's hard-won rules staying in shell scripts where an agent cannot forget them; everything publishing as a draft until the quality bar is signed off; and a pinned release rather than tracking the latest.

The parts needing a fresh decision: the Nous Portal question, now settled the other way; whether Hermes is the orchestrator at all or the side project the companion file makes it; the model names and prices; and whether the newsroom is the right first job for it given the branch's own conclusion that **RoomCare** — a separate business, a separate repo, an independent agent with its own memory and its own number — is the better-isolated place to experiment.

## Related

[[YFarmX]] · [[Map - In Progress]] · [[OpenRouter Budget and Model Lanes (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Tool - Hermes Agent (published reference)]] · [[Mega Monetisation Plan]] · [[Article Pipeline (YFarmX)]] · [[RoomCare]] · [[Decisions - YFarmX]] · [[Home]]
