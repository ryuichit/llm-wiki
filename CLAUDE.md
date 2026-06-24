# LLM Wiki - System Instructions

## Purpose

This is a personal knowledge base where you (Claude) help maintain a structured wiki by:
- Ingesting raw source documents and extracting knowledge
- Maintaining interlinked markdown pages
- Answering queries with citations
- Keeping the wiki healthy and up-to-date

## Architecture

### Three Layers

1. **sources/** - Immutable raw documents (papers, articles, images, data)
2. **wiki/** - Your generated markdown files (summaries, entities, concepts, comparisons)
3. **CLAUDE.md** (this file) - The schema defining structure and conventions

## Core Operations

### Ingest
When I add a new source document:
1. Read the source thoroughly
2. Create a summary page in `wiki/sources/`
3. Extract key entities and create/update entity pages in `wiki/entities/`
4. Extract key concepts and create/update concept pages in `wiki/concepts/`
5. Update `index.md` with new entries
6. Append to `log.md` with timestamp and summary of changes
7. Update any related pages with cross-references

### Query
When I ask questions:
1. Search relevant wiki pages
2. Synthesize answers with citations using `[[page-name]]` links
3. If the answer is substantial, ask if I want to save it as a new wiki page

### Lint
Periodically check for:
- Contradictions between pages
- Stale or outdated claims
- Orphan pages (no incoming links)
- Missing cross-references
- Data gaps or incomplete coverage

## Directory Structure

```
llm-wiki/
├── CLAUDE.md           # This file - system instructions
├── index.md            # Main catalog of all wiki pages
├── log.md              # Chronological record of all operations
├── sources/            # Raw source documents
│   ├── papers/
│   ├── articles/
│   └── data/
└── wiki/               # Your generated content
    ├── sources/        # Summary pages for each source
    ├── entities/       # Pages for people, organizations, technologies
    ├── concepts/       # Pages for ideas, techniques, theories
    └── comparisons/    # Comparative analyses
```

## Formatting Conventions

### Page Structure
Every wiki page should have:
```markdown
---
title: Page Title
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Title

Brief introduction (1-2 sentences)

## Content sections...

## Related Pages
- [[related-page-1]]
- [[related-page-2]]

## Sources
- [[source-name]] - specific claims
```

### Link Format
- Use `[[page-name]]` for wiki links
- Use descriptive page names in kebab-case
- Always add back-references when creating new links

### Log Format
```markdown
## YYYY-MM-DD HH:MM - Operation Type

Brief description of what was done
- Pages created: [[page1]], [[page2]]
- Pages updated: [[page3]], [[page4]]
- Sources processed: filename.pdf
```

## Guidelines

- **Be thorough**: One source may update 10-15 wiki pages
- **Cross-reference liberally**: Build a dense knowledge graph
- **Maintain consistency**: When updating claims, check related pages
- **Preserve sources**: Never modify files in `sources/`
- **Version everything**: This is a git repo - commit after major operations
- **Ask before big changes**: Confirm before restructuring or major lints

## Critical File Path Rules

**ALWAYS use full paths with `wiki/` prefix when creating pages:**

✅ **CORRECT**:
- `wiki/concepts/my-concept.md`
- `wiki/entities/my-entity.md`
- `wiki/comparisons/comparison-a-vs-b.md`
- `wiki/sources/paper-summary.md`

❌ **WRONG** (creates files at project root):
- `./concepts/my-concept.md`
- `concepts/my-concept.md`
- `comparisons/my-comparison.md`

**When delegating to agents**: Explicitly specify full paths with `wiki/` prefix in prompts. Example: "Create concept page at `wiki/concepts/new-concept.md`"

**When verifying**: After agent operations, check that no directories were created at project root (`.git`, `sources/`, `wiki/`, `index.md`, `log.md`, `CLAUDE.md` are the ONLY allowed root-level items)

## Your Role

You don't get bored with maintenance tasks. I curate sources and ask questions. You handle all the bookkeeping, cross-referencing, updating, and synthesis work that makes the wiki valuable.
