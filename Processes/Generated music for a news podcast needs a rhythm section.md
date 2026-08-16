---
tags: [process, yfarmx]
source: yfarmx scripts/make-intro.py; Jay's intro review 15-16 Aug 2026; Audio Network track metadata for "A Diabolical Caper"
updated: 2026-08-16
---

# Generated music for a news podcast needs a rhythm section

**The claim:** when you ask a music model for a serious news-podcast theme and it keeps coming back sounding like classical chamber music, the missing ingredient is not the instruments, it is a **steady rhythm-section pulse**. Say the tempo, name the bass and drums, and ban rubato explicitly.

## What happened

Jay asked for a proper podcast intro for [[Podcast - YFarmX Briefings]] on 15 August 2026, after rejecting a set of hand-synthesised stings. The first generated family briefed Lyria 3 for "sombre chamber strings", "solo piano" and "a dignified motif that resolves". It came back genuinely well-made and Jay's verdict was *"that one was too classical music"*. He named the targets: the Telegraph's **Ukraine: The Latest**, the Guardian's daily, and Rory Stewart's **The Rest Is Politics**.

## The one documented reference, and what it shows

Only one of those three has a publicly documented theme. Wikipedia identifies The Rest Is Politics' theme as **"A Diabolical Caper" by Luke Richards**, an Audio Network library track — the Wikipedia sentence itself is uncited, so treat the attribution as plausible rather than confirmed, but the track is real and its publisher metadata is hard fact:

> Tense, shady staccato strings with driving upright bass & drums. **A minor, 4/4, 95 BPM, marked "Fast".**

The Guardian's *Today in Focus* theme is a bespoke composition by **Axel Kacoutié**, its sound designer, written to interlock with speech pacing. *Ukraine: The Latest* has no documented musical credit anywhere.

## The rule that came out of it

The difference between a current-affairs theme and a classical or film cue is **how the instruments are played**, not which ones:

- **Strings are percussive** — staccato, on the beat, a rhythmic instrument. Not a legato melodic line.
- **A bass-and-drums pulse runs underneath**, metronomic from first beat to last.
- **A short repeating riff**, not a developing melody.
- **Minor key driving urgency through rhythm**, not through sustained dissonance.

Classical chamber music and film scores have freely-paced phrasing, sustained legato lines and no fixed pulse. **That absence of a beat is what reads as "too classical".** So the brief must state the tempo (95 to 105 BPM), name the rhythm section, and ban rubato in words.

## Which briefs Lyria actually honours (measured, 16 Aug 2026)

Asked for six different contemporary genres in one batch, **five drifted to pastiche**: "driving news theme" returned retro synthpop video-game music, "hybrid orchestral-electronic" returned synthwave, "frontier technology" returned lo-fi hip hop, "jazz-noir" returned gypsy swing, and the foreign-despatch brief **returned vocals despite an explicit instrumental clause**. The only one that landed was framed as **noir cinematic orchestral**.

**So: Lyria follows orchestral and cinematic vocabulary reliably, and answers contemporary genre labels with pastiche.** Write the brief in the register the model keeps its footing in and add the rhythm clause to stop it going classical, rather than naming the genre you want and fighting it.

Two more traps from the same work:
- **Spell the brand `Y-Farm-X` for any text-to-speech**, or every voice says "you-farm-ex". Verified by transcribing the output back with `gemini-flash-latest` and asking for phonetics — the cheap general way to check any pronunciation before shipping audio.
- **An automated style-checker is not a substitute for ears.** Asked to describe the same file twice, `gemini-flash-latest` called it "dramatic cinematic orchestral suspense" and then "lively acoustic guitar duet". It is reliable for the objective question (*are there vocals?*) and unreliable for the aesthetic one. Use it as a gate, never as a judge, and send the candidates to the person whose taste decides.

## Links

[[Podcast - YFarmX Briefings]] · [[Audio and Voice Production (YFarmX)]] · [[YFarmX]] · [[Map - Processes]]
