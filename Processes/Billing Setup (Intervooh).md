---
tags: [process, intervooh]
source: docs/SETUP-BILLING.md
updated: 2026-08-06
---

# Billing Setup (Intervooh)

Ops runbook: switch on Stripe billing for [[Intervooh]]. The code is fully wired — billing goes live the moment the secrets exist. Until then, upgrade buttons say "Billing is not configured yet" and the app keeps working free.

**Money flow:** customer → Stripe Checkout (Stripe's hosted payment page: cards + Apple/Google Pay) → Stripe pays out to the Revolut Business account (its GBP sort code + account number set as the payout account in Stripe). No PayPal needed.

## The runbook

- [ ] **Create the Stripe account** at dashboard.stripe.com (sole trader is fine to start); complete identity + payout details (Revolut Business).
- [ ] **Create two products** in the catalogue and copy each price id:
  - `Intervooh Everything` — recurring £16.00/month → `STRIPE_PRICE_MONTHLY`
  - `Intervooh One Interview Pass` — one-off £29.00 → `STRIPE_PRICE_PASS`
- [ ] **Copy the secret API key** → `STRIPE_SECRET_KEY` (use the test-mode key while testing).
- [ ] **Add the webhook** (this is what keeps plans in sync): endpoint `https://<project-ref>.supabase.co/functions/v1/billing`, events `checkout.session.completed`, `customer.subscription.updated`, `customer.subscription.deleted`; copy the signing secret → `STRIPE_WEBHOOK_SECRET`.
- [ ] **Enable the customer portal** (Stripe Settings → Billing) so "Manage billing" works: cancelling, updating cards, viewing invoices.
- [ ] **Add the secrets in Supabase** (Project Settings → Edge Functions → Secrets): `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `STRIPE_PRICE_MONTHLY`, `STRIPE_PRICE_PASS`, optional `SITE_URL`, and `SUPABASE_SERVICE_ROLE_KEY` (needed by the billing webhook and account deletion).
- [ ] **Run the database migration** `supabase/migrations/0002_billing_and_caps.sql` (SQL editor or `supabase db push`). It adds the pro-until and Stripe-customer columns, AI cost tracking, and the per-minute burst counter. **The AI budget limits only bite once this has run** — until then the function falls back to the old calls-only cap.
- [ ] **Test before going live**: test-mode key + test price ids + test webhook; pay with card `4242 4242 4242 4242`; watch the plan flip to `pro`; cancel in the test portal; watch it flip back. Then swap the four secrets to live values.

## What the plans enforce (server-side)

| | Free | Pro (£16/mo or £29 pass) |
|---|---|---|
| Daily AI budget | ~$0.25 (a real taste) | ~$2.50 ≈ 3 h of coaching |
| Lifetime free taste | ~$1.50 total | — |
| Monthly ceiling | — | ~$25 |
| Burst limit | 4 calls/min | 8 calls/min |

All tunable via `AI_*` environment secrets without redeploying.

## VAT note (UK)

No VAT registration needed until ~£90k turnover, so at launch prices are simply prices. If registering (or selling meaningfully into the EU, where digital-services VAT applies from the first sale): turn on Stripe Tax and set both prices tax-inclusive.

## Related

- [[Map - Processes]] · [[Intervooh]] · [[Supabase Stack Pattern]]
- Companion runbook: [[Accounts and AI Switch-On (Intervooh)]] (billing rides on the same edge-function setup)
- Pricing decision history: [[Decisions - Intervooh]]
