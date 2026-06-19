---
title: SEO + GEO Agent Loop Methodology
created: 2026-06-09
updated: 2026-06-14
type: summary
tags: [seo, geo, pipeline, automation, data-flow, script]
sources: []
confidence: high
source_url: file:///home/bog/Downloads/seo_geo_agent_loop.pdf
ingested: 2026-06-09
sha256: 3198ada744de0dbede2679ed174ee31fe918a7e3173bddfb69a48e3a738dd6e0
---

SEO + GEO AI Agent: Loop Methodology & Step
Breakdown
Audience: Developer implementing a Hermes AI agent with Obsidian as persistent
memory.
Purpose: Authoritative technical reference for the complete SEO + GEO agent loop —
every phase, every sub-step, every design decision — grounded in peer-reviewed
research and production implementations.
Version: 1.0 | Compiled June 2026

Relevant wiki pages: [[geo-generative-engine-optimization]], [[seo-loop-methodology]], [[agenticgeo-inner-loop]], [[seo-geo-master-agent-loop]], [[token-saving-strategies]], [[human-vs-ai-seo-agent]].

Table of Contents
1. Introduction
2. The Core Agent Loop Philosophy
3. The Master Agent Loop — All Steps
4. Methodology Deep Dive — SEO Loop
5. Methodology Deep Dive — GEO Loop
6. Human vs. AI Agent Comparison Table
7. AgenticGEO Inner Loop
8. Token-Saving Notes for Hermes
9. Sources

1. Introduction
This document defines the complete loop methodology for a unified SEO + GEO AI agent. It is
intended as a developer-facing reference for implementing the agent in the Hermes framework
with Obsidian acting as long-term, structured memory.

What SEO and GEO Are
Search Engine Optimization (SEO) is the practice of improving a page's visibility in traditional
keyword-ranked search results (Google, Bing). It operates across technical signals, on-page
content quality, and off-page authority. The discipline relies on keyword intent matching, E-EA-T signals, and backlink authority (Padma & VS, 2023).
Generative Engine Optimization (GEO) is a distinct, newer paradigm. Aggarwal et al. (2023)
define it as the first formal framework for "optimizing content to increase its visibility in
generative engine responses" — outputs from LLM-powered search engines such as ChatGPT,
Perplexity, Claude, and Google AI Overview (Aggarwal et al., 2023, arXiv:2311.09735). Unlike SEO,
GEO measures visibility via Attributed Word Count (how many words from a source are used in

the engine's answer) and Position-Weighted Citation Order (how early in the answer the source
appears).
Answer Engine Optimization (AEO) targets direct-answer contexts (voice search, featured
snippets, PAAs). Manasa et al. (2025) frame SEO, GEO, and AEO as a unified continuum sharing
critical factors: structured data, entity recognition, authoritative sourcing, and intent-driven
content design (Manasa et al., 2025, DOI:10.55041/ijsrem53069).

Why an Agent Must Do Both
AI search engines process over 1 billion prompts per day (ChatGPT alone) and have an
overwhelming bias toward earned media — third-party authoritative sources — rather than
brand-owned content, unlike Google (Chen et al., 2025, arXiv:2509.08919). LLMs cite only 2–7
domains per response on average, versus Google's 10 blue links (Profound, 2025). A content team
that optimizes only for Google is invisible to the 71%+ of users who now use AI search to
research purchases. Conversely, a GEO-only approach sacrifices the existing organic channel.
An AI agent is uniquely suited to manage both channels simultaneously because:
The monitoring surface spans 8+ platforms continuously
The optimization signals from both channels sometimes conflict and require trade-off
reasoning
Content freshness cycles differ: Google rankings persist months to years; AI citations decay
after approximately 13 weeks without updates (Frase, 2026)
The full pipeline (research → publish → monitor → fix) involves 9–14 hours of manual work
per article, reducible to 30–60 minutes with a properly architected agent (Frase, 2026)

2. The Core Agent Loop Philosophy
The Observe → Plan → Act → Evaluate → Iterate Pattern
All AI agent architectures reduce to a continuous decision cycle. The canonical formulation is
the ReAct (Reasoning + Acting) pattern: the agent thinks, takes an action, observes the result,
and repeats until the goal is satisfied (Oracle Select AI Agent documentation).
Expanded for a planning-capable agent:
OBSERVE → PLAN → ACT → EVALUATE → ITERATE
↑
|
└────────────────────────────────────────────┘

Each beat:
Beat

Description

Observe

Perceive environment state: current rankings, AI visibility scores, page content, Obsidian memory, crawl data

Beat

Description

Plan

Reason about the gap between current state and goal; select tools and sequence of sub-tasks

Act

Execute tool calls: crawl, write, publish, call APIs

Evaluate

Compare observations against goal criteria and success metrics

Iterate

Update memory (Obsidian), decide whether to continue, re-run, or escalate

The Oracle AI agent loop article describes this as a "self-sustaining intelligence cycle" where agents
perceive, reason, plan, act, and learn continuously — with each loop making the agent
incrementally more informed. The Hugging Face agents course formalizes the same cycle as
Thought → Action → Observation, noting that the loop continues via a while condition until the
objective is fulfilled.

Multi-Agent Specialization
Yuksel & Sawaf (2024) demonstrate that a 5-agent loop — Refinement → Execution → Evaluation
→ Modification → Documentation — achieves optimal performance autonomously by generating
and testing hypotheses without human input, using iterative LLM-powered feedback loops
(Yuksel & Sawaf, 2024, arXiv:2412.17149). The key insight: specialized sub-agents working in
sequence outperform a single generalist agent on every measurable quality dimension,
mirroring how specialized human content teams outperform generalists.
For the Hermes implementation, each major phase of the master loop can be delegated to a
specialized sub-agent, with Obsidian serving as the shared memory bus.

