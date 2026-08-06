---
tags: [doctrine, cross]
source: Jay's instruction, 6 August 2026
updated: 2026-08-06
---

# Session Doctrine

**The rule, in one line: no piece of work is finished until Todoist and this vault both say so.**

Jay set this on 6 August 2026 after the vault fell behind the work twice in one day. It applies to every Claude session in every repo, whether or not anyone repeats it.

## The three-way split (unchanged, and the reason this works)

- **Todoist owns STATE** — what is happening right now, what is blocked, what is next.
- **The repo docs own the RECORD** — what shipped, in the code's own language, next to the code.
- **This vault owns KNOWLEDGE** — how things are done and why, across all five businesses.

A thing that changes state goes on the board. A thing that changes how work is done comes here. Most real work does both.

## What every session must do

**At the start:**
1. Read the Todoist board for this repo's project. Somebody may already own the task.
2. Check this vault for the process note covering what you are about to touch. Follow it, or say why you are departing from it.

**During:**
3. Move the Todoist card when the state changes, not in a batch at the end.
4. Create a card for anything discovered and not done.

**Before saying anything is finished:**
5. **Todoist:** close what is genuinely finished. Leave blocked things open with the reason. Anything needing Jay goes to `Inbox` with enough context to act on without scrolling back — the exact screen, the exact value, the reason. Never a secret value, only its name.
6. **Vault:** if the work created or changed a process, a skill, an agent, a decision or a piece of knowledge, write or update the note **in the same session**. New note, or edit the existing one, plus its map entry.
7. **Push the vault.** `roomcareOS/vault`, straight to `main`. Jay's PC pulls it automatically. An unpushed vault change did not happen.

## What earns a note, and where it goes

| The work… | Goes to |
|---|---|
| changed how something is done, repeatably | `Processes/` |
| changed how Claude or an agent should behave | `Agents and Skills/` |
| settled a question that could be reopened | `Decisions/` |
| established a fact worth keeping (research, evidence) | `Research/` |
| shipped a change to what a business *is* | the business note's status snapshot |
| is built but not merged or not live | `In Progress/`, naming the branch |
| is a raw capture not yet shaped | `Inbox/` |

**Not everything earns a note.** A bug fix, a copy tweak, a dependency bump: the commit is the record. Ask: *would a session six weeks from now do the wrong thing without this?* If no, skip it. The vault dies of clutter faster than of gaps.

## The standing rules for notes

- One idea per note, titled as the claim it makes.
- Every note carries frontmatter: `tags` (kind + business), `source` (where it came from), `updated`.
- Every note links its business and its map. Link generously — the graph is the product.
- Link to the repo, do not copy the repo. A copy drifts silently and then two answers exist.
- Plain English, British spelling, jargon translated in brackets. Jay works by voice from a phone.
- **Read his transcription charitably.** "WiFi Max", "WiFi Mix" and "WiFi Masks" always mean **YFarmX** — the speech-to-text mangles it every time. Likewise "Intervooh" often arrives as "interview", "Interview dot com" or "Intervoo". Never act on the literal mis-transcription; if a name is genuinely ambiguous, ask.
- Never a secret value anywhere. Environment-variable names only.
- Status snapshots go inside the business note under a dated heading, never in a separate note.

## When the vault and reality disagree

The vault is wrong. Fix the note in that session and say so. A note that quietly lies is worse than no note — that is what happened when the vault was built from `main` alone and missed everything on unmerged branches, which is why [[Map - In Progress]] now exists.

## Where this is wired in

Pointed at from the `CLAUDE.md` of every repo, so a session picks it up whether or not Jay says so (6 August 2026):

| Repo | Where | State |
|---|---|---|
| `roomcareOS/v1` (RoomCare) | `CLAUDE.md`, above the Todoist section | live on `main` |
| `roomcareOS/yfarmx` | `CLAUDE.md`, above the working rules | live on `main` |
| `roomcareOS/myhomework` | `CLAUDE.md` — created; the repo had none | live on `main` |
| `roomcareOS/scenecast` | `CLAUDE.md`, worded for a public repo | live on `main` |
| `roomcareOS/interviewprep` (Intervooh) | `CLAUDE.md` — created; the repo had none | on branch `claude/convex-vs-super-bass-dq1657`, **needs merging** |

## Related
- [[Todoist Doctrine]] — the board itself: the projects, what belongs in each, the eight rules
- [[Vault and Workflow Design (YFarmX)]] — the original design this extends
- [[Home]] · [[Map - Processes]]
