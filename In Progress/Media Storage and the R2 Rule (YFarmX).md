---
tags: [in-progress, yfarmx]
source: yfarmx@claude/workflow-architecture-plan-ptk232:docs/media-storage.md
updated: 2026-08-06
---

# Media Storage and the R2 Rule (YFarmX)

**Lives on branch `claude/workflow-architecture-plan-ptk232`, not merged.** The same branch also carries the 22-file vault seed (`docs/vault-seed/`), which is the intended long-term home of this vault and moves to its own repo once that repo exists. Until the branch merges, the branch is the only copy of the policy below.

## Read this before the video week

The free Seedance week on the Higgsfield Max plan runs **7 to 13 August**, and its Todoist cards name this document directly. The line they quote is the whole policy:

> **Everything goes to R2, never git.**

A burst week is exactly when the rule gets broken, because a render is right there in the folder and committing it takes one second. That second is the expensive one. Every clip generated next week goes straight to a bucket and gets an index entry, or the week produces a pile nobody can search.

## The three tiers, decided by what a file *is*

| Tier | What goes there | Why |
|---|---|---|
| **Git** | Text, and small brand vectors: scripts, prompts, shot lists, render configs, the logo SVG and its derivatives | Versioned, comparable line by line, needed when the site builds, and small enough that keeping the history costs nothing. This is where the *knowledge* about media lives, never the media |
| **R2 public** | Anything a browser or a podcast app actually requests: article audio, podcast episodes, video the site serves. Served from `media.yfarmx.com` | Permanent and public. **Never replace the bytes under a live URL** — rename to `-v2` and repoint. `scripts/upload-media.mjs` enforces this; it will refuse an overwrite |
| **R2 private** | Production source: generated brand loops, b-roll, raw renders, superseded takes, project files | Nobody on the internet ever asks for these. No custom domain, no public access; the render pipeline fetches them with credentials |

(R2 is Cloudflare's file storage — a bucket is just a named folder living on their servers rather than on your machine.)

**The public/private split matters more than it looks.** Published media is permanent and must never break; production source is churn, iterated and superseded and deleted. Mix the two and a tidy-up in the b-roll can take down a live article, and the question "what are we actually serving?" stops having an answer.

## Why video never goes in git

Git keeps **every byte of every binary for ever**. Deleting a video from the repo reclaims nothing: the file is still in the history, and getting it out means a history rewrite that changes every commit's identifier and breaks every backup tag pointing at them. That is how the yfarmx repo reached about **535 MB** (decision 45).

**The same door is open right now in two other repos.** The RoomCare films sit in `marketing/films/` and the intervooh films likewise, and both are collecting renders in git today. They are still small. Moving them costs an afternoon now; in six months it costs the same afternoon *plus* permanent weight in the history that never comes out. Do it while it is cheap.

## The buckets: one pair per property

Businesses separate (decision 39), so buckets separate. A bucket that might one day be handed over with RoomCare should contain nothing but RoomCare.

| Bucket | Access | Holds |
|---|---|---|
| `yfarmx-audio` | Public, via `media.yfarmx.com` | Published article audio and podcast episodes. The name is historical — read it as "the published-media bucket" |
| `yfarmx-studio` | **Private** | Brand loops, b-roll, raw renders, takes |
| `roomcare-studio` | Private | The five films, their sources and renders |
| `intervooh-studio` | Private | The five films, their sources and renders |
| `norwichdrones-studio` | Private | Drone footage and cut sources |

Create each one only when there is something to put in it.

**Cost is not the constraint.** Storage runs about $0.015 per GB per month with the first 10 GB free and no charge for serving files out, so a few hundred clips costs pennies. Findability is the constraint, which is what the rest of this note is about.

## Naming, so a filename tells you what it is

```
<kind>/<subject>-<variant>-<nn>.<ext>

loops/cube-rotate-slow-01.mp4        a reusable brand background
loops/cube-rotate-slow-01.jpg        its poster frame
broll/norwich-cathedral-dusk-03.mp4
renders/2026-08/v01-state-of-ai.mp4  a finished cut, dated
takes/film3-vo-04.mp3                a superseded take, safe to prune
```

`renders/` is dated because finished cuts are the things you go looking for as "the one from August". `takes/` is understood by everyone to be disposable.

## Uploading and referencing

- **Upload through the script, not the Cloudflare web page.** `scripts/upload-media.mjs` is the single door, and it carries the never-overwrite gate. It gains a `--bucket` flag so the same gate covers production uploads, and it generates and uploads the poster frame in the same call.
- **Published media is referenced by its `media.yfarmx.com` URL** — that URL is a promise, so the bytes behind it never change. A new version is a new name (`-v2`) and a repoint.
- **Private media is never referenced by URL at all.** The pipeline fetches it with credentials held as environment variables and GitHub Actions secrets — `CLOUDFLARE_R2_TOKEN` and its siblings by name only, never a value in a repo, a note, a Todoist card or a chat message.

## Poster frames are not optional

**Save a poster frame (`.jpg`) beside every clip and embed it in the index entry.** One frame, taken at upload time:

```
ffmpeg -ss 00:00:01 -vframes 1
```

Choosing a background is a visual decision, and a wall of filenames cannot support a visual decision. A wall of thumbnails in the vault can.

## The index is what decides whether any of this works

A bucket of 200 plausibly-named clips is still a junk drawer. **Bytes in R2, knowledge in the vault** — the same split that governs everything else here.

Every clip gets an entry in `vault/registry/brand-assets.md`:

- what it looks like, in one line a human can picture
- duration, aspect ratio, dominant colour
- where it has already been used, so one loop does not open three videos in a row
- approved, or draft

That index is also what makes the library usable by an agent. "Find me a slow, dark, blue-dominant loop for the quantum piece" is answerable from text alone, and the clip it names is one fetch away. Without it, the same question needs a human to open forty files.

**An index begun late is an index never begun.** Start it with the first clip of the Seedance week, not at the end of it.

## What to do now

1. Create `yfarmx-studio` (private, no custom domain) the moment the first brand loop exists.
2. Add the `--bucket` flag to `scripts/upload-media.mjs`, with poster-frame generation in the same call.
3. Start `vault/registry/brand-assets.md` with clip number one.
4. Move the RoomCare and intervooh films out of git, in those repos' own sessions, while the history cost is still small.

## Related

[[YFarmX]] · [[Map - In Progress]] · [[Home]] · [[Todoist Doctrine]] · [[Video Production (YFarmX)]] · [[Podcast - YFarmX Briefings]] · [[Vault and Workflow Design (YFarmX)]] · [[Audio and Voice Production (YFarmX)]] · [[Staging and Backups (YFarmX)]]
