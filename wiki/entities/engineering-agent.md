---
title: Engineering Agent (ENG)
tags: [entity, agent, autodata, multi-agent-system]
created: 2026-06-24
updated: 2026-06-24
---

# Engineering Agent (ENG)

A specialized agent in [[autodata|AutoData's]] [[development-squad|Development Squad]] responsible for implementing the data collection pipeline by generating executable code based on the research blueprint.

## Role and Responsibilities

The Engineering Agent serves as AutoData's code generator and implementation specialist. After receiving the comprehensive blueprint from the [[blueprint-agent|Blueprint Agent]], ENG translates strategic specifications into working Python code that executes web crawling, REST API calls, or hybrid data collection approaches.

Key responsibilities include:
- **Code generation**: Writing executable Python scripts for data collection
- **Web crawler implementation**: Creating scrapers using BeautifulSoup, Selenium, or Scrapy
- **API integration**: Implementing REST API calls with proper authentication and error handling
- **Data parsing**: Extracting structured data from HTML, JSON, or XML responses
- **Error handling**: Implementing retry logic, timeout management, and graceful failures
- **Rate limiting**: Ensuring compliance with website terms of service and API quotas
- **Library integration**: Incorporating tools recommended by [[tool-agent|TOL]]

For example, ENG might generate a crawler that navigates academic paper pages, extracts author names via CSS selectors identified by [[web-agent|WEB]], handles pagination, and outputs structured JSON data.

## Architecture Integration

ENG is the first agent activated in the Development Squad workflow. It receives the consolidated blueprint from the [[manager-agent|Manager Agent]], implements the collection code, and passes it to the [[test-agent|Test Agent]] for verification before [[validation-agent|Validation Agent]] executes it on real data.

The Engineering Agent's output is executable code stored in [[ohcache|OHCache's]] [[local-cache-system|Local Cache System]], making it available to TST for testing and VAL for validation without redundant message passing.

## Tools and Capabilities

The Engineering Agent employs multiple implementation capabilities:
- **Python code generation**: Creating well-structured, documented scripts
- **Library selection**: Choosing appropriate tools (requests, selenium, BeautifulSoup, pandas)
- **HTML parsing**: Implementing DOM navigation and CSS/XPath selectors
- **API client creation**: Building REST API wrappers with authentication
- **Data transformation**: Converting raw responses into structured formats
- **File I/O**: Saving collected data to CSV, JSON, or database formats

Following the [[react-paradigm|ReAct paradigm]], ENG operates iteratively:
1. **Reasoning**: Analyzes blueprint to determine optimal implementation approach
2. **Acting**: Generates code modules (crawler, parser, validator)
3. **Observation**: Reviews generated code for completeness
4. **Refinement**: Adjusts code based on feedback from [[test-agent|TST]]

## Interaction with Other Agents

**Direct collaboration**:
- Receives blueprint from [[manager-agent|MGR]]
- Provides code to [[test-agent|TST]] for unit testing
- Receives feedback when tests fail, iterates on implementation
- Coordinates with [[validation-agent|VAL]] regarding data format

**Upstream dependencies**:
- Implements strategies defined by [[blueprint-agent|BLU]]
- Uses DOM selectors identified by [[web-agent|WEB]]
- Integrates tools recommended by [[tool-agent|TOL]]
- Follows task structure from [[plan-agent|PLN]]

The Engineering Agent's code quality directly determines AutoData's data collection success rate.

## Communication via OHCache

ENG benefits from [[ohcache|OHCache's]] architecture:
- **Blueprint access**: Receives comprehensive specification without needing raw research data
- **Code storage**: Stores generated code in local cache for TST and VAL access
- **Selective updates**: Only receives feedback on failed tests, not all test results
- **Version tracking**: Maintains code versions as implementation evolves

This structured communication via [[oriented-hyperedge-formatter|Oriented Hyperedge Formatter]] ensures ENG has exactly the information needed for implementation without information overload.

## Dual-Mode Implementation

ENG implements both of AutoData's collection modes:

**Web crawling**:
```python
# Example: Using BeautifulSoup with requests
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')
titles = soup.select('.paper-title')
```

**REST API calls**:
```python
# Example: API with authentication
headers = {'Authorization': f'Bearer {api_key}'}
response = requests.get(api_endpoint, headers=headers)
data = response.json()
```

**Hybrid approach**:
- Combines both methods when blueprint specifies
- Uses API for metadata, scraping for supplementary data
- Implements fallback from API to scraping if rate-limited

## Code Quality and Robustness

ENG generates production-quality code with:
- **Error handling**: Try-except blocks for network failures, parsing errors
- **Logging**: Detailed logs for debugging and monitoring
- **Configurability**: Parameterized URLs, selectors, API keys
- **Documentation**: Comments explaining logic and assumptions
- **Modularity**: Separate functions for fetching, parsing, saving

This quality is essential since [[validation-agent|VAL]] executes the code on real websites where unexpected conditions frequently occur.

## Performance Impact

The Engineering Agent's implementation efficiency directly affects AutoData's metrics:
- **Speed**: Average 5.58 minutes per task includes ENG's generation time
- **Cost**: Efficient code reduces API calls and token usage during validation
- **Success rate**: Robust error handling prevents cascade failures

In the [[instruct2ds|Instruct2DS]] benchmark, ENG's generated code achieved:
- **Academic domain**: 91.85% F1 score
- **Finance domain**: 96.75% F1 score  
- **Sports domain**: 90.14% F1 score
- **Average**: 92.91% F1 across all domains

This outperforms human-written scrapers (88.91% F1) and other AI coding assistants like [[manus|Manus]], [[cursor|Cursor]], and [[cline|Cline]].

## Related Agents

**Development Squad**:
- [[manager-agent|Manager Agent (MGR)]] - Provides blueprint
- [[test-agent|Test Agent (TST)]] - Tests generated code
- [[validation-agent|Validation Agent (VAL)]] - Executes code on real data

**Research Squad**:
- [[blueprint-agent|Blueprint Agent (BLU)]] - Defines implementation strategy
- [[web-agent|Web Agent (WEB)]] - Identifies DOM selectors used in code
- [[tool-agent|Tool Agent (TOL)]] - Recommends libraries imported in code
- [[plan-agent|Plan Agent (PLN)]] - Defines task structure followed in implementation

## See Also

- [[autodata|AutoData System]]
- [[autodata-paper|AutoData Research Paper]]
- [[development-squad|Development Squad]]
- [[ohcache|OHCache]]
- [[react-paradigm|ReAct Paradigm]]
- [[dual-mode-web-collection|Dual-Mode Web Collection]]
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]]
