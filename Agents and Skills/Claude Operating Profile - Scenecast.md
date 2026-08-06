---
tags: [agent, scenecast]
source: [CLAUDE.md]
updated: 2026-08-06
---

# Claude Operating Profile - Scenecast

How Claude Code is directed on the [[Scenecast]] repo (`CLAUDE.md`). Where the [[Claude Operating Profile - RoomCare]] is about working safely with a non-developer founder, this one is about protecting a single product promise on a public, security-first codebase. `PRD.md` and `SECURITY.md` are named as the sources of truth; CLAUDE.md is just the map.

## The one thing to protect

> **Add your logo and your clips. Get a finished, on-brand video. Everything else is automatic.**

That promise *is* the product. Claude must lead with it in the README, help text, error messages, commit messages — any copy at all. The test for every design decision: if a choice makes the tool feel like a framework or a spec to hand-write, that choice is wrong. Positioning rule: never claim "first of its kind"; lead with "done for you, from your brand, locally", and describe competitors (Remotion, editly) in plain parallel statements.

## Working style

The owner (Jay) values **small surgical steps with honest caveats** and reviews each one. Do not batch large changes: build one module, show it working, move on. Same cadence as on [[RoomCare]].

## Non-negotiables (never traded for speed)

- **The security controls in `SECURITY.md` are mandatory.** External commands through argument arrays only (never a shell string, which closes the command-injection door). ffmpeg gets a protocol allowlist and timeouts. The headless browser gets no network egress and no filesystem access beyond the job directory. All user text escaped in generated HTML. Media validated by magic bytes with size, dimension and duration limits. Paths validated, writes confined to the output directory. The §15 checklist is the release gate — see [[Project Practices (Scenecast)]].
- **Contrast gate.** Every produced theme passes WCAG AA contrast before a build proceeds. A failure is a clear warning, never a silent pass. (The same legibility-first instinct as [[Copy Rules (RoomCare)]].)
- **Silent by default.** No audio track unless the user supplies one.
- **Inspectable decisions.** When footage is selected or dropped, the tool can emit the decision list (kept, dropped, why). Nothing is a black box.
- **Reproducible.** Same inputs, same output file.

## Writing rules (all code comments, docs, help text, README)

Kept close to verbatim because the exact wording is the rule:

- British English throughout: colour, organise, licence (noun) / license (verb), behaviour, centre.
- **No em dashes** — use commas, colons, or the word "to".
- **Never the word "matters".**
- **No buzzwords:** seamless, empower, cutting-edge, game-changing, leveraging, revolutionise, state-of-the-art.
- **No antithesis constructions:** avoid "not X, it's Y", "instead of", "rather than". State things positively; comparisons are written as plain parallel statements.
- Plain, concrete, warm language. No AI-sounding phrasing.

## Architecture Claude is told to respect

- Python; one responsibility per module (`brand.py`, `scenes.py`, `render.py`, `film.py`, `copywriter.py` and so on).
- **`security.py` is written first and everything routes through it**: the safe subprocess runner, path validation and limit checks are the single choke point for all file and command handling.
- The render core: a `timeline.json` becomes one HTML page exposing a deterministic `window.seek(t)`; Playwright drives Chromium frame by frame; frames pipe to ffmpeg. No animation runs on a wall clock, so output is reproducible and renders resume from cached segments.
- Bundled IBM Plex Serif font (open licence) so the look is identical on every machine.

## Where Claude is pointed

- What to build and why: `PRD.md` (phasing in §12; the MVP is Phase 1).
- How to keep it safe: `SECURITY.md` (§15 is the release gate).
- Config shape: `scenecast.example.yaml` and `config.schema.json`.

[[Map - Agents and Skills]] · [[Scenecast]] · [[Map - Businesses]] · [[Todoist Doctrine]] · [[Tool - Claude Code]]
