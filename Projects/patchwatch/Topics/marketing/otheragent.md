# Other AI Agent Marketing Tasks

> Agents available: Gemini Pro (Google AI Studio / Gemini Advanced), ChatGPT Pro (GPT-4o)
> Use these for tasks that benefit from their specific strengths or where Claude is unavailable.

---

## Gemini Pro Tasks
> Strengths: large context window (1M tokens), Google ecosystem integration, multimodal (good at analyzing imagery), Workspace automation (Docs, Sheets, Gmail)

### Image Analysis for Social Posts
- Task: given a before/after satellite image pair (PNG), describe the visible land change in plain English for a general audience
- Use case: auto-caption social posts without requiring manual interpretation
- Input: export before/after thumbnails from R2 bucket
- Output: 2-3 sentence caption + suggested emoji
- How: Gemini API via n8n node or Google AI Studio batch

### Google Sheets NGO Database Maintenance
- Task: use Gemini in Sheets (`=GEMINI()`) to:
  - Classify NGO focus area from their website description
  - Suggest email subject line personalized per org
  - Flag organizations likely to be interested based on their geography + mandate
- One-time setup; runs as spreadsheet formula

### Translate Content for International Audiences
- Task: translate top 5 blog posts + email templates into Spanish, Portuguese, French (key languages for target deforestation regions: Amazon, Congo, SE Asia)
- Gemini handles long documents better; use Gemini 1.5 Pro with full blog post in context
- Output: translated .md files in `blog-drafts/translated/`

### Google News Alert Monitoring
- Task: set up Gemini-powered summarization of Google News RSS for keywords: "deforestation 2026", "illegal logging satellite", "forest clearance [region]"
- Summarize into 5 bullet points daily, save to `intelligence/news-YYYY-MM-DD.md`
- Can wire via Google Apps Script + Gemini API

---

## ChatGPT Pro Tasks
> Strengths: reliable instruction following, strong structured output, good at persuasive writing, DALL-E image generation, browsing for current data

### DALL-E: Social Media Visual Assets
- Task: generate illustration assets for social posts
  - "Satellite view illustration of a lush green forest polygon being monitored, with a glowing boundary line, clean tech aesthetic"
  - "Before/after concept: green forest → brown cleared land, with data overlay UI, flat design"
  - "Email alert screenshot mockup, minimalist, showing deforestation alert notification"
- Output: PNG assets for use in Twitter headers, blog post hero images, Product Hunt screenshots
- Cadence: one batch of 10 at launch; refresh quarterly

### GPT-4o Browsing: Real-Time Deforestation News Hooks
- Task: browse current news for deforestation events happening now; suggest 3 story angles where PatchWatch could offer a relevant satellite monitor angle
- Use case: reactive social posts timed to news cycles
- Cadence: run ad-hoc when a deforestation story breaks

### Reddit/HN Community Post Drafts
- Task: write Show HN post and 3 Reddit posts (r/remotesensing, r/environment, r/MapPorn) tailored to each community's tone
- HN style: technical, humble, show the interesting engineering
- Reddit style: community member tone, not promotional
- GPT-4o tends to better match community register for these

### Structured Output: FAQ and Objection Handling
- Task: generate a 20-question FAQ for the PatchWatch landing page, covering: how it works, data accuracy, privacy, pricing, NGO use cases
- Output: JSON array `[{question, answer}]` ready to import into landing page component
- One-time; GPT-4o structured output is reliable for this

### Email Subject Line A/B Variants
- Task: for each of the 3 NGO cold emails, generate 5 subject line variants optimized for open rate
- Output: table with variant + rationale
- Use the top 2 per email to A/B test in ConvertKit

