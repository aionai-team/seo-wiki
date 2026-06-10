# Wiki Schema — SEO/GEO Knowledge Base

## Domain
Search engine optimization (SEO) and generative engine optimization (GEO) for modern web applications, with a focus on Angular/SPA sites, Greek-language markets, and multi-engine visibility (Google, ChatGPT, Perplexity, Gemini, Bing, Claude).

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `google-core-web-vitals.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `[[wikilinks]]` to link between pages (minimum 2 outbound links per page)
- When updating a page, always bump the `updated` date
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`
- **Provenance markers:** On pages that synthesize 3+ sources, append `^[raw/articles/source-file.md]` at the end of paragraphs whose claims come from a specific source
- **Language:** SEO/GEO terms kept in English where standard; Greek notes OK in summaries or Greek-market specific pages

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
confidence: high | medium | low
contested: true                        # set when sources disagree
contradictions: [other-page-slug]      # pages this one conflicts with
---
```

### raw/ Frontmatter
```yaml
---
source_url: https://example.com/article
ingested: YYYY-MM-DD
sha256: <hex digest of body only>
---
```

## Tag Taxonomy

All tags must come from this list. Add new tags here BEFORE using them.

### Technical SEO
- `technical-seo` — general technical optimization
- `core-web-vitals` — LCP, FID/INP, CLS
- `structured-data` — JSON-LD, schema.org, rich results
- `crawlability` — robots.txt, sitemaps, crawl budget, JS rendering
- `sitemap` — XML sitemaps
- `canonical` — canonical URLs, duplicate content
- `hreflang` — multi-language/region targeting
- `angular-seo` — SPA-specific: prerendering, SSR, hash routing
- `page-speed` — load time, optimization, Core Web Vitals

### On-Page SEO
- `on-page` — general on-page optimization
- `meta-tags` — title, description, OG tags
- `headings` — H1-H6 hierarchy, keyword placement
- `keyword-research` — keyword discovery, analysis, mapping
- `internal-linking` — link architecture, anchor text, silos
- `content-optimization` — content quality, readability, relevance
- `image-optimization` — alt text, compression, lazy loading

### Off-Page SEO
- `off-page` — general off-page optimization
- `backlinks` — link building, link profile, link quality
- `domain-authority` — DR, DA, trust metrics
- `local-seo` — Google Business Profile, local citations

### GEO / AI Search
- `geo` — Generative Engine Optimization (umbrella)
- `ai-overview` — Google AI Overviews, SGE
- `llm-retrieval` — optimization for LLM-based search (ChatGPT, Perplexity, Gemini)
- `citation-optimization` — making content cite-worthy for AI
- `featured-snippets` — position zero, featured snippet targeting
- `answer-engine` — Perplexity, You.com, other answer engines

### Analytics & Monitoring
- `analytics` — GA4, GTM, event tracking
- `search-console` — GSC, Bing Webmaster Tools
- `rank-tracking` — keyword rank monitoring, SERP analysis
- `competitor-analysis` — competitive research
- `audit` — site audits, tools, methodologies

### Strategy & Process
- `strategy` — overall approach, roadmaps
- `google-update` — algo updates, core updates, impact
- `case-study` — experiments, before/after results
- `tool` — SEO tools (Ahrefs, SEMrush, Screaming Frog, etc.)
- `pitfall` — common mistakes, things that caused regressions

## Page Thresholds
- **Create a page** when an entity/concept appears in 2+ sources OR is central to one important source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or outside scope
- **Split a page** when it exceeds ~200 lines — break into sub-topics with cross-links
- **Archive a page** when its content is fully superseded — move to `_archive/`, remove from index

## Entity Pages
One page per notable SEO entity (tool, algorithm update, standard, organization). Include:
- Overview / what it is
- Key facts, dates, version history
- Relevance to aionAI / Greek market
- Relationships to other entities ([[wikilinks]])
- Source references

## Concept Pages
One page per concept or technique. Include:
- Definition / explanation
- Current best practice
- Angular-specific considerations (if any)
- Greek-market considerations (if any)
- Open questions or debates
- Related concepts ([[wikilinks]])

## Comparison Pages
Side-by-side analyses. Include:
- What is being compared and why
- Dimensions of comparison (table format preferred)
- Verdict or recommendation for aionAI
- Sources

## Update Policy
When new information conflicts with existing content:
1. Check the dates — newer sources generally supersede older ones
2. If genuinely contradictory, note both positions with dates and sources
3. Mark the contradiction in frontmatter: `contradictions: [page-name]`
4. Flag for user review in the lint report

## Integration with aionAI Workflow
- Pages about aionAI-specific tactics live under `entities/aionai-` or `concepts/aionai-*`
- Raw audits, crawl exports, and analytics exports go to `raw/articles/` with descriptive names
- Every SEO experiment or tactic deployed on aionAI should be logged here with expected vs. actual impact
