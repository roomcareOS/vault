---
tags: [map, cross]
updated: 2026-08-07
---

# Map - In Progress

Work that **exists but has not landed**. Every note here distils something built on an unmerged git branch — real work, sometimes finished and tested, that nobody visiting the live sites can see and that no session reading the main branch will find.

This section exists because that work kept going missing. The vault was first built from the main branch only, and a whole layer of Jay's thinking — the 60-video plan, the podcast, the film scripts, the AI budget — turned out to be invisible. These notes make it visible and say plainly what is blocking each one.

**The rule for this folder:** when a branch merges or is closed, its note either moves to [[Map - Processes]] (it is now how things are done) or is deleted (the work was abandoned). Nothing should sit here forever. Live task state belongs in Todoist, not here — see [[Todoist Doctrine]].

## YFarmX

- [[TikTok 60-Video Plan (YFarmX)]] — 30 days, two posts a day, six series, a full brief per video. Blocked on Jay approving the plan, approving the voice, and the empty Google AI Studio credits.
  *Branch: `claude/wifi-max-tiktok-plan-wzkahd`*
- [[Media Storage and the R2 Rule (YFarmX)]] — **read this before the Seedance video week (7–13 Aug)**: media never goes into git, one private bucket per property, index as you go.
  *Branch: `claude/workflow-architecture-plan-ptk232`*
- [[OpenRouter Budget and Model Lanes (YFarmX)]] — what the £1,400/month is meant to buy, the per-key lane caps, and why only auto-top-up-off is a real spending guard. Parts written against 28 July state and now stale.
  *Branch: `claude/open-router-budget-plan-nu1s99`*
- [[Hermes Orchestration Plan (YFarmX)]] — the orchestration layer's intended shape, and how it meets the newsroom pipeline that is already live.
  *Branch: `claude/open-router-budget-plan-nu1s99`*
- [[Arcade Game Ideas (YFarmX)]] — fifteen costed game proposals plus the built Jailbreak cabinet. Decision due: promote through staging, or close the branch.
  *Branch: `claude/yfarmx-arcade-game-ideas-f2l6wk`*

## RoomCare

- [[Films (RoomCare)]] — the five vertical films. One and two finished and waiting to be posted; three rendered but silent with a rewrite awaiting approval; four and five effectively unbuilt. **There is no `marketing/` folder on the main branch at all.**
  *Branch: `claude/roomcare-motion-video-ad-ap7267`*

## Cross-estate

- [[Vault and Workflow Design (YFarmX)]] — the design of this vault (Decision 42) and how agents are meant to work across the five businesses. The same branch carries the 22-file vault seed that is this vault's intended long-term home.
  *Branch: `claude/workflow-architecture-plan-ptk232`*

## The branches, at a glance

| Branch | Repo | Holds | Decision |
|---|---|---|---|
| `claude/wifi-max-tiktok-plan-wzkahd` | yfarmx | the 60-video plan | approve, then render |
| `claude/podcast-setup-plan-0byrij` | yfarmx | the whole podcast build | merge (safe — the flag keeps it private) |
| `claude/workflow-architecture-plan-ptk232` | yfarmx | vault seed, media rule, workflow plan | merge or move the seed to the vault repo |
| `claude/open-router-budget-plan-nu1s99` | yfarmx | budget and orchestration plans | cherry-pick onto a fresh branch, or close — 316 commits behind |
| `claude/yfarmx-arcade-game-ideas-f2l6wk` | yfarmx | arcade proposals + Jailbreak | promote through staging, or close |
| `claude/roomcare-motion-video-ad-ap7267` | v1 | all five RoomCare films | merge — main has none of it |

Each of these has a Todoist card. Todoist owns the state; this map owns the picture.

## Related
- [[Home]] · [[Map - Processes]] · [[Map - Businesses]] · [[Map - Decisions]]
- [[YFarmX]] · [[RoomCare]]
