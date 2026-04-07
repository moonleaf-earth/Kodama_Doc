# Coworker Test Plan — Progress State
**Paused:** 2026-04-07
**Resume:** 2026-04-08 05:30
**URL under test:** `https://patchwatch-2604-eci8laxia-moonleaf-earths-projects.vercel.app`
**Account:** `coworker_patchwatch@moonleafearth.com` / `TestPass123!` (confirmed)

---

## Results So Far

| Section | Step | Result | Notes |
|---|---|---|---|
| **1. Auth** | 1 — /signup loads, form submits | ✓ PASS | |
| | 2 — Confirmation email sent | ✓ PASS | Email confirmed by user |
| | 3 — Redirect to /dashboard after login | ✓ PASS | |
| | 4 — Session persists after reload | ✓ PASS | |
| | 5 — Logout redirects to / or /login | ✗ FAIL | Clicking Log Out does not end session; /dashboard still accessible after reload |
| | 6 — /dashboard while logged out → /login | ✓ PASS | |
| | 7 — Duplicate email signup → error | ⚠ NOTE | Shows "Check your email" again (Supabase enum guard) — no explicit error message |
| **2. Zone Management** | Draw polygon | ⏳ IN PROGRESS | Draw mode activated, 4 points placed, Finish clicked — awaiting screenshot to confirm zone saved |
| **3. Community Map** | /map loads without auth | ✓ PASS | |
| | No private data visible | ✓ PASS | |
| **4. Settings** | — | ⏳ NOT STARTED | |
| **5. Billing** | — | ⏳ NOT STARTED | |
| **6. Security** | No secrets in DOM/NEXT_DATA | ✓ PASS | |
| | Cross-user zone access | ⏳ NOT STARTED | Needs a zone ID from another account |

---

## Resume Instructions

1. Check if the polygon drawn in Section 2 was saved (sidebar should show "1 / 3 zones used")
2. If not saved, redraw — the draw mode was active with 4 points placed over Nigeria area; try again after verifying session is active
3. Continue Section 2 steps: sidebar name, detail panel, rename, delete, then create 3 zones and test 4th zone upgrade prompt
4. Section 4: Settings — /dashboard/settings, email shown, Free plan locks, API key generation
5. Section 5: Billing — upgrade to Pro via Stripe test card `4242 4242 4242 4242`
6. Section 6 spot-check 2: Cross-user zone access (need a zone UUID from another user)
7. **BUG TO REPORT:** Logout (Section 1 step 5) does not clear session

---

## Open Bugs
- **[BUG-1] Logout non-functional** — clicking "Log Out" in nav does not terminate session; user remains authenticated after page reload.
