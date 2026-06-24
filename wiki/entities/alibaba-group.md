---
title: Alibaba Group
tags: [entity, organization]
created: 2026-06-24
updated: 2026-06-24
---

# Alibaba Group

Major Chinese technology conglomerate and parent company of [[tongyi-lab|Tongyi Lab]], a leading AI research organization focused on large language models, autonomous agents, and advanced natural language processing.

## Overview

**Type**: Technology conglomerate

**Headquarters**: Hangzhou, China

**Industry**: E-commerce, cloud computing, digital media, artificial intelligence

**Research Division**: [[tongyi-lab|Tongyi Lab]]

## AI Research Contributions

### Tongyi Lab

Alibaba's primary AI research laboratory, [[tongyi-lab|Tongyi Lab]], focuses on:
- Large language models and reasoning systems
- [[information-seeking-agents|Autonomous information-seeking agents]]
- [[agentic-rl|Reinforcement learning for agentic tasks]]
- Synthetic data generation for agent training
- Multi-modal AI systems

### Key Research Projects

#### Tongyi DeepResearch Series

**[[webdancer|WebDancer]]**:
- First end-to-end trained autonomous information-seeking agent
- Published at NeurIPS 2025
- Achieved 51.5% on GAIA benchmark, 47.9% on WebWalkerQA
- Novel two-stage training: SFT + on-policy RL
- Open-sourced at github.com/Alibaba-NLP/DeepResearch

**QwQ Models**:
- QwQ-Plus: Long chain-of-thought reasoning model
- QwQ-32B: Instruction model for agent execution
- Focus on deep reasoning and multi-step problem solving

### Novel Algorithms and Methods

**[[dapo|DAPO Algorithm]]**:
- Decoupled Clip and Dynamic Sampling Policy Optimization
- Novel on-policy RL approach for agentic tasks
- Addresses sparse rewards and long-horizon credit assignment
- Separate actor and critic optimization for stability

**Data Synthesis Methods**:
- [[crawlqa|CRAWLQA]]: Web crawling-based QA generation (60K samples)
- [[e2hqa|E2HQA]]: Easy-to-hard QA progression (40K samples)
- Zero human annotation training pipelines

## Research Team

### WebDancer Authors

**Co-First Authors**:
- [[jialong-wu|Jialong Wu]]
- [[baixuan-li|Baixuan Li]]
- [[runnan-fang|Runnan Fang]]
- [[wenbiao-yin|Wenbiao Yin]]

**Senior Researchers**:
- [[yong-jiang|Yong Jiang]]
- [[pengjun-xie|Pengjun Xie]]
- [[fei-huang|Fei Huang]]
- [[jingren-zhou|Jingren Zhou]]

**Contributing Researchers**:
- [[liwen-zhang|Liwen Zhang]]
- [[zhenglin-wang|Zhenglin Wang]]
- [[zhengwei-tao|Zhengwei Tao]]
- [[dingchu-zhang|Dingchu Zhang]]
- [[zekun-xi|Zekun Xi]]
- [[xiangru-tang|Xiangru Tang]]

## Core Research Areas

### Autonomous Agents

**Information-Seeking Systems**:
- Multi-step web navigation and search
- Information synthesis and citation generation
- Complex question answering
- Research automation

**Training Methodologies**:
- [[synthetic-agent-training-data|Synthetic agent training data]]
- [[two-stage-agent-training|Two-stage training paradigms]]
- [[on-policy-rl|On-policy reinforcement learning]]
- [[rejection-sampling-finetuning|Rejection sampling fine-tuning]]

### Natural Language Processing

**Language Models**:
- Instruction-following models
- Long-context reasoning systems
- Multi-modal understanding
- Tool use and API integration

**Training Insights**:
- SFT essential for agent cold-start
- RL enables longer reasoning and better generalization
- Short-CoT transfers better to instruction models
- Reasoning models benefit less from RL in agentic tasks

### Reinforcement Learning

**Agentic RL Challenges**:
- Sparse reward environments
- Long-horizon credit assignment
- Exploration-exploitation trade-offs
- Sample efficiency

**Solutions**:
- DAPO decoupled optimization
- Dynamic sampling strategies
- On-policy vs off-policy trade-offs
- LLM-as-Judge evaluation

## Industry Applications

### Research Assistance

