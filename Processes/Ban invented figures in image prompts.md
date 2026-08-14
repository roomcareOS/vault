---
tags: [process, yfarmx]
source: Jay, 14 August 2026, on the LATEST LLMS hub banner
updated: 2026-08-14
---

# Ban invented figures in image prompts

**The claim: an image model asked for a spec sheet, a benchmark table or a chart will fill it with authoritative-looking invention. Ask for blank paper instead, and name the only strings allowed to be legible.**

## What happened

The first pass at the `/ai/llms/` hub banner came back beautiful and wrong: a benchmark table listing GPT-4o, Claude 3.5 Sonnet, Gemini 1.5 Pro and Llama 3.5, with fabricated scores, under a headline reading "LATEST LLMS", on a page whose actual newest entries were GLM-5.3, Gemini 3.7 Flash, DeepSeek V4 Pro and Grok 4.6. Also an invented spec sheet (671B parameters, 128K context) and a release timeline ending in 2026.

One regeneration with the clause below produced ten accurate logos, correct category tabs, and nothing to fact-check. Cost of the lesson: $0.13.

## The clause that works

> Render NO invented numbers, parameter names, code, prices, benchmark scores, dates or version numbers anywhere in the image. Where a document, dial, chart or screen would normally carry data, show blank ruled paper, unlabelled gradations, or handwriting too small to read. The only legible text is the headline, the middot subtitle line, the reference line, and the real company logos named above.

The mechanism: generators treat empty document surfaces as space to fill. Give them a legitimate way to fill it (blank rules, illegible handwriting) and they stop inventing. Naming the permitted legible strings is what makes it enforceable.

## The two accuracy bars

Jay's words: *"tbh its fine, little text with outdated data in heros dont rly matter. its very small not noticable. infographics must be accurate... but try accurate ok."*

- **Infographics: absolute.** Every figure verified against sources before shipping. They are read as data.
- **Heroes: incidental small text tolerated, aim for accurate anyway.** Marginalia nobody can read at card size is not worth a regeneration; a legible fabrication contradicting the page still is.

## Model policy (same day)

ChatGPT Image 2 (`openai/gpt-5.4-image-2`) for intricate work: heroes, collages, anything with logos or a headline. A cheap model for basic art. Route through `/api/v1/images` (the `make-image.mjs` default without `--input`), which honours the aspect ratio and bills a fraction of the chat route: roughly $0.035 per 16:9 hero against $0.24.

## Related
- [[Map - Processes]] · [[Image Style and Prompt Libraries (YFarmX)]]
- Repo: `docs/image-style.md` carries the same rules next to the code.
