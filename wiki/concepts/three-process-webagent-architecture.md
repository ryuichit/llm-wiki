# Three-Process WebAgent Architecture

**Category**: Architecture Pattern  
**Domain**: Web Agents, Autonomous Systems  
**Source**: [[webagents-survey-paper|WebAgents Survey (2025)]]

## Overview

The Three-Process WebAgent Architecture is the fundamental design pattern that organizes all LFM-powered web agents into three consecutive and critical processes: **Perception**, **Planning & Reasoning**, and **Execution**. This architectural framework, systematized in the WebAgents survey (2025), provides a unified lens for understanding how autonomous agents interact with web environments to complete user-defined tasks.

**Core Principle**: WebAgents must first accurately observe the environment (Perception), then analyze and determine appropriate actions (Planning & Reasoning), and finally interact with the environment to execute those actions (Execution).

## The Three Processes

### 1. Perception

**Purpose**: Accurately observe the current web environment

**Challenge**: Web environments present information in multiple modalities (HTML structure, visual rendering, JavaScript interactions) that must be comprehended for effective navigation.

**Approaches**:

#### Text-based Perception
- **Data Source**: HTML, accessibility trees, DOM elements
- **Advantages**: Direct access to structured content, explicit semantic information
- **Limitations**: Verbose, poor generalization across different sites, doesn't align with visual GUI
- **Example Systems**: [[mindact|MindAct]], [[ash|ASH]], [[rci|RCI]], [[synapse|Synapse]]

#### Screenshot-based Perception
- **Data Source**: Visual screenshots of rendered webpages
- **Advantages**: Aligns with human cognitive processes, better generalization
- **Foundation**: Large Vision-Language Models (VLMs)
- **Limitations**: Requires sophisticated visual understanding, coordinate-based interaction
- **Example Systems**: [[seeclick|SeeClick]], [[omniparser|OmniParser]], [[showui|ShowUI]], [[falcon-ui|Falcon-UI]]

#### Multi-modal Perception
- **Data Source**: Combined text + visual information
- **Advantages**: Comprehensive environmental understanding leveraging complementary strengths
- **Approach**: Integrate screenshots with accessibility trees or HTML metadata
- **Example Systems**: [[webvoyager|WebVoyager]], [[mmac-copilot|MMAC-Copilot]], [[autowebglm|AutoWebGLM]], [[oscar|OSCAR]]

**Key Insight**: Multi-modal perception generally outperforms single-modality approaches by combining structural (text) and visual (screenshot) information.

### 2. Planning & Reasoning

**Purpose**: Analyze the environment, interpret user tasks, predict appropriate next actions

**Challenge**: Complex tasks require decomposition, sequential reasoning, and strategic decision-making under uncertainty.

**Sub-processes**:

#### Task Planning
Determine the sequence of steps to complete user-defined tasks.

**Explicit Planning**:
- Decompose instructions into sub-tasks
- Generate step-by-step plans before execution
- Examples: [[screenagent|ScreenAgent]] (structured workflow + reflection), [[os-copilot|OS-Copilot]] (DAG for parallel execution)
- Advantage: Interpretable, can leverage hierarchical structure

**Implicit Planning**:
- Direct action generation without explicit decomposition
- Examples: [[webwise|WebWISE]], [[openwebagent|OpenWebAgent]]
- Advantage: Simpler, fewer failure points in planning stage

#### Action Reasoning
Generate the specific next action based on current state.

**Reactive Reasoning**:
- Direct action from observation + instruction
- Simple input-output mapping
- Examples: [[agent-e|Agent-E]], [[ash|ASH]]
- Advantage: Fast, low overhead

**Strategic Reasoning**:
- Additional operations to enhance reasoning quality
- **Exploration**: Simulate action outcomes before execution ([[webdreamer|WebDreamer]])
- **In-context Enhancement**: Retrieve similar examples or intents ([[auto-intent|Auto-intent]])
- Examples: [[showui|ShowUI]], [[oscar|OSCAR]], [[os-copilot|OS-Copilot]]
- Advantage: Higher accuracy, fewer errors

#### Memory Utilization
Leverage internal/external information to improve action prediction.

**Short-term Memory**:
- Previous actions in current task
- Avoid redundant operations
- Examples: [[autowebglm|AutoWebGLM]], [[llmpa|LLMPA]], [[clickagent|ClickAgent]]

