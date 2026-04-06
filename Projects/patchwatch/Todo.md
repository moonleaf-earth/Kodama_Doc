## PatchWatch — Active Tasks

- [x] Apply migration `005_fix_team_members_rls.sql` to Supabase (run in SQL editor)
- [ ] Scope Vercel env vars: set live keys for Production, test keys for Preview+Dev (Stripe + Supabase) — see Knowledges/Vercel.md
- [ ] Set `STRIPE_WEBHOOK_SECRET` in `.env.local` (copy signing secret from Stripe Dashboard → Webhooks → your endpoint)
- [ ] Set up Cloudflare R2 bucket `patchwatch-images` when image persistence is needed (free tier: 10 GB / 1M ops/month)
