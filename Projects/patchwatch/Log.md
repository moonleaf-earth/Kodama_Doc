# Log

## [2026-04-01] Cloudflare + Vercel domain setup
- Cloudflare DNS: single CNAME → `cname.vercel-dns.com`, DNS only (not proxied)
- Vercel DNS records page: not needed when using Cloudflare — ignore it
- Vercel Project Settings → Domains: keep both `patchwatch.moonleafearth.com` and `patchwatch-2604.vercel.app`
- Screenshots archived in `Topics/`

## [2026-04-01] Stripe test mode safety
- Test Stripe keys (`pk_test_`/`rk_test_`) reject real cards — no accidental purchases possible
- Options to hide payment UI: env var gate, Vercel password protection, or remove UI
- Recommendation: leave as-is until ready to go live
- Test card reference created at `Knowledges/Stripe.md`
- Payment-blocking solutions documented at `Knowledges/Vercel.md`

## [2026-04-01] Pre-release test plan created
- `Architecture/test-plan.md` — manual tests (browser, visual, third-party dashboards)
- `Architecture/auto-test-spec.md` — automated tests (unit/integration/e2e) with file structure

## [2026-04-03] Constants centralised + Team plan restored on homepage
- Created `src/lib/constants.ts` — single source of truth for APP_NAME, APP_URL, EMAIL_FROM, geo constants, detection thresholds, API key prefix, localStorage keys
- Updated 8 files to import from constants instead of hardcoding
- Email from address changed to `alerts@patchwatch.moonleafearth.com`
- Homepage pricing section now shows all 3 plans (Free / Pro $12 / Team $29); Pro price corrected from $9→$12

## [2026-04-06] R2, Stripe scoping, RLS fix
- R2 free tier confirmed sufficient for early PatchWatch volume
- Stripe/Supabase env scoping guidance added to `Knowledges/Vercel.md`
- RLS infinite recursion fixed: `005_fix_team_members_rls.sql` created `get_my_team_ids()` SECURITY DEFINER function; applied to Supabase ✓
- Permissions Q&A documented in `Topics/permissions.md`

## [2026-04-03] Setup: permissions, knowledges, cross-repo linking
- Permissions already global via `.claude/settings.json` — no change needed
- Created `Knowledges/Stripe.md` and `Knowledges/Vercel.md`
- Updated codebase CLAUDE.md with Obsidian project folder link
- Saved feedback: no terminal duplication, use relative paths
