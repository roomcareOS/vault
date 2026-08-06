---
tags: [inbox, cross]
source: Jay's request, 6 August 2026
updated: 2026-08-06
---

# Your Next Actions (6 August)

Step by step, in the order that clears the most. **Todoist still owns the state** — tick the cards there as you go; this is just the how-to.

Rule throughout: **never paste a secret value into chat.** Where I need one, put it in the place named and tell me the *name*, not the value. The two exceptions are marked.

---

## 1. Cloudflare — R2 credentials and the media domain

**Why:** unblocks the podcast launch, the 79-file audio cutover, off-site backups, and gives the Seedance footage somewhere to live. The free week opens tomorrow.

### 1a. Create the S3 credentials

1. **dash.cloudflare.com** → sign in.
2. Left sidebar → **R2 Object Storage** → **Overview**.
3. Top right of that page → **Manage API tokens**. (Not "Create bucket".)
4. **Create API token**.
5. **Token name:** `yfarmx-audio-rw`
6. **Permissions:** choose **Object Read & Write**. *Not* Admin, *not* Read only.
7. **Specify bucket(s):** select **only** `yfarmx-audio`.
8. **TTL:** leave as forever, or set 1 year.
9. **Create API Token**.
10. The next screen shows **Access Key ID** and **Secret Access Key**, and it shows the secret **once only**.

**What to do with them:** go to **github.com/roomcareOS/yfarmx → Settings → Secrets and variables → Actions → New repository secret**, and add two:
- Name `R2_ACCESS_KEY_ID`, value = the Access Key ID
- Name `R2_SECRET_ACCESS_KEY`, value = the Secret Access Key

Then tell me **"R2 secrets are in"**. I never need to see the values.

> **Why this and not a "Workers R2 Storage" token:** that other kind is for wrangler and the Cloudflare API. Our upload script talks to R2's S3-compatible endpoint, which needs an Access Key ID and Secret instead. Picking the wrong one is the single most common way this task gets done twice.

### 1b. Bind media.yfarmx.com

1. **R2 Object Storage** → click the **yfarmx-audio** bucket.
2. **Settings** tab.
3. Find **Custom Domains** → **Connect Domain**.
4. Enter `media.yfarmx.com` → **Continue** → **Connect domain**.
5. Cloudflare adds the DNS record itself. **Wait for the status to go green** (usually a minute or two).

**Do NOT use "Public Development URL"** on the same page. Cloudflare's own docs say the `r2.dev` URL is rate-limited, development-only and has no caching. Podcast apps poll enclosure URLs hard and it would fall over.

### 1c. CORS, while you are on that page

1. Still in the bucket's **Settings**, find **CORS Policy** → **Add CORS policy**.
2. Allowed origins: `https://yfarmx.com`
3. Allowed methods: tick **GET** and **HEAD** only.
4. Save.

Skip R2 Data Catalog, Object Lifecycle Rules, Bucket Lock and Default Storage Class — none apply.

**How you know it worked:** `media.yfarmx.com` shows green under Custom Domains. Tell me and I will verify a file end to end before we move the other 78.

---

## 2. Cloudflare — the staging password

**Why:** staging fails closed right now, so nobody can see it, including you. It also blocks promoting the Space hub.

1. **dash.cloudflare.com** → **Workers & Pages** → click **yfarmx**.
2. **Settings** tab.
3. **At the top there is a "Choose Environment" selector. Switch it from Production to → Preview.** This is the step people miss. Staging deploys into Preview; setting it on Production does nothing.
4. **Variables and secrets** → **Add**.
5. Type: **Secret** (not Plaintext). Name: `STAGING_PASSWORD`. Value: a password of your choosing.
6. **Save**.

Then open `https://staging.yfarmx.pages.dev` on phone, Chromebook and PC and sign in once on each — the cookie lasts 30 days. Save the password in your password manager (there is a Todoist card for that).

---

## 3. Google Search Console — the DNS verification

**Why:** the biggest business impact on the board. Google still serves a snapshot of the old WordPress site; roughly 100 articles published since 18 July are not indexed. The old verification was a WordPress plugin tag that died at cutover.

1. **search.google.com/search-console** → sign in.
2. Property selector (top left) → **Add property**.
3. Choose the **Domain** box on the left, **not** "URL prefix". Enter `yfarmx.com` → **Continue**.
4. It shows a **TXT record** to add. Copy the value.
5. In another tab: **dash.cloudflare.com** → **yfarmx.com** → **DNS** → **Records** → **Add record**.
   - Type: **TXT**
   - Name: `@`
   - Content: paste the Google value
   - Proxy status: not applicable for TXT
   - **Save**
6. Back in Search Console → **Verify**. If it fails, wait five minutes and try again.

### Then, immediately after verifying

7. Left menu → **Sitemaps**. Add both, one at a time:
   - `sitemap-index.xml`
   - `news-sitemap.xml`
