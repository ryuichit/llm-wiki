---
title: OHCache (Oriented Hypergraph Cache System)
tags: [concept, architecture, multi-agent-systems, communication, efficiency]
created: 2026-06-24
updated: 2026-06-24
---

# OHCache (Oriented Hypergraph Cache System)

A novel coordination mechanism for multi-agent systems that uses oriented hypergraphs for selective message routing and local caching for artifact management, developed as part of the [[autodata|AutoData]] system.

## Overview

OHCache addresses the fundamental challenge of efficient multi-agent communication in LLM-based systems: reducing token costs and information overload while maintaining effective coordination. It replaces traditional broadcast communication with selective, structured message routing through an oriented hypergraph architecture.

## Core Problem

### Traditional Multi-Agent Communication

**Broadcast Approach**:
- Every agent receives all messages
- O(n²) message complexity
- High token consumption
- Information overload
- Cognitive burden on agents

**LLM Cost Implications**:
- Each message incurs token costs
- Broadcast multiplies costs by number of agents
- Quickly becomes prohibitively expensive
- Limits scalability of multi-agent systems

**Example**:
```
8 agents × broadcast = 64 message deliveries per round
With 1000 tokens/message: 64,000 tokens/round
At $0.01/1K tokens: $0.64/round
```

### OHCache Solution

**Selective Routing**:
- Agents receive only relevant messages
- O(n) message complexity
- ~60% token reduction
- Targeted information delivery
- Focused agent attention

**Cost Improvement**:
```
8 agents × selective routing = ~15 message deliveries per round
With 1000 tokens/message: 15,000 tokens/round  
At $0.01/1K tokens: $0.15/round
~77% cost reduction
```

## Architecture Components

### 1. Oriented Message Hypergraph

[[oriented-message-hypergraph|Oriented Message Hypergraph]] provides the communication backbone.

**Hypergraph Structure**:
- **Nodes**: Individual agents (PLN, WEB, TOL, BLU, ENG, TST, VAL, MGR)
- **Hyperedges**: Communication channels connecting multiple agents
- **Orientation**: Directed message flow with explicit sender/receiver(s)
- **Selectivity**: Messages routed only to relevant agents

**Example Hyperedge**:
```
Hyperedge: "Research Planning"
  Source: Manager Agent (MGR)
  Targets: {Plan Agent (PLN), Web Agent (WEB)}
  Content: User instruction + task context
  Type: BROADCAST_TO_SUBSET
```

**vs Traditional Graph**:
- Traditional: Pairwise edges (agent A → agent B)
- Hypergraph: Multi-party edges (agent A → {B, C, D})
- More natural for group communication
- Reduces edge count, simplifies routing

**Workflow Example in AutoData**:
```
User Instruction
    ↓ (hyperedge 1)
Manager (MGR)
    ↓ (hyperedge 2: to Research Squad)
{PLN, WEB, TOL, BLU}
    ↓ (hyperedge 3: from Research Squad)
Manager (MGR)
    ↓ (hyperedge 4: to Development Squad)
{ENG, TST}
    ↓ (hyperedge 5: to Validation)
VAL
    ↓ (hyperedge 6: to Manager)
Manager (MGR)
    ↓ (hyperedge 7: to User)
Structured Output
```

### 2. Oriented Hyperedge Formatter

[[oriented-hyperedge-formatter|Oriented Hyperedge Formatter]] standardizes message structure.

**Message Format**:
```json
{
  "hyperedge_id": "unique_identifier",
  "source_agent": "MGR",
  "target_agents": ["PLN", "WEB", "TOL", "BLU"],
  "message_type": "TASK_ASSIGNMENT",
  "content": {
    "instruction": "Collect stock prices from Yahoo Finance",
    "context": {...},
    "requirements": {...}
  },
  "timestamp": "2026-06-24T10:30:00Z",
  "version": "1.0"
}
```

**Benefits**:
- **Clarity**: Explicit sender/receiver identification
- **Type safety**: Structured content with schemas
- **Versioning**: Backward compatibility support
- **Debugging**: Traceable message flow
- **Standardization**: Consistent protocol across agents

