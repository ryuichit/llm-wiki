# LLM Wiki - Operation Log

Chronological record of all wiki operations.

---

## 2026-06-24 - WebCloak Paper Ingest

Completed comprehensive ingest of WebCloak research paper on LLM-driven web agents as intelligent scrapers.

### Source Processed
- **WebCloak: Characterizing and Mitigating Threats from LLM-Driven Web Agents as Intelligent Scrapers**
- Authors: Xinfeng Li, Tianze Qiu, Yingbin Jin, Lixu Wang, Hanqing Guo, Xiaojun Jia, XiaoFeng Wang, Wei Dong
- Institutions: Nanyang Technological University, Hong Kong Polytechnic University, University of Hawaii at Manoa

### Pages Created

**Source Summary** (1 page):
- [[webcloak-paper|WebCloak Research Paper]] - Comprehensive summary with all key findings, methodologies, and implications

**Entity Pages** (8 pages):
- [[xinfeng-li|Xinfeng Li]] - First author, NTU researcher
- [[wei-dong|Wei Dong]] - Corresponding author, NTU researcher
- [[nanyang-technological-university|Nanyang Technological University]] - Lead institution
- [[hong-kong-polytechnic-university|Hong Kong Polytechnic University]] - Collaborating institution
- [[university-of-hawaii|University of Hawaii at Manoa]] - Collaborating institution
- [[gemini|Gemini]] - Google's LLM, top L2S performer (84.2% recall)
- [[crawl4ai|Crawl4AI]] - Top LNC tool (98% recall)
- [[browser-use|Browser-Use]] - Top LWA framework (99.3% recall)

**Concept Pages** (11 pages):
- [[llm-driven-web-agents|LLM-Driven Web Agents]] - Overview of the three scraping paradigms
- [[parse-then-interpret|Parse-Then-Interpret Mechanism]] - Universal vulnerability across all paradigms
- [[webcloak|WebCloak]] - Dual-layer defense system achieving 100% protection
- [[dynamic-structural-obfuscation|Dynamic Structural Obfuscation]] - Layer 1: HTML randomization defense
- [[semantic-labyrinth|Semantic Labyrinth]] - Layer 2: Adversarial prompt-based confusion
- [[llm-to-script|LLM-to-Script (L2S)]] - Code generation scraping paradigm
- [[llm-native-crawlers|LLM-Native Crawlers (LNC)]] - Integrated LLM crawling systems
- [[llm-based-web-agents|LLM-based Web Agents (LWA)]] - Browser-controlling autonomous agents
- [[llmcrawlbench|LLMCrawlBench]] - Benchmark with 237 pages, 10,895 images, 50 websites
- [[agent-as-attacker|Agent-as-Attacker Paradigm]] - Security paradigm shift

### Key Knowledge Captured

**Threat Characterization**:
- Three LLM scraping paradigms: L2S (84.2% best), LNC (98% best), LWA (99.3% best)
- 84.4% of 32 tested variants successfully scraped content
- Democratization: novices (71%) vs experts (78%) - only 7% gap
- Parse-then-interpret mechanism as universal vulnerability

**Defense System**:
- WebCloak reduces scraping from 88.7% to 0% recall
- Dual-layer protection: structural obfuscation + semantic confusion
- Minimal overhead: ~52ms client-side, ~3min server-side setup
- Preserves user experience completely

**Technical Details**:
- CSPRNG-based HTML tag/attribute randomization
- Per-session unique obfuscation (no learnable patterns)
- Client-side JavaScript restoration for browsers
- Adversarially optimized defensive prompts
- LLM-agnostic protection across GPT, Claude, Gemini, etc.

**Evaluation Results**:
- LLMCrawlBench: 237 webpages, 10,895 images, 50 websites
- 5 categories: Marketplaces, Travel, Design, Lifestyle, Entertainment
- Tested across 7 LLMs, 3 paradigms, multiple platforms
- 100% defense effectiveness across all variants

### Cross-References

Created dense knowledge graph with extensive cross-linking:
- All entity pages reference related concepts and sources
- All concept pages reference related entities, other concepts, and sources
- Source summary page links to all extracted entities and concepts
- Bidirectional references maintained throughout

### Statistics After Ingest

- **Total pages**: 20 (1 source + 8 entities + 11 concepts)
- **Total sources**: 1
- **Cross-references**: 150+ wiki links
- **Coverage**: Security, LLM agents, web scraping, defense mechanisms, evaluation benchmarks

Status: First ingest complete, wiki operational

---

## 2026-06-24 - Initialization

Wiki structure created with:
- CLAUDE.md system instructions
- index.md catalog
- log.md (this file)
- Directory structure for sources and wiki content

Status: Ready for first ingest