8. Top search bar → **URL inspection** → paste a URL → **Request indexing**. The daily quota is roughly 10–12, so spend it deliberately, in this order:
   `https://yfarmx.com/` , `/ai/` , `/crypto/` , `/quantum/` , `/tools/exploit-tracker/`, then your five best recent articles.

**Send me** a screenshot of **Indexing → Pages** once it has data (give it 48 hours) and I will work the 404 and soft-404 lists.

---

## 4. The Seedance shot list

**Why:** the free week runs Fri 7 – Thu 13 Aug and cannot be extended.

**This one is mine, not yours** — I can draft the whole list from the existing plans (`docs/tiktok-plan/`, the RoomCare and Intervooh film scripts, `docs/media-storage.md`). It would cover yfarmx brand loops, Norwich Drones intro/outro, RoomCare films four and five, the Intervooh re-renders, and whether myhomework wants anything at all.

**Just say "draft the Seedance shot list"** and you will have it before the window opens.

---

## 5. Stripe — Intervooh billing

**Why:** the code is already written and tested. This is the fastest route to revenue anywhere in the estate.

Full detail is in `docs/SETUP-BILLING.md`. **Do it in test mode first** — the doc walks it through.

1. **dashboard.stripe.com** → make sure the **Test mode** toggle (top right) is ON.
2. **Product catalogue** → **Add product**:
   - Name: `Intervooh Everything` · Price: **£16.00** · Recurring, **monthly** · Save.
   - **Copy the price ID** (starts `price_...`).
3. **Add product** again:
   - Name: `Intervooh One Interview Pass` · Price: **£29.00** · **One-off** · Save.
   - **Copy the price ID.**
4. **Developers → Webhooks → Add endpoint.**
   - URL: your Supabase billing function URL (I will give you the exact URL — ask me).
   - Events: `checkout.session.completed`, `customer.subscription.updated`, `customer.subscription.deleted`.
   - Add endpoint, then **copy the Signing secret** (starts `whsec_...`).
5. **Developers → API keys** → copy the **Secret key** (`sk_test_...` in test mode).

**Where they go:** Supabase → your Intervooh project → **Project Settings → Edge Functions → Secrets**, add four:
`STRIPE_SECRET_KEY` · `STRIPE_WEBHOOK_SECRET` · `STRIPE_PRICE_MONTHLY` · `STRIPE_PRICE_PASS`

Then tell me **"Stripe test mode is wired"** and I will run the rehearsal: pay with a test card, watch the plan flip to Pro, cancel in the portal, watch it flip back. When that passes, repeat steps 1–5 with **Test mode OFF** and swap the four values to live.

**Depends on:** task 6b below (the service role key) — the webhook throws without it.

---

## 6. Supabase — migrations and switches

You have three separate projects. **Check the project name at the top left before running anything.**

### 6a. The migrations

For each one: **SQL Editor → New query → paste the whole file → Run**.

| Project | File (from the repo) | What it turns on |
|---|---|---|
| `roomcare-pilot` | `supabase/migrations/0003_full_logs_and_hardening.sql` (v1 repo) | the full audit ledger, manager unpair, pairing throttle |
| Intervooh's project | `supabase/migrations/0002_billing_and_caps.sql` (interviewprep repo) | real spend budgets, and the £29 pass being able to grant Pro |
| myhomework's project | `supabase/migrations/0006_delete_account.sql` (myhomework repo) | the "Delete everything" button actually deleting |

None can damage existing data — they add functions, triggers and columns. **Run 0002 even if you think it is done**; the AI function degrades quietly without it, so you would see weaker limits rather than an error.

### 6b. The service role key (Intervooh)

1. Intervooh project → **Project Settings → API**.
2. Copy the **service_role** key (starts `sb_secret_`). **This one is powerful — never paste it into chat.**
3. **Project Settings → Edge Functions → Secrets → Add**: name `SUPABASE_SERVICE_ROLE_KEY`, paste the value.

Without it the Stripe webhook throws and account deletion returns "temporarily unavailable".

### 6c. The auth switches

**roomcare-pilot** → **Authentication → Sign In / Up**:
- **User Signups:** turn **OFF** "Allow new users to sign up" (staff and family are invited, never self-serve).
- **Passwords:** minimum length **12**, and turn **ON** "Prevent use of leaked passwords".

**Intervooh's project** → **Authentication**:
- **Attack Protection** → enable **CAPTCHA** → provider **Cloudflare Turnstile** → paste the Turnstile secret (get it from Cloudflare → Turnstile → Add site). Send me the **site key** (that one is public and safe to share) so I can wire it into the sign-up form.
- **Sign In / Providers → Email** → confirm **"Confirm email"** is ON. Tell me either way — I cannot read this from the code.

### 6d. Paste the updated myhomework AI function

