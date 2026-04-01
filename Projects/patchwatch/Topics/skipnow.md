# Skipped Items — Needs User Action

## Completed (from Phase 3)
- ~~Stripe Product & Price IDs~~ — Done
- ~~Stripe Customer Portal Configuration~~ — Done

## Still Pending

### 1. Run Database Migrations
Run these in Supabase SQL Editor (in order): (DONE)
- `supabase/migrations/002_alert_rules.sql` — Custom alert rules table
- `supabase/migrations/003_api_keys_and_teams.sql` — API keys, teams, team members, user webhooks tables
- `supabase/migrations/004_extended_change_types.sql` — Extended change type enum + breakdown column

### 2. Stripe Webhook Secret (optional, low priority)
- Route at `/api/billing/webhook` is fully implemented
- Set up when you have a deployed URL: Stripe Dashboard → Developers → Webhooks → Add endpoint
- Events: `checkout.session.completed`, `customer.subscription.updated`, `customer.subscription.deleted`
- Add `STRIPE_WEBHOOK_SECRET` to `.env.local` / Vercel env vars
- Not blocking — plan sync works via polling on settings page

### 3. Vercel Deployment
- Push to GitHub and connect to Vercel
- Set all env vars in Vercel project settings
- Configure Vercel Cron (already defined in `vercel.json`)
