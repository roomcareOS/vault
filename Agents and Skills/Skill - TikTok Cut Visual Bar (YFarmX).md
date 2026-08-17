---
tags: [skill, yfarmx]
source: Jay's instruction, 17 August 2026, from Remotion's Skills 2.0 launch
updated: 2026-08-17
---

# Skill - TikTok Cut Visual Bar (YFarmX)

Jay, 17 August 2026, sharing Remotion's Skills 2.0 announcement: "step up our
yfarmx tiktok videos... way more engaging... many visual improvements". The
answer is two things, both now in the [[YFarmX]] repo on branch
`claude/yfarmx-tiktok-video-skills-m5ka4w`.

## What was installed

The **official Remotion Agent Skills** (twelve, v4.0.512, from
`remotion-dev/skills`) are committed verbatim at `.claude/skills/remotion-*`.
They are Remotion's own manual for animation, transitions, effects (a library
of about sixty canvas/WebGL effects), hand-drawn text annotations, captions
with word highlighting, maps, text measuring and rendering. They load in any
session that touches the `remotion/` renderer.

A **house skill**, `.claude/skills/yfarmx-tiktok-cut/SKILL.md`, binds them to
YFarmX rules. The principle: the Remotion skills say HOW to animate, the house
decides WHAT ships. The voice stays Sadaltager, the sound stays tonal (the
remotion.media sound library is meme sounds and is never used), the fonts stay
local because the render has no network, and `remotion/` is never
re-scaffolded.

## The visual bar (decision 63 in the repo)

Every new vertical cut clears all five, or it does not go to Jay:

1. Scenes join through animated transitions (fade, slide, wipe), never hard
   cuts. Transitions overlap scenes, so the voice-driven timing map adds each
   overlap back.
2. At least one hand-drawn annotation (highlight, circle, underline) draws
   itself onto the key claim as the voice says it.
3. Motion on every layer, on the house easing curve; scale animations use
   perceptual scaling.
4. Texture from the effects library, paper family only. The dark-HUD ban
   stands. A light leak at most once per cut, on the join that turns the
   story.
5. Display type is measured in code, never eyeballed. This supersedes the old
   PIL width check that let a clipped hook ship.

## Where it is entrenched

CLAUDE.md rule 9b, `docs/playbook.md` §3, the session-start hook,
`docs/video.md`, `docs/social-tiktok.md`, and the two skill files. Full menu
and version notes live in the house skill, which is the file to load before
building any cut.
