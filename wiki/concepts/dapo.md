---
title: DAPO (Decoupled Clip and Dynamic Sampling Policy Optimization)
tags: [concept, reinforcement-learning, algorithm, optimization]
created: 2026-06-24
updated: 2026-06-24
---

# DAPO (Decoupled Clip and Dynamic Sampling Policy Optimization)

A novel on-policy reinforcement learning algorithm developed by [[tongyi-lab|Tongyi Lab]] for training [[information-seeking-agents|information-seeking agents]], featuring decoupled clipping for actor and critic plus dynamic sampling strategies for improved stability and performance.

## Overview

DAPO is the core RL algorithm used to train [[webdancer|WebDancer]], the first end-to-end trained autonomous research agent. It addresses challenges specific to [[agentic-rl|agentic reinforcement learning]]: sparse rewards, long-horizon credit assignment, large action spaces (text generation), and non-stationary web environments.

## Key Innovations

### 1. Decoupled Clipping

**Problem**: Standard PPO uses coupled clipping for both policy (actor) and value function (critic), which may not be optimal for different optimization dynamics.

**Solution**: Separate clip ratios for actor and critic

**Actor Clipping** (Policy Update):
```
L_actor = -min(
    ratio * advantage,
    clip(ratio, 1 - epsilon_actor, 1 + epsilon_actor) * advantage
)
```

**Critic Clipping** (Value Function Update):
```
L_critic = (value - target)^2  # with separate critic clipping
```

**Benefits**:
- Actor clip: Prevents drastic policy changes, maintains stability
- Critic clip: Stabilizes value estimation
- Decoupling allows independent tuning
- Better convergence properties

**vs Standard PPO**:
- PPO: Single epsilon for both actor and critic
- DAPO: epsilon_actor and epsilon_critic can differ
- More flexibility in controlling update magnitudes

### 2. Dynamic Sampling Strategy

**Problem**: Fixed exploration parameters (temperature) may not adapt to changing environment dynamics or learning progress.

**Solution**: Adaptive sampling with dynamic temperature adjustment

**Features**:
- Exploration-exploitation balance
- Adjust based on learning progress
- Respond to environment dynamics
- On-policy trajectory collection

**Benefits**:
- Better exploration in early training
- More exploitation as policy improves
- Adapt to web environment non-stationarity
- Sample efficiency improvements

**Implementation**:
```python
# Dynamic temperature adjustment
temperature = adjust_temperature(
    current_performance,
    learning_progress,
    environment_statistics
)

# Sample actions with adaptive temperature
action = policy.sample(state, temperature=temperature)
```

### 3. On-Policy Learning

