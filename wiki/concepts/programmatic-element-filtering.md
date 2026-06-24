# Programmatic Element Filtering

**Category**: Web agent filtering technique  
**Introduced**: [[prune4web-paper|Prune4Web]] (Beihang University, 2025)  
**Purpose**: Generate executable scoring programs for DOM element filtering  
**Performance**: 97.64% recall@20 with 0.5B model (vs 85.56% GPT-4o zero-shot)

## Overview

Programmatic Element Filtering is a novel approach to DOM element selection that shifts the task from "LLM processes and filters DOM" to "LLM generates program, program filters DOM." Instead of exposing massive DOM structures (10,000-100,000 tokens) to LLMs for direct filtering, this technique has LLMs generate lightweight Python scoring programs based solely on low-level sub-tasks. These programs execute outside the LLM to traverse the DOM, score elements, and select top candidates.

## Core Concept

### The Paradigm Shift

**Traditional LLM Filtering**:
```
Input: Low-level sub-task + Full DOM (10,000-100,000 tokens)
Process: LLM reads all elements, ranks by relevance
Output: Top-k elements
Problem: Expensive, slow, attention dilution, strong model required
```

**Programmatic Element Filtering**:
```
Input: Low-level sub-task ONLY (no DOM)
Process: LLM generates scoring program (keywords + weights)
Execution: Program runs outside LLM, traverses DOM
Output: Top-k elements
Advantages: Fast, efficient, interpretable, works with tiny models
```

**Key insight**: Low-level sub-tasks contain semantic clues sufficient to generate effective filtering programs without ever seeing the DOM.

### Example

**Low-level sub-task**: "Find the destination field and Type NYC"

**LLM generates** (programmatic filter):
```python
keyword_weights = {
    "destination": 50,
    "city": 30,
    "field": 20,
    "input": 15,
    "to": 10
}
```

**Program executes** (outside LLM):
- Traverses pre-filtered DOM (interactive elements)
- Scores each element using keyword matching
- Multi-tier weighting (visible text > semantic attributes > structural attributes)
- Fuzzy matching for robustness
- Returns top-20 highest-scoring elements

**Result**: 500+ elements → 20 candidates in <1 second, no LLM inference

## Technical Implementation

### Three-Component Architecture

**1. LLM Keyword Generator**:
- Input: Low-level sub-task (e.g., "Find destination field")
- Model: Tiny LLM sufficient (Qwen2.5-0.5B achieves 97.64% recall@20)
- Output: `keyword_weights` dictionary
- Example: `{"destination": 50, "city": 30, "field": 20}`
- Format: JSON with keywords (strings) and weights (integers 1-50)

**2. Scoring Function Template**:
- Hard-coded heuristic template (not LLM-generated)
- Receives keyword_weights as parameters
- Implements multi-tier weighted matching
- Uses rapidfuzz for fuzzy string matching
- Uses nltk.stem.PorterStemmer for normalization
- Returns scored elements with justification paths

**3. Top-N Selector**:
- Sorts elements by score (descending)
- Selects top N candidates (default N=20)
- Returns pruned list to downstream Action Grounder

### Scoring Function Template

**Multi-tier attribute weighting**:
```python
# Tier 1: Visible text content (highest priority)
β1 = 2.0  # User-facing, most reliable

# Tier 2: Semantic attributes (medium priority)
β2 = 1.0  # aria-label, placeholder, name, title

# Tier 3: Structural attributes (lowest priority)
β3 = 0.5  # class, id, other technical attributes
```

**Match type ranking** (quality):
```python
# Match quality (α values)
α1 = 5.0  # Exact match: text == keyword
α2 = 3.0  # Phrase match: keyword substring in text
α3 = 2.0  # Word match: keyword in tokenized list
α4 = 1.0  # Fuzzy match: FuzzyScore(k, text) > 0.8
```

