---
title: Two-Stage Agent Training
tags: [concept, reinforcement-learning, agent-training, sft, rl]
created: 2026-06-24
updated: 2026-06-24
---

# Two-Stage Agent Training

A training paradigm for autonomous agents that combines supervised fine-tuning (SFT) for cold-start initialization with reinforcement learning (RL) for generalization and improvement. This approach, pioneered by [[webdancer|WebDancer]], solves the critical cold-start problem in agentic RL where direct reinforcement learning achieves only 5% performance without supervised warm-up.

## Overview

Two-stage agent training addresses the fundamental challenge that language models cannot learn complex agentic behaviors through pure reinforcement learning from random initialization. The paradigm splits training into two sequential phases: Stage 1 uses supervised fine-tuning on high-quality trajectories to establish basic competence, while Stage 2 applies on-policy reinforcement learning to improve generalization, explore better strategies, and adapt to out-of-distribution scenarios.

## How It Works

### Stage 1: Supervised Fine-Tuning (SFT)

**Purpose**: Cold-start initialization to establish basic agent behavior

**Process**:
1. **Trajectory Collection**: Sample successful agent trajectories from strong models
   - [[webdancer|WebDancer]]: GPT-4o (Short-CoT) and QwQ-Plus (Long-CoT)
   - Expert demonstrations or rule-based systems
   - Human-labeled examples
   - Synthetic data generation

2. **Quality Filtering**: Multi-stage filtering ensures high-quality training data
   - **Validity control**: Verify correct format and structure
   - **Correctness verification**: Check task completion and answer quality
   - **Quality assessment**: Evaluate reasoning depth and efficiency
   - **Rejection sampling**: Keep only top-quality trajectories

3. **Supervised Training**: Train base model to imitate successful behaviors
   - Standard cross-entropy loss
   - Learn action sequences
   - Acquire reasoning patterns
   - Establish basic competence

**Data Requirements** ([[webdancer|WebDancer]] example):
- 7,678 Short-CoT trajectories (avg 4.56 actions, 510 tokens)
- 6,550 Long-CoT trajectories (avg 2.31 actions, 1599 tokens)
- Total: 14,228 high-quality trajectories
- Relatively modest dataset size

**Critical Finding**: Without SFT, direct RL achieves only ~5% success rate
- Sparse reward signals insufficient for random exploration
- Agent cannot discover successful behaviors through trial-and-error alone
- Cold-start problem fundamental barrier
- SFT provides essential starting point

### Stage 2: Reinforcement Learning (RL)

**Purpose**: Improve generalization beyond training distribution

**Process**:
1. **On-Policy Rollouts**: Agent generates trajectories from own policy
   - Explores environment with current behavior
   - Collects experience from own distribution
   - Enables learning from mistakes
   - Discovers novel strategies

2. **Reward Evaluation**: Assess trajectory quality
   - Task completion success/failure
   - Answer correctness
   - Process quality (format, efficiency)
   - [[llm-as-judge|LLM-as-Judge]] automated evaluation

3. **Policy Optimization**: Update agent using RL algorithms
   - [[dapo|DAPO]] (WebDancer): Decoupled clip and dynamic sampling
   - PPO (Proximal Policy Optimization)
   - Other on-policy methods
   - Iterative improvement

4. **Convergence**: Continue until performance plateaus

**Benefits**:
- **Longer reasoning**: RL increases average thought length and action count
- **Better exploration**: Discovers more effective information-seeking strategies
- **Generalization**: Adapts to out-of-distribution questions
- **Emergent capabilities**: Finds strategies not present in SFT data

**Key Insight**: RL enables generalization that pure imitation cannot achieve
- SFT learns to replicate training data
- RL learns to adapt and explore
- On-policy learning essential for agent improvement
- Combination creates autonomous capability

## Why Both Stages Are Essential

### Why SFT is Necessary

**The Cold-Start Problem**:
- Language models have no innate agentic behavior
- Random exploration in complex action spaces intractable
- Sparse reward signals provide insufficient guidance
- Credit assignment over long trajectories extremely difficult

**Empirical Evidence** ([[webdancer-paper|WebDancer research]]):
- Direct RL without SFT: ~5% success rate on GAIA benchmark
- SFT alone: Reasonable performance (baseline)
- SFT + RL: Best performance (51.5% on GAIA)
- SFT provides crucial initialization