Memory Architecture (Obsidian)
Obsidian serves three memory functions in this agent:
1. Working memory: Active task context, current target URL/keyword, loop state, in-flight
results
2. Episodic memory: Prior audit results, historical visibility scores, what fixes were applied
and what their outcomes were
3. Strategy memory: The MAP-Elites-style archive of rewriting strategies that worked for
which content types (see Section 7)
Each note maps to a domain, a page, or a strategy. The agent reads relevant notes at Phase 0 and
writes updated notes at Phase 11.

3. The Master Agent Loop — All Steps
This is the unified SEO + GEO loop. It should be implemented as a single state machine with
explicit phase transitions. The loop targets one content unit (URL or topic cluster) per full
execution.

Phase 0: Initialization & Memory Load
Goal: Establish current context from persistent memory before any external calls.
Sub-steps:
1. Load target: Read target URL, keyword, or topic from task queue (Obsidian inbox note or
external task feed)
2. Load memory: Query Obsidian for:
Prior audit results for this URL/domain
Historical SEO metrics (traffic, rankings, backlinks)
Historical GEO metrics (visibility scores, citation platforms)
Previously applied fixes and their outcomes
Content strategy notes (brand voice, tone, personas)
MAP-Elites strategy archive (see Section 7)
3. Set baseline: If prior data exists, load it as the evaluation baseline. If no prior data, set
baseline to null (first run).
4. Confirm loop state: Check if a previous run was interrupted mid-loop (incremental state
file). Resume from the last completed phase if so.
5. Load config: API keys, target platforms list (ChatGPT, Perplexity, Claude, Gemini, Google AI,
Grok, Copilot, DeepSeek), CMS credentials, crawl rate limits.
Obsidian note structure for this phase:
--target_url: https://example.com/page
last_audit: 2026-05-01
loop_phase: 0
---

Output: Populated context object passed to all subsequent phases.

Phase 1: Crawl & Technical Audit
Goal: Identify all technical SEO issues on the target URL and its domain context.
Sub-steps:
1. Visit URL in real browser (Playwright/Chromium, not headless): Render JavaScript, detect
login walls, redirects, and 404s (dannwaneri/seo-agent, GitHub)
2. Extract on-page signals via LLM API call:
Title tag (check: present, ≤60 chars, keyword upfront)
Meta description (check: present, 50–160 chars)
H1 count (check: exactly one)

H2–H6 hierarchy
Canonical tag (present, self-referential or correct cross-domain)
3. Async broken link check: httpx async scan of all same-domain outbound links; log non-200
responses
4. Edge case handling: If 404 / login wall / redirect chain detected → log to needs_human[] in
state; continue to next URL if in --auto mode
5. Core Web Vitals: LCP, CLS, INP via PageSpeed API or Playwright performance metrics
6. Mobile-friendliness: Viewport check; load time target <1.8s (Profound, 2025)
7. HTTPS check: Confirm TLS; flag mixed content
8. Sitemap & robots.txt: Confirm URL is included/not blocked
9. Schema markup inventory: Detect existing structured data types (Article, FAQ, HowTo,
Author, BreadcrumbList); flag missing types relevant to content
10. Crawl depth check: Count clicks from homepage; flag if >4
11. Duplicate content scan: Check canonical chain for duplicates
12. Write to state file incrementally: Each URL result written to report.json immediately —
safe to interrupt
Output: technical_audit.json with PASS/FAIL per signal, written to Obsidian audit note.
Tiered routing (cost optimization): Run deterministic Python checks (title length, H1 count,
link parsing) at Tier 1 ($0), Haiku for meta description suggestions at Tier 2 (~$0.0001/URL), full
Sonnet extraction only when Tier 1/2 flag issues at Tier 3 (~$0.006/URL) (dannwaneri/seo-agent).

Phase 2: Keyword & Intent Research
Goal: Map the full keyword landscape for the target topic including search volume, difficulty,
intent, and long-tail variants.
Sub-steps:
1. Primary keyword extraction: Identify target keyword from Obsidian note or derive from
page content
2. Keyword expansion: Retrieve secondary and long-tail variants via SERP API or keyword
tool; annotate each with: search volume, keyword difficulty, search intent (informational /
navigational / transactional / commercial)
3. Competitor keyword gap analysis: Identify keywords competitors rank for that the target
page does not
4. SERP intent analysis: For each target keyword, what content format is ranking? (listicle,
how-to, definition, comparison) — this determines the content format the agent should
produce
5. Question/prompt research (GEO layer): Collect real user prompts from:
Reddit threads on the topic

"People Also Ask" boxes in SERP results
Sales call transcripts (if available in Obsidian memory)
Social listening data
Target 20–30 unique prompts per core topic (Profound, 2025)
6. Funnel mapping: Assign each prompt/keyword to a funnel stage (awareness /
consideration / decision) per the Profound framework
7. Entity map: Extract named entities relevant to the topic; identify which entities topranking pages reference (these become required entities for content creation)
Output: keyword_map.json + prompt_library.json, written to Obsidian research note.

