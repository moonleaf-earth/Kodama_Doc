
## Task: Fix BUG-1, BUG-2, BUG-4 and get Playwright e2e green

**Code path:** `~/Projects/Kodama/Ongoing_Projects/260328_patchwatch`
**Doc path:** `/Users/freedom/Library/Mobile Documents/iCloud~md~obsidian/Documents/O1/Kodama/Projects/patchwatch`

---

### One-time setup (first run only)

```bash
cd ~/Projects/Kodama/Ongoing_Projects/260328_patchwatch
npm install
npx playwright install chromium
```

Add to `.env.local`:
```
E2E_EMAIL=coworker_patchwatch@moonleafearth.com
E2E_PASSWORD=<ask user if not present>
```

---

### Test command

```bash
npm run test:e2e
```

Dev server starts automatically. Auth setup runs first, then all specs.

---

### Bugs to fix (in order)

**BUG-1 — Logout non-functional** (`src/components/ui/DashboardNav.tsx`)
- `signOut()` is called and awaited, redirect fires — but session persists on reload.
- Likely cause: Supabase SSR cookies not cleared. After `signOut()`, also clear cookies via the server route or use `@supabase/ssr`'s `createServerClient` pattern.
- Test: `tests/e2e/auth.spec.ts` → "BUG-1: logout clears session"

**BUG-2 — Profile section missing** (`src/app/(dashboard)/dashboard/settings/page.tsx`)
- Settings page has no section showing the user's email/account info.
- Add a Profile section above "Current Plan" that shows the logged-in user's email. Fetch from `supabase.auth.getUser()` client-side.
- Test: `tests/e2e/settings.spec.ts` → "BUG-2: profile section shows user email"

**BUG-4 — No UI gate on draw tool at zone limit** (`src/app/(dashboard)/dashboard/page.tsx` or map component)
- When user is at 3/3 zones, the Leaflet draw toolbar still activates.
- Fix: disable the draw button or immediately show an upgrade modal when at limit.
- Test: `tests/e2e/zone-limit.spec.ts` → "BUG-4: draw tool is disabled or shows upgrade prompt"

**BUG-3 — API Keys missing: SKIP** — intentionally hidden (settings/page.tsx line 245). Do not implement.

---

### Loop instructions

1. Run `npm run test:e2e`
2. For each failing test: read the error, find the component, fix it
3. Re-run tests — repeat until all pass (except skipped BUG-3 test)
4. Run `npm run test` (vitest) to confirm no regressions
5. Commit: `git commit -m "Fix BUG-1, BUG-2, BUG-4 — e2e tests passing"`
6. Write results summary at bottom of this file

---

### Notes
- BUG-1 fix likely needs a server-side signout route (`/api/auth/signout`) that clears the `sb-*` cookies, since client `signOut()` alone doesn't clear SSR cookies
- The `E2E_EMAIL` account must exist in Supabase and be on the free plan for zone-limit tests to work correctly
- `tests/e2e/.auth/` is gitignored — the setup step regenerates it each run
