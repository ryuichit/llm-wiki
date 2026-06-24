---
title: CRAWLQA
tags: [entity, technology, dataset]
created: 2026-06-24
updated: 2026-06-24
---

# CRAWLQA

A web crawling-based question-answer dataset synthesis method developed by [[tongyi-lab|Tongyi Lab]] for training [[webdancer|WebDancer]], the first end-to-end trained autonomous information-seeking agent. CRAWLQA generates 60,000 training samples by crawling diverse web sources and constructing questions that require multi-hop reasoning and click actions.

## Overview

CRAWLQA (Crawling-based Question-Answer generation) addresses the challenge of creating training data for information-seeking agents without human annotation. By systematically crawling web pages from academic, technical, and encyclopedic sources, CRAWLQA generates question-answer pairs that naturally require the kind of multi-step web navigation that research agents must perform.

## Dataset Composition

### Scale

**Size**: 60,000 question-answer pairs

**Distribution**:
- Academic sources (arxiv): ~15,000 samples
- Technical sources (github, stackoverflow): ~20,000 samples
- Encyclopedic sources (wikipedia): ~20,000 samples
- Other web sources: ~5,000 samples

### Source Domains

**ArXiv (Academic Papers)**:
- Research paper abstracts and content
- Technical methodology descriptions
- Scientific findings and results
- Author affiliations and citations
- Publication metadata

**GitHub (Code Repositories)**:
- Repository documentation
- README files and wikis
- Issue discussions
- Code comments and docstrings
- Project metadata

**Wikipedia (Encyclopedia)**:
- Article content across diverse topics
- Infoboxes and structured data
- Cross-references and citations
- Historical information
- Biographical data

**Stack Overflow (Technical Q&A)**:
- Programming questions
- Solution explanations
- Technical discussions
- Best practices
- Framework documentation

## Construction Process

### Stage 1: Web Crawling

**Crawling Strategy**:
```
For each source domain:
  1. Identify seed URLs (topic pages, popular repositories, etc.)
  2. Crawl pages systematically
  3. Extract structured content
  4. Filter low-quality pages
  5. Store page hierarchy and links
```

**Content Extraction**:
- **Title extraction**: Page/article titles
- **Body extraction**: Main content text
- **Metadata extraction**: Authors, dates, categories
- **Link extraction**: Internal and external references
- **Structure preservation**: Headings, sections, lists

**Quality Filtering**:
- Minimum content length threshold
- Readability score requirements
- Remove duplicate or near-duplicate pages
- Filter spam and low-quality content
- Verify page accessibility

### Stage 2: Hierarchical Information Extraction

**Three-Level Hierarchy**:

**Level 1 - Title**:
- Extracted from page title or main heading
- Provides topic context
- Seeds question generation

**Level 2 - Body**:
- Main content paragraphs
- Key facts and information
- Supporting details
- Contextual information

**Level 3 - Reasoning**:
- Connections between facts
- Multi-hop inference requirements
- Cross-page relationships
- Deep understanding needs

### Stage 3: Question Generation

**Question Types**:

**Factual Questions**:
- "What is the affiliation of [author] in [paper]?"
- "When was [repository] created?"
- "Who founded [organization]?"

**Multi-Hop Questions**:
- "What technique did [author] use in their [year] paper?"
- "Which paper introduced the concept used in [framework]?"
- "How does [method A] compare to [method B] proposed in [paper]?"

**Deep Reasoning Questions**:
- "What problem does [system] solve that [prior-system] couldn't?"
- "Why did [author] choose [approach] over alternatives?"
- "What are the implications of [finding] for [domain]?"

**Generation Process**:
```
For each crawled page:
  1. Extract key entities (people, places, concepts, dates)
  2. Identify relationships between entities
  3. Generate candidate questions requiring multi-hop reasoning
  4. Filter questions by:
     - Answerability from web content
     - Requirement for click actions (not just search)
     - Reasonable difficulty level
     - Clear unambiguous answers
  5. Create question-answer pairs with source URLs
```

### Stage 4: Answer Verification

**Verification Methods**:

**Ground Truth Extraction**:
- Extract answer from source page
- Verify answer appears in page content
- Check answer completeness
- Ensure factual correctness

**Multi-Source Validation**:
- Cross-reference answers across multiple pages
- Verify consistency of information
- Check for conflicting information
- Prioritize authoritative sources

**Quality Assessment**:
- LLM-based answer quality evaluation
- Completeness check
- Factual accuracy verification
- Relevance to question

### Stage 5: Trajectory Annotation

**Multi-Step Trajectory Creation**:

Each question-answer pair is enhanced with:

**Required Actions**:
- Search queries needed
- URLs to visit
- Information extraction steps
- Synthesis requirements

**Example Trajectory**:
```
Question: "What university is the lead author of the WebDancer paper affiliated with?"

Action 1: search("WebDancer paper lead author")
→ Find paper page or author list

Action 2: visit(paper_url)
→ Extract author names and affiliations

Action 3: answer("Tongyi Lab, Alibaba Group")
→ Provide final answer
```

**Trajectory Characteristics**:
- Average 3-5 actions per question
- Requires both search and visit actions
- Multi-page information gathering
- Answer synthesis from multiple sources

## Key Design Principles

### Multi-Hop Reasoning Requirements

**Principle**: Questions must require navigating multiple web pages

**Implementation**:
- Questions cannot be answered from search results alone
- Require visiting specific pages
- Need information synthesis across sources
- Involve entity disambiguation

**Example**:
- Simple (excluded): "What is Python?" - answerable from search results
- Multi-hop (included): "What optimization algorithm was used in the Python implementation of [specific paper]?" - requires finding paper, then code

### Deep Query Construction

**Principle**: Questions should require understanding and planning

**Click Action Necessity**:
- Questions designed so answers are not in search snippets
- Must visit pages to extract detailed information
- Require navigation through page hierarchies
- Involve cross-referencing

**Query Complexity**:
- Avoid simple factual lookups
- Emphasize reasoning and synthesis
- Require contextual understanding
- Test information-seeking strategies

### Diverse Domain Coverage

**Principle**: Broad topic distribution for generalization

**Domain Balance**:
- Academic: Research papers, conferences, institutions
- Technical: Software, frameworks, programming concepts
- Encyclopedic: Historical events, biographies, organizations
- Professional: Industry practices, company information

**Topic Diversity**:
- Computer science and AI (30%)
- Natural sciences (20%)
- Engineering and technology (20%)
- Social sciences and humanities (15%)
- Other domains (15%)

## Dataset Statistics

### Question Characteristics

**Length Distribution**:
- Average question length: 18 words
- Median: 16 words
- Range: 8-40 words
- Most common: 12-20 words (65%)

**Complexity Levels**:
- Easy (2-3 hops): 35%
- Medium (4-5 hops): 45%
- Hard (6+ hops): 20%

**Question Types**:
- What/Which: 40%
- Who/Where/When: 25%
- How: 20%
- Why: 10%
- Other: 5%

### Answer Characteristics

**Length Distribution**:
- Average answer length: 12 words
- Median: 8 words
- Range: 1-50 words
- Most common: 5-15 words (70%)

**Answer Types**:
- Named entities: 45%
- Numerical values: 15%
- Short phrases: 25%
- Descriptive sentences: 15%

### Source Characteristics

**Pages per Question**:
- Average: 2.3 pages required
- Median: 2 pages
- Range: 1-5 pages
- Most common: 2 pages (55%)

**Action Requirements**:
- Average actions: 3.8
- Search actions: 1.5 avg
- Visit actions: 2.3 avg
- Answer action: 1 (terminal)

## Complementarity with E2HQA

### Different Approaches

**CRAWLQA**:
- Bottom-up: Start from web pages
- Natural information distribution
- Real-world web structure
- Diverse but uncontrolled complexity

**[[e2hqa|E2HQA]]**:
- Top-down: Start from simple questions
- Controlled complexity progression
- Synthetic refinement process
- Systematic difficulty increase

### Combined Benefits

**Coverage**:
- CRAWLQA: Diverse domains and natural questions
- E2HQA: Systematic complexity and question types
- Together: Comprehensive training distribution

**Difficulty Spectrum**:
- E2HQA provides easy-to-medium progression
- CRAWLQA adds naturally complex questions
- Balanced difficulty distribution

**Training Dynamics**:
- E2HQA: Foundation and basic reasoning
- CRAWLQA: Realistic web navigation
- Together: Robust information-seeking behavior

## Use in WebDancer Training

### Role in Training Pipeline

**Stage 1: Question Generation**:
- CRAWLQA provides 60K questions
- Combined with E2HQA's 40K questions
- Total: 100K training questions

**Stage 2: Trajectory Sampling**:
- GPT-4o samples Short-CoT trajectories
- QwQ-Plus samples Long-CoT trajectories
- Both use CRAWLQA questions

**Stage 3: Multi-Stage Filtering**:
- Validity control: ReAct format
- Correctness verification: Answer accuracy
- Quality assessment: Trajectory quality
- Results in ~7K high-quality trajectories from CRAWLQA

