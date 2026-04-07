# PatchWatch — Product Requirements Document

## 1. Overview

**Product:** PatchWatch — Deforestation & Land-Change Alert Service
**Tagline:** "Google Alerts for ecosystems."
**Date:** 2026-03-28

PatchWatch is a freemium web application that lets anyone monitor land-use changes anywhere on Earth using free satellite imagery. Users draw watch zones on a map and receive automated alerts when deforestation, wetland loss, illegal clearing, or other significant land changes are detected.

---

## 2. Goals

| Goal | Metric |
|------|--------|
| Democratize land-change monitoring | 1,000 active watch zones within 6 months of launch |
| Enable faster response to habitat destruction | Median alert latency < 7 days from change event (free tier) |
| Build a sustainable business on freemium model | 3-5% free-to-paid conversion rate within first year |
| Grow organically through conservation communities | 80%+ of signups from organic/referral channels |
| Provide conservation-grade data without technical barriers | Non-technical users can set up a zone in < 2 minutes |

---

## 3. User Personas

### 3.1 Community Guardian
**Who:** Local resident or indigenous community member near a threatened area.
**Need:** Wants to know immediately when land near their community is being cleared.
**Behavior:** Sets up 1-2 watch zones, checks alerts weekly, shares evidence on social media or with local authorities.

### 3.2 Conservation Analyst
**Who:** Staff member at an NGO (e.g., rainforest protection org).
**Need:** Monitors dozens of sites across a region, generates reports for donors and policymakers.
**Behavior:** Paid tier user. Manages team workspace, exports GIS data, uses API to feed into internal dashboards.

### 3.3 Environmental Journalist
**Who:** Reporter covering deforestation, land grabs, or climate stories.
**Need:** Finds and verifies land-change events for investigative pieces.
**Behavior:** Free tier user. Uses before/after imagery as story evidence. Occasional paid report exports.

### 3.4 Researcher
**Who:** Academic studying land-use change, ecology, or remote sensing.
**Need:** Lightweight monitoring without building a custom pipeline.
**Behavior:** Uses API for data integration. Values historical analysis and exportable data.

---

## 4. User Stories

### Account & Onboarding
- **US-1:** As a new user, I can sign up with email or OAuth (Google/GitHub) so I can get started quickly.
- **US-2:** As a new user, I see a guided onboarding that shows me how to create my first watch zone in under 2 minutes.

### Watch Zones
- **US-3:** As a user, I can draw a polygon or circle on an interactive map to define a watch zone.
- **US-4:** As a user, I can name my watch zone and add optional notes (e.g., "proposed logging concession near village").
- **US-5:** As a free user, I can create up to 3 watch zones, each up to 10 km radius.
- **US-6:** As a paid user, I can create unlimited watch zones with larger coverage areas.
- **US-7:** As a user, I can edit or delete my watch zones at any time.

### Change Detection & Alerts
- **US-8:** As a free user, my watch zones are scanned weekly using Sentinel-2 imagery.
- **US-9:** As a paid user, my watch zones are scanned daily with access to higher-resolution sources.
- **US-10:** As a user, I receive an email alert when a significant land change is detected in my watch zone.
- **US-11:** As a user, I can view before/after satellite imagery for each detected change.
- **US-12:** As a paid user, I can set custom alert rules (minimum change area, change type filters).
- **US-13:** As a user, I can view a timeline of all detected changes in a watch zone (last 6 months free, up to 10 years paid).

### Public Watch Map
- **US-14:** As a user, I can opt-in to display my watch zones on the public community map.
- **US-15:** As a visitor (no account), I can browse the public watch map to see community-monitored areas.

### Exports & API
- **US-16:** As a paid user, I can export change reports in GIS-compatible formats (GeoJSON, KML, PDF).
- **US-17:** As a paid user, I can access a REST API to query my zones and change data programmatically.

### Team & Organization
- **US-18:** As a paid org user, I can invite team members to a shared workspace.
- **US-19:** As a team member, I can view and manage shared watch zones and add collaborative notes.

### Billing
- **US-20:** As a user, I can upgrade to a paid plan via Stripe checkout.
- **US-21:** As a paid user, I can manage my subscription (upgrade, downgrade, cancel) from my account settings.

---

## 5. Functional Requirements

### 5.1 Map Interface
- Interactive map (Mapbox GL JS or Leaflet) with satellite basemap
- Drawing tools: polygon, circle, rectangle for zone creation
- Zone list sidebar with search and filter
- Click-to-inspect: click any zone to see its status, last scan, and change history

### 5.2 Satellite Data Pipeline
- **Primary source:** ESA Copernicus Sentinel-2 (10m resolution, 5-day revisit)
- **Secondary source:** NASA/USGS Landsat 8/9 (30m resolution, 8-day revisit)
- Data access via Copernicus Data Space Ecosystem API and/or Google Earth Engine
- Cloud masking to filter out cloud-covered imagery before analysis
- Scheduled processing: cron-triggered serverless jobs (weekly for free, daily for paid)

### 5.3 Change Detection Algorithm
- Normalized Difference Vegetation Index (NDVI) comparison between consecutive clear images
- Threshold-based alerting: flag pixels where NDVI drops beyond a configurable threshold
- Minimum change area filter to reduce noise (default: 0.5 hectares)
- Change type classification (v2): vegetation loss, water body change, bare soil exposure
- Output: change mask polygon, area in hectares, confidence score, before/after thumbnails

### 5.4 Notification System
- Email alerts via transactional email service (Resend or SendGrid)
- Each alert includes: zone name, change summary, before/after image, link to dashboard
- Web push notifications (optional, progressive web app)
- Alert batching: aggregate multiple changes per zone into one notification per scan cycle

