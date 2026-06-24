---
title: Lagrangian Constrained RL
tags: [concept, reinforcement-learning, constrained-optimization, lagrange-multipliers]
created: 2026-06-24
updated: 2026-06-24
---

# Lagrangian Constrained RL

A constrained reinforcement learning technique that uses Lagrange multipliers and dual optimization to transform constrained RL problems into unconstrained saddle-point optimization, enabling simultaneous policy learning and constraint satisfaction.

## Overview

Constrained RL problems require maximizing reward while satisfying cost constraints. Direct constraint enforcement is difficult during gradient-based learning. Lagrangian methods convert the constrained problem into an unconstrained min-max game between policy parameters (primal) and constraint penalties (dual), with convergence guarantees to constraint-satisfying optimal policies.

## Mathematical Foundation

### Primal Problem (Constrained)

```
maximize_θ E[R(τ)]
subject to E[Cᵢ(τ)] ≤ bᵢ  for i = 1,...,n
```

**Components**:
- θ: Policy parameters
- R(τ): Cumulative reward over trajectory τ
- Cᵢ(τ): Cumulative cost for constraint i
- bᵢ: Budget threshold for constraint i

### Dual Problem (Unconstrained)

**Lagrangian Function**:
```
L(θ, λ) = E[R(τ)] - Σᵢ λᵢ·(E[Cᵢ(τ)] - bᵢ)
       = E[R(τ)] - Σᵢ λᵢ·E[Cᵢ(τ)] + Σᵢ λᵢ·bᵢ
```

**Lagrange Multipliers** (λᵢ ≥ 0):
- Penalty weight for violating constraint i
- Higher λᵢ → stronger enforcement of constraint i
- λᵢ = 0 → constraint i not active

**Saddle-Point Formulation**:
```
min_λ≥0 max_θ L(θ, λ)
```

**Optimal Solution**:
- θ*: Policy satisfying all constraints and maximizing reward
- λ*: Optimal constraint penalties (KKT conditions)

### Bidirectional Gradient Updates

**Primal Step (Policy Maximization)**:
```
θ ← θ + α_θ · ∇θ L(θ, λ)
  = θ + α_θ · ∇θ [E[R(τ)] - Σᵢ λᵢ·E[Cᵢ(τ)]]
```
Gradient ascent: Increase reward, decrease weighted costs.

**Dual Step (Multiplier Minimization)**:
```
λᵢ ← max(0, λᵢ + α_λ · (E[Cᵢ(τ)] - bᵢ))
```
Gradient ascent on dual: Increase λᵢ if constraint violated, decrease if satisfied.

**Projection**: max(0, ·) ensures λᵢ ≥ 0 (non-negative penalty).

## Application to Web Agents

From [[web-agent-rl-constraints-paper|constrained web agent research]]:

### Multi-Cost Web Agent Lagrangian

```
L(θ, λ) = E[R(τ)] - λ₁·(E[C₁(τ)] - b₁) - λ₂·(E[C₂(τ)] - b₂) - λ₃·(E[C₃(τ)] - b₃)
```

**Three Constraints**:
1. **Request count**: E[C₁] ≤ b₁ = 12 requests
2. **Response latency**: E[C₂] ≤ b₂ = 600ms average
3. **Failure penalties**: E[C₃] ≤ b₃ = 3.0 failure cost

**Three Lagrange Multipliers**:
- λ₁: Request quota penalty
- λ₂: Latency SLA penalty  
- λ₃: Failure risk penalty

See [[multi-cost-rl|Multi-Cost RL]] for detailed cost modeling.

### Training Algorithm

**Episode Collection**:
```python
for episode in range(num_episodes):
    # Rollout policy to collect trajectory
    trajectory = agent.rollout(task, policy_θ)
    
    # Measure costs
    c1 = count_requests(trajectory)
    c2 = avg_latency(trajectory)
    c3 = failure_penalty(trajectory)
    
    # Measure reward
    reward = task_success(trajectory)
    
    # Store in replay buffer
    buffer.add(trajectory, reward, [c1, c2, c3])
```

**Policy Update (Primal)**:
```python
batch = buffer.sample(batch_size)
policy_loss = -mean(batch.reward) + λ1*mean(batch.c1) + λ2*mean(batch.c2) + λ3*mean(batch.c3)
optimizer_θ.step(policy_loss)
```

**Multiplier Update (Dual)**:
```python
constraint_violations = [
    mean(batch.c1) - b1,  # request budget violation
    mean(batch.c2) - b2,  # latency budget violation
    mean(batch.c3) - b3   # failure budget violation
]

for i in range(3):
    λ[i] = max(0, λ[i] + α_λ * constraint_violations[i])
```

