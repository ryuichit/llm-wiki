---
title: Manager Agent (MGR)
tags: [entity, agent, autodata, multi-agent-system]
created: 2026-06-24
updated: 2026-06-24
---

# Manager Agent (MGR)

The central orchestrator in the [[autodata|AutoData]] multi-agent system, responsible for coordinating workflow across all specialized agents and ensuring successful completion of web data collection tasks.

## Role and Responsibilities

The Manager Agent serves as the command center for AutoData's dual-squad architecture. It receives natural language instructions from users, orchestrates the workflow across both Research and Development squads, monitors progress, handles failures, and returns final structured data to users. The MGR is the only agent that communicates directly with external users and maintains oversight of the entire collection pipeline.

Key responsibilities include:
- **Task initialization**: Parsing user instructions and distributing work to appropriate agents
- **Workflow coordination**: Managing sequential execution from research to development phases
- **Progress monitoring**: Tracking completion status across all agents
- **Error recovery**: Implementing retry mechanisms and fallback strategies
- **Quality control**: Validating outputs at critical checkpoints before proceeding
- **Final delivery**: Returning collected structured data to users

## Architecture Integration

The Manager Agent sits at the apex of AutoData's [[oriented-message-hypergraph|Oriented Message Hypergraph]] architecture, connected to all other agents through directed hyperedges:

```
User Instruction → MGR → Research Squad → MGR → Development Squad → MGR → User Output
```

It dispatches tasks to the [[research-squad|Research Squad]] ([[plan-agent|PLN]], [[web-agent|WEB]], [[tool-agent|TOL]], [[blueprint-agent|BLU]]) to analyze requirements and design strategies, then forwards the consolidated blueprint to the [[development-squad|Development Squad]] ([[engineering-agent|ENG]], [[test-agent|TST]], [[validation-agent|VAL]]) for implementation and validation.

## Tools and Capabilities

The Manager Agent operates following the [[react-paradigm|ReAct (Reasoning + Acting)]] paradigm:
- **Reasoning**: Analyzes task complexity to determine appropriate workflow paths
- **Acting**: Issues targeted messages through [[ohcache|OHCache]] to specific agents
- **Coordination**: Uses the [[oriented-hyperedge-formatter|Oriented Hyperedge Formatter]] for structured communication
- **Caching**: Leverages the [[local-cache-system|Local Cache System]] to store intermediate artifacts

Unlike specialized agents, MGR does not directly interact with web crawling or API tools. Instead, it delegates technical tasks to appropriate specialists while maintaining strategic oversight.

## Communication via OHCache

The Manager Agent is the primary beneficiary of [[ohcache|OHCache's]] selective message routing architecture. Rather than broadcasting messages to all agents, MGR sends targeted communications through oriented hyperedges, reducing token costs by approximately 60%. It receives summarized outputs from each squad rather than raw intermediate data, preventing information overload while maintaining sufficient context for decision-making.

The MGR coordinates two distinct communication phases:
1. **Research phase**: Collects analysis from PLN, WEB, TOL, and BLU to form comprehensive blueprint
2. **Development phase**: Provides blueprint to ENG, receives code for TST evaluation, coordinates VAL verification

## Decision Making and Adaptation

The Manager Agent implements adaptive workflow strategies based on task characteristics:
- **Simple tasks**: Direct path from research to engineering to validation
- **Complex scenarios**: Iterative refinement with feedback loops between squads
- **API vs crawling**: Determines optimal collection method based on tool agent findings
- **Error handling**: Triggers re-execution of specific agents when validation fails

This flexibility enables AutoData to achieve 92.91% F1 score across the [[instruct2ds|Instruct2DS]] benchmark while maintaining efficiency.

## Performance Impact

Ablation studies in the [[autodata-paper|AutoData research paper]] demonstrate that removing the Manager Agent significantly degrades system performance. Without centralized coordination, specialized agents struggle with proper sequencing, redundant work, and conflicting strategies. The MGR's orchestration is essential for AutoData's superiority over single-agent baselines and uncoordinated multi-agent approaches.

## Related Agents

**Research Squad**:
- [[plan-agent|Plan Agent (PLN)]]
- [[web-agent|Web Agent (WEB)]]
- [[tool-agent|Tool Agent (TOL)]]
- [[blueprint-agent|Blueprint Agent (BLU)]]

**Development Squad**:
- [[engineering-agent|Engineering Agent (ENG)]]
- [[test-agent|Test Agent (TST)]]
- [[validation-agent|Validation Agent (VAL)]]

## See Also

- [[autodata|AutoData System]]
- [[autodata-paper|AutoData Research Paper]]
- [[ohcache|OHCache]]
- [[oriented-message-hypergraph|Oriented Message Hypergraph]]
- [[react-paradigm|ReAct Paradigm]]
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]]
