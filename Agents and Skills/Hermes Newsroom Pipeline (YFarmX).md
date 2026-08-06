---
tags: [agent, yfarmx]
source: yfarmx/CLAUDE.md, yfarmx/docs/master-plan.md, yfarmx/docs/playbook.md, yfarmx/docs/publishing-plan.md
updated: 2026-08-06
---

# Hermes Newsroom Pipeline (YFarmX)

Hermes is [[YFarmX]]'s automation pipeline: the machine that is meant to carry the newsroom to 90–95% automation at roughly five articles a day. Designed pre-pivot for WordPress, carried through the pivot with one change: **the publish step now writes Markdown files and commits them to git** instead of calling a CMS API.

## The pipeline, stage by stage (each stage logged with its cost)

1. **Discovery** — scan RSS feeds, official blogs, arXiv (the research-paper archive) and security disclosures for stories. Cheap model.
2. **Verification** — minimum two independent reputable sources per claim, or the claim is labelled or dropped. Primary sources only; a tip-off (screenshot, social post, rival outlet) is a pointer, never a citation.
3. **Draft** — strong writing model, house style: clear English, jargon explained on first use, no hype.
4. **Editor pass** — a *different* model re-reads: every claim traced to a source, clarity, en-GB, internal links to explainer pages checked in.
5. **Image** — hero and infographic in the house art direction ([[Image Style and Prompt Libraries (YFarmX)]]).
6. **SEO fields** — title, meta description, internal links to related pages and glossary terms.
7. **Publish = write the file and commit.** Hermes POSTs to the same `/api/publish` door as Jay's phone form, with `draft: true` and `via: "hermes"`, so the piece waits.
8. **Review queue** — Jay approves from his phone; approval flips the draft flag and the site deploys itself.
9. **Social syndication** — platform-appropriate posts to X and LinkedIn ([[Social Syndication (YFarmX)]]).

## Money and safety rails

- **Cost logging per article** from day one; daily spend summary per workstream; monthly rollup.
- **Hard cap of £15/day on the newsroom key only** (resets midnight UTC), so automation can never run away silently. Interactive work is never hard-blocked.
- Cheap models for scanning and classification; strong models for writing and editing. Model names live in config, swappable in minutes.

## Quality gates that never relax

No invented quotes, no unsourced claims, sources listed on every article, public corrections policy, AI use disclosed. The per-article disclosure and the human-review record are injected automatically so they can never be forgotten. Start fully human-reviewed; automation rises towards 90%+ only after a written quality bar is met consistently (and that quality-bar document goes to Jay for sign-off first).

## Standing Hermes jobs beyond articles

- Appending verified entries to the tracker datasets: `exploits.json` (Crypto Exploit Tracker), `ai-incidents.json` (AI Risk Radar), `quantum-threats.json` (Quantum Threat Tracker)
- Refreshing `events.json` with organiser-verified event listings

## Where it stands

The plumbing exists (the publish door, review-queue mechanics, social queues, build gates); the day-to-day articles are still produced through the human-in-the-loop [[Article Pipeline (YFarmX)]], which is deliberate: everything human-reviewed until the quality bar is earned. The name also gave its title to the site's reference page on Nous Research's unrelated "Hermes Agent" product — see [[Tool - Hermes Agent (published reference)]]; the two are different things.

## Links

[[YFarmX]] · [[Map - Agents and Skills]] · [[Article Pipeline (YFarmX)]] · [[Claude Operating Profile - YFarmX]] · [[Social Syndication (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]]
