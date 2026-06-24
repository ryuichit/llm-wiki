---
title: ReAct Paradigm (Reasoning + Acting)
tags: [concept, agent-architecture, llm, execution-model]
created: 2026-06-24
updated: 2026-06-24
---

# ReAct Paradigm (Reasoning + Acting)

An execution model for LLM-based agents that combines explicit reasoning with tool-based actions, enabling agents to think through problems before executing and learn from outcomes iteratively.

## Overview

ReAct (Reasoning + Acting) is the execution paradigm used by all agents in [[autodata|AutoData]]. It structures agent behavior into alternating cycles of reasoning (analyzing context and planning) and acting (executing tasks with tools), creating a transparent and effective decision-making process.

## Core Components

### Reasoning Step

**Purpose**: Analyze context and plan next action

**Activities**:
- Understand current state
- Analyze available information
- Consider possible actions
- Plan optimal next step
- Explain reasoning explicitly

**Example (Web Agent)**:
```
Reasoning: The user wants to collect stock prices from Yahoo Finance. 
I need to analyze the webpage structure to identify where price data 
is located. I'll use the web browser to load the page and examine 
the HTML structure.
```

### Acting Step

**Purpose**: Execute concrete actions with tools

**Activities**:
- Use specific tool (browser, API, code executor)
- Execute planned action
- Observe outcome
- Capture results for next reasoning step

**Example (Web Agent)**:
```
Acting: Load https://finance.yahoo.com/quote/AAPL
Tool: Web Browser
Action: Navigate and extract HTML
Result: [HTML structure captured]
```

### Iteration

**Process**:
1. Reason about current state
2. Act to change state or gather information
3. Observe results
4. Reason about new state
5. Repeat until task complete

**Benefits**:
- Adaptive behavior
- Error recovery
- Continuous refinement
- Learning from feedback

## Application in AutoData

### Plan Agent (PLN) Example

**Task**: Decompose "Collect NeurIPS 2024 paper titles and authors"

**Reasoning 1**: "This task requires identifying the NeurIPS 2024 accepted papers page, extracting paper titles and author names, and structuring the data."

**Acting 1**: Create subtasks list: [Find NeurIPS 2024 page, Extract titles, Extract authors, Structure data]

**Reasoning 2**: "Subtasks defined. Need to specify data format and validation criteria."

**Acting 2**: Define expected output schema: {title: string, authors: [string]}

### Web Agent (WEB) Example

**Task**: Analyze Yahoo Finance stock page structure

**Reasoning 1**: "Need to locate price, volume, and market cap elements. Will load page and analyze HTML."

**Acting 1**: Navigate to Yahoo Finance page, capture HTML

**Reasoning 2**: "Found price in `<fin-streamer>` tag with specific attributes. Market cap in table with class `quote-statistics`."

**Acting 2**: Document selector patterns and cache structure analysis

### Engineering Agent (ENG) Example

**Task**: Generate web scraper code

**Reasoning 1**: "Based on Web Agent's analysis, I need to create a Python scraper using BeautifulSoup. Will target identified selectors."

**Acting 1**: Generate code skeleton with imports and basic structure

**Reasoning 2**: "Code structure complete. Need to add error handling and data validation."

**Acting 2**: Add try-catch blocks and validation logic, store final code in cache

## Advantages

### Transparency

**Explicit Reasoning**:
- Decisions are explained, not black-box
- Easier to debug and understand
- Build trust in agent behavior
- Audit trail for failures

### Adaptability

**Iterative Refinement**:
- Agents adjust based on observations
- Recover from errors gracefully
- Handle unexpected situations
- Learn optimal strategies through experience

### Effectiveness

**Structured Problem-Solving**:
- Think before acting reduces errors
- Tool use is purposeful, not random
- Complex tasks broken into manageable steps
- Higher success rates than pure action or pure reasoning

## Comparison with Alternatives

### vs Pure Reasoning (Plan-Only)

**Pure Reasoning**:
- Generate complete plan upfront
- No execution, only abstract thinking
- Cannot adapt to actual outcomes

**ReAct**:
- Interleaves planning and execution
- Adapts plan based on real results
- Handles unexpected situations

### vs Pure Acting (Trial-and-Error)

**Pure Acting**:
- Execute actions without reasoning
- Random or heuristic-based
- Inefficient, many failed attempts

**ReAct**:
- Reasoned action selection
- Fewer wasted attempts
- Purposeful tool use

### vs Behavior Trees

**Behavior Trees**:
- Pre-defined decision structure
- Fixed logic
- Limited adaptability

**ReAct**:
- Dynamic reasoning at runtime
- Flexible decision-making
- Context-aware adaptation

## Implementation in Multi-Agent Systems

### Agent Coordination

**Independent ReAct**:
- Each agent runs its own ReAct loop
- Reasoning considers messages from other agents
- Actions may include sending messages to other agents

**Example**:
```
Plan Agent Reasoning: "Manager requested task decomposition. 
I'll analyze the instruction and create subtasks."

Plan Agent Acting: Create subtask list, send to Manager
```

### Collective Intelligence

**Emergent Behavior**:
- Individual agent ReAct loops
- Coordination through [[ohcache|OHCache]] messaging
- Collective reasoning from specialized perspectives
- Coordinated actions toward common goal

### Manager Role

**Orchestration ReAct**:
- Manager reasons about workflow state
- Acts by dispatching tasks to agents
- Observes agent outputs
- Reasons about next workflow step

## Related Pages

### Concepts

- [[autodata|AutoData]]
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]]
- [[ohcache|OHCache]]
- [[plan-agent|Plan Agent]]
- [[web-agent|Web Agent]]
- [[engineering-agent|Engineering Agent]]

### Sources

- [[autodata-paper|AutoData Paper]]

## Sources

- [[autodata-paper|AutoData Research Paper]] (NeurIPS 2025)
- ReAct: Synergizing Reasoning and Acting in Language Models (Yao et al., 2022)
