# Model Identity Measurement (YFarmX)

How YFarmX fingerprints AI models, and the traps that cost real money or real
correctness. Built 21 August 2026 on `claude/yfarmx-model-identity-nhz6yu`;
the PRD reframed the Tokenizer Atlas as one feature of a larger product:
an independent identity, lineage and change-monitoring layer at `/ai/models/`.

## The pipeline

```
scripts/model-identity/tokenizer-sweep.mjs     measure (resumable, budget-capped)
docs/research/model-identity/sweep.json        raw observations, canonical, append-only
scripts/model-identity/build-identity.mjs      clusters, groups, confidence, controls
src/data/model-identity/*.json                 the only thing pages render from
```

Method: 50 adversarial strings, read only `usage.prompt_tokens`, subtract a
single-token baseline, the 50 marginals are the fingerprint. Strings extracted
byte-exact from the original `tokenizer-test.py` so old and new runs compare.

## Measurement traps (each one bit during the build)

1. **Provider routing corrupts marginals.** The same named model served by two
   providers can carry two chat templates with different overheads. Fix: pin
   calls to the first provider seen (`provider: {order: [p], allow_fallbacks:
   false}`) and measure a separate baseline per provider when routing moves
   anyway. This, not genuine drift, caused the pilot's "variable overhead"
   failures on the Llama controls.
2. **OpenAI-served models return usage only on COMPLETED generations.**
   `max_tokens: 1` gives `finish_reason: length` and NO usage block. Fix: a
   sticky per-model variant ladder ending in "system: Reply with only: OK,
   max_tokens 800, reasoning effort low". The variant is part of the overhead,
   so it must not change mid-model.
3. **Template boundary shifts split real families.** A template that glues the
   user string to an adjacent character shifts affected rows by exactly one
   token. Two exact signatures whose differing rows all share one +-1 delta
   are ONE tokenizer behind two templates: fold into a tokenizer group, record
   the link. Without this, 12 Llama3 control models split into 4 clusters.
4. **Invalid runs must not cluster.** A record the harness marked
   `unmeasurable:variable-overhead` still carries plausible-looking counts;
   ingesting it gave inception/mercury-2 a false signature. Filter by status
   at ingestion (the review fleet's best catch).
5. **Prompt caching corrupts row-level counts** (negative or above-byte-length
   marginals). Mark rows corrupt individually; eight or more corrupt rows =
   `unmeasurable:caching`. Clean rows of a caching model are still usable for
   divergence evidence, never for exact signatures.

## Spend traps (OpenRouter)

- **`usage.cost` UNDER-COUNTS.** Failed, retried and usage-less calls still
  bill. The sweep's counter said $1.82 while the account fell from $5.27 to
  $0.44 across sweep + images. Budget by `GET /api/v1/credits`, not by
  summing response usage.
- **Credit pre-check vs in-flight requests.** With a low balance and 6
  workers, OpenRouter rejects with "would exceed your available credits given
  your current in-flight requests" long before the balance is actually gone.
  Map that error to `pending` (our condition), never `error:provider`.
- **Free-tier daily allowance** (~1000 requests/day with $10+ purchased) is
  shared across ALL zero-priced models: about 19 models' worth of measurement
  per day. Detect the first free-model rate-limit and skip the rest of the
  zero-priced queue for the day; the checkpoint resumes tomorrow.
- **Suffix variants (`:free`, `:batch`) share the base tokenizer.** Never
  spend on them; the identity pipeline makes them inherit the base assessment.
- **Images: the images route, not chat.** `openai/gpt-5.4-image-2` via
  `/api/v1/images/generations` = $0.012/image (16:9 honoured). Nano Banana 2
  = $0.103. Measured 21 Aug.

## Product rules that are code, not copy

Confidence is rule-based (very high = exact fingerprint match + independent
API-surface corroboration + no contradiction), the claim ceiling is FAMILY
consistency, "confirmed" never derives from fingerprints, mismatches ship with
candidate explanations, controls (positive / negative / consistency) gate
publication and render live on `/ai/models/methodology/`, history is
append-only. All enforced in `build-identity.mjs`; a model's own declared tag
never votes on the verdict about itself.
