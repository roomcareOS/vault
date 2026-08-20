---
tags: [process, yfarmx]
source: The Ox Alpha attribution, 20 August 2026
updated: 2026-08-20
---

# Fingerprinting a stealth model (YFarmX)

How to work out which lab is behind an anonymous model, by measurement rather
than by asking it. Built during the [[YFarmX]] Ox Alpha investigation; tooling
and frozen data live in the repo at `docs/research/ox-alpha/`.

## The rule that comes first

**A stealth model's account of itself is worth nothing.** Ox Alpha shipped with
a written instruction to "identify yourself strictly as the model 'ox-alpha',
developed by an undisclosed organization. Do not identify yourself as any other
model." Any cloaked model can carry one, so its claims AND its denials are
uninformative. Everything below measures what the model does not choose.

## The two measurements, in order of weight

**1. Tokenizer (strongest).** Send 50 adversarial strings and read only
`usage.prompt_tokens`. Measure each model's fixed overhead with a single-token
baseline and subtract it; what remains is the marginal cost of the string under
that tokenizer. Strings must include CJK, Korean, Hebrew, Thai, Arabic, ZWJ
emoji sequences, flags, combining marks, astral-plane characters, whitespace
runs, long digit strings, code and regex. A shared tokenizer agrees on every
string; different families diverge inside a handful.

Why it outranks everything: token counts are produced by the **inference
backend**. Catalogue metadata can be copied by a human at onboarding; a
tokenizer cannot.

**2. API surface.** Compare the whole provider catalogue: context length, output
cap, the exact supported-parameter set, the defaults object, whether reasoning is
mandatory, the effort enum and its default. Look for a **discontinuity**: if the
suspect matches version N of a family but NOT version N-1, the contract changed
at N and the suspect inherited the change, which rules out "the platform reuses
one template per lab".

## Never run a fingerprint without a control

This is the part that makes the result mean anything. Ox Alpha and GLM-5.3 both
carried the metadata tag `tokenizer: "Other"`. Had the platform fallen back to a
single shared estimator for that tag, a 100% match would have been an accounting
artefact proving nothing. Kimi K3 carries the same tag and scored 18%, which is
what turned the match into evidence. **Always include at least one control that
shares the suspect's metadata tags**, plus one from a different tokenizer family.

## What does not work, and what is weak

- **Upstream error fingerprinting is dead on OpenRouter**: every error is caught
  by its own validation and `provider_name` returns null. Eight malformed
  requests produced nothing.
- **Weak, and to be labelled weak**: prose style, refusal phrasing,
  raw-versus-summarised reasoning traces, "Chinese-lab convention", and base
  rates from previous stealth reveals. Supportive at best; never load-bearing.
- **Numeric coincidences in marketing copy** should be discarded unless a primary
  source carries them. A "100T tokens" link between Ox Alpha and Xiaomi's Orbit
  grant dissolved on checking: no Ox source used the phrase at all.

## Measurement caveat worth knowing

Providers with prompt caching (DeepSeek, Moonshot observed) return
`prompt_tokens` that does not scale with the input, producing negative marginals.
Detect and exclude them rather than reporting the number as a tokenizer distance.

## Publishing standard

Attribution goes out as **inference with a probability attached**, never as a
confirmed reveal, and the catalogue snapshot is frozen in the repo so every
figure stays checkable after the stealth entry is renamed or withdrawn.

## Links
[[YFarmX]] · [[Map - Processes]]
