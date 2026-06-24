---
title: Scrapy
tags: [technology, web-scraping, python-framework, crawling]
created: 2026-06-24
updated: 2026-06-24
---

# Scrapy

A comprehensive Python framework for web scraping that supports asynchronous requests, structured crawling rules, and native export formats, evaluated as a traditional scraping baseline in the [[beyond-beautifulsoup-paper|Beyond BeautifulSoup research]].

## Overview

Scrapy is a full-featured web crawling and scraping framework that provides a complete infrastructure for extracting data from websites, including built-in support for handling requests, following links, managing sessions, and exporting data.

## Technical Details

**Version Evaluated**: Scrapy 2.13.1 with Python 3.10

**Core Capabilities**:
- Asynchronous request handling
- Link following and crawling rules
- Built-in session and cookie management
- Middleware architecture
- Structured data export (CSV, JSON, XML)
- Item pipelines for data processing
- Default fetching without client-side script execution

**Architecture**:
```python
import scrapy

class ProductSpider(scrapy.Spider):
    name = 'products'
    start_urls = ['https://example.com/products']
    
    def parse(self, response):
        for product in response.css('div.product-item'):
            yield {
                'name': product.css('.product-name::text').get(),
                'price': product.css('.price::text').get(),
                'rating': product.css('.rating::attr(data-rating)').get(),
            }
        
        # Follow pagination
        next_page = response.css('a.next::attr(href)').get()
        if next_page:
            yield response.follow(next_page, self.parse)
```

## Performance in Beyond BeautifulSoup Benchmark

### Extraction Success Rate (ESR)

| Category | Success Rate |
|----------|-------------|
| Simple HTML | 82% |
| Complex HTML | 20% |
| Simple Authentication | Not Supported |
| Complex Authentication | Not Supported |
| CAPTCHA | Not Supported |

### Key Findings

**Strengths**:
- **High success on simple HTML**: 82% success rate
- **Fast execution**: Under 2 seconds for simple static sites
- **Structured framework**: Built-in patterns for common tasks
- **Scalability**: Designed for large-scale crawling

**Limitations**:
- **Lower success than BeautifulSoup**: 82% vs 93% on simple HTML
- **Struggles with complex HTML**: Only 20% success on complex structures
- **No authentication support**: Cannot handle login workflows with basic prompts
- **Steeper learning curve**: More complex than BeautifulSoup for simple tasks

**Manual Effort Required**:
- Simple HTML: Medium effort (•• - 2-4 retries with tweaking)
- Complex HTML: Medium effort (•• - more configuration needed)

### Surprising Result: Lower Performance on Complex HTML

**Observation**: Scrapy achieved only 20% success on complex HTML compared to BeautifulSoup's 80%

**Possible Reasons**:
- More rigid structure expectations
- Default settings less tolerant of varied layouts
- Spider configuration overhead
- CSS selector specificity issues
- Framework complexity adding failure points

## Use Cases

**Ideal For**:
- Large-scale crawling projects
- Multi-page data extraction
- Structured data pipelines
- Sites with consistent patterns
- Projects needing middleware/extensions
- Team-based scraping infrastructure

**Not Suitable For**:
- Quick one-off extraction tasks
- Authentication-protected content (without advanced configuration)
- JavaScript-heavy dynamic sites
- CAPTCHA challenges
- Highly varied page structures

## Comparison with Other Tools

### Scrapy vs BeautifulSoup

**Scrapy**:
- Full framework with infrastructure
- Built-in session/cookie management
- Better for large-scale projects
- 82% success on simple HTML
- Lower 20% on complex HTML

**[[beautifulsoup|BeautifulSoup]]**:
- Lightweight parsing library
- Manual request handling
- Better for simple tasks
- 93% success on simple HTML
- Higher 80% on complex HTML

**Recommendation**: Use BeautifulSoup for flexibility, Scrapy for large-scale structured crawling.

### Scrapy vs LLM Agents

**Scrapy** (traditional):
- Fast (< 2 seconds) on static content
- Medium manual effort required
- Completely fails on authentication
- Best for experienced programmers

**LLM Agents**:
- Slower (10-20 seconds) on static content
- Low manual effort (plug-and-play)
- 63-70% success on authentication
- Accessible to novices

## LLM-Assisted Usage

In [[llm-assisted-scripting|LLM-Assisted Scripting]] workflows:

**Process**:
1. User describes crawling task
2. LLM generates Scrapy spider code
3. User runs spider with scrapy commands
4. Iterative refinement of selectors and rules

**Example Prompt**:
```
Create a Scrapy spider to crawl [WEBSITE], extracting article 
titles, authors, and publication dates. Handle pagination to get 
all articles from the last month. Export to JSON.
```

**Challenges**:
- More complex code structure than BeautifulSoup
- Requires understanding of Scrapy project structure
- Configuration overhead for novices
- More points of failure in generated code

## Advanced Features

**Middleware**:
- Custom request/response processing
- User-agent rotation
- Proxy support
- Rate limiting

**Item Pipelines**:
- Data cleaning and validation
- Deduplication
- Database storage
- External API integration

**Extensions**:
- Statistics collection
- Logging
- Email notifications
- Custom monitoring

**Limitations in Benchmark**:
These advanced features not utilized in Beyond BeautifulSoup evaluation (simulating novice use with basic prompts).

## Historical Context

**Development**: Created by Scrapy developers community, actively maintained since 2008

**Position in Ecosystem**:
- Industry standard for production scraping
- More comprehensive than BeautifulSoup
- Pre-dates LLM-assisted era
- Still relevant for specific use cases despite agent alternatives

## Evolution and Future

**Current State**:
- Remains powerful for large-scale crawling
- Integration with async Python (Twisted)
- Active community and plugins

**LLM Era Challenges**:
- Higher complexity barrier vs. natural language prompts
- Configuration overhead vs. agent automation
- Less suitable for novice democratization
- Still valuable for expert users and production systems

**Potential Adaptations**:
- LLM-generated Scrapy configurations
- Natural language rule specification
- Integration with agent orchestration
- Hybrid workflows (agent auth + Scrapy extraction)

## Related Pages

### Concepts
- [[llm-assisted-scripting|LLM-Assisted Scripting]]
- [[llm-to-script|LLM-to-Script]]
- [[traditional-vs-agent-scraping|Traditional vs Agent-Based Scraping]]
- [[web-crawling|Web Crawling]]

### Technologies
- [[beautifulsoup|BeautifulSoup]]
- [[claude-computer-use|Claude]]
- [[simular-ai|Simular.ai]]

### Papers
- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup Research Paper]]

## Sources

- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup Paper]]
- Official documentation: docs.scrapy.org
