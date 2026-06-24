---
type: concept
tags: [caching, storage, optimization, autodata, architecture]
---

# Local Cache System

A local cache system provides temporary storage for large artifacts outside the main communication channels in [[multi-agent-systems|multi-agent systems]]. By storing HTML documents, downloaded files, and intermediate data products separately from agent messages, cache systems dramatically reduce token consumption and improve system efficiency.

## Motivation

[[web-agents|Web agents]] frequently handle large artifacts:
- **HTML documents**: 50-500KB per page
- **Downloaded files**: PDFs, images, data files
- **Intermediate results**: Extracted tables, scraped datasets
- **Context snapshots**: Page state for [[hierarchical-planning|planning]]

Passing these through [[multi-agent-communication|message channels]] inflates token costs. For example, a 100KB HTML page consumes ~25K tokens when included in agent context. In a [[multi-agent-systems|multi-agent system]] with 5 agents, broadcasting this document costs 125K tokens per page.

## AutoData's Cache Architecture

[[autodata-framework|AutoData]] implements a local cache with the following design:

### Storage Layer

Artifacts are stored in a filesystem-backed cache with content-addressed identifiers:

```
cache/
├── html/
│   ├── a3f9b8c2.html    # Cached page content
│   └── d4e7a1f5.html
├── data/
│   ├── extracted_001.json
│   └── extracted_002.json
└── metadata/
    └── index.json        # Cache directory
```

Each artifact receives a unique cache ID (hash or sequential). The [[local-cache-system|cache]] maintains an index mapping IDs to file paths and metadata (timestamp, size, content type).

### Message Protocol

Instead of inline content, agents exchange **cache references**:

```json
{
  "from": "web_navigator",
  "to": ["data_extractor"],
  "type": "html_ready",
  "cache_id": "a3f9b8c2",
  "url": "https://example.com/data",
  "size_bytes": 102400
}
```

Recipients retrieve artifacts on-demand:

```python
html_content = cache.get("a3f9b8c2")
```

This decouples storage from communication, allowing large artifacts without message bloat.

## Token Cost Reduction

The [[autodata-paper|AutoData paper]] reports token savings:

| Approach | Tokens per Task |
|----------|----------------|
| Inline content (baseline) | 847K |
| [[oriented-message-hypergraph|Hypergraph routing]] | 368K |
| + Local cache | 184K |

The cache contributes a **2x additional reduction** beyond routing optimizations, achieving **4.6x total savings** versus naive broadcast.

Savings breakdown:
- **HTML storage**: ~50K tokens per page → ~20 tokens (cache ID)
- **Extracted data**: ~10K tokens → ~20 tokens
- **Intermediate results**: ~5K tokens → ~20 tokens

For a task visiting 10 pages, the cache saves approximately **650K tokens** in message overhead.

## Retrieval Strategies

Agents retrieve cached artifacts using different patterns:

### On-Demand Retrieval

Agents fetch content only when needed:

```python
if cache.has(cache_id):
    content = cache.get(cache_id)
    process(content)
```

This minimizes memory usage but adds retrieval latency.

### Prefetching

The [[coordinator-agent|coordinator]] can predict which agents need which artifacts:

```python
# Coordinator knows extractor will need HTML
cache.prefetch(cache_id, target_agent="extractor")
```

Prefetching reduces latency at the cost of memory.

### Lazy Loading

Large artifacts are loaded incrementally:

```python
# Read HTML in chunks to extract specific elements
with cache.stream(cache_id) as html_stream:
    for chunk in html_stream:
        if target_element_found(chunk):
            break
```

Useful for massive documents where only portions are relevant.

## Cache Invalidation

Cache entries are evicted using policies:

### Time-Based Expiration

Artifacts expire after fixed duration:
- **Short-lived** (minutes): Task-specific intermediate results
- **Medium-lived** (hours): Retrieved web pages
- **Long-lived** (days): Reference data, schemas

### Size-Based Eviction

When cache exceeds size limit, evict using:
- **LRU** (Least Recently Used): Remove oldest accessed items
- **LFU** (Least Frequently Used): Remove rarely accessed items
- **Size-priority**: Remove largest items first

AutoData uses **task-scoped caches** that are automatically cleared after task completion, avoiding the need for sophisticated eviction.

## Distributed Caching

For [[distributed-agents|distributed multi-agent systems]], caches can be:

### Centralized

Single shared cache service:
- **Pros**: Simple consistency, no duplication
- **Cons**: Network bottleneck, single point of failure

Used in AutoData's single-machine deployment.

### Replicated

Each agent maintains a local cache replica:
- **Pros**: Low latency, fault tolerance
- **Cons**: Consistency challenges, storage overhead

Suitable for [[parallel-task-execution|parallel execution]] across machines.

### Hybrid

Hot data replicated, cold data centralized:
- **Pros**: Balances performance and efficiency
- **Cons**: Complex cache coherence protocol

## Integration with Other Optimizations

Local caching synergizes with:

### Oriented Message Hypergraph

[[oriented-message-hypergraph|Selective routing]] determines which agents need cache access. Agents not in the hyperedge target set never fetch artifacts, saving retrieval overhead.

### Parallel Task Execution

[[parallel-task-execution|Concurrent agents]] can share cached artifacts without duplication. One navigator agent populates cache; multiple extractors read simultaneously.

### Rate Limiting

[[rate-limiting|Throttled web requests]] populate cache at controlled rate. Cache hits avoid redundant requests to rate-limited sources.

## Implementation Example

Simplified AutoData cache implementation:

```python
class LocalCache:
    def __init__(self, base_dir):
        self.base_dir = Path(base_dir)
        self.index = {}
    
    def put(self, content: bytes, metadata: dict) -> str:
        cache_id = hashlib.sha256(content).hexdigest()[:8]
        path = self.base_dir / f"{cache_id}.dat"
        path.write_bytes(content)
        self.index[cache_id] = {
            "path": str(path),
            "timestamp": time.time(),
            "metadata": metadata
        }
        return cache_id
    
    def get(self, cache_id: str) -> bytes:
        entry = self.index.get(cache_id)
        if not entry:
            raise KeyError(f"Cache miss: {cache_id}")
        return Path(entry["path"]).read_bytes()
```

## Performance Impact

AutoData benchmarks show:
- **Cache hit rate**: 93% for repeated web scraping tasks
- **Retrieval latency**: <1ms for local files vs. 500-2000ms for web refetch
- **Memory overhead**: ~50MB cache storage vs. 500MB in-memory messages

## Related Concepts

- [[autodata-framework|AutoData Framework]]
- [[oriented-message-hypergraph|Oriented Message Hypergraph]]
- [[multi-agent-systems|Multi-Agent Systems]]
- [[web-agent-cost-modeling|Web Agent Cost Modeling]]
- [[caching-strategies|Caching Strategies]]
- [[rate-limiting|Rate Limiting]]

## References

- [[autodata-paper|AutoData: Automated Multi-Agent Data Collection]]
- [[web-agent-rl-paper|Reinforcement Learning for Web Agents]]
- [[autogen-paper|AutoGen: Multi-Agent Conversation Framework]]
