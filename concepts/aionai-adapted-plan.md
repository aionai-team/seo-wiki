---
title: aionAI Adapted SEO/GEO Plan
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [strategy, angular-seo, geo, local-seo, citation-optimization, crawlability]
sources: [raw/articles/seo-geo-agent-loop-methodology.md]
confidence: medium
---

# aionAI Adapted SEO/GEO Plan

Adapted from the [[seo-geo-master-agent-loop]] 12-phase methodology for the aionAI project: an Angular 21.2 SPA with hash-based routing, Greek-language landing page, 3 views (home, privacy, terms), no SSR/prerendering.

## Core Differences from Generic Research

| Dimension | Research 12-Phase Loop | aionAI Adaptation |
|---|---|---|
| Site type | Generic multi-page | Angular 21.2 SPA, 3 hash views |
| Crawlability | Assumed | #1 risk — hash routing + `<app-root>` fallback |
| Language | English | Greek (el) + English (en) |
| Monitoring scope | 8 platforms | 3 platforms (GSC, Perplexity, ChatGPT Search) |
| Citation targets | 20 high-authority domains/quarter | Need Greek-market calibration |
| Content units | One URL per loop | Entire SPA is one JS bundle with 3 views |

## The 6-Phase Adapted Plan

### Phase 1: Crawlability & Render Audit
- Verify Googlebot renders Angular JS via URL Inspection Tool
- Confirm `#/privacy` and `#/terms` are indexable
- Test: what does no-JS curl see? (Currently `<app-root></app-root>`)
- Check robots.txt allows JS/CSS/API access

### Phase 2: Fix the Static Foundation
- **getCanonical() bug**: returns `https://aionai.gr/` for ALL views — fix to return per-view canonical
- **Static JSON-LD** in `index.html` hardcoded to homepage — dynamic update via `updateStructuredData()` may not execute if JS fails
- **Sitemap.xml**: only 1 URL — add all 3 views
- **Initial HTML fallback**: title/meta in `index.html` is good but should match each view

### Phase 3: Content & On-Page GEO
- **Answer-first structure**: hero already does this well; solutions section needs definition-first rewrite
- **FAQPage schema** injected dynamically for home view — but static HTML version in `index.html` would be safer
- **Missing GEO elements**: TL;DR block (40-80 words), inline citations with hyperlinks, author bio block
- **FAQ prompts** already use natural Greek queries — strong for Perplexity matching

### Phase 4: Prerendering
- Full SSR is overkill; lightweight build-time prerendering is the right move
- Angular 21 `@angular/build` application builder supports `ssr` and `prerender` — but custom hash routing blocks OOTB use
- Alternative: static HTML generation per view post-build via headless browser
- Minimum: ensure each view has correct title/meta in the initial `index.html` served

### Phase 5: Right-Sized Monitoring
| Platform | Method | Cadence |
|---|---|---|
| Google Search Console | Track organic traffic, Core Web Vitals | Weekly |
| Perplexity | Manual check of 5 key Greek prompts | Weekly |
| ChatGPT Search | Manual check | Bi-weekly |
- Skip Grok, Copilot, DeepSeek, Gemini for now
- 13-week freshness alert (from research) — manual check, not automated

### Phase 6: Authority Building
- **Google Business Profile**: single highest-impact local SEO move — complete it (currently has TODOs)
- **Greek directories**: Vrisko, Κατάλογος Επιχειρήσεων — consistent NAP
- **LinkedIn**: company page + individual profiles (LLMs scrape LinkedIn)
- **Greek tech/business blogs**: guest posts on AI automation for Greek SMEs
- **Original research**: even small survey of Greek SME AI adoption earns citations

## Quick Wins (<10 minutes each)
1. Fix `getCanonical()` to return per-view URL
2. Update `sitemap.xml` with all 3 views
3. Add TL;DR block at page top
4. Add inline citations to statistical claims
5. Complete Google Business Profile

## Related
- [[aionai-collaboration-model]] — Collaboration workflow & task backlog
- [[seo-geo-master-agent-loop]] — Parent methodology
- [[geo-generative-engine-optimization]] — GEO framework
- [[seo-loop-methodology]] — Human SEO baseline
- [[token-saving-strategies]] — Cost optimization patterns
