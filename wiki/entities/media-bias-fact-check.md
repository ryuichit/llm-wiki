---
title: Media Bias/Fact Check
tags: [entity, organization]
created: 2026-06-24
updated: 2026-06-24
---

# Media Bias/Fact Check

Independent fact-checking and media monitoring organization that provides credibility ratings and bias assessments for news sources worldwide, used extensively in academic research on misinformation and web content quality.

## Overview

**Type**: Independent fact-checking organization

**Abbreviation**: MBFC

**Focus**: Source credibility classification, media bias assessment, misinformation tracking

**Usage**: Academic research, media literacy education, content quality evaluation

## Classification System

### Credibility Ratings

**Source Categories**:
- **Pro-Science**: Evidence-based, scientifically rigorous reporting
- **Least Biased**: Minimal editorial bias, objective reporting
- **Left-Center**: Slightly left-leaning but factual
- **Right-Center**: Slightly right-leaning but factual
- **Questionable Source**: Low credibility, unreliable content
- **Conspiracy-Pseudoscience**: Conspiracy theories, pseudoscientific claims

### Factual Reporting Levels

**Ratings**:
- **Very High**: Exceptional accuracy, strong sourcing
- **High**: Reliable, well-sourced reporting
- **Mixed**: Some accuracy issues
- **Low**: Frequent factual errors
- **Very Low**: Pervasive misinformation

### Overall Credibility

**Assessment Factors**:
- Editorial transparency
- Source credibility
- Factual accuracy
- Correction policies
- Journalistic standards

## Research Applications

### robots.txt Gatekeeping Study

**[[robots-txt-gatekeeping-paper|WWW '26 Paper]]** used MBFC classifications to analyze:

**Reputable News Websites** (3,369 sites):
- Labels: Pro-Science, Least Biased, Left-Center, Right-Center
- Factual reporting: High or Very High
- Overall credibility: High
- Finding: 60% block AI crawlers (May 2025)

**Misinformation Websites** (710 sites):
- Labels: Questionable Source, Conspiracy-Pseudoscience
- Factual reporting: Low or Very Low
- Overall credibility: Low
- Finding: Only 9.1% block AI crawlers (May 2025)

### Key Research Insights

**Asymmetric Blocking Behavior**:
- Reputable sites 6.6x more likely to block AI crawlers
- Gap widening over time (23% to 60% for reputable, Sep 2023 - May 2025)
- Content quality inversely correlated with accessibility
- Implications for [[llm-training-data-ethics|LLM training data quality]]

**Methodology Value**:
- Provides systematic classification of news sources
- Enables large-scale credibility analysis
- Widely used in academic research
- Transparent categorization criteria

## Use Cases

### Academic Research

**Misinformation Studies**:
- [[misinformation-accessibility|Misinformation accessibility]] analysis
- Content quality evaluation
- Source credibility tracking
- Web gatekeeping behavior

**Data Ethics**:
- LLM training data composition
- AI crawler behavior analysis
- Content protection patterns
- Web transparency research

### Media Literacy

**Educational Applications**:
- Teaching critical evaluation of sources
- Understanding media bias
- Recognizing misinformation
- Developing information literacy

**Public Awareness**:
- Source credibility ratings
- Transparency reports
- Media landscape analysis
- Fact-checking resources

## Methodology

### Evaluation Process

**Assessment Criteria**:
- Ownership and funding transparency
- Editorial policies and corrections
- Source citations and evidence
- Headline accuracy
- Consistency of reporting
- Historical track record

**Review Process**:
- Human expert evaluation
- Multi-source verification
- Regular updates and reassessments
- Community feedback integration

### Coverage

**Scope**:
- Primarily English-language sources
- Global coverage with regional focus
- News websites and media outlets
- Digital and traditional media

**Limitations**:
- Subjective human judgment
- English-language focus limits global coverage
- Binary categorizations simplify complex landscapes
- Political bias assessments may be contentious

## Related Research

### robots.txt and Web Gatekeeping

**[[robots-txt-gatekeeping-paper|WWW '26 Study]]**:
- Used MBFC to classify 4,079 websites
- Analyzed differential blocking of [[ai-crawlers|AI crawlers]]
- Demonstrated misinformation accessibility asymmetry
- Informed discussions on [[content-control-mechanisms|content control]]

### Misinformation Ecosystem

**Key Findings**:
- Low-credibility sites remain open to AI crawlers
- High-credibility sites increasingly protective
- Voluntary protocols ([[robots-txt|robots.txt]]) create two-tier landscape
- Training data bias implications

## Impact on AI Development

### Training Data Quality

**Accessibility Patterns**:
- High-quality sources blocking AI crawlers
- Low-quality sources remaining accessible
- Potential bias toward misinformation in training data
- Need for source credibility filtering

**Implications**:
- LLM training dataset composition concerns
- Misinformation amplification risk
- Content quality asymmetry
- Ethical data collection challenges

### Research and Policy

**Contributions**:
- Evidence base for AI regulation discussions
- Source credibility standards
- Transparency in data collection
- Responsible AI development practices

## Limitations and Considerations

### Classification Challenges

**Subjectivity**:
- Based on human judgment
- Political bias assessments may be contentious
- Binary categories oversimplify nuance
- Regional and cultural context limitations

**Coverage Gaps**:
- English-language focus
- Limited non-Western coverage
- Resource constraints on comprehensive review
- Evolving media landscape challenges

### Research Usage Caveats

**Academic Applications**:
- Widely used but not definitive
- Should be interpreted with awareness of limitations
- Useful for large-scale categorization
- Requires methodological transparency

**Mitigation Strategies**:
- Acknowledge classification subjectivity
- Use multiple credibility sources when possible
- Focus on clear-cut cases (very high vs. very low credibility)
- Document methodology limitations

## Related Pages

### Concepts

- [[misinformation-accessibility|Misinformation Accessibility]]
- [[llm-training-data-ethics|LLM Training Data Ethics]]
- [[robots-txt|robots.txt]]
- [[web-gatekeeping|Web Gatekeeping]]
- [[content-control-mechanisms|Content Control Mechanisms]]
- [[ai-crawlers|AI Crawlers]]

### Entities

**Organizations**:
- [[max-planck-institute-informatics|Max Planck Institute for Informatics]]
- [[saarland-university|Saarland University]]
- [[iit-bombay|IIT Bombay]]
- [[internet-archive|Internet Archive]]

**Researchers**:
- [[ha-dao|Ha Dao]]
- [[nicolas-steinacker-olsztyn|Nicolas Steinacker-Olsztyn]]
- [[devashish-gosain|Devashish Gosain]]

### Sources

- [[robots-txt-gatekeeping-paper|robots.txt Gatekeeping Paper]] (WWW '26)

## Sources

- [[robots-txt-gatekeeping-paper|Is Misinformation More Open? A Study of robots.txt Gatekeeping on the Web]] (WWW '26)
- Media Bias/Fact Check methodology documentation
