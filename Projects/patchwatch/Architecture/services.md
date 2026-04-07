# PatchWatch — External Services

## Core Infrastructure

### Supabase
- **Role:** Database + Authentication
- **Used for:** User auth/sessions, data persistence (zones, change events, alerts, profiles)
- **Packages:** `@supabase/supabase-js`, `@supabase/ssr`
- **Env vars:** `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`, `SUPABASE_SECRET_KEY`
- **Key files:** `src/lib/supabase/`

---

## Payments

### Stripe
- **Role:** Billing & Subscriptions
- **Used for:** Pro/Team plan checkout, customer portal, webhook-driven subscription sync
- **Packages:** `stripe`, `@stripe/stripe-js`
- **Env vars:** `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY`, `STRIPE_SECRET_KEY`, `STRIPE_WEBHOOK_SECRET`, `STRIPE_PRO_PRICE_ID`, `STRIPE_TEAM_PRICE_ID`
- **Key files:** `src/lib/stripe/`, `src/app/api/billing/`

---

## Notifications

### Resend
- **Role:** Transactional Email
- **Used for:** Alert notification emails to users on change detection
- **Package:** `resend`
- **Env vars:** `RESEND_API_KEY`
- **Key files:** `src/lib/email/send.ts`, `src/app/api/cron/send-alerts/route.ts`

---

## Satellite Data

### Copernicus Data Space Ecosystem
- **Role:** Satellite Imagery Provider
- **Used for:** Sentinel-2 imagery, NDVI computation, true-color images, change detection
- **Auth:** OAuth2 client credentials
- **Endpoints:**
  - Token: `identity.dataspace.copernicus.eu/auth/realms/CDSE/protocol/openid-connect/token`
  - Catalog: `catalogue.dataspace.copernicus.eu/odata/v1/Products`
  - Processing: `sh.dataspace.copernicus.eu/api/v1/process`
- **Env vars:** `COPERNICUS_CLIENT_ID`, `COPERNICUS_CLIENT_SECRET`
- **Key files:** `src/lib/satellite/`

---

## Mapping

### OpenStreetMap
- **Role:** Base Map Tiles
- **Used for:** Background map in the zone drawing UI
- **URL:** `https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png`
- **No account required**

### Leaflet / react-leaflet / leaflet-draw
- **Role:** Map UI Framework
- **Used for:** Interactive map, polygon/rectangle/circle zone drawing
- **Packages:** `leaflet`, `leaflet-draw`, `react-leaflet`, `@react-leaflet/core`

---

## Scheduling

### Cron Service (Vercel Cron)
- **Role:** Job Scheduler
- **Used for:** Triggering satellite scans and alert delivery on schedule
- **Secured via:** Bearer token (`CRON_SECRET`)
- **Endpoints:** `POST /api/cron/scan`, `POST /api/cron/send-alerts`
- **Env vars:** `CRON_SECRET`

---

## Outbound Integrations

### User Webhooks (app-managed)
- **Role:** Event Push to User Systems
- **Used for:** Delivering change-detection events to user-defined URLs
- **Auth:** HMAC-SHA256 signed payloads
- **Key files:** `src/lib/webhooks.ts`

---

## Summary

| Service | Type | Env Vars |
|---|---|---|
| Supabase | DB / Auth | 3 |
| Stripe | Payments | 5 |
| Resend | Email | 1 |
| Copernicus | Satellite API | 2 |
| OpenStreetMap | Map tiles | — |
| Leaflet | Frontend lib | — |
| Vercel Cron | Scheduler | 1 |
| User Webhooks | Outbound push | — |