### Convergence Dynamics

**Early Training** (λ ≈ 0):
- Policy explores without constraint awareness
- Costs often exceed budgets
- Multipliers begin increasing

**Mid Training** (λ growing):
- Policy starts reducing costs to avoid penalties
- Some constraints become satisfied
- Active constraints have growing λᵢ

**Late Training** (λ stable):
- Policy satisfies all constraints
- Multipliers stabilize at optimal values
- Convergence to constrained optimum

**Typical Evolution** (from experiments):
- Episodes 0-100: λ₁=0.02, λ₂=0.01, λ₃=0.05
- Episodes 100-500: λ₁=0.15, λ₂=0.08, λ₃=0.22
- Episodes 500+: λ₁=0.28, λ₂=0.12, λ₃=0.35 (stable)

## Advantages

### 1. Automatic Constraint Balancing

**No Manual Weight Tuning**:
- Traditional reward shaping: R' = R - α₁c₁ - α₂c₂ - α₃c₃ (must tune αᵢ)
- Lagrangian: Only specify budgets bᵢ, multipliers adapt automatically

**Dynamic Adaptation**:
- Tight constraints get higher λᵢ
- Slack constraints get lower λᵢ
- Self-adjusting to problem difficulty

### 2. Convergence Guarantees

**Theoretical Properties** (under convexity assumptions):
- Converges to optimal constrained policy
- Satisfies all constraints in expectation
- Achieves highest reward among feasible policies

**Practical Convergence** (non-convex, neural network policies):
- Local optima possible
- Empirically robust with proper hyperparameters
- [[web-agent-rl-constraints-paper|Web agent experiments]]: 95%+ constraint satisfaction

### 3. Scalability to Multiple Constraints

**Single Constraint**: One multiplier λ
**Multiple Constraints**: Multiple multipliers λ₁, λ₂, ..., λₙ

**Computational Overhead**:
- Linear in number of constraints
- Each adds one scalar update per iteration
- Efficient even for 5-10 constraints

### 4. Integration with Existing RL

**Compatible with**:
- Policy gradient methods (REINFORCE, A2C, PPO)
- Actor-Critic architectures
- Off-policy methods (DQN, SAC)
- On-policy methods ([[dapo|DAPO]])

**Integration**: Replace loss function
```python
# Standard RL
loss = -E[R]

# Lagrangian Constrained RL
loss = -E[R] + Σᵢ λᵢ·E[Cᵢ]
```

## Comparison with Alternatives

### vs Projection Methods

**Projection Approach**:
- Enforce constraints by projecting policy onto feasible set
- Hard constraint satisfaction at every step
- Requires computing feasible policy space (intractable for complex constraints)

**Lagrangian Approach**:
- Soft constraint penalties
- Converges to satisfaction in expectation
- Simple gradient updates

**Trade-off**: Lagrangian easier to implement, projection gives hard guarantees.

### vs Constrained Policy Optimization (CPO)

**CPO (Achiam et al. 2017)**:
- Trust region method for constrained RL
- Guarantees constraint satisfaction at each policy update
- More conservative, slower learning

**Lagrangian (This Work)**:
- Faster convergence (less conservative)
- Soft constraint satisfaction during learning
- Simpler implementation

**Use Case**: CPO for safety-critical (robotics), Lagrangian for cost-critical (web agents).

### vs Reward Shaping

**Reward Shaping**:
```
R' = R - α₁c₁ - α₂c₂ - α₃c₃
```
- Fixed penalties αᵢ
- No guarantee of budget satisfaction
- Sensitive to hyperparameter choice

**Lagrangian**:
```
L = R - λ₁(c₁ - b₁) - λ₂(c₂ - b₂) - λ₃(c₃ - b₃)
```
- Adaptive penalties λᵢ
- Converges to budget satisfaction
- Only requires specifying budgets bᵢ

**Advantage**: Lagrangian separates "what constraint" (budgets) from "how to enforce" (multipliers learn automatically).

## Performance Results

### Web Agent Experiments

From [[web-agent-rl-constraints-paper|original research]]:

**Constraint Satisfaction Rate**:
- Request constraint: 98.7% episodes within budget
- Latency constraint: 96.3% episodes within budget
- Failure constraint: 94.8% episodes within budget

**Task Performance**:
- Unconstrained: 72.3% completion, violates budgets
- Lagrangian constrained: 80.5% completion, satisfies budgets

**Key Finding**: Lagrangian approach achieves HIGHER completion rate while satisfying constraints, not just trading off success for constraint satisfaction.

### Convergence Speed

