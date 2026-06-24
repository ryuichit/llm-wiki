---
title: Web Agent (WEB)
tags: [entity, agent, autodata, multi-agent-system]
created: 2026-06-24
updated: 2026-06-24
---

# Web Agent (WEB)

A specialized agent in [[autodata|AutoData's]] [[research-squad|Research Squad]] responsible for navigating and analyzing web sources to identify data locations, understand page structures, and extract relevant knowledge for the collection blueprint.

## Role and Responsibilities

The Web Agent serves as AutoData's webpage intelligence specialist. It visits target websites, analyzes HTML structures, identifies where desired data resides, and documents the navigation paths needed to access that data. Unlike traditional web scrapers that execute predefined extraction rules, WEB dynamically understands page layouts and adapts to diverse structures.

Key responsibilities include:
- **Webpage analysis**: Parsing HTML/CSS structures to understand layout
- **Data localization**: Identifying specific elements containing target data
- **Navigation mapping**: Documenting paths through multi-page websites
- **Structure documentation**: Describing DOM patterns for extraction code
- **Dynamic content detection**: Identifying JavaScript-rendered elements
- **Anti-scraping assessment**: Recognizing protective measures that may complicate collection

For example, analyzing an academic paper page, WEB might identify: title in `<h1 class="paper-title">`, authors in `<div class="author-list">`, abstract in `<section id="abstract">`, and pagination links in `<nav class="pages">`.

## Architecture Integration

WEB operates within the Research Squad alongside [[plan-agent|PLN]], [[tool-agent|TOL]], and [[blueprint-agent|BLU]]. It receives webpage URLs from the [[manager-agent|Manager Agent]], conducts analysis, and provides structural insights to the [[blueprint-agent|BLU]] for blueprint synthesis.

The Web Agent's findings directly inform the [[engineering-agent|Engineering Agent's]] crawler implementation, ensuring generated code targets the correct DOM elements and follows proper navigation sequences.

## Tools and Capabilities

The Web Agent employs multiple tools for webpage analysis:
- **Browser automation**: Headless browser for rendering JavaScript-heavy pages
- **HTML parsing**: BeautifulSoup-style DOM traversal and querying
- **CSS selector identification**: Locating elements through optimal selectors
- **XPath generation**: Creating robust path expressions for data extraction
- **Screenshot capture**: Visual analysis of page layout
- **Network monitoring**: Observing AJAX calls and API endpoints

Following the [[react-paradigm|ReAct paradigm]], WEB iteratively explores pages:
1. **Reasoning**: Hypothesizes where target data might appear
2. **Acting**: Navigates to pages and inspects elements
3. **Observation**: Confirms or revises understanding based on findings
4. **Documentation**: Records successful extraction patterns

## Interaction with Other Agents

**Direct collaboration**:
- Receives target URLs and data requirements from [[manager-agent|MGR]]
- Provides structural analysis to [[blueprint-agent|BLU]]
- Coordinates with [[tool-agent|TOL]] when API endpoints are discovered
- Informs [[plan-agent|PLN]] about webpage complexity

**Downstream impact**:
- WEB's DOM selectors are used directly by [[engineering-agent|ENG]] in crawler code
- Navigation paths guide [[test-agent|TST]] test case design
- Data location accuracy affects [[validation-agent|VAL]] verification success

## Communication via OHCache

The Web Agent benefits from [[ohcache|OHCache's]] architecture:
- **Selective routing**: Receives only relevant URLs, not entire crawl histories from other agents
- **Structured reporting**: Uses [[oriented-hyperedge-formatter|Oriented Hyperedge Formatter]] to communicate findings
- **Local caching**: Stores page structure patterns in [[local-cache-system|Local Cache System]] for similar sites
- **Token efficiency**: Sends summarized structural insights rather than full HTML content

This targeted communication is crucial since raw HTML can consume thousands of tokens per page.

## Dual-Mode Collection Support

WEB supports both of AutoData's collection modes:
- **Web crawling mode**: Analyzes DOM structure for scraper implementation
- **API discovery mode**: Identifies REST endpoints by monitoring network traffic

When WEB detects that data is loaded via AJAX rather than server-rendered HTML, it reports this to [[tool-agent|TOL]] to investigate API-based collection as an alternative or complement to crawling.

## Performance Impact

The Web Agent's accuracy directly affects data collection success. Precise element identification leads to robust extractors that handle variations across pages. In the [[instruct2ds|Instruct2DS]] benchmark, AutoData achieved:
- **Academic domain**: 91.85% F1 score (complex nested paper structures)
- **Finance domain**: 96.75% F1 score (well-structured tabular data)
- **Sports domain**: 90.14% F1 score (dynamic score updates)

WEB's ability to adapt to these diverse structures was essential for consistent performance.

## Related Agents

**Research Squad**:
- [[manager-agent|Manager Agent (MGR)]] - Provides target URLs
- [[plan-agent|Plan Agent (PLN)]] - Defines data requirements
- [[tool-agent|Tool Agent (TOL)]] - Discovers API alternatives
- [[blueprint-agent|Blueprint Agent (BLU)]] - Synthesizes structural insights

**Development Squad**:
- [[engineering-agent|Engineering Agent (ENG)]] - Implements based on WEB's findings
- [[test-agent|Test Agent (TST)]] - Tests extraction from identified elements
- [[validation-agent|Validation Agent (VAL)]] - Validates collected data

## See Also

- [[autodata|AutoData System]]
- [[autodata-paper|AutoData Research Paper]]
- [[research-squad|Research Squad]]
- [[ohcache|OHCache]]
- [[react-paradigm|ReAct Paradigm]]
- [[dual-mode-web-collection|Dual-Mode Web Collection]]
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]]
