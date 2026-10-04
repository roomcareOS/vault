---
tags: [skill, yfarmx, research]
source: Jay's standing tracing instruction and supplied Zcash discussion, 4 October 2026
updated: 2026-10-04
---

# Skill - Fund tracing (YFarmX)

Every request to trace blockchain funds in [[YFarmX]] loads the repo's
canonical `.agents/skills/trace-funds/SKILL.md`. AGENTS.md section 9,
CLAUDE.md rule 16 and playbook section 8 carry the instruction for both
Codex and Claude. The method and its detailed reference live in the repo;
this note links to that maintained source.

[Read the skill on staging](https://github.com/roomcareOS/yfarmx/blob/staging/.agents/skills/trace-funds/SKILL.md).
Its linked reference preserves the Zcash exit census, service-order lookup,
bridge-message decoding, integer amount maths, candidate multiplicity,
controlled behavioural fingerprints and subsequent-spend checks. The earlier
private tracing manual remains a protocol reference, read under the skill.

The enduring lesson is to grade the route separately from the actor. A
service record can prove a payout while timing, amounts or a wallet default
still provide only a candidate incident link. Preserve ambiguous searches
and rejected matches, and identify the exact corroborating record needed.
The old case counts are dated observations to recheck. Raw working stays
private under the repo's OPSEC rules.

Related: [[Map - Agents and Skills]] · [[YFarmX]] · [[Session Doctrine]]
