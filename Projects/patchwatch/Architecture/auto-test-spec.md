# PatchWatch — Automated Test Spec

Reference for the coding agent. Every test case below must have a corresponding test file.
Framework: Vitest + @testing-library/react. Supabase mocked via `msw` or test DB.

---

## Unit Tests

### `src/lib/stripe/plans.ts`
- `getPlanLimits('free')` returns correct max_zones (3), area, scan_frequency
- `getPlanLimits('pro')` returns correct limits
- `getPlanLimits('team')` returns correct limits
- Unknown plan falls back to free

### `src/lib/stripe/server.ts`
- `getProPriceId()` / `getTeamPriceId()` return env values
- Throws if env vars missing

### Zone validation helpers (if extracted)
- Area calculation: polygon → km² matches expected
- Self-intersecting polygon rejected
- Zone count check: returns false when at plan limit

### Alert rule matching
- Rule with `min_area: 100` skips events < 100 km²
- Rule with `change_types: ['vegetation_loss']` skips `water_change` events
- Rule with `confidence: 0.8` skips events below threshold
- Multiple rules: event matches if ANY rule matches

---

## Integration Tests (API Routes)

### Auth middleware
- Request without session → 401
- Request with valid session → passes through with user context

### `POST /api/zones`
- Valid geometry + auth → 201, zone in DB
- Missing geometry → 400
- Exceeds plan zone limit → 403 with upgrade message
- Exceeds plan area limit → 403

### `GET /api/zones`
- Returns only zones belonging to authenticated user
- Empty list for new user → 200, `[]`

### `DELETE /api/zones/[id]`
- Owner can delete → 200, zone gone
- Non-owner → 403
- Non-existent ID → 404

### `GET /api/zones/[id]/history`
- Returns change events in chronological order
- Free plan: events older than 6 months excluded
- Pro/Team: all events returned

### `GET /api/zones/public`
- Returns only public zones
- No auth required
- Does not leak private zone data (no user IDs, no private fields)

### `POST /api/billing/checkout`
- Valid plan + auth → 200, returns Stripe session URL
- Invalid plan name → 400
- No auth → 401

### `POST /api/billing/webhook`
- Valid signature + `checkout.session.completed` → plan updated in DB
- Valid signature + `customer.subscription.deleted` → plan reset to free
- Invalid signature → 400
- Duplicate event (idempotency) → no duplicate writes

### `GET /api/billing/status`
- Returns current plan from profiles
- Syncs plan if Stripe subscription status differs from DB

### `POST /api/billing/portal`
- User with stripe_customer_id → 200, portal URL
- User without stripe_customer_id → 400

### `POST /api/cron/scan`
- Missing `CRON_SECRET` header → 401
- Valid secret → processes zones, creates change_events
- Copernicus API failure → error logged, no crash

### `POST /api/cron/send-alerts`
- Missing `CRON_SECRET` → 401
- Pending alerts → emails sent via Resend, status updated to `sent`
- No pending alerts → 200, no emails sent
- Resend API failure → alert marked `failed`

### `GET/POST /api/alerts`
- Auth required
- Returns only user's alerts
- Free user creating custom rule → 403

### `GET/POST /api/teams`
- Non-team-plan user → 403
- Team-plan user → can create/list teams

### `GET/POST /api/keys`
- Auth required
- Generate → returns key once
- List → keys masked

---

## System / E2E Tests

### Full signup → zone → scan → alert flow
1. Create user (Supabase auth)
2. Create zone via API
3. Trigger scan cron → change event created
4. Trigger alert cron → alert email sent
5. Verify alert in `/api/alerts` response

### Upgrade → downgrade flow
1. User on free plan, 3 zones
2. Simulate `checkout.session.completed` webhook → plan = pro
3. Verify zone limit increased (can create zone #4)
4. Simulate `customer.subscription.deleted` webhook → plan = free
5. Verify existing zones preserved, scan frequency = weekly

### Cross-user isolation
1. User A creates zone
2. User B calls `GET /api/zones` → does not see User A's zone
3. User B calls `GET /api/zones/[A's zone id]` → 403 or 404

### Cron secret enforcement
- All cron endpoints (`/api/cron/scan`, `/api/cron/send-alerts`) reject without header
- All cron endpoints accept with correct header

---

## Test File Structure

```
src/
  __tests__/
    unit/
      stripe-plans.test.ts
      zone-validation.test.ts
      alert-matching.test.ts
    integration/
      zones-api.test.ts
      billing-api.test.ts
      cron-scan.test.ts
      cron-alerts.test.ts
      auth-middleware.test.ts
      teams-api.test.ts
      keys-api.test.ts
      public-zones.test.ts
    e2e/
      signup-to-alert.test.ts
      upgrade-downgrade.test.ts
      cross-user-isolation.test.ts
      cron-secret.test.ts
```

## Setup Required
- `vitest` + `@testing-library/react` + `msw` (for mocking Supabase/Stripe/Resend)
- Test Supabase project or local Supabase (`supabase start`) for integration tests
- Stripe test fixtures via `stripe-mock` or recorded responses
- `vitest.config.ts` with path aliases matching `tsconfig.json`
