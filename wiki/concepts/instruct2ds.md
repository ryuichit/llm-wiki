---
title: Instruct2DS (Instruction-to-Dataset)
tags: [concept, benchmark, evaluation, dataset, web-scraping]
created: 2026-06-24
updated: 2026-06-24
---

# Instruct2DS (Instruction-to-Dataset)

The first comprehensive benchmark for evaluating automated web data collection systems, consisting of 237 webpages and 234 natural language tasks across three domains: Academic, Finance, and Sports.

## Overview

Instruct2DS addresses the lack of standardized evaluation for web data collection systems by providing real-world webpages, diverse tasks with natural language instructions, and ground truth data for quantitative assessment. Developed alongside [[autodata|AutoData]], it enables rigorous comparison of different data collection approaches.

## Benchmark Composition

### Scale

- **237 webpages** with diverse HTML structures
- **234 tasks** with natural language instructions
- **3 domains** covering different data types
- **Ground truth labels** for automated evaluation

### Domains

**Academic Domain (93 webpages)**:
- Conference paper listings
- Author profile pages
- Research metrics and citations
- Publication databases
- Academic institution pages

**Finance Domain (75 webpages)**:
- Stock price quotes
- Company information pages
- Market indices
- Financial news
- Earnings reports

**Sports Domain (69 webpages)**:
- Game scores and schedules
- Player statistics
- Team rankings
- League standings
- Sports news and updates

### Task Types

**Single-Page Extraction**:
- Extract specific data from one webpage
- Example: "Collect all paper titles from ICLR 2024 accepted papers"

**Multi-Page Crawling**:
- Navigate across multiple linked pages
- Example: "Gather player stats from NBA team rosters"

**API-Based Collection**:
- Use REST APIs to retrieve data
- Example: "Fetch real-time stock prices via Yahoo Finance API"

**Hybrid Approaches**:
- Combine web scraping and API calls
- Example: "Collect company profiles using web and API data"

**Complex Nested Structures**:
- Handle hierarchical and nested data
- Example: "Extract citation networks from survey papers"

## Benchmark Design

### Diversity Principles

**Structural Diversity**:
- Simple static HTML pages
- Complex dynamic JavaScript sites
- Various CSS frameworks (Bootstrap, Tailwind, custom)
- Different pagination patterns
- Multiple table layouts

**Content Diversity**:
- Text-heavy content
- Structured tables
- Lists and hierarchies
- Mixed media (text, numbers, links)
- Temporal data (dates, schedules)

**Complexity Diversity**:
- Simple: 1-3 data fields from single page
- Moderate: 5-10 fields, possibly multi-page
- Complex: 15+ fields, nested structures, multiple sources

### Real-World Characteristics

**Authentic Websites**:
- Actual production websites (captured at point in time)
- Not artificially constructed for testing
- Real-world messiness and inconsistencies
- Various HTML quality levels

**Practical Tasks**:
- Based on actual use cases
- Meaningful data collection scenarios
- Relevant to research and business needs
- Representative of common challenges

**Ground Truth Quality**:
- Manually verified correct answers
- Multiple annotators for validation
- Clear evaluation criteria
- Consistent labeling standards

## Evaluation Metrics

### Primary Metrics

**Precision**:
```
Precision = (True Positives) / (True Positives + False Positives)
```
- Measures accuracy of extracted data
- Penalizes incorrect extractions
- Important for data quality

**Recall**:
```
Recall = (True Positives) / (True Positives + False Negatives)
```
- Measures completeness of extraction
- Penalizes missing data
- Important for comprehensive collection

**F1 Score**:
```
F1 = 2 × (Precision × Recall) / (Precision + Recall)
```
- Harmonic mean of precision and recall
- Balanced measure of accuracy and completeness
- Primary evaluation metric for Instruct2DS

### Secondary Metrics

**Execution Time**:
- Time from instruction to final output
- Measures efficiency
- Important for practical deployment

**Cost**:
- LLM token costs for systems using language models
- Measures economic efficiency
- Critical for scalability

**Success Rate**:
- Percentage of tasks completed without errors
- Binary success/failure per task
- Measures reliability

## Benchmark Results

### AutoData Performance

**By Domain**:
- **Academic**: 91.85% F1
- **Finance**: 96.75% F1 (best performance)
- **Sports**: 90.14% F1
- **Average**: 92.91% F1

**Efficiency**:
- **Average time**: 5.58 minutes per task
- **Average cost**: $0.57 per task
- **Success rate**: >90% across domains

