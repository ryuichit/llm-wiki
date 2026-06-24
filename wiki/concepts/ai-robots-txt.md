---
type: concept
tags: [web-crawling, ethics, compliance, ai-training, legal]
---

# AI-Specific Robots.txt

AI-specific robots.txt directives address the unique concerns of AI model training crawlers, which differ fundamentally from traditional search engine indexing. While conventional crawlers like Googlebot index content for retrieval, AI crawlers like GPTBot and ClaudeBot extract content for training [[large-language-models|large language models]], raising distinct ethical and legal questions.

## Why AI Needs Separate Rules

Traditional `robots.txt` was designed for search engines that:
- Index content to drive traffic back to the source
- Provide attribution through search results
- Create mutual value (visibility for sites, results for users)

AI training crawlers operate differently:
- Content is used for model training, not retrieval
- No direct attribution or traffic back to sources
- Value accrues primarily to model developers

This asymmetry motivates separate crawler rules. A site may welcome Googlebot (for SEO) while blocking GPTBot (to prevent uncompensated training use).

## Standard AI User-Agents

Major AI providers have introduced distinct user-agent strings:

### OpenAI
```
User-agent: GPTBot
Disallow: /
```

Used by OpenAI for training GPT models. Introduced August 2023 after significant backlash over unauthorized scraping.

### Anthropic
```
User-agent: ClaudeBot
Disallow: /
```

Anthropic's crawler for Claude training data. Respects robots.txt and provides opt-out mechanisms.

### Google
```
User-agent: Google-Extended
Disallow: /
```

Separate from Googlebot, used specifically for Bard/Gemini training. Blocking Google-Extended doesn't affect search indexing.

### Common AI
```
User-agent: CCBot
Disallow: /
```

Common Crawl's bot, which provides datasets used by many AI companies. Blocking CCBot prevents inclusion in widely-used training corpora.

## Granular Control Examples

Sites can implement fine-grained policies:

### Allow Search, Block AI
```
User-agent: Googlebot
Allow: /

User-agent: Google-Extended
Disallow: /

User-agent: GPTBot
Disallow: /

User-agent: ClaudeBot
Disallow: /
```

### Allow Some AI, Block Others
```
User-agent: ClaudeBot
Allow: /

User-agent: GPTBot
Disallow: /private/
Allow: /public/
```

### Rate Limiting
```
User-agent: GPTBot
Crawl-delay: 10
```

While not universally supported, `Crawl-delay` suggests minimum seconds between requests, relevant for [[rate-limiting|rate limiting]].

## Compliance and Enforcement

### Voluntary Compliance

Major providers claim robots.txt compliance:
- **OpenAI**: "GPTBot respects robots.txt"
- **Anthropic**: "ClaudeBot honors opt-out preferences"
- **Google**: "Google-Extended follows robots.txt directives"

However, enforcement is voluntary—there's no technical mechanism preventing non-compliant crawling.

### Legal Status

The legal force of robots.txt remains ambiguous:

- **HiQ vs. LinkedIn (2019)**: Court ruled that scraping public data doesn't necessarily violate CFAA, even if robots.txt says otherwise
- **Terms of Service**: Many sites supplement robots.txt with contractual terms prohibiting scraping
- **GDPR/Privacy Law**: European regulations may provide stronger protections than robots.txt alone

The [[data-collection-ethics|ethical data collection]] community generally treats robots.txt as binding, but legal cases are ongoing.

## Detection and Blocking

Sites employ multiple strategies beyond robots.txt:

### User-Agent Filtering

Server-side blocking based on user-agent:
```nginx
if ($http_user_agent ~* (GPTBot|CCBot|ClaudeBot)) {
    return 403;
}
```

However, crawlers can spoof user-agents, making this unreliable.

### Behavioral Analysis

Identify AI crawlers by behavior:
- Rapid sequential requests ([[rate-limiting|rate limiting]] violations)
- Access patterns (systematic traversal)
- JavaScript capability tests (some bots lack JS execution)

### CAPTCHA Challenges

[[captcha-challenges|CAPTCHA]] systems block automated access, but advanced [[web-agents|web agents]] increasingly solve simple CAPTCHAs.

### IP Blocking

Block known AI crawler IP ranges. OpenAI publishes IP lists for GPTBot, enabling preemptive blocking.

## AutoData Compliance

[[autodata-framework|AutoData]] implements robots.txt compliance as a core principle:

1. **Pre-request checking**: Fetches and parses robots.txt before any data collection
2. **Transparent identification**: Uses clear user-agent strings
3. **Rate limiting**: Built-in [[rate-limiting|throttling]] respects server capacity
4. **Opt-out respect**: Immediately stops if disallowed

The [[autodata-paper|AutoData paper]] emphasizes this ethical stance: "The framework respects robots.txt files and website terms of service, ensuring responsible data collection practices."

This distinguishes AutoData from aggressive scraping tools that prioritize data volume over consent.

## Future Directions

Emerging proposals for AI-specific crawler control:

### TDM Reservation

**Text and Data Mining (TDM) Reservation** headers:
```
TDM-Reservation: 1
```

Proposed EU standard explicitly reserving rights for AI training use.

### AI.txt Standard

Community-driven initiative for granular AI permissions:
```
# ai.txt
User-agent: GPTBot
Permission: training
Attribution: required
Commercial: prohibited
```

More expressive than robots.txt, but not yet widely adopted.

### Licensing Frameworks

Organizations like **Spawning AI** provide:
- **Do Not Train** registry: Sites opt out of AI training
- **API access**: Check crawler permissions programmatically
- **Attribution tracking**: Monitor training data provenance

## Practical Recommendations

For content owners:
1. **Audit current robots.txt**: Ensure AI crawlers are explicitly addressed
2. **Monitor access logs**: Detect unauthorized AI crawler activity
3. **Terms of service**: Supplement robots.txt with legal terms
4. **Selective blocking**: Consider allowing academic/research crawlers while blocking commercial ones

For AI researchers:
1. **Check robots.txt**: Always verify permissions before [[web-data-collection|data collection]]
2. **Use clear user-agents**: Identify yourself transparently
3. **Honor opt-outs**: Respect Do Not Train registries
4. **Document sources**: Track data provenance for [[data-collection-ethics|ethical compliance]]

## Related Concepts

- [[data-collection-ethics|Data Collection Ethics]]
- [[web-data-collection|Web Data Collection]]
- [[autodata-framework|AutoData Framework]]
- [[rate-limiting|Rate Limiting]]
- [[web-agents|Web Agents]]

## References

- [[autodata-paper|AutoData: Automated Multi-Agent Data Collection]]
- [[webvoyager-paper|WebVoyager: Building Multimodal Web Agents]]
- [OpenAI GPTBot Documentation](https://platform.openai.com/docs/gptbot)
- [Anthropic ClaudeBot Information](https://www.anthropic.com/crawler)
