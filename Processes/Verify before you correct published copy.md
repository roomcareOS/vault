---
tags: [process, yfarmx, cross]
source: The John Kamal byline sweep, 16 August 2026 (43-agent review of 60 published articles)
updated: 2026-08-16
---

# Verify before you correct published copy

A fact-checking agent that says an article is wrong is **evidence, not a verdict**. On 16 August 2026 a fleet of agents re-checked the 60 articles then bylined John Kamal against their own sources. Sixteen findings were serious enough to claim a published factual error. Each was handed to a second agent told to *refute* it. **Five of the sixteen did not survive** — the article was right and the finding was wrong.

That is a 31% false-positive rate on the findings that would have changed published copy. Editing straight from a checker's output would have introduced five new errors into articles carrying Jay's name.

## The failure mode, from the clearest example

A checker reported that an article overstated Revolut's customers: the annual report says 68.3 million, the article said 70 million, attributed to that report. It quoted the pages and stated *"no figure of 70 million appears anywhere in the 208-page document."*

The verifier downloaded the same PDF and searched the whole text. Page 10, the CEO's letter: *"now exceeding 70 million customers."* The same page also carries the 13 million UK figure the article used. The checker had read the audited-figures section and stopped.

The pattern: **a sampled read of a long source produces a confident absence claim.** "X does not appear" is the single least reliable finding a checker can return, and it is the one that reads most convincingly.

## The rule

1. **Two independent passes before any published sentence changes.** The second is told to refute the first, and to check the primary source itself rather than the first agent's account of it.
2. **Absence claims get the whole document.** Full text, searched, not the section that looked relevant.
3. **A finding whose supporting evidence is itself wrong is dead**, however plausible the conclusion.
4. **Unverifiable is not wrong.** Datacentre IPs get 403s, X links do not fetch, PDFs defeat HTML converters. Log those; never edit on them. The 16 August sweep left 99 such items in `docs/john-kamal-sweep-followups.md` with "verify before edit" at the top, rather than acting on them.
5. **How the correction itself is presented depends on launch state (Jay, 16 August 2026).** *Pre-launch, now:* fix the text, keep the original publication date, add nothing else — no dated note, no `updated:` stamp. His words: *"just put the correct version and the original posting date, and that's it ... we're not fully launched yet."* *After launch:* the `/corrections/` page's promise binds — fix the text, stamp `updated:`, and append a dated italic note in its taxonomy (Correction / Clarification / Retraction). The switch happens the moment Jay says we are live.
6. **A later development is not an error.** Judge the copy against what was knowable at its publication date. A company statement that lands after publication earns an update note, not a correction.
7. **Text only, unless told otherwise.** Jay, 16 August: *"only fix text not audio."* A corrected article keeps its original MP3 and podcast episode; re-cutting is a separate, explicit decision.

## Machine-applying fixes: what went wrong

46 style fixes were applied programmatically, by string replacement, where the proposed fix was literal replacement text. Roughly a dozen came out mangled: instruction prose written into the article body (*"End the sentence at..."*, *"Attach the caveat where the claim is made at line 30"*), and sentences duplicated where the fix restated the quote it replaced.

Every one was caught by reading the complete diff line by line before committing. **If you batch-apply agent output, read the entire diff afterwards. Not a sample.** Two cheap detectors worth keeping: grep the corpus for instruction verbs (`rewrite as`, `End the sentence`, `at line \d`, `[outlet]`), and scan for a repeated 8-word phrase within a 60-word window.

## The gate that stops this reaching published copy at all

Corrections are the expensive path. From 16 August 2026 every article is attacked **before** it reaches Jay, which is the cheap one:

- **`scripts/preflight-article.mjs`** — the mechanical pass. Fetches every source and body URL; fails on a dead link, a quoted passage found verbatim in none of them, a banned outlet, fewer than two sources, a banned word, an over-long In brief. Warns on vague attribution, narrating tics, retired caveat headings, US spellings and dates, Title Case headlines. Blocked fetches (403, timeout) report as **unverifiable**, never as errors. Seconds, no tokens.
- **The `red-team` agent** (`.claude/agents/red-team.md`) — the judgement pass. A fresh agent with no attachment to the draft, told to find what is wrong: altered quotes first, then figures against their sources, then unsourced claims. It is instructed to refute its own absence claims before reporting them, which is exactly the failure above.

Then the rule at the top of this note applies to whatever they find.

## Related
- [[The named credit on a YFarmX article is Edited by, not By]]
- [[Article Pipeline (YFarmX)]] · [[YFarmX]]
- Repo: `docs/playbook.md` §1.4 (the baseline is for migrated copy only), `docs/john-kamal-sweep-followups.md`
