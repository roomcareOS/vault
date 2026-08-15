---
tags: [process, yfarmx]
source: yfarmx docs/podcast.md, docs/podcast-cast.md, flags.mjs; Jay's launch call 6 Aug, page-redesign instruction 7 Aug, casting and canon rules 8 Aug, format review answers 15 Aug 2026
updated: 2026-08-15
---

# Podcast - YFarmX Briefings

**LIVE since 6 August 2026.** `PODCAST_PUBLIC = true` in `src/lib/flags.mjs` on Jay's call. The show page (`yfarmx.com/podcast/`) and the RSS feed (`/podcast/feed.xml`) are public, indexed, in the sitemap, and — since 7 August — linked from the header's **More** menu and the footer. Fourteen episodes are on the feed, all serving from media.yfarmx.com since the 15 August R2 cutover. **Not yet submitted to any directory**: external feed validation and a real-app playthrough come first, then Jay submits with his logins (`docs/podcast.md` §7).

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

**The close, completed 9 Aug 2026:** *"the podcast has to have a nice 'thanks for listening, see you on the next podcast' ending"*. So the order is fixed: thanks for listening, then each speaker names themselves in character, then the LAST speaker looks ahead in five or six words ("see you on the next one"). This is the single place "see you next time" is allowed, and here it is required, so the show ends on a person rather than a full stop. Still barred even in the sign-off: a call to action, a web address read aloud, asking for follows or reviews, and trailing what the next episode covers. The sign-off earns its place when the line is in character rather than a name read out. From the first episode under the rule, on the open weights letter: Alice, *"glad we got through that without anyone reading the room"* (a callback to the Sacks post the episode quoted); Bob, *"and the letter still asks nobody to open anything"*; Sally, *"two open questions, and no published evidence for either"*.

**Hand-editing the script is the designed workflow, not a fallback.** `--script-only` writes to `data/podcast-scripts/<slug>.txt`, and `--script=<path>` renders an edited file, so a weak interruption or a stray em dash is fixed for free instead of by re-rolling a paid generation. The container needs `pip install miniaudio lameenc` before the first render of a session; the script only imports them at assembly time, so it fails after the whole script has been written.

## The format v2: Jay's 15 August 2026 rulebook

Jay reviewed the whole format in a structured question round on 15 August and his answers are now the rulebook every episode must follow. Engraved in `scripts/make-podcast.py` and `docs/podcast-cast.md`; where these touch earlier rules, these win. The companion decision the same day: **the single-voice read-out returned as every article's DEFAULT audio** (title then body, never the In brief, never tables, headings kept), with the episode as the encouraged "Listen to this podcast" option on the same player — see [[Audio and Voice Production (YFarmX)]].

