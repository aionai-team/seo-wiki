---
title: New Pipeline Features — 2026-06-14
created: 2026-06-14
updated: 2026-06-14
type: summary
tags: [pipeline, tools, data-organization, automation]
sources: []
confidence: high
source_url: internal
ingested: 2026-06-14
sha256: SESSION_2026-06-14
---

# New Pipeline Features — 2026-06-14

## Summary

Major pipeline overhaul adding URL classification, intent mismatch detection, gap scoring, secondary intelligence extraction, and data reorganization. All scripts moved to subdirectories under `scripts-seo/`.

See [[pipeline-architecture]] for the full architecture diagram and [[data-directory-structure]] for the data layout.

## Changes Made

### New Files
- `lib/url_utils.py` — URL classification, blocklist, intent, competitor confidence scoring
- `blocklist.txt` — 22 domains + 16 path patterns
- `scrapers/secondary_extractor.py` — Reddit/Quora questions, directory agencies, news topics, YouTube signals
- `analysis/gap_scorer.py` — Gap Score + intent mismatch detection
- `tools/generate_status.py` — Lightweight status.json generator
- `data/organize_data.py` — Data file organization by run date

### Modified Files
- `scrapers/serp_scraper.py` — Added rank position, URL blocking, classification, `--intent-filter`
- `scrapers/competitor_scraper.py` — Added blocklist/classification filter, `--no-filter`
- `analysis/trend_validator.py` — Added intent passthrough to output (was missing)
- `analysis/comparison_report.py` — Fixed `glob("*.json")` → `glob("*_googlebot.json")` (was reading batch_summary as competitor)

### Bugs Fixed
1. comparison_report was reading `batch_summary.json` and `*_jsonld.json` as competitors
2. trend_validator lost intent data between keyword_discovery and serp_scraper
3. url_utils.classify_url didn't handle subdomains (en.wikipedia.org was "COMPETITOR", not "WIKIPEDIA")
4. competitive_analyzer.py was superseded but not archived

## Data Architecture

Old: flat files with date in filename (e.g., `data/keywords_2026-06-14.json`)
New: 
```
data/runs/YYYY-MM-DD/        ← Historical run data
data/latest/                  ← Symlinks to most recent run
data/latest/status.json       ← Lightweight index (~2.5KB)
```

### Autonomous Scheduling

The agent decides when to run what. `status.json` flags outdated data:
```json
"outdated": {
  "serp": "Old format — needs refresh for URL classification",
  "gap_report": "No scored keywords — needs SERP data with classification"
}
```

## Files Moved
All scripts reorganized from flat `scripts-seo/` into:
- `lib/` — url_utils.py (shared library)
- `scrapers/` — serp_scraper, competitor_scraper, secondary_extractor, site_scraper
- `analysis/` — keyword_discovery, trend_validator, gap_scorer, comparison_report
- `tools/` — generate_status.py
- `archive/` — superseded scripts
