---
title: Information-Seeking Agents
tags: [concept, agents, information-retrieval, autonomous-systems]
created: 2026-06-24
updated: 2026-06-24
---

# Information-Seeking Agents

Autonomous systems that actively navigate information spaces (web, databases, documents) to gather, synthesize, and provide answers to complex questions requiring multi-step reasoning and multi-source integration.

## Overview

Information-seeking agents represent an evolution from passive search engines to active research assistants. Unlike traditional retrieval systems that return ranked results, these agents autonomously formulate queries, navigate sources, extract information, and synthesize coherent answers through iterative exploration.

## Core Characteristics

### Autonomous Behavior

**Active Exploration**:
- Formulate queries based on current understanding
- Select relevant sources to investigate
- Decide when sufficient information gathered
- Adapt strategy based on findings

**Multi-Step Reasoning**:
- Decompose complex questions into sub-goals
- Iteratively refine understanding
- Connect information across sources
- Synthesize final answers

**Adaptive Strategies**:
- Learn from successes and failures
- Adjust exploration approach
- Balance breadth vs depth
- Optimize for efficiency and accuracy

### Tool Use

**Search Actions**:
- Web search engines
- Database queries
- Document retrieval
- API calls

**Navigation Actions**:
- Visit URLs
- Read documents
- Follow links
- Extract content

**Synthesis Actions**:
- Summarize findings
- Compare sources
- Verify claims
- Generate answers

## Key Examples

### WebDancer

[[webdancer|WebDancer]] - First end-to-end trained information-seeking agent:

**Approach**:
- [[react-framework|ReAct framework]]: Thought → Action → Observation
- Actions: search, visit, answer
- Training: SFT + [[agentic-rl|RL with DAPO algorithm]]
- Data: [[crawlqa|CRAWLQA]] + [[e2hqa|E2HQA]] synthetic generation

**Performance**:
- GAIA: 51.5% (QwQ-32B)
- WebWalkerQA: 47.9%
- BrowseComp: 3.8% Pass@1

**Innovation**:
- End-to-end training from scratch
- Two-stage SFT + RL paradigm
- Synthetic data generation at scale
- Novel DAPO algorithm for agentic RL

### AutoData (Related but Different Focus)

[[autodata|AutoData]] - Multi-agent system for structured data collection:

**Approach**:
- 8 specialized agents (Research + Development squads)
- Natural language instruction → structured data
- Dual-mode: Web crawling + REST API
- [[ohcache|OHCache]] coordination

**Performance**:
- Instruct2DS: 92.91% F1 average
- Outperforms human programming
- 5.58 min per task, $0.57 cost

**Distinction**:
- AutoData: Structured data collection
- WebDancer: Research question answering
- Complementary capabilities in information domain

## Technical Approaches

### ReAct Framework

**Structure**:
```
while not done:
    Thought: [Analyze state, plan action]
    Action: [Execute tool use]
    Observation: [Environment feedback]
```

**Benefits**:
- Explicit reasoning transparency
- Tool-grounded execution
- Iterative refinement
- Multi-step capability

**Implementations**:
- [[webdancer|WebDancer]]: search, visit, answer actions
- [[autodata|AutoData]]: All agents follow ReAct pattern
- Many research systems and commercial products

### Training Paradigms

#### Prompting-Based

**Approach**:
- Pre-trained LLM + carefully designed prompts
- Few-shot examples
- Chain-of-thought prompting
- Tool use instructions

**Examples**:
- Search-o1
- R1-Searcher
- WebThinker (partially)

**Advantages**:
- No training required
- Rapid deployment
- Flexible modification

**Limitations**:
- Depends on base model capabilities
- Less adaptation to specific tasks
- Prompt engineering challenges

#### Trained Agents

**Approach**:
- Supervised fine-tuning on trajectories
- Reinforcement learning for optimization
- End-to-end training pipeline
- Synthetic or human-labeled data