**Long-term Memory**:
- External knowledge: web search, action trajectories from past tasks
- Enable few-shot learning and generalization
- Examples: [[agent-s|Agent S]] (online search + narrative memory), [[synapse|Synapse]] (Trajectory-as-Exemplar), [[awm|AWM]] (workflow memory)

**Key Insight**: Strategic reasoning with both short-term and long-term memory dramatically outperforms reactive approaches without memory.

### 3. Execution

**Purpose**: Interact with webpages and execute generated actions

**Challenge**: Locate correct interactive elements among many on complex webpages, then perform precise actions.

**Sub-processes**:

#### Grounding
Locate the specific element to interact with.

**Direct Grounding**:
- Agent directly generates coordinates or selects HTML elements
- Examples: [[showui|ShowUI]] (coordinate prediction), [[auto-intent|Auto-intent]] (HTML element selection)
- Advantage: End-to-end learning, no auxiliary modules
- Challenge: Difficult with many elements, requires precise training

**Inferential Grounding**:
- Auxiliary modules to locate target elements
- **Two-stage**: High-level description → precise localization
- Examples: [[ponder-and-press|Ponder & Press]] (interpreter MLLM + locator MLLM), [[oscar|OSCAR]] (visual + semantic dual-grounding), [[mindact|MindAct]] (small LM ranking → LLM selection)
- Advantage: More accurate in complex scenarios, separation of concerns

#### Interacting
Execute the action on the located element.

**Web Browsing-based**:
- Human-like actions: Click, Type, Scroll, Select
- Standard action space across most agents
- Examples: [[naviqate|NaviQAte]] ([Click], [Type], [Select]), [[falcon-ui|Falcon-UI]], [[agentoccam|AgentOccam]] (simplified action space)
- Advantage: Natural, mimics human behavior, broadly applicable

**Tool-based**:
- Direct API calls, external tools
- Bypass GUI entirely for some operations
- Examples: [[api-calling-agent|API-calling agent]] (API interactions), [[infogent|Infogent]] (Google Search API + scraper)
- Advantage: More efficient, can access functionality not exposed in GUI
- Challenge: Requires API availability and documentation

**Key Insight**: Inferential grounding with auxiliary modules is more robust for complex webpages, while direct grounding suffices for simpler scenarios.

## Mathematical Formulation

At timestep t, the agent operates as:

**Perception**: Observe current state
```
s_t = Perceive(Webpage, Modality)
```

**Planning & Reasoning**: Generate next action
```
a_t = f_Θ(T, s_t, {a_1, a_2, ..., a_{t-1}}, Memory)
```
Where:
- T: User instruction
- s_t: Current observation
- {a_1, ..., a_{t-1}}: Previous actions (short-term memory)
- Memory: Long-term memory (trajectories, web search results)
- f_Θ: LFM-empowered agent with parameters Θ

**Execution**: Interact with environment
```
s_{t+1} = Environment(a_t)
```

**Iterative Loop**: Repeat until task completion

## Design Considerations

### Perception

**Text vs. Visual Trade-off**:
- Text: Explicit structure, verbose, poor generalization
- Visual: Aligns with human cognition, requires VLM, coordinate-based interaction
- Multi-modal: Best performance but higher complexity

**Input Representation**:
- HTML: Full structure but overwhelming for LFMs
- Accessibility Tree: Simplified but may miss visual context
- Screenshot: Rich but requires grounding mechanism
- Hybrid: Accessibility tree + screenshot annotations

### Planning & Reasoning

**Explicit vs. Implicit Planning**:
- Explicit: Interpretable, can handle complex multi-step tasks, but additional failure point
- Implicit: Simpler, relies on LFM's implicit reasoning, less controllable

**Reactive vs. Strategic**:
- Reactive: Fast, low overhead, suitable for simple tasks
- Strategic: Higher accuracy, necessary for complex exploration, but higher computational cost

**Memory Management**:
- Short-term: Sliding window of recent actions (avoid context overflow)
- Long-term: Retrieval-based (RAG) or episodic memory (store full trajectories)

### Execution

**Grounding Precision**:
- Direct: Requires high-quality training data with precise annotations
- Inferential: More robust but adds latency and complexity

