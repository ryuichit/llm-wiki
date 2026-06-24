# Prune4Web

**Type**: Web agent system with programmatic DOM pruning  
**Institution**: [[beihang-university|Beihang University]]  
**Release**: November 2025  
**Paper**: [[prune4web-paper|Prune4Web: DOM Tree Pruning Programming for Web Agent]]

## Overview

Prune4Web is a novel web agent framework that addresses DOM information overload through **DOM Tree Pruning Programming** - a paradigm shift from LLM-based filtering to programmatic pruning. Instead of processing massive DOM structures (10,000-100,000 tokens) with LLMs, Prune4Web generates lightweight Python scoring programs that dynamically filter elements based on semantic clues from decomposed sub-tasks.

## Key Innovation

**Paradigm Shift**:
- **Traditional**: LLM directly processes full DOM → token truncation, attention dilution, information loss
- **Prune4Web**: LLM generates scoring program from sub-task → program traverses DOM → 25-50× reduction → LLM processes pruned list

**Core insight**: Low-level sub-tasks contain semantic clues about relevant DOM elements. Generate a locator program based on sub-task alone, avoiding full DOM exposure to LLM.

## Architecture

### Three-Stage Pipeline

**1. Planning Stage**:
- Model: Qwen2.5VL-3B-Instruct (fine-tuned)
- Input: High-level task, screenshot, history
- Output: Low-level sub-task (e.g., "Find destination field and Type NYC")
- Note: Does NOT access HTML source code

**2. Filtering Stage** (Core Innovation):
- Model: Programmatic Element Filter
- Input: Low-level sub-task, complete HTML
- Process:
  1. Rule-based pre-filtering (interactive elements only)
  2. LLM generates keywords + weights: `{"destination": 50, "city": 30, "field": 20}`
  3. Scoring function template executes (multi-tier weighted matching)
  4. Top-N selection (default N=20)
- Output: Pruned candidate list (~20 elements from 500+)
- **25-50× reduction** in candidates

**3. Action Grounding Stage**:
- Model: Action Grounder
- Input: Low-level sub-task, pruned candidate list
- Output: Final action (e.g., `click_element(index=495)`)
- Note: Only processes small, refined list

## Programmatic Element Filtering

### Scoring Function Template

**Multi-tier attribute weighting**:
- **Tier 1** (β₁): Visible text content (highest priority)
- **Tier 2** (β₂): Semantic attributes (aria-label, placeholder, name)
- **Tier 3** (β₃): Structural attributes (class, id, other)

**Match type ranking**:
- **Exact match** (α₁): Full string equality
- **Phrase match** (α₂): Keyword substring in raw text
- **Word match** (α₃): Keyword in tokenized list
- **Fuzzy match** (α₄): FuzzyScore above threshold

**Score calculation**: S[e] = Σ (W[k] × α × β) for all keyword-attribute matches

**LLM task**: Generate only `keyword_weights` dictionary (not complex scoring logic)

**Advantages**:
- Lightweight execution (no LLM inference)
- Interpretable scoring paths (debugging support)
- Robust (hard-coded template logic)
- Efficient (0.5B model sufficient)

## Performance Results

### Element Grounding (Ground-Truth Sub-Tasks)

**Low-level sub-task grounding benchmark** (1,101 test cases):

| Configuration | Recall@20 | Grounding Accuracy |
|---------------|-----------|-------------------|
| No Pruning (Qwen2.5VL-3B) | – | 46.80% |
| Oracle Pruning (Qwen2.5VL-3B) | – | 90.28% |
| GPT-4o (Prune4Web) | 85.56% | 80.65% |
| GPT-4o-mini (Prune4Web) | 89.19% | 73.75% |
| **Qwen2.5-0.5B (Prune4Web)** | **97.64%** | **88.28%** |
| **Qwen2.5VL-3B (Prune4Web)** | **97.46%** | **88.28%** |

**Key findings**:
- **+41.48 absolute gain** (46.80% → 88.28%) over no pruning
- **0.5B model outperforms GPT-4o** (97.64% vs 85.56% recall@20)
- **>90% recall at N=3** (GT element almost always in top 3)
- Programmatic filtering essential for smaller models

### End-to-End Performance (Multimodal-Mind2Web)

**Prune4Web-3B (Two-turn Dialogue Unified)**:

| Split | Element Acc | Operation F1 | Step Success Rate |
|-------|-------------|--------------|-------------------|
| Cross-Task | 58.4% | 84.1% | **52.4%** |
| Cross-Website | 49.2% | 84.4% | **46.1%** |
| Cross-Domain | 50.2% | 81.2% | **44.9%** |

- Outperforms GPT-4o, SeeAct, SeeClick-9.6B, MiniCPM-3.1B
- Competitive with ScribeAgent-32B, GPT-4o UGround, EDGE-9.6B
- Achieved with ~5,000 training trajectories (excellent data efficiency)

### Online Evaluation (30 Dynamic Tasks)

**Filtering method comparison**:
- GPT-4o-mini: 26.3% (LLM Top-N) → **31.6%** (Prune4Web) [+5.3%]
- Qwen2.5VL-3B: 0.0% (LLM Top-N) → **5.2%** (Prune4Web) [critical for functionality]

