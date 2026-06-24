---
type: concept
tags: [multi-agent, communication, graph-theory, autodata, optimization]
---

# Oriented Message Hypergraph

The oriented message hypergraph is a communication topology that enables selective message routing in [[multi-agent-systems|multi-agent systems]]. Unlike broadcast architectures where every message reaches every agent, hypergraph routing sends each message only to relevant recipients, dramatically reducing token costs and communication overhead.

## Graph-Theoretic Foundation

Traditional [[multi-agent-communication|agent communication]] uses simple graphs where edges connect pairs of agents. A **hypergraph** generalizes this: each hyperedge connects one source node to multiple target nodes simultaneously.

**Formal definition:**
- Agents form the vertex set: V = {A₁, A₂, ..., Aₙ}
- A directed hyperedge e = (s, T) has:
  - Source: s ∈ V (single agent)
  - Targets: T ⊆ V (subset of agents)

The "oriented" qualifier indicates directionality—messages flow from source to targets, not bidirectionally. This models realistic information dissemination where one agent produces output consumed by specific others.

## AutoData Implementation

[[autodata-framework|AutoData]] employs oriented message hypergraphs for its [[coordinator-agent|coordinator-agent]] architecture. The system includes:

- **Task Coordinator**: Receives user queries, routes to specialized agents
- **Web Navigator**: Produces URLs and HTML content
- **Data Extractor**: Consumes HTML, produces structured data
- **Quality Validator**: Consumes extracted data, produces validation results

The hypergraph structure encodes these dependencies:

```
Coordinator → {Navigator, Extractor, Validator}  [initial broadcast]
Navigator → {Extractor}                          [HTML delivery]
Extractor → {Validator}                          [data delivery]
Validator → {Coordinator}                        [results delivery]
```

This creates a **directed acyclic graph (DAG)** of information flow, preventing circular dependencies while enabling parallel execution.

## Token Cost Reduction

In a broadcast system with n agents and message size m tokens, total cost is **O(n × m)** per message. For complex tasks requiring many messages, this becomes prohibitive.

The AutoData paper reports that oriented routing achieves:
- **2.3x token reduction** compared to full broadcast
- **4.6x total cost reduction** when combined with [[local-cache-system|local caching]]

The savings come from:
1. **Relevance filtering**: Extractors don't receive coordinator-navigator routing messages
2. **Specialization**: Each agent maintains smaller, focused context
3. **Cached references**: Large artifacts use [[local-cache-system|cache IDs]] instead of inline data

## Dynamic Routing Strategies

While AutoData uses a fixed hypergraph topology, research explores **dynamic routing** where edge structure adapts during execution:

**Content-based routing:**
- Messages tagged with semantic labels (e.g., "html", "data", "error")
- Agents subscribe to relevant tags
- Router matches tags to subscriptions

**Learning-based routing:**
- [[reinforcement-learning|RL]] agents learn optimal communication patterns
- Minimize tokens while maximizing task success
- Explored in [[web-agent-rl-paper|Web Agent RL]] for cost optimization

**Hierarchical routing:**
- [[hierarchical-agents|Multi-level agent hierarchies]]
- Messages routed to subtree roots, then selectively propagated
- Used in [[auto-rag-system|AutoRAG]] for document processing

## Comparison to Alternative Topologies

| Topology | Token Cost | Latency | Flexibility |
|----------|-----------|---------|-------------|
| Broadcast | O(n × m) | Low | High |
| Point-to-point | O(m) | Medium | Low |
| Hypergraph | O(k × m), k<n | Low | Medium |
| Publish-subscribe | O(k × m) | Medium | High |

**Broadcast** (used in [[autogen-framework|AutoGen]]) maximizes flexibility but wastes tokens. **Point-to-point** minimizes cost but requires predetermined routing. **Hypergraph** balances both by grouping related recipients.

## Implementation Considerations

Building hypergraph routing requires:

1. **Topology specification**: Define source-target mappings statically or compute dynamically
2. **Message envelopes**: Include sender ID and recipient list in each message
3. **Delivery guarantees**: Ensure all targets receive messages (vs. best-effort)
4. **Failure handling**: Reroute if target agents fail or timeout

[[autodata-paper|AutoData]] implements this via a central message queue where the coordinator inspects each message's metadata to determine routing. More decentralized approaches use [[agent-registry|agent registries]] where agents publish capabilities and subscribe to message types.

## Performance Characteristics

AutoData experiments on [[srn-dataset|SRN]] and [[swde-dataset|SWDE]] benchmarks show:

- **Baseline broadcast**: 847K tokens per task
- **Hypergraph routing**: 368K tokens per task (2.3x reduction)
- **+ Local caching**: 184K tokens per task (4.6x total reduction)

Latency remains comparable because hypergraph routing is computed in microseconds, negligible compared to LLM inference time (seconds).

## Related Concepts

- [[multi-agent-systems|Multi-Agent Systems]]
- [[autodata-framework|AutoData Framework]]
- [[local-cache-system|Local Cache System]]
- [[coordinator-agent|Coordinator Agent]]
- [[multi-agent-communication|Multi-Agent Communication]]
- [[web-agent-cost-modeling|Web Agent Cost Modeling]]

## References

- [[autodata-paper|AutoData: Automated Multi-Agent Data Collection]]
- [[autogen-paper|AutoGen: Multi-Agent Conversation Framework]]
- [[web-agent-rl-paper|Reinforcement Learning for Web Agents]]
