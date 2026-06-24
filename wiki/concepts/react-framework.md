---
title: ReAct Framework
tags: [concept, agent-architecture, reasoning, action-execution]
created: 2026-06-24
updated: 2026-06-24
---

# ReAct Framework

A paradigm for structuring autonomous agent behavior that interleaves reasoning (Thought) and acting (Action-Observation) in iterative cycles. ReAct enables language models to generate explicit reasoning traces while grounding their actions in environmental feedback, creating transparent and effective agent execution patterns.

## Overview

ReAct (Reasoning + Acting) provides a structured execution pattern where agents alternate between generating reasoning thoughts about their current state and goal, taking actions in an environment, and receiving observations from those actions. This explicit interleaving of reasoning and acting improves both interpretability and task performance compared to pure action sequences or pure reasoning chains.

## How It Works

### Core Pattern

**Execution Cycle**:
```
Thought: [Agent's internal reasoning about situation and next step]
Action: [Concrete action to take in environment]
Observation: [Feedback from environment after action]
[Repeat until task completion]
```

**Example** (from [[webdancer|WebDancer]] information seeking):
```
Thought: I need to find information about the capital of France.
Action: search("capital of France")
Observation: Search results show Paris is the capital of France...

Thought: The search results clearly indicate Paris. I have enough information to answer.
Action: answer("Paris is the capital of France")
Observation: [Task complete]
```

### Three Components

**1. Thought Generation**:
- Agent reflects on current state
- Analyzes previous observations
- Plans next action
- Explicit reasoning trace
- Natural language expression

**Purpose**:
- Guides action selection
- Provides transparency
- Enables debugging
- Improves decision quality

**2. Action Execution**:
- Concrete operation in environment
- Grounded in specific action space
- Structured format (function call-like)
- Deterministic mapping to environment

**Common Action Types**:
- **Search**: Query information sources
- **Navigate**: Move through environment
- **Manipulate**: Change environment state
- **Communicate**: Exchange information
- **Terminate**: Provide final answer

**3. Observation Receipt**:
- Environment feedback
- Results of action
- New information state
- Grounds next reasoning step

**Forms**:
- Search results
- Page content
- API responses
- Success/failure signals
- Environmental state

### Termination

**End Condition**: Special action indicates completion
- `answer(text)`: Provide final response
- `submit(result)`: Submit solution
- `finish()`: Explicit termination
- Maximum step limit reached

## Applications and Examples

### WebDancer: Information-Seeking Agent

[[webdancer|WebDancer]] uses ReAct for autonomous research:

**Action Space**:
- `search(query)`: Search web for information
- `visit(url)`: Navigate to specific webpage
- `answer(text)`: Provide final answer

**Example Trajectory**:
```
Question: What year did the creator of Python start developing the language?

Thought: I need to find who created Python first.
Action: search("Python programming language creator")
Observation: Guido van Rossum created Python...

Thought: Now I need to find when he started developing it.
Action: search("Guido van Rossum Python development start year")
Observation: Development began in late 1989...

Thought: I have the answer - late 1989.
Action: answer("Guido van Rossum started developing Python in late 1989")
```

**Training**: [[two-stage-agent-training|Two-stage SFT + RL]] learns ReAct behavior
- SFT on 14,228 ReAct trajectories
- RL optimizes thought length and exploration
- Format reward ensures ReAct compliance

### AutoData: Multi-Agent System

[[autodata|AutoData]] uses ReAct across 8 specialized agents:

**Per-Agent ReAct**:
- Each agent follows Thought-Action-Observation pattern
- Actions specific to agent role (research vs. development)
- Coordinator orchestrates agent interactions
- OHCache optimizes multi-agent ReAct

**Example** (Research Squad - Goal Refinement Agent):
```
Thought: Need to clarify user's data extraction requirements
Action: analyze_instruction("extract product information from e-commerce site")
Observation: Ambiguous - need to specify fields (price, title, rating...)

Thought: Should generate refined goal specification
Action: refine_goal("Specify product fields: title, price, rating, availability")
Observation: Goal refined and passed to next agent
```

**Benefits**:
- Clear agent reasoning visible
- Debugging multi-agent coordination easier
- Action transparency for each role
- Structured agent communication