### Baseline Comparisons

**Human Programming**:
- **Performance**: 88.91% F1 average
- **Time**: Hours per task
- **AutoData advantage**: +4% F1, 10-100× faster

**Manus (Multi-Agent Coding System)**:
- **Performance**: Similar F1 to AutoData
- **Time**: 14.8 minutes average
- **Cost**: $2.48 per task
- **AutoData advantage**: 63% faster, 77% cheaper

**Cursor (AI Code Editor)**:
- **Performance**: ~80-85% F1 range
- **AutoData advantage**: +8-13% F1, more robust

**Cline (LLM Coding Assistant)**:
- **Performance**: ~75-82% F1 range
- **AutoData advantage**: +11-18% F1, better multi-step reasoning

**Single-Agent Baselines**:
- **Performance**: ~70-80% F1 range
- **AutoData advantage**: +13-23% F1 from multi-agent coordination

### Performance by Task Complexity

**Simple Tasks** (1-3 fields, single page):
- All systems: >95% F1
- Ceiling effect, less discriminative

**Moderate Tasks** (5-10 fields, possible multi-page):
- AutoData: ~92% F1
- Baselines: ~82-88% F1
- Good discrimination between approaches

**Complex Tasks** (15+ fields, nested, multi-source):
- AutoData: ~89% F1
- Baselines: ~65-80% F1
- Largest performance gap, most discriminative

## Benchmark Usage

### Evaluation Protocol

**Standard Setup**:
1. Provide task instruction in natural language
2. Allow system to execute autonomously
3. Compare output against ground truth
4. Calculate precision, recall, F1
5. Record time and cost

**Controlled Conditions**:
- Same LLM backend (GPT-4 class) for fair comparison
- Same network conditions
- Same evaluation script
- Consistent ground truth

### Comparison Guidelines

**Fair Comparison**:
- Same LLM model version
- Same computational resources
- Same time limits
- Same evaluation metrics

**Reporting Requirements**:
- Overall F1 and per-domain F1
- Time and cost statistics
- Success rate
- Failure analysis

### Extending the Benchmark

**Adding Domains**:
- Define new domain (e.g., E-commerce, Healthcare)
- Collect representative webpages
- Create diverse tasks with natural language instructions
- Manually create ground truth labels

**Adding Tasks**:
- Identify new data collection scenarios
- Write clear natural language instructions
- Ensure ground truth availability
- Test for ambiguity

## Dataset Characteristics

### Academic Domain Details

**Paper Pages** (35 webpages):
- Conference accepted papers
- Journal article listings
- Preprint repositories
- Example: "Extract all paper titles and authors from NeurIPS 2024"

**Author Profiles** (28 webpages):
- Google Scholar profiles
- University faculty pages
- Research group pages
- Example: "Collect h-index and publication count for Professor X"

**Metrics & Citations** (30 webpages):
- Citation counts
- Impact factors
- Altmetrics
- Example: "Get citation count and year for paper Y"

**Complexity**: Nested structures (papers → authors → affiliations)

### Finance Domain Details

**Stock Quotes** (32 webpages):
- Real-time prices
- Historical data
- Volume information
- Example: "Fetch current price, volume, and market cap for AAPL"

**Company Pages** (25 webpages):
- Company profiles
- Financial statements
- Executive information
- Example: "Extract CEO name, revenue, and employee count for Google"

**Market Indices** (18 webpages):
- Index values
- Constituent lists
- Performance metrics
- Example: "Get current S&P 500 value and top 10 gainers"

**Complexity**: Time-sensitive data, numerical precision important

### Sports Domain Details

**Game Scores** (28 webpages):
- Live scores
- Final scores
- Box scores
- Example: "Collect final score and top scorer for Lakers vs Celtics game"

**Player Stats** (23 webpages):
- Season statistics
- Career statistics
- Per-game averages
- Example: "Get points, rebounds, and assists for LeBron James this season"

**Standings & Rankings** (18 webpages):
- League standings
- Team rankings
- Playoff brackets
- Example: "Extract current NBA Western Conference standings"

**Complexity**: Tabular data, frequent updates

## Benchmark Significance

### Research Impact

**Standardization**:
- First benchmark specifically for web data collection
- Enables apples-to-apples comparison
- Facilitates reproducible research
- Establishes evaluation standards

**Progress Tracking**:
- Baseline for future improvements
- Quantifiable progress metrics
- Community-wide benchmarking
- Competitive evaluation

