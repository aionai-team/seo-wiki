---
title: Memory Contents (for when memory is upgraded)
created: 2026-06-15
updated: 2026-06-15
type: summary
tags: [memory, meta, session-management]
confidence: high
---

# Memory Contents

This page lists all facts that should be saved to persistent memory when storage allows.
Check this file when the user says "memory upgraded — save it".

## 1. Session Startup

First action every session: read `data/latest/status.json` (one read_file, ~2.5KB). Check `outdated` keys for refresh needs, `site.sandbox_stage` for context, `gaps.top_opportunities` for priorities. Then propose next action to user.

## 2. Project Structure

scripts-seo/ with lib/, scrapers/, analysis/, tools/, data/ subdirs.
Data in data/runs/YYYY-MM-DD/ + symlinks in data/latest/.
Wiki at ~/wiki-seo/ for persistent knowledge. Read SCHEMA.md → index.md → data/latest/status.json at session start.

## 3. External AI Briefs

When user provides an external analysis (Perplexity etc.), critique first against actual stack/phase/budget. Bucket into KEEP/ADAPT/SKIP. Present comparison before implementing. The external AI doesn't know our context.

## 4. User Preferences

- NEVER skip broken files — fix root cause completely. "Μην κανεις τσαπατσο δουλειες".
- Shebang must be line 1 of every .py script. sys.path boilerplate goes AFTER shebang+docstring, before imports.
- One change at a time: PROPOSE → REVIEW → COMMIT → OBSERVE.
- Front page (Angular SPA) is OFF-LIMITS. New content = static HTML in public/.
- Data accuracy > SEO. Never fabricate metadata.
- "Εσυ θα τα κανεις ολα" — take initiative.

## 5. GEO Pipeline (live 2026-06-15)

### site_scraper.py --geo-check
Self-GEO analysis of our Angular source code. Checks: static body words, FAQ visibility, TL;DR, answer-first H1, lists, tables, citations, author, statistics, schema types, llms.txt, meta tags.
- Our score: 67/100
- Critical gap: 0 static body words (Angular SPA shell)
- High gap: H1 is CTA-style, not answer-first
- LOW gap: no data tables

### competitor_scraper.py enhanced
8 GEO extraction functions added:
1. extract_geo_tldr() — TL;DR/summary blocks
2. extract_geo_faq_text() — FAQ sections in visible HTML
3. extract_geo_lists() — <ul>/<ol> counts
4. extract_geo_tables() — <table> count
5. extract_geo_citations() — citation-style outbound links
6. extract_geo_author() — author meta/byline/schema
7. extract_geo_answer_first() — first paragraph answers query?
8. extract_geo_statistics() — percentage/number claims

Plus calculate_geo_readiness() — weighted 0-100 score from all signals.
Each competitor result now includes geo_signals + geo_score.

### Market comparison results (2026-06-15)
- 46 competitors analyzed with GEO signals
- We lead in: FAQ schema (+93% vs market), org schema (+91%), TL;DR (+93%), FAQ section (+76%)
- We lag in: word count ≥2000 (-72%), answer-first H1 (-22%), tables (-17%), citations (-13%)
- Top competitor GEO scores: AWS (47), Salesforce (46), ChatbotAI (45), Microsoft (41)

### Data files
- data/latest/geo_self_check.json — our GEO analysis
- data/latest/competitors_with_geo.json — competitors with GEO signals

### Pipeline flow with GEO
```
SERP URLs → competitor_scraper.py (enhanced)
  → competitors_with_geo.json (per-competitor GEO signals)
  
competitors_with_geo.json → geo_scorer.py (NEW — not yet built)
  → geo_report.json (market comparison)
  
competitors_with_geo.json + geo_report.json → gap_scorer.py (enhanced)
  → gap_report.json (with geo_opportunity_factor)
```

## 6. SERP Scraper
- IP 109.242.92.36 (Cosmote Greece) — currently working, no CAPTCHA
- Uses Playwright with 8-layer stealth
- Max 5-8 keywords per day, 15-25s delays between searches
- Check IP before running: curl -s https://api.ipify.org?format=text

## 7. Content Strategy
- Greek keywords > English (English AI keywords saturated by global giants)
- Topics to target: AI agents, automation, CRM/ERP integration for Greek SMEs
- Static pages to create: ypiresies/, blog/ai-agents/, blog/automation/
- Each page should have: TL;DR, GIS signals (Table, Lists, Author, FAQ), schema, answer-first H1