**Architecture ablation** (GPT-4o-mini):
- Action Grounder Only: 21.1%
- + Planner: 26.3% (+5.2)
- **+ Prune4Web Filter**: **31.6%** (+10.5 total)

### Plug-and-Play Enhancement

**UI-TARS integration** (no retraining):
- UI-tars-1.5-7B (End-to-End): 37.8% Step SR
- **UI-tars + Prune4Web**: **53.6%** Step SR [+15.8 absolute]

## Training Methodology

### Data Synthesis

**Source**: [[multimodal-mind2web|Multimodal-Mind2Web]] re-annotated with GPT-4o

**Annotation pipeline**:
1. Low-level sub-tasks for Planner (with GT element for accuracy)
2. Keywords + weights for Programmatic Element Filter
3. Pruned DOM trees + thought processes for Action Grounder

**Quality control**:
- Automatic filtering: Remove if GT element not in top-20 after filtering
- Token length filtering: Prevent training issues
- Manual verification: Sampling check

**Final dataset**: ~5,000 high-quality interaction steps (80% train, 20% test)

### Supervised Fine-Tuning (SFT)

**Two paradigms**:

**Separate Models**:
- Three independent models (Planner, Filter, Grounder)
- Each specialized for one task

**Unified Model (Two-Turn Dialogue)** [Better performance]:
- Single model, two-turn conversation
- Turn 1: Generate plan + keywords/weights
- Turn 2: After program execution, output final action
- 52.4% Step SR vs 42.2% for separated models

**Base model**: Qwen2.5VL-3B-Instruct
- Learning rate: 5.0e-5 (cosine schedule)
- Effective batch size: 64
- Epochs: 3
- Precision: bf16 mixed-precision

### Reinforcement Fine-Tuning (RFT)

**Algorithm**: Group Relative Policy Optimization (GRPO)

**Target**: Planner only (first turn of unified model)
- Filter and Grounder are deterministic, learned well via SFT
- Planner requires long-horizon planning → benefits from RL

**Hierarchical Reward Mechanism**:
- Rtotal = Rformat + Rfiltering + Rgrounding
- **Rformat** (binary): Correct JSON format
- **Rfiltering** (binary): GT element in top-20 pruned candidates
- **Rgrounding** (binary): Final action matches GT exactly
- Step-wise: Failure at one stage → 0 reward for subsequent stages

**Effectiveness**:
- Separate Models: +4.3% (37.9% → 42.2%)
- Two-turn Dialogue: +5.9% (46.5% → 52.4%)

**Innovation**: Uses downstream success as reward signal for upstream planning

## Technical Details

### Rule-Based Pre-Filtering (JavaScript)

**buildDomTree.js** (runs in browser):
- Traverses entire DOM tree
- Identifies interactive elements:
  - HTML tags: <a>, <button>, <input>, <select>, <textarea>
  - ARIA roles: button, link, checkbox, radio, tab, menuitem
  - Attributes: onclick, tabindex, contenteditable
  - CSS: cursor: pointer
  - Event listeners: window.getEventListeners
- Filters invisible/obscured elements
- Attaches non-interactive text as context to nearest interactive element

### Action Space

**7 discrete actions**:
1. CLICK(element_id)
2. TYPE(element_id, text)
3. SCROLL(direction)
4. SELECT_OPTION(element_id, value)
5. NAVIGATE(url)
6. DONE()
7. FAIL()

**Execution**: Multi-strategy interaction (standard → JavaScript fallback for React/Vue)

### Scoring Function Implementation

**Dependencies**:
- rapidfuzz: Efficient fuzzy string matching
- nltk.stem.PorterStemmer: Keyword/text normalization

**Complexity**: O(E × A × K) where E=elements, A=attributes, K=keywords

**Hyperparameters**:
- α₁ > α₂ > α₃ > α₄ (match type quality)
- β₁ > β₂ > β₃ (attribute tier priority)
- Fuzzy threshold θ for minimum similarity

## Use Cases and Applications

### Demonstrated Capabilities

**E-commerce**:
- Multi-step form filling (recipient email, from field, delivery date)
- Gift card purchase workflows
- Add to cart interactions

**Information Retrieval**:
- Amazon product searches (including CAPTCHA handling)
- Protection plan cost extraction
- Review and rating information

**Account Management**:
- GitHub signup flows
- Email verification handling
- Multi-step authentication

**Content Navigation**:
- Amtrak menu navigation
- Multi-level website hierarchy
- Semantic link selection

### Successful Use Cases

**Complex HTML** (100% success):
- Deep nesting, dynamic classes handled
- Visual agents excel over traditional tools

**Authentication** (63-70% success for agents):
- Username/password login
- Email/SMS verification
- Multi-tab workflows

**CAPTCHA** (5-10% success, still challenging):
- Visual puzzles
- Behavioral tests
- Strongest protection tier

## Limitations

### Planning Bottleneck

**Observation**: Perfect grounding can't compensate for flawed planning
- Rottentomatoes failure: 25-step ineffective loop due to wrong strategy
- Carmax failure: Incorrect goal identification led to wrong path
- SFT+RFT with ~5,000 samples insufficient for robust planning