**What SFT Provides**:
1. **Basic competence**: Establishes minimally viable agent behavior
2. **Action space understanding**: Learns which actions are valid and useful
3. **Task structure**: Understands problem format and solution patterns
4. **Reasonable starting policy**: Enables productive RL exploration

**Analogy**: Like teaching someone to drive before letting them optimize racing performance

### Why RL is Necessary

**Limitations of Pure Imitation**:
- SFT limited to training distribution
- Cannot adapt to novel situations
- Overfits to demonstration style
- Misses better strategies not in training data

**What RL Adds**:
1. **Generalization**: Adapts to out-of-distribution scenarios
2. **Exploration**: Discovers strategies beyond demonstrations
3. **Optimization**: Finds more efficient solution paths
4. **Robustness**: Learns to recover from mistakes

**Empirical Observations** ([[webdancer|WebDancer]]):
- After RL training, agents show:
  - Increased average thought length
  - More exploratory actions
  - Better performance on hard questions
  - Novel reasoning patterns
- Pass@3 improvements: Multiple sampling benefits from exploration

**Analogy**: Like transitioning from following a recipe to becoming an expert chef who improvises

## Technical Details

### SFT Data Collection Strategies

**Model-Generated Trajectories** (WebDancer approach):
- Use strong models (GPT-4o, QwQ-Plus) to generate demonstrations
- Cheaper than human annotation
- Scalable to large datasets
- Quality depends on generator capability

**Synthetic Data Generation**:
- [[crawlqa|CRAWLQA]]: Web crawling-based QA generation
- [[e2hqa|E2HQA]]: Easy-to-hard progression
- Diverse question coverage
- Zero human annotation

**Human Demonstrations**:
- Expert annotators perform tasks
- High quality but expensive
- Limited scale
- Good for complex domains

**Hybrid Approaches**:
- Combine model-generated and human data
- Use humans for quality control
- Iterative refinement
- Best of both worlds

### Short-CoT vs Long-CoT

**Short-CoT** (e.g., GPT-4o):
- Average 4.56 actions per trajectory
- Average 510 tokens per trajectory
- Concise reasoning steps
- More learnable for instruction models

**Long-CoT** (e.g., QwQ-Plus reasoning models):
- Average 2.31 actions per trajectory
- Average 1599 tokens per trajectory
- Extensive reasoning content
- Difficulty transferring to instruction models

**Key Finding**: Short-CoT from instruction models works better than Long-CoT from reasoning models
- Long-CoT knowledge doesn't transfer effectively
- Instruction models have different reasoning mechanisms
- Shorter, focused reasoning more practical
- Use instruction models for trajectory sampling

### RL Algorithm Choices

**On-Policy Methods** (WebDancer: [[dapo|DAPO]]):
- Learn from agent's own distribution
- Continuous policy improvement
- Better for exploration
- Higher sample efficiency for agentic tasks

**Key Features**:
- Decoupled clip mechanism (separate actor and critic)
- Dynamic sampling strategy (adaptive temperature)
- Sparse reward signals (end-of-trajectory)
- LLM-as-Judge evaluation

**Alternative: Off-Policy Methods**:
- Learn from replay buffer
- More sample efficient in some settings
- Can reuse older experiences
- Less common for agentic LLM training

### Reward Design

**WebDancer Reward Structure**:
1. **Binary format reward**: +1 if follows ReAct format
   - Ensures proper action structure
   - Validates observation presence
   - Maintains interaction protocol

2. **Answer correctness reward**: LLM-as-Judge evaluation
   - Assesses factual accuracy
   - Checks completeness
   - Evaluates relevance
   - Scalable automated assessment

**Sparse vs Dense Rewards**:
- WebDancer: Sparse (end-of-trajectory only)
- Advantages: Simpler, avoids reward hacking
- Disadvantages: Difficult credit assignment
- Future work: Dense reward shaping

## Applications and Examples

### WebDancer: Information-Seeking Agent

[[webdancer|WebDancer]] demonstrates two-stage training for autonomous research:

**Stage 1 (SFT)**:
- 14,228 trajectories from GPT-4o and QwQ-Plus
- CRAWLQA + E2HQA synthetic data
- Multi-stage filtering for quality
- Learns basic search → visit → answer behavior

**Stage 2 (RL with DAPO)**:
- On-policy trajectory generation
- Binary format + answer correctness rewards
- Decoupled actor-critic optimization
- Dynamic exploration-exploitation balance

**Results**:
- GAIA benchmark: 51.5% (Level 1: 61.5%, Level 2: 50%, Level 3: 25%)
- WebWalkerQA: 47.9%
- Demonstrates generalization to complex research tasks
- First end-to-end trained agent in Tongyi DeepResearch series

### Constrained Web Agents

[[web-agent-rl-constraints-paper|Web Agent RL with Constraints]]:
- Extends WebDancer paradigm
- Adds cost constraints (robots.txt violations, timeouts)
- Multi-cost RL on top of two-stage training
- Maintains SFT + RL structure with safety bounds

**Similarities**:
- SFT for cold start
- On-policy RL for generalization
- Web navigation domain
- Sparse reward signals

**Extension**:
- Lagrangian multipliers for constraints
- Multi-objective optimization
- Safety-aware exploration
- Practical deployment considerations

### General Applicability

**Other Agent Types**:
- **Tool-using agents**: API calls, code execution
- **Game-playing agents**: Strategy games, simulation
- **Dialogue agents**: Multi-turn conversation
- **Planning agents**: Task decomposition, scheduling

**Core Pattern**:
1. Collect/generate high-quality demonstrations
2. SFT to establish basic competence
3. RL to optimize and generalize
4. Domain-specific reward design

## Comparison with Alternatives

### Pure Supervised Learning

**Approach**: Train only on demonstrations, no RL

**Limitations**:
- Limited to training distribution
- Cannot improve beyond demonstrations
- Overfits to specific examples
- No exploration or adaptation

**When Sufficient**:
- Simple, well-defined tasks
- Complete demonstration coverage
- No need for generalization
- Cheaper and faster

### Pure Reinforcement Learning

**Approach**: Direct RL from random initialization

**Why It Fails** (5% performance):
- No starting point for exploration
- Sparse rewards provide insufficient guidance
- Action space too large for random search
- Credit assignment intractable

**Requirements for Success**:
- Dense reward signals
- Simpler action spaces
- Shorter episode lengths
- Or massive computational resources

### Imitation + Preference Learning (RLHF)

**Approach**: SFT + reward model training + RL optimization

**Similarities to Two-Stage**:
- Uses SFT for initialization
- Applies RL for improvement
- Off-policy preference learning

**Differences**:
- RLHF for alignment, not capability
- Human preferences vs task rewards
- Primarily for text generation
- Two-stage for agentic tasks

**Complementary**:
- Can combine both approaches
- RLHF for safety/alignment
- Two-stage for capability
- Comprehensive training pipeline

### Behavior Cloning with Augmentation

**Approach**: SFT on demonstrations + data augmentation

**Advantages**:
- Cheaper than RL
- More controllable
- Easier to debug

**Limitations**:
- Still limited to training distribution
- Augmentation strategies domain-specific
- No online learning
- Missing emergent capabilities

## Challenges and Future Directions

### Current Challenges

**SFT Data Quality**:
- Depends on demonstration source quality
- Expensive if using human experts
- Generator model biases transfer
- Coverage gaps in training distribution

**RL Sample Efficiency**:
- Requires many on-policy rollouts
- Expensive for large language models
- Slow convergence for complex tasks
- Computational cost significant

**Reward Design**:
- Sparse rewards make credit assignment hard
- Dense rewards risk reward hacking
- LLM-as-Judge has limitations
- Human evaluation expensive

**Long-CoT Transfer**:
- Reasoning model knowledge doesn't transfer well
- Instruction models learn differently
- Gap between model types
- Limits demonstration sources

**Hyperparameter Sensitivity**:
- Learning rates critical
- Clip thresholds important
- Exploration parameters affect performance
- Requires careful tuning

### Future Directions

**Better SFT Data**:
- Improved synthetic data generation
- Active learning for demonstration selection
- Curriculum learning (easy to hard)
- Adversarial augmentation

