---
title: Claude (Computer Use / Tool-Enabled Agent)
tags: [technology, llm-agent, web-agent, anthropic, computer-use]
created: 2026-06-24
updated: 2026-06-24
---

# Claude (Computer Use / Tool-Enabled Agent)

A general-purpose LLM agent by Anthropic with built-in tool-use capabilities, evaluated in the [[beyond-beautifulsoup-paper|Beyond BeautifulSoup research]] as an [[end-to-end-llm-agents|end-to-end LLM agent]] for web scraping tasks.

## Overview

Claude (specifically Claude 3.5 Sonnet) is a large language model that can act as an autonomous agent through built-in tools for web browsing, DOM access, structured extraction, and automated retries. Users provide natural language goal prompts and the agent completes tasks end-to-end with minimal intervention.

## Technical Details

**Version Evaluated**: Claude 3.5 Sonnet

**Core Capabilities**:
- Natural language instruction following
- Built-in web browsing tools
- DOM access and element selection
- Structured data extraction
- Automated retry mechanisms
- Error detection and recovery
- Multi-step workflow execution

**Agent Architecture**:
- Tool-enabled LLM (not visual browser agent)
- Internal reasoning and planning
- Iterative action-observation loops
- Context management across steps

## Performance in Beyond BeautifulSoup Benchmark

### Extraction Success Rate (ESR)

| Category | Success Rate |
|----------|-------------|
| Simple HTML | 100% |
| Complex HTML | 57% |
| Simple Authentication | 20% |
| Complex Authentication | 12% |
| CAPTCHA | 5% |

### Key Findings

**Strengths**:
- **Perfect on simple HTML**: 100% success rate (matches visual agents)
- **Zero-shot capability**: Works without specific training on scraping tasks
- **Natural language interface**: Accessible to non-experts
- **Automated execution**: Minimal user intervention needed

**Limitations**:
- **Moderate complex HTML performance**: 57% success (lower than Simular.ai's 100%)
- **Low authentication success**: 20% simple, 12% complex (much lower than Simular.ai's 63-70%)
- **Minimal CAPTCHA capability**: 5% success (visual challenges difficult)
- **No real browser**: Lacks visual understanding advantages

**Manual Effort Required**:
- Simple HTML: Low effort (• - plug-and-play)
- Complex HTML: Medium effort (•• - some prompt refinement)
- Simple Authentication: High effort (••• - 5+ retries, extensive debugging)
- Complex Authentication: High effort (••• - substantial manual debugging)
- CAPTCHA: High effort (••• - rarely successful even with many attempts)

### Execution Time

- Simple HTML: 10-20 seconds (10-20× slower than traditional tools)
- Complex scenarios: Variable (seconds to minutes)
- Trade-off: Slower than traditional but handles cases they cannot

## Comparison with Simular.ai

### Claude vs Simular.ai

**Claude**:
- Tool-enabled LLM agent
- No real browser execution
- 100% simple HTML, 57% complex HTML
- 20% simple auth, 12% complex auth
- Better for simple tasks, struggles with complex ones

**[[simular-ai|Simular.ai]]**:
- Visual web agent with real browser
- DOM rendering and visual understanding
- 100% simple HTML, 100% complex HTML
- 63% simple auth, 70% complex auth
- Superior overall performance, especially on authentication

**Key Difference**: Simular.ai's visual browser-based approach significantly outperforms Claude's tool-enabled approach on complex and authenticated sites.

## Use Cases in Beyond BeautifulSoup Context

**Suitable For**:
- Simple HTML extraction where automation desired
- Tasks where 10-20 second delay acceptable
- Non-expert users preferring natural language
- Exploratory scraping without code

**Not Recommended For**:
- Authentication-protected sites (low success rates)
- Complex HTML with deep nesting (moderate success)
- CAPTCHA-protected content (minimal success)
- Time-critical extraction (too slow)

## Example Usage

### Simple HTML Extraction

**User Prompt**:
```
Load the home pages of the following websites:
- https://thehackernews.com/
- https://www.wikipedia.org/

For each website, extract the main headlines and record success/failure.
Save results to CSV.
```

**Agent Behavior**:
1. Navigate to each URL
2. Identify headline elements using tools
3. Extract text content
4. Structure into CSV format
5. Report success

**Success Rate**: 100% on simple static sites

### Authentication Attempt

**User Prompt**:
```
Login to https://www.github.com with username <> and password <>.
After successful login, extract repository names from the dashboard.
```

**Agent Behavior**:
1. Navigate to login page
2. Attempt to identify and fill login form
3. Submit credentials
4. Check for successful authentication
5. Extract data if authenticated

**Success Rate**: Only 20% - struggles with login flows, session management, and authentication verification

## Advantages

**Accessibility**:
- Natural language only - no programming needed
- Immediate availability through Anthropic's platform
- Low learning curve
- Conversational refinement of tasks

**Flexibility**:
- General-purpose agent, not specialized for scraping
- Adapts to varied task descriptions
- No code generation/execution overhead
- Direct tool use

**Transparency**:
- Reasoning steps visible
- Tool calls observable
- Error messages interpretable

## Limitations

**Technical**:
- No real browser (lacks visual understanding)
- Limited authentication handling
- No CAPTCHA solving capability
- Slower than traditional tools on simple tasks

**Practical**:
- High manual effort on complex scenarios
- Inconsistent success on authentication
- Requires prompt refinement and debugging
- May need multiple attempts (5+ retries common)

## Evolution and Position

**Current Generation**:
- Part of Claude 3.5 family
- Anthropic's first computer use capabilities
- General agent, not scraping-specific

**Compared to Traditional Tools**:
- More accessible (natural language)
- Slower execution
- Better on simple tasks, worse on complex/authenticated

**Compared to Visual Agents**:
- Inferior performance to Simular.ai
- Lacks visual reasoning advantages
- Tool-based vs browser-based approach

**Future Potential**:
- Visual capabilities (Claude can process images)
- Improved authentication handling
- Better multi-step reasoning
- Enhanced tool integration

## Related Pages

### Concepts
- [[end-to-end-llm-agents|End-to-End LLM Agents]]
- [[llm-based-web-agents|LLM-based Web Agents]]
- [[computer-use-agents|Computer Use Agents]]
- [[authentication-bypass-agents|Authentication Bypass with Agents]]

### Technologies
- [[simular-ai|Simular.ai]] (superior visual agent)
- [[beautifulsoup|BeautifulSoup]] (traditional alternative)
- [[scrapy|Scrapy]] (traditional alternative)

### Comparisons
- [[las-vs-ela|LLM-Assisted Scripting vs End-to-End Agents]]
- [[claude-vs-simular|Claude vs Simular.ai]]

### Papers
- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup Research Paper]]

## Sources

- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup Paper]]
- Anthropic Claude documentation: anthropic.com/claude
