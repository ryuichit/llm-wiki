---
title: LLMCrawlBench
tags: [concept, benchmark, evaluation, dataset, research]
created: 2026-06-24
updated: 2026-06-24
---

# LLMCrawlBench

A comprehensive benchmark dataset for evaluating [[llm-driven-web-agents|LLM-driven web scraping]] capabilities, consisting of 237 webpages with 10,895 images from 50 real-world websites across diverse categories.

## Overview

LLMCrawlBench was developed as part of the [[webcloak-paper|WebCloak research]] to systematically characterize the threat landscape of LLM-powered web scraping. It represents the first standardized evaluation framework for assessing scraping performance across different paradigms: [[llm-to-script|L2S]], [[llm-native-crawlers|LNC]], and [[llm-based-web-agents|LWA]].

## Dataset Composition

### Scale

- **237 webpages** with diverse structural patterns
- **10,895 images** including product photos, logos, graphics
- **50 real-world websites** from production environments
- **5 major categories** covering common web use cases

### Categories

**1. Marketplaces** (~80 pages)
- E-commerce product listings
- Shopping cart interfaces
- Product detail pages
- Review sections
- Price comparison pages

**2. Travel** (~50 pages)
- Flight booking interfaces
- Hotel search results
- Travel itineraries
- Destination guides
- Booking confirmation pages

**3. Design** (~40 pages)
- Portfolio websites
- Design showcases
- Template galleries
- Asset libraries
- Creative project pages

**4. Lifestyle** (~35 pages)
- Blog articles
- Recipe websites
- Fitness tracking interfaces
- Health information pages
- Personal homepages

**5. Entertainment** (~32 pages)
- Movie databases
- Music streaming interfaces
- Gaming news sites
- Event listings
- Media galleries

### Website Diversity

**Structural Variations**:
- Simple static HTML pages
- Complex JavaScript-heavy SPAs
- Server-side rendered applications
- Hybrid rendering approaches
- Various CSS frameworks (Bootstrap, Tailwind, custom)

**Content Types**:
- Text content (articles, descriptions)
- Structured data (prices, ratings, dates)
- Media (images, videos, audio)
- Interactive elements (forms, buttons, dropdowns)
- Dynamic content (lazy-loaded, infinite scroll)

**Layout Patterns**:
- Grid layouts (product galleries)
- List layouts (search results)
- Card-based designs
- Table structures
- Complex nested hierarchies

## Ground Truth Annotations

### Data Labeling

Each webpage includes manually verified ground truth for:

**1. Target Elements**
- Precise HTML selectors
- Expected content values
- Element relationships
- Structural context

**2. Extraction Targets**
- Product names and descriptions
- Prices and availability
- User-generated content (reviews, ratings)
- Metadata (dates, authors, categories)
- Images and media URLs

**3. Edge Cases**
- Missing optional fields
- Variant structures on same site
- Dynamic content variations
- Conditional elements

### Quality Assurance

**Validation Process**:
1. Manual extraction by human annotators
2. Cross-validation by multiple reviewers
3. Automated consistency checks
4. Iterative refinement
5. Final verification

**Inter-Annotator Agreement**: >95% for structural elements

## Evaluation Metrics

### Primary Metrics

**1. Recall**
```
Recall = (Correctly Extracted Items) / (Total Ground Truth Items)
```

Measures completeness of extraction:
- 100% = All target items extracted
- 0% = No items extracted
- Primary metric for [[webcloak-paper|WebCloak research]]

**2. Precision**
```
Precision = (Correctly Extracted Items) / (Total Extracted Items)
```

Measures accuracy of extraction:
- 100% = No false positives
- Low precision = Many incorrect extractions

**3. F1 Score**
```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```

Harmonic mean balancing completeness and accuracy

### Secondary Metrics

**Structural Accuracy**:
- Correct identification of element relationships
- Preservation of data hierarchy
- Proper handling of nested structures

**Content Fidelity**:
- Exact text matching
- Proper formatting preservation
- Correct data type extraction

**Robustness**:
- Handling of missing elements
- Graceful degradation on errors
- Consistency across similar pages

## Benchmark Results

### Baseline Performance (No Defense)

Tested 32 scraper variants across three paradigms:

**[[llm-to-script|LLM-to-Script (L2S)]]** (12 variants)
- Best: [[gemini|Gemini-2.5-pro]] at 84.2% recall
- GPT-4o: ~80% recall
- Claude-3.5-Sonnet: ~75% recall
- Llama-3-70B: ~65% recall
- Average: ~72% recall

**[[llm-native-crawlers|LLM-Native Crawlers (LNC)]]** (10 variants)
- Best: [[crawl4ai|Crawl4AI]] at 98% recall
- Firecrawl: ~92% recall
- Jina Reader: ~88% recall
- Scrapegraph-ai: ~85% recall
- Average: ~89% recall

**[[llm-based-web-agents|LLM-based Web Agents (LWA)]]** (10 variants)
- Best: [[browser-use|Browser-Use]] at 99.3% recall
- Skyvern: ~95% recall
- AutoCrawler: ~92% recall
- LaVague: ~88% recall
- Average: ~91% recall

**Overall Success Rate**: 84.4% (27 out of 32 variants achieved >70% recall)

### With WebCloak Defense

Performance after [[webcloak|WebCloak]] protection applied:

- **All 32 variants**: 0% recall
- **100% defense effectiveness**
- **User experience**: Preserved (no degradation)
- **Performance overhead**: ~52ms client-side

### Key Findings

**1. All Paradigms Pose Real Threats**
- Minimum successful recall: 65%
- Top performers: 84.2% to 99.3%
- Consistent effectiveness across categories