Phase 3: SERP + AI Engine Visibility Audit
Goal: Establish quantitative baselines for both traditional search visibility and AI engine
citation presence.
Sub-steps:
1. SERP position check: For each target keyword, query SERP API (e.g. SerpApi) and record:
Current ranking position for target URL
Presence of AI Overview, Featured Snippet, PAA, Image Pack, Video Results, Local Pack
(dannwaneri/seo-agent SERP feature detector)
Top 20 ranking pages (for gap analysis)
2. AI engine visibility scan: Query each of the 8 monitored AI platforms (ChatGPT, Perplexity,
Claude, Gemini, Google AI, Grok, Copilot, DeepSeek) with the 20–30 prompts from Phase 2;
record:
Whether target domain appears in response
Attributed Word Count (word): Count of words from target page used in engine's
answer (Aggarwal et al., 2023)
Position-Weighted Citation Order (pos): Position of first citation of target domain in
response (AgenticGEO, Yuan et al., 2026)
Sentiment of AI references to brand/content
3. Share of voice: Calculate what percentage of AI responses in category cite target domain vs.
competitors
4. Competitor citation audit: Identify which competing domains are being cited and why
(what content elements likely triggered citation)
5. Cross-platform stability check: Chen et al. (2025) find that AI search engines differ
significantly in domain diversity, freshness bias, cross-language stability, and phrasing
sensitivity — log platform-specific patterns (Chen et al., 2025, arXiv:2509.08919)
6. Baseline record: Write all scores to Obsidian with timestamp; this is the comparison
baseline for Phase 10

Output: visibility_baseline.json with per-platform scores, written to Obsidian monitoring
note.

Phase 4: Content Gap Analysis
Goal: Identify exactly what the target content is missing compared to top-ranking and top-cited
competitors.
Sub-steps:
1. Fetch top 20 SERP results: Extract full text of top-ranking pages (Frase pipeline Stage 1)
2. Entity gap: Which named entities appear in ≥3 competitor pages but not in the target page?
3. Topic gap: Which subtopics, sections, or questions do competitors answer that the target
page does not?
4. Structural gap: Are competitors using content structures that aid AI citation? (TL;DR
blocks, bullet summaries, FAQ sections, tables, data with inline citations)
5. Citation gap: Are competitors citing authoritative external sources (original research,
government data, industry reports) that the target page lacks?
6. Freshness gap: Is competitor content more recently updated? (AI citations decay faster
than Google rankings — approximately 13-week half-life without updates) (Frase, 2026)
7. E-E-A-T gap: Do competitors have more visible author expertise signals (bios, credentials,
publication records)?
8. Word count gap: Note recommended word count based on competitor mean/median (for
Google ranking); note that for GEO, answer-first structured content sometimes outperforms
length
9. Schema gap: Which schema types do competitors implement that the target page lacks?
Output: content_gap_report.json, prioritized by impact estimate, written to Obsidian gap note.

Phase 5: Content Strategy Generation
Goal: Produce a complete, actionable content brief that an LLM writer (Phase 6) can execute
against.
Sub-steps:
1. Define content objective: New article, rewrite of existing, or section additions only (based
on gap severity from Phase 4)
2. Generate content brief containing:
Primary keyword + secondary keywords + entity requirements
Recommended word count
Heading structure (H1, H2, H3 outline)
Internal linking strategy (which existing pages to link to/from, with anchor text)

Required external citations (≥20 high-authority domain citations per quarter target)
(Profound, 2025)
Competitive positioning statement
Brand voice profile (loaded from Obsidian memory)
GEO structural requirements: TL;DR block, FAQ section, tables, inline citations for
every statistic
Schema types to implement
Earned media targets (which third-party publications to pitch for links/mentions)
3. Align with business KPIs: Map content objective to business outcomes (leads, revenue,
signups) — GEO KPIs tracked separately from SEO KPIs (Profound Step 1)
4. Select rewriting strategies: Query the MAP-Elites strategy archive from Obsidian memory;
select top-N strategies ranked by critic score for this content type (see Section 7)
Output: content_brief.md, written to Obsidian strategy note.

Phase 6: Content Creation / Rewriting
Goal: Produce a full draft that simultaneously scores well for SEO and GEO criteria, or rewrite
targeted sections of existing content.
Sub-steps:
1. Load content brief from Phase 5 output
2. Load existing content (if rewrite): Fetch current live page text as base
3. Load brand voice profile from Obsidian memory
4. LLM drafting (Hermes as rewriter):
Write full draft following heading outline from brief
Include required entities throughout at recommended density
Maintain fact density: every statistical claim needs an inline hyperlinked citation
Use answer-first structure for each section: state the key point in the first sentence,
then elaborate
Include TL;DR block at top of article
Include FAQ section using exact phrasing from prompt library (Phase 2)
Include data tables where relevant (comparisons, benchmarks)
5. AgenticGEO multi-turn rewriting (up to 3 rewrite steps): Apply critic-guided strategy
selection to iteratively improve the draft's GEO visibility score (see Section 7; Yuan et al.,
2026)
6. SEO scoring pass: Score draft against:
Keyword placement (title, H1, first 100 words, subheadings)
Internal link count and anchor text quality

Word count vs. target
Entity coverage completeness
7. GEO scoring pass: Score draft against:
Attributed Word Count estimate
Structured summary quality
Citation density
FAQ coverage of target prompts
8. Dual-score loop: If either SEO score or GEO score is below threshold, re-enter rewriting
with specific instructions to fix the failing dimension. Note: SEO and GEO sometimes pull
in different directions (persuasive brand voice vs. neutral factual tone; long-form depth vs.
answer-first structure) — the agent must balance trade-offs explicitly (Frase, 2026)
9. Write draft to Obsidian: Save versioned draft with both scores recorded
Output: draft_v[n].md with SEO score and GEO score metadata, written to Obsidian content
note.

