# Coworker Test Plan — Final Report
**Completed:** 2026-04-08
**URL tested:** `https://patchwatch-2604-eci8laxia-moonleaf-earths-projects.vercel.app`
**Account:** `coworker_patchwatch@moonleafearth.com`

---

## Results

### 1. Auth

| Step | Result | Notes |
|---|---|---|
| 1 — /signup loads, form submits | ✓ PASS | |
| 2 — Confirmation email sent | ✓ PASS | Email confirmed manually |
| 3 — Redirect to /dashboard post-login | ✓ PASS | |
| 4 — Session persists after reload | ✓ PASS | |
| 5 — Logout redirects to / or /login | ✗ **FAIL** | Clicking "Log Out" does not end session — user remains authenticated after reload |
| 6 — /dashboard while logged out → /login | ✓ PASS | |
| 7 — Duplicate email signup → clear error | ⚠ NOTE | Shows "Check your email" again (Supabase email-enumeration guard). No explicit "email already in use" error. Intentional or UX gap? |

**Section verdict: PARTIAL PASS** (1 bug, 1 UX note)

---

### 2. Zone Management

| Step | Result | Notes |
|---|---|---|
| Draw polygon on map | ⚠ NOTE | Leaflet Draw UI unresponsive to automated browser clicks; tested via `/api/zones` POST directly |
| Zone appears in sidebar | ✓ PASS | Name + "Polygon" type shown |
| Sidebar count updates | ✓ PASS | "1 / 3 zones used" |
| Click zone → detail panel opens | ✓ PASS | Panel shows name, Timeline/Alert Rules/History/Export tabs |
| Rename zone → persists after reload | ✓ PASS | PATCH /api/zones/:id returns 200, name updated |
| Delete zone | ✓ PASS | DELETE /api/zones/:id returns 200, zone removed |
| Create 3 zones, attempt 4th | ✓ PASS | API returns 400 "Zone limit reached. Upgrade your plan for more zones." |
| UI upgrade prompt at limit | ⚠ NOTE | Draw mode activates normally at 3/3 — no UI gate on the draw button itself. Error only surfaces at API level after submitting. |

**Section verdict: PASS (API)** — UI draw interaction could not be tested via automation; logic is sound at API level.

---

### 3. Community Map

| Step | Result | Notes |
|---|---|---|
| /map loads without login | ✓ PASS | |
| No private zone data visible | ✓ PASS | No user_id, email, or private fields in DOM |

**Section verdict: PASS**

---

### 4. Settings

| Step | Result | Notes |
|---|---|---|
| Profile section shows correct email | ✗ **FAIL** | Profile section not present on /dashboard/settings |
| Pro/Team features locked for Free tier | ✓ PASS | Locked items clearly marked with ✗; "Upgrade to Pro/Team" CTAs visible |
| API Keys section — generate key | ✗ **FAIL** | API Keys section not present on /dashboard/settings |

**Section verdict: PARTIAL FAIL** — plan comparison renders correctly but profile and API key sections are missing (not yet built?)

---

### 5. Billing — Stripe (Test Mode)

| Step | Result | Notes |
|---|---|---|
| Click "Upgrade to Pro" | ✓ PASS | |
| Redirect to Stripe Checkout (test mode) | ✓ PASS | URL: `checkout.stripe.com/c/pay/cs_test_…` |
| Complete checkout with test card | ⏳ MANUAL | Chrome extension cannot interact with Stripe domain |
| Confirm plan shows "Pro" | ⏳ MANUAL | |
| Cancel subscription via billing portal | ⏳ MANUAL | |
| Plan reverts to Free | ⏳ MANUAL | |

**Section verdict: PARTIAL** — redirect confirmed, checkout flow needs manual completion

---

### 6. Security Spot-checks

| Step | Result | Notes |
|---|---|---|
| No secrets in client bundle (sk_, rk_, sb_secret_) | ✓ PASS | DOM, NEXT_DATA, and localStorage clean |
| Cross-user zone access blocked | ✓ PASS | GET /api/zones/:foreign-id → 404 "Zone not found" |

**Section verdict: PASS**

---

## Bugs to Fix

| ID | Severity | Description |
|---|---|---|
| BUG-1 | High | **Logout non-functional** — clicking "Log Out" in nav does not end session; user remains authenticated after page reload. |
| BUG-2 | Medium | **Profile section missing** from /dashboard/settings — email not displayed, user has no way to verify their account identity. |
| BUG-3 | Medium | **API Keys section missing** from /dashboard/settings — not yet implemented. |
| BUG-4 | Low | **No UI gate on draw tool at zone limit** — draw mode opens when at 3/3; upgrade prompt only surfaces as an API error, not a proactive UI modal. |
| NOTE-1 | Low | **Duplicate signup shows "Check your email" instead of error** — this is Supabase's enumeration-protection default but may confuse users. |

---

## Manual Steps Still Needed
1. **Billing steps 3–7** — enter Stripe test card `4242 4242 4242 4242`, complete checkout, verify Pro plan, cancel, verify downgrade.
2. **Logout** — once BUG-1 is fixed, re-test logout + session termination.
