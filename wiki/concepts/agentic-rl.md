---
title: Agentic Reinforcement Learning
tags: [concept, reinforcement-learning, agents, machine-learning]
created: 2026-06-24
updated: 2026-06-24
---

# Agentic Reinforcement Learning

The application of reinforcement learning techniques to train autonomous agents (particularly LLM-based agents) to perform complex, multi-step tasks through interaction with environments and learning from sparse reward signals.

## Overview

Agentic RL combines the reasoning capabilities of large language models with reinforcement learning's ability to optimize behavior through trial and error. Unlike traditional RLHF (which aligns model outputs to preferences), agentic RL trains agents to accomplish tasks autonomously through tool use, environment interaction, and multi-step planning.

## Core Characteristics

### Environment Interaction

**Agent-Environment Loop**:
```
Agent observes state → Generates action → Environment responds → Agent receives reward → Repeat
```

**For LLM Agents**:
- **State**: Current context, conversation history, observations
- **Action**: Tool use (search, visit, API calls), answer generation
- **Observation**: Tool outputs, environment feedback
- **Reward**: Task completion, correctness, efficiency

### Sparse Rewards

**Challenge**: Feedback only at episode end

**Example (WebDancer)**:
- Agent takes 4-5 actions (search, visit, answer)
- Reward only when final answer provided
- Must credit reward back to all preceding actions
- Long-horizon credit assignment problem

**Implications**:
- Difficult exploration (random behavior rarely succeeds)
- Need for good initialization (SFT cold-start)
- Sample inefficiency
- Training instability

### Multi-Step Reasoning

**Characteristics**:
- Decompose complex tasks into subtasks
- Iterative information gathering
- Adaptive strategy adjustment
- Synthesis of final output

**Example ([[webdancer|WebDancer]])**:
1. Formulate search query based on question
2. Execute search, observe results
3. Select relevant URL to visit
4. Read page, extract information
5. Decide if sufficient or need more
6. Repeat or provide final answer

## Key Approaches

### Two-Stage Training (SFT + RL)

**[[two-stage-agent-training|WebDancer Paradigm]]**:

**Stage 1: Supervised Fine-Tuning**:
- Train on high-quality trajectories
- Learn basic agent behavior
- Establish ReAct format
- Provide reasonable initialization

**Critical Insight**: Without SFT, direct RL only achieves ~5% success

**Stage 2: Reinforcement Learning**:
- On-policy trajectory collection
- Reward based on answer correctness
- Policy optimization ([[dapo|DAPO algorithm]])
- Generalization improvement

**Benefits**:
- SFT provides cold-start solution
- RL enables generalization beyond training distribution
- Longer reasoning after RL (increased thoughts + actions)
- Better exploration strategies

### On-Policy vs Off-Policy

**On-Policy (WebDancer)**:
- Agent collects trajectories from current policy
- Learns from its own experience
- Better for non-stationary environments
- Sample inefficient (data discarded after update)

**[[dapo|DAPO Algorithm]] Features**:
- Decoupled clip for actor and critic
- Dynamic sampling strategy
- Adaptive exploration-exploitation
- Stable training

**Off-Policy**:
- Agent learns from replay buffer
- Can reuse past experience
- Sample efficient
- Risk of distribution shift

**Trade-offs**:
- On-policy: More stable, less efficient
- Off-policy: More efficient, less stable
- WebDancer chose on-policy for web environment dynamics

### Reward Design

**Binary Format Reward**:
- +1 if trajectory follows correct ReAct format
- 0 otherwise
- Ensures agent maintains structured behavior

**Answer Correctness Reward**:
- LLM-as-Judge evaluates answer quality
- Factual accuracy, completeness, relevance
- Scalable automated evaluation
- No human annotation required

**Combined Reward**:
```
R = R_format + R_answer
```

**Challenges**:
- Sparse signal (only at end)
- Binary vs continuous trade-off
- Judge model quality dependency
- Reward hacking risks

## Key Algorithms

### DAPO (WebDancer)

[[dapo|DAPO (Decoupled Clip and Dynamic Sampling Policy Optimization)]]:

**Innovations**:
1. **Decoupled Clipping**:
   - Separate clip ratios for actor (policy) and critic (value)
   - Actor clip: Prevents drastic policy changes
   - Critic clip: Stabilizes value estimation
   - Better convergence than coupled clipping

2. **Dynamic Sampling**:
   - Adaptive temperature adjustment
   - Exploration-exploitation balance
   - On-policy trajectory collection
   - Sample efficiency improvements

**Training Loop**:
```python
for iteration in range(num_iterations):
    # Collect on-policy trajectories
    trajectories = agent.rollout(questions)
    
    # Evaluate with LLM-as-Judge
    rewards = judge.evaluate(trajectories)
    
    # Compute advantages
    advantages = compute_advantages(trajectories, rewards)
    
    # Update policy (actor) with clipping
    actor_loss = -min(ratio * adv, clip(ratio, 1-ε_actor, 1+ε_actor) * adv)
    update_actor(actor_loss)
    
    # Update value function (critic) with clipping
    critic_loss = (value - target)^2  # with clipping
    update_critic(critic_loss)
```

