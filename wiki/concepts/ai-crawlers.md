---
title: AI Crawlers
tags: [concept, web-crawling, llm, data-collection]
created: 2026-06-24
updated: 2026-06-24
---

# AI Crawlers

Automated bots that collect web content specifically for AI-related purposes, including LLM training data, model fine-tuning, and AI-powered search features. These crawlers have fundamentally transformed [[web-gatekeeping|web gatekeeping]] practices since 2023.

## Overview

AI crawlers represent a distinct class of web bots designed to collect content for machine learning and AI applications, particularly large language model training. Unlike traditional search engine crawlers that index content for retrieval, AI crawlers extract content for incorporation into AI systems, raising new questions about copyright, consent, and [[data-collection-ethics|data ethics]].

## Major AI Crawlers

### OpenAI Crawlers

**[[gptbot|GPTBot]]**:
- **Purpose**: Training data collection for GPT models
- **Announcement**: August 2023 (significant backlash)
- **Blocking rate**: 52.5% of reputable sites (May 2025)
- **Most blocked AI crawler** across the board

**[[chatgpt-user|ChatGPT-User]]**:
- **Purpose**: Real-time web access for ChatGPT responses
- **Blocking rate**: 45.3% of reputable sites (May 2025)
- **Use case**: Inference, not training

### Anthropic Crawlers

**[[anthropic-ai|Anthropic-AI]]**:
- **Purpose**: Training data for Claude models
- **Blocking rate**: 43.6% of reputable sites (May 2025)
- **Active blocking target**: 16.9% of reputable sites block both Anthropic-AI and ClaudeBot

**[[claudebot|ClaudeBot]]**:
- **Purpose**: Claude model training and updates
- **Blocking rate**: 42.2% of reputable sites (May 2025)
- **Active blocking**: 24.8% of reputable sites

**[[claude-web|Claude-Web]]**:
- **Purpose**: Web access for Claude responses
- **Blocking rate**: 41.8% of reputable sites (May 2025)
- **Multiple crawler strategy**: Anthropic operates several specialized crawlers

### Google Crawlers

**[[google-extended|Google-Extended]]**:
- **Purpose**: Training data for Bard (Gemini) and Vertex AI
- **Announcement**: September 2023 (response to publisher concerns)
- **Blocking rate**: 43.7% of reputable sites (May 2025)
- **Distinction**: Separate from Googlebot (search indexing)

### Meta Crawlers

**Meta-ExternalAgent**:
- **Purpose**: Training data for Llama models
- **Blocking rate**: 2.6% of misinformation sites, lower on reputable sites
- **Lower profile**: Less documented than other major crawlers

**FacebookBot**:
- **Purpose**: Multiple uses including AI training
- **Blocking rate**: 36.7% of reputable sites (May 2025)
- **Dual use**: Social media + AI training

### Other Notable Crawlers

**[[perplexitybot|PerplexityBot]]**:
- **Purpose**: AI-powered search (Perplexity AI)
- **Controversy**: [[perplexity-robots-txt-scandal|Ignored robots.txt]] (June 2024)
- **Blocking rate**: 39.1% of reputable sites (May 2025)
- **Dramatic shift**: 26 sites switched from allowing to disallowing after scandal

**[[ccbot|CCBot]]** (Common Crawl):
- **Purpose**: Public web archive, data used by AI companies
- **Blocking rate**: 48.3% of reputable sites (May 2025)
- **Indirect AI use**: Not operated by AI company but data used for training
- **Second most blocked** crawler

**[[omgilibot|omgilibot]] and [[omgili|omgili]]**:
- **Purpose**: Content aggregation and AI applications
- **Blocking rate**: 41.5% and 39.9% respectively (May 2025)
- **Less prominent**: Smaller scale than major LLM providers

**ByteSpider** (ByteDance):
- **Purpose**: Training data for various AI models
- **Blocking rate**: 2.9% of misinformation sites
- **Regional focus**: Strong in Asia-Pacific