**Action Space Design**:
- Minimal: [Click], [Type], [Scroll] (simple, easy to learn)
- Rich: Add [Select], [Note], [Stop], [Hover] (more expressive)
- Tool-augmented: Include API calls, search, code execution (most powerful)

## Implementation Patterns

### Pattern 1: Text-only Simple Agent
```
Perception: HTML → Filtered DOM elements
Planning: Reactive reasoning with short-term memory
Execution: Direct grounding on HTML element IDs
Example: WebWISE, early RCI
```

### Pattern 2: Vision-Language Agent
```
Perception: Screenshot → VLM encoding
Planning: Strategic reasoning with exploration simulation
Execution: Inferential grounding (description → coordinates)
Example: WebVoyager, ShowUI, Ponder & Press
```

### Pattern 3: Multi-modal Strategic Agent
```
Perception: Screenshot + Accessibility tree → Unified representation
Planning: Explicit planning (task decomposition) + Long-term memory retrieval
Execution: Dual grounding (visual + semantic) → Web browsing + Tools
Example: OSCAR, Agent S, AutoWebGLM
```

### Pattern 4: Training-free Prompting Agent
```
Perception: Depends on foundation model (GPT-4V → screenshot, GPT-4 → HTML)
Planning: Chain-of-thought / Chain-of-action prompting
Execution: Direct grounding via prompting
Example: Auto-GUI, CoAT
```

## Performance Considerations

### Perception
- **Text-based**: Fast (direct parsing), limited by HTML verbosity
- **Screenshot-based**: Slower (VLM inference), limited by image resolution
- **Multi-modal**: Slowest (both modalities) but highest quality

### Planning & Reasoning
- **Implicit + Reactive**: Fastest, single LFM call
- **Explicit + Strategic**: Slowest, multiple LFM calls for planning + reasoning + exploration

### Execution
- **Direct Grounding**: Fast, single-step localization
- **Inferential Grounding**: Slower, multi-step (description generation → localization)

**Trade-off**: Accuracy vs. speed. Strategic + multi-modal + inferential approaches achieve highest success rates but 3-5x slower than reactive + text + direct.

## Relationship to Other Concepts

### Complementary Concepts
- [[lfm-empowered-agents|LFM-empowered Agents]]: Underlying technology powering all three processes
- [[vision-language-models|Vision-Language Models]]: Enable screenshot-based perception
- [[set-of-mark-prompting|Set-of-Mark Prompting]]: Technique for grounding in execution
- [[trajectory-as-exemplar|Trajectory-as-Exemplar]]: Memory utilization strategy in planning

### Related Architectures
- [[information-seeking-agents|Information-Seeking Agents]]: Specialized instantiation (e.g., WebDancer)
- [[multi-agent-systems|Multi-agent Systems]]: Can be organized as specialized perception/planning/execution agents (e.g., AutoData)
- [[rl-based-agents|RL-based Agents]]: Alternative to LFM-based, still follows three-process structure

### Trustworthiness Concerns
- **Perception**: Adversarial HTML, misleading visual elements
- **Planning**: Malicious instructions embedded in webpages, hallucination
- **Execution**: Privacy leakage through actions, unintended consequences

## Empirical Findings (from Survey)

### Perception Modality Performance
- **Text-based**: Sufficient for simple, structured websites
- **Screenshot-based**: Better generalization across visual layouts
- **Multi-modal**: Consistently outperforms single modality (e.g., AutoWebGLM, WebVoyager top performers)

### Planning Strategy Impact
- **Explicit planning**: Improves performance on multi-step tasks (e.g., ScreenAgent, OS-Copilot)
- **Strategic reasoning**: 5-15% higher success rate vs. reactive (WebDreamer vs. baseline)
- **Memory utilization**: Long-term memory provides 10-20% improvement (Agent S, Synapse)

### Execution Grounding
- **Inferential grounding**: More robust on complex pages with many elements
- **Direct grounding**: Competitive when trained with high-quality data (ShowUI)

## Limitations

1. **Sequential Bottleneck**: Three processes must execute sequentially; errors propagate
2. **Perception Overhead**: Multi-modal perception requires significant computation (VLM + HTML parsing)
3. **Planning Failures**: Explicit planning can fail if task decomposition is incorrect
4. **Grounding Errors**: Incorrect element localization leads to wrong actions (no recovery)
5. **Context Length**: Long HTML or many previous actions exceed LFM context windows
6. **Latency**: Full pipeline (multi-modal perception + strategic reasoning + inferential grounding) can take 10-30 seconds per action

