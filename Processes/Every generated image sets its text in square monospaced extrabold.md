---
tags: [process, yfarmx]
source: Jay, 16 August 2026, choosing from a five-candidate type sheet
updated: 2026-08-16
---

# Every generated image sets its text in square monospaced extrabold

**The claim: one typeface across every image [[YFarmX]] generates — heroes and infographics, every model, every desk. A square monospaced extrabold. Nothing is left to the generator's default.**

## What happened

Jay's note, on hero art: the headline was coming out in *"the default chat GPT font"*, and the fix he wanted was estate-wide rather than per-picture — *"all of the texts for the hero image and the infographics, the font needs to be changed."*

Rather than pick for him, one PORTRAIT 3:4 sheet went out with five candidates in five stacked bands, the same headline and middot line in each so only the letterforms varied: ultra-condensed heavy grotesque, neo-grotesque black, square monospaced extrabold, high-contrast Didone serif, wide extended industrial. He chose on sight — *"03 mono wow yes save for all future"*. Cost of the deciding sheet: $0.0543.

## Why the mono is the right answer, not just a preference

It is **JetBrains Mono's register** — the face the site already sets its tickers, data and code in. The artwork and the page underneath it now share a voice, which none of the other four candidates would have given. The mono-caps annotation codes, ref numbers and registration squares already in our images join the same family, so the whole picture is one type system.

The face it replaced was whatever ChatGPT Image 2 reached for unprompted, which is the same face every other AI image on the internet is wearing.

## The clause

Paste it into every hero and infographic prompt, verbatim:

> All rendered text — the headline, the middot subtitle line, every label and annotation — is set in a SQUARE MONOSPACED EXTRABOLD typeface: a technical terminal face, every letter on the same fixed width, squared blunt terminals, a rectangular skeleton, wide even letter spacing, with clean crisp edges. Not a pixelated, bitmap, 8-bit or stair-stepped font, and never a default sans-serif.

**Describe letterforms, never only a font name.** Generators do not honour names; they honour descriptions of shapes. This is the same mechanism as [[Ban invented figures in image prompts]].

## The two things that go wrong

- **Keep the "not pixelated" half of the clause.** The winning sheet came back with faint stair-step notches cut into the S and C. Invisible at sheet size; it would show at hero size. The clause names every way that failure presents (pixelated, bitmap, 8-bit, stair-stepped) because naming one is not enough.
- **Headlines get SHORTER.** Every monospaced character occupies the same box, so a headline that fitted comfortably in a condensed grotesque will not fit in this. Two words is the target, three short ones the ceiling. Past that the headline either shrinks until it is no longer the hero graphic, or runs off the canvas.

## Scope

Binds new images only; nothing is regenerated retrospectively. Applies on the Space and robotics desks too, which keep their dark palette and their one-or-two-word headline but now set it in this face like everywhere else.

## Related
- [[Map - Processes]] · [[Image Style and Prompt Libraries (YFarmX)]] · [[Ban invented figures in image prompts]]
- Repo: `docs/image-style.md` ("THE HOUSE TYPEFACE"), `docs/playbook.md` §3, decision 56 in `docs/decisions.md`.