**Results**:
- GAIA: 51.5% (QwQ-32B)
- WebWalkerQA: 47.9%
- Significant improvement over SFT-only

### PPO (Baseline)

**Proximal Policy Optimization**:
- Common baseline for LLM RL
- Clipped surrogate objective
- Trust region constraint
- Stable training

**Comparison with DAPO**:
- PPO: Coupled actor-critic clipping
- DAPO: Decoupled clipping + dynamic sampling
- DAPO shows advantages for agentic tasks

### Other Approaches

**DQN/SAC (Off-Policy)**:
- Value-based or actor-critic
- Replay buffer for sample efficiency
- Used in some agent research

**Hierarchical RL**:
- High-level policy selects sub-goals
- Low-level policy executes actions
- Promising for long-horizon tasks

**Curiosity-Driven**:
- Intrinsic motivation for exploration
- Predict next state novelty
- Helps with sparse rewards

## Critical Insights (from WebDancer)

### 1. SFT Essential for Cold-Start

**Finding**: Direct RL without SFT achieves only ~5%

**Analysis**:
- Sparse rewards insufficient for random initialization
- Agent cannot discover successful behaviors through pure exploration
- SFT provides reasonable starting policy
- RL then refines and generalizes

**Implication**: Always warm-start with supervised learning

### 2. RL Enables Longer Reasoning

**Finding**: After RL, agents show:
- Increased average thought length
- Increased average action count
- Better exploration strategies
- Higher success rates

**Analysis**:
- SFT learns to imitate training trajectories
- RL learns to adapt and explore
- On-policy exploration discovers better strategies
- Longer reasoning correlates with accuracy

**Implication**: RL crucial for generalization

### 3. Reasoning Models Benefit Less from RL

**Finding**: Reasoning models (QwQ-Plus) show smaller RL gains than instruction models

**Analysis**:
- Sparse reward signals less effective for reasoning models
- Reasoning models may already internally optimize reasoning
- Agentic tasks require action selection, not just reasoning depth
- Different optimization objectives

**Implication**: Use instruction models for agentic RL

### 4. Web Environment is Non-Stationary

**Finding**: Temperature has minimal impact on performance

**Analysis**:
- Web content changes over time
- Search results vary
- External stochasticity dominates model stochasticity
- Agent must be robust to dynamics

**Implication**: Robustness more important than precise control

### 5. Short-CoT Better than Long-CoT

**Finding**: [[short-cot|Short-CoT trajectories]] (GPT-4o) transfer better than [[long-cot|Long-CoT]] (QwQ-Plus)

**Analysis**:
- Short-CoT: 4.56 actions, 510 tokens - more learnable
- Long-CoT: 2.31 actions, 1599 tokens - knowledge transfer difficulty
- Reasoning model knowledge doesn't translate to agent behavior
- Concise, action-focused reasoning more effective

**Implication**: Use instruction models for trajectory sampling

## Challenges

### Sparse Rewards

**Problem**:
- Feedback only at trajectory end
- Long-horizon credit assignment
- Exploration difficulty
- Sample inefficiency

**Solutions**:
- SFT warm-start (essential)
- Dense reward shaping (future)
- Auxiliary tasks
- Curiosity-driven exploration

### Large Action Spaces

**Problem**:
- Text generation creates combinatorial action space
- Search queries can be arbitrary text
- URLs can be any from search results
- Difficult to explore effectively

**Solutions**:
- Constrain action space (limited tool set)
- Hierarchical policies (high-level + low-level)
- Learned action proposals
- Guided exploration

### Sample Inefficiency

**Problem**:
- On-policy RL requires many trajectories
- Each trajectory requires environment interaction
- Web environment interaction is slow
- Training computationally expensive

**Solutions**:
- Off-policy methods (trade stability)
- Parallel environment execution
- Simulated environments (where possible)
- Transfer learning

### Evaluation Challenges

**Problem**:
- Open-ended answers hard to evaluate
- Human evaluation expensive and slow
- LLM-as-Judge has biases and errors
- Benchmarks may not cover real-world diversity

**Solutions**:
- Multiple evaluation metrics
- Human validation of subset
- Improved judge models
- Comprehensive benchmark suites

## Comparison with Related RL Applications

### vs RLHF (Alignment)

**RLHF**:
- Align LLM outputs to human preferences
- Reward model trained on preference data
- Single-step generation task
- Dense feedback signal

**Agentic RL**:
- Train agents to accomplish tasks
- Reward based on task success
- Multi-step reasoning and action
- Sparse feedback signal

**Distinction**: Alignment vs capability

### vs Traditional RL (Games, Robotics)

**Traditional RL**:
- Discrete or continuous low-dim actions
- Dense reward signals (per timestep)
- Stationary environments (mostly)
- Learn from scratch or simple features