**Score calculation** (pseudo-code):
```python
def score_element(element, keyword_weights):
    total_score = 0
    paths = []  # Justification
    
    for attribute, text in element.attributes:
        # Determine tier weight
        if attribute in ['text', 'innerText']:
            β = β1  # Visible text
        elif attribute in ['aria-label', 'placeholder', 'name', 'title']:
            β = β2  # Semantic
        else:
            β = β3  # Structural
        
        # Normalize text
        normalized_text = stem(text.lower())
        tokens = tokenize(normalized_text)
        
        for keyword, weight in keyword_weights.items():
            normalized_keyword = stem(keyword.lower())
            
            # Determine match type
            if normalized_text == normalized_keyword:
                α = α1  # Exact
            elif ' ' in keyword and keyword in text:
                α = α2  # Phrase
            elif normalized_keyword in tokens:
                α = α3  # Word
            else:
                fuzzy_score = FuzzyScore(normalized_keyword, normalized_text)
                if fuzzy_score > 0.8:
                    α = α4 * fuzzy_score  # Fuzzy
                else:
                    continue  # No match
            
            # Accumulate score
            contribution = weight × α × β
            total_score += contribution
            paths.append(f"{keyword} ({weight}) × {match_type} ({α}) × {attribute} ({β}) = {contribution}")
    
    return total_score, paths
```

**Final ranking**:
```python
scored_elements = [(score_element(e, keyword_weights), e) for e in pre_filtered_elements]
sorted_elements = sorted(scored_elements, key=lambda x: x[0], reverse=True)
top_N = [e for (score, path), e in sorted_elements[:N]]
```

### Dependencies

**rapidfuzz**: Efficient fuzzy string matching
- Levenshtein distance calculation
- Optimized C++ implementation
- Threshold-based matching (typically >0.8)

**nltk.stem.PorterStemmer**: Text normalization
- Reduces words to stems (e.g., "typing" → "type")
- Improves matching across variations
- English-focused (extensible to other languages)

## Performance Results

### Filtering Precision (Recall@N)

**How often GT element in top-N**:

| Model | Recall@1 | @3 | @5 | @10 | @20 |
|-------|----------|----|----|-----|-----|
| GPT-4o (zero-shot) | 51.41% | 72.12% | 76.75% | 82.29% | 85.56% |
| GPT-4o-mini (zero-shot) | 51.04% | 72.21% | 79.02% | 84.38% | 89.19% |
| MindAct | 51.10% | 79.74% | 87.48% | 94.20% | 97.15% |
| **Prune4Web 0.5B** | **74.30%** | **90.83%** | **93.55%** | **95.64%** | **97.64%** |
| **Prune4Web 3B** | **74.66%** | **90.83%** | **94.01%** | **95.82%** | **97.46%** |

**Key findings**:
- **>90% recall at N=3**: GT element almost always in top 3 candidates
- **>95% recall at N=5**: Near-optimal filtering
- **0.5B matches 3B**: Keyword generation simple enough for tiny models
- **+23 points @1 vs GPT-4o**: 74% vs 51% (massive improvement)
- **+18 points @3 vs GPT-4o**: 91% vs 72% (near-perfect top-3)

### Grounding Accuracy (With Filtering)

**End-to-end accuracy** (given GT low-level sub-task):

| Configuration | Recall@20 | Grounding Accuracy |
|---------------|-----------|-------------------|
| No Pruning (Qwen2.5VL-3B) | – | 46.80% |
| Oracle Pruning (GT in list) | – | 90.28% |
| GPT-4o + Prune4Web Filter | 85.56% | 80.65% |
| GPT-4o-mini + Prune4Web Filter | 89.19% | 73.75% |
| **0.5B Filter + 3B Grounder** | **97.64%** | **88.28%** |
| **3B Filter + 3B Grounder** | **97.46%** | **88.28%** |

**Insights**:
- **97.64% recall** means filtering almost never misses target
- **88.28% accuracy** approaches oracle performance (90.28%)
- **0.5B filter outperforms GPT-4o** (97.64% vs 85.56%)
- Filtering quality critical for downstream grounding success

### Efficiency Gains

**Inference time**:
- Keyword generation: <0.1 seconds (tiny model)
- Program execution: <1 second (no LLM inference)
- Total filtering: <2 seconds (vs 10-120 seconds for full DOM processing)

**Token efficiency**:
- Input to LLM: Low-level sub-task only (50-200 tokens)
- Output from LLM: Keyword dictionary (20-50 tokens)
- DOM never exposed to LLM (0 tokens)
- Downstream grounder: Pruned list only (400-4,000 tokens vs 10,000-100,000)

**Cost efficiency**:
- 0.5B model: Minimal cost per inference
- No repeated LLM calls for ranking
- Scalable to millions of tasks

## Advantages Over Alternatives

### vs Direct LLM Filtering

**Direct LLM**:
- Processes full DOM (10,000-100,000 tokens)
- Expensive inference (GPT-4o: $0.01-$0.10 per page)
- Attention dilution over long context
- Requires strong model (GPT-4o: 85.56% recall@20)

