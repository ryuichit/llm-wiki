---
title: WebDancer
tags: [technology, agent, information-seeking, reinforcement-learning]
created: 2026-06-24
updated: 2026-06-24
---

# WebDancer

The first end-to-end trained autonomous information-seeking agent in the Tongyi DeepResearch series, developed by [[tongyi-lab|Tongyi Lab]] at [[alibaba-group|Alibaba Group]]. WebDancer combines novel synthetic data generation with two-stage training (SFT + RL) to achieve strong performance on complex research tasks.

## Overview

WebDancer demonstrates that autonomous research agents can be built from scratch using synthetic training data and reinforcement learning, without human annotation. It achieves 51.5% on GAIA and 47.9% on WebWalkerQA using the QwQ-32B base model, establishing a new paradigm for training information-seeking agents.

## Architecture

### ReAct Framework

WebDancer follows the [[react-framework|ReAct (Reasoning + Acting)]] paradigm:

**Action Space**:
- **search(query)**: Search the web for information
- **visit(url)**: Navigate to and read a webpage
- **answer(text)**: Provide final answer and terminate

**Execution Pattern**:
```
Thought: [Analyze current state, plan next action]
Action: [Execute search/visit/answer]
Observation: [Environment feedback]
... (iterate until answer provided)
```

### Agent Behavior

**Multi-Step Reasoning**:
- Decomposes complex questions into sub-goals
- Iteratively gathers information
- Synthesizes findings into coherent answers
- Adapts strategy based on observations

**Information Seeking**:
- Formulates effective search queries
- Selects relevant URLs to visit
- Extracts key information from pages
- Connects information across sources

## Training Pipeline

### Stage 1: Data Synthesis

**[[crawlqa|CRAWLQA]] (60K samples)**:
- Web crawling-based QA generation
- Sources: arxiv, github, wikipedia, stackoverflow
- Multi-hop reasoning requirements
- Diverse domain coverage

**[[e2hqa|E2HQA]] (40K samples)**:
- Easy-to-hard QA progression
- SimpleQA-style iterative refinement
- Controlled complexity increase
- Systematic question type coverage

**Trajectory Sampling**:
- Short-CoT: GPT-4o generates 7,678 trajectories (4.56 actions, 510 tokens avg)
- Long-CoT: QwQ-Plus generates 6,550 trajectories (2.31 actions, 1599 tokens avg)
- Multi-stage filtering: validity, correctness, quality
- Result: 14,228 high-quality training trajectories

### Stage 2: Supervised Fine-Tuning

**Cold-Start Training**:
- Initialize from base instruction model
- Train on filtered trajectories
- Learn ReAct format and basic behaviors
- Essential for agent bootstrapping (direct RL only achieves 5%)

**What is Learned**:
- ReAct format compliance
- Basic search and visit strategies
- Information extraction patterns
- Answer synthesis approaches

### Stage 3: On-Policy RL (DAPO)

**[[dapo|DAPO Algorithm]]**:
- Decoupled Clip and Dynamic Sampling Policy Optimization
- On-policy trajectory collection
- LLM-as-Judge reward evaluation
- Separate actor and critic updates

**Benefits**:
- Longer reasoning (increased thought length + actions)
- Better generalization beyond training distribution
- Adaptive exploration strategies
- Improved information-seeking behavior

## Performance

### GAIA Benchmark

**Overall**: 51.5% average (QwQ-32B base)

**By Difficulty**:
- Level 1 (Easy): 61.5%
- Level 2 (Medium): 50.0%
- Level 3 (Hard): 25.0%

**Pass@3**: 64.1% (significant improvement from single attempt)

### WebWalkerQA Benchmark

**Overall**: 47.9% average (QwQ-32B base)

**By Difficulty**:
- Easy: 52.5%
- Medium: 59.6%
- Hard: 35.4%

**Pass@3**: 62.0%

### BrowseComp Benchmark

**Pass@1**: 3.8%
**Pass@3**: 7.9%

**Analysis**: Very challenging real-world web tasks, demonstrates room for improvement

## Key Innovations

### End-to-End Training

