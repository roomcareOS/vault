---
tags: [in-progress, yfarmx]
source: yfarmx@claude/wifi-max-tiktok-plan-wzkahd:docs/tiktok-plan/
updated: 2026-08-06
---

# TikTok 60-Video Plan (YFarmX)

**Lives on branch `claude/wifi-max-tiktok-plan-wzkahd`, not merged. Nothing has been rendered or posted.**

It is a finished written plan for sixty vertical videos, two a day for thirty days, each one teaching a single useful thing off a [[YFarmX]] hub page and sending the viewer to yfarmx.com.

The plan is seven files: a README holding the schedule and the standards, and six series files holding a full brief per video (hook, beat-by-beat narration, every screenshot with its URL and the phrase to scroll to, image prompts, footage and its licence position, caption, hashtags, sources). This note is the shape and the rules. **The per-video briefs are not reproduced here and should not be: read them in `docs/tiktok-plan/` on that branch.**

## The thirty-day shape

Two slots a day: **09:00 UK** and **17:00 UK**. Mornings are the AI track, evenings are the crypto and quantum track, so every day carries one of each. Day 1 is the first day after Jay approves the plan and the first two cuts exist.

| Days | Morning, 09:00 | Evening, 17:00 |
|---|---|---|
| 1 to 10 | Series 1, the state of AI (V01 to V10) | Series 4, crypto rules and money (V31 to V40) |
| 11 to 20 | Series 2, AI video (V11 to V20) | Series 5, quantum from zero (V41 to V50) |
| 21 to 30 | Series 3, agents and robots (V21 to V30) | Series 6, the toolkit (V51 to V60) |

Order inside a series is deliberate, because each video leans on words the earlier ones taught. A slot can still be swapped for a breaking story: pull the relevant video forward and slide the rest back.

## The production standards (the reusable part)

These govern every cut, and they are the reason the plan is worth keeping even if the video list changes.

- **Remotion is the renderer, always.** All sixty are built in `remotion/`, not the older Python image renderer. Python still owns time and sound: it speaks and measures every line and writes the timeline, so captions stay frame-accurate and editing a line automatically re-times the picture ([[Video Production (YFarmX)]]).
- **Format is fixed:** 1080x1920, 30fps, 35 to 42 seconds, burned-in captions, everything inside the safe area. One file serves TikTok, Reels and Shorts unchanged.
- **Open cold, close identical.** No logo intro and no channel ident: the hook starts at frame one and lands inside three seconds. Every video ends on the same outro, the real cube logo loaded from the repo (never redrawn), the wordmark, the address. That closing frame is the only one all sixty share.
- **One fixed voice for the whole run.** Gemini text-to-speech (a machine read from written words), a professional British female newsreader delivery, held to a fixed style prompt so it never drifts between videos. It reads from the `GOOGLE_API_KEY` environment variable. **Scope is the TikTok cuts only**, the article audio players keep their own house voice ([[Audio and Voice Production (YFarmX)]]). The exact voice gets locked on the first cut and written into the README so nobody re-picks it later.
- **Sound is tonal.** Sines only, a slow two-chord pad, struck notes on data points. Nothing sampled, so no music licence attaches to our files, and the bed sits quiet enough that a trending sound can be laid over it in the app.
- **Show the receipts.** A claim appears on the company's or the regulator's own page wherever possible, captured at about 620 CSS pixels wide with the address bar visible, scrolled to the exact sentence the scene is about. Each brief names the URL and the phrase.
- **Teach one to three terms per video, the house way:** the plain phrase is the big label, the trade word sits small beneath it. Every term already exists in the site glossary.
- **Motion earns its place.** The brief names a base scene type and the build is expected to raise it: split plates (footage running while facts stamp in beside it), animated charts, dated timelines, card grids that deal and swap, layered parallax, term reveals. Motion is spring-based and settles. **Every animated figure is a real sourced number, never decoration.**
- **Footage discipline.** External clips run about five seconds, from public-domain or cleared sources only, with the licence position recorded in the brief. C-SPAN's own camera coverage is **not** public domain: use the committee's or the government's own feed instead. If no clean clip exists, screenshots carry the claim.
- **Image prompts go to Jay before anything renders.** Bright editorial collage per the house style, real logos asked for by name. Never a placeholder in a published video ([[Image Style and Prompt Libraries (YFarmX)]]).
- **Copy is the site's copy.** British English, primary sources only, no invented quotes, the banned-words gate. Narration runs 90 to 110 words and reads as one story: what changed, the proof, what it means for you.
- **Captions:** the first line has to work alone before the fold, three to eight accurate hashtags, and the last line is always the address written out in words, which is the sanctioned exception to the no-bare-domain rule ([[Social Syndication (YFarmX)]]). Cover frame is a still from the cut itself, normally the hook with the headline fully stamped in.
- **Posting stays manual,** uploaded from the render output folder, and each posted video gets logged in the repo the same way the 30 July Microsoft post was.

## The six series

**Series 1, the state of AI (V01 to V10, mornings, days 1 to 10).** Who leads large language models at the end of July 2026, what each family costs, what the free downloadable wave changes, and the vocabulary to read any AI announcement unaided. Strongest cuts: **V01**, "Three companies lead the most powerful technology on Earth. Here is the map." **V03**, "OpenAI just cut the price of its AI by eighty per cent. Here is why." **V06**, "Why does AI forget what you told it? The answer is a window."

