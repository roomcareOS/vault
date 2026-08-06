---
tags: [in-progress, cross]
source: yfarmx@claude/workflow-architecture-plan-ptk232:docs/vault-design.md, yfarmx@claude/workflow-architecture-plan-ptk232:docs/workflow-architecture-plan.md, yfarmx@claude/workflow-architecture-plan-ptk232:docs/agent-todoist-brief.md, yfarmx@claude/workflow-architecture-plan-ptk232:docs/setup-checklist.md
updated: 2026-08-06
---

# Vault and Workflow Design (YFarmX)

**Lives on branch `claude/workflow-architecture-plan-ptk232`, not merged.** The same branch also carries the 22-file vault seed (`docs/vault-seed/`), which is the intended long-term home of this vault and moves to its own repo once that repo exists.

This is the design of **this vault** — logged as Decision 42, written 5 August 2026 from Jay's own brief and his two corrections to the first draft: this is a business vault rather than a personal notebook, so everything goes in; and capture is voice-first. It also sets out how agents are meant to work across the five properties.

Worth a wry note: the document describing how the vault should work was itself not *in* the vault until today. It has now filed itself.

## The four rules

1. **Never spend more than 1% of your time tweaking the vault.** The failure mode of every knowledge system is that fiddling with it feels like working. Default theme, minimum plugins, no elaborate hierarchy. If a session goes on the system rather than in it, stop.
2. **Everything business-related belongs in it — but ONCE.** The usual "start empty, never bulk-import" advice is written for personal notebooks, where imported material is dead weight nobody revisits. This is the opposite case: the repo docs are live operational truth, already curated, and both agents need all of them. So the whole estate goes in. What to avoid is not volume, it is **copies** — a duplicated document drifts from its original in silence and then two answers exist. **The repos stay the operational truth; the vault links rather than copies.**
3. **One idea per note, and the title is the claim, not the topic.** "Static sites beat a CMS when the publisher is a machine", not "Static sites". Two benefits: it forces the idea to be processed rather than hoarded, and it gives agents clean single-subject files to retrieve instead of documents where the useful sentence is buried on page four. (This applies to notes Jay writes; repo docs are what they are.)
4. **Capture must work from the phone, by voice, or the vault dies.** An idea that arrives while walking and is not captured in seconds is lost. The agent does the filing, so capture costs two taps and zero decisions.

## The folder scheme: the workspace *is* the vault

Jay's requirement was everything connected in one graph, with no duplication. That is met by pointing Obsidian at a folder that already contains everything, rather than importing into a separate one. All the repos live under one parent folder on the PC, and **that parent is opened as the vault**:

```
yfarmx-workspace/            ← open THIS as the vault
  yfarmx/                    ← the newsroom repo (articles, docs, playbook)
  roomcare/  norwichdrones/  myhomework/  intervooh/
  vault/                     ← the notes repo
  .obsidian/                 ← Obsidian's own settings, above every repo
```

What that buys, with nothing copied anywhere: every `.md` in every repo is a note (the real file — edit it in Obsidian and git sees the change); the graph spans the whole estate, article to article and decision to the document it governs; links cross repo boundaries and the backlink appears on the other side; and each repo still commits independently, because Obsidian only reads and writes files.

Inside it, the `vault/` repo holds what belongs to no single property:

| Folder | Holds |
|---|---|
| `notes/` | The atomic notes — one idea each, linked |
| `sources/` | One note per primary source (see below) |
| `registry/` | Screenshots and captures, filed by the agent |
| `properties/` | One page per property: what it is, where it lives, its state |
| `sop/` | Repeatable procedures worth not re-deriving |
| `attachments/` | Images and files |

**Settings to apply once, before exploring:** default attachment location set to `vault/attachments/`; "automatically update internal links" ON, so renaming repairs links instead of breaking them; **excluded files** set to `node_modules`, `dist`, `.git`, `site-recovery`, or thousands of machine-generated files drown the search and the graph. Default theme — theme-hunting is the classic breach of rule 1. No plugins at first; every community plugin is third-party code with reach into every repo in the workspace, so each gets checked before it goes in.

**Two cautions inside repo folders.** Do not rename repo files in Obsidian: an article's filename *is* its web address, so renaming it moves the live page. And article front matter (the settings block at the top of each file) is checked by the build, so edit it through Obsidian's Properties view rather than by hand.

**Reading a graph this size:** 700+ articles make a dense picture. Filter it (`path:src/content/articles`) and colour by path — articles one colour, `docs/` another, notes another — and the estate map becomes readable.

**Cost: £0, and it stays that way.** Obsidian is free for commercial use. Publish, Catalyst and the commercial licence are all "no". Sync is not the transport and never will be: it moves notes between Obsidian apps only, so agents cannot see it. **Git is what makes the vault agent-readable, and git is free.**

## Tags: output type only

| Tag | For |
|---|---|
| `#quote` | A verbatim line worth using, with attribution |
| `#study` | A finding, a figure, a piece of research |
| `#anecdote` | A story or example that illustrates |
| `#question` | Something unresolved and worth answering |
| `#idea` | Jay's own thinking, not sourced from anywhere |
| `#tool` | Something to use: a command-line tool, a skill, a service |

**Topic tags are deliberately not used.** Everything here is "AI" and "business" and "strategy" at once, so topic tags collapse into noise. Output tags stay useful: when a video needs a quote, the vault filters instead of being read.

**Source notes are mandatory**, and they are the newsroom's real win. Every idea taken from something names its source as its own note in `sources/` — a report, a filing, a regulator's page, a talk. The backlinks panel then answers "what else did we take from that FCA consultation?" in one click rather than a repo-wide search.

