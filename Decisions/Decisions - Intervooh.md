---
tags: [decision, intervooh]
source: docs/DECISIONS.md
updated: 2026-08-06
---

# Decisions - Intervooh

Running log of the ten launch-shaping questions from the PRD (§18) plus the M1 architecture decision. The PRD stays frozen; this records what got decided and when. Business: [[Intervooh]].

## The ten launch questions

| # | Question | Decision | Date |
|---|---|---|---|
| 1 | Name + domain | **Intervooh — intervooh.com** (bought on GoDaddy). Logo landed: two chairs, navy + mint. Accent colours now derive from it: mint `#4FB393`, deepened `#1B7C5E` for light mode, ink navy `#1B2A44` | 17 Jul 2026 |
| 2 | First audience wedge (graduates vs all professionals) | *Open* | |
| 3 | Launch pricing | **£16/month subscription** + free tier (setup, programme, questions-to-expect, daily quick-fire). Supersedes the PRD's £19-pack / £12-month debate. Stripe wiring pending — see [[Billing Setup (Intervooh)]] | 18 Jul 2026 |
| 4 | Role families for launch (pick 5) | *Superseded in practice*: the jobs database ships **all 200 titles across 24 sectors**; trimming is just a data edit. See [[Question Database (Intervooh)]] | 17 Jul 2026 |
| 5 | Technical drills depth at v1 | *Open* | |
| 6 | Voice at launch or M2 | **Voice input at launch** — browser speech recognition transcribes answers on-device, typing always available. **AI-scored voice mocks also at launch.** Founder asked for real mic listening and full AI; delivered | 18 Jul 2026 |
| 7 | Company packs (model knowledge vs wait for live web research) | *Open* | |
| 8 | Free tier size | *Open* | |
| 9 | Report sharing (private-only vs PDF export) | *Open* | |
| 10 | 16–18 school leavers | *Open* — PRD recommends adults-only at launch (keeps compliance light; no Children's Code) | |

## M1 — accounts + AI architecture (18 Jul 2026)

The load-bearing decision: the app ships **fully working in local/device mode** — no accounts, no AI — and lights up accounts + the AI coach/mocks **only when environment variables are present**. Free experience stays instant and offline-first; the paid, bespoke layer is opt-in per deployment.

- **Accounts + data** → Supabase (auth, Postgres with owner-only row-level security, private CV storage), **London region (eu-west-2)** to keep UK data in the UK. Local data syncs up on first sign-in; cloud wins when it has content. This is the [[Supabase Stack Pattern]] shared with [[MyHomework]].
- **AI** → **Google Gemini** (`gemini-2.5-pro` at decision time) — chosen because it was the funded, ready API; the proxy is provider-agnostic and can be repointed. Called **only** by a single Supabase edge function (server-side program), never from the browser. The Gemini key is a server-side secret; only the Supabase URL and publishable key are public (row-level security protects the data).
- The function enforces a per-user daily cap and runs two guards: a **fabrication guard** (the coach can't invent experience for the user) and an **emotion-inference guard** (feedback stays on the behavioural rubric).
- Founder turn-on steps: [[Accounts and AI Switch-On (Intervooh)]].

## Related

- [[Map - Decisions]] · [[Intervooh]] · [[Research - Intervooh Evidence Base]]
