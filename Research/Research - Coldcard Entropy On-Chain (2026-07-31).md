---
tags: [research, yfarmx]
source: yfarmx/docs/research/2026-07-31-coldcard-entropy-onchain.md
updated: 2026-08-06
---

# Research - Coldcard Entropy On-Chain (2026-07-31)

**What was researched:** a Lookonchain screenshot reporting ~$38m swept from Coldcard hardware wallets. Rebuilt entirely from Coinkite's own two advisories and a direct read of Bitcoin mainnet (via mempool.space) — including recovering the full sweep address the tip-off had truncated.

## Key findings

- **The bug:** during a 2021 code migration, seed generation (creating the wallet's secret key) silently fell back to a weak software random-number generator instead of the hardware chip, because a build check tested whether a setting existed rather than whether it was switched on. Result: **Mk3 seeds have ~40 bits of effective randomness** (about 1.1 trillion candidates — searchable by any competent attacker); **Mk4/Mk5/Q seeds ~72 bits** (billions of times harder, not brute-forceable today). That single difference is why Mk3 wallets were emptied and the others were not.
- **The sweep, read off the chain:** 594.477 BTC (~$38.0–38.2m at the time) from **501 outputs in 15 minutes 18 seconds** on Thursday 30 July, then 562 BTC moved in one 341-input transaction to a second address, **still unspent**. Median victim ~0.41 BTC; largest ~29.9 BTC (~$1.9m).
- **What the chain showed that the coverage got wrong:** four blocks not three; "consolidated into a single address" inverts the flow (562 BTC *left* the consolidating address); Thursday not Friday; and Coinkite's position **widened inside 24 hours** from "Mk3 only" to the entire current range.
- **The link between bug and sweep is correlated, not confirmed** — the piece says so plainly.

## What Jay should remember

- The playbook's never-cite-a-rival-outlet rule **cut the strongest line** (Coinkite's earlier "Mk4/Q/Mk5 not affected" quote, carried only by The Block, unretrievable elsewhere). Find the primary or drop the claim — the rule costs stories, and that cost is the product's credibility. The rule is doctrine in the [[Article Pipeline (YFarmX)]].
- Dated amendments matter: the advisory's 31 July update stamp is what proved the scope widened.
- Same discipline as [[Research - Ostium Exploit On-Chain (2026-07-30)]]: when the claim is on-chain, read the chain, and publish what remains unverified as open questions.

[[YFarmX]] · [[Map - Research]]