**Message Types**:
- `TASK_ASSIGNMENT`: Manager → Agents (start work)
- `RESULT_REPORT`: Agent → Manager (completion)
- `COLLABORATION_REQUEST`: Agent → Agent (peer help)
- `STATUS_UPDATE`: Agent → Manager (progress)
- `ERROR_NOTIFICATION`: Agent → Manager (failure)

### 3. Local Cache System

[[local-cache-system|Local Cache System]] manages artifacts and intermediate results.

**What's Cached**:
- **Code artifacts**: Generated scrapers, API scripts
- **Analysis results**: Webpage structure, API documentation
- **Test cases**: Unit tests, integration tests
- **Validation outputs**: Quality checks, error reports
- **Intermediate data**: Partial results, debugging info

**Cache Operations**:
```python
# Store artifact
cache.store(
    key="webpage_analysis_yahoo_finance",
    content=analysis_result,
    agent="WEB",
    task_id="task_123"
)

# Retrieve artifact
analysis = cache.retrieve(
    key="webpage_analysis_yahoo_finance",
    agent="ENG"  # Engineering Agent needs Web Agent's analysis
)

# List related artifacts
artifacts = cache.list_by_task("task_123")
```

**Benefits**:
- **Reuse**: Similar tasks leverage cached results
- **Persistence**: Artifacts survive across workflow stages
- **Sharing**: Agents access each other's outputs
- **Efficiency**: Avoid redundant computation
- **Debugging**: Historical record for troubleshooting

**Cache Policies**:
- **Expiration**: Time-based or task-based invalidation
- **Scope**: Task-specific or global caching
- **Sharing**: Per-agent private vs shared cache
- **Persistence**: In-memory vs disk-backed storage

## How OHCache Works in AutoData

### Stage 1: Research Squad

**Manager → Research Squad**:
```
Hyperedge: "Research Assignment"
  Source: MGR
  Targets: {PLN, WEB, TOL, BLU}
  Content: User instruction + domain context
```

**Research Squad Execution** (parallel):
- PLN: Analyzes instruction, caches task decomposition
- WEB: Analyzes webpages, caches structure information
- TOL: Searches APIs, caches tool documentation
- BLU: Synthesizes, caches blueprint

**Research Squad → Manager**:
```
Hyperedge: "Research Results"
  Sources: {PLN, WEB, TOL, BLU}
  Target: MGR
  Content: Individual agent outputs + references to cached artifacts
```

**Efficiency Gain**:
- Without OHCache: Each agent sends full output (4 × large messages)
- With OHCache: Agents send references, MGR retrieves from cache (4 × small messages)

### Stage 2: Development Squad

**Manager → Engineering Agent**:
```
Hyperedge: "Implementation Task"
  Source: MGR
  Targets: {ENG}
  Content: Blueprint reference + cache pointers to Research Squad artifacts
```

**Engineering Agent**:
- Retrieves blueprint from cache
- Retrieves webpage analysis from cache
- Retrieves tool documentation from cache
- Generates code, stores in cache

**Engineering → Test Agent**:
```
Hyperedge: "Testing Task"
  Source: MGR (or ENG directly)
  Target: TST
  Content: Code artifact reference
```

**Test Agent**:
- Retrieves code from cache
- Creates test cases, stores in cache

**Development → Validation**:
```
Hyperedge: "Validation Task"
  Source: MGR
  Target: VAL
  Content: Code + test references
```

**Validation Agent**:
- Retrieves code and tests from cache
- Runs validation, stores results in cache
- Reports to Manager

### Message Flow Comparison

**Without OHCache (Broadcast)**:
```
MGR → all 7 agents (7 messages)
PLN → all agents (7 messages)
WEB → all agents (7 messages)
TOL → all agents (7 messages)
BLU → all agents (7 messages)
ENG → all agents (7 messages)
TST → all agents (7 messages)
VAL → all agents (7 messages)
Total: 56 messages
```

**With OHCache (Selective)**:
```
MGR → {PLN, WEB, TOL, BLU} (4 messages)
{PLN, WEB, TOL, BLU} → MGR (4 messages)
MGR → ENG (1 message)
ENG → MGR (1 message)
MGR → TST (1 message)
TST → MGR (1 message)
MGR → VAL (1 message)
VAL → MGR (1 message)
Total: 14 messages
75% reduction
```

## Performance Impact

### Token Cost Reduction

