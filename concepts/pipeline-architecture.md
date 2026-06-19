---
title: SEO Pipeline Architecture
created: 2026-06-14
updated: 2026-06-14
type: concept
tags: [pipeline, data-flow, tools, automation]
sources: [raw/articles/new-pipeline-features-2026-06-14]
confidence: high
---

# SEO Pipeline Architecture

## Overview

The aionAI SEO pipeline is a modular Python system in `projects/scripts-seo/`. Each phase produces JSON output consumed by the next phase. The agent decides cadence adaptively — no fixed cron schedule.

## Directory Structure

```
scripts-seo/
├── lib/url_utils.py              ← Shared: URL classification, blocklist, intent
├── scrapers/
│   ├── serp_scraper.py           ← Phase A: Google SERP URLs + classification
│   ├── competitor_scraper.py     ← Phase B: competitor site analysis
│   ├── secondary_extractor.py    ← Non-competitor intelligence (Reddit, directories)
│   └── site_scraper.py           ← Googlebot visibility checks (our site)
├── analysis/
│   ├── keyword_discovery.py      ← Google Suggest API keyword research
│   ├── trend_validator.py        ← pytrends validation + intent
│   ├── gap_scorer.py             ← Gap Score + intent mismatch detection
│   └── comparison_report.py      ← aionAI vs competitors Googlebot comparison
├── tools/
│   └── generate_status.py        ← Lightweight status.json generator
├── data/
│   ├── latest/                   ← Symlinks to most recent run
│   ├── runs/YYYY-MM-DD/          ← Historical run data
│   └── competitors/              ← Per-competitor Googlebot data
├── blocklist.txt                 ← 22 domains + 16 path patterns
└── requirements.txt
```

## Pipeline Flow

```
keyword_discovery.py  →  trend_validator.py  →  serp_scraper.py
                                                    │
                                     ┌────────────────┼────────────────┐
                                     ▼                ▼                ▼
                          competitor_scraper    secondary_extractor   gap_scorer.py
                                     │                │                │
                                     ▼                ▼                ▼
                          comparison_report    content ideas     ranked opportunities
```

## Key Design Decisions

1. **Two-phase SERP analysis**: Phase A (serp_scraper) extracts only URLs from Google with stealth protections. Phase B (competitor_scraper) visits sites with plain requests — no CAPTCHA risk.
2. **URL classification at extraction time**: Every URL is classified as COMPETITOR/REDDIT/DIRECTORY/NEWS/WIKIPEDIA immediately when extracted from SERP.
3. **Intent-first targeting**: Keywords classified by intent (INFO/COMMERCIAL/TRANSACTIONAL/NAVIGATIONAL) at discovery time. COMMERCIAL keywords routed to competitor analysis, INFO to content research.
4. **No paid APIs**: Gap scoring uses pytrends data + URL classification heuristics. No OpenPageRank, no backlink APIs.
5. **Autonomous scheduling**: Agent decides cadence based on data changes, not calendar.

## Related

[[data-directory-structure]] — How runs and latest/ are organized
[[perplexity-vs-agent-approach]] — Why we chose heuristics over paid APIs
