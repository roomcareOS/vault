---
tags: [tool, yfarmx]
source: [README.md, PRD.md, PROGRESS.md]
updated: 2026-08-06
---

# Scenecast

**Not a business — a tool [[YFarmX]] ships.** Jay's call, 6 August 2026: Scenecast began as an experiment and belongs to YFarmX as open-source marketing, not as a project in its own right. The thinking behind that, and the rules for the tools that follow it, are in [[Open Source Tools (YFarmX)]].

A free command-line tool (run by typing commands, no app window), MIT licensed. Repo is currently `roomcareos/scenecast` and credited "built by RoomCare" — **both move to the YFarmX organisation in the org split, and the credit changes with it.**

**The promise, which is the whole product:** *Add your logo and your clips. Get a finished, on-brand video. Everything else is automatic.*

Scenecast reads a brand's colours straight out of its logo, watches the user's own footage to find where the shots change, writes text and colours onto the video at those points, and encodes a finished MP4 — all on the user's own machine, with nothing uploaded. One command (`scenecast auto logo.png clips/`) produces a polished vertical film ready for TikTok, Reels or Shorts.

## Why it exists

- **Strategically:** it is the maintained, public version of the video pipeline [[RoomCare]] already uses in-house to make its own films. Packaging it earns GitHub stars, developer goodwill and feedback — the promotional goal — while giving RoomCare a documented version of its own tooling. The engine is proven; the new work is packaging and hardening.
- **For users:** the alternatives are slow manual editors, subscription template tools that hold your media on their servers, or hand-rolled scripts. Scenecast is the "done for you, from your brand, on your own machine" option: two inputs, no timeline, no code, no spec.

## What makes it different (priority order)

1. **Two inputs, and that is all** — a logo and a folder of clips.
2. **Brand read from the logo** — colours extracted and applied, with text contrast checked (WCAG AA, the readability standard) so words always stay legible.
3. **It watches the footage** — scene detection finds every cut; text cards land on the cuts by themselves.
4. **Local and private** — media never leaves the machine, and the tool is hardened against hostile files (see [[Project Practices (Scenecast)]]).
5. **Looks right by default** — house-style card timing and spacing, no fiddling.

Comparison line used everywhere: Remotion makes you write React components; editly makes you write a JSON spec; Scenecast takes a logo and a folder — you write nothing.

## How it works, in one breath

Logo → colour palette + contrast-checked theme. Media → validated, cleaned, fitted to shape. Clips → scene detection finds exact cuts. Text cards → placed on the cuts, timed for reading. Render → the timeline becomes a single HTML page driven frame by frame in a headless browser (a browser with no visible window), captured and encoded to MP4 by ffmpeg (the free video engine). Every frame is a pure function of the seek position, so the same inputs always produce the same file, and long renders resume from where they stopped.

An optional research mode (`--brief` + `--research`, needs `ANTHROPIC_API_KEY`) has an AI write the on-screen story about the user's subject with web search — verified facts only, sources recorded, story always shown for review before rendering. Only the brief and a few trait words travel; footage never leaves the machine.

## Roadmap

- **Phase 1 (MVP):** the full pipeline — built.
- **Phase 2:** heuristic "best bits" selection, optional supplied audio, more platform presets.
- **Phase 3:** learned aesthetic selection, auto-drafted card text (always human-reviewed), subtitles.
- **Phase 4:** an optional MCP server (a way for AI assistants to drive the tool), shipped only with its full security posture.

## Status snapshot (6 August 2026)

- **Phase 1 is built and working end to end.** All seven commands live: `auto`, `init`, `brand`, `probe`, `build`, `film`, `preview`. Auto mode probes and scores footage, drops the dull moments, writes the story itself in the house voice (seeded, so reruns are reproducible), and records every decision in a story file beside the output.
- **Verified in real runs:** a branded 720x1280 build from a logo, three photos and three cards; scene detection finding both cuts on a real clip at the exact frames; resume reusing cached segments; a 7-second cinematic film rendering in about 20 seconds.
- **Release gate** (the security checklist in `SECURITY.md` §15) is nearly closed: everything is done in code except the non-root, read-only-filesystem, memory-capped container, which is the next packaging task.
- **Next:** PyPI release (`pip install scenecast`) and the container image with browser and ffmpeg pinned; then full SVG logo support via a vetted rasteriser in the container.

## Related

- [[Claude Operating Profile - Scenecast]] — how Claude is directed on this repo.
- [[Project Practices (Scenecast)]] — release gate, contribution rules, security disclosure.
- [[RoomCare]] — the parent company and origin of the pipeline; the contrast-and-legibility obsession is the same DNA as [[Copy Rules (RoomCare)]].
- [[Map - Businesses]] · [[Home]]
- Estate-wide board rule: [[Todoist Doctrine]]