**More Efficient RL**:
- Off-policy methods for sample efficiency
- Model-based RL for planning
- Hierarchical RL for long-horizon tasks
- Meta-learning for quick adaptation

**Advanced Reward Shaping**:
- Dense intermediate rewards
- Multi-objective optimization
- Intrinsic motivation (curiosity)
- Self-supervised signals

**Transfer Learning**:
- Pre-train on simpler tasks
- Multi-task learning
- Knowledge distillation from reasoning models
- Cross-domain transfer

**Scalability**:
- Distributed training infrastructure
- Parallel environment execution
- Efficient trajectory collection
- Model compression for deployment

## Why It Matters

1. **Solves Cold-Start Problem**: SFT enables RL training by providing essential initialization; direct RL achieves only 5% performance without it.

2. **Enables Generalization**: RL phase allows agents to exceed training distribution and discover novel strategies impossible with pure imitation.

3. **Empirically Validated**: WebDancer demonstrates 51.5% on GAIA benchmark, proving paradigm effectiveness for complex reasoning tasks.

4. **Broadly Applicable**: Pattern extends beyond information-seeking to tool use, game playing, dialogue, and planning agents.

5. **Practical Training Recipe**: Provides concrete framework for building autonomous agents, balancing supervision and exploration.

6. **Foundation for Innovation**: Enables research on constrained RL, multi-agent coordination, and safety-aware agent training.

## Papers That Use This Concept

### Primary Source

**[[webdancer-paper|"WebDancer: Towards Autonomous Information Seeking Agency"]]** (Wu et al., NeurIPS 2025):
- Introduces two-stage paradigm explicitly
- Demonstrates 5% direct RL baseline
- Shows SFT + RL achieves 51.5% on GAIA
- CRAWLQA + E2HQA synthetic data
- DAPO algorithm for Stage 2
- Comprehensive ablation studies

### Extensions and Applications

**[[web-agent-rl-constraints-paper|"Reinforcement Learning for Web Agents Under Safety Constraints"]]**:
- Builds on WebDancer's two-stage approach
- Adds multi-cost RL constraints
- Maintains SFT + RL structure
- Safety-aware exploration
- Demonstrates paradigm extensibility

### Related Work

**Search-o1, R1-Searcher**: Information-seeking agents with alternative training approaches

**WebThinker**: Different agent architecture but shares two-stage philosophy

**RLHF Literature**: Related paradigm (SFT + RL) for alignment rather than capability

**Tool-Using Agents**: Apply similar cold-start + improvement patterns

## Related Concepts

- [[agentic-rl|Agentic Reinforcement Learning]] - RL for language model agents
- [[dapo|DAPO]] - Specific RL algorithm for Stage 2
- [[information-seeking-agents|Information-Seeking Agents]] - Primary application domain
- [[react-framework|ReAct Framework]] - Action structure for agent behavior
- [[crawlqa|CRAWLQA]] - Synthetic SFT data generation
- [[e2hqa|E2HQA]] - Easy-to-hard SFT data generation
- [[llm-as-judge|LLM-as-Judge]] - Reward evaluation method
- [[sparse-reward-signals|Sparse Reward Signals]] - Challenge in RL stage

## Related Pages

### Concepts
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[information-seeking-agents|Information-Seeking Agents]]
- [[react-framework|ReAct Framework]]
- [[dapo|DAPO]]
- [[on-policy-rl|On-Policy Reinforcement Learning]]
- [[sparse-reward-signals|Sparse Reward Signals]]

### Technologies
- [[webdancer|WebDancer]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]
- [[llm-as-judge|LLM-as-Judge]]

### Sources
- [[webdancer-paper|WebDancer Paper]]
- [[web-agent-rl-constraints-paper|Web Agent RL Constraints Paper]]

### Entities
- [[jialong-wu|Jialong Wu]]
- [[baixuan-li|Baixuan Li]]
- [[tongyi-lab|Tongyi Lab]]

### Comparisons
- [[webdancer-vs-autodata|WebDancer vs AutoData]]
- [[constrained-rl-vs-unconstrained-rl|Constrained RL vs Unconstrained RL]]
- [[short-cot-vs-long-cot|Short-CoT vs Long-CoT]]
