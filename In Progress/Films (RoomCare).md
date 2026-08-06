---
tags: [in-progress, roomcare]
source: v1@claude/roomcare-motion-video-ad-ap7267:marketing/films/PLAN.md, marketing/films/README.md, marketing/tiktok-ad/README.md
updated: 2026-08-06
---

# Films (RoomCare)

**Lives on branch `claude/roomcare-motion-video-ad-ap7267` in the v1 repo, not merged — there is no `marketing/` folder on main.**

Anyone reading the main branch cannot see any of this: the five films, the tooling that builds them, the copy that has already been through Jay, or the finished MP4s. Nothing is lost, but nothing is findable either. Merging the branch (or at least the `marketing/` folder) is the cheapest thing on this whole list.

Two bodies of work sit there:

- **`marketing/films/`** — the five vertical films, the current work. Each is 1080x1920, 40 seconds, 30fps, H.264 (the standard portrait video for phones), with a recorded British voiceover *and* every line on screen as type, so a film reads with the sound off.
- **`marketing/tiktok-ad/`** — the earlier standalone 30-second cut, `roomcare-tiktok-30s.mp4`. Finished and silent by design, with a beat sheet for whoever adds music. Superseded by the five, but the file and its poster still stand up on their own.

The spine of the set: **some asks a fitted room can answer on its own, and some need a person.** RoomCare knows which is which.

## Where each film stands

| # | Film | Who it is for | Picture | Voice | What blocks it |
|---|---|---|---|---|---|
| 1 | **Two kinds of ask** | General, investor | Built, rendered, approved | Recorded | Nothing. **Waiting on Jay to post it.** |
| 2 | **One walk** | Owners, managers | Built, rendered, approved | Recorded | Nothing. **Waiting on Jay to post it.** |
| 3 | **Between visits** | Families | Built and rendered | **Silent** | Two things: no Google AI Studio credits, so no voice take; and the script is being rewritten and needs Jay's approval before anything is rebuilt. |
| 4 | **Alongside, never in place of** | Trust and safety | **Old cut** | None | Treat as unbuilt. Needs copy first. |
| 5 | **Her room, her rules** | Residents, dignity | **Old cut** | None | Treat as unbuilt. Needs copy first. |

Films four and five still hold the first draft from the original batch: the old wipe between scenes, the old copy, and app screenshots rather than a drawn interface. They have never cleared the audit in that form. **They are not "nearly done", they are unstarted.**

### Film three: the rewrite waiting on Jay

The cut that is already rendered says "she" or "her" twelve times and never says who she is, what the shelf is, or what RoomCare is. Jay's note was **say "resident", not "her"**, and explain the product to someone who has never heard of it, in concise phrases.

A full replacement is drafted in `PLAN.md` under **PENDING REVISION** — six beats and this voiceover. **Approved by nobody. Do not build it without asking.**

> Made for the days in between. Family send photos, voice notes and short videos.
>
> Each one waits on the resident's shelf, on the screen beside the bed. No ring, no buzz, no pop-up.
>
> Nothing plays until the resident opens it, in their own time.
>
> A photograph, a voice, a face.
>
> And the family sees it was opened. Margaret opened this at ten past four.
>
> The resident chooses what reaches them, and can change it at any time.

The six on-screen beats carry the same words as eyebrow, display line and sub. Once approved it still cannot be voiced until credits return.

**Also undecided:** whether films one and two get the same "resident, not her" treatment. They are approved, rendered and voiced, so changing them costs a re-render and — until credits come back — costs them their voice too. Cheap to leave, expensive to redo later.

## The order of work that works

The one lesson worth carrying to films four and five, and to anything after them:

**Bring Jay the copy and the voice script first. Get approval. Then build.**

Every expensive moment in this project came from building a picture around words that had not been signed off. Film three is rendered, beautiful, and its script is being replaced. A film is roughly a day of building and 1200 frames of rendering; a script is twenty minutes of reading. Approve the cheap thing first.

## Building one

From `marketing/films/`, one film at a time:

```
node lib/build.mjs   01-two-kinds-of-ask          # inline the images into one file
node lib/audit.mjs   01-two-kinds-of-ask          # the checks, below
node lib/preview.mjs 01-two-kinds-of-ask 3.4 16   # single frames, at those seconds
node lib/render.mjs  01-two-kinds-of-ask          # 1200 frames, then the MP4
```

`lib/render-all.sh <film> <film> ...` does several in sequence; `lib/audit-all.sh` builds and audits all five.

**Never run two renders at once, and never preview or audit while one is going.** A render holds a 20 MB page plus 1200 frames in memory. Two at a time exhausted the machine once and the system killed a render *silently* — leaving frames on disk and no MP4, which looks exactly like a render still in progress. That failure wastes an hour before anyone notices.

Copy lives in the `<section>` blocks of `film.html`; timing lives in the `B` beat table near the top, where every cue is a beat plus an offset, so editing one row retimes that beat and nothing else. Lengthening a beat adds hold, and hold is what gives a reader time to finish a line.

Needs `playwright` (a browser driven by code) and a full `ffmpeg` with H.264 — the one Playwright ships only does WebM. `AD_CHROME` and `AD_FFMPEG` point at them if they are somewhere unusual.

## The four checks every film must clear

`lib/audit.mjs` fails a film on any of these, and all five in their *current* state pass. (Worth noting: the README calls these "three gates" and PLAN.md calls them "four gates". Same checks, two counts. Worth settling in the docs.)

1. **Reading time.** Steps the whole film and measures how long each line is both legible *and* motionless, then compares that to a plain reading estimate. This is the check that matters most: a beautiful line that leaves before it can be finished is worth nothing in a sound-off feed. It is why three supporting lines were cut from the set.
2. **Safe area.** TikTok lays its own captions and buttons over roughly the bottom 430px and the right 190px. Nothing readable may render below y=1450 or past x=890. The trap is that the camera magnifies the frame outward as it moves, so a line can sit inside the design box and still land under TikTok's buttons in the finished frames — so the authoring box is deliberately tighter: columns end at x=870, no readable line below y=1420. The check measures real text extents, not the column box, and it caught a hook's last word hiding under the buttons on a film that had already been built once.
3. **One animation per property.** Every animation holds its value before and after it runs. If two of them target the same property of the same element, the later one wins for the whole film, so something can sit visible from frame one when it should only appear on its cue. This happened twice during the build and is almost invisible in a spot check, because only the frames *outside* the beat are wrong.
4. **The copy scan.** `node scripts/check-copy.mjs` runs over this folder as well as the apps: British English, no em dashes, no banned wording. Same gate the product's own strings go through, per [[Copy Rules (RoomCare)]].

## The voice

```
GEMINI_API_KEY=... node lib/voice.mjs 01-two-kinds-of-ask
GEMINI_API_KEY=... node lib/voice.mjs 01-two-kinds-of-ask --voice Vindemiatrix
```

The words live in `<film>/voice.txt`, the take lands as `<film>/voice.mp3`, and `render.mjs` mixes it in automatically whenever it is there. Films one and two were recorded with the voice named **Achernar**. The key comes from the environment and nowhere else: never in a file, a log line or a commit.

One direction string in `lib/voice.mjs` goes out with every request, so the five films sound like one performance rather than five. It names the length budget out loud, because the first take paused at every full stop and came back at 57 seconds against a 40 second picture. The tool prints the length of every take and warns when a script has outgrown its film.

The picture still carries every line as type regardless. The voice is for the feeds that play sound; the captions are for the ones that do not.

**This is the blocker on film three.** No Google AI Studio credits, no take, and the film stays mute.

## Copy rules that bite in these films

Everything in [[Copy Rules (RoomCare)]] applies. These are the ones that decide shots and lines here.

