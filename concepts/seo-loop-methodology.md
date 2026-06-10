---
title: SEO Loop Methodology (Human Practice)
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [strategy, technical-seo, on-page, off-page, keyword-research, content-optimization]
sources: [raw/articles/seo-geo-agent-loop-methodology.md]
confidence: high
---

# SEO Loop Methodology

The human SEO methodology, as practiced by professionals using SEMrush, AMA standards, and LinkedIn SEO expert consensus. The AI agent automates these phases end-to-end.

## Six-Phase Methodology

### Phase A — Audit & Baseline
Collect benchmark data: organic traffic, keyword rankings, bounce rate, conversion rate, backlink count. Crawl site for crawl errors, broken links, duplicate content, technical blockers. Verify indexing, test mobile-friendliness, analyze page speed against Core Web Vitals.

**Production pattern** (dannwaneri/seo-agent): visit each URL in real Chromium browser, extract signals via LLM API, async broken-link detection, write results incrementally to JSON (crash-safe), generate plain-English summary.

### Phase B — Research
Keyword research: primary, secondary, long-tail — annotated with volume, difficulty, intent. Competitor keyword gap analysis. SERP composition analysis (what formats rank?). GEO-layer question research: collect actual prompts from sales calls, Reddit, PAA boxes, social listening.

### Phase C — On-Page Optimization

| Element | Standard | Rationale |
|---|---|---|
| Title tag | ≤60 chars; keyword upfront | Truncation prevention; relevance signal |
| H1 | Exactly one; contains primary keyword | Semantic structure |
| H2–H6 | Logical hierarchy; secondary keywords distributed | Crawl + readability |
| Meta description | 50–160 chars; keyword + value prop | CTR signal |
| URL | Clean, hyphenated, keyword-containing | Crawl + user trust |
| Image alt text | Keyword-relevant, descriptive | Accessibility + indexing |
| Internal links | No orphan pages; contextual anchor text | PageRank distribution |
| E-E-A-T signals | Author bios, credentials, sourcing transparency | Google + AI source selection |
| Schema | FAQ, HowTo, Article, Author, BreadcrumbList | Rich results + AI extraction |

### Phase D — Technical SEO
Fix crawl errors, verify sitemap, confirm robots.txt. Enforce HTTPS. Maintain architecture <4 clicks from homepage. Resolve duplicate content via canonical tags. Monitor Core Web Vitals: LCP <2.5s, CLS <0.1, INP <200ms.

### Phase E — Off-Page / Authority
Analyze backlink profile, disavow toxic links. Execute link acquisition (guest posting, digital PR, niche edits). Monitor brand mentions. Manage reviews and review schema. Target: ≥20 high-authority domain citations per quarter.

### Phase F — Monitor & Iterate
Track organic traffic (GA4), keyword rankings (GSC + SERP API), backlink growth, conversion rates, AI search visibility. Quarterly content gap analysis. A/B test headlines, formats, CTAs. Update content every 90–180 days for SEO; every 90 days for GEO.

## Related
- [[seo-geo-master-agent-loop]] — Where these phases map to automation
- [[geo-generative-engine-optimization]] — The GEO counterpart methodology
- [[human-vs-ai-seo-agent]] — Time comparison table

## References
- dannwaneri/seo-agent (GitHub, open source)
- Frase (2026). AI Agents for SEO: Complete Guide.
- Padma & VS (2023). Insights into SEO using NLP and ML. IJACSA. DOI: 10.14569/ijacsa.2023.0140211.