**Programmatic Filtering**:
- Processes sub-task only (50-200 tokens)
- Minimal cost (0.5B model: $0.0001 per page)
- No attention dilution (focused keyword generation)
- Works with tiny model (0.5B: 97.64% recall@20)

**Winner**: Programmatic Filtering (12 points better, 100× cheaper, 100× faster)

### vs LLM-Based Ranking

**LLM Ranking** (Mind2Web, WebLinx):
- Separate ranking model
- Processes all candidates with LLM
- Scores each element individually
- Still expensive (full inference per element)

**Programmatic Filtering**:
- Single keyword generation call
- Program scores all elements
- No LLM inference during scoring
- One-time cost

**Winner**: Programmatic Filtering (1 LLM call vs N calls)

### vs Rule-Based Heuristics

**Heuristics** (accessibility tree):
- Fixed rules (tag + role based)
- No task-specific adaptation
- Rigid, poor generalization
- Misses context

**Programmatic Filtering**:
- Dynamic keywords from sub-task
- Task-specific semantic matching
- Flexible, generalizes to any task
- Rich context via multi-tier matching

**Winner**: Programmatic Filtering (88.28% vs lower accuracy)

## Why Tiny Models Succeed

### Keyword Generation is Simple

**Task complexity**:
- Extract key nouns and verbs from sub-task
- Assign relevance weights based on importance
- No complex reasoning required
- No DOM understanding needed

**Example analysis**:
```
Sub-task: "Find the destination field and Type NYC"
Keywords (automatic extraction):
  - "destination" (explicit mention, high weight: 50)
  - "field" (explicit mention, medium weight: 20)
  - "input" (implicit, medium weight: 15)
  - "NYC" (target value, low weight: 10)
  - "city" (contextual, low weight: 10)
```

**0.5B model sufficient**:
- Simple pattern matching
- No multi-hop reasoning
- No long-range dependencies
- Template-guided generation

### Template Handles Complexity

**LLM responsibility**: Keywords + weights only

**Template responsibility** (hard-coded):
- Multi-tier attribute weighting (β1, β2, β3)
- Match type ranking (α1, α2, α3, α4)
- Fuzzy matching logic (rapidfuzz)
- Score accumulation
- Normalization (stemming, lowercasing)
- Top-N selection

**Division of labor**:
- LLM: High-level semantic understanding (simple)
- Template: Low-level scoring logic (complex, but fixed)

### Empirical Evidence

**0.5B vs 3B performance**:
- Recall@20: 97.64% vs 97.46% (nearly identical)
- Grounding accuracy: 88.28% vs 88.28% (exact match)
- Inference time: <0.1 sec vs <0.2 sec (both fast)

**Conclusion**: Diminishing returns beyond 0.5B for this task

## Integration Patterns

### With Planner-Grounder Architecture

**Standard pattern** (Prune4Web):
```
Planner (vision-language model)
    ↓ low-level sub-task
Programmatic Element Filter (0.5B model)
    ↓ keyword_weights
Scoring Function Template (program execution)
    ↓ pruned candidates (~20 elements)
Action Grounder (LLM)
    ↓ final action
```

**Benefits**:
- Each component optimized for its task
- Planner: Visual + semantic reasoning
- Filter: Lightweight keyword extraction
- Template: Efficient DOM traversal
- Grounder: Final decision on small list

### With Multi-Agent Systems

**AutoData integration** (proposed):
```
Manager Agent
    ↓ sub-task assignment
Plan Agent (decomposition)
    ↓ low-level sub-tasks
Web Agent + Programmatic Filter
    ↓ pruned elements
Web Agent + Action Grounder
    ↓ element grounding
Validation Agent
    ↓ verify correctness
```

**Expected improvements**:
- Web Agent (WEB) better element localization
- Higher F1 on Instruct2DS benchmark
- Lower token cost (OHCache efficiency + pruning)

### With RL-Trained Agents

**WebDancer integration** (proposed):
```
WebDancer Policy
    ↓ thought (implicit sub-task)
Programmatic Element Filter
    ↓ pruned candidates
WebDancer Action Grounder
    ↓ final action
Environment
    ↓ reward
WebDancer Policy (update via DAPO)
```

**Expected improvements**:
- Better element grounding (88.28% vs current)
- Higher GAIA/WebWalkerQA scores
- Faster training (reduced action space)

### Plug-and-Play Enhancement

**Requirements**:
1. Agent outputs reasoning process or sub-task
2. Parse reasoning as input to filter
3. Replace agent's element selection with pruned candidates
4. No retraining required