### 5.5 Authentication & Authorization
- Email/password and OAuth (Google, GitHub)
- Role-based access: free user, paid individual, paid org admin, paid org member
- Rate limiting on API endpoints

### 5.6 Billing
- Stripe integration for subscription management
- Plans: Free, Pro (individual), Team (organization)
- Usage metering for API calls (paid tier)

### 5.7 Public Watch Map
- Anonymous browsable map of all opt-in watch zones
- Aggregate statistics: total zones monitored, total area covered, changes detected
- No user-identifiable information shown unless user opts in to display name

---

## 6. Non-Functional Requirements

### 6.1 Performance
- Map loads in < 2 seconds on broadband
- Change detection processing completes within 1 hour of scheduled trigger
- Alert delivery within 15 minutes of processing completion

### 6.2 Scalability
- Serverless architecture scales to 10,000+ watch zones without infra changes
- Database can handle 100k+ change records with indexed spatial queries

### 6.3 Reliability
- 99.5% uptime for web application
- Graceful degradation when satellite data sources are temporarily unavailable (retry with backoff, notify users of delays)

### 6.4 Security
- All data in transit over HTTPS/TLS
- User passwords hashed with bcrypt/argon2
- API keys for paid tier with scoped permissions
- No sensitive user data stored beyond what's required for the service

### 6.5 Data Privacy
- Users can delete their account and all associated data
- Watch zone locations are private by default (public only with explicit opt-in)
- GDPR-compliant data handling

### 6.6 Accessibility
- WCAG 2.1 AA compliance for all UI
- Keyboard-navigable map and dashboard
- Alt text for satellite imagery in alerts

---

## 7. Technical Architecture

```
User Browser
    |
    v
[Next.js Frontend — Vercel/Cloudflare Pages]
    |
    v
[API Layer — Serverless Functions (AWS Lambda / Cloudflare Workers)]
    |
    +--> [Supabase Postgres — users, zones, alerts, change history]
    |        (PostGIS extension for spatial queries)
    |
    +--> [Satellite Data Pipeline — Scheduled Serverless Jobs]
    |        |
    |        +--> Copernicus Data Space API (Sentinel-2)
    |        +--> Google Earth Engine API (Landsat, processing)
    |        |
    |        v
    |    [Change Detection Processing]
    |        |
    |        v
    |    [Object Storage (S3/R2) — before/after thumbnails, change masks]
    |
    +--> [Notification Service]
    |        +--> Resend/SendGrid (email)
    |        +--> Web Push API
    |
    +--> [Stripe API — billing & subscriptions]
```

### Key Technical Decisions
- **PostGIS** for spatial queries (zone intersection, area calculations)
- **Google Earth Engine** for heavy satellite processing (avoids needing GPU infrastructure)
- **Object storage** for imagery (cheap, CDN-backed for fast delivery)
- **Serverless-first** to keep ops burden near zero

---

## 8. Phased Delivery Plan

### Phase 1 — MVP (Weeks 1-8)
**Goal:** Core free-tier experience end-to-end.
- User auth (email + OAuth)
- Interactive map with zone drawing (up to 3 zones)
- Sentinel-2 data pipeline with weekly scans
- NDVI-based change detection
- Email alerts with before/after imagery
- Basic dashboard: zone list, change history, alert log
- Landing page

### Phase 2 — Beta Launch (Weeks 9-11)
**Goal:** Polish and community features.
- Onboarding flow refinement
- Public watch map (opt-in)
- Aggregate community stats on landing page
- Alert history and change timeline per zone
- Bug fixes from early beta testers

### Phase 3 — Paid Tier (Weeks 12-15)
**Goal:** Revenue-generating features.
- Stripe subscription integration
- Daily scan frequency for paid users
- Expanded zone limits (unlimited zones, larger areas)
- Export reports (GeoJSON, KML, PDF)
- Custom alert rules (area threshold, change type filter)
- Historical analysis (up to 10 years)

### Phase 4 — API & Teams (Weeks 16-21)
**Goal:** Power users and organizations.
- REST API with API key auth
- Team workspaces with shared zones
- Org billing (per-seat pricing)
- Advanced change type classification
- Webhook integrations for alerts

---

## 9. Success Metrics

| Metric | Target (6 months post-launch) |
|--------|-------------------------------|
| Registered users | 5,000 |
| Active watch zones | 1,000 |
| Weekly active users | 1,500 |
| Free-to-paid conversion | 3-5% |
| Alert accuracy (true positive rate) | > 85% |
| Median time from change to alert | < 7 days (free), < 2 days (paid) |
| Monthly recurring revenue | $2,000-5,000 |
| Organic/referral signup share | > 80% |

---

## 10. Open Questions & Risks

| Risk | Mitigation |
|------|-----------|
| Cloud cover in tropical regions delays change detection | Use multi-source imagery (Sentinel + Landsat), implement cloud-gap filling algorithms |
| Change detection false positives (seasonal changes, agriculture) | Tunable sensitivity thresholds, minimum area filters, user feedback loop to improve model |
| Google Earth Engine API rate limits or pricing changes | Build fallback pipeline using open-source tools (rasterio) on serverless; GEE is free for non-commercial use |
| Low initial adoption | Seed with 5-10 NGO partnerships pre-launch for immediate content on public map |
| Satellite data source downtime | Multi-source redundancy, graceful degradation with user notifications |
| Cost scaling with many watch zones | Monitor processing costs closely; optimize batch processing; set zone limits per tier |

---

## 11. Out of Scope (for now)

- Mobile native app (PWA-first approach covers mobile)
- Real-time streaming alerts (near-real-time batch is sufficient)
- Custom ML model training per user
- Drone or ground-sensor integration
- Carbon credit calculation or marketplace