### Constrained Web Agents

[[web-agent-rl-constraints-paper|Web agents with safety constraints]]:

**Extended ReAct**:
- Standard Thought-Action-Observation
- Additional cost tracking for constraint violations
- Safety-aware action selection
- Multi-cost RL optimization

**Example**:
```
Thought: Need product data but site may have robots.txt restrictions
Action: check_robots("example.com")
Observation: GPTBot blocked by robots.txt

Thought: Crawling would violate robots.txt, switch to API
Action: api_call("example.com/api/products")
Observation: API returns product data (no constraint violation)
```

### Tool-Using Agents

**General Pattern**:
- Thought: Decide which tool to use and why
- Action: Call tool with parameters
- Observation: Tool output
- Repeat until task complete

**Examples**:
- Calculator use for math problems
- Code execution for programming tasks
- Database queries for information retrieval
- API calls for external services

## Benefits and Advantages

### Transparency and Interpretability

**Explicit Reasoning**:
- Thoughts reveal agent's decision process
- Easy to identify reasoning errors
- Understand why actions taken
- Debug failure modes

**Human Oversight**:
- Monitor agent behavior in real-time
- Intervene if reasoning goes wrong
- Assess trustworthiness
- Verify correctness

### Grounded Execution

**Environment Feedback**:
- Actions grounded in observations
- Reality check on reasoning
- Prevents hallucination
- Adaptive behavior

**Error Recovery**:
- Failed actions provide feedback
- Agent can adjust strategy
- Observation-driven replanning
- Robust to mistakes

### Structured Learning

**Training Benefits**:
- Clear supervision signal (format compliance)
- Reward shaping on thoughts vs. actions
- Easier to imitate for SFT
- RL can optimize reasoning depth

**[[webdancer|WebDancer]] Training**:
- Binary format reward: +1 if follows ReAct
- Answer correctness reward: LLM-as-Judge
- Learn appropriate thought length through RL
- Discover better exploration via RL

### Modularity and Extensibility

**Flexible Action Space**:
- Easy to add new actions
- Domain-specific operations
- Tool integration straightforward
- Compositional design

**Agent Specialization**:
- Different agents, same ReAct pattern
- [[autodata|AutoData]]: 8 agents, all ReAct
- Role-specific actions
- Uniform execution model

## Technical Details

### Implementation Pattern

**Basic Loop**:
```python
context = initial_state
done = False

while not done:
    # Generate thought
    thought = agent.generate_thought(context)
    
    # Select and execute action
    action = agent.select_action(thought, context)
    observation = environment.execute(action)
    
    # Update context
    context.append(thought, action, observation)
    
    # Check termination
    if action.is_terminal():
        done = True
    if len(context) > max_steps:
        done = True  # Timeout
```

**Prompt Structure** (for LLM-based agents):
```
System: You are an agent that solves tasks by reasoning and acting.
Use the following format:
Thought: [your reasoning]
Action: [action_name](arguments)

Available actions: search(query), visit(url), answer(text)

Task: [task description]

[Previous Thought-Action-Observation history]

Generate next Thought and Action:
```

### Action Space Design

**Function-Like Syntax**:
- `action_name(arg1, arg2, ...)`
- Clear parameter specification
- Easy to parse
- Familiar to developers

**Validation**:
- Check action name in allowed set
- Validate argument types and counts
- Ensure well-formed syntax
- Reject malformed actions

**Execution**:
- Map action string to environment function
- Pass arguments
- Capture output as observation
- Handle errors gracefully

### Thought Generation Strategies

**Short-CoT** (Concise reasoning):
- Brief, task-focused thoughts
- Efficient execution
- Average 510 tokens per trajectory (WebDancer)
- Better for instruction-following models

**Long-CoT** (Extended reasoning):
- Detailed analytical thinking
- Exploratory reasoning
- Average 1599 tokens per trajectory (WebDancer)
- From reasoning models (QwQ-Plus)
- Difficulty transferring to other models

**Optimization via RL**:
- RL increases thought length naturally
- Finds optimal reasoning depth
- Task-adaptive verbosity
- [[webdancer|WebDancer]]: Longer thoughts after RL

### Multi-Step Planning

