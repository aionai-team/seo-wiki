# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`

## [2026-06-14] create | Wiki initialized
- Created SCHEMA.md, index.md, log.md
- Created directory structure: concepts/, comparisons/, raw/, entities/, queries/

## [2026-06-18] create | Site expansion + content optimization
- Created AIONAI-EXPANSION-SPEC.md (17 routes, full content, SEO specs)
- Created CONTENT-OPTIMIZATION-PLAN.md (page-by-page audit)
- Created GEO-HEALTH-CHECK-2026-06-18.md (57/100 home, 62/100 solutions)
- Pipeline: 171 keywords, 34 ranked. New rising: ai consulting (+20%), ai engineers (+27%), ai για δικηγόρους/λογιστές (+100%)
- llms.txt updated, 13 citations added, JSON-LD in constructor for SSR
- 17 static routes prerendered via @angular/ssr

## [2026-06-14] ingest | Pipeline architecture
- Created [[pipeline-architecture]] in concepts/
- Created [[data-directory-structure]] in concepts/
- Created [[perplexity-vs-agent-approach]] in comparisons/

## [2026-06-15] create | GEO pipeline extension implemented
- Enhanced `competitor_scraper.py` with 8 GEO extraction functions: TL;DR, FAQ text, lists, tables, citations, author, answer-first, statistics + `calculate_geo_readiness()`
- Added `--geo-check` mode to `site_scraper.py` for self-GEO analysis
- Run results: aionAI GEO score = 67/100, 46 competitors analyzed
- Documentation: [[geo-pipeline-extension]] (updated with implementation details)
- Created [[competitor-keyword-gap-results]] in concepts/
- Updated index.md with new page
- PDF documentation generated: aionai-seo-pipeline-documentation.pdf
- Created skill: [[competitor-keyword-gap-analysis]]

## [2026-06-16] create | Proper unified GEO pipeline + measurement fix
- Discovered CRITICAL measurement bug: our site was scored from source code (67/100) while competitors from rendered HTML (21.9 avg) — completely non-comparable
- Created `lib/geo_scorer.py` — unified GEO scoring with identical signals and weights for ALL sites
- Created `analysis/geo_market_analysis.py` — competitor GEO analysis via rendered HTML with unified scoring
- Created `analysis/geo_gap_scorer.py` — us vs market comparison with recommendations
- Created `scrapers/citation_scraper.py` — GEO citation gap check via Google search (proxy for ChatGPT/Perplexity)
- **Real GEO score: 34/100** (market avg 17.7, max 50). Previous 67/100 was measurement error.
- Run results: 20 competitors analyzed, 12-signal comparison, full gap report at data/latest/geo_gaps.json
- Health check confirmed: Googlebot sees 0 words, SPA shell, only JSON-LD + meta visible
- Phone truncation bug confirmed on live server: +30 693461355 (12 digits) vs source +30 6934613555 (13 digits)
- Wiki: created concepts/geo-pipeline-unified-scoring.md

## [2026-06-14] ingest | New pipeline features
- Created [[new-pipeline-features-2026-06-14]] in raw/articles/