- **"Resident", never "her" on its own.** Jay's note, and the reason film three is being rewritten: **say "resident", not "her"**. A viewer who has never heard of RoomCare needs telling who she is, what the shelf is and what the product does, in concise phrases. Pronouns without an introduction read as an in-joke.
- **Never suggest a resident holds back from asking, and never call her needs small or minor.** No hesitation, no reluctance, no "little things". The asks are ordinary and legitimate; the film's subject is what happens to them, never whether she should have made them. (The older 30-second TikTok cut still carries "Small asks have nowhere to go", which reads against this rule — worth a look before that file is used again.)
- **The keep-line is dropped from film three onwards, per Jay.** The line is the one sanctioned antithesis in the house style:

  > **"Works alongside the nurse call, never in place of it."**

  Films one and two close on it over a shot of the pull cord. Three onwards spends that sixth beat elsewhere — in film three, on the resident's own consent scope. *Open question for Jay:* film four is titled "Alongside, never in place of" and is built around exactly that idea, so someone has to decide what it closes on when it is rebuilt.
- **Nothing may suggest RoomCare touches the nurse call.** PRD §7 is explicit that the orchestration layer never touches the nurse-call system, so an ask can never be described as arriving *with* the buzzer.
- **Credit the equipment a home already owns.** A nurse call reports the room, on an over-door light and a handset. Saying it tells you nothing is both wrong and rude about a system the buyer pays for. **The gap is the ask, and only the ask.**
- **Never imply a care team is idle, aimless or guessing.** They answer every call and always have. What the buzzer does not carry is information, and that is the only gap these films are allowed to point at.
- **Physical room control is Phase 2.** Only the simulated driver ships today, so every line about lights, blinds or curtains is qualified with **"in a room fitted for it"**.
- **Nothing invented.** No fabricated interface, no market statistics, no claim the product does not already make. Two claims were pulled after a check against the PRD. When the app's own router answered "fresh towels" with a clarifying question, the film changed to an ask the router genuinely handles rather than faking the screen. Both asks in film two were checked against the router before a word was written.
- **Burgundy `#7A2F3C` only ever appears on the nurse-call and emergency path.** Same rule as the apps.

## Two picture rules worth keeping

- **Every interface is drawn**, from the shared design tokens, not screenshotted. A phone-sized screenshot blown up to a 1080px frame arrives soft, with the wrong type sizes and its own compression; a drawn component is sharp at any size and each part of it animates on its own cue, so choosing an option is something that *happens* on screen. The cost is one rule: an element gets exactly one animation, so a chosen state is a second complete copy stacked and cross-faded, never a colour tween. Films four and five still use screenshots and this is most of why they need rebuilding.
- **No large blank screen may sit in shot.** Several photographic plates were generated with a deliberately empty dark panel, meant for pasting a screen into. With that compositing abandoned, a big flat black rectangle just reads as a broken television — and a dead screen under the line "Works alongside the nurse call" is the worst place in the whole set to put one. Those plates were swapped for the pull cord. A phone or tablet held in a hand is fine; a wall-mounted slab is not.

## The dead compositing tools

`find-screens.mjs` and `check-rect.mjs` are still sitting in `lib/`, left over from the abandoned approach where a screenshot was pasted onto a screen inside a photograph. The README says plainly that they are **"no longer part of the build"** — but they are still there to be found, read and half-trusted by whoever opens the folder next.

This is a confusing halfway state: the documentation admits the tools are dead and the tools are still present. Either delete them or move them somewhere clearly marked as retired. **Has its own Todoist card.**

## Other loose ends

- **The directory names disagree.** `PLAN.md` calls film two `02-one-walk`; the README's file tree and its example render command both say `02-when-the-buzzer-rings`. One of them will send a command at a folder that does not exist.
- Films four and five need copy and a voice script drafted and approved before any building starts, per the order of work above.
- The whole `marketing/` folder needs merging, or this note is the only record anyone has.

## Related

- [[RoomCare]] · [[Map - In Progress]] · [[Home]]
- [[Copy Rules (RoomCare)]] — the word-level law these films inherit
- [[Decisions - RoomCare]] · [[Claude Operating Profile - RoomCare]]