### Filtering/Grounding Challenges

**Non-standard HTML**:
- Misuse of <div> as buttons (no explicit roles)
- Visual agents (Simular.ai) handle better (100% complex HTML)

**Lack of semantic features**:
- Icon-only buttons (no text/aria-labels)
- Keyword matching fails

**Visual-source inconsistency**:
- CSS-driven presentation ≠ HTML structure
- Misleads filtering

### WebCloak Vulnerability

**Still vulnerable** to [[webcloak|WebCloak]]-style defenses:
- Programmatic filter receives full DOM (pre-filtered)
- DOM structure obfuscation breaks keyword matching
- Semantic confusion defeats scoring program
- **Partial mitigation only**: Reduces LLM exposure but doesn't eliminate parse-then-interpret

## Comparison with Related Work

### vs WebCloak (Defense)

**Complementary perspectives**:
- Prune4Web: Efficiency optimization for legitimate agents
- WebCloak: Defense against unauthorized scraping
- Both recognize DOM processing as bottleneck
- **WebCloak would still work**: Obfuscated DOM defeats keyword matching

### vs AutoData (Multi-Agent Collection)

**Integration potential**:
- AutoData: Multi-agent coordination (92.91% F1)
- Prune4Web: Optimized element grounding (88.28% accuracy)
- **Synergy**: Prune4Web's Filter + Grounder could enhance AutoData's Web Agent (WEB)

### vs WebDancer (Single-Agent RL)

**Complementary optimizations**:
- WebDancer: End-to-end RL (51.5% GAIA, 47.9% WebWalkerQA)
- Prune4Web: Modular with programmatic pruning (88.28% grounding)
- **Synergy**: Apply Prune4Web filtering to WebDancer's Action Grounder

### vs Traditional DOM Pruning

**Advantages over alternatives**:
- **Rule-based** (accessibility tree): Too rigid, poor generalization
- **LLM ranking** (Mind2Web): Doesn't reduce context burden
- **Prune4Web**: Lightweight program, 25-50× reduction, interpretable

## Future Directions

### Planning Enhancements

- Larger, more diverse training data
- Exploration mechanisms (Monte Carlo Tree Search)
- Multi-turn refinement
- Hierarchical planning (>120 steps)

### Filtering/Grounding Improvements

- **Multimodal fusion**: Visual understanding for CSS-defined elements
- **Layout analysis**: Infer function of non-semantic tags
- **Icon recognition**: Computer vision for icon-only buttons
- **Robustness**: Handling malformed HTML

### System Extensions

- Dense reward shaping (per-step feedback)
- Extended action space (summarize, compare, verify)
- Long-term memory across sessions
- Off-policy methods for sample efficiency

### Application Domains

- Scientific literature review
- Legal research
- Medical information aggregation
- Financial analysis
- Domain-specific specialization

## Impact and Significance

**Addresses critical bottleneck**: DOM information overload (10,000-100,000 tokens)

**Paradigm shift**: LLM processes full DOM → LLM generates program, program processes DOM

**Dramatic accuracy gains**: 46.8% → 88.28% grounding accuracy (+41.48 absolute)

**Enables smaller models**: 0.5B achieves 97.64% recall@20 (better than GPT-4o 85.56%)

**Universal enhancement**: Plug-and-play for existing agents (+15.8% for UI-TARS)

**Production-ready**: ~52ms overhead, interpretable, preserves user experience

## See Also

### Paper and Authors
- [[prune4web-paper|Prune4Web Paper]] - Detailed research paper
- [[jiayuan-zhang|Jiayuan Zhang]] - Co-first author
- [[kaiquan-chen|Kaiquan Chen]] - Co-first author
- [[jing-zhang-beihang|Jing Zhang]] - Corresponding author
- [[beihang-university|Beihang University]] - Institution

### Core Concepts
- [[dom-tree-pruning|DOM Tree Pruning]] - Core paradigm
- [[programmatic-element-filtering|Programmatic Element Filtering]] - Filtering methodology
- [[two-turn-dialogue-training|Two-Turn Dialogue Training]] - Training strategy
- [[hierarchical-reward-rl|Hierarchical Reward RL]] - RFT innovation
- [[web-agent-token-efficiency|Web Agent Token Efficiency]] - Optimization focus
- [[multimodal-web-agents|Multimodal Web Agents]] - Framework category

### Related Technologies
- [[webcloak|WebCloak]] - Defense that counters this approach
- [[autodata|AutoData]] - Multi-agent collection (integration potential)
- [[webdancer|WebDancer]] - Single-agent RL (integration potential)
- [[multimodal-mind2web|Multimodal-Mind2Web]] - Source benchmark

### Comparisons
- [[prune4web-vs-webcloak|Prune4Web vs WebCloak]] - Efficiency vs defense
- [[prune4web-vs-autodata|Prune4Web vs AutoData]] - Single-agent vs multi-agent
- [[prune4web-vs-webdancer|Prune4Web vs WebDancer]] - Programmatic vs end-to-end RL