**Iterative Refinement**:
- Each cycle builds on previous
- Observations inform next thoughts
- Adaptive strategy
- Error correction through feedback

**Credit Assignment**:
- Sparse rewards at trajectory end
- Must assign credit across many steps
- ReAct structure helps with interpretability
- RL learns which thought-action patterns succeed

## Comparison with Alternatives

### Chain-of-Thought (CoT) Only

**CoT**: Pure reasoning without actions
- Single reasoning chain to answer
- No environmental interaction
- Limited to model's knowledge
- Cannot gather external information

**ReAct Advantages**:
- Grounds reasoning in real observations
- Can search for information
- Adaptive to environment
- More capable for complex tasks

**When CoT Sufficient**:
- Task solvable from model knowledge
- No need for external information
- Simpler implementation
- Faster execution

### Action-Only (No Explicit Reasoning)

**Pure Action Sequences**: No thought generation
- More concise trajectories
- Faster execution
- Less interpretable
- Harder to debug

**ReAct Advantages**:
- Transparency for users
- Debugging capability
- Better performance (thoughts guide actions)
- Training signal for reasoning quality

**Empirical**: WebDancer format reward encourages ReAct compliance

### Plan-Then-Execute

**Alternative**: Generate complete plan, then execute
1. Create full action sequence upfront
2. Execute actions without replanning
3. No adaptation to observations

**ReAct Advantages**:
- Adapts to unexpected observations
- Can recover from errors
- More robust to environment changes
- Suitable for uncertain environments

**Plan-Then-Execute Advantages**:
- More efficient if environment predictable
- Complete plan visibility upfront
- Easier optimization
- Good for static, fully observable tasks

## Challenges and Limitations

### Verbosity and Efficiency

**Problem**: Thoughts add token cost
- Slower execution
- More expensive (LLM inference)
- Longer trajectories
- Storage overhead

**Mitigation**:
- Short-CoT over Long-CoT
- RL learns appropriate verbosity
- Terminate early when confident
- Compress thought representation

### Thought Quality

**Problem**: Thoughts may be unreliable
- Agent may generate incorrect reasoning
- Thoughts might not reflect true decision process
- Post-hoc rationalization
- Hallucination in thoughts

**Mitigation**:
- Focus on actions and observations (ground truth)
- Use thoughts as soft guidance, not guarantees
- Validate reasoning through outcomes
- Training to improve thought quality

### Action Space Limitations

**Problem**: Constrained by predefined actions
- Cannot take actions outside action space
- Creativity limited by available operations
- Domain-specific design needed
- Maintenance overhead for action set

**Mitigation**:
- Rich, flexible action set
- Compositional actions
- Meta-actions (e.g., code execution)
- Regular action space updates

### Format Compliance

**Problem**: LLMs may not follow ReAct format
- Malformed actions
- Missing thoughts
- Wrong argument structure
- Breaks execution loop

**Mitigation**:
- Strong prompting and examples
- Format reward during RL training (WebDancer)
- Validation and error handling
- Rejection sampling during SFT

## Future Directions

### Enhanced Reasoning

**Multi-Perspective Thoughts**:
- Consider multiple reasoning angles
- Parallel thought generation
- Ensemble reasoning
- Richer decision-making

**Hierarchical ReAct**:
- High-level strategic thoughts
- Low-level tactical thoughts
- Multi-level action abstraction
- Nested ReAct loops

**Meta-Reasoning**:
- Reflect on reasoning quality
- Adjust reasoning depth dynamically
- Self-correction mechanisms
- Confidence estimation

### Advanced Action Spaces

**Compositional Actions**:
- Combine primitive actions
- Learned action sequences
- Skills and macros
- Hierarchical action structure

**Tool Learning**:
- Discover new tools dynamically
- Learn tool usage from documentation
- Adaptive tool selection
- Open-ended action space

**Code Execution**:
- Generate code as actions
- Execute for flexible operations
- Program synthesis
- Maximum flexibility

### Multimodal ReAct

**Visual Observations**:
- Images, videos as observations
- Vision-language integration
- GUI navigation
- Embodied agents

**Multimodal Actions**:
- Generate images
- Produce audio
- Physical manipulations
- Rich action space

