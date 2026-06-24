# Crawler Generation

## Definition
The task of automatically generating executable rules or action sequences (crawlers) that can extract target information from web pages, using large language models instead of manual programming. Core paradigm introduced by AUTOCRAWLER framework.

## Overview
Crawler generation bridges the gap between traditional web scraping approaches (manual wrappers) and pure generative agents. It leverages LLMs to create reusable extraction rules that can be applied across multiple similar web pages without requiring repeated LLM inference.

## Problem Formulation

### Task Definition
Given:
- **W**: Set of webpages on the same website
- **s**: Subject entity (topic entity)
- **r ∈ R**: Predefined target attribute
- **Goal**: Generate executable action sequence **A** to extract target information **o** from all webpages

### Action Sequence Format
```
Aseq = [XPath1, XPath2, ..., XPathn]
```

Where:
- XPath1 to XPathn-1: Pruning operations (simplify HTML)
- XPathn: Extraction operation (get target value)
- Each XPath executed sequentially by parser

## Motivation

### Limitations of Traditional Approaches

#### Manual Wrappers
- **Problem**: Require 1,400+ webpage annotations per website
- **Issue**: Limited adaptability to new websites
- **Constraint**: Closed-world scenario only
- **Effort**: Heavy human involvement

#### Generative Agents
- **Problem**: Poor performance (~2% success rate on open-world tasks)
- **Issue**: No reusability across similar pages
- **Constraint**: LLM dependency for every webpage
- **Efficiency**: Low throughput on large-scale tasks

### Advantages of Crawler Generation

1. **Adaptability**: Quickly adjusts to different websites
2. **Reusability**: One crawler works for many similar pages
3. **Efficiency**: Reduces LLM calls after initial generation
4. **Performance**: Higher success rates than pure agents
5. **Scalability**: Better for large-scale extraction tasks

## Methodology

### Stage 1: Progressive Generation
```
Input: Seed webpages, task instruction
Process:
  1. Top-down: Generate XPath to target element
  2. Execute: Test XPath on HTML
  3. Validate: Check if extraction matches expected
  4. Step-back: If failed, move up DOM tree
  5. Repeat: Until successful or max retries
Output: Executable action sequence
```

### Stage 2: Synthesis
```
Input: ns seed webpages (typically 3)
Process:
  1. Generate action sequence for each seed
  2. Execute all sequences on all seeds
  3. Collect results from each sequence
  4. Select sequence that extracts correctly from all seeds
Output: Generalized action sequence
```

## Evaluation Framework

### Executable Evaluation (Novel Metric)
Unlike traditional IE metrics, focuses on generalizability across webpage collections:

1. **Correct** (↑): Precision, Recall, F1 all = 1.0
2. **Precision**: Only precision = 1.0 (missed some instances)
3. **Recall**: Only recall = 1.0 (some false positives)
4. **Unexecutable** (↓): Recall = 0 (complete failure)
5. **Over-estimate**: Precision = 0 (extracts when should be empty)
6. **Else**: Partial extraction situations

### Traditional IE Evaluation
- **Precision**: Accuracy of extracted instances
- **Recall**: Completeness of extraction
- **F1**: Harmonic mean
- **Limitation**: Doesn't measure transferability

## Performance Results

### SWDE Dataset (320 test cases, 61,566 webpages)

**GPT-4 + AUTOCRAWLER:**
- Correct: 71.56%
- Unexecutable: 4.06%
- F1: 88.69%

**Mixtral 8x7B + AUTOCRAWLER:**
- Correct: 46.88%
- Unexecutable: 30.31%
- F1: 59.75%

**CodeLlama + AUTOCRAWLER:**
- Correct: 23.99%
- Unexecutable: 64.94%
- F1: 28.41%

### Efficiency Analysis

**Threshold for Efficiency:**
```
NW ≥ (ns × Tg + Ts) / (Td - Te)
```

Where:
- NW: Number of webpages
- ns: Seed webpages (3)
- Tg: Time to generate wrapper (~5 × Td)
- Ts: Synthesis time (~Td)
- Td: Direct extraction time
- Te: Parser execution time (Td >> Te)

**Result**: Crawler generation more efficient when website has >16 pages

## Key Challenges

### XPath Fragility
Generated XPath expressions can be brittle:
- **Text predicates**: "contains(text(), 'value')" breaks when text changes
- **Position predicates**: [1], [2] break when structure changes
- **Attribute predicates**: @class sensitive to CSS changes

**Fragility Rates (GPT-4 on SWDE):**
- Contains predicate: 0.61% bad cases
- Equal predicate: 2.90% bad cases

### Multi-valued Information
Difficulty extracting when target appears in multiple locations:
- Address in header, footer, and content
- Phone number in multiple formats
- Prices with/without currency symbols

