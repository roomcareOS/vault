---
tags: [in-progress, yfarmx]
source: yfarmx@claude/podcast-setup-plan-0byrij:docs/podcast.md, docs/podcast-cast.md
updated: 2026-08-06
---

# Podcast - YFarmX Briefings

**Lives on branch `claude/podcast-setup-plan-0byrij`, not merged. Built and working; nothing is public.** YFarmX Briefings is a two-to-three-minute daily show in which two or three of five house characters take one [[YFarmX]] article apart — what happened, what is established, what is only claimed, and the number that matters.

It is **not the article read aloud** — that was Jay's call on 5 August 2026. The single-voice read on the article page carries on unchanged as a separate product ([[Audio and Voice Production (YFarmX)]]); the feed carries only episodes.

The launch switch is `PODCAST_PUBLIC` in `src/lib/flags.mjs`, currently `false`. While it is off the show page is noindexed (invisible to Google), absent from the sitemap and off the nav. Three episodes are produced and the build is clean. Flipping that flag is a five-second job — everything below is what has to be true first.

## The cast

Jay picked these five by ear on 5 August 2026 from an eighteen-voice blind audition. **Do not change a voice without him** — a listener knows a cast, and swapping one is the audio equivalent of changing the masthead.

| Character | Accent | What they are for |
|---|---|---|
| **Alice** | British | Anchor. Opens, steers, hands off. Asks the question the listener would. |
| **Bob** | British | Explainer. Takes the technical thing and makes it plain. Dry. |
| **Sally** | British | Sceptic. Asks what is proven and what is only claimed. |
| **Jim** | Neutral | Context. Why it matters and what came before. Unhurried. |
| **Melissa** | American | Markets and money. Numbers, funding, who pays. Brisk. |

Each is a different supplier's voice, and they behave differently enough that the machinery has to compensate: one will only return raw uncompressed sound, one returns audio at the wrong sample rate, and their loudness varies enormously. Every turn is therefore converted to a common format and volume-matched before assembly, otherwise the cast sounds recorded in four different rooms. That is plumbing, not preference.

**Cost:** roughly **7p to 13p an episode** including the script, so **£10 to £20 a month** at five a day, out of the existing OpenRouter budget. Jim is ten times dearer than the rest, so he earns his place on stories that need the long view rather than appearing in everything.

**Written for double speed.** Most people play news podcasts at 2x, so the scripts are built for it: one idea per line, short declarative sentences, no filler ("right", "yeah", "exactly" — they turn to mush at speed), numbers spelled as spoken, and unfamiliar names introduced in context the first time rather than dropped in bare. The definitive cast list lives in `scripts/make-podcast.py`; the copy in `src/lib/podcast.ts` is only what the show page prints.

## How an episode is made

Two stages, deliberately separated, run per article:

```
python3 scripts/make-podcast.py <slug> --wire
```

1. **Write the script** (Claude Opus 5). Saved to `data/podcast-scripts/<slug>.txt` **before anything is rendered**, as plain text you can read and edit by hand.
2. **Render and assemble.** Each character's line is fetched from their own supplier, converted to a common format, volume-matched and joined into one MP3 at `public/media/podcast/<year>/<month>/<slug>.mp3`.

**`--script-only` is the quality gate, and it is the point of the two-stage design.** It writes the script and stops. Reading 300 words takes a minute; rendering is the expensive half. Once the script reads right, re-running renders it without paying to write it again (`--regen` forces a rewrite; `--cast=Alice,Bob` overrides the automatic casting).

Two accuracy guards are built in. The script prompt makes the article the only source, bans invented figures and quotations, and **requires any hedge in the article to survive into the briefing** — if the article says a claim traces only to secondary reporting, the briefing has to say so too. That distinction is the product. Separately, the script prints a warning listing any figure spoken aloud that does not appear word-for-word in the article. It warns rather than fails, because numbers are legitimately reworded, but every warning is worth a look before publishing.

An article with no podcast audio simply is not an episode — producing one is a deliberate act, never automatic. `podcast: false` additionally holds a finished episode out of the feed.

## Where things live, and the hosting shape

- **Feed:** `https://yfarmx.com/podcast/feed.xml` · **Show page:** `https://yfarmx.com/podcast/`
- **Audio:** uploaded to Cloudflare R2 (cheap file storage on the account we already have) at `media.yfarmx.com`, by `scripts/push-audio.mjs`. **MP3s are never committed to the repository.** At five episodes a day, keeping audio in git would add about 6 GB a year to a repo already at half a gigabyte, and git keeps every version of every file forever. R2 also costs nothing to serve, however popular the show gets — that is the whole reason for choosing it. See [[Media Storage and the R2 Rule (YFarmX)]].
- **Why a manifest exists:** the feed must state each file's exact byte length and playing time, for a file that no longer sits in the repo. `scripts/audio-manifest.mjs` measures every MP3 once into `src/data/audio-manifest.json`. An episode with no entry is left out of the feed rather than published with a wrong length — a wrong length is what makes a player's scrub bar lie.
- **Directories are not separate uploads.** Apple, Spotify, YouTube Music and the rest all simply read one RSS feed you own. You paste the same URL into each once and they poll it forever. So we self-host the feed and submit it everywhere — both, and we keep the keys. Letting Spotify host it would hand them the address everything is built on.
- **The show starts 5 August 2026** (`SHOW.epoch` in `src/lib/podcast.ts`) because the format did not exist before then. Older articles keep their on-page player and are not episodes.
- **The synthetic voices are disclosed**, in the show description and every episode's notes, matching the position at `/how-we-use-ai/`. Apple and Spotify both permit synthetic narration; what gets shows removed is pretending.