### Collaborative ReAct

**Multi-Agent Coordination**:
- Shared observation space
- Agent communication as actions
- Distributed reasoning
- [[autodata|AutoData]]-style systems

**Human-Agent Collaboration**:
- Human as special agent
- Thoughts include human input
- Collaborative reasoning
- Interactive systems

## Why It Matters

1. **Transparency Imperative**: Explicit reasoning traces make AI agent decisions interpretable and debuggable, critical for trust and safety.

2. **Grounded Execution**: Interleaving actions and observations prevents hallucination and enables adaptive behavior based on environmental feedback.

3. **Training Efficiency**: ReAct structure provides clear supervision signal for [[two-stage-agent-training|SFT]] and format rewards for RL optimization.

4. **Empirical Success**: [[webdancer|WebDancer]] achieves 51.5% on GAIA using ReAct, demonstrating effectiveness for complex reasoning tasks.

5. **Broad Applicability**: Pattern extends across information-seeking ([[webdancer|WebDancer]]), multi-agent systems ([[autodata|AutoData]]), constrained agents, and tool use.

6. **Standardization**: ReAct emerging as de facto standard for agentic LLM execution, enabling ecosystem development and interoperability.

## Papers That Use This Concept

### Primary Sources

**[[webdancer-paper|"WebDancer: Towards Autonomous Information Seeking Agency"]]** (Wu et al., NeurIPS 2025):
- Implements ReAct for information seeking
- Search-Visit-Answer action space
- Binary format reward for ReAct compliance
- 14,228 ReAct trajectories for SFT
- RL optimizes thought length and exploration

**[[autodata-paper|"AutoData: Multi-Agent Automated Data Collection"]]**:
- ReAct across 8 specialized agents
- Per-agent Thought-Action-Observation
- Multi-agent ReAct coordination
- OHCache optimizes ReAct efficiency

**[[web-agent-rl-constraints-paper|"Reinforcement Learning for Web Agents Under Safety Constraints"]]**:
- ReAct with constraint awareness
- Safety-aware thought generation
- Cost-conscious action selection
- Extends WebDancer ReAct pattern

### Original ReAct Paper

**Yao et al. (2022)**: "ReAct: Synergizing Reasoning and Acting in Language Models"
- Introduces ReAct paradigm
- Demonstrates benefits over CoT and action-only
- HotpotQA and other benchmarks
- Foundation for modern agentic systems

### Related Work

**Tool-using agents**: Apply ReAct for tool selection and execution

**Game-playing agents**: ReAct for strategy and decision-making

**Dialogue agents**: Thought-response patterns in conversation

**Planning systems**: ReAct for hierarchical planning

## Related Concepts

- [[information-seeking-agents|Information-Seeking Agents]] - Primary application domain
- [[two-stage-agent-training|Two-Stage Agent Training]] - Training paradigm for ReAct agents
- [[agentic-rl|Agentic Reinforcement Learning]] - RL optimization of ReAct execution
- [[dapo|DAPO]] - RL algorithm optimizing ReAct trajectories
- [[short-cot|Short Chain-of-Thought]] - Concise thought generation
- [[long-cot|Long Chain-of-Thought]] - Extended reasoning in thoughts
- [[llm-as-judge|LLM-as-Judge]] - Evaluation of ReAct trajectories

## Related Pages

### Concepts
- [[information-seeking-agents|Information-Seeking Agents]]
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[agentic-rl|Agentic Reinforcement Learning]]
- [[dapo|DAPO]]

### Technologies
- [[webdancer|WebDancer]]
- [[autodata|AutoData]]
- [[crawlqa|CRAWLQA]]
- [[e2hqa|E2HQA]]

### Sources
- [[webdancer-paper|WebDancer Paper]]
- [[autodata-paper|AutoData Paper]]
- [[web-agent-rl-constraints-paper|Web Agent RL Constraints Paper]]

### Entities
- [[jialong-wu|Jialong Wu]]
- [[baixuan-li|Baixuan Li]]
- [[tongyi-lab|Tongyi Lab]]

### Comparisons
- [[webdancer-vs-autodata|WebDancer vs AutoData]]
- [[short-cot-vs-long-cot|Short-CoT vs Long-CoT]]