1. **Pace 1.2, not 1.36** (*"speed, slightly too fast"*). The file carries the pace; players multiply from there.
2. **Length varies with the story, capped at five minutes**; two to three remains typical.
3. **The cold open stays pure.** No ident, no headline announce, no date; the story is the first thing heard.
4. **Free-form structure**, governed by the creative-but-human rules, never a fixed running order.
5. **Fixed pairings per desk**: AI = Alice+Bob, crypto = Melissa+Bob, quantum = Alice+Jim, space = Sally+Jim, any security story = Sally+Bob. Two voices default; the writer may add Melissa (money) or Jim (history) only when the story earns a third. This replaced the model cast-pick call.
6. **Add before you ask** (*"weird how the female just asks a question, without adding to previous speaker's context"*). No bare questions from anyone: contribute a fact or respond first, then let the question ride on it. Two experts in conversation, both adding facts they have read.
7. **No critic's-voice lines** (*"'that ordering is the story, not a...' doesn't sound good"*): never talk about "the story" as an object; state the fact and its consequence.
8. **Human, not overacted**: the 14 Aug newsreader register stands, but the voices sound like real people in natural conversation, with no long pauses or audible breaths in the gaps.
9. **Interruptions stay at two to three per episode.**
10. **Background strictly from our own published reporting**: a BACKGROUND block of previously published YFarmX articles (body-linked first, then the desk's recent coverage) is the only permitted source beyond the article. A sentence of continuity at most.
11. **Character memory sheets** in `data/podcast-cast-memory/<Name>.md`: one line per episode a character appears in, appended when the episode is cut, shown to the writer so a regular can carry a position forward. Hand-editable.
12. **The sign-off gains a follow ask**: after the names ("It's been Sally." / "I was Jim."), the last speaker closes with a very brief "follow YFarmX Briefings", worded slightly differently every episode, plus "see you on the next one". Web addresses, review begging and next-episode teasers stay barred; this is the show's only call to action.
13. **Short podcast-native episode titles**: the writer's first line is `TITLE:` (three to seven words), wired as `podcastTitle:`; the feed and apps prefer it. The back catalogue keeps its old titles.
14. **Every article stays an episode**; specials only when Jay orders them.
15. **Show notes are minimal**: article description, article link, disclosure line. No key points, no sources list.
16. **Nothing regenerated retrospectively** — all of this binds future episodes only; the catalogue stays as it is.

**Jim's cost myth, corrected the same day**: the "ten times the others" figure was the four-provider era's price table (MiniMax at $100/M chars), which survived the 7 August all-Gemini recast unnoticed in `docs/podcast-cast.md`. Since the recast the whole cast costs the same per episode. The stale table is fixed; the trap to remember is that a price table is provider-era-specific and dies with the provider.

## The show intro is generated, not composed (15 August 2026)

Jay rejected a first set of five hand-synthesised stings outright (*"i dont like previous noises, use google a proper podcast intro"*) and supplied a Google AI key to do it properly. The intro is now produced by `scripts/make-intro.py`: **Google Lyria 3 Pro writes the music**, **Gemini TTS speaks the ident** *"You're listening to YFarmX Briefings"*, and the script mixes them into a six-second opener on the standard broadcast shape (music alone on its hook, ducked under the words, back up for a beat, fade into the episode). Five presets ship with it: the-daily, broadcaster, broadsheet, dark-ambient, newsroom-pulse.

**Two production findings worth keeping, both learned the hard way:**

1. **Spell the brand `Y-Farm-X` for any text-to-speech.** Given the plain "YFarmX" every voice says *"you-farm-ex"*. Verified by transcribing the generated audio back with `gemini-flash-latest` and asking for the phonetics, which is the cheap general way to check ANY pronunciation before shipping audio.
2. **Lyria drifts to upbeat retro unless the prompt carries explicit negatives.** Round one asked for "restrained broadsheet" and returned synthwave, 1980s synth pop and EDM. Every preset now carries a NOT list, and that is what makes them land. A pizzicato/marimba brief drifted to whimsical folk twice even *with* the negatives, so that palette is avoided entirely. This is the same lesson the image prompts learned: positive adjectives alone drift, the negative clauses are load-bearing.

Also: Lyria occasionally returns a candidate with no content at all. It is transient and a plain retry succeeds, so the caller treats it as a network error rather than crashing.

**Still open from the round**: Jay's pick of one music preset plus one ident voice (Charon, Schedar, Orus and Algieba auditioned alongside Aoede; the three non-cast voices read as a distinct announcer rather than a character introducing the show), and his approval of the regenerated 3000x3000 cover. The sample episode in the new format is blocked only on `OPENROUTER_API_KEY` reaching the session environment, since `make-podcast.py` writes its scripts through OpenRouter.

**Provider note, unresolved:** the intro runs on Google, which contradicts the 14 August record that Google had been dropped estate-wide in favour of OpenRouter. Jay's instruction was "google tts use". An Inbox card asks him whether that reversal is permanent and how far it reaches.

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

All in `docs/podcast.md` §7. **Step 1 is DONE (15 August 2026):** Jay created the R2 credentials and bound `media.yfarmx.com` on the 14th; the cutover ran the next evening through the one-button `audio-cutover.yml` workflow (dry-run → one verified file → the batch of 102, 290 MB). Every file was verified on the CDN — 200 with the manifest's exact byte length, 206 on Range requests (Apple requires seeking), CORS for yfarmx.com — **before** `SHOW.audioBase` flipped, so the feed never pointed at a missing key. All enclosures now serve from `media.yfarmx.com`; guids are unchanged, so no re-delivery. Two traps for the record: `push-audio.mjs` originally matched only `audio:` and would have left all 17 `podcastAudio:` episode files behind, and `verify-seo` would have failed the first CDN URL — both fixed in the same change. The MP3s are still in the repo tree; deleting them is a deliberate later cleanup commit.

1. ~~R2 credentials + `media.yfarmx.com` binding → run the audio cutover~~ ✅ done, above.
2. Validate the feed externally (podba.se/validate or castfeedvalidator.com) — our checker and theirs are not the same test.
3. Play one episode end to end in a real podcast app.
4. **Only then** submit: Apple → Spotify → YouTube Music → Amazon → Podcast Index. A feed address is permanent; a bad first submission is expensive in a way almost nothing else on the site is.
5. As each directory approves, fill its URL in `DIRECTORIES` in `src/lib/podcast.ts` — the show page renders pending chips until then.

## Related

[[YFarmX]] · [[Map - Processes]] · [[Audio and Voice Production (YFarmX)]] · [[Article Pipeline (YFarmX)]] · [[Media Storage and the R2 Rule (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Home]] · [[Decisions - YFarmX]]
