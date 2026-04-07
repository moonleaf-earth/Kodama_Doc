
[2026-04-06] Kodama: Examined env var scoping (task 2).

4 vars need per-environment values in Vercel dashboard:
- `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` — live for Prod, test for Preview/Dev
- `STRIPE_SECRET_KEY` — live for Prod, test for Preview/Dev
- `STRIPE_WEBHOOK_SECRET` — live endpoint secret for Prod, test endpoint secret for Preview/Dev
- `NEXT_PUBLIC_APP_URL` — `https://patchwatch.moonleafearth.com` for Prod, `https://patchwatch-2604.vercel.app` for Preview/Dev

All other vars (Supabase, Resend, Copernicus, CRON_SECRET) — set once across all environments, unless you have a separate Supabase staging project.

Q: Single Supabase project or separate staging project?

M:Single.  I added those in Preview/Dev. Stripe is currently in sandbox so I can't find live key, add into Todo for later.


M: execute coworker_tp.md

[2026-04-08] Kodama: Coworker test complete. Full report in Decision/coworker_tp_progress.md. Summary:

**PASS:** Auth flow (except logout), session persistence, /dashboard guard, community map (no private data), secrets scan, cross-user zone access blocked, zone CRUD via API, plan limits enforced, Stripe redirect (test mode confirmed).

**BUGS (4):**
- BUG-1 [High] Logout button does not end session — user stays authenticated after reload
- BUG-2 [Medium] Profile section missing from /dashboard/settings
- BUG-3 [Medium] API Keys section missing from /dashboard/settings
- BUG-4 [Low] No UI gate on draw tool when at zone limit (error only at API level)
- NOTE: Duplicate signup shows "Check your email" instead of a clear error (Supabase enumeration guard)

**Manual still needed:** Stripe checkout steps 3–7 (card entry, plan upgrade, cancellation) — Chrome extension cannot interact with checkout.stripe.com.

M:
1. Hide the Plan TEAM from webpage for now. (Create a separate commit so it can be reverted later)
2. Make a test plan for Claude Coworker to use google chrome to validate the test version (Preview/Development) of app.  You can refer to  `/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch/Architecture/test-plan.md` , create coworker_tp.md under `/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch/Decision`

    Q: How Coworker register? should I give it an gmail account? 

3. Claude Code App are now connected with MCP of Stripe, Vercel, Supabase, Github, Cloudflare.  Do I need to do extra on terminal side? 

[2026-04-06] Kodama:

1. Done — commit `ac75bfa` "[Temp] Hide Team plan from homepage". Grid changed to 2-col, subtitle updated. Revert that single commit to restore Team card.

2. Done — `Decision/coworker_tp.md` created. Covers Auth, Zone CRUD, Community Map, Settings, Stripe Checkout, and Security spot-checks. Skips satellite/cron and Team tests (not applicable now).
   Re your Q about registration: a Gmail alias like `yourname+coworker@gmail.com` is the easiest — emails land in your own inbox so you can forward the confirmation link to Coworker. No separate Gmail account needed. I've noted this in the test plan.

3. With those MCPs connected in Claude Code App, no extra terminal setup is needed for most tasks — you can manage Stripe, Supabase, and Vercel resources directly from here. The one optional addition: install the Vercel CLI (`npm i -g vercel`) to enable `vercel env pull` (sync env vars locally), `vercel deploy`, and `vercel logs` from the terminal. It's not blocking — just useful for faster local workflows.

