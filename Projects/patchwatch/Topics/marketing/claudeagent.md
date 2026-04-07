# Claude Agent Marketing Tasks

> Claude Pro plan available. Three modes:
> - **Chat** — one-off research, drafting, ideation in Claude.ai chat
> - **Coworker** — scheduled/autonomous agent runs (Claude Code in coworker mode)
> - **Code** — Claude Code writes scripts, automation, data pipelines

---

## Content Production

### [Coworker] Weekly Blog Post Generation
- Input: trending deforestation news (scraped via RSS) + target keyword list
- Task: draft a 600-900 word SEO blog post, save to `/blog-drafts/YYYY-MM-DD-title.md`
- Schedule: every Monday 09:00
- Output: Markdown ready to paste into CMS (Notion → Webflow, or directly to Next.js blog)
- Review: diff review only, approve by pushing to main

### [Coworker] Social Post Batch (Weekly)
- Input: that week's blog post + any new change alerts detected in DB
- Task: generate 10 social posts (5 Twitter, 5 LinkedIn), save to `social-queue/YYYY-WW.md`
- Format: include hashtags, mention suggestions, image prompt for Midjourney/DALL-E
- Schedule: every Monday 10:00 (after blog draft is done)

### [Chat] NGO Cold Email Templates
- Task: write 3-touch cold email sequence for NGO audience
- Inputs: persona = "conservation analyst at a mid-size NGO", problem = no affordable land monitoring
- Output: 3 emails, under 150 words each, plain text style
- One-time task; re-use template for 6 months

### [Chat] Press Pitch Templates
- Task: write 5 journalist pitch variants tied to different news hooks (Amazon fires, palm oil, Indonesia logging, African savanna loss, wetland reclamation)
- Output: 5 email pitches, each under 200 words, punchy subject lines

### [Chat] Product Hunt Copy
- Task: write PH tagline, short description (260 chars), long description, first comment
- Include: before/after satellite imagery angle, "free" emphasis, hero use case

---

## Research & Intelligence

### [Chat] NGO Target List Research
- Task: find 100 mid-size environmental NGOs with land protection mandates; output as CSV (name, country, website, contact email if findable, focus area)
- Sources: WWF affiliate list, Rainforest Alliance partners, conservation.org grantees
- One-time; refresh quarterly

### [Chat] Competitor Analysis
- Task: analyze Global Forest Watch, Descartes Labs, Planet, Orbio Earth — positioning, pricing, audience, weaknesses
- Output: comparison table + PatchWatch differentiation points
- One-time; update on new competitor launch

### [Chat] Keyword Research
- Task: given niche (deforestation monitoring, satellite alerts, land change), generate 50 target keywords with estimated intent (informational / commercial / navigational)
- Output: ranked list with suggested content format per keyword

### [Coworker] Weekly Alert Digest for Social Proof
- Input: query Supabase for change events detected in last 7 days (public zones only)
- Task: format top 3 most dramatic changes as tweetable stats ("X hectares detected deforested in [region] this week")
- Schedule: every Friday 08:00, save to `social-queue/digest-YYYY-WW.md`

---

## Email Automation

### [Code] ConvertKit / Resend Sequence Setup Script
- Task: write a script that creates the 3-touch NGO email sequence via Resend API
- Includes: subscriber tagging (ngo_lead), send scheduling (Day 0, Day 3, Day 7)
- Language: TypeScript, run once to seed the sequence

### [Code] NGO Email List Importer
- Task: write a script to parse the NGO CSV (from research task above), validate emails, import to ConvertKit/Resend audience with `tag: ngo_outreach`
- Language: TypeScript/Node

---

## Analytics & Reporting

### [Coworker] Weekly Marketing Report
- Task: query Plausible/Umami analytics API + Supabase signups table; produce a Markdown summary of: new signups, top traffic sources, top landing pages, conversion rate
- Save to `Reports/marketing-YYYY-WW.md`
- Schedule: every Monday 07:00 (before content tasks)

### [Coworker] Referral Source Tagging Audit
- Task: monthly check that UTM parameters are correctly set on all outbound links (NGO emails, social posts, PH); flag any missing ones
- Schedule: 1st of each month

