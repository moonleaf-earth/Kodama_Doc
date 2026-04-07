# PatchWatch — Manual Test Plan

Human-only tests: browser flows, visual checks, third-party dashboards.

---

## 1. Auth
- [ ] Sign up → confirmation email → profile created with `plan: free`
- [ ] Login → session persists on reload → logout clears session
- [ ] Invalid credentials / duplicate email show clear errors
- [ ] Unauthenticated visit to `/dashboard/*` redirects to login

## 2. Zone Management
- [ ] Draw polygon on map → zone saved, appears in sidebar
- [ ] Click zone in sidebar → highlights on map, detail panel correct
- [ ] Edit zone name → saved. Delete zone → gone from map + sidebar
- [ ] Plan limit hit → upgrade prompt shown (try creating zone #4 on free)

## 3. Community Map
- [ ] `/map` loads without login, shows public zones only
- [ ] No private zone data visible

## 4. Satellite Scanning
- [ ] After cron runs: new change events appear in zone history panel
- [ ] HistoryPanel timeline renders correctly, empty state handled
- [ ] Free user sees max 6 months; Pro/Team sees full history

## 5. Alerts
- [ ] Free user: alert rules UI hidden
- [ ] Pro/Team: create alert rule → rule triggers on next scan → email received
- [ ] Email contains zone name, change type, area, dashboard link

## 6. Billing (Stripe)
- [ ] Pre-launch: switch to live keys, configure `STRIPE_WEBHOOK_SECRET`, verify price IDs
- [ ] Free → Pro upgrade via Checkout → plan updates in settings
- [ ] Cancel subscription via billing portal → downgrades to free
- [ ] Stripe dashboard: webhook deliveries succeeding, no failures

## 7. Teams
- [ ] Team plan user: create team, invite member, member joins
- [ ] Non-team user: team section hidden, team API returns 403

## 8. API Keys & Webhooks
- [ ] Generate API key → shown once → masked after
- [ ] Register webhook URL → fires on change event with correct payload

## 9. Security
- [ ] `.env.local` in `.gitignore` (currently NOT — fix before launch)
- [ ] No secrets in client bundle (check page source for `sk_`, `rk_`, `sb_secret_`)
- [ ] Test cross-user data access: user A cannot see user B's zones via API

## 10. Deployment
- [ ] `NEXT_PUBLIC_APP_URL` set to `https://patchwork.moonleafearth.com`
- [ ] All env vars in Vercel dashboard
- [ ] Build succeeds, preview deploy tested before production promote
- [ ] Cron jobs visible in Vercel logs
- [ ] SSL valid, domain resolves