**Amazonbot**:
- **Purpose**: Amazon's AI initiatives
- **Blocking rate**: 3.9% of misinformation sites (May 2025)
- **Lower blocking**: Less scrutiny than major LLM providers

## Crawler Behavior and Compliance

### Declared Compliance

**Research Findings** (Liu et al. 2025, Kim et al. 2025):
- **Generally respectful**: Major AI crawlers that claim to honor [[robots-txt|robots.txt]] mostly do
- **Moderate compliance**: Not perfect but better than malicious bots
- **Spoofing detected**: Some non-compliance due to fake User-agent headers

**Verification Challenges**:
- Difficult to confirm actual behavior
- User-agent spoofing easy
- IP ranges not always public
- Self-reported compliance unverified

### Non-Compliance Cases

**[[perplexity-robots-txt-scandal|Perplexity Scandal]]** (June 2024):
- Amazon-backed Perplexity caught ignoring robots.txt
- New York Times sent cease-and-desist
- Public backlash and media coverage
- **Impact**: 26 reputable sites switched from allowing to blocking PerplexityBot

**Implications**:
- Voluntary protocol has limits
- Reputation damage for violators
- Drives shift toward [[active-blocking|active blocking]]
- Demonstrates need for technical enforcement

## Blocking Patterns

### Reputable News Websites

**[[robots-txt-gatekeeping-paper|Empirical Data]]** (May 2025):
- **60% block at least one AI crawler**
- **Average 15.5 AI agents blocked per site**
- **25% block 10+ AI agents**
- **Most restrictive sites block 54 agents**

**Top Targets**:
1. GPTBot: 52.5%
2. CCBot: 48.3%
3. ChatGPT-User: 45.3%
4. Google-Extended: 43.7%
5. Anthropic-AI: 43.6%

**Pattern**: Comprehensive, systematic blocking
- Regular updates as new crawlers emerge
- Multi-agent blocklists
- DisallowAll dominant (98.4% of restrictions)
- Multi-layered defenses (robots.txt + [[active-blocking|active blocking]])

### Misinformation Websites

**Blocking Rates** (May 2025):
- **9.1% block any AI crawler**
- **Average 0.77 AI agents blocked per site**
- **80% block zero agents**
- **All individual crawlers <5% blocked**

**Most Blocked** (among those that block):
- PetalBot (Huawei): 3.3%
- GPTBot: 4.8%
- ByteSpider: 2.9%

**Pattern**: Minimal, unsystematic
- No response to AI developments
- Ad-hoc blocking with no clear strategy
- Passive approach
- Minimal correlation between robots.txt and active blocking

### The 6.6x Disparity

**Content Accessibility Asymmetry**:
- Reputable content increasingly restricted
- Misinformation content remains open
- Gap widening over time (5x → 6.6x, Sep 2023 → May 2025)
- Implications for [[llm-training-data-ethics|LLM training data quality]]

## Evolution Timeline

### Pre-2023: Traditional Era

**Focus**: Search engine crawlers
- Googlebot, Bingbot widely allowed
- Preferential treatment for major engines
- Correlation with market share
- Generally permissive policies

### August 2023: GPTBot Announcement

**Turning Point**: OpenAI reveals GPTBot
- Immediate publisher backlash
- **Reputable site blocking surges**: 23% → 50% in 5 months
- GPTBot becomes most-blocked crawler
- Marks beginning of AI crawler era

### September 2023: Google-Extended

**Response to Concerns**: Google creates separate AI crawler
- Allows sites to block AI training without blocking search
- Distinction between indexing and training
- Publisher-friendly approach
- **Blocking rate**: 43.7% by May 2025

### June 2024: Perplexity Scandal

**Crisis Event**: [[perplexity-robots-txt-scandal|Perplexity caught ignoring robots.txt]]
- Amazon investigation
- New York Times legal action
- **26 reputable sites switch from allowing to disallowing**
- Demonstrates limits of voluntary compliance
- Accelerates shift to active defenses

### August 2024: EU AI Act