**Choice**: DAPO is on-policy (learns from current policy's trajectories)

**Rationale for Agentic Tasks**:
- Web environment is non-stationary
- Content changes over time
- Search results vary
- On-policy handles distribution shift better

**Trade-offs**:
- **Advantage**: More stable, better for non-stationary environments
- **Disadvantage**: Sample inefficient (data discarded after update)

**vs Off-Policy**:
- Off-policy: Can reuse replay buffer, more sample efficient
- On-policy: More stable, better for WebDancer's web environment

## Algorithm Details

### Training Loop

```python
for iteration in range(num_iterations):
    # 1. Collect on-policy trajectories
    trajectories = []
    for question in batch_questions:
        trajectory = agent.rollout(question)
        trajectories.append(trajectory)
    
    # 2. Evaluate trajectories with LLM-as-Judge
    rewards = []
    for trajectory in trajectories:
        format_reward = judge.check_format(trajectory)
        answer_reward = judge.evaluate_answer(trajectory)
        total_reward = format_reward + answer_reward
        rewards.append(total_reward)
    
    # 3. Compute advantages using value function
    advantages = compute_advantages(trajectories, rewards, value_function)
    
    # 4. Update actor (policy) with decoupled clipping
    for trajectory, advantage in zip(trajectories, advantages):
        ratio = new_policy.prob(trajectory) / old_policy.prob(trajectory)
        clipped_ratio = clip(ratio, 1 - epsilon_actor, 1 + epsilon_actor)
        actor_loss = -min(ratio * advantage, clipped_ratio * advantage)
        optimizer_actor.step(actor_loss)
    
    # 5. Update critic (value function) with separate clipping
    for trajectory, reward in zip(trajectories, rewards):
        value_pred = value_function(trajectory.state)
        value_target = compute_target(reward, trajectory)
        critic_loss = (value_pred - value_target)**2  # with clipping
        optimizer_critic.step(critic_loss)
    
    # 6. Update sampling strategy
    update_temperature(current_performance)
```

### Reward Design

**Binary Format Reward**:
- +1 if trajectory follows ReAct format (Thought → Action → Observation)
- 0 otherwise
- Ensures structured behavior

**Answer Correctness Reward**:
- [[llm-as-judge|LLM-as-Judge]] evaluates answer quality
- Considers: factual accuracy, completeness, relevance
- Scalable automated evaluation
- No human annotation required

**Combined Reward**:
```
R(trajectory) = R_format(trajectory) + R_answer(trajectory.answer)
```

**Sparse Signal**:
- Reward only at end of trajectory
- No intermediate feedback
- Long-horizon credit assignment challenge
- Mitigated by SFT cold-start

### Advantage Computation

**Generalized Advantage Estimation (GAE)**:
```
A_t = sum_{l=0}^{infinity} (gamma * lambda)^l * delta_{t+l}

where delta_t = r_t + gamma * V(s_{t+1}) - V(s_t)
```

**Parameters**:
- gamma: Discount factor (future reward weighting)
- lambda: GAE parameter (bias-variance trade-off)

**Purpose**:
- Reduces variance in policy gradients
- Better credit assignment
- More stable training

## Application in WebDancer

### Stage 2 of Training

**Prerequisites**:
- Stage 1 (SFT) completed
- Agent has reasonable initialization
- Without SFT, direct DAPO only achieves ~5%

**Process**:
1. Agent generates trajectories on research questions
2. LLM-as-Judge evaluates (format + answer)
3. DAPO updates policy to maximize rewards
4. Iterate until convergence

**Results**:
- Improves over SFT-only baseline
- Enables generalization beyond training distribution
- Increases thought length and action count
- Better exploration strategies

### Hyperparameters

**Critical Parameters**:
- epsilon_actor: Clip ratio for policy updates (e.g., 0.2)
- epsilon_critic: Clip ratio for value updates (e.g., 0.2, can differ)
- learning_rate_actor: Policy learning rate
- learning_rate_critic: Value function learning rate
- temperature: Initial exploration temperature
- batch_size: Number of trajectories per update
- num_iterations: Total training iterations

**Tuning Considerations**:
- Larger epsilon: More aggressive updates, less stable
- Smaller epsilon: More conservative, slower learning
- Separate tuning for actor and critic enables flexibility
- Dynamic temperature adapts during training

## Performance Impact

### GAIA Benchmark

**SFT-Only Baseline**: ~40-45% (estimated)
**SFT + DAPO**: 51.5% (QwQ-32B)

**Improvement**:
- Significant gains from RL
- Better generalization
- Longer reasoning chains
- Adaptive strategies

### WebWalkerQA Benchmark

**SFT-Only Baseline**: ~40% (estimated)
**SFT + DAPO**: 47.9%

**Benefits**:
- Multi-step navigation improvement
- Better exploration
- Information synthesis quality

### Pass@3 Results

**GAIA Pass@3**: 64.1% (vs 51.5% Pass@1)
**WebWalkerQA Pass@3**: 62.0% (vs 47.9% Pass@1)

**Analysis**:
- Multiple attempts significantly help
- Agent learns diverse strategies
- RL enables exploration of multiple paths

## Comparison with Standard PPO

### PPO (Proximal Policy Optimization)

**Original PPO**:
```
L_PPO = min(
    ratio * advantage,
    clip(ratio, 1 - epsilon, 1 + epsilon) * advantage
)
```

**Characteristics**:
- Single epsilon for all updates
- Coupled actor-critic clipping
- Fixed sampling strategy
- Widely used baseline

### DAPO Improvements

**Decoupled Clipping**:
- PPO: Single epsilon
- DAPO: epsilon_actor and epsilon_critic
- Better control over different update dynamics

**Dynamic Sampling**:
- PPO: Fixed temperature/sampling
- DAPO: Adaptive exploration-exploitation
- Better for non-stationary environments

**Agentic Focus**:
- PPO: General RL
- DAPO: Optimized for agent tasks (text generation, sparse rewards, web interaction)

**Performance**:
- DAPO shows advantages on WebDancer benchmarks
- Particularly beneficial for agentic tasks
- Stability improvements in long-horizon scenarios

## Key Insights from WebDancer

### 1. SFT Essential for DAPO Success

**Finding**: Direct DAPO without SFT achieves only ~5%

**Analysis**:
- Sparse rewards insufficient for random initialization
- Agent cannot discover successful behaviors
- SFT provides reasonable starting policy
- DAPO then refines and generalizes

**Implication**: Always warm-start with supervised learning before DAPO

### 2. RL Enables Longer Reasoning

**Finding**: After DAPO training:
- Increased average thought length
- Increased average action count
- Better exploration strategies
- Higher success rates

**Analysis**:
- SFT learns to imitate training trajectories
- DAPO learns to adapt and explore beyond training data
- On-policy exploration discovers better strategies
- Longer reasoning correlates with accuracy

**Implication**: RL crucial for generalization

### 3. Web Environment Dynamics

**Finding**: Temperature has minimal impact on performance

**Analysis**:
- Web content changes over time
- Search results vary
- External stochasticity dominates model stochasticity
- Dynamic sampling adapts but environment dominates

**Implication**: Robustness more important than precise control

### 4. On-Policy Better for Web

**Choice**: DAPO is on-policy

**Rationale**:
- Web environment non-stationary
- On-policy handles distribution shift
- More stable training
- Worth sample inefficiency trade-off

**Alternative**: Off-policy could be more sample efficient but less stable

## Advantages

### For Agentic Tasks

**Sparse Rewards**:
- Decoupled clipping stabilizes training
- Dynamic sampling improves exploration
- Advantage estimation handles long horizons

**Large Action Spaces**:
- Text generation creates combinatorial spaces
- Policy clipping prevents drastic changes
- Gradual improvement over iterations

**Non-Stationary Environments**:
- On-policy handles distribution shift
- Dynamic sampling adapts to changes
- Robust to web dynamics

**Long Horizons**:
- Multi-step trajectories (4-5 actions avg)
- Credit assignment through advantages
- Value function estimates future returns

### Compared to Alternatives

**vs Vanilla Policy Gradient**:
- DAPO: Clipping for stability
- VPG: High variance, unstable
- DAPO clearly superior

**vs Q-Learning (DQN)**:
- DAPO: Policy-based, handles continuous text actions
- DQN: Value-based, discrete actions
- DAPO better for LLM agents

**vs SAC (Off-Policy)**:
- DAPO: On-policy, more stable for web
- SAC: Off-policy, more sample efficient
- Trade-off: stability vs efficiency, DAPO chose stability

## Limitations

### Sample Inefficiency

**Problem**:
- On-policy requires many trajectories
- Data discarded after each update
- Computationally expensive

**Mitigation**:
- Parallel trajectory collection
- Batch updates
- Efficient SFT cold-start reduces RL iterations needed

### Hyperparameter Sensitivity

**Problem**:
- Multiple hyperparameters (epsilon_actor, epsilon_critic, learning rates, temperature)
- Expensive to tune (each run costly)
- Interaction effects

**Mitigation**:
- Systematic hyperparameter search
- Transfer from similar tasks
- Adaptive methods (dynamic sampling helps)

### Evaluation Challenges

**Problem**:
- LLM-as-Judge has biases
- Open-ended answers hard to evaluate
- Reward signal quality critical

**Mitigation**:
- Multiple judge models
- Human validation subset
- Improved judge training

## Future Enhancements

### Dense Rewards

**Opportunity**:
- Per-step feedback instead of end-of-trajectory
- Intermediate correctness signals
- Reasoning quality metrics
- Action efficiency rewards

**Benefits**:
- Easier exploration
- Faster learning
- Better credit assignment
- Sample efficiency

### Off-Policy Variants

**Opportunity**:
- Hybrid on-off policy
- Replay buffer for efficiency
- Importance sampling corrections

**Benefits**:
- Sample efficiency
- Faster training
- Better data utilization

**Challenge**:
- Maintain stability
- Handle distribution shift
- Preserve DAPO advantages

### Hierarchical Extension

**Opportunity**:
- High-level policy: Select sub-goals
- Low-level policy: Execute actions
- Temporal abstraction

**Benefits**:
- Long-horizon tasks
- Reusable skills
- Better credit assignment
- Transfer learning

## Related Pages

### Entities

- [[webdancer|WebDancer]]
- [[tongyi-lab|Tongyi Lab]]

### Concepts

- [[agentic-rl|Agentic Reinforcement Learning]]
- [[two-stage-agent-training|Two-Stage Agent Training (SFT + RL)]]
- [[on-policy-rl|On-Policy Reinforcement Learning]]
- [[sparse-reward-signals|Sparse Reward Signals]]
- [[llm-as-judge|LLM-as-Judge]]
- [[information-seeking-agents|Information-Seeking Agents]]

### Sources

- [[webdancer-paper|WebDancer Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- Proximal Policy Optimization (Schulman et al., 2017)
- Generalized Advantage Estimation (Schulman et al., 2016)