1. myhomework project → **Edge Functions** → **ai**.
2. Select all the existing code and replace it with the contents of `supabase/functions/ai/index.ts` from the myhomework repo (open on GitHub, click **Raw**, copy all).
3. **Deploy**.

Safe to paste over — the safety rules verified on 18 July are all still in it.

---

## 7. Vercel — unblock myhomework

**Why:** the live site is frozen on 18 July. Three deploys were rejected before the build even started.

1. **vercel.com** → switch to the team called **RoomCare** (slug `room-care`) using the scope selector, top left.
2. Check the **notification bell**, top right.
3. **Settings → Billing.**

Vercel usually explains a block in one line — a plan limit, a payment needing attention, or a team that now needs Pro. **Tell me exactly what it says** and I will deploy the moment it clears.

**Worth knowing before you decide:** you said before you did not want myhomework paying for a RoomCare team. Moving the project to your personal scope is free, but the `myhomework.app` domain has to be detached and reattached, so the site goes briefly down. Your call.

---

## 8. Vercel — intervooh.com as primary

**Why:** the apex returns 503 while every canonical tag and all 230 sitemap URLs point at it. Google is being told the canonical URL is one it gets bounced off.

1. **vercel.com** → the **intervooh** project → **Settings → Domains**.
2. Find `intervooh.com` (the apex, no www) → its **⋯** menu → **Set as primary domain**.
3. `www.intervooh.com` should flip to redirecting to it.
4. Open `https://intervooh.com` in a browser and confirm it loads rather than 503s.

---

## 9–11. The approvals

- **The 60-video TikTok plan.** Branch `claude/wifi-max-tiktok-plan-wzkahd`, folder `docs/tiktok-plan/`. **Read the README first** — 30-day schedule and production standards. Then say approve, or what to change. Nothing renders until you do.
- **Film three's rewrite.** The full replacement script is in `marketing/films/PLAN.md` under "PENDING REVISION". Your two notes were: say "resident" not "her", and explain it to someone unfamiliar. Reply approve, or say what a stranger would still not understand.
- **Podcast cover art, 3000×3000.** Apple needs square, at least 1400×1400. The current brand master is 1254×1254 — just under, and I will not upscale it into something soft. Send me a 3000×3000 PNG, or say "regenerate it" and I will produce one from the brand cube.

---

## 12. Security — the key rotations

- **OpenRouter:** openrouter.ai → **Settings → Keys** → revoke the current key, create a new one. **Put it straight into GitHub → roomcareOS/yfarmx → Settings → Secrets and variables → Actions → `OPENROUTER_API_KEY`.** Do not send it to me in chat.
- **Also while you are there:** **Settings → Credits → turn auto top-up OFF** before you buy the £1,400. An account with no auto top-up cannot spend money it does not have. Every per-key cap is a scheduling guard; this is the only real spending guard.
- **Gemini:** Google AI Studio → **API keys** → delete the exposed key. No replacement — we have dropped Google.
- **Todoist:** **Settings → Integrations → Developer → Reset API token** (your own deadline is 12 Aug). Tell me when you do, because my access stops working at that moment and I will need the new one.

---

## The multiplier: two scoped tokens

These would move tasks **1, 2, 6a and 6c** — and most of the future ones like them — off your list permanently.

### Cloudflare token

1. **dash.cloudflare.com** → profile icon (top right) → **My Profile** → **API Tokens** → **Create Token** → **Create Custom Token**.
2. Name: `claude-yfarmx`
3. Permissions — add these four rows:
   - Zone · **DNS** · **Edit**
   - Zone · **Zone Settings** · **Edit**
   - Account · **Workers R2 Storage** · **Edit**
   - Account · **Cloudflare Pages** · **Edit**
4. **Zone Resources:** Include → Specific zone → **yfarmx.com**
5. **TTL:** set an end date a week out. You can always mint another.
6. Create, then put the value in **GitHub → roomcareOS/yfarmx → Settings → Secrets and variables → Actions** as `CLOUDFLARE_API_TOKEN`, and tell me it is there.

### Supabase token

1. **supabase.com/dashboard/account/tokens** → **Generate new token** → name it `claude`.
2. Same place: GitHub Actions secret, name `SUPABASE_ACCESS_TOKEN`.

> **Honest caveat:** a token in a GitHub Actions secret is readable by anything that can run a workflow in that repo, which is me. That is the point, and it is also the risk. Both are revocable in one click, both are scoped, and I would rather you set a short expiry and re-mint than hand over anything standing. If you would prefer to keep clicking the dashboards yourself, that is a completely reasonable answer and the instructions above stand on their own.

---

## Related
- [[Session Doctrine]] · [[Home]] · [[Mega Monetisation Plan]]
- [[Media Storage and the R2 Rule (YFarmX)]] · [[Billing Setup (Intervooh)]] · [[Staging and Backups (YFarmX)]]
