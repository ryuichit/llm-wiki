---
title: Web Scraping Difficulty Tiers
tags: [concept, taxonomy, classification, security]
created: 2026-06-24
updated: 2026-06-24
---

# Web Scraping Difficulty Tiers

A five-tier classification system for web scraping complexity introduced in the [[beyond-beautifulsoup-paper|Beyond BeautifulSoup research]], organizing websites by technical complexity and authentication/protection requirements to systematically evaluate scraping tool capabilities.

## Overview

The tier system classifies websites from simple static HTML to CAPTCHA-protected sites, enabling systematic comparison of different scraping approaches (traditional tools vs LLM agents) across progressively challenging scenarios. Each tier represents both increasing technical complexity and stronger protection mechanisms.

## The Five Tiers

### Tier 1: Simple HTML

**Technical Complexity**: ❍ (Low)
**Authentication/Protection**: ❍ (None)

**Characteristics**:
- Public-facing pages
- Simple, static HTML structures
- No login required
- No JavaScript execution needed
- No client-side rendering
- Straightforward DOM structure
- Predictable page layouts

**Examples**:
- The Hacker News (https://thehackernews.com/)
- Wikipedia (https://www.wikipedia.org/)
- Investopedia (https://www.investopedia.com/)
- Goodreads (https://www.goodreads.com/)

**Scraping Approach**:
- Simple HTML parsing sufficient
- CSS selectors or XPath
- HTTP GET requests
- No session management
- Minimal error handling

**Success Rates** (Beyond BeautifulSoup):
- [[beautifulsoup|BeautifulSoup]]: 93%
- [[scrapy|Scrapy]]: 82%
- [[claude-computer-use|Claude]]: 100%
- [[simular-ai|Simular.ai]]: 100%

**Best Tool**: Traditional tools (BeautifulSoup) for speed (< 2 seconds); agents for ease-of-use

### Tier 2: Complex HTML

**Technical Complexity**: ◗ (Medium)
**Authentication/Protection**: ❍ (None)

**Characteristics**:
- Static pages with deep structure
- Nested tables and div layers
- Multi-column layouts
- Dynamic class names
- Varied CSS patterns
- Complex DOM trees
- Still no authentication required

**Examples**:
- CNN (https://www.cnn.com/)
- BBC (https://www.bbc.com/)
- New York Times (https://www.nytimes.com/)
- Reuters (https://www.reuters.com/)
- National Geographic (https://www.nationalgeographic.com/)
- NPR (https://www.npr.org/)
- Vimeo (https://vimeo.com/)

**Scraping Challenges**:
- Deep nesting requires specific selectors
- Dynamic class names confuse static selectors
- Multiple extraction patterns needed
- Table parsing complexity
- Inconsistent structure across pages

**Success Rates** (Beyond BeautifulSoup):
- [[beautifulsoup|BeautifulSoup]]: 80%
- [[scrapy|Scrapy]]: 20% (surprisingly low)
- [[claude-computer-use|Claude]]: 57%
- [[simular-ai|Simular.ai]]: 100%

**Best Tool**: Visual agents (Simular.ai) achieve perfect success with low effort

**Key Finding**: Simular.ai's 100% vs Claude's 57% demonstrates visual understanding advantage

### Tier 3: Simple Authentication

**Technical Complexity**: ◗ (Medium)
**Authentication/Protection**: ◗ (Medium)

**Characteristics**:
- User credentials required (username/password)
- Login form workflows
- Session management needed
- Cookie handling required
- No advanced anti-bot measures
- No multi-factor authentication
- Standard login patterns

**Examples**:
- PayPal (https://www.paypal.com/)
- eBay (https://www.ebay.org/)
- edX (https://www.edx.org/)
- Coursera (https://www.coursera.org/)
- Figma (https://www.figma.com/)
- YouTube (https://www.youtube.com/)
- Netflix (https://www.netflix.com/)
- Apple (https://www.apple.com/)
- Spotify (https://www.spotify.com/)
- GitHub (https://www.github.com/)

**Scraping Challenges**:
- Login workflow navigation
- Form identification and filling
- Session persistence
- Cookie management
- Redirect following
- Authentication verification
- CSRF token handling

**Success Rates** (Beyond BeautifulSoup):
- [[beautifulsoup|BeautifulSoup]]: Not Supported (0%)
- [[scrapy|Scrapy]]: Not Supported (0%)
- [[claude-computer-use|Claude]]: 20%
- [[simular-ai|Simular.ai]]: 63%

**Best Tool**: Visual agents only viable option; Simular.ai significantly better than Claude

**Breakthrough**: This tier marks the critical divide—traditional tools completely fail; agents enable novice access

### Tier 4: Complex Authentication

**Technical Complexity**: ◗ (Medium)
**Authentication/Protection**: ● (High)

**Characteristics**:
- Multi-factor authentication (MFA)
- Email verification required
- SMS codes or authenticator apps
- Multi-step authentication flows
- Strong verification mechanisms
- Session security measures
- Additional security layers

**Examples**:
- Facebook (https://www.facebook.com/)
- Reddit (https://www.reddit.com/)
- Microsoft (https://www.microsoft.com/)
- Ticketmaster (https://www.ticketmaster.com/)
- Booking.com (https://www.booking.com/)
- Macy's (https://www.macys.com/)
- Bitly (https://bitly.com/)
- Amazon (https://www.amazon.com/)
- Slack (https://slack.com/)
- Expedia (https://www.expedia.com/)

**Scraping Challenges**:
- Multi-step authentication coordination
- Email/SMS code retrieval
- Time-sensitive verification codes
- Multi-tab coordination (email access)
- Complex state management
- Additional security checks
- Device verification

**Example Workflow** (Expedia MFA):
1. Navigate to login page
2. Enter credentials
3. Wait for MFA prompt
4. Switch to Gmail
5. Retrieve verification code from email
6. Return to site
7. Enter code
8. Complete authentication

**Success Rates** (Beyond BeautifulSoup):
- [[beautifulsoup|BeautifulSoup]]: Not Supported (0%)
- [[scrapy|Scrapy]]: Not Supported (0%)
- [[claude-computer-use|Claude]]: 12%
- [[simular-ai|Simular.ai]]: 70%

**Best Tool**: Simular.ai with medium effort; Claude struggles even more than on simple auth

**Notable**: Simular.ai achieves higher success (70%) on complex auth than simple auth (63%), suggesting the system handles structured multi-step flows well

### Tier 5: CAPTCHA

**Technical Complexity**: ● (High)
**Authentication/Protection**: ● (High)

**Characteristics**:
- Consistent CAPTCHA challenges on every access
- Visual puzzle solving required
- Human verification tests
- Demo/testing environments
- Reproducible CAPTCHA workflows
- Anti-automation measures

**Examples** (Demo Sites):
- reCAPTCHA v2 Checkbox Demo (https://www.google.com/recaptcha/api2/demo)
- 2Captcha Normal Demo
- reCAPTCHA v3 Scores Demo
- 2Captcha v3 Enterprise Demo
- Geetest Adaptive CAPTCHA Demo

**Why Demo Sites**: Public websites trigger CAPTCHA intermittently with varying complexity based on suspicious behavior (hard to reproduce); demo sites ensure consistent, reproducible challenges

**CAPTCHA Types**:
- Checkbox ("I am not a robot")
- Image selection puzzles
- Text recognition
- Behavioral analysis
- Risk scores
- Adaptive challenges

**Scraping Challenges**:
- Visual reasoning required
- Puzzle-solving capability
- Human-like interaction patterns
- Behavioral mimicry
- Time constraints
- Adaptive difficulty

**Success Rates** (Beyond BeautifulSoup):
- [[beautifulsoup|BeautifulSoup]]: Not Supported (0%)
- [[scrapy|Scrapy]]: Not Supported (0%)
- [[claude-computer-use|Claude]]: 5%
- [[simular-ai|Simular.ai]]: 10%

**Best Tool**: Simular.ai with high effort and low success; still best novice option

**Key Finding**: CAPTCHA remains highly effective—90-95% of scraping attempts fail even with advanced visual agents

## Success Rate Visualization

```
Tier 1 (Simple HTML):     ████████████ 93% (BS)  ██████████████ 100% (Sim)
Tier 2 (Complex HTML):    ████████░░░░ 80% (BS)  ██████████████ 100% (Sim)
Tier 3 (Simple Auth):     ░░░░░░░░░░░░  0% (BS)  █████████░░░░░  63% (Sim)
Tier 4 (Complex Auth):    ░░░░░░░░░░░░  0% (BS)  ██████████░░░░  70% (Sim)
Tier 5 (CAPTCHA):         ░░░░░░░░░░░░  0% (BS)  █░░░░░░░░░░░░░  10% (Sim)
```

Legend: BS = BeautifulSoup, Sim = Simular.ai

## Tier Transition Analysis

### Tier 1 → 2: Complexity Increase

**Change**: Simple → Complex HTML structure
**Traditional Tools**: 93% → 80% (BeautifulSoup), 82% → 20% (Scrapy)
**Agents**: 100% → 100% (Simular.ai), 100% → 57% (Claude)

**Insight**: Structural complexity affects traditional tools moderately; visual agents (Simular.ai) handle perfectly; tool-based agents (Claude) struggle

### Tier 2 → 3: Authentication Introduction

**Change**: No protection → Simple authentication
**Traditional Tools**: 80/20% → 0% (complete failure)
**Agents**: 100/57% → 63/20% (significant but partial capability)

**Insight**: This is the critical divide—authentication completely blocks traditional approaches but agents retain majority success (Simular.ai 63%)

**Democratization Impact**: Previously impossible for novices; now 63% accessible

### Tier 3 → 4: Authentication Complexity

**Change**: Simple → Complex (MFA) authentication
**Traditional Tools**: 0% → 0% (still complete failure)
**Agents**: 63/20% → 70/12% (Simular.ai actually improves; Claude degrades further)

**Insight**: Structured multi-step flows may be easier for visual agents than simple auth; Claude struggles increasingly

### Tier 4 → 5: CAPTCHA Introduction

**Change**: MFA → CAPTCHA protection
**Traditional Tools**: 0% → 0% (still complete failure)
**Agents**: 70/12% → 10/5% (dramatic drop for both)

**Insight**: CAPTCHA remains highly effective—85-90% reduction in agent success; still the strongest protection

## Manual Effort Required (MER) by Tier

### Traditional Tools (BeautifulSoup)

| Tier | Effort Level | Description |
|------|-------------|-------------|
| Tier 1 | •• Medium | 2-4 retries with tweaking |
| Tier 2 | •• Medium | Selector refinement needed |
| Tier 3-5 | N/A | Not supported with basic prompts |

### End-to-End Agents (Simular.ai)

| Tier | Effort Level | Description |
|------|-------------|-------------|
| Tier 1 | • Low | Plug-and-play, 0-1 retries |
| Tier 2 | • Low | Fully autonomous |
| Tier 3 | •• Medium | 2-4 retries, routine handling |
| Tier 4 | •• Medium | Standard multi-step flow |
| Tier 5 | ••• High | 5+ retries, rarely successful |

**Key Finding**: Agents require less effort even on scenarios where both succeed (Tiers 1-2); only agents work on Tiers 3-5

## Defensive Effectiveness by Tier

### Current State (2026)

**Tier 1-2** (No Protection):
- Highly vulnerable to all scraping approaches
- 80-100% success rates
- Fast extraction (seconds)
- Low to medium effort
- **Recommendation**: Add protection ([[webcloak|WebCloak]], authentication, or CAPTCHA)

**Tier 3** (Simple Authentication):
- Blocks traditional tools completely (100% effective)
- Blocks 37-80% of agents (depending on tool)
- Simular.ai: 63% still succeed
- Medium effort for attackers
- **Effectiveness**: Moderate—helps but not sufficient

**Tier 4** (Complex Authentication):
- Blocks traditional tools completely (100% effective)
- Blocks 30-88% of agents (depending on tool)
- Simular.ai: 70% still succeed
- Medium effort for attackers
- **Effectiveness**: Moderate—MFA doesn't significantly improve over simple auth vs agents

**Tier 5** (CAPTCHA):
- Blocks traditional tools completely (100% effective)
- Blocks 90-95% of agents
- Only 5-10% agent success
- High effort for attackers
- **Effectiveness**: High—most effective protection measured

### Recommendations by Content Sensitivity

**Public Content** (low sensitivity):
- Tier 1-2 acceptable
- Consider robots.txt and rate limiting
- Official APIs for legitimate use

**Moderate Sensitivity**:
- Minimum Tier 3 (simple authentication)
- Better: Tier 3 + [[webcloak|WebCloak]]-style obfuscation
- Layered defense approach

**High Sensitivity**:
- Tier 4 (complex authentication) or Tier 5 (CAPTCHA)
- Combine: Authentication + CAPTCHA + WebCloak
- Behavioral detection
- Rate limiting with IP tracking

## Research Applications

### Benchmarking Tool Capabilities

The tier system enables systematic evaluation:
- Each tier tests specific capabilities
- Progressive difficulty reveals strengths/weaknesses
- Comparable metrics across tools
- Identifies capability gaps

**Example**: Simular.ai 100% on Tier 2 vs Claude 57% reveals visual understanding advantage

### Measuring Democratization

Tier-by-tier success rates quantify accessibility:
- Tier 1-2: Novices approach expert level (82-93%)
- Tier 3-4: Novices achieve majority success (63-70%) where previously 0%
- Tier 5: Still challenging (5-10%) but non-zero

**Impact**: Democratization measured empirically across security levels

### Informing Defense Design

Tier effectiveness guides protection choice:
- Tier 1-2 insufficient for sensitive content
- Tier 3-4 help but not sufficient alone
- Tier 5 most effective but UX trade-off
- Layering recommended

### Comparing Research Studies

Common framework for cross-study comparison:
- Beyond BeautifulSoup: Tier-based evaluation
- [[autodata|AutoData]]: Domain-based evaluation (academic, finance, sports)
- [[webcloak|WebCloak]]: Defense effectiveness at Tier 1-2 level

**Future**: Unified benchmarks spanning both tier types

## Limitations and Extensions

### Current Limitations

**Static Classification**:
- Sites may combine characteristics from multiple tiers
- Real-world complexity not always clear-cut
- Hybrid protections not captured

**Demo CAPTCHA**:
- Production CAPTCHA may behave differently
- Intermittent triggering not evaluated
- Varying difficulty not captured

**Limited Scope**:
- Only 35 websites total
- May not represent all variations
- English-language bias

### Potential Extensions

**Additional Tiers**:
- Tier 2.5: JavaScript-heavy SPAs
- Tier 3.5: OAuth/social login
- Tier 6: Advanced anti-bot (behavioral detection, fingerprinting)
- Tier 7: [[webcloak|WebCloak]]-style structural obfuscation

**Sub-Categorization**:
- Within-tier difficulty gradations
- Domain-specific challenges
- Geographic/language variations

**Longitudinal Tracking**:
- Tier success rates over time
- Tool evolution impact
- Defense adaptation effectiveness

## Related Pages

### Research
- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup Research Paper]]
- [[novice-web-scraping-benchmark|Novice Web Scraping Benchmark]]
- [[scraping-democratization|Scraping Democratization]]

### Tools
- [[beautifulsoup|BeautifulSoup]] (traditional, Tiers 1-2)
- [[scrapy|Scrapy]] (traditional, Tiers 1-2)
- [[claude-computer-use|Claude]] (agent, all tiers, lower performance)
- [[simular-ai|Simular.ai]] (agent, all tiers, best performance)

### Concepts
- [[authentication-bypass-agents|Authentication Bypass with Agents]]
- [[captcha-solving-with-llms|CAPTCHA Solving with LLMs]]
- [[anti-bot-protection|Anti-Bot Protection]]

### Defense
- [[webcloak|WebCloak]] (potential Tier 6-7 protection)

## Sources

- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup Paper]]
