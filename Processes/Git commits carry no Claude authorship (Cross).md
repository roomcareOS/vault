---
tags: [process, cross, git]
source: Jay's instruction, 17 August 2026
updated: 2026-08-17
---

# Git commits carry no Claude authorship (Cross)

**The rule: no commit or tag in any roomcareOS repo names Claude as author or
co-author. This binds every Claude session in every repo.**

Jay, 17 August 2026: remove Claude as co-author or author on all git commits
across all repos.

## What sessions must do

- Commit as `roomcareOS <301913580+roomcareOS@users.noreply.github.com>`.
- Never add a `Co-Authored-By: Claude` trailer, a `Claude-Session` link or a
  "Generated with Claude Code" line, in commits or in PR bodies.
- Each repo's `.claude/settings.json` carries
  `"attribution": {"commit": "", "pr": "", "sessionUrl": false}`, which stops
  Claude Code adding them. Leave it in place; add it to any new repo at
  creation.

## What was done on 17 August 2026

Every repo's full history was rewritten with git-filter-repo: Claude
author/committer identities became roomcareOS, the trailers were removed, and
every file tree was verified byte-identical before force-pushing, so builds,
backup tags and deploys were unaffected. `vault`, `myhomework` and `v1` landed
in-session; the `yfarmx` rewrite was verified and staged, with the force-push
waiting on Jay's go-ahead at the permission gate; `interviewprep` and
`scenecast` still need the same treatment. Branch NAMES like `claude/...` were
left alone: they are refs, not authorship.

## The trap this prevents

The attribution is added by the tool by default, so a repo without the
settings file drifts back within a session or two. The history rewrite is the
expensive fix; the settings file is the cheap guard. Check it exists before
the first commit in an unfamiliar repo.