**Measurements on [[instruct2ds|Instruct2DS]]**:
- **Without OHCache**: ~$2.20 average per task
- **With OHCache**: ~$0.57 average per task
- **Reduction**: 74% cost savings

**Breakdown**:
- Selective routing: ~40% reduction
- Cache reuse: ~20% reduction
- Message format efficiency: ~14% reduction

### Execution Time

**Impact on Speed**:
- Reduced message processing overhead
- Faster agent context loading
- Parallel execution enabled by clear dependencies
- Cache hits avoid redundant computation

**Results**:
- AutoData with OHCache: 5.58 minutes average
- Manus without OHCache: 14.8 minutes average
- 63% faster execution

### Scalability

**Agent Count Scaling**:
- Traditional broadcast: O(n²) messages
- OHCache selective: O(n) messages
- Enables larger multi-agent systems

**Task Complexity Scaling**:
- Cache reuse benefits increase with similar tasks
- Amortized costs decrease over time
- Better handling of multi-step workflows

## Design Principles

### 1. Selectivity Over Broadcasting

**Principle**: Send messages only to agents who need them

**Implementation**:
- Analyze workflow dependencies
- Identify relevant agents per message type
- Route through hyperedges with specific targets

**Benefit**: Reduced token costs, less information overload

### 2. Structure Over Ad-Hoc

**Principle**: Use formatted, typed messages rather than free-form

**Implementation**:
- Define message schemas
- Enforce formatting through Oriented Hyperedge Formatter
- Version control for protocol evolution

**Benefit**: Better coordination, easier debugging, extensibility

### 3. Caching Over Recomputation

**Principle**: Store and reuse artifacts rather than regenerate

**Implementation**:
- Local Cache System for persistent storage
- Reference-based message content
- Cache-aware agent design

**Benefit**: Efficiency, consistency, reduced costs

### 4. Orientation Over Symmetry

**Principle**: Directed communication with clear sender/receiver

**Implementation**:
- Oriented hyperedges (not undirected)
- Explicit source and target identification
- Workflow-aware routing

**Benefit**: Clear responsibilities, better traceability

## Technical Implementation

### Hypergraph Data Structure

**Node Representation**:
```python
class AgentNode:
    def __init__(self, agent_id, agent_type, capabilities):
        self.agent_id = agent_id
        self.agent_type = agent_type
        self.capabilities = capabilities
        self.inbox = []
        self.outbox = []
```

**Hyperedge Representation**:
```python
class OrientedHyperedge:
    def __init__(self, edge_id, source, targets, content):
        self.edge_id = edge_id
        self.source = source  # Single agent
        self.targets = targets  # List of agents
        self.content = content
        self.timestamp = now()
        self.status = "pending"
```

**Hypergraph Operations**:
```python
class OrientedHypergraph:
    def add_edge(self, source, targets, content):
        edge = OrientedHyperedge(gen_id(), source, targets, content)
        self.edges.append(edge)
        return edge
    
    def route_message(self, edge):
        for target in edge.targets:
            target.inbox.append(edge.content)
    
    def get_edges_from(self, agent):
        return [e for e in self.edges if e.source == agent]
    
    def get_edges_to(self, agent):
        return [e for e in self.edges if agent in e.targets]
```

### Cache Implementation

**Storage Layer**:
```python
class LocalCache:
    def __init__(self):
        self.storage = {}  # In-memory or persistent
        self.metadata = {}
    
    def store(self, key, content, agent, task_id):
        self.storage[key] = content
        self.metadata[key] = {
            "agent": agent,
            "task_id": task_id,
            "timestamp": now(),
            "size": len(content)
        }
    
    def retrieve(self, key):
        return self.storage.get(key)
    
    def list_by_task(self, task_id):
        return [k for k, m in self.metadata.items() 
                if m["task_id"] == task_id]
```

**Cache-Aware Messaging**:
```python
# Instead of sending large content
message = {
    "source": "WEB",
    "targets": ["ENG"],
    "content": {
        "type": "ANALYSIS_RESULT",
        "cache_key": "webpage_analysis_yahoo_finance",
        "summary": "Found 15 data elements across 3 sections"
    }
}

# Receiver retrieves from cache
analysis = cache.retrieve("webpage_analysis_yahoo_finance")
```

## Advantages

### vs Broadcast Communication