## Launch checklist, in order

Nothing here is done. The order matters — the last two steps come **before** any submission.

1. **Upload the existing episodes to R2** and repoint the articles at the new addresses (needs the credentials below).
2. **Flip `PODCAST_PUBLIC` to `true`** in `src/lib/flags.mjs`.
3. **Run `npm run build`** and confirm it is clean.
4. **`scripts/verify-podcast.mjs` must pass.** It fails the build on anything Apple or Spotify would reject — missing file length, an insecure link, artwork out of spec, a bad category, a duplicate episode ID. If it complains, do not proceed.
5. **Validate the feed with an outside checker** (podba.se/validate or castfeedvalidator.com). Our own checker and theirs are not the same test.
6. **Play an episode end to end in a real podcast app.** Not a browser, not the show page — subscribe to the feed on a phone and listen to a whole episode.
7. **Only then submit**, in this order: Apple → Spotify → YouTube Music → Amazon → Podcast Index. Apple emails a verification code to `podcast@yfarmx.com`.
8. Link the show from the nav and footer, add subscribe links to the article player, and announce on X and LinkedIn through the existing poster.

**Why steps 5 and 6 are non-negotiable.** A feed address is permanent. Once Apple and Spotify have read `yfarmx.com/podcast/feed.xml`, that address has to keep working for the life of the show — moving it means a redirect maintained for years and some listeners lost regardless. A bad first submission is expensive to undo in a way almost nothing else on this site is, so the cost of checking twice is nothing against it.

## Blocked on four things

All four are Todoist cards, and none of them can be done by Claude — they need Jay's accounts or Jay's ears.

1. **R2 credentials.** An API token scoped to the `yfarmx-audio` bucket only, Object Read & Write. Jay creates it in the Cloudflare dashboard and passes over the Access Key ID, the Secret and the Account ID; they go into GitHub Actions secrets and never touch the repository, same handling as every other key. Nothing can be uploaded until this exists.
2. **The `media.yfarmx.com` binding.** Create the `yfarmx-audio` bucket, connect the domain (Cloudflare adds the DNS record itself), and allow `GET` and `HEAD` requests from `https://yfarmx.com`. About two minutes of clicking. Until it resolves over HTTPS there is nowhere for the audio to live.
3. **Jay approving the cover art.** Every image is generated from the brand cube by `scripts/podcast-art.mjs` and can be regenerated identically, so nothing is hand-edited. **Judge the cover as a one-centimetre square on a phone**, because that is where people see it.
4. **The article-voice decision** — the one that actually needs Jay's judgement rather than his login. See below.

## The open decision: does the cast read the articles too?

Every article page has a "Listen to this article" player. Today that is one fixed house voice, Aoede with a plain British brief, and it is the same voice on 500 pages ([[Audio and Voice Production (YFarmX)]]).

The question is whether the five-voice cast should also take over those article reads, or whether article pages keep the single house voice and the cast stays exclusive to the podcast.

Worth holding in mind while deciding:

- **A voice change is retrospective in effect.** Readers hear the narrator as part of the brand. Changing it splits the site into before and after, and re-cutting the archive to match would mean a new filename for every audio file, because published media is never changed under an unchanged address ([[Article Pipeline (YFarmX)]]).
- **Two voices is a legitimate answer**, and arguably the tidier one: the article player is one person reading you a piece, the podcast is a conversation about it. They are different products already.
- **Cost does not decide this.** Even the dearest option on the table is a few percent of the OpenRouter budget.
- **If a voice sounds wrong, change the voice, not the brief.** Adjectives in the brief are what produced the posh read and the cockney read; the plain one-line brief with no adjectives is the one Jay approved.

There is a separate loose end worth one word from Jay: moving narration billing to OpenRouter would remove the free-tier request cap that currently throttles audio work, regardless of which voice wins.

## Related

[[YFarmX]] · [[Map - In Progress]] · [[Audio and Voice Production (YFarmX)]] · [[Article Pipeline (YFarmX)]] · [[Media Storage and the R2 Rule (YFarmX)]] · [[Home]] · [[Hermes Newsroom Pipeline (YFarmX)]] · [[Decisions - YFarmX]]
