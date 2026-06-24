---
title: Reputable vs Misinformation Blocking Behavior: 6.6x Disparity
tags: [comparison, research-papers, web-gatekeeping, misinformation, training-data-quality]
created: 2026-06-24
updated: 2026-06-24
---

# Reputable vs Misinformation Blocking Behavior: 6.6x Disparity

A comprehensive analysis of the stark asymmetry in [[ai-crawlers|AI crawler]] blocking behavior between reputable news websites and misinformation sites, as revealed by [[robots-txt-gatekeeping-paper|longitudinal research]]. The 6.6x disparity in [[robots-txt|robots.txt]] adoption (60% vs 9.1%) creates a training data quality crisis where high-quality content is increasingly restricted while low-quality misinformation remains freely accessible to AI systems.

## Overview

A groundbreaking 20-month longitudinal study (September 2023 - May 2025) of 3,369 reputable news websites and 710 misinformation websites reveals a dramatic and widening gap in AI crawler gatekeeping behavior. As reputable sites rapidly adopt [[robots-txt|robots.txt]] directives to block AI crawlers (23% → 60%), misinformation sites remain largely passive (4.6% → 9.2%), creating what researchers term [[misinformation-accessibility|misinformation accessibility asymmetry]].

## Quick Comparison Table

| Metric | Reputable News Sites | Misinformation Sites | Disparity |
|--------|---------------------|---------------------|-----------|
| **robots.txt Blocking (May 2025)** | 60.0% | 9.1% | **6.6x** |
| **Avg AI Agents Blocked** | 15.5 | 0.77 | **20x** |
| **Median Agents Blocked** | 25+ | 0 | **∞** |
| **Active Blocking Rate** | 16.9% | 9.8% | 1.7x |
| **Growth (Sep 2023 → May 2025)** | 23% → 60% (+161%) | 4.6% → 9.2% (+100%) | Widening |
| **DisallowAll Usage** | 98.4% | 72.7% | 1.4x |
| **Active + robots.txt Correlation** | 47.3% | 1.8% | **26x** |
| **Response to Events** | Systematic, rapid | Minimal, passive | Stark contrast |

## Website Classification

### Reputable News Websites (3,369 sites)

**Source**: [[media-bias-fact-check|Media Bias/Fact Check (MBFC)]]

**Criteria**:
- Labels: Pro-Science, Least Biased, Left-Center, or Right-Center
- Factual reporting: High or Very High
- Overall credibility: High
- Editorial standards: Professional journalism, fact-checking, corrections
- Sourcing: Evidence-based, credible sources cited
- Transparency: Editorial oversight, author accountability

**Examples** (types, not specific sites):
- Major international newspapers (NYT-style)
- National news organizations (Reuters-style)
- Respected regional papers
- Established news magazines
- Science journalism outlets
- Investigative journalism sites

**Characteristics**:
- Significant investment in content creation
- Professional journalists and editors
- Revenue models: subscriptions, advertising, licensing
- Reputation-dependent business model
- Legal and ethical accountability

### Misinformation Websites (710 sites)

**Source**: [[media-bias-fact-check|Media Bias/Fact Check (MBFC)]]

**Criteria**:
- Labels: "Questionable Source" or "Conspiracy-Pseudoscience"
- Factual reporting: Low or Very Low
- Overall credibility: Low
- Editorial standards: Minimal or absent
- Sourcing: Unreliable, lacking, or fabricated
- Transparency: Limited author information, no corrections

**Examples** (types):
- Conspiracy theory sites
- Pseudoscience promoters
- Partisan propaganda outlets
- Deliberately misleading "news" sites
- Health misinformation platforms
- Climate denial sites

**Characteristics**:
- Low-cost content creation
- Engagement-driven (not quality-driven)
- Revenue models: ads, affiliate marketing, donations
- No reputation to protect (or benefit from bad reputation)
- Minimal accountability

## Blocking Behavior Comparison

### robots.txt Adoption

