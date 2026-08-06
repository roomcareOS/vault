---
tags: [in-progress, yfarmx]
source: yfarmx@claude/yfarmx-arcade-game-ideas-f2l6wk:docs/arcade-games.md
updated: 2026-08-06
---

# Arcade Game Ideas (YFarmX)

**Lives on branch `claude/yfarmx-arcade-game-ideas-f2l6wk`, not merged. Nothing is live.**

A branch is a parallel copy of the code that nobody visiting yfarmx.com can see. This one holds six commits across 42 files (7,466 lines added): a menu of fifteen costed game proposals for the [[YFarmX]] arcade, plus one of them actually built. The site only deploys when work lands on the main line, so none of it is on the web and none of it can break anything that is. Conflict risk against main is low today and creeps up with every article commit, so this is a decision that gets slightly more expensive to postpone.

The arcade already has four cabinets live (Crypto Quiz, Memory Match, Whole Bitcoiner, Neon Snake, plus a hidden one on the 404 page). Everything below is designed to sit inside the same cabinet frame, so the arcade stays one room rather than fifteen separate toys.

## The fifteen proposals

Build cost is the doc's own honest estimate, measured in *sessions* — one session being one working chat with an agent. **S** = 1 to 2 sessions. **M** = 3 to 5. **L** = 6 to 12. **XL** = 15 or more, effectively a project of its own.

| Game | The idea in one line | Area | Build |
| --- | --- | --- | --- |
| **Jailbreak** | Talk a guarded assistant into giving up the secret it was told to keep. | AI | M — **built** |
| **Next Token** | Four ways a sentence could continue; pick the one the model actually predicted. | AI | S |
| **Neural Forge** | Build a neural network by hand in 3D and watch it genuinely learn. | AI | L |
| **Ground Truth** | Spot the fake: the invented citation, the generated image, the claim its source does not support. | AI | M |
| **Cluster** | Run an AI lab and discover why it costs what it costs. Electricity bankrupts you first. | AI | L–XL |
| **Bloch** | A real quantum circuit editor disguised as forty puzzle levels. Par is gate count, like golf. | Quantum | L |
| **Q-Day** | Twenty turns to move a bank off breakable encryption before quantum computers arrive. | Quantum | M |
| **Surface Code** | Keep one qubit alive by guessing where errors ran from where they ended. | Quantum | M–L |
| **Spooky** | Play the CHSH game, hit the classical 75% wall, then break it with entanglement. | Quantum | S–M |
| **Foundry** | Build a quantum computer from the cryostat up. The honest "one day" project. | Quantum | XL |
| **Mempool** | Ninety seconds to pack a block with the most valuable transactions. Tetris with fees. | Crypto | M |
| **The Heist** | On-chain detective work; every level is a real robbery from our own Attack Log. | Crypto | L, then S per level |
| **Hard Fork** | You run the chain for thirty turns. Upset a faction badly enough and it splits. | Crypto | L |
| **Cold Storage** | An escape room where the thing you are protecting is a seed phrase. | Crypto | M |
| **Difficulty** | Seventeen years of mining as an idle game, on the real difficulty and halving curve. | Crypto | M |

Eight of the fifteen run on data the site already owns and Hermes already maintains, which is why they stay cheap to keep alive — a new entry in the Crypto Attack Log is most of a new Heist level for free.

**Where the doc says to start:** Spooky, Mempool, Next Token. Between them they build the shared plumbing (scoring, pause, thumb controls, the daily-puzzle loop) while each staying small enough to finish. Then the two flagships, Bloch and The Heist. Two proposals carry a caveat: Difficulty needs Jay's call on whether historical prices go in the scoring (recommendation: keep the score in coins mined, price as end-of-run context only, and a solicitor's view before anything livelier), and a trading-prediction game was considered and parked on the same grounds.

## Jailbreak, the one that exists

A fifth cabinet, already built on the branch. You face eight AI systems, each holding a passphrase and each told to protect it, and you have to talk it out of them. Rather than typing free text you assemble your prompt from tactic cards — three slots, fifteen cards — and the assembled attack writes itself out in full as you build it, so composing one reads like writing one. A suspicion meter ends the session if you fill it. There is a DEFEND mode too, where you build the defensive instructions and a fixed battery of attacks runs at them; you lose points for refusing reasonable requests, because a system that refuses everything has won nothing. The eight systems escalate from one with no defences at all to one with every defence correctly configured, which loses anyway.

**State of the build:** only the first mission (SENTRY) has the full narrative arc. The other seven currently run a simpler loop and need converting.

### The deterministic referee, and why it is the important decision

There is no AI model running inside this game. The guard is a **deterministic rules engine** — a fixed set of written rules and pattern checks that always produce the same answer for the same input, rather than a model deciding in the moment. Everything lives in the player's own browser: no accounts, no server, no stored data. That buys four things, and they are the reason to keep it this way:

- **It costs nothing to run.** No metered API sits behind a public free-play button, so there is no bill to run away with and nothing worth attacking.
- **It behaves identically for every player.** Same move, same response, every time.
- **It works offline**, including in the installed app.
- **Par scores mean something.** A leaderboard is only fair if the opponent is the same opponent for everyone — which a live model would not be.

### The editorial line

The game teaches the *shapes* of prompt injection — the categories of trick used to talk a machine out of its own instructions — and never a working attack. Every system, every passphrase and every defence in it is invented. No level ships text you could paste at a real assistant today and have it work. This was built in as a rule rather than chosen afterwards, so it functions as a veto on future levels, not a preference.

## What Jay has to decide

**The branch itself: promote or close.** Either it goes through staging (the private copy of the site, reviewed before anything reaches readers) on its way to live, or it gets closed and the work is archived. Leaving it parked is the only option that gets worse with time, because every article committed to main makes the eventual merge slightly harder.

Two open questions are sitting in the Inbox:

1. **Global leaderboards — yes or no?** Recommendation: **no for now.** Scores shared between players mean a database, a line in the privacy policy, and breaking the arcade's current promise that nothing leaves your device. Every game works today on-device only, and daily puzzles with a shared seed give most of the competitive pull without any of that.
2. **A live-model mode for Jailbreak?** Recommendation: **not yet.** It would trade away all four benefits above for novelty. Worth revisiting only if the arcade ever gains accounts and limits.

The four follow-on jobs (reviewing the branch, converting the seven remaining systems, a 3D attack-path cutaway, and a modular kit for building the arcade artwork) are tracked as Todoist cards — Todoist owns the state, this note owns the reasoning. See [[Todoist Doctrine]].

## Related

[[YFarmX]] · [[Map - In Progress]] · [[Home]] · [[Decisions - YFarmX]] · [[Staging and Backups (YFarmX)]] · [[Todoist Doctrine]]