**Status is not tracked here.** It would duplicate the Todoist board (decision 41), and two systems tracking the same work means neither gets trusted. A note needing work gets a Todoist task pointing at it.

## Capture: voice-first, agent-filed

| What arrives | Door | What the agent does with it |
|---|---|---|
| A screenshot (a tool, a thing to remember) | WhatsApp to Hermes; the Drive "AI Inbox" folder until Hermes is hosted | Becomes a real note |
| A spoken idea | Voice note to Hermes, or voice-to-text | Transcribed into `notes/`, linked, tagged |
| A task | Todoist, by voice | Stays in Todoist. Never the vault |
| Something read at the desk | Straight into Obsidian | `notes/` |

**The screenshot loop Jay asked for, in three stages.** *Capture:* send it, caption optional — the caption is context ("this is for the video pipeline"), not a filing instruction. *Transform:* the agent writes a proper note — what the thing is, what it is FOR, when to reach for it, the image itself in `attachments/`, tagged, and **linked to the areas it touches**. *Recall:* every agent's standing instructions say to read the notes linked to an area before working in it, and to say so when one is used. So a screenshot of a tool resurfaces at the moment a task could use it.

The whole value sits in the linking. **A note filed without links is a screenshot with extra steps.** The sweep also prunes, because a registry nobody prunes becomes a second pile of screenshots.

## The workflow architecture, in plain terms

Five properties — yfarmx.com (the daily newsroom and the pattern everything else copies), roomcare.uk (raising funds), norwichdrones.com (TikTok-first, 2,500 followers heading for 10,000), myhomework.app and intervooh.com (both mid-build, databases but no logins or payments yet). Each is its own repo, its own deploy, its own budget line, all on the **same skeleton**, so any agent walking into any of them already knows where everything is.

The principles, all learned the expensive way:

- **The repo is the business.** Articles, queues, decisions, status: files in git. No dashboard as the source of truth. If it is not committed, it does not exist — and that is precisely what makes agents first-class operators rather than bolted-on scripts.
- **Quality lives in the repo, not in the agent.** The standard is the playbook, the banned-words list, the schema that fails the build, the checking scripts, and staging before live. Any agent writing into that pipe comes out at the same standard. That is the entire answer to "how do two different agents write to one quality bar".
- **Two businesses never share a blast radius.** Separate repos, separate databases, separate keys, so one property's mistake or bill can never reach another. RoomCare is the sharpest case: investors will look at its organisation, and it should contain nothing but RoomCare.
- **Secrets never touch a repo**, nothing destructive happens without a backup and Jay's confirmation, one meaningful change per commit, and ambiguous means ask while risky means stop.

**Two writers, one board.** Claude Code is the primary operator; Hermes runs daily on purpose so the habit builds. **Whoever starts an article finishes it** — no standing cross-agent editing pass, because consistency is a property of the pipe, not of cross-checking. Handoffs are the exception and happen only through the card, with the outgoing agent writing full state first.

**Money is one ledger with three meters:** Claude Max (fixed, so load everything interactive onto it), OpenRouter (£1,400/month metered, and the pot media generation comes out of), and subscriptions (a single page listing every recurring charge, reviewed monthly — solo operations die of subscription creep quietly). Media bytes go to R2 under [[Media Storage and the R2 Rule (YFarmX)]].

**Where it currently stands:** Todoist is built with five projects and the newsroom board; Higgsfield is on the Max plan; Hermes is confirmed running in the cloud but **its channels are not yet configured**, which is the literal switch for the WhatsApp and Discord doors. Still outstanding: Bitwarden (which stores the accounts and recovery codes that actually exist — losing access is a likelier disaster than being hacked, and the one with no fix afterwards), and deciding which of the two database accounts survives the merge. A Todoist token that was pasted into a chat is dated for regeneration on 12 August; the rule stands that secrets go into a secret store, never a message.

## How agents should use Todoist

The doctrine itself is [[Todoist Doctrine]]. Three things this design adds:

- **The brief travels by repo, not by chat.** Only sessions in the yfarmx repo can read its playbook, so the Todoist rules get pasted **once per repo into that repo's own `CLAUDE.md`**. Pasted into a single conversation, they die with the conversation.
- **Connection is a matter of surface, not repo, and only Jay can fix it.** Sessions on claude.ai need the Todoist connector added once in Settings → Connectors, and then **enabled in each individual chat** — that second step is what catches people out. Command-line sessions add the server themselves. Anything headless uses the REST interface with a token from the environment, `TODOIST_TOKEN` by name only.
- **If a session cannot reach the board, it says so and carries on** — reporting the gap plainly and writing the intended card changes out in full in its reply, so Jay can apply them by hand. Never silently skip the board, and never claim a card was moved that was not.

Two failure modes this was written against, both of which have already happened here: work done in one session and invisible in another (an R2 bucket was created on one session's instruction and another session did not know until Jay mentioned it), and tasks Jay cannot act on. "Ask Jay about the token" is not a task he can do; "Cloudflare → API Tokens → Custom → Workers R2 Storage: Edit, then add it to GitHub secrets under this name" is.

## Why it is worth the effort

A thinking space protects your judgment from being outsourced, and that lands harder here than most places, because the estate runs on machine-written output at volume. What makes the newsroom worth reading is not that a model can write. It is Jay's judgment about what is true, what counts, and what the desk will not say. That judgment has to live somewhere it can compound — and because the workspace is the vault, it lives next to the work rather than in a separate room.

## Related

[[YFarmX]] · [[Map - In Progress]] · [[Home]] · [[Todoist Doctrine]] · [[Media Storage and the R2 Rule (YFarmX)]] · [[RoomCare]] · [[Norwich Drones]] · [[MyHomework]] · [[Intervooh]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Article Pipeline (YFarmX)]]
