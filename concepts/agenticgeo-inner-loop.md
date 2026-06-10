---
title: AgenticGEO — Self-Evolving Agentic System for GEO
created: 2026-06-09
updated: 2026-06-09
type: concept
tags: [geo, strategy, tool, content-optimization, llm-retrieval]
sources: [raw/articles/seo-geo-agent-loop-methodology.md]
confidence: high
---

# AgenticGEO Inner Loop

AgenticGEO (Yuan et al., 2026) is the most advanced academic treatment of automated GEO. It addresses the core problem: a single globally-optimal rewriting strategy does not exist. The optimal strategy depends on specific content, domain, and generative engine.

## Three-Phase Architecture

### Phase 1: Offline Critic Alignment (Warm-Start)
Initialize the critic model using offline preference data before expensive generative engine calls.
- Collect offline pairs: (content_before_rewrite, content_after_rewrite, visibility_delta)
- Fine-tune a lightweight **Co-Evolving Critic** (Qwen2.5-1.5B with LoRA adapters) to predict whether a candidate rewrite will increase visibility
- Run once per domain type at deploy time; stored for reuse

### Phase 2: Online Strategy–Critic Co-Evolution
Maintain and continuously improve a quality-diversity archive of rewriting strategies.

**MAP-Elites Archive:** A quality-diversity evolutionary algorithm maintaining a grid of "elite" solutions — the best-observed strategy for each (content-type, domain) combination. Stored as JSON:
```json
{
  "strategy_id": "s_042",
  "description": "add_inline_statistics_with_citations",
  "top_domains": ["health", "finance", "tech"],
  "avg_word_delta": 0.22,
  "avg_pos_delta": 0.15,
  "n_applications": 89,
  "last_updated": "2026-05-15"
}
```

**Co-Evolution:** The Evolver proposes new candidates by mutating/combining archive entries. New strategies are evaluated against real generative engine responses. The Critic recalibrates when strategies produce unexpected scores. Both archive and critic improve over time with each loop run. Maintains strong performance even with few generative engine queries per round.

### Phase 3: Inference-Time Multi-Turn Rewriting
For a given piece of content, select and execute the best sequence of rewriting strategies.
1. Load MAP-Elites archive from wiki/Obsidian
2. Critic scores each archived strategy against current content, selects top-25
3. Rewriter executes highest-scoring strategy, producing rewritten draft
4. Critic re-scores; agent decides to apply another rewrite step or stop
5. Up to 3 rewrite steps per content piece
6. Final draft = output of last accepted step

AgenticGEO outperforms 14 baselines across 3 datasets (GEO-bench, MSdata, E-commerce data).

## Mapping to This Agent

| AgenticGEO Component | This Implementation | Wiki Role |
|---|---|---|
| Co-Evolving Critic (Qwen2.5-1.5B) | Lightweight scoring LLM call with rubric | Stores critic preference pairs |
| MAP-Elites Strategy Archive | JSON file of strategies with scores | `entities/strategy-archive.md` or similar |
| Evolver (Qwen2.5-7B) | Hermes reasoning for strategy mutation | Proposes new strategy variants |
| Rewriter (Qwen2.5-32B) | Hermes rewriting with strategy instructions | Draft versions in wiki |
| GE feedback (real queries) | Phase 9 monitoring results | Monitoring logs feed back to archive |
| Offline preference pairs | Historical audit results | Audit log → training data |

## Related
- [[geo-generative-engine-optimization]] — The broader GEO framework
- [[seo-geo-master-agent-loop]] — Where AgenticGEO feeds into Phase 5 & 6
- [[token-saving-strategies]] — MAP-Elites compression and cost optimization

## References
- Yuan et al. (2026). AgenticGEO: A Self-Evolving Agentic System for Generative Engine Optimization. GitHub: AIcling/agentic_geo.
