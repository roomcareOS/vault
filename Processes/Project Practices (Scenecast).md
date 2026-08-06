---
tags: [process, scenecast]
source: [SECURITY.md, CONTRIBUTING.md, PRD.md]
updated: 2026-08-06
---

# Project Practices (Scenecast)

The working processes of [[Scenecast]] as a public, security-first open-source project: the release gate, the contribution rules, and how security reports are handled. The security thinking here is a hard requirement equal in weight to the features — a feature that cannot be made safe is not shipped.

## Why the paranoia is justified

The tool does two historically risky things: it parses untrusted media files (image and video parsers have long bug histories) and it renders HTML in a headless browser (a big attack surface). A hostile file could try to read local files, reach internal network services, or exhaust the machine. So *everything the user provides is treated as untrusted* — including the config file and the card text — and trust begins only after a validation layer. The posture in one line (verbatim from `SECURITY.md`):

> Treat every input as hostile, run every processor with the least privilege inside a locked-down container, never build a command from a string, give the browser no way out to the network or the filesystem, cap every resource, keep nothing, and pin everything.

## The release gate

A build is not release-ready until every item in `SECURITY.md` §15 is true. The checklist, compressed:

- [ ] External commands run through argument arrays, never a shell string.
- [ ] ffmpeg/ffprobe run with a protocol allowlist and per-run timeouts.
- [ ] The headless browser has no internal-network egress and no file access beyond the job directory.
- [ ] All user text escaped in generated HTML; content-security policy forbids remote and inline script.
- [ ] SVG logos sanitised with external entities disabled (SVGs can carry scripts).
- [ ] Media type verified by magic bytes (the file's real signature, not its extension); size, dimension, duration and count limits enforced.
- [ ] Decompression-bomb guard (a tiny file that balloons in memory) caps dimensions before allocation.
- [ ] Paths validated; traversal and absolute escapes refused; writes confined to the output directory.
- [ ] Per-job temp directory, securely created, cleaned up on success and failure.
- [ ] Resolution, length, disk, memory and time budgets enforced.
- [ ] Non-root user with a read-only root filesystem in the container.
- [ ] Dependencies pinned, scanned and minimal.
- [ ] No telemetry; logs exclude media content; metadata stripped.
- [ ] Disclosure policy and supported-versions statement in the repo.

As of the current [[Scenecast]] status snapshot, everything is ticked except the container items (non-root, read-only filesystem, memory cap), which close with the container image. Security tests — hostile media, injection attempts, path traversal, malicious SVGs, egress checks, resource exhaustion — run in CI and gate releases. The same "prove it refuses attacks before shipping" habit as the [[RoomCare]] security proof and [[RLS and Schema Change Process]].

## Contribution rules (from `CONTRIBUTING.md`)

- **Security is not optional.** Any change touching file handling or command execution routes through the shared safety helpers in `security.py`.
- **Keep it simple.** A change that makes the tool feel like a framework or a hand-written spec works against the point — the same promise-protection test as in [[Claude Operating Profile - Scenecast]].
- **Tests with every change**, including a hostile-input test wherever the change touches parsing, paths or command building.
- **Writing style:** British English, no em dashes, plain concrete language.
- **Pull requests:** small and focused, describing what changed, why, and any security consideration.
- Dev setup: fork, venv, `pip install -e ".[dev]"`, Playwright Chromium, ffmpeg on the machine; checks are `ruff check src tests` and `python -m pytest` (integration tests skip themselves where ffmpeg or the browser is absent).

## Security disclosure process

- Reports go **privately** to `security@roomcare.uk` or via GitHub's private vulnerability reporting — never a public issue.
- Include affected version, description, steps to reproduce.
- Promise: acknowledge within a stated number of working days, assess, agree a fix and disclosure timeline, then coordinated disclosure with credit to the reporter if wanted.

## Privacy stance (worth remembering as doctrine)

Local by default, no telemetry, metadata (location, device data) stripped from media, minimal logging, temp files cleaned even on failure. Under UK GDPR, media of identifiable people is personal data; the tool keeps processing local and under the user's control, and the README says so plainly.

[[Map - Processes]] · [[Scenecast]] · [[Map - Agents and Skills]]
