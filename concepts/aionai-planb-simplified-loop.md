---
title: aionAI Plan B — Simplified Loop (keyword pipeline)
created: 2026-06-12
updated: 2026-06-13
type: concept
tags: [strategy, workflow, loop-definition, keyword-research, google-suggest, pytrends, site-crawler]
confidence: high
sources:
  - concepts/aionai-collaboration-model.md
  - concepts/seo-geo-master-agent-loop.md
---

# aionAI Plan B — Simplified Loop

## Decision

User has 3 plans and chose Plan B (his own loop). Simplified for zero-traffic start.

## The Pipeline (Implemented 2026-06-13)

```
  Site Scraper  ──→  Keyword Discovery  ──→  Trend Validator  ──→  Ranked List  ──→  Phase 2
  (source code)      (Google Suggest)       (pytrends GR)          (27 keywords)      (content)
```

No GSC data needed — site has zero traffic. Google Suggest API used instead (free, no CAPTCHA).

## Scripts Implemented

### 1. site_scraper.py
- **Source**: Reads aionAI's Angular TS/HTML source files
- **Action**: Extracts meta, JSON-LD, headings, FAQ, use cases, services, areas
- **Output**: 50+ intent-based short seeds derived from actual site content
- **Bonus mode**: `--health-check` renders the SPA via Playwright to compare source vs rendered

### 2. keyword_discovery.py (replaced PAA scraper)
- **Source**: Google Suggest API (`output=toolbar&hl=el&gl=gr`)
- **Seeds**: Built-in aionAI seeds (services + industries + areas) OR custom via `--seed-file`
- **Action**: Short seeds (1-3 words) → Google Suggest → 5-10 suggestions each
- **Relevance filter**: Keeps only business/AI/automation queries
- **Output**: 70-120 Greek user queries per full run
- **Note**: Google Suggest works best with SHORT seeds (long queries return 0)

### 3. trend_validator.py
- **Source**: pytrends with hl='el-GR', tz=360, geo='GR', timeframe='today 12-m'
- **Action**: Fetches weekly interest data for each keyword
- **Metrics**: trend_score (0-100), trend_direction (rising/falling/stable), percent_change, peak_month
- **Output**: Ranked JSON with `--min-score` filter
- **Rate**: ~1.2s per keyword (pytrends rate limit)

## Data Storage

All results saved with timestamps in `scripts-seo/data/`:
```
keywords_YYYY-MM-DD.json    → raw Google Suggest output
ranked_YYYY-MM-DD.json      → pytrends validated + ranked
site_YYYY-MM-DD.json        → site content inventory + seeds
```

Monthly cadence for re-running to track changes.

## Next Steps

1. ✅ Pipeline scripts built (3 scripts)
2. ⏳ Competitive analysis — check SERP rankings for the 27 keywords
3. ⏳ SERP analyzer — top 5 competitor URLs per keyword
4. ⏳ Phase 2 — Content creation for 8 selected keywords

## Key Learnings

- Path.home() in Hermes returns profile home (~/.hermes/.../home), NOT real home (/home/bog). Always use absolute paths.
- Google Suggest API is far more reliable than scraping PAA (no CAPTCHA, free, fast)
- Short seeds (1-3 words) yield results; longer queries return 0
- Relevance filter is critical — without it, Suggest returns too much noise
- DuckDuckGo will be used for competitive SERP analysis (free, no CAPTCHA)

## Related
- [[aionai-seo-monitoring-loop]] — Revised architecture with Googlebot-view measurement (supersedes this plan)
- [[aionai-collaboration-model]] — Collaboration workflow and original backlog
- [[aionai-static-pages-workflow]] — How pages are published
- [[aionai-adapted-plan]] — Original adapted plan