### Non-generalizability
Structural variations across pages prevent universal rules:
- Some pages have target in different DOM locations
- "Not Available" shown differently than actual values
- Optional fields present on some pages only

### LLM Capability Requirements
Small models struggle significantly:
- Mistral 7B: 97.19% unexecutable
- GPT-4: Only 4.06% unexecutable
- Requires strong structural understanding

## Comparison with Alternatives

### vs. AutoData
- **AutoData**: Multi-agent system, each agent specialized
- **AUTOCRAWLER**: Single agent with progressive understanding
- **Difference**: AUTOCRAWLER focuses on reusable crawler generation, AutoData on collaborative extraction

### vs. WebDancer
- **WebDancer**: Information seeking agent, interactive exploration
- **AUTOCRAWLER**: Crawler generation, creates reusable rules
- **Difference**: WebDancer navigates dynamically, AUTOCRAWLER pre-generates extraction logic

### vs. Manual Programming
- **Manual**: Write BeautifulSoup/Scrapy code for each site
- **AUTOCRAWLER**: LLM generates XPath rules automatically
- **Advantage**: 10-100x faster development time

### vs. Supervised Learning
AUTOCRAWLER (zero-shot) beats most supervised baselines:
- Render-Full: 84.30% F1
- SimpDOM: 83.06% F1
- MarkupLMBASE: 84.31% F1
- AUTOCRAWLER + GPT-4: 88.69% F1

## Use Cases

### E-commerce
- Product information across competitors
- Price monitoring from multiple sites
- Review aggregation
- Inventory tracking

### Real Estate
- Property listings across platforms
- Price comparisons
- Feature extraction
- Contact information

### Job Boards
- Position details from multiple sites
- Salary information
- Company profiles
- Application requirements

### News Aggregation
- Article content extraction
- Author and date information
- Category classification
- Related links

### Sports Statistics
- Player stats across leagues
- Game scores and schedules
- Team information
- Historical data

## Implementation Considerations

### Preprocessing Steps
1. Remove `<script>` and `<style>` tags
2. Keep only `@class` attributes (remove others)
3. Replace escape characters for consistency
4. Normalize text representations

### Parameter Tuning
- **dmax**: Maximum retry times (5 recommended)
- **ns**: Seed webpages (3 recommended)
- **Timeout**: XPath execution limits
- **Validation**: Strictness of match checking

### Model Selection
**Recommended for Production:**
- GPT-4: Best performance, highest cost
- Mixtral 8x7B: Good balance, lower cost
- GPT-3.5-Turbo: Acceptable performance, moderate cost

**Not Recommended:**
- Models <7B parameters: Too many failures
- Mistral 7B: Insufficient structural understanding

## Limitations

### Scope Constraints
- **Focus**: Vertical information pages only
- **Not suitable**: Complex interactive sites, SPAs, JavaScript-heavy pages
- **Not suitable**: Mind2Web, WebArena type environments

### Structural Requirements
- **Needs**: Consistent DOM structure across pages
- **Struggles with**: Highly variable layouts
- **Fails on**: Completely different structures per page

### Maintenance Issues
- **Website updates**: May break generated crawlers
- **Dynamic content**: Requires different approaches
- **Authentication**: Not handled by current approach

## Future Directions

### Enhanced HTML Understanding
1. **Specialized pre-training**: LLMs trained on HTML corpus
2. **Multi-modal**: Combine with rendered page screenshots
3. **Structural embeddings**: Encode DOM tree relationships

### Robustness Improvements
1. **Robust XPath**: Generate more flexible predicates
2. **Self-repair**: Detect and fix broken crawlers
3. **Structural anchors**: Use stable page elements

### Scope Extension
1. **Interactive pages**: Handle JavaScript and AJAX
2. **Multi-page flows**: Navigate through pagination
3. **Authentication**: Login and session management
4. **Complex environments**: WebArena, Mind2Web tasks

## Related Concepts
- [[progressive-understanding|Progressive Understanding]] - Core mechanism
- [[web-automation|Web Automation]] - Broader context
- [[information-extraction|Information Extraction]] - End goal
- [[xpath|XPath]] - Query language used
- [[dom-tree|DOM Tree]] - Structure exploited

## Related Technologies
- [[autocrawler|AUTOCRAWLER]] - Primary implementation
- [[autodata|AutoData]] - Multi-agent alternative
- [[webdancer|WebDancer]] - Interactive alternative
- [[scrapy|Scrapy]] - Traditional manual framework
- [[beautifulsoup|BeautifulSoup]] - HTML parsing library

## Tags
#concept #crawler-generation #web-scraping #automation #llm #information-extraction #xpath #reusability
