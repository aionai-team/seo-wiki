---
title: aionAI SEO Monitoring Loop — Revised Architecture
created: 2026-06-14
updated: 2026-06-14
type: concept
tags: [strategy, workflow, monitoring, angular-seo, googlebot, competitor-analysis, gap-analysis, ajax-crawl]
confidence: high
sources:
  - concepts/aionai-planb-simplified-loop.md
  - concepts/aionai-adapted-plan.md
  - raw/articles/seo-geo-agent-loop-methodology.md
---

# aionAI SEO Monitoring Loop — Revised Architecture

## Core Insight

The fundamental measurement unit for SEO comparison must be **what Googlebot actually sees**, not what exists in source code.

Previous approach compared:
- Source code word counts → **wrong**, inflated by TypeScript/JS noise
- Rendered competitor word counts → **misleading**, doesn't reflect search visibility

Revised approach:
- **Googlebot view** (raw HTML, no JS) = the minimum baseline
- **Googlebot rendered view** (Chromium, 5s timeout) = what modern Googlebot may index
- Compare **BOTH** against source to find real gaps

## The Pipeline (Revised 2026-06-14)

```
┌─────────────────────────────────────────────────────────────┐
│                    KEYWORD INTELLIGENCE                      │
├─────────────────────────────────────────────────────────────┤
│  keyword_discovery.py (Google Suggest API, hl=el&gl=gr)     │
│    → 70-120 Greek user queries                              │
│    ↓                                                         │
│  trend_validator.py (pytrends geo='GR', 12-month)           │
│    → ranked keywords with trend_score, trend_direction       │
│    → filtered by intent: INFO | COMMERCIAL | TRANSACTIONAL   │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    SERP ANALYSIS                             │
├─────────────────────────────────────────────────────────────┤
│  serp_scraper.py (Playwright stealth)                       │
│    → for each hot keyword: who ranks, what URLs              │
│    → fallback: Bing (no CAPTCHA) or manual browser           │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    COMPETITOR DEEP DIVE                      │
├─────────────────────────────────────────────────────────────┤
│  competitor_scraper.py (requests)                           │
│    → rendered content: word count, headings, schema, FAQ     │
│    ↓                                                         │
│  site_scraper.py `--googlebot-raw` (curl + Googlebot UA)    │
│    → what AI overview/featured snippets actually see         │
│    ↓                                                         │
│  site_scraper.py `--googlebot-rendered` (Playwright 5s)     │
│    → what modern Googlebot may index after JS execution      │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    SELF-ASSESSMENT                           │
├─────────────────────────────────────────────────────────────┤
│  site_scraper.py `--extract` (source code)                  │
│    → what we wrote (internal reference, not comparison)      │
│    ↓                                                         │
│  site_scraper.py `--googlebot-raw` (aionai.gr)              │
│    → what Googlebot sees of US                               │
│    ↓                                                         │
│  site_scraper.py `--googlebot-rendered` (aionai.gr)         │
│    → what Googlebot may index of US after JS                 │
└─────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────┐
│                    COMPARISON REPORT                         │
├─────────────────────────────────────────────────────────────┤
│  Level A: Our source vs Our Googlebot views                 │
│    → SPA gap analysis (JSON-LD only vs full content)        │
│    → JSON-LD validation (phone, email, URL format)          │
│    → score 0-100                                             │
│    ↓                                                         │
│  Level B: Our Googlebot views vs Competitors Googlebot views│
│    → REAL competitive gaps (not inflated word counts)        │
│    → Who has SSR, who is SPA, who has schema                 │
│    ↓                                                         │
│  Level C: Keyword coverage matrix                           │
│    → For each hot keyword: do we have content?               │
│    → Do competitors cover it? How? (blog, service page, etc) │
└─────────────────────────────────────────────────────────────┘
```

## site_scraper.py — Modes

