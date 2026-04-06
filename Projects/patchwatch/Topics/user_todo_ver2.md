# PatchWatch — Deployment Checklist (v2)

Everything from domain registration to production-ready, in order.

**Already done:** Supabase project, Google OAuth, Copernicus credentials, Resend API key, Stripe products/prices, cron secret, all DB migrations, local dev working.

---

## Step 1: Buy Domain on Cloudflare

1. Go to [dash.cloudflare.com](https://dash.cloudflare.com) and create an account (free)
2. Go to **Domain Registration > Register Domains**
3. Search for your domain (e.g., `patchwatch.earth`, `moonleaflab.com`, or another)
4. Purchase it — Cloudflare charges at-cost, ~$10-12/year for `.com`
5. The domain is now automatically using Cloudflare DNS (no transfer needed)

> **Decision:** If you want one domain for all future projects, buy `moonleaflab.com` and use subdomains (`patchwatch.moonleaflab.com`). If PatchWatch is standalone, buy `patchwatch.earth` or similar.

---

## Step 2: Set Up Cloudflare Email Routing

This lets you receive email at `anything@yourdomain.com` forwarded to your Gmail — free.

1. In Cloudflare dashboard, select your domain
2. Go to **Email > Email Routing**
3. Click **Get started** if first time
4. Under **Destination addresses**, add `moonleaf.tao@gmail.com` and verify it (check Gmail for confirmation link)
5. Under **Routing rules**, enable **Catch-all** → forward to `moonleaf.tao@gmail.com`
6. Cloudflare will auto-add the required MX and TXT DNS records

**Test it:** Send an email to `test@yourdomain.com` and confirm it arrives in Gmail.

---

## Step 3: Set Up Gmail Filters

1. In Gmail, go to **Settings > Filters and Blocked Addresses > Create a new filter**
2. In the "To" field, enter `*@yourdomain.com`
3. Click **Create filter**, check **Apply the label**, create label "PatchWatch"
4. Optionally create sub-filters per service (e.g., `supabase@yourdomain.com` → "PatchWatch/Supabase")

---

## Step 4: Push to GitHub

1. Create a new repository on [github.com](https://github.com) (private recommended)
   - Name: `patchwatch`
   - Don't initialize with README (you already have code)
2. Verify `.env.local` is in `.gitignore`:
   ```bash
   cat .gitignore | grep env
   ```
3. Add remote and push:
   ```bash
   git remote add origin git@github.com:YOUR_USERNAME/patchwatch.git
   git branch -M main
   git push -u origin main
   ```

> **Security check:** Before pushing, run `git diff --cached` and `git status` to make sure no `.env.local` or secret files are staged.

---

## Step 5: Deploy to Vercel

1. Go to [vercel.com](https://vercel.com) and sign in (use GitHub account)
2. Click **Add New > Project**
3. Import your `patchwatch` GitHub repo
4. Framework preset: **Next.js** (auto-detected)
5. **Add environment variables** — copy each from your `.env.local`:

   | Variable | Value | Notes |
   |----------|-------|-------|
   | `NEXT_PUBLIC_SUPABASE_URL` | `https://xrbe...supabase.co` | |
   | `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY` | your key | |
   | `SUPABASE_SECRET_KEY` | your key | |
   | `RESEND_API_KEY` | your key | |
   | `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` | `pk_test_...` | Switch to `pk_live_` for production |
   | `STRIPE_SECRET_KEY` | your key | Switch to `sk_live_` for production |
   | `STRIPE_PRO_PRICE_ID` | `price_...` | |
   | `STRIPE_TEAM_PRICE_ID` | `price_...` | |
   | `STRIPE_WEBHOOK_SECRET` | (leave empty for now) | Set in Step 7 |
   | `COPERNICUS_CLIENT_ID` | your ID | |
   | `COPERNICUS_CLIENT_SECRET` | your secret | |
   | `CRON_SECRET` | your hex string | |
   | `NEXT_PUBLIC_APP_URL` | (leave empty for now) | Set in Step 6 |

6. Click **Deploy**
7. Wait for build to complete — Vercel gives you a URL like `patchwatch-xyz.vercel.app`
8. Test the deployed app at that URL

---

## Step 6: Connect Domain to Vercel

1. In **Vercel > Project Settings > Domains**, add your domain:
   - If standalone domain: `patchwatch.earth` (or whatever you bought)
   - If subdomain: `patchwatch.moonleaflab.com`
2. Vercel will show DNS records to add. Go to **Cloudflare DNS**:
   - For apex domain (`patchwatch.earth`): Add **A** record → `76.76.21.21` (Vercel's IP)
   - For subdomain (`patchwatch.moonleaflab.com`): Add **CNAME** → `cname.vercel-dns.com`
   - **Important:** In Cloudflare, set the proxy status to **DNS only** (gray cloud, not orange) for Vercel domains
3. Back in Vercel, click **Verify** — should confirm within minutes
4. Vercel automatically provisions an SSL certificate
5. Update the Vercel environment variable:
   ```
   NEXT_PUBLIC_APP_URL=https://patchwatch.yourdomain.com
   ```
6. Redeploy (Vercel auto-redeploys on env var change, or push a commit)

**Test it:** Visit `https://patchwatch.yourdomain.com` — should load with HTTPS.

---

## Step 7: Set Up Stripe Webhook

Now that you have a live URL:

1. Go to **Stripe Dashboard > Developers > Webhooks**
2. Click **Add endpoint**
3. Endpoint URL: `https://patchwatch.yourdomain.com/api/billing/webhook`
4. Select events:
   - `checkout.session.completed`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
5. Click **Add endpoint**
6. Copy the **Signing secret** (`whsec_...`)
7. In Vercel: add environment variable `STRIPE_WEBHOOK_SECRET` = the signing secret
8. Redeploy

**Test it:** In Stripe Webhooks, click **Send test webhook** → `checkout.session.completed`. Check Vercel function logs for success.

---

## Step 8: Update Google OAuth Redirect URI

1. Go to [Google Cloud Console > APIs & Services > Credentials](https://console.cloud.google.com/apis/credentials)
2. Click your OAuth 2.0 Client ID
3. Under **Authorized redirect URIs**, add:
   ```
   https://xrbedzjihhkciaruxxgq.supabase.co/auth/v1/callback
   ```
   (This should already be there from dev setup)
4. Also update the **Authorized JavaScript origins** if listed — add your production domain
5. In **Supabase > Authentication > URL Configuration**, update:
   - **Site URL**: `https://patchwatch.yourdomain.com`
   - **Redirect URLs**: add `https://patchwatch.yourdomain.com/**`

---

## Step 9: Verify Domain in Resend (Email Delivery)

Currently emails send from `alerts@patchwatch.app` — this will fail in production without domain verification.

1. Go to [resend.com](https://resend.com) > **Domains** > **Add Domain**
2. Enter your domain (e.g., `patchwatch.yourdomain.com` or the root domain)
3. Resend shows DNS records to add (SPF, DKIM, DMARC)
4. Add all records in **Cloudflare DNS**
5. Click **Verify** in Resend — takes up to 72 hours but usually minutes
6. Update the `from` address in `src/lib/email/send.ts` line 39:
   ```typescript
   from: "PatchWatch <alerts@yourdomain.com>",
   ```
7. Commit and push to trigger redeploy

---

## Step 10: Switch Stripe to Live Mode (When Ready for Real Payments)

**Do this only when you're ready to accept real money.**

1. In Stripe Dashboard, toggle from **Test mode** to **Live mode**
2. Complete Stripe's activation checklist (business info, bank account)
3. Create live products and prices mirroring your test ones
4. Update Vercel env vars:
   - `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` → `pk_live_...`
   - `STRIPE_SECRET_KEY` → `sk_live_...`
   - `STRIPE_PRO_PRICE_ID` → new live price ID
   - `STRIPE_TEAM_PRICE_ID` → new live price ID
5. Add a new webhook endpoint for live mode (same URL, new signing secret)
6. Update `STRIPE_WEBHOOK_SECRET` with the live webhook secret
7. Redeploy

---

## Step 11: Choose Image Storage (Before Real Satellite Scans)

Currently satellite imagery uses base64 data URLs (placeholder). For production:

**Recommended: Cloudflare R2** (since you already have Cloudflare)
- Free egress (no bandwidth charges), 10 GB free storage
- S3-compatible API
- Create a bucket in Cloudflare Dashboard > R2
- Generate API tokens with read/write access to that bucket only

Alternatives: Vercel Blob (simpler, $0.15/GB), AWS S3 (most features, complex pricing).

Add credentials to Vercel env vars once chosen. Claude can wire the storage integration when you're ready.

---

## Step 12: Security Hardening Before Launch

- [ ] Enable 2FA on: ~~Cloudflare~~, ~~GitHub~~, ~~Vercel~~, Stripe, ~~Supabase~~, Google Cloud, ~~Resend~~
- [x] Use a password manager for all service credentials
- [ ] In Supabase, review RLS policies — run a few test queries as an anonymous user to confirm they block unauthorized access
- [x] In Stripe, use restricted API keys with minimal permissions for production
- [x] In Google Cloud, remove `localhost` from OAuth redirect URIs in the production client
- [x] Run `git log --all --diff-filter=A -- '*.env*'` to confirm no secrets were ever committed
- [ ] Set up Vercel environment variable scoping: production secrets only in Production environment, test keys in Preview/Development

---

## Quick Reference: What's Working Now vs. What Needs Steps Above

| Feature | Status |
|---------|--------|
| Zone creation & map | Working locally |
| Google sign-in | Working locally |
| Stripe test payments | Working locally |
| Custom alert rules | Working locally (needs migration 002 — already done) |
| API keys & REST API | Working locally (needs migration 003 — already done) |
| Team workspaces | Working locally (needs migration 003 — already done) |
| Change detection | Working locally (synthetic data) |
| Email alerts | Working locally (Resend test domain) |
| Cron scans | Need Vercel deployment (Step 5) |
| Stripe webhooks | Need deployed URL (Step 7) |
| Real satellite imagery | Need image storage (Step 11) |
| Production payments | Need Stripe live mode (Step 10) |