Phase 7: On-Page + GEO Optimization
Goal: Apply all on-page SEO signals and GEO structural elements to the draft before publishing.
On-page SEO sub-steps:
1. Title tag: ≤60 chars, primary keyword in first 30 chars
2. Meta description: 50–160 chars, includes primary keyword and clear value proposition
3. H1: Exactly one, contains primary keyword
4. H2–H6 hierarchy: Logical nesting; secondary keywords distributed across H2s
5. URL slug: Clean, hyphenated, keyword-containing, no stop words
6. Image optimization: Alt text for all images (keyword-relevant, descriptive); file names
keyword-matched
7. Internal link audit: Confirm all required internal links present; confirm target page
receives links from relevant existing pages (no orphan pages)
8. Canonical tag: Set to self (unless cross-domain canonical is required)
9. Readability check: Target reading level appropriate for audience; paragraph length ≤3
sentences for AI scannability
GEO structural sub-steps:
1. TL;DR block: 40–80 word summary at article top; direct answer to primary prompt
2. FAQ schema preparation: Each FAQ item uses exact phrasing from the prompt library;
answer is ≤100 words per question
3. Statistics and citations: Every numerical claim has an inline hyperlinked source to
authoritative domain (government data, peer-reviewed research, industry reports)

4. Table markup: Data comparisons formatted as Markdown tables (render as HTML <table>)
5. Author bio block: Visible author credentials, publication history, organizational affiliation
(E-E-A-T signal for AI source selection)
6. LLMs.txt: Confirm domain-level LLMs.txt file exists and is current (Profound, 2025)
7. Earned media check: Verify any third-party citations to the page exist; if not, queue
outreach task in Obsidian
Schema markup generation sub-steps:
1. Generate Article schema (headline, author, datePublished, dateModified, publisher)
2. Generate FAQPage schema from FAQ section
3. Generate HowTo schema if content is procedural
4. Generate BreadcrumbList schema
5. Generate Author schema with credentials
6. Validate all schema via Google Rich Results Test API
Output: Finalized optimized_draft.md + schema_markup.json