**Series 2, AI video and how to use it (V11 to V20, mornings, days 11 to 20).** Takes a viewer from "what is AI video" to making one: the five labs that define the field, the four doors a beginner actually opens, the two shop-fronts that resell several models at once, then pricing, vocabulary and how to check whether a clip was generated. Strongest cuts: **V11**, "The best AI video generator on Earth is not the one you have heard of." **V12**, "Your first AI video can cost nothing, and you can make it today." **V13**, "You cannot download the most famous AI video app any more. Here is why."

**Series 3, agents, robots and the rest of AI (V21 to V30, mornings, days 21 to 30).** The AI that acts rather than chats: what an agent is, the coding agents, the connector standard the whole industry adopted, then image, music and voice models, humanoids, robotaxis, and two grounding pieces on a real security incident and on jobs. Strongest cuts: **V21**, "A chatbot answers you. An agent acts for you. That difference is the whole story." **V26**, "A humanoid robot now costs less than a small car." **V29**, "Anthropic says its own AI broke into three real companies, by accident."

**Series 4, crypto rules and money (V31 to V40, evenings, days 1 to 10).** The rules being written around crypto and the money moving through it: the American bill that stalled, the stablecoin law that passed, what actually holds a digital dollar at a dollar, the 2026 market in numbers, the UK and EU rulebooks, crime, tokenised funds. **V31 is the flagship of the run**, the only cut carrying committee footage. Strongest cuts: **V31**, "One law decides who polices crypto in America. It just stalled." **V34**, "Bitcoin is worth half what it was at the peak. Five numbers explain the whole market." **V38**, "8.2 million dollars, gone in one transaction. The thief broke no code at all."

**Series 5, quantum from zero (V41 to V50, evenings, days 11 to 20).** Zero to a working grasp: the 30 July advantage claims as the news peg, then the qubit, the two algorithms that threaten encryption, the harvest-now-decrypt-later problem, the replacement locks, error correction, the hardware race, Bitcoin's exposure and the listed stocks. Strongest cuts: **V42**, "Everything your phone does runs on coins lying flat. A quantum computer spins them." **V45**, "Your encrypted data can be stolen years before anyone can read it. That is the plan." **V46**, "The internet's replacement locks are already in your browser. And one candidate lock just died."

**Series 6, the toolkit (V51 to V60, evenings, days 21 to 30).** The closer. Three ten-word sprints against the glossary (AI, crypto, quantum), a sourcing-method video, four desk explainers, a genuinely-free tools round-up, and a tour of the site's own trackers, which is the video that converts viewers into readers. Strongest cuts: **V54**, "Most AI news is a screenshot of a screenshot. Here is how to check it." **V56**, "One company lost 8.6 billion dollars in three months, and plans to keep going." **V60**, "One site tracks crypto hacks, AI incidents and the quantum countdown, free."

## What unblocks it

Three cards, all Jay's, all priority 1. Todoist holds the live state; this note holds the shape.

1. **Review the 60-video TikTok plan before anything is rendered.** Read the README first. Approving it starts production.
2. **Approve the Gemini British female voice for the TikTok cuts.** The standing rule is that the video voice does not change without Jay, so this records the choice in writing. Confirming it lets the first cut lock the exact voice for all sixty.
3. **Top up the Google AI Studio balance for the Gemini API.** Every Gemini model currently returns a depleted-credit error, which is a balance rather than a rate limit, so it will not clear on its own. **Until it is topped up no voice can render at all**, which stalls this run whatever else is approved. It is the same top-up that is holding up the RoomCare and Intervooh voiceovers, so one visit clears all of them.

**What happens first, once those clear:** V01 and V31, the day-1 pair. V01 is the state-of-AI opener in the morning slot, V31 the CLARITY Act footage cut in the evening. Both build with the Remotion pipeline, and the image prompts inside each brief go to Jay before either one renders. The remaining fifty-eight follow across the thirty days.

## Claims to check before they are spoken

The briefs are sourced throughout and several flag their own soft spots. These are the ones to settle rather than say on camera:

- **V04, Gemini.** The hub page contradicts itself over which model holds the heavy tier. The brief says outright that this has to be resolved before the video is cut, and the spoken line re-confirmed against the corrected page.
- **V41, quantum advantage.** The qubit figure in the narration depends on what IBM's own post says at capture time. Check it before recording, and use the stronger figure only if the page carries it.
- **V46, the withdrawn encryption candidate.** The 29 July withdrawal date comes from our own tracker. Independent write-ups confirm that it was withdrawn but do not date it, so either find a primary date or drop the date from the spoken line.
- **V49, Bitcoin and quantum.** The brief deliberately leads on a different forecast year from the one the site page leads on. That is a defensible editorial choice, but the video and the page should not appear to disagree, so decide which one moves.
- **V16, the aggregator video.** Its plan prices could not be verified without a real browser, so no price is spoken at all and the only figures on screen come from the live pricing screenshot. Keep it that way.
- **Anything with a live number** (coin prices, fund flows, register counts, share drawdowns) was true at the end of July. Re-check on the day the cut is made, because a stale figure on screen with a source underneath it is worse than no figure.
- **Footage licences** are recorded per clip and are the one thing that cannot be fixed after publishing. The committee webcast in V31 is a government work and is cleared; the vendor b-roll used in the quantum series is editorial-use, so confirm the per-asset terms shown on the page before it goes into a monetised cut.

## Related

[[YFarmX]] · [[Map - In Progress]] · [[Social Syndication (YFarmX)]] · [[Video Production (YFarmX)]] · [[Audio and Voice Production (YFarmX)]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Social Platforms (YFarmX)]] · [[Mega Monetisation Plan]] · [[Home]]