**Cost**:
- Broadcast: O(n²) message deliveries
- OHCache: O(n) message deliveries
- Real reduction: 60-75%

**Information Quality**:
- Broadcast: Agents overwhelmed with irrelevant info
- OHCache: Agents receive focused, relevant context

**Scalability**:
- Broadcast: Breaks down with many agents
- OHCache: Scales linearly with agent count

### vs Pairwise Communication

**Flexibility**:
- Pairwise: One-to-one only
- OHCache: One-to-many via hyperedges

**Coordination**:
- Pairwise: Manual orchestration needed
- OHCache: Workflow encoded in hypergraph

**Efficiency**:
- Pairwise: Redundant messages for group communication
- OHCache: Single hyperedge for group messaging

### vs Shared Blackboard

**Structure**:
- Blackboard: Unstructured shared memory
- OHCache: Structured messages + typed artifacts

**Routing**:
- Blackboard: Agents poll for relevant data
- OHCache: Directed routing to relevant agents

**Efficiency**:
- Blackboard: Agents process all entries to find relevant ones
- OHCache: Agents receive only relevant messages

## Limitations

### Current Constraints

**Complexity**:
- More complex than broadcast
- Requires workflow analysis upfront
- Hypergraph management overhead

**Static Routing**:
- Routing rules defined at design time
- Limited dynamic adaptation during execution
- Requires anticipation of communication patterns

**Cache Management**:
- Cache invalidation complexity
- Storage overhead for artifacts
- Potential consistency issues

### Not Suitable For

**Highly Dynamic Communication**:
- Ad-hoc, unpredictable message patterns
- Frequent changes in agent collaboration needs

**Small Agent Systems**:
- Overhead may not be justified for 2-3 agents
- Broadcast acceptable at small scale

**Tightly Coupled Agents**:
- All agents need all information
- No selective routing benefit

## Future Enhancements

### Dynamic Hypergraph

**Adaptive Routing**:
- Learn optimal message routing from execution history
- Adjust hyperedges based on actual information needs
- Runtime modification of communication structure

**Agent Composition**:
- Dynamically add/remove agents based on task requirements
- Automatic hypergraph reconfiguration
- Support for variable-size agent teams

### Intelligent Caching

**Semantic Cache**:
- Understand content similarity
- Retrieve similar (not just exact match) artifacts
- Cross-task reuse based on semantic similarity

**Predictive Caching**:
- Anticipate future cache needs
- Preload likely-needed artifacts
- Optimize cache eviction policies

### Multi-Level Caching

**Hierarchical Cache**:
- Agent-local cache (fast, private)
- Squad-level cache (shared within squad)
- System-level cache (shared across all agents)
- Automatic promotion/demotion

## Related Research

### Hypergraph Theory

**Foundations**:
- Hypergraphs generalize graphs
- Hyperedges connect arbitrary-size subsets
- Rich mathematical theory

**Applications in CS**:
- Database query optimization
- Circuit design
- Social network analysis
- Now: Multi-agent coordination

### Multi-Agent Communication

**Traditional Approaches**:
- Contract Net Protocol
- Blackboard systems
- Message-passing protocols
- Shared memory coordination

**OHCache Innovation**:
- Combines hypergraph topology with structured messaging
- Adds caching layer for efficiency
- Optimized for LLM-based agents

### Token Economics in LLM Systems

**Problem**:
- Token costs dominate LLM application expenses
- Multi-agent systems multiply costs
- Need efficient communication protocols

**OHCache Contribution**:
- Demonstrates 60-77% cost reduction
- Proves viability of selective routing
- Opens research direction for efficient LLM multi-agent systems

## Related Pages

### Concepts

- [[oriented-message-hypergraph|Oriented Message Hypergraph]]
- [[oriented-hyperedge-formatter|Oriented Hyperedge Formatter]]
- [[local-cache-system|Local Cache System]]
- [[selective-message-routing|Selective Message Routing]]
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]]

### Entities

- [[autodata|AutoData]]
- [[university-of-notre-dame|University of Notre Dame]]
- [[yanfang-ye|Yanfang Ye]]

### Sources

- [[autodata-paper|AutoData Paper]]

## Sources

- [[autodata-paper|AutoData Research Paper]] (NeurIPS 2025)
