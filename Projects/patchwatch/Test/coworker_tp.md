# PatchWatch — Coworker Chrome Test Plan

**Environment:** Preview / Development deployment  
**Agent:** Claude Coworker (browser via Chrome)  
**Scope:** End-to-end validation of core flows before production promote  

---
### Base URL
Use the Vercel Preview URL (e.g. `https://patchwatch-2604.vercel.app`).

---

## Test Sections

### 1. Auth
1. Visit `/signup` → fill in email + password → submit  (with email: coworker_patchwatch@moonleafearth.com)
2. Check inbox for confirmation email → click confirm link  
3. Confirm redirect to `/dashboard`  
4. Reload — session should persist  
5. Click logout → redirected to `/` or `/login`  
6. Try `/dashboard` while logged out → should redirect to `/login`  
7. Try signing up with the same email again → expect a clear error  

**Pass criteria:** All steps complete without errors; session persists across reload.

---

### 2. Zone Management
1. Log in → go to `/dashboard`  
2. Draw a polygon on the map (click to place points, close shape)  
3. Confirm zone appears in the sidebar with a default name  
4. Click zone in sidebar → map highlights it; detail panel shows name + area  
5. Rename zone → confirm name saves after reload  
6. Delete zone → zone disappears from map and sidebar  
7. Create 3 zones (Free plan limit) → attempt to create a 4th → confirm upgrade prompt appears  

**Pass criteria:** CRUD works; plan limit enforces upgrade prompt at zone #4.

---

### 3. Community Map
1. Log out (or open incognito tab)  
2. Visit `/map`  
3. Confirm page loads without login  
4. Confirm no private zone data is visible  

**Pass criteria:** Public map accessible unauthenticated; no private data leaked.

---

### 4. Settings Page
1. Log in → go to `/dashboard/settings`  
2. Confirm profile section shows correct email  
3. For Free plan: confirm Pro/Team features are hidden or locked  
4. Check API Keys section: generate a key → confirm it is shown once, then masked  

**Pass criteria:** Settings render correctly for Free tier; API key generation works.

---

### 5. Billing — Stripe (Test Mode)
1. From settings or upgrade prompt, click upgrade to Pro  
2. Confirm redirect to Stripe Checkout (test mode)  
3. Use Stripe test card: `4242 4242 4242 4242`, any future expiry, any CVC  
4. Complete checkout → confirm redirect back to dashboard  
5. Confirm plan shown in settings is now "Pro"  
6. Visit billing portal (if available) → cancel subscription  
7. Confirm plan reverts to Free  

**Pass criteria:** Checkout flow completes; plan reflects in dashboard; cancellation downgrades correctly.

---

### 6. Security Spot-checks
1. View page source — search for `sk_`, `rk_`, `sb_secret_` → none should appear  
2. While logged in as Coworker, attempt to access a zone ID belonging to another user via direct API call (if known) → expect 403 or empty result  

**Pass criteria:** No secrets in client bundle; cross-user data access blocked.

---

## What Coworker Should Report

For each section, report:
- **PASS / FAIL**
- Screenshot or description of any failure
- Exact URL and step where failure occurred
- Any console errors observed (open DevTools → Console)

---

## Notes

- Satellite scan and alert tests (sections 4–5 of the original test-plan.md) require a cron job to run — skip unless manually triggered.
- Team plan features are currently hidden from the UI — skip Team-related tests.
- Stripe is in **test/sandbox mode** — use test card numbers only.
