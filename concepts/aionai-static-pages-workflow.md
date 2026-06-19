---
title: aionAI Static Pages & Workflow
created: 2026-06-10
updated: 2026-06-10
type: concept
tags: [strategy, content-architecture, workflow, angular-seo, geo]
sources: [concepts/aionai-adapted-plan.md, entities/aionai-site.md]
confidence: high
---

# aionAI Static Pages & Workflow

## Architecture Decision

New pages (blog, service details) are **static HTML files**, NOT Angular hash views.

```
aionai.gr/
├── index.html              Angular SPA (front page, untouched)
├── ypiresies/              Static service detail pages
│   ├── ai-agent.html
│   ├── crm-automation.html
│   └── erp-integration.html
├── blog/                   Auto-generated blog
│   ├── index.html          Blog listing
│   ├── post-title.html
│   └── ...
└── assets/                 Shared images, css, fonts
```

Benefits over Angular hash views:
- Clean crawlable URLs (`/blog/...` not `/#/blog`)
- Full schema markup in static HTML (no JS rendering needed for bots)
- Google/Perplexity/LLM bots see content immediately
- No Angular code changes needed

## Workflow Loop

| Step | Who | What |
|------|-----|------|
| 1 | ESY | Approve topic list |
| 2 | EGO | Research keywords via pytrends (region=GR), rank by trend |
| 3 | EGO | Crawl top 5 SERP competitors per keyword |
| 4 | EGO | Build H2/H3 outline with gap analysis |
| 5 | ESY | Approve outline (~10 min) |
| 6 | EGO | Generate full article in Greek + schema markup + .html file |
| 7 | ESY | Review + tweak (~15 min) |
| 8 | ESY | Upload to server (or EGO pushes via FTP/SSH if access given) |
| 9 | EGO | Track GSC at 14/30/60 days |
| 10 | EGO | Re-loop every 2 weeks with updated keyword data |

User time investment: ~20 min/week (topic approval + article review)

## First 3 Steps

1. Pick 3 service areas from existing site content for dedicated pages
2. Create blog folder with first 3 posts based on Greek keyword research
3. Link from front page sections using normal `<a href="/ypiresies/...">` — no Angular changes

## Related
- [[aionai-adapted-plan]] — Parent SEO/GEO strategy
- [[geo-generative-engine-optimization]] — GEO framework