## Future Directions

1. **Parallel Processing**: Overlap perception and planning stages
2. **Hierarchical Execution**: Multi-level agents (high-level planner → low-level executors)
3. **Error Recovery**: Mechanisms to detect and recover from perception/grounding errors
4. **Adaptive Strategies**: Switch between reactive/strategic based on task complexity
5. **Efficient Perception**: Compressed representations reducing context overhead
6. **Continuous Learning**: Update memory and models based on execution feedback

## Relationship to Wiki Papers

### WebCloak Defense
- **Target**: Perception process
- **Mechanism**: Dynamic Structural Obfuscation disrupts HTML parsing, Semantic Labyrinth confuses reasoning
- **Implication**: Defenders can attack each process independently

### AutoData Multi-agent
- **Architecture**: Specialized agents for different processes
  - Plan Agent / Web Agent: Planning & Reasoning
  - Tool Agent / Engineering Agent: Execution
  - Blueprint Agent / Validation Agent: Meta-planning and verification
- **Coordination**: OHCache for cross-process information sharing

### WebDancer
- **End-to-end Training**: All three processes jointly optimized via RL
- **Perception**: Multi-modal (text + potential visual in future)
- **Planning**: Implicit planning via DAPO algorithm
- **Execution**: Direct grounding + web browsing actions
- **Memory**: Synthetic data (CRAWLQA, E2HQA) as long-term memory

## Examples in Practice

### Example 1: Simple Form Filling
```
User: "Fill out the contact form with my email"

1. Perception (Text-based):
   - Parse HTML, extract <form> elements
   - Identify <input type="email"> field

2. Planning & Reasoning (Reactive):
   - Instruction: Fill email field
   - Action: Type "user@example.com" into email field

3. Execution (Direct Grounding):
   - Locate element by ID="email"
   - Execute: Type action

Result: Single-step, fast, reliable
```

### Example 2: Product Research
```
User: "Find the best-rated wireless headphones under $100"

1. Perception (Multi-modal):
   - Screenshot: Capture search results page
   - Accessibility tree: Extract product cards

2. Planning & Reasoning (Strategic + Long-term memory):
   - Explicit plan: [Search] → [Filter price] → [Sort by rating] → [Compare top 3]
   - Retrieve similar trajectories from memory
   - Strategic reasoning: Simulate clicking "Sort by rating" vs "Filter" first

3. Execution (Inferential Grounding + Tools):
   - Description: "Sort dropdown in upper right"
   - Grounding module locates dropdown at [x, y]
   - Execute: Click dropdown, then select "Rating: High to Low"

Iterate: Perception (see sorted results) → Planning (compare top 3) → Execution (click details)...

Result: Multi-step, slower but accurate
```

### Example 3: Calendar Scheduling (AutoData-style)
```
User: "Schedule a meeting with Leon at Starbucks on Nov 23, 4pm"

1. Multi-agent Decomposition:
   - Plan Agent: Decompose → [Open calendar] → [Find Nov 23] → [Create 4pm slot] → [Add Leon as attendee]
   
2. Specialized Execution:
   - Web Agent (Perception): Observe calendar app
   - Web Agent (Planning): Navigate to correct date
   - Tool Agent (Execution): Use calendar API to create event
   
3. Coordination (OHCache):
   - Share state across agents
   - Validation Agent checks: Event created? Correct time? Leon added?

Result: Parallel processing, robust error checking
```

## Summary

The Three-Process WebAgent Architecture is the foundational pattern unifying all web agents:

1. **Perception**: Observe environment (text, visual, or multi-modal)
2. **Planning & Reasoning**: Determine actions (explicit/implicit planning, reactive/strategic reasoning, memory utilization)
3. **Execution**: Interact with environment (grounding + action execution)

**Key Principle**: Multi-modal perception + strategic reasoning with memory + inferential grounding achieves highest performance but at cost of latency and complexity.

**Design Guideline**: Choose modality, planning strategy, and grounding approach based on task complexity, available compute, and required accuracy.

---

*Tags*: [architecture], [web-agents], [design-pattern], [perception], [planning], [reasoning], [execution], [survey-framework]
