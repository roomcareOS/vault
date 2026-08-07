---
tags: [process, yfarmx]
source: yfarmx docs/podcast.md, flags.mjs; Jay's launch call 6 Aug and page-redesign instruction 7 Aug 2026
updated: 2026-08-07
---

# Podcast - YFarmX Briefings

**LIVE since 6 August 2026.** `PODCAST_PUBLIC = true` in `src/lib/flags.mjs` on Jay's call. The show page (`yfarmx.com/podcast/`) and the RSS feed (`/podcast/feed.xml`) are public, indexed, in the sitemap, and — since 7 August — linked from the header's **More** menu and the footer. Four episodes are on the feed. **Not yet submitted to any directory**: Apple, Spotify and the rest wait on the R2 audio cutover and external feed validation ([[Media Storage and the R2 Rule (YFarmX)]], `docs/podcast.md` §7).

## The format: one narrator, not a cast

**An episode is the article read aloud by a single synthetic voice.** Jay set this on 6 August after pulling the three five-character conversation episodes cut the day before ("their audio was shite") — those articles carry `podcast: false` and their files are untouched, so nothing was destroyed.

The five-voice cast (Alice, Bob, Sally, Jim, Melissa — Jay's blind-audition picks of 5 August) still exists in `scripts/make-podcast.py`, and restoring the `CAST` list in `src/lib/podcast.ts` brings the conversation format back, page copy and all. **Do not change or revive a voice without Jay** — a listener knows a cast.

The disclosure line ships on the show and every episode: the narrator is not a real person; the reporting is written and edited by the desk. Matches `/how-we-use-ai/`.

## How an episode happens

1. The desk publishes an article — the episode never leads the reporting.
2. Narration is produced deliberately per article (`podcastAudio:` in front matter). An article with no episode audio simply is not an episode; `podcast: false` holds a finished one out of the feed.
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
