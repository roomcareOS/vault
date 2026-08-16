---
tags: [process, cross]
source: Jay's X bookmarks export, 15–16 August 2026; supersedes the Inbox how-to of 6 August
updated: 2026-08-16
---

# Bookmark Triage (Cross)

Turning a pile of saved links — X bookmarks or browser bookmarks — into something that can be acted on. Saving is not reading: the pile is only worth having if a pass over it produces a short list of things to do.

This note replaces `Inbox/How to import X bookmarks.md`, which recommended the official X data archive or copying links out by hand. Both were wrong in practice. **X still has no export and no date filter**, and the archive route was never actually used.

## Getting the export

**X:** it needs a browser extension. As of 16 August 2026 the working pick is **X Bookmark Manager & Exporter (Twitter)** on the Chrome Web Store — the only one still advertising filters by date, handle, tag or text. Fallbacks: *X Bookmarks Exporter* (CSV/JSON/XLSX, no date filter) and *Twitter Bookmark Exporter* (JSON).

The extension named in the 6 August Todoist card, *X & Twitter Bookmarks Exporter*, **had vanished from the store by 15 August**. Expect this to keep happening, and re-check before recommending one.

Two traps:

1. **Scroll the bookmarks list to the bottom before exporting.** These tools read what the page has loaded, not X's servers. A short export usually means a short scroll, not a small pile.
2. **Check permissions before installing** — the extension reads the live X session. Prefer a visible download count and a linked repository, and remove it once the export is done.

**Browsers:** Chrome/Edge/Brave `chrome://bookmarks` → three dots → Export bookmarks. Firefox: Ctrl+Shift+O → Import and Backup → Export Bookmarks to HTML.

## Filtering it

Two sibling scripts in `roomcareOS/yfarmx`, sharing one keyword list (`scripts/lib/bookmark_keywords.py`) so a term added for one applies to both:

```
python3 scripts/filter-x-bookmarks.py <export.json>   --days=90 --out=leads.md
python3 scripts/filter-bookmarks.py   <bookmarks.html> --days=40 --out=leads.md
```

Both take `--days`, `--all` (skip the keyword filter), `--keywords=a,b,c` and `--out`. Both emit dated Markdown grouped by day. The X one deduplicates on post ID, so the `twitter.com` and `x.com` forms of one post do not both land.

**Resolve the shortlinks.** Roughly two-thirds of the useful destinations hide behind `t.co`. A `curl -L` sweep over the unique shortlinks turns them into real URLs — on the August run, 112 shortlinks yielded 38 external destinations, mostly GitHub repositories and product pages. Without that step the tools in the pile are invisible.

## Sorting it

**Sort by what Jay would do with the item, not by topic.** Topic sorting just reproduces the pile in a new order. The categories that worked in August 2026:

- Install and try — anything with a real repository or product page behind it
- MCP servers and integrations
- Agent CLIs and workflow
- Free compute and credits
- Business models and money
- Prompting and craft
- Watch list — releases that change what the stack can do
- Security and housekeeping

**Mark the unverifiable.** A large share of this material is engagement bait: claimed earnings, invented star counts, "$300 an hour" with no mechanism. Flag those rather than dropping them silently, so Jay can see they were read and judged rather than missed. Same standard as the newsroom's no-unsourced-claims rule.

**Note what expires.** Free tiers, contest deadlines and introductory pricing all decay. The August run surfaced exactly one hard deadline and several offers that had already closed.

## Where the raw exports live

**Raw exports go in the vault, never in a publishing repo.** `roomcareOS/yfarmx` is private, so this is not about exposure — it is that a saved-links history is personal knowledge, not site content, and that repo publishes from `public/` and its content collections. `data/x-bookmarks-*.json` and `bookmarks*.html` are gitignored there so the rule holds by default.

The archive is kept as a Markdown note rather than the raw JSON: this vault is markdown-only, and Obsidian search over a note is worth far more than a blob nobody opens. See [[Research - X Bookmarks Archive (2026-08-15)]] — 744 posts back to February 2023, with the 140 security and OSINT entries marked `[sec]` at Jay's request as a rainy-day knowledge base.

## What it produced, August 2026

744 bookmarks exported; 169 in the last 90 days; 101 sorted into an artifact for Jay; 68 set aside as news already in the newsroom digest, or politics and memes. Best finds: an OpenRouter MCP that prices and picks models live (relevant to the £1,400 ceiling), a local voice-cloning repository relevant to [[Audio and Voice Production (YFarmX)]], NVIDIA's free multi-model API key, and ASD-STE100 Simplified Technical English as a testable house-style rule for [[Article Pipeline (YFarmX)]].

## Links
- [[Home]] · [[Map - Processes]]
- [[Research - X Bookmarks Archive (2026-08-15)]]
- [[Article Pipeline (YFarmX)]] — where story-shaped leads end up
- [[Session Doctrine]]