**Capabilities**:
- Automated literature review
- Multi-source information aggregation
- Complex question answering
- Knowledge discovery and synthesis

### Enterprise Use Cases

**Business Intelligence**:
- Competitive analysis
- Market research automation
- Technical documentation search
- Customer support automation

**E-commerce Integration**:
- Product information retrieval
- Consumer behavior analysis
- Supply chain optimization
- Recommendation systems

## Publications

### NeurIPS 2025

**WebDancer: Towards Autonomous Information Seeking Agency**:
- First end-to-end agent training framework
- Novel data synthesis + two-stage training paradigm
- Systematic analysis of Short-CoT vs Long-CoT approaches
- Comprehensive agentic RL methodology
- Open-source code for reproducibility

### Research Impact

**Academic Contributions**:
- Novel training paradigm for autonomous agents
- First comprehensive agentic RL framework
- Systematic study of reasoning model transfer
- Public benchmarks and datasets

**Industry Impact**:
- Production-ready agent systems
- Scalable training methodologies
- Real-world deployment insights
- Open-source community contributions

## Infrastructure and Resources

### Computing Resources

**Large-Scale Infrastructure**:
- Distributed training systems
- High-performance computing clusters
- Production deployment platforms
- Real-time inference systems

### Data Resources

**Web-Scale Data**:
- E-commerce data
- User interaction patterns
- Web crawling infrastructure
- Multi-modal content sources

### Development Tools

**Open Source**:
- github.com/Alibaba-NLP/DeepResearch
- Training pipelines and scripts
- Evaluation benchmarks
- Documentation and tutorials

## Related Research Organizations

### Comparison with Other Labs

**[[university-of-notre-dame|University of Notre Dame]]**:
- AutoData: Multi-agent structured data collection
- Focus: Coordinated multi-agent systems

**[[nanyang-technological-university|Nanyang Technological University]]**:
- WebCloak: Web scraping defense mechanisms
- Focus: Content protection and security

**[[max-planck-institute-informatics|Max Planck Institute for Informatics]]**:
- robots.txt gatekeeping research
- Focus: Web transparency and data ethics

### Complementary Research

**Autonomous Agents**:
- AutoData: Multi-agent coordination
- WebDancer: Single-agent research assistance
- Different approaches to information retrieval

**Web Security**:
- WebCloak: Active defense mechanisms
- robots.txt: Voluntary compliance protocols
- Content protection strategies

## Open Source Contributions

### DeepResearch Repository

**Contents**:
- WebDancer implementation
- DAPO algorithm code
- Data synthesis pipelines
- Training and evaluation scripts
- Benchmarks and baselines

**Community Impact**:
- Academic reproducibility
- Industry adoption
- Research collaboration
- Educational resources

## Future Directions

### Research Roadmap

**Agent Capabilities**:
- Extended action spaces
- Multi-modal understanding
- Long-term memory systems
- Meta-learning approaches

**Training Methods**:
- Dense reward shaping
- Off-policy efficiency improvements
- Multi-task learning
- Transfer learning across domains

**Applications**:
- Domain-specific specialization
- User-facing products
- Enterprise tools
- Educational systems

## Related Pages

### Entities

**Organization**:
- [[tongyi-lab|Tongyi Lab]]

**Researchers**:
- [[jialong-wu|Jialong Wu]]
- [[baixuan-li|Baixuan Li]]
- [[runnan-fang|Runnan Fang]]
- [[wenbiao-yin|Wenbiao Yin]]
- [[yong-jiang|Yong Jiang]]
- [[pengjun-xie|Pengjun Xie]]
- [[fei-huang|Fei Huang]]
- [[jingren-zhou|Jingren Zhou]]

**Systems**:
- [[webdancer|WebDancer]]

### Concepts

- [[information-seeking-agents|Information-Seeking Agents]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[dapo|DAPO Algorithm]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[synthetic-agent-training-data|Synthetic Agent Training Data]]
- [[on-policy-rl|On-Policy Reinforcement Learning]]

### Sources

- [[webdancer-paper|WebDancer Paper]] (NeurIPS 2025)

## Sources

- [[webdancer-paper|WebDancer: Towards Autonomous Information Seeking Agency]] (NeurIPS 2025)
- github.com/Alibaba-NLP/DeepResearch