**First of Its Kind**:
- Complete training pipeline from scratch
- No human trajectory annotation
- Synthetic data generation at scale
- Two-stage training paradigm

**Advantages**:
- Scalable to large datasets
- Rapid iteration on training methods
- Cost-effective development
- Reproducible research

### Short-CoT vs Long-CoT

**Key Finding**: Short-CoT (GPT-4o) trajectories transfer better to instruction models than Long-CoT (QwQ-Plus)

**Analysis**:
- Short-CoT: 4.56 actions, 510 tokens - more learnable
- Long-CoT: 2.31 actions, 1599 tokens - knowledge transfer difficulty
- Reasoning models don't benefit as much from RL in agentic tasks
- Instruction models better suited for agent training

### SFT + RL Combination

**Critical Insight**: Both stages essential

**SFT Role**:
- Provides reasonable initialization
- Establishes basic agent behavior
- Enables RL exploration from good starting point
- Without SFT: RL only achieves ~5%

**RL Role**:
- Improves generalization
- Enables longer, better reasoning
- Adapts to out-of-distribution questions
- Discovers improved strategies

## Capabilities

### Multi-Step Information Gathering

**Complex Queries**:
- Requires connecting multiple sources
- Iterative refinement of understanding
- Disambiguation of terms
- Cross-reference validation

**Example Flow**:
1. Thought: "Need to find author affiliation for paper X"
2. Action: search("paper X authors affiliation")
3. Observation: [Search results with conference page]
4. Thought: "Conference page likely has author info"
5. Action: visit(conference_url)
6. Observation: [Page content with author list]
7. Thought: "Found affiliation: University Y"
8. Action: answer("University Y")

### Adaptive Exploration

**Strategy Selection**:
- Formulates appropriate search queries
- Chooses relevant URLs from results
- Decides when sufficient information gathered
- Balances breadth vs depth of search

**Learned Through RL**:
- Exploration-exploitation trade-offs
- Query reformulation strategies
- URL relevance assessment
- Information sufficiency judgment

## Limitations

### Current Constraints

**Performance**:
- Hard questions still challenging (25-35%)
- BrowseComp very low (3.8% Pass@1)
- Room for significant improvement
- Generalization limitations

**Technical**:
- Limited action space (only search, visit, answer)
- Sparse reward signals (end-of-trajectory only)
- Long-CoT knowledge transfer difficulty
- Web dynamics and stale information

**Scope**:
- Text-only (no multimodal understanding)
- Limited to web search + page visit
- No long-term memory across sessions
- Single-task focus

### Known Issues

**Reasoning Models**:
- Benefit less from RL in agentic tasks
- Long-CoT trajectories don't transfer well
- Sparse rewards insufficient for optimization
- Different from instruction model dynamics

**Web Environment**:
- Dynamic and non-stationary
- Content changes over time
- Search result variability
- Page availability issues

## Comparison with Related Systems

### WebDancer vs AutoData

**Different Focus**:
- **WebDancer**: Research questions → answers with reasoning
  - Single agent trained end-to-end
  - Open-ended information seeking
  - GAIA, WebWalkerQA benchmarks
  
- **[[autodata|AutoData]]**: Instructions → structured data
  - Multi-agent coordination system
  - Structured data collection
  - Instruct2DS benchmark

**Complementary Roles**:
- WebDancer: Research assistance
- AutoData: Data collection automation
- Both: Information gathering from web

### WebDancer vs WebCloak

**Opposing Perspectives**:
- **WebDancer**: Information seeker (attacker role)
  - Autonomous research agent
  - Navigate web for answers
  - Legitimate research assistance
  
- **[[webcloak|WebCloak]]**: Content protector (defender role)
  - Defense against scraping
  - Structural + semantic obfuscation
  - Prevent unauthorized extraction

**Interaction**:
- WebCloak would likely block WebDancer's scraping attempts
- Represents attacker-defender dynamic
- Both assume LLM capabilities, opposite goals

### WebDancer vs Search-o1/R1-Searcher

**Training Paradigm**:
- **WebDancer**: End-to-end trained with SFT + RL
  - Synthetic data generation
  - Two-stage training
  - DAPO algorithm
  