**Agentic RL**:
- High-dimensional text actions
- Sparse rewards (end of episode)
- Non-stationary environments (web)
- Leverage pre-trained LLM knowledge

**Distinction**: Scale and initialization

### vs Tool Use Learning

**Tool Use**:
- Learn to use specific APIs/tools
- Often single tool call per task
- Clear tool documentation
- More structured problem

**Agentic RL**:
- Multi-step tool orchestration
- Iterative decision-making
- Exploration and planning
- Open-ended problems

**Distinction**: Single-step vs multi-step

## Applications

### Information Seeking

**[[webdancer|WebDancer]]**:
- Search and visit actions
- Multi-hop question answering
- GAIA, WebWalkerQA benchmarks
- 51.5% accuracy on GAIA

**Benefits**:
- Adaptive exploration
- Learned search strategies
- Generalization to new questions

### Task Automation

**Potential Applications**:
- Email management
- Calendar scheduling
- Document processing
- Workflow automation

**RL Benefits**:
- Learn user preferences
- Adapt to changing requirements
- Optimize for efficiency
- Continuous improvement

### Code Generation

**Applications**:
- Test generation
- Bug fixing
- Code refactoring
- API usage learning

**RL Benefits**:
- Learn from execution feedback
- Optimize for correctness
- Explore solution space
- Improve over iterations

### Game Playing

**Examples**:
- Text-based games
- Strategy games
- Negotiation scenarios
- Multi-agent competition

**RL Benefits**:
- Learn optimal strategies
- Adapt to opponents
- Long-term planning
- Emergent behaviors

## Future Directions

### Dense Reward Shaping

**Approaches**:
- Intermediate subgoal rewards
- Per-step correctness signals
- Reasoning quality metrics
- Action efficiency rewards

**Benefits**:
- Easier exploration
- Faster learning
- Better credit assignment
- Sample efficiency

### Hierarchical RL

**Structure**:
- High-level policy: Select subgoals
- Low-level policy: Execute actions
- Temporal abstraction
- Reusable skills

**Benefits**:
- Long-horizon tasks
- Transfer learning
- Compositional generalization
- Interpretability

### Multi-Task RL

**Approach**:
- Train on multiple tasks simultaneously
- Shared representations
- Task-specific heads
- Transfer across tasks

**Benefits**:
- Better generalization
- Sample efficiency (shared learning)
- Robust representations
- Broader capabilities

### Off-Policy Efficiency

**Approaches**:
- Improved replay buffers
- Off-policy corrections
- Hybrid on-off policy
- Model-based RL

**Benefits**:
- Sample efficiency
- Faster training
- Better data utilization
- Lower computational cost

### Better Exploration

**Approaches**:
- Curiosity-driven exploration
- Information gain objectives
- Diversity encouragement
- Learned exploration strategies

**Benefits**:
- Overcome sparse rewards
- Discover novel strategies
- Robust to initialization
- Better coverage

## Practical Considerations

### Training Infrastructure

**Requirements**:
- Large-scale computing (GPUs/TPUs)
- Distributed training capability
- Environment parallelization
- LLM-as-Judge infrastructure

**WebDancer Scale**:
- Base model: QwQ-32B (32 billion parameters)
- Thousands of on-policy trajectories
- Multiple training iterations
- Significant computational investment

### Hyperparameter Tuning

**Critical Parameters**:
- Learning rates (actor and critic)
- Clip epsilons
- Temperature for exploration
- Batch size and trajectory count

**Challenges**:
- Expensive to tune (each run costly)
- High variance in performance
- Environment-specific optimal values
- Interaction effects

### Evaluation Strategy

**Metrics**:
- Task success rate (primary)
- Average reward
- Trajectory length
- Action efficiency
- Generalization to held-out tasks

**Benchmarks**:
- GAIA: General AI assistants
- WebWalkerQA: Web navigation
- BrowseComp: Complex browsing
- Domain-specific benchmarks

## Related Pages

### Entities

- [[webdancer|WebDancer]]
- [[tongyi-lab|Tongyi Lab]]
- [[dapo|DAPO Algorithm]]

### Concepts

- [[two-stage-agent-training|Two-Stage Agent Training (SFT + RL)]]
- [[dapo|DAPO Algorithm]]
- [[information-seeking-agents|Information-Seeking Agents]]
- [[react-framework|ReAct Framework]]
- [[short-cot|Short Chain-of-Thought]]
- [[long-cot|Long Chain-of-Thought]]
- [[sparse-reward-signals|Sparse Reward Signals]]
- [[llm-as-judge|LLM-as-Judge]]
- [[on-policy-rl|On-Policy Reinforcement Learning]]
- [[synthetic-agent-training-data|Synthetic Agent Training Data]]

### Sources

- [[webdancer-paper|WebDancer Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- Proximal Policy Optimization (Schulman et al., 2017)
- Various RL textbooks and research papers