**2. Democratization Validated**
- Novices with LLM tools: 71% average recall
- Expert manual scrapers: 78% average recall
- Gap: Only 7 percentage points

**3. LWA Supremacy**
- Highest overall performance (99.3%)
- Best handling of dynamic content
- Most robust across edge cases

**4. Traditional Defenses Ineffective**
- Rate limiting bypassed
- IP blocking circumvented
- CAPTCHAs solved by agents
- JavaScript obfuscation insufficient

## Usage in Research

### Evaluation Protocol

**Standard Testing Procedure**:
```python
# Pseudo-code for benchmark evaluation
def evaluate_scraper(scraper, benchmark=LLMCrawlBench):
    results = []
    
    for page in benchmark.pages:
        # Run scraper on page
        extracted = scraper.scrape(page.url)
        
        # Compare with ground truth
        ground_truth = page.ground_truth
        
        # Calculate metrics
        precision = calculate_precision(extracted, ground_truth)
        recall = calculate_recall(extracted, ground_truth)
        f1 = calculate_f1(precision, recall)
        
        results.append({
            'page': page.id,
            'precision': precision,
            'recall': recall,
            'f1': f1
        })
    
    return aggregate_results(results)
```

### Reproducibility

**Public Availability**:
- Benchmark dataset publicly accessible
- Ground truth annotations included
- Evaluation scripts provided
- Baseline results documented

**Standardization Benefits**:
- Consistent comparison across research
- Reproducible results
- Fair evaluation methodology
- Community validation

## Design Principles

### 1. Real-World Relevance

**Authentic Websites**:
- Production environments, not synthetic pages
- Real user interfaces and experiences
- Actual business-critical content
- Representative of threat landscape

**Practical Scenarios**:
- Common scraping targets
- Valuable data types
- High-stakes use cases
- Diverse difficulty levels

### 2. Comprehensive Coverage

**Structural Diversity**:
- Various HTML patterns
- Different CSS frameworks
- Multiple rendering approaches
- Range of complexity levels

**Content Variety**:
- Text, images, structured data
- Static and dynamic elements
- Simple and nested hierarchies
- Complete and partial information

### 3. Scalability

**Manageable Size**:
- 237 pages: Large enough for statistical validity
- Small enough for rapid iteration
- Balanced across categories
- Representative sampling

**Efficient Evaluation**:
- Automated ground truth comparison
- Fast metric calculation
- Parallel testing support
- Low computational requirements

### 4. Defense Validation

**Protection Testing**:
- Same benchmark before/after defense
- Direct performance comparison
- Quantifiable effectiveness
- User experience validation

**Robustness Assessment**:
- Multiple attack variants
- Different LLM capabilities
- Various tool implementations
- Adaptive attack attempts

## Limitations and Future Work

### Current Limitations

**1. Temporal Snapshot**
- Websites evolve over time
- Structures may change
- Content updates regularly
- Maintenance required

**2. Category Coverage**
- Focus on common domains
- Some niches underrepresented
- Specific use cases missing
- Potential bias in selection

**3. Language Scope**
- Primarily English content
- Limited multilingual coverage
- International sites underrepresented

**4. Technical Constraints**
- Static ground truth (not dynamic content variations)
- Limited edge case coverage
- Specific to web scraping (not other agent tasks)

### Future Enhancements

**Expansion**:
- Additional categories (social media, forums, dashboards)
- More websites per category
- Multilingual pages
- Mobile-optimized sites

**Dynamism**:
- Live content variations
- Session-dependent pages
- User authentication scenarios
- Real-time data extraction

**Complexity**:
- Multi-page workflows
- Form submission tasks
- Search and filter operations
- Cross-site data aggregation

**Adversarial Testing**:
- Defended pages included
- Various protection mechanisms
- Adaptive attack scenarios
- Arms race simulation

## Impact on Security Research

### Contributions

**1. Quantified Threat Level**
- First systematic measurement of LLM scraping capabilities
- Validated democratization hypothesis
- Established baseline performance metrics
- Identified most dangerous paradigms

**2. Defense Evaluation Framework**
- Standardized testing for protection mechanisms
- Before/after comparison methodology
- User experience validation protocol
- Reproducible effectiveness measurement

**3. Community Resource**
- Public benchmark for future research
- Comparison baseline for new defenses
- Evaluation standard for scraping tools
- Educational resource for security training

### Research Enabled

**Follow-up Studies**:
- Comparative analysis of new LLM models
- Novel defense mechanism evaluation
- Scraping democratization studies
- Legal and ethical framework development

**Industry Impact**:
- Content protection strategy validation
- Risk assessment for web properties
- Defense deployment justification
- Cost-benefit analysis foundation

## Related Pages

### Concepts
- [[llm-driven-web-agents|LLM-Driven Web Agents]]
- [[llm-to-script|LLM-to-Script (L2S)]]
- [[llm-native-crawlers|LLM-Native Crawlers (LNC)]]
- [[llm-based-web-agents|LLM-based Web Agents (LWA)]]
- [[parse-then-interpret|Parse-Then-Interpret Mechanism]]
- [[webcloak|WebCloak]]
- [[dynamic-structural-obfuscation|Dynamic Structural Obfuscation]]
- [[semantic-labyrinth|Semantic Labyrinth]]

### Entities
- [[nanyang-technological-university|Nanyang Technological University]]
- [[xinfeng-li|Xinfeng Li]]
- [[wei-dong|Wei Dong]]
- [[gemini|Gemini]]
- [[crawl4ai|Crawl4AI]]
- [[browser-use|Browser-Use]]

### Sources
- [[webcloak-paper|WebCloak Research Paper]]

## Sources

- [[webcloak-paper|WebCloak Paper]]
