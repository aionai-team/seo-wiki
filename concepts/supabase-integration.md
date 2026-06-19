---
title: Supabase Integration — Pipeline Data Storage
created: 2026-06-19
updated: 2026-06-19
type: concept
tags: [supabase, database, pipeline, migration, storage, infrastructure]
confidence: high
sources:
  - entities/aionai-site.md
  - concepts/pipeline-architecture.md
---

# Supabase Integration

Το pipeline αποθηκεύει πλέον όλα τα δεδομένα σε Supabase PostgreSQL αντί για JSON files.

## Project

- **Project ID:** `zgngzcbfyrujpcttqwdf`
- **URL:** `https://zgngzcbfyrujpcttqwdf.supabase.co`
- **Credentials:** `scripts-seo/.env` (excluded from git)

## Schema — 11 Tables

| Table | Περιγραφή | Script |
|---|---|---|
| `pipeline_runs` | Γονέας για κάθε εκτέλεση pipeline | `keyword_discovery` |
| `keywords` | Αναλυτικά keywords + intent | `keyword_discovery`, `trend_validator` |
| `serp_results` | SERP αποτελέσματα | `serp_scraper` |
| `competitors` | Competitor analysis | `competitor_scraper` |
| `gap_analysis` | Gap scoring | `gap_scorer` |
| `secondary_intel` | Secondary intelligence | `secondary_extractor` |
| `geo_self` | GEO self-assessment | `geo_self_rendered` |
| `geo_market` | GEO market analysis | `geo_market_analysis` |
| `geo_gaps` | GEO gap analysis | `geo_gap_scorer` |
| `site_assessments` | Site crawl assessments | `site_scraper`, `comparison_report` |
| `status_snapshots` | Status summaries | `generate_status` |

## Architecture

```
lib/supabase_client.py  (shared singleton)
  ├── get_supabase()       → returns client
  ├── create_pipeline_run() → returns run_id
  ├── complete_pipeline_run()
  └── upsert_status()

Every script: --run-id <uuid>  (optional — without it, works as before)
Dual-write: JSON files still written (backward compat)
```

## Usage

```bash
RUN_ID=$(python3 -c "from lib.supabase_client import get_supabase, create_pipeline_run; print(create_pipeline_run(get_supabase()))")

python3 analysis/keyword_discovery.py --output data/latest/keywords.json --run-id $RUN_ID
python3 analysis/trend_validator.py --input data/latest/keywords.json --run-id $RUN_ID
# ... etc
```

## Migration

- `tools/migrate_to_supabase.py` — one-time migration of all existing JSON data
- Commit: `a5e8a34` (15 files changed, 779 insertions)
- Verified: full pipeline run `01b28898` writes all data correctly

## Related
- [[pipeline-architecture]] — Full pipeline documentation
