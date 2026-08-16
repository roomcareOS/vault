---
tags: [decision, yfarmx]
source: Jay, by voice, 16 August 2026, during the John Kamal byline sweep
updated: 2026-08-16
---

# The named credit on a YFarmX article is "Edited by", not "By"

Jay's instruction, 16 August 2026: *"instead of by John Kamal, can you write edited by John Kamal, please?"*

Articles on [[YFarmX]] are machine-assisted desk work. The credit that names a real person is therefore the **editor** line, never the author line. Every article page reads **"Edited by John Kamal"**, linked to `/authors/john-kamal/`. No article says "By John Kamal".

## How it is wired

The site already had both fields, and the article template already suppressed the house author line whenever a named editor was set. So the change was data, not design:

- `src/content.config.ts`: the `author` default flipped from `John Kamal` to `YFarmX`. The `editor` default stays `John Kamal`.
- The 60 articles then carrying `author: "John Kamal"` were flipped to the house byline.
- `src/pages/authors/[slug].astro` now collects **both** credits, so his page still lists his whole catalogue (545 articles) rather than emptying out. It shows the newest 24 with the true total in the count badge.
- Structured data still records the truth either way: `Organization` as author, `Person` as editor.

Set a named `author` only for copy a person actually wrote. This supersedes the 16 July 2026 decision "Byline: Jay's real name on posts"; the E-E-A-T signal now rides on the editor credit and the author page, which is the honest version of the same claim.

## Why it matters beyond the label

Jay's reason, in the same breath: *"because they're under my name. They have to be immaculate. Perfect."* A byline is a quality gate, not a decoration. The same session reviewed all 60 articles against the banned list, house style and their own sources before the credit went on them ([[Verify before you correct published copy]]).

## Related
- [[Article Pipeline (YFarmX)]] · [[YFarmX]]
- [[Verify before you correct published copy]]
- Repo record: `docs/playbook.md` §2, `docs/decisions.md` 56–58
