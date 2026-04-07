# PatchWatch — Marketing Execution Roadmap

> Checklist format. Work top-to-bottom. Each phase can begin once the prior phase's milestones are done.
> **You (manual)** = steps only you can do. **Delegate** = hand off to AI agent first, then you act on output.

---

## PHASE 1: Setup
> Goal: all tools wired up, AI agents configured, content assets ready before any outreach begins.

### Milestone 1: Create Accounts
- [ ] **You** — Sign up at [buffer.com](https://buffer.com) (free plan, social scheduling)
- [ ] **You** — Sign up at [resend.com](https://resend.com) *(already have for alerts — check if marketing sequences are enabled)*
- [ ] **You** — Sign up at [convertkit.com](https://convertkit.com) (free up to 1,000 subscribers) for email sequences
- [ ] **You** — Sign up at [app.apollo.io](https://app.apollo.io) or [hunter.io](https://hunter.io) (NGO email finding, free tier)
- [ ] **You** — Sign up at [plausible.io](https://plausible.io) or [umami.is](https://umami.is) (privacy-friendly analytics)
- [ ] **You** — Sign up at [ahrefs.com/webmaster-tools](https://ahrefs.com/webmaster-tools) (free SEO tracking — verify site ownership)

### Milestone 2: Set Up Claude Coworker Scheduled Tasks
- [ ] **You** — Open Claude Coworker, create the following recurring agents (prompts in `claudeagent.md`):
  - [ ] Weekly Marketing Report — every Monday 07:00
  - [ ] Weekly Blog Post — every Monday 09:00
  - [ ] Weekly Social Post Batch — every Monday 10:00
  - [ ] Weekly Alert Digest — every Friday 08:00
  - [ ] Monthly Referral UTM Audit — 1st of each month

### Milestone 3: AI Research Tasks (one-time, run now)
- [ ] **Delegate → Claude Chat** — NGO target list: 100 orgs → save as `Topics/marketing/data/ngo-list.csv`
- [ ] **Delegate → Claude Chat** — Keyword research: 50 target keywords → save as `Topics/marketing/data/keywords.md`
- [ ] **Delegate → Claude Chat** — Competitor analysis (GFW, Planet, Orbio) → save as `Topics/marketing/data/competitors.md`
- [ ] **Delegate → Gemini Pro** — Set up Google Sheet with NGO list; use `=GEMINI()` to classify + suggest subject lines
- [ ] **Delegate → Claude Chat** — Write 3-touch NGO cold email sequence → save as `Topics/marketing/data/email-sequence.md`
- [ ] **Delegate → Claude Chat** — Write 5 journalist pitch variants → save as `Topics/marketing/data/press-pitches.md`
- [ ] **Delegate → Claude Chat** — Write Product Hunt copy (tagline, description, first comment) → save as `Topics/marketing/data/producthunt-copy.md`
- [ ] **Delegate → Claude Chat** — Generate 20-question FAQ (or use GPT-4o structured output) → save as `Topics/marketing/data/faq.json`

### Milestone 4: Visual Assets
- [ ] **Delegate → ChatGPT Pro / DALL-E** — Generate 10 social/brand image assets (prompts in `otheragent.md`)
- [ ] **You** — Download generated images → store in `public/marketing/` in codebase (or Notion)

### Milestone 5: Automation Scripts
- [ ] **Delegate → Claude Code** — Write `scripts/import-ngo-list.ts` (parses CSV, imports to ConvertKit with `tag: ngo_outreach`)
- [ ] **Delegate → Claude Code** — Write `scripts/setup-email-sequence.ts` (creates 3-touch Resend/ConvertKit sequence via API)
- [ ] **You** — Run both scripts: `npx tsx scripts/import-ngo-list.ts` then `npx tsx scripts/setup-email-sequence.ts`
- [ ] **You** — Verify in ConvertKit dashboard: subscribers imported, sequence active

### Milestone 6: Analytics Integration
- [ ] **You** — Add Plausible/Umami script to Next.js `layout.tsx`
- [ ] **You** — Verify pageview events are tracking on local/preview deploy
- [ ] **You** — Add UTM parameters to all outbound links (email CTA, social bios, PH listing)

---

## PHASE 2: Pre-Launch
> Goal: public map seeded, content pipeline warm, launch materials ready. No PH listing live yet.

### Milestone 1: Seed NGO Founding Partnerships
- [ ] **Delegate → Claude Chat** — Draft personalized emails for 5-10 specific NGOs (offer: free Pro access + public zone visibility)
- [ ] **You** — Send emails to 5-10 NGO contacts *(only manual step in this milestone)*
- [ ] **You** — Follow up after 5 days if no response (Claude can draft follow-up)
- [ ] **Goal:** at least 3 confirmed; they set up watch zones → public map looks alive at launch

### Milestone 2: First Content Batch
- [ ] **Delegate → Claude Coworker** — Trigger first blog post run manually (don't wait for Monday)
- [ ] **You** — Review first blog draft (~5 min read) → approve or edit → merge to main
- [ ] **Delegate → Claude Coworker** — Trigger first social post batch after blog is approved
- [ ] **You** — Review 10 social posts (~5 min) → approve → schedule in Buffer
- [ ] **Delegate → Gemini Pro** — Translate top blog post to Spanish + Portuguese (for Amazon/Brazil audience)

### Milestone 3: Product Hunt Prep
- [ ] **You** — Create PH listing at [producthunt.com/posts/new](https://producthunt.com/posts/new) using copy from `data/producthunt-copy.md` *(save as draft, do not publish)*
- [ ] **You** — Upload DALL-E-generated visuals to PH listing
- [ ] **You** — Find a PH hunter with 500+ followers to post on your behalf (search PH community or r/producthunt)
- [ ] **Delegate → Claude Chat** — Draft hunter outreach message

### Milestone 4: Press List Prep
- [ ] **You** — Find contact emails for 20 environmental journalists using Apollo/Hunter (use `data/press-pitches.md` as guide for targeting)
- [ ] **You** — Load list into ConvertKit under `tag: press_outreach` (or manage as CSV)
- [ ] Email sequence ready to send at launch (no send yet)

### Milestone 5: FAQ Published on Landing Page
- [ ] **Delegate → Claude Code** — Add FAQ section to landing page using `data/faq.json`
- [ ] **You** — Review rendered FAQ on preview deploy

---

## PHASE 3: Launch
> Goal: maximum visibility in a short window. Sequence matters — do in order.

### Milestone 1: Community Posts (Day 1 — early morning)
- [ ] **Delegate → ChatGPT Pro** — Final-polish Reddit posts for r/remotesensing, r/environment, r/MapPorn
- [ ] **You** — Post to Reddit (3 communities, staggered 1 hour apart)
- [ ] **Delegate → ChatGPT Pro** — Final-polish Show HN post
- [ ] **You** — Post to Hacker News (aim for 08:00–10:00 Pacific)

### Milestone 2: Product Hunt Launch (Day 1 — same morning)
- [ ] **You** — Confirm hunter publishes the PH listing at 00:01 Pacific (PH day resets at midnight)
- [ ] **You** — Post PH link on Twitter/LinkedIn/Discord immediately
- [ ] **You** — Engage PH comments for ~2 hours *(use Claude-drafted response templates from `claudeagent.md`)*

### Milestone 3: Press Outreach (Day 1–2)
- [ ] **You** — Send journalist pitches from `data/press-pitches.md` (5 pitches, personalized per news hook)
- [ ] **You** — Send follow-up at Day 5 if no reply (Claude drafts)

### Milestone 4: Social Announcement Posts (Day 1)
- [ ] **You** — Publish pre-scheduled Buffer posts (already approved in Phase 2)
- [ ] **You** — Post manually on LinkedIn with founder note (Claude drafts, you personalize 1 sentence)

### Milestone 5: NGO Announcement Email (Day 1)
- [ ] **You** — Trigger NGO cold email sequence in ConvertKit (first touch sends to all 100 NGOs)

---

## PHASE 4: Ongoing (Post-Launch Steady State)
> Goal: keep the machine running with minimal weekly time.

### Weekly (automated — ~15 min your time)
- [ ] **Auto → Claude Coworker** — Monday 07:00: marketing report generated
- [ ] **You (Mon)** — Read report; flag anything needing a decision (~5 min)
- [ ] **Auto → Claude Coworker** — Monday 09:00: blog post drafted
- [ ] **You (Mon)** — Review blog draft, approve → merge (~5 min)
- [ ] **Auto → Claude Coworker** — Monday 10:00: 10 social posts drafted
- [ ] **You (Mon)** — Review social queue in Buffer, approve (~5 min)
- [ ] **Auto → Claude Coworker** — Friday 08:00: alert digest drafted for social post
- [ ] **Auto → Buffer** — Posts publish throughout the week

### Monthly
- [ ] **Auto → Claude Coworker** — 1st: UTM audit report generated
- [ ] **You** — Review UTM audit, fix any broken links (~10 min)
- [ ] **Auto → Gemini** — News digest continues daily (passive)
- [ ] **Delegate → Claude Chat** — Refresh NGO outreach if conversion < 2% (new batch of 50 orgs)

### As-Needed
- [ ] **You** — Respond to inbound press inquiries (Claude drafts, you send)
- [ ] **You** — Accept/reject NGO partnership requests
- [ ] **You** — Review and act on monthly analytics if MRR growth stalls

---

## Summary: Your Total Time Commitment

| Phase | Est. Your Time |
|-------|---------------|
| Phase 1 Setup | 3-4 hours (one-time) |
| Phase 2 Pre-Launch | 2-3 hours (one-time) |
| Phase 3 Launch | 4-5 hours (launch day only) |
| Phase 4 Ongoing | ~15 min/week |
