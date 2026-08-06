---
tags: [process, cross]
source: [CLAUDE.md, docs/Security-and-Compliance.md, docs/Connect-The-Backend.md]
updated: 2026-08-06
---

# RLS and Schema Change Process

Jay's rule for touching any database, written down hardest in [[RoomCare]] but applied everywhere he uses the [[Supabase Stack Pattern]] ([[MyHomework]], [[Intervooh]] included). RLS is row-level security: the database itself decides which rows each signed-in person may see, so a bug or a tampered app cannot leak data.

## The two rules (verbatim, from RoomCare's CLAUDE.md)

1. **"Every schema change ships with Row Level Security policies and a test that a wrong-home or wrong-consent read fails."** The change and its proof travel together; a migration without a failing-read test is not done.
2. **"Append-only tables (`request_events`, `audit_log`) get INSERT policies only; no UPDATE or DELETE, ever."** History can be added to, never edited. An undo is a recorded correction event that voids the original; nothing is quietly rewritten. This is what makes the audit trail worth showing a family or an inspector.

## The process for any schema change

- [ ] Write the migration as a reviewed file in version control (never ad-hoc console edits).
- [ ] Write the RLS policies in the same migration: staff see only their home, family only their resident within consent, devices only their own room.
- [ ] Write the negative test: a wrong-home read and a wrong-consent read must **fail**. A test that only proves the right person can read proves nothing.
- [ ] Append-only tables: grant INSERT only. Check no UPDATE or DELETE privilege exists for any client role.
- [ ] Run the proof before real data enters. RoomCare's live proof (12 July 2026) refused all seven simulated attacks against the production database, including anonymous reads on every table and edits to the append-only history.

## Why enforcement lives in the database

Three layers must all fail before data leaks: app checks in shared code, guarded database functions that re-validate every write, and RLS on every read. Authorisation never lives solely in client code, because a modified client gains nothing when the database refuses out-of-scope reads regardless of what the app asks for. Fail closed: when in doubt, the system declines and a person decides.

## Related

- [[Map - Processes]] · [[RoomCare]] · [[MyHomework]] · [[Intervooh]]
- [[Supabase Stack Pattern]] · [[Claude Operating Profile - RoomCare]]
- [[Deploy and Backend Runbooks (RoomCare)]] (where the proof is run)
