---
title: aionAI Collaboration Model & Task Backlog
created: 2026-06-10
updated: 2026-06-10
type: concept
tags: [workflow, collaboration, task-management, aionai-specific, loop-definition]
confidence: high
sources:
  - concepts/aionai-adapted-plan.md
  - SKILL.md from seo-geo skill (Princeton 9, audit framework, pre-publish checklist, keyword research)
  - SEO.md (4-layer framework)
  - concepts/seo-geo-master-agent-loop.md (12-phase loop methodology)
---

# aionAI Collaboration Model & Task Backlog

## How We Work Together

**The core loop:**

```
  RESEARCH ── I use all tools: SEO.md + seo-geo skill (Princeton 9,
  │            audit framework, pre-publish checklist) + Perplexity
  │            research + competitor analysis of Greek AI agency sites
  │
  ▼
  PROPOSE  ── I write up one specific change with exact diffs,
  │            cross-referenced against all sources. Show before/after.
  │            Explain which layer it fixes and why it matters.
  │
  ▼
  REVIEW   ── You check it. Say yes, no, or "adjust X".
  │            I never touch code without your approval.
  │
  ▼
  COMMIT   ── You merge. I record what was done in wiki + memory.
  │
  ▼
  OBSERVE  ── I track results. Next cycle I check whether the change
  │            moved the needle using GSC + visibility prompts.
  │
  ▼
  REPEAT   ── Next task, next proposal. Each pass is smarter.
              Wiki records what worked and what didn't.
```

**Rules of engagement:**
- One task at a time. Never the whole plan at once.
- Exact diffs, not general suggestions.
- Nothing merged without user approval.
- User may provide custom scripts to improve or automate loops later.
- User reviews the loop itself as we go — we refine together.

## Tools I Have (and must use in every proposal)

| Tool | What it is | I use it for |
|---|---|---|
| **SEO.md** | User's 4-layer framework | Primary diagnostic: Findable → Understandable → Intent-matched → Trustworthy |
| **seo-geo skill** (SKILL.md + 8 refs) | Battle-tested SaaS SEO/GEO methodology: Princeton 9, biweekly audit, pre-publish checklist, keyword research, content clusters | On-page optimization, GEO scoring, audit structure, schema checks, AI bot access |
| **Perplexity research** (wiki) | 12-phase agent loop, MAP-Elites archive, AgenticGEO inner loop, 13-week freshness decay | Continuous improvement cycle design, monitoring cadence, quality-diversity optimization |
| **Competitor research** | Manual analysis of top Greek AI agency sites | Market benchmarking, content gap detection, structure inspiration |

## Task Backlog — aionAI SEO/GEO Foundation

All 20 tasks, cross-referenced against all tools. Stored here for sequential approval.

### Group A: Crawlability & Technical Foundation

| # | Task | Sources | Est. time | Status |
|---|---|---|---|---|
| A1 | **Fix canonical URLs** — `getCanonical()` returns `https://aionai.gr/` for ALL views. Privacy and terms pages marked as duplicates of homepage. Fix: return per-view URL based on hash. File: `src/app/app.ts` ~line 323 | SEO(L1), K, R | 2 min | pending |
| A2 | **Update sitemap.xml** — currently only has homepage URL. Add all 3 views with proper hash URLs. File: `public/sitemap.xml` | SEO(L1), K, R | 5 min | pending |
| A3 | **Update robots.txt** — explicitly allow GPTBot, ClaudeBot, PerplexityBot, Google-Extended. Current file has generic `Allow: /` which works but explicit is safer for GEO. File: `public/robots.txt` | K (geo-optimization.md), R | 2 min | pending |
| A4 | **Static JSON-LD in index.html** — hardcoded to homepage. Dynamic `updateStructuredData()` in `app.ts` may not execute if JS fails. Fix: either make static JSON-LD match each view, or rely solely on dynamic. Need validation. File: `src/index.html` | SEO(L1+2), K | 10 min | pending |
| A5 | **Verify AI bot crawlability** — use Google URL Inspection Tool to test render of `#/privacy` and `#/terms`. Test with `curl -A "GPTBot"` to see what no-JS bots receive. | SEO(L1), K | 15 min | pending |

### Group B: On-Page & Schema

