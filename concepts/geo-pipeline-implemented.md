---
title: GEO Pipeline Implementation
created: 2026-06-15
updated: 2026-06-15
type: concept
tags: [geo, pipeline, implementation, competitor-analysis, site-analysis, scripts]
sources: [concepts/geo-generative-engine-optimization.md]
confidence: high
---

# GEO Pipeline Implementation

The GEO pipeline extends the existing scripts-seo pipeline with Generative Engine Optimization analysis. Built 2026-06-15.

## Components

### 1. `site_scraper.py --geo-check` — Self GEO Analysis
New mode that reads Angular source files and extracts GEO signals:
- **Static body words**: 0 (Angular SPA shell — CRITICAL gap)
- **FAQ schema**: Present in static HTML (JSON-LD)
- **FAQ text**: In JSON-LD but NOT visible as text without JS
- **TL;DR**: Detected (from meta/pattern match)
- **Answer-first H1**: CTA-style, not definitive — HIGH gap
- **Lists**: 2 `<ul>` in legal pages, 0 in content
- **Tables**: 0
- **Citations**: 0 external links in content
- **Author meta**: Present ("aionAI")
- **Statistics**: 0 in visible content
- **llms.txt**: 42 lines, comprehensive
- **Schema types**: ProfessionalService, FAQPage

**Our GEO Score: 67/100**

### 2. Enhanced `competitor_scraper.py` — 8 New GEO Functions

| Function | What it detects | Output fields |
|----------|----------------|---------------|
| `extract_geo_tldr()` | TL;DR/συνοπτικά/περίληψη blocks | `has_tldr`, `in_text_snippet` |
| `extract_geo_faq_text()` | FAQ sections in visible HTML | `has_faq_section`, `faq_section_count`, `faq_headings` |
| `extract_geo_lists()` | `<ul>`/`<ol>` elements | counts + average items per list |
| `extract_geo_tables()` | `<table>` elements | integer count |
| `extract_geo_citations()` | Citation-style outbound links | counts + keyword matches |
| `extract_geo_author()` | Author meta, byline, JSON-LD author | `has_author_meta`, `has_byline`, `has_author_schema`, `author_name` |
| `extract_geo_answer_first()` | First paragraph after H1 = direct answer? | `is_answer_first`, `confidence` |
| `extract_geo_statistics()` | Percentage/number claims in text | `stat_count`, `stats_sample` |

Plus `calculate_geo_readiness()` — weighted composite score (0-100) from all signals.

All functions are integrated into the main scraping loop so every competitor page gets `geo_signals` + `geo_score` in its output.

### 3. Market Comparison Results (2026-06-15)

46 competitors analyzed. Key findings:

| Signal | Market % | Us | Advantage |
|--------|---------|-----|-----------|
| has_faq_schema | 7% | ✅ | +93% — Strong differentiator |
| has_org_schema | 9% | ✅ | +91% |
| has_tldr | 7% | ✅ | +93% |
| faq_section_text | 24% | ✅ | +76% |
| word_count ≥2000 | 72% | ❌ | -72% — Our biggest gap |
| answer_first H1 | 22% | ❌ | -22% |
| tables | 17% | ❌ | -17% |
| citations ≥2 | 13% | ❌ | -13% |
| statistics | 0% | ❌ | 0% (no one has any) |

## Usage

```bash
# Self GEO check
python3 scrapers/site_scraper.py --geo-check --output data/latest/geo_self_check.json

# Enhanced competitor scraping (includes GEO)
python3 scrapers/competitor_scraper.py --input data/latest/serp.json \\
  --output data/latest/competitors_with_geo.json --verbose
```

## Related

- [[geo-generative-engine-optimization]] — GEO theory and frameworks
- [[agenticgeo-inner-loop]] — Advanced self-evolving GEO system
- [[aionai-adapted-plan]] — Overall SEO/GEO plan
- [[pipeline-architecture]] — Base SEO pipeline
