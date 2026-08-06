---
tags: [process, roomcare]
source: [docs/Security-and-Compliance.md]
updated: 2026-08-06
---

# UK Compliance Position (RoomCare)

The regulatory answer [[RoomCare]] gives to a care home manager, a council procurement officer or an investor's diligence team. This is the commercial half of `docs/Security-and-Compliance.md` (v2, 12 July 2026); the engineering half is [[Security Architecture (RoomCare)]].

RoomCare Ltd, England and Wales, company no. 17317321. The whole position rests on one fact: **RoomCare is non-clinical, holds no medical records, and works alongside the nurse call, never in place of it.**

## The position, regime by regime

**UK GDPR / Data Protection Act 2018.** Lawful bases are documented: legitimate interests for care requests, consent for family sharing with the history and basis recorded. Retention is written and per-home configurable — requests 90 days then anonymised, media per the gallery lifecycle, audit 24 months, handover 12 months. Subject-access export and cascading deletion are designed into the schema rather than bolted on. **A DPIA is completed with each home before go-live**, and **ICO registration as controller is a founder action before any pilot data** — carded, and a hard gate.

**Cyber Essentials (NCSC/IASME).** The five controls are already satisfied architecturally: boundary protection, secure configuration, access control, malware protection, patch management. Certification is a purchase, not a build. **Cyber Essentials Plus adds independent testing and so doubles as the first external penetration test.** Care homes and councils increasingly require it of suppliers — which is why the [[Mega Monetisation Plan]] treats CE Plus as fundraising and procurement collateral rather than a technical need.

**Clinical safety (DCB0129 / DCB0160).** These NHS clinical-risk standards apply to health IT. RoomCare sits outside them deliberately — no diagnosis, no monitoring, no alarms, no medical records (PRD §10 and §12). If a commissioning body asks for sign-off anyway, a qualified **Clinical Safety Officer** produces the safety case. Jay's 6 August decision moved this from "only if asked" to planned spend at £1,000–£1,500, because it strengthens procurement and diligence alike. Worth knowing: clinical-safety questions also arrive from insurers and CQC-registered managers, not only council commissioners.

**CQC context.** Homes remain the controllers of care records. RoomCare never replaces the nurse call and never holds clinical data, so it stays an operational aid inside the home's existing regulatory posture rather than becoming a regulated system of its own.

**NHS DSPT.** Not required — no NHS data. The substance a customer would look for (UK residency, row-level security, audit, minimisation) already exists if one demands it.

**NCSC secure design principles.** Least privilege, defence in depth, make compromise difficult and detection easy, no security through obscurity. This is why the source document can be handed to a buyer as-is: **disclosure does not weaken the system.**

## What is not done yet, stated honestly

The source doc's own list, because an honest gap list is worth more in diligence than a claim of completeness. Each is carded.

1. **Live-database proof.** The wrong-home and wrong-consent refusal tests run the day the production database exists, before any real resident data enters. **Non-negotiable gate** — see [[RLS and Schema Change Process]].
2. **Independent penetration test.** Commissioned around pilot, with Cyber Essentials Plus as the vehicle.
3. **Device auth upgrade.** Hashed room tokens through guarded functions today; short-lived scoped sessions per device at P1, with the path already written into the SQL.
4. **Staff mode hardening.** Shared PIN today; personal passkey identification of the individual staff member, plus the 5-minute auto-revert, land at pilot setup.
5. **Edge rate limiting, EU error monitoring with personal data scrubbed, upload virus scanning.** All switched on during pilot setup — they need the live project to configure.
6. **Business continuity.** Provider Pro tier for daily backups at pilot, and a written incident-response one-pager (who telephones the home, the ICO 72-hour clock, evidence preservation from the append-only logs) **before first real data**.

## Two gaps this vault adds

Found during the monetisation review, 6 August — not in the source doc and not yet carded in full:

- **Consent where a resident lacks capacity.** Resident data is special-category data and the current consent model assumes a resident who can consent. A Mental Capacity Act best-interests process needs writing into the DPIA before a real resident uses the system.
- **Commercial paperwork.** A pilot agreement template, a pricing sheet and a data-processing agreement do not exist yet. **Nothing can be invoiced until they do.**

## The paragraph for the door of a care home

Verbatim — this is the sales asset, tested and deliberately worded:

> "Resident data lives encrypted in London and never leaves the UK/EU. The database itself, not just our apps, refuses to show anyone anything beyond their role: your staff see only your home, a family member sees only their own relative within the resident's consent, and a bedside tablet sees only its own room. Staff use of a resident's tablet happens in an audited staff mode that shuts itself after five minutes. The record of every action can be added to but never edited. Photos stay in the resident's own gallery, served through links that die within the hour. We hold no medical data at all, and RoomCare works alongside the nurse call, never in place of it."

## Related
- [[Security Architecture (RoomCare)]] — the engineering that earns these claims
- [[RoomCare]] · [[Decisions - RoomCare]] · [[RLS and Schema Change Process]] · [[Supabase Stack Pattern]]
- [[Map - Processes]] · [[Home]]