Phase 8: Publishing + Schema Injection
Goal: Format content for CMS, inject all metadata and schema, publish or queue for editorial
review.
Sub-steps:
1. CMS formatting: Convert Markdown to target CMS format (HTML blocks, Gutenberg
blocks, Sanity portable text, Webflow rich text, etc.)
2. **Meta tag injection**: Set `<title>`, `<meta name=\"description\">`, `<link rel=\"ca

3. Schema injection: Embed all structured data as <script type=\"application/ld+json\"> in
page <head>
4. Internal links: Confirm all internal link hrefs resolve to live URLs before publish
5. Image upload: Upload all images to CDN; replace Markdown image references with CDN
URLs
6. CMS publish or queue: Publish directly if agent has publish permissions; otherwise create
draft and add to editorial queue in Obsidian inbox
7. Sitemap update: Trigger sitemap regeneration or ping Google Search Console
8. Indexing request: Submit URL to Google Search Console indexing API
9. State update: Record publish timestamp in Obsidian page note; this is the anchor for
freshness decay monitoring
Output: publish_confirmation.json with live URL, publish timestamp, and schema validation
status.

Phase 9: Monitoring (SEO + GEO Across 8 Platforms)
Goal: Continuous post-publish tracking of performance across all search channels.
Sub-steps (run on schedule — daily for AI visibility; weekly for SEO rankings):
1. SEO ranking check:
Query SERP API for each target keyword; record position
Query Google Search Console API: impressions, clicks, CTR, average position
Track Core Web Vitals via PageSpeed API (performance regression alert)
Backlink growth: New referring domains since last check
2. AI visibility scan (8 platforms: ChatGPT, Perplexity, Claude, Gemini, Google AI, Grok,
Copilot, DeepSeek):
Send each platform the 20–30 prompts from the prompt library
Record Attributed Word Count and Position-Weighted Citation Order per prompt per
platform (Aggarwal et al., 2023)
Track sentiment delta (positive / neutral / negative brand mentions)
Track share of voice vs. competitors
3. Cross-platform stability logging: Chen et al. (2025) document that different AI engines have
distinct biases (domain diversity, freshness, cross-language stability, phrasing sensitivity)
— log platform-specific patterns to Obsidian for strategy refinement (Chen et al., 2025,
arXiv:2509.08919)
4. Trend analysis: Compare current scores against baseline (Phase 3) and against prior
monitoring snapshots; compute delta and trend direction
5. Alert thresholds:
SEO: Position drops >3 positions on any target keyword → trigger Phase 10
GEO: Attributed Word Count drops >20% on any platform → trigger Phase 10
Traffic: Organic sessions down >15% week-over-week → trigger Phase 10
6. Write monitoring snapshot to Obsidian: Timestamped note with all platform scores;
append to historical time series
Output: monitoring_snapshot_[timestamp].json, written to Obsidian monitoring log.

Phase 10: Diagnosis & Fix Generation
Goal: When a performance drop is detected, diagnose the root cause and generate specific,
actionable fixes ready to apply.
Sub-steps (the Frase Content Watchdog Level 3 approach):

1. Fetch triggering signal: Load the drop alert from Phase 9 (which metric, which platform,
magnitude)
2. Root cause diagnosis — evaluate each hypothesis:
Competitor content: Did a competitor publish new/updated content that is now
ranking or being cited above the target page?
Freshness decay: Is the content more than 90 days old without an update? Has AI
citation rate dropped in line with the ~13-week decay curve?
Intent shift: Has the dominant search intent for this keyword changed (e.g. from
informational to commercial)?
Algorithm change: Is the drop domain-wide (algorithm update) or page-specific
(content issue)?
Technical regression: Did a site change break crawlability, canonical tags, schema, or
page speed?
Entity drift: Are new entities now dominant in the topic that the target page does not
cover?
3. Generate fixes (specific to diagnosed cause):
New sections to add (topic gap fix)
Statistics to update with fresher data (freshness fix)
Schema markup to add or correct (technical fix)
Entities to incorporate (entity gap fix)
New authoritative citations to add (citation authority fix)
Prompt library update (new user queries to target)
4. Prioritize fixes: Score each fix by estimated visibility impact / effort ratio
5. Apply or queue:
Auto-apply: Technical fixes (schema, canonical, meta tags), freshness updates to
statistics with direct replacement sourcing
Queue for review: Major content rewrites, new section additions requiring expert
knowledge
Write fix queue to Obsidian inbox
6. Re-trigger Phase 6: If content rewriting is required, pass the diagnosed content brief back
to Phase 6 with specific instructions
Output: fix_plan_[timestamp].json, written to Obsidian fix-queue note.

Phase 11: Memory Update (Save to Obsidian)
Goal: Persist all learnings from this loop run to Obsidian so future runs benefit from
accumulated knowledge.
Sub-steps:
1. Update page note: Record final SEO and GEO scores, fixes applied, publish/update
timestamp, current rankings
2. Update strategy archive: If any rewriting strategy produced measurable GEO visibility
improvement, add to MAP-Elites archive (see Section 7); record content type, domain,
strategy applied, score delta
3. Update critic model preferences: If Co-Evolving Critic is active (see Section 7), write new
preference pairs from this run's outcomes
4. Update keyword performance log: Record which keywords moved, in which direction, for
future prioritization
5. Update prompt effectiveness log: Which prompts triggered citations, which did not —
refine the prompt library
6. Update competitor intelligence: What new competitor content was found; what entity gaps
were identified
7. Update freshness schedule: Set next-review date for this page (default: 90 days; 30 days if
GEO-critical)
8. Summarize run: Generate plain-English summary note in Obsidian: what was done, what
improved, what is queued
Obsidian note update pattern:
## Audit Log — [ISO timestamp]
- SEO score: [n] (delta: [+/-n])
- GEO score: [n] (delta: [+/-n])
- Platforms cited: [list]
- Fixes applied: [list]
- Next review: [date]
- Strategy used: [strategy_id from MAP-Elites archive]

Output: Updated Obsidian vault with all run artifacts; state machine set to complete.

Phase 12: Loop Decision (Continue or Re-Run)
Goal: Decide what the agent does next.
Decision tree:
Is there a next target in the task queue?
├── YES → Return to Phase 0 with next target
└── NO → Is any page in Obsidian past its review date?

├── YES → Return to Phase 0 with that page as target
└── NO → Is any fix queued in the Obsidian inbox?
├── YES → Execute highest-priority fix (Phase 6 → 7 → 8)
└── NO → IDLE — set next wake time based on earliest review date

Loop termination conditions:
Task queue empty AND no pages past review date AND no queued fixes
Maximum daily budget exceeded (cost-capping parameter)
Human intervention required (edge case flagged in needs_human[])

4. Methodology Deep Dive — SEO Loop
The human SEO methodology, as practiced by professionals using SEMrush, AMA standards,
and LinkedIn SEO expert consensus, breaks into six phases. The AI agent automates these
phases end-to-end.

Phase A — Audit & Baseline
Collect benchmark data: organic traffic, keyword rankings, bounce rate, conversion rate,
backlink count. Crawl the site to identify crawl errors, broken links, duplicate content, and
technical blockers. Verify indexing status and clean unnecessary indexed pages. Test mobilefriendliness and analyze page speed against Core Web Vitals benchmarks (LCP, CLS, INP).
The dannwaneri/seo-agent implementation demonstrates the production pattern: visit each URL
in a real Chromium browser (not headless), extract signals via LLM API, run async broken-link
detection, write results incrementally to JSON (crash-safe), and generate a plain-English
summary on completion.

Phase B — Research
Keyword research targeting primary, secondary, and long-tail keywords; annotate with search
volume, keyword difficulty, and intent type. Run competitor keyword gap analysis. Analyze
SERP composition: what formats are ranking (listicles, how-tos, definitions), what intent is
dominant. Add GEO-layer question research: collect actual prompts users send to AI search
engines, sourced from sales calls, Reddit, PAA boxes, and social listening.

Phase C — On-Page Optimization
Element

Standard

Rationale

Title tag

≤60 chars; keyword upfront

Truncation prevention; relevance signal

H1

Exactly one per page; contains primary keyword

Semantic structure

H2–H6

Logical hierarchy; secondary keywords distributed

Crawl + readability

Meta description

50–160 chars; includes keyword + value prop

CTR signal

URL

Clean, hyphenated, keyword-containing

Crawl + user trust

Element

Standard

Rationale

Image alt text

Keyword-relevant, descriptive

Accessibility + indexing

Internal links

No orphan pages; contextual anchor text

PageRank distribution

E-E-A-T signals

Author bios, credentials, sourcing transparency

Google + AI source selection

Schema

FAQ, HowTo, Article, Author, BreadcrumbList

Rich results + AI extraction

Phase D — Technical SEO
Fix crawl errors; verify sitemap completeness; confirm robots.txt correctness. Enforce HTTPS
across all pages. Maintain site architecture at <4 clicks from homepage. Resolve duplicate
content via canonical tags. Monitor Core Web Vitals continuously: LCP target <2.5s, CLS <0.1,
INP <200ms.

Phase E — Off-Page / Authority
Analyze backlink profile; disavow toxic links. Execute link acquisition: guest posting, digital
PR, niche edits. Monitor brand mentions. Manage reviews and ratings (review schema). The
Profound framework sets a benchmark of earning citations from ≥20 high-authority domains
per quarter to build the citation authority that AI engines use for source selection (Profound,
2025).

Phase F — Monitor & Iterate
Track: organic traffic (GA4), keyword rankings (GSC + SERP API), backlink growth, conversion
rates, AI search visibility. Run content gap analysis quarterly. Prioritize content updates based
on performance data. A/B test headlines, formats, and CTAs. Update content every 90–180 days
for SEO; every 90 days for GEO (AI citations decay on a shorter cycle).

5. Methodology Deep Dive — GEO Loop
Three academic papers define the GEO optimization methodology. They are complementary,
covering different aspects of the problem.

5.1 Core GEO Framework (Aggarwal et al., 2023)
The founding paper (arXiv:2311.09735, presented at ACM KDD 2024, DOI: 10.1145/3637528.3671900)
establishes GEO as a black-box optimization problem — the agent cannot access the internals of
the generative engine, so it must measure, modify, and re-measure.
Core GEO loop:
1. Measure: Compute visibility in generative engine responses using two metrics:
Attributed Word Count (word): How many words from the source document appear
verbatim or near-verbatim in the engine's response

Position-Weighted Citation Order (pos): A weighted score giving higher credit to citations
that appear earlier in the engine's answer
2. Apply: Choose and apply one or more content modification strategies from a defined set
(adding statistics, adding citations, adding quotations, restructuring for scanability, etc.)
3. Re-measure: Run the same query set through the generative engine and recompute both
metrics
4. Iterate: Continue modifying until visibility scores converge or plateau
Key empirical result: Content modifications produce up to +40% visibility boost across
domains, but the optimal strategies vary by domain — there is no universally dominant single
strategy. This necessitates a content-adaptive selection mechanism (see Section 7).
GEO-bench: A benchmark of diverse queries + web source documents used for systematic
evaluation across the research community.

5.2 E-GEO: Tractable Optimization for E-Commerce (Bagga et al., 2025)
(arXiv:2511.20867, GitHub: psbagga17/E-GEO)
E-GEO makes two contributions relevant to the agent implementation:
1. Formulates GEO as a tractable optimization problem: Rather than random strategy search,
E-GEO defines a lightweight iterative prompt-optimization algorithm that is
computationally efficient enough for production use.
2. Universal domain-agnostic GEO pattern: By evaluating 15 common rewriting heuristics
across e-commerce data, E-GEO identifies a baseline set of modifications that improve
visibility regardless of specific domain — this becomes the agent's default starting strategy
before domain-specific strategies are applied.
The 15 heuristics tested include: adding statistics, adding authoritative citations, adding
quotations from experts, restructuring with bullet points, adding TL;DR summaries, adding
FAQ sections, increasing entity density, and reformatting for machine scannability.

5.3 AI Search Bias Empirics (Chen et al., 2025)
(arXiv:2509.08919)
The key empirical finding: AI search engines have overwhelming bias toward earned media
(citations from authoritative third-party sources) over brand-owned content — unlike Google,
which ranks a mix of authoritative domains and well-optimized brand pages. This has direct
strategic implications:
Strategic Pillar

Implementation

Machine scannability and justification
engineering

Answer-first structure; TL;DR blocks; tables; inline citations per claim

Dominating earned media

Guest posts on authoritative domains; digital PR; thought leadership publications;
original research

Strategic Pillar

Implementation

Engine-specific language-aware
strategies

Different AI engines respond differently to phrasing; test prompt variants per engine

Overcoming big brand bias

Small brands must over-invest in citation authority and entity coverage to compete
against established names

Cross-platform instability finding: Visibility performance on one AI engine does not reliably
predict performance on another due to differences in retrieval architecture, freshness
indexing, and domain diversity preferences. The agent must monitor all 8 platforms
independently.

5.4 Integrated SEO + GEO + AEO Framework (Manasa et al., 2025)
(DOI: 10.55041/ijsrem53069)
The convergence factors that serve all three optimization channels simultaneously:
Structured data: Schema markup aids Google's structured results, AI engines' extraction,
and voice assistants' direct answers
Entity recognition: Named entity density and accuracy improves all three channels
Authoritative sourcing: Third-party citations improve Domain Authority (SEO), citation
probability (GEO), and answer confidence (AEO)
Intent-driven content design: Content that directly answers the user's real question (not a
keyword-stuffed approximation) performs better across all three channels
The agent should always prioritize modifications that serve all three channels (schema,
entities, authoritative citations) before making channel-specific trade-offs.

6. Human vs. AI Agent Comparison Table
Task

Human SEO Professional

AI Agent (This Implementation)

Technical audit (20 URLs)

2–4 hours

5–15 minutes (async parallel)

Keyword research

2–3 hours

3–5 minutes (SERP API + LLM clustering)

Competitor gap analysis

1–2 hours

5 minutes (automated diff against top 20)

Content brief creation

1–2 hours

2–3 minutes (Phase 5)

Full article draft

4–6 hours

15–30 minutes (Phase 6 with multi-turn rewriting)

On-page optimization

1–2 hours

5 minutes (Phase 7, scored and auto-corrected)

Schema markup
generation

30–60 min

Instant (template-driven + LLM)

GEO visibility
measurement

Manual/impossible at scale

Automated across 8 platforms (Phase 9)

AI citation monitoring

Not feasible manually

Continuous (daily scheduled)

Task

Human SEO Professional

AI Agent (This Implementation)

Root cause diagnosis

1–3 hours (guesswork + analysis)

10–20 minutes (systematic hypothesis evaluation,
Phase 10)

Fix implementation

2–4 hours

Auto-apply (technical) or queue (editorial)

Memory/history retention

Notes, spreadsheets — degrades over
time

Persistent Obsidian vault — improves over time

Strategy learning

Informal (practitioner intuition)

MAP-Elites archive (explicit, quantified, reusable)

Per-article total

9–14 hours

30–60 minutes

Consistency

Variable (practitioner state, fatigue)

Uniform — agent never skips steps

Platform coverage

1–2 platforms typically

8 AI platforms + Google Search Console

Content update cadence

Often delayed; missed until rankings
drop

Scheduled; Phase 10 triggers at threshold

Source: Time estimates from Frase 6-stage pipeline analysis; strategy and memory advantages from
AgenticGEO Yuan et al., 2026 and Yuksel & Sawaf, 2024.

7. AgenticGEO Inner Loop
AgenticGEO (Yuan et al., 2026, Semantic Scholar ID: 286762329, GitHub: AIcling/agentic_geo) is
the most advanced academic treatment of automated GEO. It addresses the core problem with
naive GEO: a single globally-optimal rewriting strategy does not exist. The optimal strategy
depends on the specific content, domain, and generative engine. AgenticGEO solves this with a
three-phase architecture.

Phase 1: Offline Critic Alignment (Warm-Start)
Goal: Initialize the critic model using offline preference data before any expensive generative
engine calls.
Mechanism:
Collect offline pairs: (content_before_rewrite, content_after_rewrite,
visibility_delta)

Fine-tune a lightweight Co-Evolving Critic model (Qwen2.5-1.5B with LoRA adapters) to
predict whether a candidate rewrite will increase visibility
This warm-start gives the critic a prior that reduces the number of real generative engine
queries needed in Phase 2
Implementation note for Hermes: This phase is run once per domain type when first deploying
the agent. The resulting LoRA weights and value head are stored in Obsidian (or a local model
directory referenced by Obsidian). On subsequent runs, skip directly to Phase 2 or 3.

Phase 2: Online Strategy–Critic Co-Evolution
Goal: Maintain and continuously improve a quality-diversity archive of rewriting strategies,
calibrated against real generative engine feedback.
Mechanism — MAP-Elites Archive:
MAP-Elites is a quality-diversity evolutionary algorithm that maintains a grid of "elite"
solutions — the best-performing example of each distinct behavior type
In this context: a strategy archive where each cell contains the best-observed rewriting
strategy for a particular (content-type, domain) combination
The agent stores strategies as JSON in Obsidian: {"strategy_id": "...", "description":
"add_statistics", "applicable_domains": ["health", "finance"], "avg_score_delta":
+0.18, "n_applications": 47}

Mechanism — Co-Evolution:
The Evolver model (Qwen2.5-7B-Instruct) proposes new strategy candidates by mutating or
combining existing archive entries
New strategies are evaluated against real generative engine responses (limited GE calls to
control cost)
The Critic is continuously recalibrated: if a strategy produces an unexpected score, the
critic updates its weights to reduce that prediction error
This co-evolution means both the strategy archive and the critic improve over time with
each loop run
Low-feedback regime: AgenticGEO maintains strong performance even with very few real
generative engine queries per round — critical for cost efficiency.

Phase 3: Inference-Time Multi-Turn Rewriting
Goal: For a given piece of content, select and execute the best sequence of rewriting strategies,
guided by the critic.
Mechanism:
1. Load the MAP-Elites archive from Obsidian
2. The Critic scores each archived strategy against the current content state, selecting the top25 candidates
3. The Rewriter (Qwen2.5-32B-Instruct; or Hermes in this implementation) executes the
highest-scoring strategy, producing a rewritten draft
4. The Critic re-scores the result; the agent decides whether to apply another rewrite step or
stop
5. Up to 3 rewrite steps are executed per content piece
6. Final draft is the output of the last accepted rewrite step
Evaluation metrics (per Aggarwal et al., 2023):

word: Attributed Word Count
pos: Position-Weighted Citation Order
overall: Composite of word + pos

Reported results: AgenticGEO outperforms 14 baselines across 3 datasets (GEO-bench, MSdata,
E-commerce data). The co-evolving architecture is especially advantageous in the low-feedback
regime — when generative engine calls must be rationed due to cost or rate limits.

Mapping AgenticGEO to Hermes + Obsidian
AgenticGEO Component

Hermes Implementation

Obsidian Role

Co-Evolving Critic (Qwen2.51.5B)

Lightweight scoring LLM call with rubric

Stores critic preference pairs

MAP-Elites Strategy Archive

JSON file of strategies with scores

Obsidian vault node:
strategies.json

Evolver (Qwen2.5-7B)

Hermes reasoning for strategy mutation

Proposes new strategy variants

Rewriter (Qwen2.5-32B)

Hermes rewriting with strategy instructions

Draft versions stored in Obsidian

GE feedback (real queries)

Phase 9 monitoring results (actual platform
scores)

Monitoring logs feed back to archive

Offline preference pairs

Historical audit results

Obsidian audit log → training data

8. Token-Saving Notes for Hermes
Each phase of the loop has token cost implications. The following design patterns reduce cost
while maintaining output quality.

8.1 Tiered Routing
Route LLM calls by task complexity (dannwaneri/seo-agent):
Tier

Task

Model

Approximate cost

0

Deterministic checks (title length, H1 count, link parser)

Pure Python

$0

1

Simple extraction (meta description check, broken link summary)

Small model / Haiku

~$0.0001/URL

2

Structured extraction (schema detection, entity listing)

Mid-tier model

~$0.001/URL

3

Full draft writing, multi-turn rewriting, root cause diagnosis

Large model / Hermes

~$0.01–0.05/task

Run Tier 0 first; escalate to Tier 1 only if Tier 0 finds an issue; escalate to Tier 3 only for content
creation and strategy tasks.

8.2 Structured JSON Outputs
Instruct Hermes to output structured JSON for all audit and analysis tasks, not prose. Example:
{

}

"title_audit": {"value": "...", "length": 47, "status": "PASS"},
"h1_audit": {"count": 1, "value": "...", "status": "PASS"},
"entity_gaps": ["entity_A", "entity_B"],
"recommended_actions": ["action_1", "action_2"]

Structured output eliminates re-parsing prose, reduces hallucination risk, and compresses
token usage by 30–50% versus free-form analysis.

8.3 Incremental State Writes
Write state to report.json / Obsidian after each URL or each phase, not at the end of the full
loop run. This means:
The agent is safe to interrupt at any point
On resume, already-completed steps are skipped automatically (--auto mode)
No work is lost to API timeouts or system crashes

8.4 Caching
Cache all SERP API results, GEO visibility scan results, and fetched page content with a
timestamp. Before any external API call, check cache for a result from the same query within
the freshness window (default: 24 hours for rankings, 7 days for page content). This eliminates
redundant API calls when Phase 10 triggers a re-run shortly after Phase 9.

8.5 Context Window Management
The Hermes context for each phase should include only the relevant Obsidian notes, not the
full vault. Load only:
The current target page's audit note
The strategy archive summary (top-25 strategies by score — not full history)
The current phase's input data (keyword map, content brief, etc.)
Do not load: unrelated page notes, historical monitoring logs beyond the last 3 snapshots, full
draft history.

8.6 Prompt Templates
Pre-build per-phase prompt templates stored in Obsidian. Each template has:
A fixed system prompt (role, rules, output format schema)
Variable injection points for phase-specific data ({target_keyword}, {content_brief}, etc.)

This prevents Hermes from re-reasoning about output format each run, saves tokens on
instruction overhead, and ensures consistent structured output.

8.7 Batch Processing for GEO Visibility Scans
Phase 9 sends 20–30 prompts per topic to 8 platforms = up to 240 API calls per content piece.
Batch these:
Group prompts by platform and send as a batch where the API supports it
Run all 8 platforms in parallel (async), not sequentially
Cache results with a 24-hour TTL to avoid duplicate scans within the same day

8.8 MAP-Elites Compression for Obsidian
Store the strategy archive as a compact JSON summary, not full strategy text:
{

}

"strategy_id": "s_042",
"description": "add_inline_statistics_with_citations",
"top_domains": ["health", "finance", "tech"],
"avg_word_delta": 0.22,
"avg_pos_delta": 0.15,
"n_applications": 89,
"last_updated": "2026-05-15"

Full strategy text (the actual rewriting instructions) is generated at inference time by Hermes
based on the strategy description, not stored verbatim. This keeps the archive small and
Obsidian-searchable.

8.9 Phase 10 Early Exit
Diagnosis in Phase 10 should follow a decision tree with early exits: if a simple freshness issue
is detected (content >90 days old, statistics dated), apply the fix immediately without running
all 6 diagnostic hypotheses. Only run the full diagnosis tree when the cause is ambiguous.

8.10 Human-in-the-Loop Checkpoints
Flag to needs_human[] and pause (don't terminate) on:
Login walls or paywalled target pages
Fix plans that would delete or substantially restructure cornerstone content
Schema changes that affect structured data used across >10 pages
Earned media outreach tasks (cannot be fully automated)
This prevents expensive runaway loops on tasks the agent cannot complete correctly without
human context.

9. Sources
Academic Papers
Citation

DOI / URL

Aggarwal, P. et al. (2023). GEO: Generative Engine Optimization. Presented at
ACM KDD 2024. arXiv:2311.09735.

DOI: 10.1145/3637528.3671900 · arXiv:
2311.09735

Yuan, Z. et al. (2026). AgenticGEO: A Self-Evolving Agentic System for
Generative Engine Optimization. Semantic Scholar ID: 286762329.

GitHub: AIcling/agentic_geo

Bagga, P. et al. (2025). E-GEO: A Testbed for Generative Engine Optimization
in E-Commerce. arXiv:2511.20867.

arXiv: 2511.20867 · GitHub:
psbagga17/E-GEO

Chen, R. et al. (2025). Generative Engine Optimization: How to Dominate AI
Search. arXiv:2509.08919.

arXiv: 2509.08919

Manasa, S. et al. (2025). Integrated Framework: SEO + GEO + AEO. IJSREM.

DOI: 10.55041/ijsrem53069

Yuksel, A. & Sawaf, A. (2024). Multi-AI Agent Autonomous Optimization via
Iterative Refinement. arXiv:2412.17149.

arXiv: 2412.17149

Padma, S. & VS, S. (2023). Insights into SEO using NLP and ML. IJACSA.

DOI: 10.14569/ijacsa.2023.0140211

Production Systems & Implementations
Source

URL

Frase — AI Agents for SEO: Complete Guide to Agentic
Content Automation (2026)

frase.io/blog/ai-agents-for-seo

Profound — 10-Step Framework for Generative Engine
Optimization (2025)

tryprofound.com/resources/articles/generative-engine-optimizati
on-geo-guide-2025

Nwaneri, D. — dannwaneri/seo-agent (GitHub, open source)

github.com/dannwaneri/seo-agent

Agent Loop Architecture References
Source

URL

Oracle — What Is the AI Agent Loop? The Core Architecture
Behind Autonomous AI Systems (2026)

blogs.oracle.com/developers/what-is-the-ai-agent-loop-the-core
-architecture-behind-autonomous-ai-systems

Hugging Face Agents Course — Agent Steps and Structure
(Think-Act-Observe)

huggingface.co/learn/agents-course/en/unit1/agent-steps-and-s
tructure

Oracle Select AI Agent — ReAct Pattern Documentation (2026)

docs.oracle.com/en/database/oracle/oracle-database/26/selai/s
elect-ai-agent2.html

End of document. All claims in this document are grounded in the sourced data listed in Section 9. No
claims have been fabricated beyond the provided research.