**Reputable Sites (60% block AI crawlers)**:
- **15.5 average** AI user agents blocked
- **25+ median** for most restrictive sites (highest: 54 agents)
- **98.4%** use DisallowAll (complete blocking, not partial)
- **Top 10 agents blocked**: GPTBot (52.5%), CCBot (48.3%), ChatGPT-User (45.3%), Google-Extended (43.7%), Anthropic-AI (43.6%), ClaudeBot (42.2%), Claude-Web (41.8%), omgilibot (41.5%), omgili (39.9%), PerplexityBot (39.1%)
- **Systematic adoption**: Rapid increase in response to events
- **Comprehensive lists**: Median 15.5 agents, some block 54

**Misinformation Sites (9.1% block AI crawlers)**:
- **0.77 average** AI user agents blocked
- **0 median** throughout entire study period
- **Over 80%** block zero AI agents
- **All agents < 5%** blocking rate individually
- **No systematic response**: Minimal change over time
- **Sparse lists**: When used, typically only 1-2 agents

**6.6x Disparity**: 60% / 9.1% = 6.6 times more likely for reputable sites to block AI crawlers

**20x Disparity (agents blocked)**: 15.5 / 0.77 = 20 times more AI agents blocked on average

### Active Blocking

**Reputable Sites (16.9% actively block)**:
- Block both ClaudeBot and Anthropic-AI based on User-agent
- **47.3% correlation** with robots.txt rules (multi-layered strategy)
- Technical enforcement beyond voluntary compliance
- Systematic approach to content protection

**Misinformation Sites (9.8% actively block)**:
- Similar blocking rate to reputable sites (only 1.7x disparity)
- **1.8% correlation** with robots.txt rules (ad-hoc, not systematic)
- Active blocking without corresponding robots.txt policy
- Likely unrelated to AI crawler concerns (may be anti-bot measures)

**Key Difference**: Reputable sites use active blocking as part of systematic multi-layered defense (47.3% correlation), misinformation sites use it ad-hoc without policy coherence (1.8% correlation).

### Temporal Evolution

**Longitudinal Trends (Sep 2023 → May 2025)**:

**Reputable Sites**:
- **Sep 2023**: 23% block at least one AI agent
- **Jan 2024**: 50% (dramatic surge, +217% relative increase)
- **May 2025**: 60% (continued growth, +161% total)
- **Acceleration**: Rapid, systematic response to events
- **Median agents blocked**: 0 → 25+ (exponential increase)

**Misinformation Sites**:
- **Sep 2023**: 4.6% block any AI agent
- **Jan 2024**: ~7% (minimal change, +52% relative increase)
- **May 2025**: 9.2% (marginal growth, +100% total)
- **Stagnation**: Slow, passive, no systematic response
- **Median agents blocked**: 0 throughout (no change)

**Gap Widening**: Absolute gap increases from 18.4 percentage points (23% - 4.6%) to 50.9 percentage points (60% - 9.1%). Relative gap stable at ~6-7x.

## Explanatory Factors

### Why Reputable Sites Block

**Intellectual Property Protection**:
- Journalism requires significant investment (reporters, editors, fact-checking)
- Content has economic value (subscriptions, licensing)
- Unauthorized use threatens business model
- Copyright concerns prominent

**Revenue Model Threat**:
- AI systems may reproduce content without attribution
- Reduces traffic to original sources
- Undermines subscription value (answers provided without visiting site)
- Ad revenue decline from reduced visits

**Ethical and Legal Concerns**:
- Content created under professional standards
- Expectation of control over usage
- Legal clarity on copyright and fair use evolving
- Proactive protection while law catches up

**Reputation and Brand**:
- Established brands with reputation to protect
- Relationships with AI companies matter (partnerships, negotiations)
- Public stance on AI data usage signals values
- Industry leadership and peer pressure

**Response to Events**:
- GPTBot announcement (Aug 2023) → immediate adoption surge
- Perplexity scandal (Jun 2024) → 26 sites switch to blocking PerplexityBot
- EU AI Act (Aug 2024) → continued acceleration
- Systematic, coordinated response across industry