**Stage 4: SFT Training**:
- Train base model on filtered trajectories
- Learn ReAct framework
- Acquire basic information-seeking behavior
- Essential for cold-start initialization

**Stage 5: RL Fine-Tuning**:
- On-policy trajectory collection using CRAWLQA questions
- DAPO algorithm optimization
- Improve generalization beyond training distribution

### Impact on Performance

**GAIA Benchmark**:
- CRAWLQA's complex questions prepare agent for GAIA
- Multi-hop reasoning practice
- Academic domain coverage
- 51.5% overall accuracy on GAIA

**WebWalkerQA Benchmark**:
- Web navigation practice from CRAWLQA
- Click action requirements
- Multi-page information gathering
- 47.9% overall accuracy

**Generalization**:
- Diverse domain coverage enables transfer
- Natural web structure familiarity
- Robust to different web environments

## Technical Implementation

### Crawling Infrastructure

**Architecture**:
- Distributed crawling system
- Rate limiting and politeness policies
- Respect robots.txt
- User-agent identification
- Error handling and retry logic

**Storage**:
- Structured page storage
- Metadata indexing
- Link graph preservation
- Version control for updates

**Processing Pipeline**:
```
Web Crawling → Content Extraction → Quality Filtering →
Question Generation → Answer Verification → Trajectory Annotation →
Final Dataset
```

### Quality Control

**Automated Checks**:
- Content length validation
- Answer extractability verification
- Question clarity assessment
- Duplicate detection

**LLM-Based Evaluation**:
- Question naturalness
- Answer correctness
- Reasoning depth required
- Overall quality scoring

**Manual Sampling**:
- Random sample inspection (5%)
- Quality metric validation
- Edge case identification
- Continuous improvement feedback

## Limitations

### Current Constraints

**Temporal Limitations**:
- Crawled content becomes stale
- Web pages change or disappear
- Information becomes outdated
- Requires periodic re-crawling

**Domain Limitations**:
- English-only content
- Text-focused (limited multimedia)
- Bias toward technical/academic domains
- Limited coverage of emerging topics

**Quality Variations**:
- Source quality varies
- Some questions harder than intended
- Answer ambiguity in some cases
- Multi-hop requirement not always verified

### Known Issues

**Web Dynamics**:
- URLs may become invalid
- Page content changes
- Search results vary
- Temporal knowledge gaps

**Question Quality**:
- Some questions too easy/hard
- Occasional ambiguity
- Answer extractability varies
- Reasoning depth inconsistent

## Future Improvements

### Enhanced Crawling

**Broader Coverage**:
- More diverse domains
- International sources
- Multilingual content
- Emerging topic tracking

**Deeper Extraction**:
- Multimodal content (images, tables)
- Interactive elements
- Dynamic content
- Structured data

### Better Question Generation

**Advanced Generation**:
- More sophisticated reasoning requirements
- Adversarial difficulty control
- Systematic complexity levels
- Question type balance

**Quality Enhancement**:
- Better answer verification
- Ambiguity detection
- Difficulty calibration
- Automated quality scoring

### Dynamic Updates

**Continuous Crawling**:
- Regular content updates
- New source addition
- Stale content detection
- Version management

**Adaptive Sampling**:
- Identify weak areas
- Generate targeted questions
- Balance difficulty distribution
- Curriculum learning support

## Related Pages

### Entities

**Technologies**:
- [[webdancer|WebDancer]]
- [[e2hqa|E2HQA]]
- [[dapo|DAPO Algorithm]]

**Organizations**:
- [[tongyi-lab|Tongyi Lab]]
- [[alibaba-group|Alibaba Group]]

### Concepts

**Data Synthesis**:
- [[synthetic-agent-training-data|Synthetic Agent Training Data]]
- [[easy-to-hard-qa|Easy-to-Hard QA Generation]]
- [[multi-hop-reasoning|Multi-Hop Reasoning]]

**Training Methods**:
- [[two-stage-agent-training|Two-Stage Agent Training]]
- [[rejection-sampling-finetuning|Rejection Sampling Fine-Tuning]]
- [[agentic-rl|Agentic Reinforcement Learning]]

**Agent Architecture**:
- [[information-seeking-agents|Information-Seeking Agents]]
- [[react-framework|ReAct Framework]]

### Sources

- [[webdancer-paper|WebDancer Paper]]

## Sources

- [[webdancer-paper|WebDancer Research Paper]] (NeurIPS 2025)
- github.com/Alibaba-NLP/DeepResearch
