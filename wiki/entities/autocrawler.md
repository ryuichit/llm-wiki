# AUTOCRAWLER

## Overview
AUTOCRAWLER is a two-stage web automation framework that generates executable web crawlers using large language models (LLMs). It leverages progressive understanding of HTML hierarchical structure to automatically create action sequences for extracting information from vertical web pages.

## Category
Technology / Framework

## Key Characteristics

### Core Innovation
Progressive understanding mechanism that combines:
- **Top-down operations**: Refining from root to target nodes
- **Step-back operations**: Moving up DOM tree when execution fails
- **Iterative HTML pruning**: Continuously removing irrelevant content

### Two-Stage Architecture

#### Stage 1: Progressive Generation
- Generates action sequences using DOM tree traversal
- LLM creates XPath expressions step-by-step
- Validates execution and learns from errors
- Maximum retry limit (dmax = 5)

#### Stage 2: Synthesis
- Uses multiple seed webpages (ns = 3)
- Generates separate sequences for each seed
- Selects best sequence that works across all seeds
- Enhances cross-page generalizability

## Technical Details

### Action Sequence Format
```
Aseq = [XPath1, XPath2, ..., XPathn]
```
- All XPaths except last: pruning operations
- Last XPath: extraction operation

### Performance Metrics (SWDE Dataset)

**With GPT-4:**
- Correct: 71.56%
- Unexecutable: 4.06%
- F1 Score: 88.69%
- Average steps: 1.57

**With Mixtral 8x7B:**
- Correct: 46.88%
- Unexecutable: 30.31%
- F1 Score: 59.75%
- Average steps: 3.00

### Efficiency Threshold
Becomes more efficient than direct LLM extraction when:
- Website contains >16 webpages
- Based on formula: NW ≥ (ns × Tg + Ts) / (Td - Te)

## Advantages

1. **Reusability**: Generates reusable crawlers, unlike pure generative agents
2. **Adaptability**: Quickly adjusts to different websites without manual annotation
3. **Efficiency**: Reduces LLM dependency for repetitive tasks
4. **Learning**: Corrects errors through step-back mechanism
5. **Generalization**: Synthesis phase improves cross-page applicability

## Limitations

1. **Scope**: Limited to vertical information extraction tasks
2. **LLM Dependency**: Performance tied to backbone LLM capability
3. **XPath Fragility**: Generated paths can break on structural changes
4. **Multi-valued Issues**: Struggles with information in multiple locations
5. **Non-generalizability**: Structure variations limit universal applicability

## Comparison with Alternatives

### vs. Traditional Wrappers
- **Pro**: No manual annotation required
- **Pro**: Fast adaptation to new websites
- **Con**: Still requires some seed webpages

### vs. Generative Agents
- **Pro**: 35x better success rate (71.56% vs ~2%)
- **Pro**: Reusable across similar pages
- **Pro**: Lower operational cost after initial generation

### vs. COT Prompting
- **Pro**: 10-25% higher correct ratio
- **Pro**: 10-39% lower unexecutable ratio
- **Pro**: Learning from execution failures

### vs. Reflexion
- **Pro**: Active DOM pruning vs reactive reflection
- **Pro**: 4-6% improvement in correct ratio
- **Pro**: Lower token consumption through pruning

## Use Cases

1. **Vertical Information Extraction**: Player stats, product details, job listings
2. **Multi-site Data Collection**: Extracting same attributes across competing websites
3. **Large-scale Web Scraping**: When many pages share similar structure
4. **Structured Data Harvesting**: E-commerce, real estate, job boards, news sites

## Implementation Details

### Algorithm Parameters
- **dmax**: Maximum retry times (default: 5)
- **ns**: Number of seed webpages (default: 3)
- **Preprocessing**: Remove `<script>` and `<style>` tags, keep only `@class` attributes

### Supported Models
**Closed-source:**
- GPT-4 (best performer)
- GPT-3.5-Turbo
- Gemini Pro

**Open-source:**
- Mixtral 8x7B (best open-source)
- Deepseek-Coder 33B
- CodeLlama 34B
- Mistral 7B

## Research Context

**Developed by**: Fudan University + Alibaba collaboration

**Research Problem**: Bridge gap between:
- Manual wrappers (high effort, low adaptability)
- Generative agents (poor performance, no reusability)

**Evaluation**: Three datasets
- SWDE: 320 test cases, 61,566 webpages
- Extended SWDE: 294 test cases
- DS1: 83 test cases

## Related Technologies
- [[xpath|XPath]] - Query language for XML/HTML
- [[dom-tree|DOM Tree]] - Document Object Model structure
- [[web-automation|Web Automation]] - Programmatic web interaction
- [[llm-agents|LLM Agents]] - Language model-based autonomous agents
- [[progressive-understanding|Progressive Understanding]] - Incremental comprehension approach

## Related Papers
- [[autodata-paper|AutoData]] - Multi-agent approach to web data extraction
- [[webdancer-paper|WebDancer]] - Information seeking web agent
- [[mind2web-paper|Mind2Web]] - Generalist web agent
- [[webarena-paper|WebArena]] - Realistic web environment for agents

## Resources
- **Repository**: https://github.com/EZ-hwh/AutoCrawler
- **Paper**: arXiv:2404.12753v1
- **Source**: [[autocrawler-paper|AUTOCRAWLER Paper]]

## Tags
#framework #web-automation #llm-agent #crawler-generation #progressive-understanding #information-extraction