### Why Misinformation Sites Don't Block

**No Economic Threat**:
- Content creation costs minimal (low/no fact-checking, often plagiarized)
- Revenue from engagement, not content quality
- AI scraping doesn't threaten core business model
- May even benefit from AI amplification

**No Reputation to Protect**:
- Low credibility already established (or desired)
- No peer pressure from reputable journalism community
- No partnerships or relationships with AI companies to maintain
- May benefit from controversial reputation

**Engagement-Driven Model**:
- Goal is virality, not quality
- AI-driven discovery could increase reach
- More accessibility = more potential engagement
- Opposite incentive structure from reputable sites

**Lack of Resources and Sophistication**:
- May not have technical knowledge to implement robots.txt
- May not monitor industry developments (GPTBot announcements, etc.)
- Minimal editorial staff to manage site policies
- Reactive rather than proactive management

**Ideological Motivation**:
- Some misinformation sites motivated by ideology, not profit
- Want maximum content spread regardless of channel
- AI systems spreading their narratives may be welcomed
- Counter to gatekeeping philosophy

**Feedback Loop Potential**:
- AI-generated misinformation sites may not block AI crawlers
- Creates recursive cycle: AI trains on AI-generated misinformation
- Amplification of low-quality content in training data

### Popularity Control Analysis

**Research Question**: Is the disparity explained by popularity/resources?

**Method**: Examine Tranco ranking (web traffic ranking) to assess if popular sites have more resources to implement blocking.

**Findings**:
- **Top 10K sites**: Both reputable and misinformation show similar initial blocking rates
- **Beyond 10K**: Clear divergence appears (reputable increases, misinformation stays low)
- **Top 10K misinformation**: 15 sites identified, 71.4% still don't block any AI agents
- **Conclusion**: Content type, not just popularity/resources, drives blocking behavior

**Implication**: Even highly popular, well-resourced misinformation sites remain open to AI crawlers, confirming content-type-driven incentive structure.

## Training Data Quality Implications

### The Asymmetry Problem

**Current State (May 2025)**:
- 60% of reputable content increasingly restricted to AI crawlers
- 91% of misinformation content remains freely accessible
- Gap widening over time (23% → 60% vs 4.6% → 9.2%)
- Systematic divergence in accessibility

**Inevitable Consequence**: AI training datasets skew toward lower-quality, less credible sources.

**Analogy**: "The signal-to-noise ratio of the accessible web is declining for AI training purposes."

### Impact on LLM Training Data

**High-Quality Content Increasingly Excluded**:
- Premium journalism behind robots.txt barriers
- Investigative reporting restricted
- Expert analysis and fact-checked content blocked
- Scientific and evidence-based reporting less accessible

**Low-Quality Content Remains Accessible**:
- Conspiracy theories freely scrapeable
- Pseudoscience available without restriction
- Partisan propaganda accessible
- Deliberately misleading content open to crawlers

**Training Dataset Skew**:
- Overrepresentation of misinformation in training data
- Underrepresentation of fact-checked journalism
- Bias toward sensationalism over accuracy
- Quality inversion: worse content more available

### Evidence of Impact

**NewsGuard (2025) Finding**:
- 28% of leading LLM responses contain false information
- Directly linked to training data quality issues
- Misinformation accessibility likely contributing factor

**Google C4 Dataset Analysis**:
- Contains white supremacy content
- Includes conspiracy theories
- Has propaganda from questionable sources
- Demonstrates training data quality challenges

**Lack of Transparency**:
- Most LLM training datasets not publicly disclosed
- Difficult to assess extent of misinformation inclusion
- No systematic auditing of source credibility
- Asymmetry likely worse than currently documented

### Future Trajectory

**If Trends Continue**:
- Reputable sites → 70-80% blocking (extrapolating growth)
- Misinformation sites → ~10-15% blocking (minimal change)
- Gap widens to 7-8x or more
- Training data quality crisis deepens