**Examples**:
- [[webdancer|WebDancer]]: SFT + RL with synthetic data
- Various research agents with human demonstrations

**Advantages**:
- Task-specific optimization
- Better performance on target benchmarks
- Learned exploration strategies

**Limitations**:
- Training cost and complexity
- Data requirements
- Less flexible to new tasks

#### Multi-Agent Systems

**Approach**:
- Specialized agents for different roles
- Coordination mechanisms
- Collective intelligence
- Distributed problem-solving

**Examples**:
- [[autodata|AutoData]]: Research + Development squads
- Other multi-agent research systems

**Advantages**:
- Specialized expertise
- Parallel processing
- Robust to individual failures
- Scalable architecture

**Limitations**:
- Coordination overhead
- Communication costs
- Complexity management

## Training Methods

### Supervised Fine-Tuning

**Data Sources**:
- Human demonstrations
- Synthetic trajectory generation (WebDancer approach)
- Prompted trajectories from stronger models
- Filtered and curated datasets

**Process**:
1. Collect high-quality trajectories
2. Train agent to imitate behavior
3. Validate on held-out questions
4. Iterate on data and training

**Critical Role**:
- WebDancer insight: SFT essential for cold-start
- Without SFT, direct RL only achieves ~5%
- Provides reasonable initialization
- Enables RL exploration from good starting point

### Reinforcement Learning

**Reward Signals**:
- Answer correctness (primary)
- Trajectory format compliance
- Efficiency metrics (action count, time)
- Information quality

**Algorithms**:
- [[dapo|DAPO]]: WebDancer's on-policy algorithm
- PPO: Common baseline
- Off-policy methods: DQN, SAC variants
- Hierarchical RL: For long-horizon tasks

**Challenges**:
- Sparse rewards (typically end-of-trajectory)
- Long-horizon credit assignment
- Large action spaces (text generation)
- Exploration difficulty

**Benefits**:
- Generalization beyond training distribution
- Adaptive behavior
- Continuous improvement
- Emergent strategies

### Synthetic Data Generation

**WebDancer Approach**:

**[[crawlqa|CRAWLQA]]** (60K samples):
- Crawl web pages (arxiv, github, wiki, stackoverflow)
- Extract hierarchical content
- Generate multi-hop questions
- Create QA pairs

**[[e2hqa|E2HQA]]** (40K samples):
- Easy-to-hard progression
- Iterative refinement
- Controlled complexity
- Systematic coverage

**Trajectory Sampling**:
- Short-CoT (GPT-4o): 7,678 trajectories
- Long-CoT (QwQ-Plus): 6,550 trajectories
- Multi-stage filtering: validity, correctness, quality
- Result: 14,228 high-quality training samples

**Advantages**:
- Scalable data generation
- Zero human annotation cost
- Diverse question coverage
- Rapid iteration

## Evaluation Benchmarks

### GAIA (General AI Assistants)

**Focus**: Real-world assistant tasks

**Difficulty Levels**:
- Level 1: Straightforward questions
- Level 2: Moderate complexity
- Level 3: Challenging multi-step reasoning

**WebDancer Performance**:
- Overall: 51.5% (QwQ-32B)
- Level 1: 61.5%, Level 2: 50.0%, Level 3: 25.0%
- Pass@3: 64.1%

### WebWalkerQA

**Focus**: Web navigation and information extraction

**Difficulty Levels**:
- Easy: Simple factual questions
- Medium: Moderate complexity
- Hard: Complex multi-hop reasoning

**WebDancer Performance**:
- Overall: 47.9%
- Easy: 52.5%, Medium: 59.6%, Hard: 35.4%
- Pass@3: 62.0%

### BrowseComp

**Focus**: Complex browsing and comparison tasks

**Characteristics**:
- Real-world web complexity
- Multi-page navigation
- Comparative analysis
- Synthesis requirements