- **Search-o1/R1-Searcher**: Reasoning model + search integration
  - Prompting-based approaches
  - Less emphasis on training
  - Reasoning model orchestration

**Innovation**:
- WebDancer: Complete training framework
- Others: Architecture and prompting focus

## Use Cases

### Academic Research

**Literature Review**:
- Find relevant papers on topic
- Extract key findings
- Compare methodologies
- Synthesize research landscape

**Fact Checking**:
- Verify claims across sources
- Find original references
- Assess consensus
- Identify conflicting information

### Technical Investigation

**Documentation Search**:
- Find API documentation
- Understand implementation details
- Locate code examples
- Troubleshoot issues

**Technology Comparison**:
- Compare features across tools
- Assess pros and cons
- Find user experiences
- Make informed decisions

### General Information Seeking

**Complex Questions**:
- Multi-step reasoning required
- Multiple sources needed
- Disambiguation necessary
- Synthesis from diverse information

**Current Events**:
- Gather recent information
- Cross-reference news sources
- Understand developing situations
- Track updates over time

## Future Improvements

### Technical Enhancements

**Extended Action Space**:
- summarize(text): Condense long content
- compare(item1, item2): Explicit comparison
- verify(claim, source): Fact checking
- bookmark(url): Save for later reference

**Dense Rewards**:
- Per-step feedback
- Intermediate correctness signals
- Reasoning quality metrics
- Action efficiency rewards

**Multimodal**:
- Image understanding
- Table parsing
- Chart interpretation
- Video content analysis

### Training Improvements

**Better Data Synthesis**:
- More diverse question sources
- Domain-specific augmentation
- Adversarial question generation
- Multi-hop reasoning emphasis

**Advanced RL**:
- Off-policy for sample efficiency
- Hierarchical RL for long-horizon
- Multi-task learning for transfer
- Curiosity-driven exploration

**Long-CoT Transfer**:
- Better integration of reasoning model knowledge
- Distillation approaches
- Hybrid training methods
- Alternative reward structures

### System Enhancements

**Memory**:
- Long-term information retention
- Cross-session learning
- User preference adaptation
- Incremental knowledge building

**Robustness**:
- Handle malformed pages
- Deal with incomplete information
- Recover from failed actions
- Detect unreliable sources

**Efficiency**:
- Faster trajectory generation
- Lower token consumption
- Parallel search strategies
- Caching mechanisms

## Implementation

### Base Model

**QwQ-32B**:
- Instruction-following model
- 32 billion parameters
- Developed by Tongyi Lab
- Optimized for agentic tasks

**Training Infrastructure**:
- Large-scale computing resources
- Distributed training
- On-policy trajectory collection
- LLM-as-Judge evaluation

### Deployment

**Inference**:
- Iterative action generation
- Web search API integration
- Page fetching and parsing
- Real-time execution

**Evaluation**:
- GAIA, WebWalkerQA, BrowseComp benchmarks
- Pass@1 and Pass@3 metrics
- Error analysis
- Continuous improvement

## Open Source

**Code Release**:
- Repository: github.com/Alibaba-NLP/DeepResearch
- Training pipeline
- Data synthesis scripts
- Evaluation tools
- Documentation

**Community**:
- Reproducible research
- Academic collaboration
- Industry adoption
- Continuous development

## Related Pages

### Entities

- [[tongyi-lab|Tongyi Lab]]
- [[alibaba-group|Alibaba Group]]
- [[dapo|DAPO Algorithm]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]
- [[autodata|AutoData]]
- [[webcloak|WebCloak]]

### Concepts

- [[information-seeking-agents|Information-Seeking Agents]]
- [[react-framework|ReAct Framework]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[short-cot|Short Chain-of-Thought]]
- [[long-cot|Long Chain-of-Thought]]
- [[synthetic-agent-training-data|Synthetic Agent Training Data]]

### Comparisons

- [[webdancer-vs-autodata|WebDancer vs AutoData]]
- [[webdancer-vs-webcloak|WebDancer vs WebCloak]]

### Sources

- [[webdancer-paper|WebDancer Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- github.com/Alibaba-NLP/DeepResearch
