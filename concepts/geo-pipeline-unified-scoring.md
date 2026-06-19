---
title: GEO Pipeline Architecture — Proper Unified Scoring
created: 2026-06-16
updated: 2026-06-16
type: concept
tags: [geo, pipeline, implementation, scoring, competitor-analysis, gap-analysis, measurement-fix]
sources:
  - concepts/geo-pipeline-implemented.md
  - concepts/pipeline-architecture.md
  - concepts/geo-generative-engine-optimization.md
confidence: high
---

# GEO Pipeline Architecture — Proper Unified Measurement

## The Measurement Bug (Fixed 2026-06-16)

The original GEO measurement had a critical flaw: **our site was measured from source code** (TypeScript files) while **competitors were measured from rendered HTML**. This produced a false score of 67/100 for us vs 21.9 market avg.

**Real score with unified measurement: 34/100** (market avg 17.7, max 50).

## New Architecture (identical to SEO pipeline structure)

```
lib/geo_scorer.py                    ← Unified scoring (SAME for everyone)
     │
     ├── site_scraper --googlebot    ← Our site (rendered HTML)
     │
     ├── geo_market_analysis.py      ← Competitors (rendered HTML)
     │     └── reads serp.json → fetches each competitor → scores
     │
     └── geo_gap_scorer.py           ← Compare us vs market
           └── reads geo_market.json + our signals → gaps + recommendations
```

## Files Created

| File | Purpose |
|---|---|
| `lib/geo_scorer.py` | Unified GEO scoring library. Same 12 signals, same weights, same method for ALL sites. Total max = 100. |
| `analysis/geo_market_analysis.py` | Fetches competitor rendered HTML, extracts GEO signals via geo_scorer, calculates market statistics |
| `analysis/geo_gap_scorer.py` | Compares our GEO score vs market average, identifies gaps, opportunities, and strengths |

## GEO Scoring Signals (unified, max 100)

| Signal | Weight | Description |
|---|---|---|
| has_faq_schema | 15 | FAQPage JSON-LD in rendered HTML |
| has_faq_text | 10 | FAQ text visible in HTML (not just schema) |
| has_org_schema | 5 | Organization/ProfessionalService schema |
| has_tldr | 10 | TL;DR / summary block |
| is_answer_first | 10 | First paragraph after H1 is definitive answer |
| word_count_ge1500 | 10 | ≥1500 words visible text |
| has_lists_ge2 | 8 | ≥2 ul/ol lists |
| has_tables | 5 | ≥1 data table |
| has_citations_ge2 | 8 | ≥2 citation outbound links |
| has_author_name | 9 | Author byline or meta |
| has_statistics | 5 | Statistical claims |
| has_llms_txt | 5 | llms.txt at domain |

## Our Real Score: 34/100 (POOR)

Signals we HAVE (saved us from 0):
- FAQPage schema: ✅ (0% of competitors have this)
- Organization schema: ✅
- Author meta: ✅
- llms.txt: ✅ (0% of competitors have this)

Signals we DON'T have (all content is SPA shell):
- FAQ text visible: ❌ (5% of market has)
- TL;DR: ❌ (10% of market has)
- Answer-first: ❌ (0% — everyone's gap)
- Word count ≥1500: ❌ (20% of market has, 85% have lists)
- Lists: ❌ (85% of market has)
- Citations: ❌ (35% have)
- Tables: ❌ (20% have)
- Statistics: ❌ (30% have)

## Key Insight

Our advantage is **structural** (schema, llms.txt) — nobody in the Greek market has these. Our disadvantage is **content** — the SPA shell makes all text invisible. The strategy is:
1. Keep structural advantages (FAQ schema, llms.txt)
2. Add visible content (TL;DR, lists, citations, tables)
3. Answer-first rewrite of H1 + first paragraph

## Related
- [[geo-pipeline-implemented]] — Previous (broken) measurement pipeline
- [[pipeline-architecture]] — SEO pipeline (same architecture pattern)
- [[geo-generative-engine-optimization]] — GEO theory
- [[aionai-adapted-plan]] — Overall strategy