**Feedback Loop Risk**:
- AI systems trained on misinformation produce misinformation
- AI-generated misinformation sites emerge (low-cost, high-volume)
- These sites likely don't block AI crawlers (no incentive)
- Recursive amplification of low-quality content

**Downstream Effects**:
- Question-answering systems cite unreliable sources
- Research agents ([[webdancer|WebDancer]], [[autodata|AutoData]]) face degraded source quality
- Public trust in AI systems declines
- Misinformation spreads via AI-powered channels

## Recommendations and Interventions

### For AI Companies

**Prioritize Quality Sources**:
- Seek licensing agreements with reputable publishers
- Pay for access to restricted high-quality content
- Don't rely solely on freely accessible web content
- Implement source credibility filtering

**Respect robots.txt**:
- Maintain compliance with voluntary protocols
- Demonstrate good faith to content creators
- Avoid Perplexity-style violations
- Build trust for future negotiations

**Source Transparency**:
- Disclose training data sources (within competitive limits)
- Provide credibility metrics for training datasets
- Enable auditing of source quality
- Build public trust through transparency

**Active Misinformation Filtering**:
- Don't passively accept accessible content
- Implement credibility scoring for sources
- Filter known misinformation sites from training
- Bias toward quality, not just accessibility

### For Content Creators (Reputable Sites)

**Continue Systematic Protection**:
- Maintain and update robots.txt policies
- Consider multi-layered defense (robots.txt + [[active-blocking|active blocking]] + [[webcloak|WebCloak]])
- Monitor for violations and respond
- Coordinate industry responses to events

**Explore Licensing Models**:
- Negotiate authorized access agreements
- Provide APIs for authorized AI training
- Balance protection with fair compensation
- Create win-win scenarios

**Don't Rely on Blocking Alone**:
- Legal frameworks needed alongside technical measures
- Advocate for clear regulations
- Participate in policy discussions
- Build industry consensus on acceptable practices

### For Policymakers and Regulators

**Address the Asymmetry**:
- Recognize training data quality as AI safety issue
- Require source credibility auditing for AI systems
- Consider fair use carve-outs for legitimate research
- Balance innovation with content creator rights

**Strengthen Copyright Enforcement**:
- Clarify legal status of AI training on copyrighted content
- Make robots.txt violations legally consequential
- Provide content creators with enforcement mechanisms
- Create incentives for responsible data collection

**Incentivize Quality Data Access**:
- Tax breaks or subsidies for licensing agreements
- Public funding for authorized training datasets
- Government-curated high-quality training corpora
- Reduce economic barriers to quality data access

**Misinformation Regulation**:
- Address misinformation sites' accessibility advantage
- Require credibility labeling for online content
- Empower platforms to restrict misinformation sources
- Balance free speech with training data quality

### For Researchers

**Study the Asymmetry**:
- Longitudinal tracking of blocking behavior trends
- Quantify training data quality impact
- Measure downstream effects on AI system outputs
- Document feedback loop dynamics

**Develop Filtering Techniques**:
- Automated source credibility assessment
- Training data quality metrics
- Misinformation detection for datasets
- Bias correction methods

**Create Public Datasets**:
- Curate high-quality, credible training datasets
- Make available for research purposes
- Enable development of quality-focused models
- Reduce reliance on indiscriminate web scraping

## Alternative Perspectives

### The Openness Argument

**Perspective**: Open access to information is fundamental to the web; blocking AI crawlers undermines this principle.

**Counterargument**:
- Display for reading ≠ permission for AI training
- Content creators should control their work
- Business models need protection
- Voluntary blocking (robots.txt) is minimal restriction

**Current Reality**: Even with 60% blocking, 40% of reputable content remains accessible; asymmetry is the issue, not absolute restriction.

### The Innovation Argument

**Perspective**: Restricting AI crawler access hinders beneficial AI development and innovation.

**Counterargument**:
- Quality over quantity: Better AI from quality data
- Licensing models enable innovation with compensation
- APIs provide authorized access pathways
- Unrestricted access to misinformation ≠ beneficial innovation

