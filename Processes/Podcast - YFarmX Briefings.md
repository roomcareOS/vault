---
tags: [process, yfarmx]
source: yfarmx docs/podcast.md, docs/podcast-cast.md, flags.mjs; Jay's launch call 6 Aug, page-redesign instruction 7 Aug, casting and canon rules 8 Aug 2026
updated: 2026-08-08
---

# Podcast - YFarmX Briefings

**LIVE since 6 August 2026.** `PODCAST_PUBLIC = true` in `src/lib/flags.mjs` on Jay's call. The show page (`yfarmx.com/podcast/`) and the RSS feed (`/podcast/feed.xml`) are public, indexed, in the sitemap, and — since 7 August — linked from the header's **More** menu and the footer. Four episodes are on the feed. **Not yet submitted to any directory**: Apple, Spotify and the rest wait on the R2 audio cutover and external feed validation ([[Media Storage and the R2 Rule (YFarmX)]], `docs/podcast.md` §7).

## The format: a scripted conversation, ENGRAVED

**An episode is two or three of five house characters taking one article apart** — Alice (anchor), Bob (explainer), Sally (sceptic), Jim (context), Melissa (markets). Jay engraved it on 7 August 2026: *"i really like the podcasts, keep the podcast voice playbook as is engraved."* The voices, models and assembly chain live in `scripts/make-podcast.py` and `docs/podcast-cast.md` (the cast moved to Gemini TTS voices on 7 Aug — Zephyr, Iapetus, Leda, Charon, Laomedeia, all British). **No session changes a voice without Jay saying so himself; a broken provider is reported and the run stops.**

