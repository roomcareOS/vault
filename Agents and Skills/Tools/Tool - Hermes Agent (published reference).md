---
tags: [tool, yfarmx]
source: yfarmx/src/content/reference/ai/agents/hermes-agent.md, yfarmx/src/content/reference/ai/agents/hermes-agent-prompting.md
updated: 2026-08-06
---

# Tool - Hermes Agent (published reference)

**What it is:** Nous Research's open-source, self-hosted agent, built around a closed learning loop — finish a task and it writes itself a reusable skill, with a background "curator" pruning the library so it does not rot. Its argument: the harness (everything wrapped round the model — identity file, bounded memory, skills, rules) decides how much of a model you actually get. **Not to be confused with [[YFarmX]]'s own unrelated [[Hermes Newsroom Pipeline (YFarmX)]]** — same name, different things; the site covers the former, Jay runs the latter.

**Why it matters to Jay:** the two reference pages are among the site's deepest, and the ideas transfer directly to his own agent setups: memory holds *what is true*, skills hold *how it is done*; sycophancy ("you're absolutely right") is reward hacking, broken by a fresh critic agent with no conversation history; write the brief (goal, location, evidence, constraint) rather than the request, then let the agent choose the route.

**Published references:** `yfarmx/src/content/reference/ai/agents/hermes-agent.md` (the agent, the lab, the harness argument) and `yfarmx/src/content/reference/ai/agents/hermes-agent-prompting.md` (the practical prompting and command guide).

[[Map - Agents and Skills]] · [[Tool - OpenClaw]] · [[Tool - Claude Code]]
