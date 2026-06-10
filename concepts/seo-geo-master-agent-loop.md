---
title: SEO + GEO Master Agent Loop
created: 2026-06-09
updated: 2026-06-10
type: concept
tags: [strategy, geo, technical-seo, content-optimization, audit, analytics, angular-seo]
sources: [raw/articles/seo-geo-agent-loop-methodology.md]
confidence: high
---

# SEO + GEO Master Agent Loop

The unified 12-phase loop for a combined SEO + [[geo-generative-engine-optimization|GEO]] AI agent, designed to target one content unit (URL or topic cluster) per full execution. Based on the ReAct (Reasoning + Acting) pattern extended with planning: **Observe → Plan → Act → Evaluate → Iterate**.

## The 12-Phase Loop

### Phase 0: Initialization & Memory Load
Load target URL/keyword from task queue. Query wiki/Obsidian for prior audits, historical metrics, previously applied fixes, content strategy notes, and the MAP-Elites strategy archive. Set baseline scores or null if first run. Check for interrupted state and resume if needed.

### Phase 1: Crawl & Technical Audit
Visit URL in real browser (Playwright/Chromium). Extract on-page signals (title, meta, H1, canonical). Async broken link scan. Core Web Vitals via PageSpeed API. HTTPS, sitemap, robots.txt checks. Schema markup inventory. Crawl depth and duplicate content checks. **Tiered routing**: deterministic Python checks first ($0), small model for meta suggestions (~$0.0001/URL), large model only when issues flagged (~$0.006/URL).

### Phase 2: Keyword & Intent Research
Extract primary keyword, expand to secondary + long-tail variants with volume, difficulty, and intent. Competitor keyword gap analysis. SERP intent analysis (what formats rank?). **GEO layer**: collect 20–30 real user prompts from Reddit, PAA boxes, social listening. Funnel mapping (awareness/consideration/decision). Entity map extraction.

### Phase 3: SERP + AI Engine Visibility Audit
Query SERP API for current rankings, AI Overview presence, featured snippets. Scan all 8 AI platforms (ChatGPT, Perplexity, Claude, Gemini, Google AI, Grok, Copilot, DeepSeek) using the prompt library. Record **Attributed Word Count** and **Position-Weighted Citation Order** per pattern. Calculate share of voice vs. competitors.

### Phase 4: Content Gap Analysis
Fetch top 20 SERP results. Identify entity gaps, topic gaps, structural gaps (TL;DR blocks, bullet summaries, FAQ sections, tables), citation gaps, freshness gaps, E-E-A-T gaps, word count gaps, and schema gaps. Prioritize by estimated impact.

### Phase 5: Content Strategy Generation
Define content objective (new/rewrite/section addition). Generate content brief with: primary + secondary keywords, entity requirements, word count, heading structure, internal linking strategy, required citations (≥20 high-authority domains/quarter), brand voice profile, GEO structural requirements, schema types, earned media targets. Select top rewriting strategies from [[agenticgeo-inner-loop|MAP-Elites archive]].

### Phase 6: Content Creation / Rewriting
Load brief + existing content + brand voice. Draft with answer-first structure, inline citations for every statistic, TL;DR block, FAQ section, data tables. Up to 3 [[agenticgeo-inner-loop|AgenticGEO]] multi-turn rewrite steps guided by critic. Dual-score pass: SEO score + [[geo-generative-engine-optimization|GEO]] score. If either below threshold, re-enter rewriting with specific fix instructions.

### Phase 7: On-Page + GEO Optimization
**SEO**: title tag (≤60 chars, keyword first 30), meta description (50–160 chars), exactly one H1, H2–H6 hierarchy, clean URL slug, image alt text, internal link audit, canonical tag, readability check (≤3 sentences/paragraph).
**GEO**: TL;DR block (40–80 words), FAQ schema with exact prompt phrasing, statistics with inline authoritative citations, table markup, author bio block, LLMs.txt, earned media check.
**Schema**: Article, FAQPage, HowTo, BreadcrumbList, Author — validate via Google Rich Results Test.

### Phase 8: Publishing + Schema Injection
Format for target CMS. Inject meta tags and schema JSON-LD in `<head>`. Upload images to CDN. Publish or queue for editorial review. Trigger sitemap regeneration. Submit URL to Google Search Console indexing API.

### Phase 9: Monitoring (8 Platforms)
Daily AI visibility scan (8 platforms). Weekly SEO ranking check (SERP API + GSC). Core Web Vitals regression alert. Backlink growth tracking. Alert thresholds: SEO position drop >3 positions, GEO Attributed Word Count drop >20%, organic sessions down >15% week-over-week.

### Phase 10: Diagnosis & Fix Generation
On alert: evaluate hypotheses — competitor content, freshness decay (~13-week half-life), intent shift, algorithm change, technical regression, entity drift. Generate fixes scored by impact/effort. Auto-apply technical fixes; queue major rewrites for review.

### Phase 11: Memory Update
Persist all learnings: final scores, fixes applied, strategy archive updates, critic preference pairs, keyword performance log, prompt effectiveness log, competitor intelligence, freshness schedule. Generate plain-English summary.

### Phase 12: Loop Decision
If next target in queue → Phase 0. If any page past review date → Phase 0. If queued fix → Phase 6. Otherwise IDLE.

## Key Design Principles

- **Single state machine** with explicit phase transitions and incremental state writes (crash-safe)
- **Multi-agent specialization** mirrors Yuksel & Sawaf (2024): specialized sub-agents outperform generalists
- **Dual-score optimization**: SEO and GEO scores sometimes pull in different directions — the agent must balance trade-offs explicitly
- **Tiered routing** minimizes cost (deterministic → cheap model → expensive model) — see [[token-saving-strategies]]
- **Human-in-the-loop checkpoints** for login walls, cornerstone content changes, broad schema changes

## References
- Aggarwal et al. (2023). GEO: Generative Engine Optimization. arXiv:2311.09735.
- Yuan et al. (2026). AgenticGEO. Semantic Scholar ID: 286762329.
- Frase (2026). AI Agents for SEO: Complete Guide.
- Profound (2025). 10-Step Framework for GEO.
- Oracle (2026). What Is the AI Agent Loop?
- Yuksel & Sawaf (2024). Multi-AI Agent Autonomous Optimization. arXiv:2412.17149.

## Related
- [[geo-generative-engine-optimization]] — The GEO framework
- [[agenticgeo-inner-loop]] — MAP-Elites archive and multi-turn rewriting
- [[token-saving-strategies]] — Cost optimization patterns
- [[seo-loop-methodology]] — Traditional human SEO practice