**Regulatory Pressure**: EU AI Act enters force
- Transparency requirements for training data
- Copyright and consent focus
- **Continued blocking acceleration**: Toward 60%
- Policy and legal framework development

### May 2025: Current State

**Established Pattern**:
- 60% of reputable sites block AI crawlers
- 9.1% of misinformation sites block
- Systematic vs. passive approach
- Median 25+ agents blocked (reputable sites)
- Trend suggests continued divergence

## Technical Characteristics

### Identification

**User-agent Strings**: Primary identification method
```
Mozilla/5.0 AppleWebKit/537.36 (KHTML, like Gecko; compatible; GPTBot/1.0; +https://openai.com/gptbot)
```

**IP Ranges**: Some crawlers publish IP lists
- Enables network-level blocking
- Not all crawlers provide this
- ClaudeBot and Anthropic-AI notably absent

**Behavior Patterns**:
- Request frequency
- Crawling patterns
- Response to robots.txt
- JavaScript execution capabilities

### Detection Challenges

**Spoofing**: Easy to fake User-agent
- Minimal authentication
- No cryptographic verification
- Undermines voluntary compliance

**Stealth Tactics**:
- Rotating User-agents
- Using browser automation tools
- Mimicking human behavior
- Distributed crawling

**Defense**: Requires behavioral analysis
- Request patterns
- Non-human interaction
- JavaScript challenges
- [[webcloak|WebCloak]]-style technical defenses

## Impact on AI Development

### Training Data Implications

**Content Quality Bias**:
- High-quality sources increasingly excluded
- Low-quality sources remain accessible
- [[misinformation-accessibility|Misinformation accessibility asymmetry]]
- Training datasets may skew toward unreliable content

**Evidence**:
- NewsGuard (2025): 28% of LLM responses contain false information
- Google C4 dataset: Contains propaganda, conspiracy content
- Lack of transparency in training datasets

**Feedback Loop Risk**:
- AI-generated misinformation sites stay open
- Content scraped for training
- New AI models generate more misinformation
- Recursive amplification

### Alternative Approaches

**Content Licensing**:
- Direct partnerships with publishers
- Compensated data access
- OpenAI, Google exploring models
- Addresses copyright and ethical concerns

**Authorized APIs**:
- Structured data access
- Rate-limited and authenticated
- Respects publisher control
- [[autodata|AutoData]] dual-mode example

**Curated Datasets**:
- Manual selection and filtering
- Quality-focused collections
- Source credibility evaluation
- Higher cost but better quality

## Impact on Information-Seeking Agents

### WebDancer

[[webdancer|WebDancer]] (RL-trained research agent):
- **Constrained by gatekeeping**: Must respect robots.txt during training and inference
- **Training data bias**: May absorb misinformation from accessible sources
- **Real-time research**: Limited to open content
- **Generalization**: Trained on biased content distribution

### AutoData

[[autodata|AutoData]] (multi-agent data collection):
- **Blocked by reputable sites**: Crawling mode restricted
- **Dual-mode advantage**: Can use APIs when crawling blocked
- **Disproportionate collection**: May gather more from open (misinformation) sources
- **Highlights need**: For authorized access channels

### Broader Ecosystem

**Research Agents**: Need for source credibility filtering
**AI Search**: May preferentially cite accessible (lower-quality) sources
**Question Answering**: Training data quality directly affects accuracy
**Information Extraction**: Gatekeeping shapes available knowledge

## Catalog and Resources

### Comprehensive Lists

**[[dark-visitors|Dark Visitors]]**:
- Most comprehensive AI crawler catalog
- Real-time updates
- Public API and website
- 63+ AI user agents documented

**[[ai-robots-txt|ai.robots.txt]]**:
- Public GitHub repository
- Community-maintained blocklist
- Easy integration for websites
- Templates and examples

**[[cloudflare-radar|Cloudflare Radar]]**:
- Network-level crawler tracking
- Verified bot identification
- Real-time insights
- Integration with CDN protection

### Documentation Standards

**Crawler Documentation** (best practices):
- Public User-agent strings
- Published IP ranges
- Respect for robots.txt commitment
- Contact information for concerns
- Transparency about data usage

