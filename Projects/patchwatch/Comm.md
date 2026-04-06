
[2026-04-06] Kodama:

**R2 pricing** — Free tier is fine. 10 GB storage + 1M Class A ops + 10M Class B ops/month is plenty for early PatchWatch satellite image volume. No need to choose a paid plan now.

**Stripe env scoping + Supabase keys** — Added to `Knowledges/Vercel.md` (new "Environment Variable Scoping" section). Added to Todo.md.

**RLS infinite recursion** — Root cause: `team_members` policies queried `team_members` to check membership, causing Postgres to recurse infinitely. Fixed in migration `005_fix_team_members_rls.sql`:
- Created `get_my_team_ids()` as a `SECURITY DEFINER` function — bypasses RLS for the inner lookup
- Replaced the 5 affected policies on `team_members`, `teams`, and `watch_zones` to use this function

**Action needed:** Run `005_fix_team_members_rls.sql` in the Supabase SQL editor.

**Permissions** — Answered in `Topics/permissions.md`: yes delete `settings.local.json`, `.claude/settings.json` is correct, relative paths not supported.

