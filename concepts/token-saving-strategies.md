---
title: Token-Saving Strategies for the SEO+GEO Agent
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [strategy, tool, angular-seo]
sources: [raw/articles/seo-geo-agent-loop-methodology.md]
confidence: high
---

# Token-Saving Strategies

Design patterns to reduce LLM token costs while maintaining output quality across the [[seo-geo-master-agent-loop]].

## 1. Tiered Routing
Route LLM calls by task complexity (from dannwaneri/seo-agent):

| Tier | Task | Model | Approx Cost |
|---|---|---|---|
| 0 | Deterministic checks (title length, H1 count, link parser) | Pure Python | $0 |
| 1 | Simple extraction (meta description, broken link summary) | Small model / Haiku | ~$0.0001/URL |
| 2 | Structured extraction (schema detection, entity listing) | Mid-tier model | ~$0.001/URL |
| 3 | Full drafting, multi-turn rewriting, root cause diagnosis | Large model / flagship | ~$0.01–0.05/task |

Run Tier 0 first; escalate only when lower tiers flag issues.

## 2. Structured JSON Outputs
Instruct agent to output structured JSON for all audit and analysis tasks, not prose. Structured output eliminates re-parsing, reduces hallucination risk, and compresses token usage by 30–50%.

## 3. Incremental State Writes
Write state to report after each URL or phase, not at end of full run. Safe to interrupt; on resume skip already-completed steps. No work lost to API timeouts or crashes.

## 4. Caching
Cache SERP API results, visibility scan results, and fetched page content with timestamps. Freshness window: 24 hours for rankings, 7 days for page content. Eliminates redundant API calls when Phase 10 triggers a re-run shortly after Phase 9.

## 5. Context Window Management
Load only relevant notes per phase:
- Current target page's audit note
- Strategy archive summary (top-25 strategies by score — not full history)
- Current phase's input data
- Do NOT load unrelated pages, historical logs beyond last 3 snapshots, full draft history

## 6. Prompt Templates
Pre-build per-phase prompt templates stored in the wiki. Each has a fixed system prompt + variable injection points. Prevents re-reasoning about output format each run, saves instruction overhead tokens.

## 7. Batch Processing for GEO Visibility Scans
Phase 9 sends 20–30 prompts x 8 platforms = up to 240 API calls per content piece. Batch by platform, run all 8 platforms in parallel (async), cache with 24-hour TTL.

## 8. MAP-Elites Compression
Store strategy archive as compact JSON (description + scores + usage count), not full strategy text. Full instructions generated at inference time from the description. Keeps archive small and searchable.

## 9. Phase 10 Early Exit
Decision tree with early exits: if simple freshness issue detected, apply fix immediately without running all 6 diagnostic hypotheses. Only run full diagnosis tree when cause is ambiguous.

## 10. Human-in-the-Loop Checkpoints
Flag to needs_human[] and pause on: login walls, cornerstone content changes, broad schema changes, earned media outreach. Prevents expensive runaway loops on tasks needing human context.

## Related
- [[seo-geo-master-agent-loop]] — Where these strategies are applied per-phase
- [[agenticgeo-inner-loop]] — MAP-Elites compression details