| # | Task | Sources | Est. time | Status |
|---|---|---|---|---|
| B1 | **Heading hierarchy audit** — check H1 count per view (home has 1 in hero). Privacy and terms need their own unique H1. Verify H2→H3 structure. | SEO(L2), K | 10 min | pending |
| B2 | **Schema markup validation** — run JSON-LD through Google Rich Results Test. FAQPage schema is dynamic — verify it works. | K (pre-publish), R | 10 min | pending |
| B3 | **Unique title/meta per view** — set dynamically in `app.ts` (good). Verify lengths: titles ≤60 chars, descriptions ≤160 chars. | SEO(L2), K | 5 min | pending |
| B4 | **TL;DR summary block** — add 40-80 word Greek summary at top of homepage. Critical for GEO extraction. | K (Princeton 9), R | 10 min | pending |
| B5 | **Inline citations** — add 2-3 cited statistics with sources to solutions/use-cases. Princeton 9: +40% visibility. | K (geo-optimization.md), R | 15 min | pending |
| B6 | **Author bio / E-E-A-T** — add author/agency bio with credentials. Trust signal for both Google and LLMs. | SEO(L4), K, R | 10 min | pending |

### Group C: Content & GEO Optimization

| # | Task | Sources | Est. time | Status |
|---|---|---|---|---|
| C1 | **Answer-first rewrite** — solutions section: move "Τι είναι οι AI Agents" definition to very first sentence. | SEO(L3), K, R | 15 min | pending |
| C2 | **FAQ self-contained check** — verify each FAQ answer works as standalone (LLMs extract individual Q&A pairs). | K (geo-optimization.md) | 10 min | pending |
| C3 | **Service areas in visible text** — neighborhoods exist in JSON-LD but not prominently in page copy. Add a sentence in hero or solutions. | SEO(L3), K | 5 min | pending |
| C4 | **Comparison table** — "aionAI vs έτοιμο λογισμικό" or service comparison. LLMs love extracting tables. | K (Princeton 9, content-strategy) | 20 min | pending |
| C5 | **FAQ validation** — current questions mirror real user searches (strong). Validate against actual sales call questions. | SEO(L3), R | 15 min | pending |

### Group D: Local SEO & Authority

| # | Task | Sources | Est. time | Status |
|---|---|---|---|---|
| D1 | **Complete Google Business Profile** — #1 local SEO signal. Name, address, phone, hours, services, photos, categories. Currently a TODO. | SEO(L4), K, R | 30 min | pending |
| D2 | **NAP consistency check** — ⚠️ **BUG FOUND**: `index.html` has `+30-210-0000000` (placeholder) but `app.ts` has `+30 6934613555` (real). Must be identical everywhere. | SEO(L4), K | 5 min | pending |
| D3 | **LLMs.txt validation** — file exists at `/llms.txt`. Verify it has: summary, all 3 view URLs, key facts, FAQ. | K (geo-optimization.md), R | 10 min | pending |
| D4 | **LinkedIn company page** — even basic page helps. LLMs scrape LinkedIn heavily. Currently a TODO. | SEO(L4), K, R | 20 min | pending |
| D5 | **Greek directory citations** — Vrisko, Κατάλογος Επιχειρήσεων, etc. Consistent NAP across all. | SEO(L4), K | 30 min | pending |

### Group E: Monitoring & Loop

| # | Task | Sources | Est. time | Status |
|---|---|---|---|---|
| E1 | **Set up biweekly audit template** — based on seo-geo audit framework. Adapt for landing page (not blog). | K (audit-framework.md) | 10 min | pending |
| E2 | **Connect Google Search Console** — verify setup for aionai.gr, export initial performance baseline. | K (onboarding), R | 15 min | pending |

## Known Bugs Found

1. **Phone mismatch** — `index.html` line 53: `+30-210-0000000` (placeholder). `app.ts` line 144: `+30 6934613555` (real). This blocks NAP consistency (D2) and should be fixed early.
2. **Canonical URL** — `getCanonical()` always returns homepage URL regardless of current hash view. Blocks correct indexing of privacy/terms pages.
3. **Static JSON-LD** — hardcoded to homepage in `index.html`. Dynamic injection via `updateStructuredData()` may or may not override this successfully depending on JS execution timing.

## The Continuous Loop (future refinement)

When the foundation is solid, the loop becomes:

```
Week A ── MONITOR  ── Check GSC data + visibility prompts
    │
    ▼
Week A ── DIAGNOSE ── Any changes? Alerts? Declines? Competitors?
    │
    ▼
Week A ── OPTIMIZE ── Apply small change
    │
    ▼
Week B ── VERIFY   ── Did it move the needle?
    │
    ▼
Week B ── LOG      ── What worked? Store in wiki + memory.
    │
    ▼
   REPEAT
```

User will co-design this loop and may provide custom scripts for automation.

## Related
- [[aionai-adapted-plan]] — Original 6-phase plan
- [[seo-geo-master-agent-loop]] — Parent methodology
- [[geo-generative-engine-optimization]] — GEO framework