**Research Directions**:
- Identifies strengths and weaknesses of approaches
- Highlights challenging scenarios
- Motivates new techniques
- Guides future work

### Practical Impact

**System Comparison**:
- Choose best tool for specific domains
- Understand cost-performance tradeoffs
- Assess reliability for production use

**Development Guidance**:
- Identify areas needing improvement
- Test new features systematically
- Validate enhancements quantitatively

**Deployment Decisions**:
- Estimate costs for real-world use
- Assess suitability for use cases
- Plan resource allocation

## Limitations

### Current Scope

**Domain Coverage**:
- Only 3 domains (Academic, Finance, Sports)
- Missing: E-commerce, Healthcare, Government, Social Media
- Expansion needed for generalization

**Language Coverage**:
- Primarily English-language websites
- Limited non-English evaluation
- International websites underrepresented

**Temporal Aspects**:
- Static snapshot at creation time
- Websites evolve over time
- Benchmark aging concerns

**Scale**:
- 237 webpages is substantial but limited
- Could expand to thousands
- Trade-off with manual ground truth cost

### Evaluation Limitations

**Exact Match Bias**:
- Ground truth requires exact match
- May penalize acceptable variations
- Semantic equivalence not captured

**Static Ground Truth**:
- Cannot capture dynamic content evolution
- Real-time data presents challenges
- Manual updates needed

**Task Ambiguity**:
- Natural language instructions can be ambiguous
- Multiple valid interpretations possible
- Scoring may not capture all valid outputs

## Future Enhancements

### Benchmark Expansion

**More Domains**:
- E-commerce (products, prices, reviews)
- Healthcare (medical information, research)
- Government (public records, regulations)
- Social media (posts, profiles, trends)
- News (articles, headlines, metadata)

**More Tasks Per Domain**:
- Increase from ~75 to 200+ tasks per domain
- Cover more edge cases
- Test more sophisticated scenarios

**International Websites**:
- Non-English websites
- Different regional layouts
- Cultural variations in data presentation

### Evaluation Improvements

**Semantic Equivalence**:
- Allow semantically equivalent answers
- LLM-based evaluation for fuzzy matching
- Capture acceptable variations

**Dynamic Content**:
- Handle time-sensitive data
- Relative comparisons instead of absolute
- Update ground truth periodically

**Difficulty Calibration**:
- Precise difficulty ratings per task
- Stratified sampling by difficulty
- Normalized scoring by difficulty

### Accessibility Improvements

**Public Release**:
- Open-source benchmark data
- Evaluation scripts
- Baseline implementations
- Leaderboard platform

**Documentation**:
- Detailed task descriptions
- Usage guidelines
- Common pitfalls
- Best practices

## Comparison with Related Benchmarks

### vs LLMCrawlBench

[[llmcrawlbench|LLMCrawlBench]] (from [[webcloak-paper|WebCloak paper]]):
- **Focus**: Evaluating LLM-driven scrapers as threats
- **Size**: 237 pages, 10,895 images, 50 websites
- **Domains**: Marketplaces, Travel, Design, Lifestyle, Entertainment
- **Goal**: Test scraping capabilities for defense research

**Instruct2DS**:
- **Focus**: Evaluating data collection systems
- **Size**: 237 pages, 234 tasks, 3 domains
- **Domains**: Academic, Finance, Sports
- **Goal**: Benchmark for system comparison and improvement

**Complementary**:
- LLMCrawlBench: Scraper testing for security
- Instruct2DS: System evaluation for performance

### vs Traditional Scraping Benchmarks

**Traditional Benchmarks**:
- Often synthetic or limited scope
- Fixed evaluation metrics
- Code-based, not instruction-based

**Instruct2DS**:
- Real-world authentic websites
- Natural language instructions
- Evaluates end-to-end automation
- Multi-agent system friendly

## Related Pages

### Concepts

- [[autodata|AutoData]]
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]]
- [[ohcache|OHCache]]
- [[llmcrawlbench|LLMCrawlBench]]

### Entities

- [[university-of-notre-dame|University of Notre Dame]]
- [[yanfang-ye|Yanfang Ye]]
- [[autodata|AutoData Technology]]

### Comparisons

- [[autodata-vs-human-programming|AutoData vs Human Programming]]
- [[autodata-vs-single-agent|AutoData vs Single-Agent]]

### Sources

- [[autodata-paper|AutoData Paper]]

## Sources

- [[autodata-paper|AutoData Research Paper]] (NeurIPS 2025)
