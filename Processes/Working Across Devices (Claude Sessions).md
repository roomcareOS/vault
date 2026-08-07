---
tags: [process, cross]
source: Jay's question + Claude Code docs check, 7 August 2026
updated: 2026-08-07
---

# Working Across Devices (Claude Sessions)

**The rule in one line: cloud sessions by default; local only when the work needs Jay's PC, and then with Remote Control switched on so the phone can still see it.**

## Why chats appear "lost" between phone and PC

There is no bug. Claude Code keeps two entirely separate kinds of session:

- **Cloud sessions** — started from claude.ai/code in a browser or the Code tab of the phone app. They run on Anthropic's machines and are visible from *every* signed-in device: phone, any browser, the desktop app.
- **Local sessions** — started from the CLI or desktop app on a specific machine. The transcript lives only on that machine (`~/.claude/projects/`). The phone cannot see them, and the PC cannot see a different PC's.

Mobile chats invisible on desktop (and vice versa) simply means one side was cloud and the other local. They never sync, and nothing will make old ones sync retroactively.

## The standing setup

1. **Default to cloud.** Start sessions at claude.ai/code (or the phone app's Code tab). They follow Jay across phone, Chromebook, PC and the desktop app with no setup.
2. **Local when needed** — installing apps, testing on real hardware, anything that must touch the PC itself. On the PC, run `/config` inside Claude Code once and turn on **Enable Remote Control for all sessions**. From then on every local session broadcasts to claude.ai/code, so the phone can watch and steer it. Caveat: the PC must stay awake; the session dies with the machine.
3. **Dispatch is a router, not a bridge.** It takes a task typed on the phone and hands it to a Code session on the desktop (or keeps it in Cowork). Useful for "fix X while I'm out" — but it does not merge local and cloud histories, so it is not the answer to the visibility problem.

## Why losing a chat does not lose work

Continuity never lives in the transcript. It lives in the three-way split from [[Session Doctrine]]:

- **Todoist owns STATE** — any session, on any surface, reads the board and knows where things stand.
- **The repo owns the RECORD** — work is committed and pushed, so any machine pulls the same truth.
- **This vault owns KNOWLEDGE** — pushed to `main`, pulled automatically by the PC.

A session that ends abruptly on one device is resumed on another by starting a *new* session, which reads the board and the repo and carries on. That is the design, not a workaround.

## How every surface picks up context

- **Claude Code (cloud or local), any repo:** the repo's `CLAUDE.md` is read automatically at session start, and every `CLAUDE.md` in the estate points at [[Session Doctrine]], the vault and the Todoist board. Nothing needs pasting; starting a session in the repo *is* the setup. (The Intervooh `CLAUDE.md` is still on an unmerged branch — see the Session Doctrine table.)
- **Plain claude.ai chats (no repo):** no `CLAUDE.md` exists there, so use a claude.ai Project with instructions that mirror the doctrine, plus the Todoist and GitHub connectors.
- **Hermes (desktop, WhatsApp or Discord):** carries the doctrine in its own system prompt and persistent memory — see [[Hermes Orchestration Plan (YFarmX)]]. Same contract: read the board first, write state back to the board, knowledge to the vault.

## Related

- [[Session Doctrine]] — the three-way split this relies on
- [[Todoist Doctrine]] — the board itself
- [[Hermes Orchestration Plan (YFarmX)]] — the messaging front-ends
- [[Home]] · [[Map - Processes]]
