---
title: Perplexity Brief vs Agent Implementation
created: 2026-06-14
updated: 2026-06-14
type: comparison
tags: [competitor-analysis, gap-analysis, tools, geo]
confidence: high
sources: [raw/articles/new-pipeline-features-2026-06-14]
---

# Perplexity Brief vs Agent Implementation

## Context

On 2026-06-14, the user asked Perplexity to analyze the aionAI SEO pipeline and suggest improvements — see [[new-pipeline-features-2026-06-14]] for context. Perplexity produced a PDF (`seo_agent_briefing.pdf`) with 4 problem areas and suggested enhancements. The agent reviewed, critiqued, and implemented a subset.

## Key Differences

| Dimension | Perplexity Suggested | Agent Implemented |
|-----------|--------------------|-------------------|
| **Tech stack** | Laravel + PHP + Vue.js + MSSQL | Python scripts + JSON files |
| **URL validation** | 4-stage filter including OpenPageRank API | 2-stage: blocklist + classifier, no paid APIs |
| **Intent filtering** | Separate module needed | Already existed in `keyword_discovery.py` |
| **Secondary intelligence** | Full pipeline with DB schema | `secondary_extractor.py` — lightweight, JSON output |
| **Gap scoring** | OpenPageRank + backlink APIs + content age | `gap_scorer.py` — heuristics only (trend_score + agency_count + intent mismatch) |
| **Database** | SQL Server schema proposed | None — all JSON files |

## Why the Agent Diverged

1. **Stack mismatch**: Perplexity assumed Laravel/PHP/MSSQL. Our pipeline is Python + JSON. Database schema proposals were irrelevant.
2. **Phase-appropriate optimization**: aionAI is pre-traffic (sandbox phase). Paid APIs for DA/backlinks are premature. Heuristics provide 80% of value at 0% cost.
3. **Existing features**: Intent classification already existed in `keyword_discovery.py`. Perplexity recommended it as a new feature.
4. **Simplicity wins**: JSON files are more portable, inspectable, and debuggable than a SQL database for a single-site SEO pipeline.

## What Was Kept

- **URL validation layer** — simplified to blocklist + classifier
- **Non-competitor URL classification** — `secondary_extractor.py`
- **Intent-based routing** — `serp_scraper --intent-filter`
- **Gap scoring** — simplified formula without paid APIs
- **Intent mismatch detection** — added to gap_scorer (Perplexity's "intent mismatch" concept)

## Related

[[pipeline-architecture]] — How the pipeline is structured
