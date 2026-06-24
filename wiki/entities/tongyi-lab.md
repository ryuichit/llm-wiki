---
title: Tongyi Lab
tags: [organization, research-lab, alibaba, ai-research]
created: 2026-06-24
updated: 2026-06-24
---

# Tongyi Lab

A research laboratory within [[alibaba-group|Alibaba Group]] focused on large language models, AI agents, and advanced natural language processing technologies.

## Overview

Tongyi Lab is the research organization behind the Tongyi DeepResearch series, including [[webdancer|WebDancer]], the first end-to-end trained autonomous information-seeking agent. The lab focuses on developing practical AI systems that combine cutting-edge research with real-world applications.

## Research Areas

### Autonomous Agents

- [[information-seeking-agents|Information-seeking agents]]
- [[agentic-rl|Agentic reinforcement learning]]
- Multi-step reasoning and planning
- Tool-augmented language models

### Training Methodologies

- [[synthetic-agent-training-data|Synthetic agent training data]]
- [[two-stage-agent-training|Two-stage training paradigms (SFT + RL)]]
- [[on-policy-rl|On-policy reinforcement learning]]
- [[rejection-sampling-finetuning|Rejection sampling fine-tuning]]

### Language Models

- Instruction-following models
- Reasoning models (QwQ series)
- Multi-modal understanding
- Tool use and API integration

## Key Projects

### Tongyi DeepResearch Series

**[[webdancer|WebDancer]]**:
- First end-to-end trained agent in the series
- Autonomous information-seeking capabilities
- 51.5% on GAIA, 47.9% on WebWalkerQA
- Published at NeurIPS 2025

**QwQ Models**:
- QwQ-Plus: Long chain-of-thought reasoning model
- QwQ-32B: Instruction model used in WebDancer
- Focus on deep reasoning capabilities
- Used for trajectory sampling and agent execution

## Research Contributions

### Novel Algorithms

**[[dapo|DAPO Algorithm]]**:
- Decoupled Clip and Dynamic Sampling Policy Optimization
- Novel on-policy RL for agentic tasks
- Separate actor and critic optimization
- Adaptive exploration-exploitation balance

### Data Synthesis Methods

**[[crawlqa|CRAWLQA]]**:
- Web crawling-based QA generation
- 60K training samples from diverse sources
- Multi-hop reasoning coverage
- arxiv, github, wikipedia, stackoverflow

**[[e2hqa|E2HQA]]**:
- Easy-to-hard QA progression
- 40K training samples
- Iterative complexity increase
- SimpleQA-style methodology

### Training Insights

- SFT essential for cold-start (direct RL only achieves 5%)
- RL enables longer reasoning and better generalization
- Short-CoT transfers better than Long-CoT to instruction models
- Web environment dynamics dominate temperature effects
- Reasoning models benefit less from RL in agentic tasks

## Publications

### NeurIPS 2025

**WebDancer: Towards Autonomous Information Seeking Agency**:
- Authors: [[jialong-wu|Jialong Wu]], [[baixuan-li|Baixuan Li]], [[runnan-fang|Runnan Fang]], [[wenbiao-yin|Wenbiao Yin]], and team
- First end-to-end agent training framework
- Novel data synthesis + two-stage training
- Code: github.com/Alibaba-NLP/DeepResearch

## Research Team

### Co-First Authors (WebDancer)

- [[jialong-wu|Jialong Wu]]
- [[baixuan-li|Baixuan Li]]
- [[runnan-fang|Runnan Fang]]
- [[wenbiao-yin|Wenbiao Yin]]

### Senior Researchers

- [[yong-jiang|Yong Jiang]]
- [[pengjun-xie|Pengjun Xie]]
- [[fei-huang|Fei Huang]]
- [[jingren-zhou|Jingren Zhou]]

### Contributing Researchers

- [[liwen-zhang|Liwen Zhang]]
- [[zhenglin-wang|Zhenglin Wang]]
- [[zhengwei-tao|Zhengwei Tao]]
- [[dingchu-zhang|Dingchu Zhang]]
- [[zekun-xi|Zekun Xi]]
- [[xiangru-tang|Xiangru Tang]]

## Technical Focus

### Information Seeking

**Autonomous Research**:
- Multi-step web navigation
- Search and visit actions
- Information synthesis
- Answer generation with citations

**Training Pipeline**:
- Synthetic data generation
- Supervised fine-tuning cold-start
- On-policy RL generalization
- LLM-as-Judge evaluation

### Reinforcement Learning

**Agentic RL**:
- Sparse reward challenges
- Long-horizon credit assignment
- Exploration-exploitation balance
- On-policy vs off-policy trade-offs

**DAPO Innovation**:
- Decoupled actor-critic updates
- Dynamic sampling strategies
- Stability improvements
- Sample efficiency

## Impact and Applications

### Academic Impact

- Novel training paradigm for agents
- Systematic analysis of Short-CoT vs Long-CoT
- First comprehensive agentic RL framework
- Open-source code for reproducibility

### Industry Applications

**Research Assistance**:
- Automated literature review
- Multi-source information aggregation
- Complex question answering
- Knowledge discovery

**Enterprise Use Cases**:
- Business intelligence gathering
- Competitive analysis
- Technical documentation search
- Customer support automation

## Relationship to Alibaba Group

**Integration**:
- Part of Alibaba's AI research portfolio
- Leverages Alibaba's infrastructure
- Contributes to Alibaba's AI products
- Collaboration with business units

**Resources**:
- Access to large-scale computing
- Web-scale data sources
- Production deployment capabilities
- Real-world testing environments

## Related Organizations

**Comparison with Other Labs**:
- [[university-of-notre-dame|University of Notre Dame]]: AutoData multi-agent system
- [[nanyang-technological-university|Nanyang Technological University]]: WebCloak defense
- Focus: Autonomous agents vs multi-agent coordination vs defense

**Complementary Research**:
- AutoData: Multi-agent structured data collection
- WebCloak: Web scraping defense
- WebDancer: Single-agent research assistance

## Open Source Contributions

**DeepResearch Repository**:
- Code: github.com/Alibaba-NLP/DeepResearch
- WebDancer implementation
- Training scripts and data pipelines
- Evaluation benchmarks
- Documentation and tutorials

**Community Engagement**:
- Open-source release for reproducibility
- Academic collaboration
- Industry partnerships
- Research community contributions

## Future Directions

### Research Roadmap

**Agent Capabilities**:
- Extended action spaces
- Multimodal understanding
- Long-term memory
- Meta-learning

**Training Methods**:
- Dense reward shaping
- Off-policy efficiency
- Multi-task learning
- Transfer learning

**Applications**:
- Domain specialization
- User-facing products
- Enterprise tools
- Educational systems

## Related Pages

### Entities

- [[alibaba-group|Alibaba Group]]
- [[webdancer|WebDancer]]
- [[jialong-wu|Jialong Wu]]
- [[baixuan-li|Baixuan Li]]
- [[runnan-fang|Runnan Fang]]
- [[wenbiao-yin|Wenbiao Yin]]

### Concepts

- [[information-seeking-agents|Information-Seeking Agents]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[dapo|DAPO Algorithm]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]
- [[two-stage-agent-training|Two-Stage Agent Training]]

### Sources

- [[webdancer-paper|WebDancer Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- github.com/Alibaba-NLP/DeepResearch