**Demonstrated** (UI-TARS):
- UI-tars: 37.8% Step SR → UI-tars + Prune4Web: 53.6% (+15.8)
- Simply replaced element selection module
- Immediate performance gain

## Training Strategy

### Supervised Fine-Tuning (SFT)

**Training data**:
- ~5,000 interaction steps from [[multimodal-mind2web|Multimodal-Mind2Web]]
- Annotated with GPT-4o:
  1. Low-level sub-tasks (with GT element for accuracy)
  2. Keywords + weights for each sub-task
  3. Verification: GT element in top-20 after filtering

**Training objective**:
- Input: Low-level sub-task (e.g., "Find destination field")
- Output: keyword_weights dictionary
- Format: JSON `{"destination": 50, "field": 20, "input": 15}`
- Loss: Cross-entropy on keyword selection + weight prediction

**Base models**:
- Qwen2.5-0.5B-Instruct (best cost-performance)
- Qwen2.5VL-3B-Instruct (slightly better, but marginal)

**Hyperparameters**:
- Learning rate: 5.0e-5 (cosine schedule)
- Batch size: 64 (effective, via gradient accumulation)
- Epochs: 3
- Precision: bf16
- Framework: LLaMA-Factory

**Quality control** (automatic):
- Remove training examples where GT element not in top-20 after filtering
- Ensures high-quality filter training
- Self-cleaning dataset

### Reinforcement Fine-Tuning (RFT)

**Not applied to filter directly**:
- Filter task deterministic, learned well via SFT alone
- RFT applied to Planner (generates sub-tasks)

**Indirect optimization via hierarchical rewards**:
- Rfiltering: Binary reward (GT element in top-20 after filtering)
- Planner receives reward based on filter success
- Encourages Planner to generate sub-tasks that lead to good filtering

**Synergy**: Better sub-tasks → better keyword extraction → better filtering

### Two-Turn Dialogue Training

**Unified model approach**:
- Turn 1: Planner + Filter (generate plan + keywords simultaneously)
- Turn 2: Grounder (after program execution, output action)

**Training template**:
```
<think>[Reasoning about task]</think>
<plan>[Low-level sub-task]</plan>
<keywords_weights>{"destination": 50, "field": 20, ...}</keywords_weights>

[System executes filtering program]

<answer>{"action": "type", "id": 31, "input_text": "NYC"}</answer>
```

**Advantages**:
- Joint optimization of planning + filtering
- Tighter coupling between stages
- Better end-to-end performance (52.4% vs 42.2% Step SR)

## Interpretability and Debugging

### Scoring Paths

**Justification for each score**:
```python
Element [31] <input aria-label="Destination city" name="dest_field">:
  Total score: 150
  
  Paths:
    1. keyword="destination" (weight=50)
       × match="phrase" (α=3.0, substring in aria-label)
       × attribute="aria-label" (β=1.0, semantic)
       = 150
    
    2. keyword="city" (weight=30)
       × match="word" (α=2.0, in tokenized aria-label)
       × attribute="aria-label" (β=1.0, semantic)
       = 60
    
    3. keyword="field" (weight=20)
       × match="exact" (α=5.0, exact match in name)
       × attribute="name" (β=1.0, semantic)
       = 100
```

**Debugging use cases**:
- **Why element scored low**: Check paths, see which keywords/attributes missed
- **Why wrong element scored high**: Identify spurious keyword matches, adjust weights
- **Refine filtering**: Add keywords for missed elements, increase weights for critical terms

### Human Oversight

**Review top-N candidates**:
- Inspect scores and paths
- Verify relevance to sub-task
- Identify false positives/negatives
- Iterative refinement

**Adjust keyword weights**:
- Increase weight for critical keywords (underweighted)
- Decrease weight for spurious keywords (overweighted)
- Add missing keywords (false negatives)
- Remove irrelevant keywords (false positives)

**Example refinement**:
```
Initial: {"destination": 50, "field": 20}
Problem: Missed "airport" dropdown

Refined: {"destination": 50, "field": 20, "airport": 40, "city": 30}
Result: Airport dropdown now in top-3
```

## Limitations and Future Work

### Non-Standard HTML

**Problem**: Elements without semantic attributes
- <div> as button (no button tag, no role, no aria-label)
- Icon-only buttons (🔍, 🛒, ❤️)
- CSS-styled interactivity (no HTML indication)

**Failure mode**: Rule-based pre-filtering misses, or keyword matching fails