**The character canon is explicit and binding (Jay, 8 August 2026).** `docs/podcast-cast.md` carries one card per character: fixed voice, personality, on-air habits and their cartoon portrait (the show page's circle avatars: Alice the professional anchor, Bob the boy genius, Sally the fox, Jim the retro robot, Melissa the cow). Every article episode follows the canon without exception — cast from the five only, fixed voices, lines that belong to their speaker — so listeners build rapport with a consistent cast.

The format's history matters because it caused a documentation trap. The first cut of the cast was pulled on 6 August ("their audio was shite") and the show relaunched for one day as a single-narrator read — the 6 Aug MetaMask episode on the feed is that format. The recast shipped the next morning, but `src/lib/podcast.ts` kept its 6 Aug comments, and the 7 Aug page redesign briefly shipped single-narrator copy written from them before the rebase surfaced the engrave commit. **When code comments and same-day docs disagree, check `git log` on both before writing copy from either.** `CAST` in `podcast.ts` is now populated (the show page renders the cast strip from it) and `AI_DISCLOSURE` describes characters performing a script.

### One male and one female voice, always (Jay, 8 August 2026)

Every episode carries at least one male and at least one female character; a two-hander is exactly one of each. Jay's words: *"have two voices, a male, British, and a female, British who are kinda bouncing off each other. because when you do female on female, you can't really tell it's a different voice sometimes."*

This is a legibility rule, not a personality one. Three of the five characters are women, so an unconstrained pick lands single-sex often, and on a phone speaker at 2x two female voices blur into one — the listener stops hearing a conversation. Pitch contrast is what keeps the turns apart.

Workable pairs: **Alice+Bob, Alice+Jim, Melissa+Bob, Melissa+Jim, Sally+Jim, Sally+Bob**. Barred: Alice/Melissa/Sally with each other, and Bob with Jim.

Enforced twice in `scripts/make-podcast.py` — the casting prompt states it as a hard rule, and `force_mixed()` swaps a character out if the reply ignores it. A `--cast=` named by hand is Jay's own call and is warned about, not overridden. Full reasoning in `docs/podcast-cast.md`.

The disclosure line ships on the show and every episode: the voices are not real people; the reporting is written and edited by the desk. Matches `/how-we-use-ai/`.

### The names come at the END (Jay, 9 August 2026)

Nobody introduces themselves at the top: the first thing a listener hears is the story. After the closing line, one speaker says thanks for listening, then each names themselves in the past tense with a handful of words only that character would say. Jay's brief: *"at the end have them introduce themselves, not in the beginning. At the end, say, thanks for listening. I was Sally, blah blah, and then the other person, blah blah, make them sound like their personality as well."* This reverses the older "never say thanks for listening" rule, which now applies to the top of the show only. The same instruction widened the interruption device from one or two an episode to two or three (*"make sure the podcast is interactive, they joke, they interrupt, really human dialogue"*). Both live in the SYSTEM prompt in `scripts/make-podcast.py`.

The sign-off earns its place when the line is in character rather than a name read out. From the first episode under the rule, on the open weights letter: Alice, *"glad we got through that without anyone reading the room"* (a callback to the Sacks post the episode quoted); Bob, *"and the letter still asks nobody to open anything"*; Sally, *"two open questions, and no published evidence for either"*.

**Hand-editing the script is the designed workflow, not a fallback.** `--script-only` writes to `data/podcast-scripts/<slug>.txt`, and `--script=<path>` renders an edited file, so a weak interruption or a stray em dash is fixed for free instead of by re-rolling a paid generation. The container needs `pip install miniaudio lameenc` before the first render of a session; the script only imports them at assembly time, so it fails after the whole script has been written.

## How an episode happens

1. The desk publishes an article — the episode never leads the reporting.
2. `python3 scripts/make-podcast.py <slug> --wire` writes the script (the article is its only source; hedges survive; `--script-only` is the cheap quality gate), renders each character's lines, volume-matches and assembles one MP3, and sets `podcastAudio:` in the front matter. An article with no episode audio simply is not an episode; `podcast: false` holds a finished one out of the feed.
3. `scripts/audio-manifest.mjs` measures the MP3 (exact bytes and seconds) into `src/data/audio-manifest.json`. **An unmeasured file is left out of the feed rather than published with a wrong length** — a wrong length makes players' scrub bars lie.
4. The feed carries it everywhere; the hourly rebuild publishes future-dated episodes on its own.

## The show page is the studio (7 August 2026)

Rebuilt on Jay's instruction ("really spruce up that page… quirky, creative animations, pictures"): animated hero, scrolling ticker, custom players (slug-seeded waveform seek, speed control persisted at up to 2×, one-at-a-time playback, Media Session metadata), a mini dock, desk tiles and a how-it-is-made strip. Two things worth keeping as rules:

- **Everything is progressive enhancement over native `<audio controls>`** — with JavaScript off the page still plays every episode. The audio files themselves are the product and are never touched by presentation work.
- **Scripts are written to sound right at double speed** (short declarative lines, no filler), so the player's 2× button is a first-class control, not an afterthought.

Seven page images were generated on OpenRouter (Nano Banana Pro) through `scripts/make-image.mjs` into `public/media/podcast-page/` — house collage style, "ON AIR" the only text in any of them, $0.98 total. Prompt discipline and the eyeball-before-ship rule are in [[Image Style and Prompt Libraries (YFarmX)]].

## Remaining to full launch, in order

All in `docs/podcast.md` §7; the blockers are Jay's Cloudflare clicks (Todoist has the cards):

1. R2 credentials + `media.yfarmx.com` binding → run the audio cutover (`scripts/push-audio.mjs`).
2. Validate the feed externally (podba.se/validate or castfeedvalidator.com) — our checker and theirs are not the same test.
3. Play one episode end to end in a real podcast app.
4. **Only then** submit: Apple → Spotify → YouTube Music → Amazon → Podcast Index. A feed address is permanent; a bad first submission is expensive in a way almost nothing else on the site is.
5. As each directory approves, fill its URL in `DIRECTORIES` in `src/lib/podcast.ts` — the show page renders pending chips until then.

## Related

[[YFarmX]] · [[Map - Processes]] · [[Audio and Voice Production (YFarmX)]] · [[Article Pipeline (YFarmX)]] · [[Media Storage and the R2 Rule (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Home]] · [[Decisions - YFarmX]]