**WebDancer Performance**:
- Pass@1: 3.8%
- Pass@3: 7.9%
- Very challenging, room for improvement

### Instruct2DS (AutoData)

**Focus**: Structured data collection

**Domains**:
- Academic (93 pages)
- Finance (75 pages)
- Sports (69 pages)

**AutoData Performance**:
- Overall: 92.91% F1
- Different task type (data collection vs question answering)

## Key Challenges

### Sparse Rewards

**Problem**:
- Feedback only at trajectory end
- No intermediate guidance
- Long-horizon credit assignment
- Exploration difficulty

**Solutions**:
- Dense reward shaping (future work)
- Intermediate correctness checks
- Auxiliary tasks
- Curiosity-driven exploration

### Information Quality

**Problem**:
- Web contains misinformation
- Sources vary in reliability
- Content may be outdated
- Conflicting information

**Solutions**:
- Source credibility assessment
- Cross-reference verification
- Recency checking
- Confidence scoring

### Dynamic Environments

**Problem**:
- Web content changes
- Search results vary
- Pages become unavailable
- Non-stationary distributions

**WebDancer Finding**:
- Temperature has minimal impact
- Environment dynamics dominate
- Robustness more important than precision

**Solutions**:
- Adaptive strategies
- Error recovery
- Alternative source finding
- Graceful degradation

### Long-CoT Transfer

**WebDancer Finding**:
- Long-CoT from reasoning models doesn't transfer well
- Short-CoT from instruction models better for agent training
- Reasoning models benefit less from RL in agentic tasks

**Analysis**:
- Different model types have different optimal approaches
- Reasoning depth vs action efficiency trade-off
- Sparse rewards insufficient for reasoning model optimization

**Implications**:
- Use instruction models for agent training
- Short, focused reasoning more learnable
- Task-specific model selection important

## Comparison with Related Paradigms

### vs Traditional Search

**Traditional Search**:
- User formulates query
- System returns ranked results
- User evaluates relevance
- Iterative refinement by user

**Information-Seeking Agents**:
- Agent formulates queries autonomously
- Agent navigates and evaluates sources
- Agent synthesizes information
- Provides direct answer

**Advantage**: Automation of entire research process

### vs Question Answering

**Traditional QA**:
- Single knowledge source
- Direct answer extraction
- Limited reasoning
- Closed-domain

**Information-Seeking Agents**:
- Multiple sources
- Multi-step navigation
- Complex reasoning
- Open-domain

**Advantage**: Handles complex, multi-hop questions

### vs Retrieval-Augmented Generation

**RAG**:
- Query → retrieve relevant docs → generate answer
- Single retrieval step
- Context limited by model capacity
- Passive retrieval

**Information-Seeking Agents**:
- Iterative retrieval based on reasoning
- Multi-step exploration
- Selective information gathering
- Active navigation

**Advantage**: Adaptive, multi-step information gathering

## Relationship to Other Agent Types

### vs Data Collection Agents (AutoData)

**Data Collection**:
- Structured data extraction
- Pre-defined schemas
- Comprehensive coverage
- Engineering focus

**Information Seeking (WebDancer)**:
- Open-ended question answering
- Flexible information synthesis
- Targeted exploration
- Research focus

**Complementary**: Different tasks in information domain

### vs Web Scraping Agents

**Web Scraping**:
- Automated data extraction
- Site-specific scripts
- Batch processing
- May face [[webcloak|WebCloak]] defenses

**Information Seeking**:
- Question-driven exploration
- General web navigation
- Interactive search
- Legitimate research use

**Distinction**: Purpose and approach differ

## Future Directions

### Extended Action Spaces

**Proposed Actions**:
- summarize(text): Condense content
- compare(item1, item2): Explicit comparison
- verify(claim, source): Fact checking
- bookmark(url): Save for later
- annotate(text, note): Add metadata

