# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete

## [2026-06-09] create | Wiki initialized
- Domain: SEO / GEO for web applications (Angular/SPA focus)
- Structure: SCHEMA.md, index.md, log.md, raw/, entities/, concepts/, comparisons/
- Schema written with full tag taxonomy for technical SEO, on-page, off-page, GEO, analytics, and strategy

## [2026-06-09] ingest | SEO+GEO Agent Loop Methodology
- Source: PDF at ~/Downloads/seo_geo_agent_loop.pdf → raw/articles/seo-geo-agent-loop-methodology.md (1158 lines)
- Created: concepts/seo-geo-master-agent-loop.md — Full 12-phase loop (Phases 0–12)
- Created: concepts/geo-generative-engine-optimization.md — GEO framework with 4 academic papers
- Created: concepts/agenticgeo-inner-loop.md — MAP-Elites, Co-Evolving Critic, multi-turn rewriting
- Created: concepts/seo-loop-methodology.md — Traditional 6-phase human SEO practice
- Created: concepts/token-saving-strategies.md — 10 cost-reduction patterns
- Created: comparisons/human-vs-ai-seo-agent.md — Human vs. agent time/capability comparison
- Updated: index.md — Added all 6 new pages
- Key themes: ReAct pattern, tiered routing, dual-score optimization, freshness decay (~13 weeks), earned media bias in AI search

## [2026-06-09] create | aionAI-adapted-plan
- Created: concepts/aionai-adapted-plan.md — 6-phase adapted SEO/GEO plan specific to the aionAI Angular 21.2 SPA
- Adapted from the 12-phase loop: collapsed to 6 phases prioritizing crawlability, static foundation fixes, on-page GEO, prerendering, right-sized monitoring, and Greek-market authority building
- Key edits to generic research: fixed canonical bug identified, sitemap gap, static JSON-LD mismatch, hash routing crawlability concerns, reduced monitoring scope from 8 to 3 platforms, calibrated citation targets for Greek market
- Saved to memory for cross-session persistence
- Updated: index.md — Added under new "Concepts (aionAI-specific)" section

## [2026-06-10] create | aionAI Collaboration Model
- Created: concepts/aionai-collaboration-model.md
- Full collaboration workflow definition: RESEARCH → PROPOSE → REVIEW → COMMIT → OBSERVE → REPEAT
- 20 task backlog across 5 groups (A-E), each cross-referenced against SEO.md + seo-geo skill + Perplexity research
- Known bugs documented: phone mismatch (index.html vs app.ts), canonical URL bug, static JSON-LD issue
- Continuous loop design for future refinement
- Key tools inventory: SEO.md, seo-geo skill (Princeton 9 + audit framework + pre-publish checklist + keyword research), Perplexity research, competitor research
- Memory updated with collaboration model + user profile
- Updated: index.md — Added entry

## [2026-06-10] lint | First wiki health check
- Ran systematic lint on all 8 wiki pages + 1 raw source + SCHEMA compliance
- **Fixed:** seo-geo-master-agent-loop.md — added 5 outbound [[wikilinks]] (GEO, AgenticGEO, MAP-Elites archive, token-saving-strategies, seo-loop-methodology) and Related section
- **Fixed:** aionai-adapted-plan.md & aionai-collaboration-model.md — removed broken source references to entities/aionai-site.md (file never created)
- **Fixed:** aionai-adapted-plan.md — added link to [[aionai-collaboration-model]] in Related section
- **Noted:** entities/ directory empty (expected — only 1 source ingested); 14 of 37 tag taxonomy tags unused (expected for small corpus); no contradictions yet
- **Remaining:** 2 pages' `updated` dates not bumped (minor — pages last written today)

## [2026-06-10] op | Git repo initialized
- `git init` in ~/wiki-seo/ — root commit `e57ddd7`
- 12 files tracked (1 raw source, 8 wiki pages, SCHEMA.md, index.md, log.md)
- `.gitignore` added for Obsidian workspace + OS junk
- Wiki now has version history, branching, and rollback per the Karpathy pattern
