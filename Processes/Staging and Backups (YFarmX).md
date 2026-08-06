---
tags: [process, yfarmx]
source: yfarmx/docs/staging-and-backups.md, yfarmx/docs/status.md, yfarmx/docs/decisions.md
updated: 2026-08-06
---

# Staging and Backups (YFarmX)

Two safety systems on [[YFarmX]], live since 5 August 2026 (decision 38). Jay's two asks: a staging site only he can see, with the push to live checked against what he actually reviewed; and daily backups he can revert to.

## Staging: how a change reaches the live site

1. Claude builds the change and pushes it to the **`staging` branch — never straight to `main`**.
2. The push auto-deploys Jay's private review copy at **staging.yfarmx.pages.dev** (2–3 minutes). It cannot be indexed: noindex header on every page, robots.txt disallow, no sitemap.
3. Jay reviews on phone or Chromebook; fix rounds land the same way.
4. Approval = running **"Promote Staging to Live"** (GitHub Actions, works from the phone app; or Jay says "push it live" and Claude runs it).

**The promote check is the point:** it merges staging into main and *refuses to push* unless every file the merge changes was part of the staging review AND is byte-for-byte identical to the staging copy — "what Jay reviewed is exactly what ships", done with git rather than by eye. Files main grew on its own (market data, social results) pass through untouched; any conflict stops everything unpushed. Afterwards staging is fast-forwarded level with main.

**Articles go through staging too** (Jay overruled the exemption the same day it was proposed): the chat draft in [[Article Pipeline (YFarmX)]] stays step 2, the staging page is his final look, and social waves fire only after promotion because posts point at live URLs.

**Outstanding one-time step:** Jay's Cloudflare dashboard toggle (Pages "Access policy" — email one-time PIN in front of all preview URLs). Until it is on, staging is unindexable but not truly private.

## Backups: three layers, fastest revert first

1. **Live site broke just now:** Cloudflare instant rollback — Deployments → last good one → Rollback. Seconds; reverts the *site*, not the repo, so the bad change must also be fixed in git.
2. **Daily restore points:** an automatic tag on main every day at 03:47 UTC (`backup/YYYY-MM-DD`), kept 90 days. Revert a file, a day, or just look, with ordinary git commands.
3. **Weekly full bundle:** every Sunday a single ~500 MB file holding the entire repository (all branches, tags, history) attached to a GitHub Release, last four kept. Cloning that one file rebuilds the whole site even from a wrecked repository.

**The honest gap, accepted for now:** every layer lives on GitHub or Cloudflare. A true off-site copy (Cloudflare R2 or a drive at home) is a small addition but needs a credential from Jay — offered, not assumed. Worth occasionally downloading the Sunday bundle to a home machine meanwhile.

## Quick reference

| Want to… | Do |
|---|---|
| See work in progress | staging.yfarmx.pages.dev |
| Approve staging → live | Actions → Promote Staging to Live → Run |
| Un-break live right now | Cloudflare → Deployments → Rollback |
| Go back to a date | Daily `backup/` tags (90 days) |
| Recover a wrecked repo | Newest `repo-backup-…` release bundle |

## Links

[[YFarmX]] · [[Map - Processes]] · [[Article Pipeline (YFarmX)]] · [[Decisions - YFarmX]] · [[Claude Operating Profile - YFarmX]]
