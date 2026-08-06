---
tags: [research, yfarmx]
source: yfarmx/docs/research/2026-08-01-openai-astra-ten-proofs.md
updated: 2026-08-06
---

# Research - OpenAI Astra Ten Proofs (2026-08-01)

**What was researched:** OpenAI's same-day announcement of ten results on long-open mathematics problems, produced by an internal version of its next major model, "Astra". The tip-off was a Grok summary of a Polymarket post — not a source. Rebuilt from the announcement (via the Internet Archive, since openai.com blocks the sandbox), the full 249-page paper, the Lean proof repository, and the databases that track the problems.

## Key findings

- **The claims:** ten results on problems open at least a decade (one 26 years, one unimproved since 1978 — the sphere-packing exponent). Token cost of *finding* the solutions: roughly $2,000 at Sol API rates — excluding a week of agent wall time and the human manuscript work. Each proof formalised in Lean (a computer program that mechanically checks proofs): zero gaps, standard axioms only.
- **The caveat the announcement does not carry:** the repository's own metadata records the formalisation as **"agent-reviewed"** — checked by agents, not independently by humans.
- **The best single detail:** a human mathematician independently and concurrently reached one of the same counterexamples, assisted by GPT-5.6 Sol.
- **The honest state of verification on day one:** all three Erdős problems OpenAI says it resolved were **still listed OPEN** in the community database, no solutions claimed. Not evidence against the results — the most useful thing in the piece.
- **The credit fight:** OpenAI credits the model as the author of the arguments; the IMU-endorsed Leiden Declaration (3,348 signatories including Tao and Scholze) says credit and responsibility belong to humans.
- **Claims that did not survive checking:** "Astra" is never named in the paper itself; supposed companion remarks from four named mathematicians were **search-engine synthesis of rival outlets** — fabricated attribution, dropped entirely.

## What Jay should remember

- The dropped-quotes finding is the standing lesson: **aggregated summaries invent quotations from real, named people**. The [[Article Pipeline (YFarmX)]] rule — every figure traces to a primary URL — is what caught it.
- The day-one verification gap *was* the story; the newsroom's edge is reporting the state of the evidence, not the press release.
- Method notes: Wayback must be fetched over https; use `raw.githubusercontent.com` when the GitHub API is scoped; mangled PDF extraction lies to searches — strip whitespace and reconstruct quotes by hand.

[[YFarmX]] · [[Map - Research]]
