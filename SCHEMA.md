# Wiki Schema — SEO/GEO for aionAI

## Domain
SEO/GEO for aionAI — an Angular 21.2 SPA landing page for an AI agency targeting Greek SMEs. Covers technical SEO, GEO (Generative Engine Optimization), keyword research, competitor analysis, and content strategy.

## Conventions
- File names: lowercase, hyphens, no spaces (e.g., `pipeline-architecture.md`)
- Every wiki page starts with YAML frontmatter (see below)
- Use `wikilinks` (`[[page-name]]` syntax) to link between pages (minimum 2 outbound links per page)
- Pages in `concepts/` for tools, techniques, pipeline architecture
- Pages in `comparisons/` for side-by-side analyses (tools, approaches, data)
- Pages in `queries/` for filed query results worth keeping
- Every new page must be added to `index.md` under the correct section
- Every action must be appended to `log.md`

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
---
```

## Tag Taxonomy
- Pipeline: pipeline, data-flow, automation
- SEO: technical-seo, onpage, content, keywords
- GEO: geo, llm-optimization, featured-snippets
- Angular: angular-spa, prerendering, googlebot
- Data: data-organization, reporting, monitoring
- Competitors: competitor-analysis, serp, gap-analysis
- Tools: script, scraper, validator, analyzer

## Page Thresholds
- **Create a page** when a concept appears in 2+ sources or is central to one source
- **Add to existing page** when a source mentions something already covered
- **DON'T create a page** for passing mentions, minor details, or things outside the domain

## Entity Pages
One page per notable entity (competitor, tool, service).

## Concept Pages
One page per concept or topic (pipeline, data structure, technique).

## Comparison Pages
Side-by-side analyses of approaches, tools, or strategies.
