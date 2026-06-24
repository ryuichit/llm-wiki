# LLM Wiki

A personal knowledge base built on the LLM Wiki pattern by Andrej Karpathy.

## What is this?

This is a collaborative knowledge management system where:
- **You** curate sources and ask questions
- **Claude** maintains the wiki, cross-references, and synthesizes knowledge

The LLM handles all the tedious maintenance work (updating summaries, noting contradictions, creating cross-references) that typically causes personal wikis to be abandoned.

## Structure

- `sources/` - Raw documents (papers, articles, data) - never modified
- `wiki/` - Generated markdown pages with summaries, entities, concepts
- `CLAUDE.md` - System instructions for Claude
- `index.md` - Catalog of all wiki pages
- `log.md` - Chronological operation log

## Usage

### Ingest a new source
Drop a file into `sources/` and ask Claude to ingest it:
```
Ingest sources/papers/paper.pdf
```

Claude will:
1. Read and summarize the source
2. Extract entities and concepts
3. Create/update relevant wiki pages
4. Update the index and log

### Query the wiki
Ask questions naturally:
```
What are the main approaches to X?
Compare Y and Z
```

Claude will search the wiki and synthesize answers with citations.

### Lint the wiki
Periodically ask for a health check:
```
Run a lint pass on the wiki
```

Claude will check for contradictions, orphan pages, missing cross-references, etc.

## Why this works

LLMs don't get bored with maintenance. The combination of human curation and AI diligence creates a knowledge base that actually grows and stays coherent over time.

## Based on

Pattern described by Andrej Karpathy: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