**Benefits**:
- Richer interaction capabilities
- More explicit reasoning steps
- Better information management
- Improved synthesis

### Multimodal Understanding

**Capabilities**:
- Image interpretation
- Table parsing
- Chart analysis
- Video content understanding
- Cross-modal reasoning

**Applications**:
- Scientific paper understanding (figures)
- Financial report analysis (tables)
- Product comparison (images)
- Tutorial comprehension (videos)

### Long-Term Memory

**Features**:
- Cross-session information retention
- Incremental knowledge building
- User preference learning
- Contextual recall

**Benefits**:
- Avoid redundant searches
- Leverage past findings
- Personalized assistance
- Continuous improvement

### Domain Specialization

**Vertical Agents**:
- Scientific literature (research assistance)
- Legal research (case law navigation)
- Medical information (clinical guidelines)
- Financial analysis (market research)

**Advantages**:
- Domain-specific knowledge
- Specialized action spaces
- Targeted training data
- Higher accuracy in domain

## Practical Applications

### Academic Research

**Use Cases**:
- Literature review automation
- Citation network exploration
- Methodology comparison
- Research gap identification
- Trend analysis

**Benefits**:
- Accelerate research process
- Comprehensive coverage
- Unbiased exploration
- Time savings

### Business Intelligence

**Use Cases**:
- Market research
- Competitive analysis
- Technology assessment
- Industry trends
- Customer insights

**Benefits**:
- Data-driven decisions
- Comprehensive analysis
- Timely information
- Cost-effective research

### Technical Support

**Use Cases**:
- Documentation search
- Troubleshooting guides
- Code example finding
- API reference lookup
- Best practice identification

**Benefits**:
- Faster problem resolution
- Comprehensive solutions
- Learning assistance
- Productivity improvement

### Journalism and Fact-Checking

**Use Cases**:
- Background research
- Source verification
- Claim checking
- Historical context
- Expert finding

**Benefits**:
- Thorough investigation
- Accuracy improvement
- Time efficiency
- Source diversity

## Ethical Considerations

### Legitimate Use

**Acceptable**:
- Academic research
- Personal information seeking
- Journalistic investigation
- Business intelligence (authorized)
- Educational assistance

**Principles**:
- Respect terms of service
- Cite sources appropriately
- Verify information quality
- Acknowledge limitations

### Concerns

**Potential Issues**:
- Unauthorized data collection
- Copyright infringement
- Privacy violations
- Misinformation spread
- Over-reliance on automation

**Mitigation**:
- Clear usage guidelines
- Source credibility assessment
- Human oversight
- Transparency about limitations
- Respect for access controls

### Interaction with Defenses

**WebCloak Perspective**:
- [[webcloak|WebCloak]] defends against unauthorized scraping
- Information-seeking agents may be blocked
- Tension between access and protection
- Need for clear boundaries

**Balance**:
- Legitimate research needs access
- Content creators need protection
- Technical + legal solutions
- Authorized access mechanisms (APIs)

## Related Pages

### Entities

- [[webdancer|WebDancer]]
- [[autodata|AutoData]]
- [[webcloak|WebCloak]]
- [[tongyi-lab|Tongyi Lab]]

### Concepts

- [[react-framework|ReAct Framework]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]
- [[dapo|DAPO Algorithm]]
- [[synthetic-agent-training-data|Synthetic Agent Training Data]]
- [[short-cot|Short Chain-of-Thought]]
- [[long-cot|Long Chain-of-Thought]]

### Comparisons

- [[webdancer-vs-autodata|WebDancer vs AutoData]]
- [[webdancer-vs-webcloak|WebDancer vs WebCloak]]

### Sources

- [[webdancer-paper|WebDancer Paper]]
- [[autodata-paper|AutoData Paper]]
- [[webcloak-paper|WebCloak Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- [[autodata-paper|AutoData Research Paper]] (NeurIPS 2025)
- Various research on autonomous agents and information retrieval
