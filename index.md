# LLM Wiki - Index

Last updated: 2026-06-24

## Overview

This is a personal knowledge base maintained collaboratively between human curation and AI assistance. Sources are ingested, knowledge is extracted and structured, and the wiki grows over time.

## Statistics

- Total pages: 132 (8 sources + 66 entities + 40 concepts + 12 comparisons + 6 supporting)
- Total sources: 8
- Last ingest: 2026-06-24 (WebAgents Survey Paper)
- Last lint: 2026-06-24 (Comprehensive lint pass - 195→60 broken links)

## Sources

Summary pages for ingested documents.

### Research Papers
- [[webcloak-paper|WebCloak: Characterizing and Mitigating Threats from LLM-Driven Web Agents]] - Security research on LLM-driven web scraping and defense mechanisms
- [[autodata-paper|AutoData: A Multi-Agent System for Open Web Data Collection]] - Multi-agent system for automated web data collection from natural language instructions
- [[webdancer-paper|WebDancer: Towards Autonomous Information Seeking Agency]] - End-to-end framework for building autonomous research agents using synthetic data and reinforcement learning
- [[web-agent-rl-constraints-paper|Web Agent Agentic RL Decision Model Under Multi-Cost and Failure Risk Constraints]] - Constrained reinforcement learning framework unifying multi-dimensional costs (requests, latency, failures) with CVaR-based tail risk control for practical web agent deployment
- [[robots-txt-gatekeeping-paper|Is Misinformation More Open? A Study of robots.txt Gatekeeping on the Web]] - First longitudinal study of robots.txt gatekeeping differences between reputable and misinformation websites, revealing stark asymmetry in AI crawler blocking (WWW '26)
- [[beyond-beautifulsoup-paper|Beyond BeautifulSoup: Benchmarking LLM-Powered Web Scraping for Everyday Users]] - Systematic evaluation of scraping democratization measuring what non-expert users can accomplish with off-the-shelf LLM tools across 35 websites spanning five security tiers (arXiv 2026)
- [[autocrawler-paper|AUTOCRAWLER: A Progressive Understanding Web Agent for Web Crawler Generation]] - Two-stage framework using progressive understanding (top-down + step-back operations) for automatic crawler generation on vertical information pages (arXiv 2024)

### Survey Papers
- [[webagents-survey-paper|A Survey of WebAgents: Towards Next-Generation AI Agents for Web Automation with Large Foundation Models]] - First comprehensive survey systematically reviewing 45+ WebAgent systems across architectures (perception, planning & reasoning, execution), training strategies (training-free, GUI comprehension, task-specific fine-tuning, post-training RL), and trustworthiness (safety, privacy, generalizability) - arXiv May 2025

## Entities

People, organizations, technologies, and other named entities.

### People
- [[xinfeng-li|Xinfeng Li]] - First author of WebCloak research
- [[wei-dong|Wei Dong]] - Corresponding author of WebCloak research
- [[tianyi-ma|Tianyi Ma]] - Co-first author of AutoData research
- [[yiyue-qian|Yiyue Qian]] - Co-first author of AutoData research
- [[yifan-ding|Yifan Ding]] - Co-first author of AutoData research
- [[yanfang-ye|Yanfang Ye]] - Corresponding author of AutoData research
- [[jialong-wu|Jialong Wu]] - Co-first author of WebDancer research
- [[baixuan-li|Baixuan Li]] - Co-first author of WebDancer research
- [[runnan-fang|Runnan Fang]] - Co-first author of WebDancer research
- [[wenbiao-yin|Wenbiao Yin]] - Co-first author of WebDancer research
- [[qianli-ma|Qianli Ma]] - Corresponding author of Web Agent RL Constraints research
- [[limengxi-yue|Limengxi Yue]] - Co-author of Web Agent RL Constraints research
- [[shuyang-xu|Shuyang Xu]] - Co-author of Web Agent RL Constraints research
- [[yanpei-shi|Yanpei Shi]] - Co-author of Web Agent RL Constraints research
- [[hongrui-liu|Hongrui Liu]] - Co-author of Web Agent RL Constraints research
- [[nicolas-steinacker-olsztyn|Nicolas Steinacker-Olsztyn]] - First author of robots.txt gatekeeping research
- [[devashish-gosain|Devashish Gosain]] - Co-author of robots.txt gatekeeping research
- [[ha-dao|Ha Dao]] - Corresponding author of robots.txt gatekeeping research
- [[arth-bhardwaj|Arth Bhardwaj]] - First author of Beyond BeautifulSoup research
- [[nirav-diwan|Nirav Diwan]] - Co-author of Beyond BeautifulSoup research
- [[gang-wang|Gang Wang]] - Co-author of Beyond BeautifulSoup research
- [[wenhao-huang|Wenhao Huang]] - Co-first author of AUTOCRAWLER research
- [[liangbo-ning|Liangbo Ning]] - Lead author of WebAgents survey
- [[wenqi-fan|Wenqi Fan]] - Corresponding author of WebAgents survey

### Organizations
- [[nanyang-technological-university|Nanyang Technological University]] - Lead research institution (WebCloak)
- [[hong-kong-polytechnic-university|Hong Kong Polytechnic University]] - Collaborating institution (WebCloak)
- [[university-of-hawaii|University of Hawaii at Manoa]] - Collaborating institution (WebCloak)
- [[university-of-notre-dame|University of Notre Dame]] - Lead research institution (AutoData)
- [[amazon|Amazon]] - Industry collaborator (AutoData)
- [[ibm-research|IBM Research]] - Research collaborator (AutoData)
- [[tongyi-lab|Tongyi Lab]] - Alibaba Group research lab (WebDancer)
- [[alibaba-group|Alibaba Group]] - Parent organization of Tongyi Lab and AUTOCRAWLER collaborator
- [[university-of-massachusetts-boston|University of Massachusetts Boston]] - Lead institution (Web Agent RL Constraints)
- [[university-of-massachusetts-amherst|University of Massachusetts Amherst]] - Collaborating institution (Web Agent RL Constraints)
- [[cornell-university|Cornell University]] - Collaborating institution (Web Agent RL Constraints)
- [[university-of-southern-california|University of Southern California]] - Collaborating institution (Web Agent RL Constraints)
- [[university-of-michigan|University of Michigan - Ann Arbor]] - Collaborating institution (Web Agent RL Constraints)
- [[saarland-university|Saarland University]] - Research institution (robots.txt gatekeeping)
- [[iit-bombay|IIT Bombay]] - Collaborating institution (robots.txt gatekeeping)
- [[max-planck-institute-informatics|Max Planck Institute for Informatics]] - Lead institution (robots.txt gatekeeping)
- [[saint-francis-high-school|Saint Francis High School]] - Affiliated institution (Beyond BeautifulSoup)
- [[university-of-illinois-urbana-champaign|University of Illinois Urbana-Champaign]] - Research institution (Beyond BeautifulSoup)
- [[fudan-university|Fudan University]] - Lead research institution (AUTOCRAWLER)
- [[media-bias-fact-check|Media Bias/Fact Check]] - Content credibility classification organization
- [[internet-archive|Internet Archive]] - Web archive for longitudinal analysis

### Technologies
- [[browser-use|Browser-Use]] - LLM-based web agent framework (99.3% recall)
- [[crawl4ai|Crawl4AI]] - LLM-native crawler (98% recall)
- [[gemini|Gemini]] - Google's LLM (84.2% recall in L2S)
- [[autodata|AutoData]] - Multi-agent system for web data collection (92.91% F1 average)
- [[ohcache|OHCache]] - Oriented Hypergraph Cache System for multi-agent coordination
- [[instruct2ds|Instruct2DS]] - Benchmark for web data collection systems
- [[webdancer|WebDancer]] - End-to-end trained information-seeking agent (51.5% GAIA, 47.9% WebWalkerQA)
- [[dapo|DAPO]] - Decoupled Clip and Dynamic Sampling Policy Optimization algorithm
- [[crawlqa|CRAWLQA]] - Web crawling-based QA generation (60K samples)
- [[e2hqa|E2HQA]] - Easy-to-hard QA progression (40K samples)
- [[beautifulsoup|BeautifulSoup]] - Lightweight Python HTML parsing library (93% success on simple HTML)
- [[scrapy|Scrapy]] - Comprehensive Python web scraping framework (82% success on simple HTML)
- [[claude-computer-use|Claude]] - Anthropic's tool-enabled LLM agent (100% simple HTML, 20-12% auth)
- [[simular-ai|Simular.ai]] - Visual web agent with real browser (100% HTML, 63-70% auth)
- [[autocrawler|AUTOCRAWLER]] - Progressive understanding crawler generation framework (71.56% correct with GPT-4)

## Concepts

Ideas, techniques, theories, and abstract concepts.

### Security & Defense
- [[webcloak|WebCloak]] - Dual-layer defense system against LLM-driven scraping
- [[dynamic-structural-obfuscation|Dynamic Structural Obfuscation]] - HTML randomization defense technique
- [[semantic-labyrinth|Semantic Labyrinth]] - Adversarial prompt-based confusion defense
- [[agent-as-attacker|Agent-as-Attacker Paradigm]] - Security paradigm shift from agent-as-victim

### Web Content Control & Ethics
- [[robots-txt|robots.txt]] - Voluntary protocol for crawler access control (RFC 9309, 1994-present)
- [[web-gatekeeping|Web Gatekeeping]] - Mechanisms for controlling automated access to web content
- [[ai-crawlers|AI Crawlers]] - Bots collecting content for AI training (GPTBot, ClaudeBot, Google-Extended, etc.)
- [[active-blocking|Active Blocking]] - Technical enforcement based on User-agent detection
- [[misinformation-accessibility|Misinformation Accessibility]] - Asymmetry where low-quality content is more accessible to AI crawlers than high-quality content
- [[llm-training-data-ethics|LLM Training Data Ethics]] - Ethical considerations in AI data collection
- [[data-collection-ethics|Data Collection Ethics]] - Responsible practices for automated data gathering
- [[voluntary-compliance-protocols|Voluntary Compliance Protocols]] - Self-enforced standards like robots.txt

### Information Seeking & Research Agents
- [[information-seeking-agents|Information-Seeking Agents]] - Autonomous systems for multi-step research and question answering
- [[webdancer|WebDancer]] - First end-to-end trained research agent with SFT + RL training
- [[react-framework|ReAct Framework]] - Thought-Action-Observation paradigm for agent execution
- [[autonomous-research-agents|Autonomous Research Agents]] - Self-directed information gathering systems

### Multi-Agent Systems
- [[multi-agent-web-data-collection|Multi-Agent Systems for Web Data Collection]] - Coordinated agent collaboration for data collection
- [[ohcache|OHCache]] - Oriented Hypergraph Cache System for efficient coordination
- [[react-paradigm|ReAct Paradigm]] - Reasoning + Acting execution model for LLM agents

#### AutoData Agent Roles
- [[plan-agent|Plan Agent (PLN)]] - Task decomposition and information gap analysis
- [[web-agent|Web Agent (WEB)]] - Web page navigation and content extraction
- [[tool-agent|Tool Agent (TOL)]] - Search engine query formulation and execution
- [[blueprint-agent|Blueprint Agent (BLU)]] - API endpoint discovery and documentation
- [[engineering-agent|Engineering Agent (ENG)]] - Code generation for scrapers and parsers
- [[test-agent|Test Agent (TST)]] - Test case generation and validation
- [[validation-agent|Validation Agent (VAL)]] - Data quality assessment and verification
- [[manager-agent|Manager Agent (MGR)]] - Workflow coordination and decision-making

### Reinforcement Learning & Training
- [[agentic-rl|Agentic Reinforcement Learning]] - RL techniques for training autonomous agents
- [[two-stage-agent-training|Two-Stage Agent Training (SFT + RL)]] - Supervised fine-tuning followed by RL optimization
- [[dapo|DAPO Algorithm]] - Decoupled Clip and Dynamic Sampling Policy Optimization
- [[on-policy-rl|On-Policy Reinforcement Learning]] - Learning from agent's own experience
- [[rejection-sampling-finetuning|Rejection Sampling Fine-Tuning]] - Multi-stage filtering for quality control
- [[sparse-reward-signals|Sparse Reward Signals]] - End-of-trajectory feedback challenges
- [[llm-as-judge|LLM-as-Judge]] - Automated evaluation using language models

### Constrained Reinforcement Learning
- [[constrained-mdp-web-agents|Constrained MDP for Web Agents]] - MDP formulation with multi-dimensional costs (requests, latency, failures) and budget constraints
- [[multi-cost-rl|Multi-Cost Reinforcement Learning]] - RL with multiple heterogeneous cost dimensions simultaneously optimized
- [[lagrangian-constrained-rl|Lagrangian Constrained RL]] - Lagrange multipliers and dual optimization for constraint satisfaction
- [[cvar-web-agent-risk|CVaR for Web Agent Risk Control]] - Conditional Value-at-Risk for tail risk suppression in web agents

### Data Synthesis
- [[crawlqa|CRAWLQA]] - Web crawling-based QA generation for agent training
- [[e2hqa|E2HQA]] - Easy-to-hard QA progression methodology
- [[synthetic-agent-training-data|Synthetic Agent Training Data]] - Automated generation of training trajectories
- [[easy-to-hard-qa|Easy-to-Hard QA Generation]] - Progressive complexity increase

### Reasoning & Chain-of-Thought
- [[short-cot|Short Chain-of-Thought]] - Concise reasoning for instruction models (GPT-4o style)
- [[long-cot|Long Chain-of-Thought]] - Extended reasoning for reasoning models (QwQ-Plus style)
- [[reasoning-models-vs-instruction-models|Reasoning Models vs Instruction Models]] - Different model types and their optimal uses

### LLM-Driven Web Scraping & Automation
- [[llm-driven-web-agents|LLM-Driven Web Agents]] - Overview of LLM-powered web scraping
- [[llm-to-script|LLM-to-Script (L2S)]] - Code generation paradigm for scraping
- [[llm-native-crawlers|LLM-Native Crawlers (LNC)]] - Integrated LLM crawling systems
- [[llm-based-web-agents|LLM-based Web Agents (LWA)]] - Autonomous browser-controlling agents
- [[parse-then-interpret|Parse-Then-Interpret Mechanism]] - Universal vulnerability in LLM scrapers
- [[llm-assisted-scripting|LLM-Assisted Scripting (LAS)]] - User executes LLM-generated code workflow
- [[end-to-end-llm-agents|End-to-End LLM Agents (ELA)]] - Autonomous agent planning and execution workflow
- [[scraping-democratization|Scraping Democratization]] - Lowering of technical barriers through LLM tools
- [[progressive-understanding|Progressive Understanding]] - Hierarchical comprehension mechanism for HTML using top-down and step-back operations
- [[crawler-generation|Crawler Generation]] - Automatic creation of reusable extraction rules using LLMs instead of manual programming

### WebAgent Architectures & Training (Survey Framework)
- [[three-process-webagent-architecture|Three-Process WebAgent Architecture]] - Fundamental design pattern: Perception → Planning & Reasoning → Execution
- [[webagent-training-strategies|WebAgent Training Strategies]] - Four progressive paradigms: training-free, GUI comprehension, task-specific fine-tuning, post-training RL
- [[trustworthy-webagents|Trustworthy WebAgents]] - Five dimensions: safety & robustness, privacy, generalizability, fairness, explainability

### Evaluation & Benchmarking
- [[llmcrawlbench|LLMCrawlBench]] - Benchmark dataset with 237 pages, 10,895 images from 50 websites
- [[instruct2ds|Instruct2DS]] - First benchmark for web data collection systems (237 pages, 234 tasks, 3 domains)
- [[gaia-benchmark|GAIA Benchmark]] - General AI Assistants evaluation (3 difficulty levels)
- [[webwalkerqa-benchmark|WebWalkerQA Benchmark]] - Web navigation and information extraction tasks
- [[browsecomp-benchmark|BrowseComp Benchmark]] - Complex browsing and comparison tasks
- [[novice-web-scraping-benchmark|Novice Web Scraping Benchmark]] - Evaluation framework for non-expert users (35 websites, 5 security tiers)
- [[web-scraping-difficulty-tiers|Web Scraping Difficulty Tiers]] - Five-tier classification from simple HTML to CAPTCHA

## Comparisons

Comparative analyses between related items.

### Technology Comparisons
- [[autodata-vs-webcloak|AutoData vs WebCloak]] - Data collection vs protection: opposing perspectives in the web agent ecosystem
- [[webdancer-vs-autodata|WebDancer vs AutoData]] - Single-agent RL vs multi-agent coordination: research questions vs structured data
- [[webdancer-vs-webcloak|WebDancer vs WebCloak]] - Information seeking vs content protection
- [[short-cot-vs-long-cot|Short-CoT vs Long-CoT]] - Concise vs extended reasoning for agent training
- [[constrained-rl-vs-unconstrained-rl|Constrained RL vs Unconstrained RL for Web Agents]] - Production-ready constrained optimization vs research-focused unconstrained maximization
- [[robots-txt-vs-webcloak|robots.txt vs WebCloak]] - Voluntary signaling vs active technical defense for content protection
- [[voluntary-vs-active-blocking|Voluntary vs Active Blocking]] - Cooperation-based vs technical enforcement approaches
- [[reputable-vs-misinformation-blocking|Reputable vs Misinformation Blocking Behavior]] - 6.6x disparity in AI crawler gatekeeping
- [[las-vs-ela|LLM-Assisted Scripting vs End-to-End LLM Agents]] - Code generation vs autonomous execution workflows
- [[beyond-beautifulsoup-vs-autodata|Beyond BeautifulSoup vs AutoData]] - Democratization assessment vs optimal system capability
- [[beyond-beautifulsoup-vs-webcloak|Beyond BeautifulSoup vs WebCloak]] - Attacker capability measurement vs defender effectiveness
- [[comparison-autocrawler-autodata-webdancer|AUTOCRAWLER vs AutoData vs WebDancer]] - Crawler generation vs multi-agent vs information seeking: three paradigms for web automation

---

*This index is automatically maintained by Claude during ingest operations*
