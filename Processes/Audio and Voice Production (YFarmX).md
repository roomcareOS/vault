---
tags: [process, yfarmx]
source: yfarmx/docs/audio.md, yfarmx/docs/video.md
updated: 2026-08-06
---

# Audio and Voice Production (YFarmX)

Every [[YFarmX]] article gets a spoken version, in **one fixed voice**. An article is not finished until it has audio, and nobody asks Jay whether he wants it ([[Article Pipeline (YFarmX)]] step 4 — apply his edits *first*, so the MP3 reads the approved words).

## The one rule above all others

**The voice does not change without Jay.** Readers should hear the same narrator on every article, so the voice is part of the brand, not a technical setting. That extends backwards too: **do not re-cut the archive without him** — the pre-5-August pieces sound different and that is a fact about the back catalogue, not a bug to fix.

## The voice (fixed — Jay chose it on 5 August 2026)

| Setting | Value |
|---|---|
| Engine | Gemini TTS (text-to-speech), `gemini-2.5-pro-preview-tts` |
| Voice | `Aoede`, female |
| Brief | `Read this aloud in a standard British accent: ` |
| Key | env var `GOOGLE_API_KEY` — the same one `make-image.mjs` uses |
| Output | 24 kHz mono, encoded to 96 kbps MP3 with LAME |

**Do not add adjectives to the brief.** This is the rule the voice work kept breaking. Three rounds were rejected and two of them failed on the brief rather than the voice:

- *"Received Pronunciation, warm, clear and measured"* produced a posh, nasal read. Jay: **"the voice is ugly"**.
- *"well spoken but not posh, warm and friendly, like you are talking to a mate"* produced cockney. Jay: **"those were atrocious. I didn't ask for cockney."**
- The bare one-line brief above, with no adjectives at all, produced the read he approved: **"just normal neutral British."**

Every adjective is a chance to overshoot in one direction or the other, so add none. If the accent ever needs changing, change the voice setting, not the brief.

**The brief has to stay short for a second reason: it is read as content if it gets long.** These models reject a separate system instruction outright ("Developer instruction is not enabled for this model"), so the brief sits in the spoken text — and the first Ori MP3 opened by saying the words "Received Pronunciation" out loud. The only real defence is `verify_opening()`, which transcribes the first chunk back and retries if the brief was spoken.

## Generate it

One-time install: `pip3 install gTTS lameenc`. Set the key in the environment, never in the repo: `export GOOGLE_API_KEY=...`

```
python3 scripts/make-audio.py <slug> --wire
```

- Takes the article slug, or a path to its `.md`.
- Reads the headline, then "In brief" plus the `summary`, then the body. Markdown is stripped (front matter, images, link URLs) and a table is spoken row by row.
- Saves to `public/media/<YYYY>/<MM>/<slug>.mp3`, year and month taken from the article's `date` — the same convention as images.
- `--wire` inserts the `audio:` field into the front matter for you. Omit it and the script just prints the line to paste.
- `--dry` prints the spoken script and exits without synthesising — use it to check what will be read before spending the run.
- A full article is a handful of requests and a couple of minutes. The service returns 503 under load and each chunk retries six times with exponential backoff, so **a few retry lines in the output are normal**.

Then commit the MP3 and the article. The template shows the "Listen to this article" player automatically once `audio` is set.

## Re-cutting a published article: `--suffix`, never `mv`

`/media/*` is cached for four hours, so a new read needs a **new filename**. Pass `--suffix=-v3 --wire` and the script writes and wires the right name in one step.

Renaming after the run is what broke the Starmind piece on 5 August 2026: the run died on a quota error, the wrapper renamed the *stale* file to `-v2` anyway, and the article ended up pointing at old audio labelled as new. This is the audio case of the general rule in [[Article Pipeline (YFarmX)]] — never change a published media file's bytes under an unchanged URL.

## Quota is the real constraint

The free tier allows **100 requests per day per model**, and one article is 4 to 7 requests. **Three articles plus a round of voice samples is enough to exhaust a model for the day.** The reset is about 24 hours and the failure is a 429 naming the per-day-per-model limit. The cap is per model, so `gemini-3.1-flash-tts-preview` is the fallback when the pro model runs dry. A billing-enabled key would remove the ceiling entirely — and this is currently a live blocker, not a theoretical one: [[Mega Monetisation Plan]] has the TikTok run held up partly on AI Studio credits, because while every call returns 429 no voice can render at all.

**Fallback voice.** With no `GOOGLE_API_KEY`, or with `--gtts`, the script uses gTTS (Google Translate's free text-to-speech) with `lang='en', tld='co.uk'`. That was the house voice from 21 July to 5 August 2026 and is what the archive before that date sounds like.

## How this relates to the video cuts

The vertical cuts in [[Video Production (YFarmX)]] deliberately use the **same house voice as the article players**, so the films sound like the site, and every renderer takes its voice from the same script. Everything around the voice is synthesised from scratch — nothing is sampled, so **no music licence attaches to any file we produce**. That is what leaves the obvious move open: TikTok rewards a trending sound from its own library, and our bed is quiet enough to sit under one or be muted entirely ([[Social Syndication (YFarmX)]]).

**One thing to confirm with Jay:** `docs/video.md` still describes the video voice as the gTTS en-GB one, which was true until the 5 August move to Gemini. Either the video path has not been switched over or the doc is stale — worth checking which, because "the videos sound like the site" is the whole point of sharing the voice.

## How this relates to the podcast

The article MP3s are the raw material for the YFarmX Briefings podcast — three episodes built, feed validating, public in October per [[Mega Monetisation Plan]]. That raises the stakes on the two rules above: a voice change does not just affect new articles, it fractures a back catalogue that is also a podcast back catalogue, and re-cutting the archive to match would mean new filenames for every file the feed already points at.

## If we ever want a different voice again

Swap `TTS_VOICE` in `scripts/make-audio.py` and nothing else; the brief carries the accent whichever voice is chosen. Gemini's other female prebuilt voices are Aoede, Autonoe, Callirrhoe, Despina, Erinome, Gacrux, Laomedeia, Leda, Pulcherrima, Sulafat, Vindemiatrix and Zephyr. **Already auditioned, so nobody re-runs them blind** — female: Sulafat, Erinome, Aoede (chosen), Leda, Vindemiatrix, Gacrux; male: Charon, Orus, Algieba, Schedar, Puck, Iapetus. The samples only ever lived in chat; regenerate rather than hunt for them. ElevenLabs remains the route to a studio-grade read if Jay ever wants it.

*Note on the source doc: `docs/audio.md` closes by calling `Kore` "THE voice", which contradicts its own settings table, its decision narrative and the audition list — all three say `Aoede`. Treated here as a leftover line, but worth one word from Jay to close it out.*

## Related

[[YFarmX]] · [[Map - Processes]] · [[Home]] · [[Article Pipeline (YFarmX)]] · [[Video Production (YFarmX)]] · [[Social Syndication (YFarmX)]] · [[Hermes Newsroom Pipeline (YFarmX)]]