**Mitigation** (future work):
- Visual analysis (screenshot + HTML fusion)
- Icon recognition (computer vision)
- Layout understanding (spatial reasoning)

### Visual-Source Inconsistency

**Problem**: CSS effects not captured in HTML
- Hidden text (display: none) revealed via CSS (::before, ::after)
- Visual buttons (CSS pointer) without HTML button tags
- Layout misleading (visual order ≠ DOM order)

**Failure mode**: Scoring based on HTML misses CSS effects

**Mitigation** (future work):
- Rendering-based analysis
- Screenshot + HTML joint understanding
- Visual confirmation of interactivity

### WebCloak Vulnerability

**Problem**: [[webcloak|WebCloak]]-style obfuscation
- Randomized tags: <button> → <aXb7f3>
- Randomized attributes: class="submit" → class="k3m9x"
- Adversarial text in HTML

**Failure mode**: Keyword matching breaks, 0% recall

**Current status**: Prune4Web partially mitigates parse-then-interpret (reduces DOM exposure) but doesn't eliminate vulnerability

**Mitigation** (future work):
- Client-side de-obfuscation (JavaScript restoration)
- Visual grounding (screenshot-based element identification)
- Behavioral analysis (interaction patterns)

### Multi-Lingual Support

**Current**: English-focused (PorterStemmer, keyword extraction)

**Future**: Extend to other languages
- Language-specific stemmers (French, German, Chinese, etc.)
- Multilingual fuzzy matching (Unicode normalization)
- Cross-lingual keyword generation (translation layer)

## Impact and Significance

### Efficiency Breakthrough

**Token reduction**: 10,000-100,000 → 50-200 tokens for filtering stage (50-500× reduction)

**Cost reduction**: 0.5B model vs GPT-4o (100× cheaper per page)

**Latency reduction**: <2 seconds vs 10-120 seconds (5-60× faster)

### Model Democratization

**Tiny models sufficient**: 0.5B achieves 97.64% recall@20
- Edge device deployment possible
- Low-cost inference at scale
- Energy efficient (green AI)
- Accessible to researchers with limited resources

### Paradigm Shift

**From**: "LLM processes everything"
**To**: "LLM generates program, program processes data"

**Broader implications**:
- Applicable beyond DOM filtering (SQL query generation, data extraction, etc.)
- Programmatic thinking for agents (meta-level control)
- Template-guided generation (constrain LLM to structured output)

### Foundation for Next-Gen Agents

**Enables**:
- Precise element localization (88.28% accuracy)
- Scalable web automation (millions of tasks)
- Production-ready deployment (low cost, low latency)
- Universal enhancement (plug-and-play for existing agents)

## See Also

### Core Prune4Web
- [[prune4web|Prune4Web]] - System overview
- [[prune4web-paper|Prune4Web Paper]] - Research paper
- [[dom-tree-pruning|DOM Tree Pruning]] - Overall paradigm
- [[two-turn-dialogue-training|Two-Turn Dialogue Training]] - Training strategy
- [[hierarchical-reward-rl|Hierarchical Reward RL]] - RFT innovation

### Related Concepts
- [[web-agent-token-efficiency|Web Agent Token Efficiency]] - Optimization focus
- [[multimodal-web-agents|Multimodal Web Agents]] - Framework category
- [[llm-driven-web-agents|LLM-Driven Web Agents]] - General category
- [[parse-then-interpret|Parse-Then-Interpret Mechanism]] - Vulnerability addressed (partial)

### Related Technologies
- [[webcloak|WebCloak]] - Defense that counters this technique
- [[autodata|AutoData]] - Multi-agent system (integration potential)
- [[webdancer|WebDancer]] - RL-trained agent (integration potential)
- [[multimodal-mind2web|Multimodal-Mind2Web]] - Source benchmark

### Authors and Institution
- [[jiayuan-zhang|Jiayuan Zhang]] - Co-first author
- [[kaiquan-chen|Kaiquan Chen]] - Co-first author
- [[jing-zhang-beihang|Jing Zhang]] - Corresponding author
- [[beihang-university|Beihang University]] - Institution

### Comparisons
- [[prune4web-vs-webcloak|Prune4Web vs WebCloak]] - Efficiency vs defense
- [[prune4web-vs-autodata|Prune4Web vs AutoData]] - Single-agent vs multi-agent
- [[prune4web-vs-webdancer|Prune4Web vs WebDancer]] - Programmatic vs end-to-end RL
- [[dom-pruning-approaches|DOM Pruning Approaches]] - Comparison of methods
