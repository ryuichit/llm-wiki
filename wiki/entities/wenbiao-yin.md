---
title: Wenbiao Yin
tags: [entity, person, researcher, information-seeking, agentic-rl, autonomous-agents]
created: 2026-06-24
updated: 2026-06-24
---

# Wenbiao Yin

Co-first author of the [[webdancer-paper|WebDancer research paper]], contributing to the development of the first end-to-end trained autonomous information-seeking agent.

## Affiliation

[[tongyi-lab|Tongyi Lab]], [[alibaba-group|Alibaba Group]]

## Research Contributions

### WebDancer Project

As co-first author, contributed to:
- End-to-end agent training framework combining synthetic data generation with two-stage training
- Novel [[crawlqa|CRAWLQA]] and [[e2hqa|E2HQA]] data synthesis methods for generating 100K training samples
- [[dapo|DAPO algorithm]] development for on-policy reinforcement learning
- Two-stage training paradigm: supervised fine-tuning for cold-start + RL for generalization
- Comprehensive evaluation on [[gaia-benchmark|GAIA]], [[webwalkerqa-benchmark|WebWalkerQA]], and [[browsecomp-benchmark|BrowseComp]] benchmarks

## Key Findings

Demonstrated through WebDancer that:
- SFT cold-start is essential for agent training (direct RL only achieves ~5%)
- On-policy RL enables longer reasoning and better generalization beyond training distribution
- Short-CoT trajectories from instruction models (GPT-4o) transfer better than Long-CoT from reasoning models (QwQ-Plus)
- Synthetic data generation without human annotation can train competitive autonomous research agents
- Two-stage training achieves 51.5% on GAIA and 47.9% on WebWalkerQA

## Research Interests

- Autonomous information-seeking agents
- Agentic reinforcement learning
- Synthetic training data generation
- Multi-step reasoning and planning
- Tool-augmented language models
- On-policy RL for agent training

## Related Pages

### Paper and Project
- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- [[webdancer|WebDancer Agent]]

### Co-Authors
- [[jialong-wu|Jialong Wu]] (co-first author)
- [[baixuan-li|Baixuan Li]] (co-first author)
- [[runnan-fang|Runnan Fang]] (co-first author)

### Organization
- [[tongyi-lab|Tongyi Lab]]
- [[alibaba-group|Alibaba Group]]

### Technologies and Concepts
- [[dapo|DAPO Algorithm]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]
- [[information-seeking-agents|Information-Seeking Agents]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[react-framework|ReAct Framework]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- github.com/Alibaba-NLP/DeepResearch