**Training Episodes to Constraint Satisfaction**:
- All multipliers stabilize by episode 500-700
- Constraint satisfaction >90% by episode 300
- Near-optimal performance by episode 800

**Comparison**:
- Reward shaping: Never fully satisfies constraints (best effort)
- CPO: Satisfies constraints earlier but lower final performance
- Lagrangian: Balance of speed and final quality

## Implementation Details

### Hyperparameter Tuning

**Critical Parameters**:

**1. Dual Learning Rate (α_λ)**:
- Typical: α_λ = 10 × α_θ
- If too low: Slow constraint satisfaction
- If too high: Oscillating multipliers

**2. Constraint Smoothing**:
```python
violation_smoothed = β * violation_current + (1-β) * violation_previous
λ += α_λ * violation_smoothed
```
- β = 0.9-0.99 (exponential moving average)
- Reduces multiplier oscillation

**3. Initial Multipliers**:
- Start λᵢ = 0 (unconstrained warmup)
- Or λᵢ = small positive (e.g., 0.01) to hint at constraints

**4. Multiplier Clipping**:
```python
λ_max = 1.0 or 10.0
λ = clip(λ, 0, λ_max)
```
- Prevents exploding multipliers on infeasible problems

### Debugging Common Issues

**Multipliers Growing Unbounded**:
- **Cause**: Constraints infeasible (budgets too tight)
- **Fix**: Relax budgets or check constraint definitions

**Constraint Not Satisfied**:
- **Cause**: α_λ too low, insufficient penalty
- **Fix**: Increase dual learning rate

**Policy Collapse (Near-Zero Reward)**:
- **Cause**: λᵢ too high, constraint penalties dominate
- **Fix**: Decrease α_λ, add multiplier clipping

**Oscillating Performance**:
- **Cause**: α_λ too high, overly reactive
- **Fix**: Decrease α_λ, add constraint smoothing

### Integration with Actor-Critic

**Modified Critic Loss**:
```python
Q_target = reward - λ1*c1 - λ2*c2 - λ3*c3 + γ * V(s_next)
Q_loss = (Q_pred - Q_target)^2
```

**Modified Actor Loss**:
```python
advantage = Q(s, a) - V(s)
actor_loss = -log π(a|s) * advantage
```

**Cost-Adjusted Advantage**:
```python
advantage = (reward - λ1*c1 - λ2*c2 - λ3*c3) + γ*V(s_next) - V(s)
```

Value function learns to predict penalized reward, actor maximizes penalized return.

## Extensions

### Adaptive Lagrangian Methods

**Standard Update**:
```
λ ← max(0, λ + α_λ · violation)
```

**Adaptive Dual Averaging**:
```
g = violation
m ← β1*m + (1-β1)*g
v ← β2*v + (1-β2)*g^2
λ ← max(0, λ + α_λ * m / sqrt(v + ε))
```
- Adam-style adaptive learning for multipliers
- More stable convergence

### Augmented Lagrangian

**Standard Lagrangian**:
```
L = R - λ·(C - b)
```

**Augmented Lagrangian**:
```
L = R - λ·(C - b) - ρ/2 * max(0, C - b)^2
```
- Adds quadratic penalty term
- Faster convergence near constraint boundary
- Requires tuning penalty coefficient ρ

### Primal-Dual Policy Gradient

**Simultaneous Updates**:
```python
# Primal: Policy gradient w.r.t. Lagrangian
θ_grad = ∇θ E[R - Σ λᵢ·Cᵢ]
θ ← θ + α_θ · θ_grad

# Dual: Constraint violation gradient
λ_grad = E[Cᵢ - bᵢ]
λ ← max(0, λ + α_λ · λ_grad)
```

Both updates use same trajectory samples, improves sample efficiency.

## Related Pages

### Core Concepts

- [[constrained-mdp-web-agents|Constrained MDP for Web Agents]]
- [[multi-cost-rl|Multi-Cost Reinforcement Learning]]
- [[web-agent-cost-modeling|Web Agent Cost Modeling]]

### Related Techniques

- [[cvar-web-agent-risk|CVaR for Web Agent Risk Control]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[actor-critic-architecture|Actor-Critic Architecture]]

### Comparisons

- [[constrained-rl-vs-unconstrained-rl|Constrained RL vs Unconstrained RL]]
- [[webdancer-dapo-vs-constrained-dapo|WebDancer's DAPO vs Constrained DAPO]]

### Source

- [[web-agent-rl-constraints-paper|Web Agent RL Constraints Paper]]

### Technologies

- [[webdancer|WebDancer]] (could apply Lagrangian constraints to DAPO)
- [[dapo|DAPO Algorithm]] (unconstrained optimization baseline)
