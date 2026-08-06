---
tags: [process, roomcare]
source: [docs/Security-and-Compliance.md]
updated: 2026-08-06
---

# Security Architecture (RoomCare)

How [[RoomCare]] is actually built to be safe, and the specific attacks it was designed against. This is the engineering half of `docs/Security-and-Compliance.md`; the buyer-facing half is [[UK Compliance Position (RoomCare)]]. Every principle here is verifiable in the codebase or the schema — that is the point of writing them down.

## The principles that matter most

The source doc lists sixteen. These are the ones that decide arguments:

- **Policy enforcement at the data layer.** Authorisation lives in the database as row-level security policies and SQL guards, never only in client code. A modified or malicious app build gains nothing, because the database refuses out-of-scope reads and writes whatever the app asks for. See [[RLS and Schema Change Process]].
- **Defence in depth.** Three independent layers must all fail before data leaks: application checks in shared code, security-definer database functions that re-validate every write, and row-level security on every read.
- **Least privilege, everywhere.** The anonymous role holds zero direct table privileges; devices reach only token-verified functions scoped to their own room.
- **Fail closed.** Unknown voice intents collapse to a human handoff; unrecognised lifecycle transitions are refused; unknown callers get one uninformative error; **missing configuration means demo data, never accidental live data**.
- **Data minimisation by design.** Stored: first names, room labels, tidied request text, consent choices, and media the family chose to send. Never stored: raw audio (speech is processed transiently and discarded), surnames beyond accounts, medical data of any kind, location traces, or behavioural analytics of residents. *What is never collected can never be breached.*
- **Immutability of the record.** `request_events` and `audit_log` are append-only in the grants themselves — no UPDATE or DELETE privilege exists for any client role. An undo is a recorded correction event pointing at what it voids. History is tamper-evident by construction, for families, managers and inspectors alike.
- **Secrets hygiene.** Only the public anon key ships in app bundles and it grants nothing alone. Pairing codes and device tokens are single-use or revocable and stored only as SHA-256 hashes: **a database dump reveals no usable credential.**
- **Capability-expiring media access.** Photos and voice notes live in a private bucket with no public URL space. Every view is a signed URL that expires in 60 minutes — so an intercepted or forwarded link goes dead within the hour, while the media itself stays permanently in the resident's gallery.
- **Separation of demo and production.** A compile-time mode flag (`demo | pilot | production`) decides whether simulated staff, example data and the demo PIN exist at all. Demonstration shortcuts are **structurally absent** from real-home builds, not merely hidden.
- **Human-honest interface as a security property.** The interface never claims an event that has not happened: no invented staff names, no premature "help is on its way", no "delivered" before the server confirms. *Truthful state is the defence against the most damaging failure in care, which is misplaced reassurance.*
- **Auditability of the machine.** The orchestrator (intent service) may only act through the same recorded-event lifecycle as a person, attributed as `system`, with every model call logged with input hash, chosen intent and confidence. It is constrained by an allowlist enforced in code around the model, **never by prompt alone**; prompt-injection attempts fail closed into a human handoff.

## The threat model, in short

The source doc carries a fifteen-row table. The shape of the answers:

| Threat | Why it fails |
|---|---|
| Stolen bedside tablet | Holds only a revocable, server-hashed token scoped to its own room; a manager revokes it in one action. No staff or family credentials on it, ever. |
| Curious or malicious staff account | Row-level security confines it to its own home; family media is unreadable by ordinary staff; every action lands in the append-only audit under its own identity. |
| Compromised family account | Reaches exactly one resident, within that resident's consent scope, uploading only to its own folder. Never care operations, never another resident, never anything the resident paused. |
| Tampered or reverse-engineered app build | Nothing to find — only the anon key inside, and every read and write is re-authorised at the database. |
| Intercepted photo link | Signed URLs die in 60 minutes and grant one object, not the bucket. |
| Replayed API calls | Client-generated idempotency ids and event-history validation return the original result or a refusal. |
| Prompt injection at the voice layer | Speech is data, never instructions; output must parse as an allowlisted intent or it becomes a human handoff. **Capability lives in code, not in the prompt.** |
| Enumeration and scraping | Non-enumerable errors — wrong scope, wrong state and missing rows all return one fixed refusal, so nothing can be probed. |
| Denial of service | **By design the physical nurse call is untouched by any RoomCare outage, so no availability failure can endanger a resident.** |
| Ransomware or data loss | Daily platform backups, point-in-time recovery, infrastructure reproducible from version-controlled migrations. *Recovery is a restore, never a negotiation.* |

## Who can do what

- **Residents never authenticate.** The tablet is paired once by a manager with a single-use code (burned on redemption, stored hashed) yielding a room-scoped revocable token. Anything administrative on that screen demands staff mode.
- **Staff** sign in with email plus passkey (preferred) or password. Roles (`staff`, `manager`) are enforced in the database, not the interface. Staff mode on a bedside tablet reverts to resident mode after 5 minutes idle, and covers setup only — **it never opens the family shelf**.
- **Family** join only by manager-issued invite bound to one resident, then authenticate with passkey or password. Their capability *is* the resident's consent, re-checked server-side on every operation including at notification time.
- **The orchestrator (intent service)** acts as `system` through the same guarded functions with full audit. Cloud processing runs in UK/EU with retention off; a fully-local model option for homes that require data never to leave the building sits behind the same boundary.
- **RoomCare administrators (Jay)** use the provider dashboard with multi-factor authentication; access is logged; production data access follows a written need-to-touch rule.

## The media lifecycle, precisely

1. A family contact uploads within consent; the object lands in a private bucket under the resident's folder; a virus scan gates availability (pilot setup); size limits apply — photo 10 MB, audio 5 MB.
2. The item **waits on the shelf**. Nothing auto-plays. The sender can take it back while it is unopened.
3. When the resident opens it, that moment is recorded — the truthful "opened at" the family sees — and the item **stays in the resident's gallery permanently**.
4. Every view, forever, is served through a fresh signed URL expiring in 60 minutes. **Persistence belongs to the resident; expiry belongs only to the link.**
5. Deletion: by the resident, by the sender while unopened, or by retention (placement end + 30 days). The object and record go; the audit keeps only the anonymised fact that an item existed.

## Related
- [[UK Compliance Position (RoomCare)]] — what this architecture lets Jay claim to buyers
- [[RLS and Schema Change Process]] — the rule that keeps it true for every future schema change
- [[RoomCare]] · [[Supabase Stack Pattern]] · [[Claude Operating Profile - RoomCare]] · [[Decisions - RoomCare]]
- [[Map - Processes]] · [[Home]]
