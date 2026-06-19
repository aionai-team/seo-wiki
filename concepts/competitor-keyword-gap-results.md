---
title: Competitor Keyword Gap Analysis — Results
created: 2026-06-15
updated: 2026-06-15
type: concept
tags: [pipeline, keywords, competitor-analysis, gap-analysis, findings]
sources: []
confidence: high
---

# Competitor Keyword Gap Analysis — Results

## Overview
On 2026-06-15 we ran the full SEO pipeline with **competitor-derived seeds** instead of built-in aionAI seeds. This page documents what worked, what didn't, and the key findings.

## Methodology
1. Identified competitors via web search and SERP data
2. Scraped their sites (headings, services, key phrases)
3. Created 49 seeds from competitor content
4. Ran `keyword_discovery.py --seed-file` → 277 keywords
5. Ran `trend_validator.py` → 15 ranked (min-score 3.0)
6. Ran `serp_scraper.py` → 8/8 success (new IP)
7. Ran `gap_scorer.py` → 8 scored

## What Worked
- **Competitor-derived seeds** produced 277 keywords (vs 148 from built-in), all AI-relevant
- **IP rotation** fixed CAPTCHA (109.242.96.x → 109.242.92.36, 0% blocked)
- **Google Suggest API** reliable, free, no CAPTCHA (https://suggestqueries.google.com/complete/search?output=toolbar&hl=el&gl=gr)
- **Intent classification** produced 137 COMMERCIAL + 131 INFO

## What Didn't Work
- **pytrends** for niche Greek keywords: only 15/277 had trend data ≥3.0
- **English AI keywords**: saturated by global giants (IBM, Microsoft, AWS, Salesforce)
- **Built-in aionAI seeds**: produced irrelevant keywords (logistics, iatreia, softone)
- **comparison_report.py**: missing dependencies (rendered_report, competitor_googlebot data)

## Key Finding
The real opportunity is in **Greek-language keywords** where few competitors exist. Example: "εταιρεία ai" (trend 4.1, RISING, COMMERCIAL) has almost no competitors.

## Site Health
Googlebot sees **0 words** (Angular SPA shell). Score: 45/100.
JSON-LD is good: ProfessionalService + FAQPage.

## Competitors Ranked
- Tier 1: [[proxima.gr]], [[aiagency.gr]]
- Tier 2: webout.gr, notthesame.gr, fliptheswitch.gr, growl.digital, flipnewmedia.com, connectingdots.gr
- Tier 3: deeplab.ai, dikaio.ai, moveo.ai

## Pipeline Data
[[pipeline-architecture]] ran end-to-end successfully. See [[data-directory-structure]] for file organization.