**Current State**: Varies widely
- Major players (OpenAI, Google, Anthropic) well-documented
- Smaller crawlers less transparent
- Inconsistent standards
- Room for improvement

## Future Directions

### Technical Evolution

**Enhanced Authentication**:
- Cryptographic crawler identity
- Tamper-proof User-agent
- IP range verification
- Blockchain-based access logs

**Fine-Grained Permissions**:
- Distinguish training vs. inference
- Allow search but block training
- Use case differentiation
- Licensing term integration

**Behavioral Fingerprinting**:
- Beyond User-agent detection
- Pattern analysis
- Machine learning classification
- Adaptive defenses

### Regulatory Framework

**Mandated Compliance**:
- Legal consequences for violations
- Enforcement mechanisms
- Transparency requirements
- Auditing and verification

**Standards Development**:
- Industry best practices
- Protocol enhancements beyond robots.txt
- International harmonization
- Stakeholder collaboration

**Licensing Markets**:
- Standardized content licensing
- Fair compensation mechanisms
- Authorized data marketplaces
- API-based alternatives to crawling

### Ecosystem Trends

**Continued Blocking**:
- Reputable sites increasingly restrictive
- Misinformation sites remain open
- Asymmetry likely to widen
- Training data quality concerns intensify

**Arms Race**:
- More sophisticated crawlers
- Advanced defenses ([[webcloak|WebCloak]] evolution)
- Cat-and-mouse dynamic
- Innovation on both sides

**Market Solutions**:
- Content licensing proliferation
- Partnership models
- API-first approaches
- Compensated access becoming standard

## Key Takeaways

1. **Fundamental Shift**: AI crawlers transformed web gatekeeping from 2023-2025, with 60% of reputable sites now blocking.

2. **Major Players**: OpenAI (GPTBot), Google (Google-Extended), Anthropic (ClaudeBot, Anthropic-AI) most frequently blocked.

3. **Compliance Generally Good**: Major AI crawlers mostly respect robots.txt, but exceptions exist (Perplexity scandal).

4. **Content Asymmetry**: Reputable sites block 6.6x more than misinformation sites, creating training data bias risk.

5. **Event-Driven Evolution**: Critical events (GPTBot announcement, Perplexity scandal, EU AI Act) drive rapid changes.

6. **Detection Challenges**: User-agent spoofing easy; requires multi-layered defenses (robots.txt + active blocking + technical).

7. **Impact on AI**: Training data quality concerns; alternative approaches (licensing, APIs) emerging.

8. **Future Trajectory**: Continued blocking, widening asymmetry, regulatory intervention, and technical arms race expected.

## Related Pages

### Concepts
- [[robots-txt|robots.txt]]
- [[web-gatekeeping|Web Gatekeeping]]
- [[active-blocking|Active Blocking]]
- [[misinformation-accessibility|Misinformation Accessibility]]
- [[llm-training-data-ethics|LLM Training Data Ethics]]
- [[data-collection-ethics|Data Collection Ethics]]
- [[voluntary-compliance-protocols|Voluntary Compliance Protocols]]

### Specific Crawlers
- [[gptbot|GPTBot]]
- [[chatgpt-user|ChatGPT-User]]
- [[anthropic-ai|Anthropic-AI]]
- [[claudebot|ClaudeBot]]
- [[claude-web|Claude-Web]]
- [[google-extended|Google-Extended]]
- [[perplexitybot|PerplexityBot]]
- [[ccbot|CCBot]]

### Resources
- [[dark-visitors|Dark Visitors]]
- [[ai-robots-txt|ai.robots.txt]]
- [[cloudflare-radar|Cloudflare Radar]]

### Related Technologies
- [[webdancer|WebDancer]]
- [[autodata|AutoData]]
- [[webcloak|WebCloak]]

### Sources
- [[robots-txt-gatekeeping-paper|robots.txt Gatekeeping Paper]]

### Events
- [[perplexity-robots-txt-scandal|Perplexity robots.txt Scandal]]