**Current Reality**: AI companies can still innovate by licensing quality content rather than scraping misinformation.

### The Fair Use Argument

**Perspective**: AI training may constitute fair use, making robots.txt blocking ethically questionable or legally weak.

**Counterargument**:
- Fair use doctrine unsettled for AI training
- robots.txt signals intent regardless of law
- Content creators' rights should be respected
- Courts will decide, but cooperation better than litigation

**Current Reality**: Legal uncertainty means voluntary protocols (robots.txt) serve as interim ethical standard.

## Comparison to Other Content Control Disparities

### Search Engine Blocking

**Traditional Search Engines**:
- Reputable sites generally allow (want discoverability)
- Misinformation sites generally allow (want reach)
- No significant disparity (both incentivized to allow)

**AI Crawlers (This Study)**:
- Reputable sites increasingly block (60%)
- Misinformation sites rarely block (9.1%)
- 6.6x disparity (opposing incentives)

**Key Difference**: Search engines drive traffic to sources (benefit content creators), AI systems reproduce content (reduce traffic, threaten business models).

### Paywalls and Authentication

**Paywalls**:
- Reputable sites often use (premium journalism)
- Misinformation sites rarely use (engagement-driven)
- Similar disparity pattern to AI blocking

**Parallel**: Both paywalls and robots.txt reflect economic value of content—reputable sites monetize quality, misinformation sites chase engagement.

### Content Licensing

**Licensing Agreements**:
- Reputable sites participate (sell syndication rights, etc.)
- Misinformation sites don't participate (no demand for their content)
- Clear value disparity

**Implication**: AI companies should follow existing licensing models rather than relying on scraping.

## Case Studies

### Event-Driven Responses

**GPTBot Announcement (August 2023)**:
- **Reputable Sites**: Immediate surge in blocking, 23% → 50% in months
- **Misinformation Sites**: Minimal response, 4.6% → ~7%
- **Disparity**: Reputable sites respond systematically, misinformation sites passive

**Perplexity Scandal (June 2024)**:
- **Reputable Sites**: 26 sites switch from allowing to disallowing PerplexityBot
- **Misinformation Sites**: No documented response
- **Disparity**: Reputational concerns drive reputable site behavior, absent for misinformation

**EU AI Act (August 2024)**:
- **Reputable Sites**: Continued acceleration to 60%
- **Misinformation Sites**: Marginal increase to 9.2%
- **Disparity**: Regulatory awareness and compliance higher for reputable sites

### Cross-Border Patterns

**Geographic Distribution**:
- Study used 7 geographically distributed vantage points (Germany, Sweden, US, Brazil, Africa, Asia, Australia)
- Results consistent across regions
- Disparity not geographically limited

**Implication**: Content type, not geography, drives blocking behavior; this is a global phenomenon.

## The "Misinformation Advantage"

### Accessibility as Competitive Advantage

**For Misinformation Sites**:
- Accessibility to AI crawlers = amplification potential
- AI systems may cite misinformation sources
- Training data inclusion spreads narratives
- Free distribution via AI channels

**Feedback Loop**:
1. Misinformation sites remain accessible
2. AI systems train on misinformation content
3. AI systems reproduce or reference misinformation
4. Misinformation spreads via AI-powered tools
5. Misinformation gains credibility through AI association
6. Cycle repeats and amplifies

**Economic Paradox**: Low-quality content gains distribution advantage over high-quality content due to inverse incentive structure.

### Reputable Sites' "Disadvantage"

**For Reputable Sites**:
- Blocking AI crawlers = reduced AI-mediated discovery
- AI systems can't cite protected journalism
- Training data exclusion reduces AI's knowledge of quality content
- Protection comes at cost of reach

**Trade-off**:
- **Protection**: Preserve revenue model, control content usage
- **Reach**: Enable AI-mediated discovery and citation
- **Current choice**: Most reputable sites (60%) choose protection over reach