| Mode | Tool | What it measures |
|------|------|-----------------|
| `--extract` | File read | Source code content (internal ref only) |
| `--googlebot-raw` | curl + Googlebot UA | Initial Googlebot crawl pass |
| `--googlebot-rendered` | Playwright, 5s timeout | Googlebot JS pass (best effort) |
| `--health-check` | Playwright, full load | Full rendered state (dev/debug) |

## 5 Identified Gaps

### Gap #1: Googlebot Dual-Pass
**Problem:** Our `--googlebot-raw` mode uses curl which only captures the first crawl pass. Modern Googlebot (2019+) has a second pass that executes JS with a Chromium engine, but with limited budget (timeout ~5-10s, no interaction, limited resources).

**Mitigation:** Add `--googlebot-rendered` mode that uses Playwright with strict constraints: 5s timeout, mobile viewport, no clicks, no scrolling. Compare rendered vs raw to understand JS execution gap.

### Gap #2: JSON-LD Validation
**Problem:** The gap report extracts JSON-LD but doesn't validate it. On live aionai.gr, the phone number shows `+30 693461355` (12 digits) vs the source `+30 6934613555` (13 digits) — a truncation bug. This could cause Google to reject the schema.

**Required checks:**
- `telephone`: valid Greek mobile format (`+30 69X XXXXXXX`, 13 chars)
- `email`: valid email format with `@`
- `url`: valid HTTPS URL
- `@type`: must be one of recognized schema.org types
- `areaServed`: must have at least 1 entry
- `serviceType`: must not be empty
- `description`: must be ≥50 chars and in Greek
- `foundingDate`: valid year format

### Gap #3: E-E-A-T Content Quality
**Problem:** Current measurement is purely quantitative (word count, heading count, FAQ count). Google's E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) framework demands qualitative analysis.

**Missing metrics:**
- Readability score (Greek text, Flesch-Kincaid equivalent for Greek)
- Author credentials / expert signals
- Citation/reference count (outbound links to authoritative sources)
- Content freshness (last update date)
- Entity coverage (does the content cover all related entities for a topic?)

### Gap #4: Domain Authority Context
**Problem:** aionAI domain was registered in 2026 with zero traffic. Even with perfect content, Google's sandbox effect may keep it unranked for months. The comparison report currently doesn't account for this.

**Mitigation:** Include domain age, estimated DA/DR, and sandbox expectations in comparison report findings.

### Gap #5: Keyword Intent Stratification
**Problem:** keyword_discovery + trend_validator find "hot" keywords but don't classify them by user intent. Writing a blog post for a transactional keyword or a pricing page for an informational keyword produces wasted effort.

**Required classification:**
- **INFORMATIONAL**: "τι είναι ai agents", "πώς λειτουργεί η αυτοματοποίηση"
- **COMMERCIAL**: "ai automation ελλάδα", "καλύτερη πλατφόρμα ai agents"
- **TRANSACTIONAL**: "ai consulting αθήνα τιμή", "αγορά ai chatbot"
- **NAVIGATIONAL**: "aionAI", "webout digital agency"

## Current Status (2026-06-14)

| Metric | Value |
|--------|-------|
| Googlebot body words (us) | 0 |
| Source clean words (us) | 862 |
| Googlebot score | 10/100 ⚠️ |
| FAQPage schema | ✅ 7 questions |
| Schema type | ProfessionalService |
| SSR/Prerendering | ❌ None — pure SPA |
| JSON-LD validity | ⚠️ Phone truncation detected |

## Next Actions

1. **Fix Gap #1**: Add `--googlebot-rendered` mode to site_scraper.py
2. **Fix Gap #2**: Add JSON-LD validation to comparison report
3. **Fix Gap #5**: Add keyword intent classification to keyword_discovery.py
4. **Rewrite comparison_report.py**: Use Googlebot views, not word counts
5. **Implement SSR**: Angular Universal or prerendering (medium-term)

## Related

- [[aionai-planb-simplified-loop]] — Previous pipeline architecture
- [[aionai-collaboration-model]] — Collaboration workflow and backlog
- [[aionai-adapted-plan]] — Original adapted SEO/GEO plan
- [[aionai-static-pages-workflow]] — Static pages publishing workflow