**Potential Resolution**: Licensing agreements provide both protection (compensation) and reach (authorized inclusion).

## Conclusion

### Stark and Widening Disparity

**6.6x Gap** (60% vs 9.1%): The most striking finding of the research.

**20x Gap in Agents Blocked** (15.5 vs 0.77): Even more dramatic when measuring intensity.

**Temporal Trend**: Gap widening over time (Sep 2023: ~5x → May 2025: 6.6x), not stabilizing.

### Content Type, Not Resources, Drives Behavior

**Popularity Control**: Even popular misinformation sites don't block (71.4% of top 10K misinformation sites remain open).

**Conclusion**: Incentive structures, not technical capacity, explain the disparity.

### Training Data Quality Crisis

**Inevitable Consequence**: AI systems increasingly trained on disproportionately low-quality content.

**Evidence of Impact**: 28% of leading LLM responses contain false information (NewsGuard 2025).

**Future Risk**: Feedback loops amplify the problem (AI-generated misinformation, recursive training).

### Multi-Stakeholder Problem

**AI Companies**: Need quality data, currently incentivized to scrape accessible (lower-quality) content.

**Reputable Sites**: Need protection and compensation, blocking as defensive measure.

**Misinformation Sites**: Benefit from accessibility, no incentive to change.

**Policymakers**: Need to address asymmetry as AI safety and societal issue.

### Interventions Required

**Technical**: Source credibility filtering, active misinformation exclusion from training.

**Economic**: Licensing agreements for quality content, compensation models.

**Policy**: Copyright enforcement, training data transparency requirements, misinformation regulation.

**Industry**: Systematic coordination among reputable sites, shared blocklists, collective bargaining.

### The Path Forward

**Short-Term**: Expect continued disparity growth, training data quality degradation, downstream impacts on AI system reliability.

**Long-Term**: Hope for balanced ecosystem with fair compensation for quality content, technical and legal frameworks preventing misinformation advantage, AI systems trained on quality data through authorized access.

**Critical Need**: Address the asymmetry before it becomes entrenched and feedback loops amplify negative consequences.

## Related Pages

### Core Concepts

- [[misinformation-accessibility|Misinformation Accessibility Asymmetry]]
- [[robots-txt|robots.txt]]
- [[active-blocking|Active Blocking]]
- [[web-gatekeeping|Web Gatekeeping]]
- [[ai-crawlers|AI Crawlers]]
- [[llm-training-data-ethics|LLM Training Data Ethics]]
- [[data-collection-ethics|Data Collection Ethics]]

### Related Comparisons

- [[voluntary-vs-active-blocking|Voluntary vs Active Blocking]]
- [[robots-txt-vs-webcloak|robots.txt vs WebCloak]]

### Specific Events

- [[perplexity-robots-txt-scandal|Perplexity robots.txt Scandal (June 2024)]]

### AI Systems Affected

- [[webdancer|WebDancer]] (research agent facing source quality degradation)
- [[autodata|AutoData]] (data collection system affected by blocking)
- All LLM training pipelines (training data quality impact)

### Entities

- [[ha-dao|Ha Dao]] (Lead researcher)
- [[nicolas-steinacker-olsztyn|Nicolas Steinacker-Olsztyn]] (First author)
- [[devashish-gosain|Devashish Gosain]] (Co-author)
- [[max-planck-institute-informatics|Max Planck Institute for Informatics]]
- [[media-bias-fact-check|Media Bias/Fact Check]] (Content classification source)
- [[internet-archive|Internet Archive]] (Longitudinal data source)

### Sources

- [[robots-txt-gatekeeping-paper|robots.txt Gatekeeping Research Paper]] (WWW 2026)

## Sources

- [[robots-txt-gatekeeping-paper|Is Misinformation More Open? A Study of robots.txt Gatekeeping on the Web]] - Primary source (WWW '26)
- NewsGuard (2025) - LLM misinformation finding
- Liu et al. (2025) - AI crawler compliance study
- Google C4 dataset analyses - Training data quality documentation
