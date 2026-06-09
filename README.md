# karpathy-llm-wiki

**A reusable skill for building Karpathy-style LLM wikis with Claude Code, Cursor, Codex, and other Agent Skills tools.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/ChaosJu/karpathy-llm-wiki?style=social)](https://github.com/ChaosJu/karpathy-llm-wiki)
[![GitHub forks](https://img.shields.io/github/forks/ChaosJu/karpathy-llm-wiki?style=social)](https://github.com/ChaosJu/karpathy-llm-wiki)
[![Agent Skills](https://img.shields.io/badge/Agent_Skills-compatible-blue)](https://agentskills.io)
[![Install](https://img.shields.io/badge/Install-npx_add--skill-green)](https://github.com/ChaosJu/karpathy-llm-wiki#install)

<p align="center">
  <img src="assets/karpathy-tweet.png" alt="Karpathy's tweet about LLM Wiki" width="560">
</p>

`karpathy-llm-wiki` packages [Karpathy's LLM Wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) into one installable [Agent Skills](https://agentskills.io) skill. Your coding agent ingests sources into `raw/`, compiles durable knowledge pages into `wiki/`, answers questions with citations, and lints the wiki for consistency.

## What Is an LLM Wiki?

An **LLM wiki** is a knowledge system where the LLM maintains structured wiki pages instead of re-searching raw documents on every question. New sources are compiled into durable markdown pages, cross-references are updated over time, and answers cite the wiki pages that already contain the synthesized knowledge.

This skill gives you three operations:

| Operation | What it does | Output |
|-----------|--------------|--------|
| **Ingest** | Collects a source into `raw/` and compiles it into the wiki | New or updated wiki pages |
| **Query** | Searches the wiki and answers with citations | Grounded answers linking to markdown pages |
| **Lint** | Checks index integrity, links, and wiki health | Auto-fixes plus reported issues |

See [SKILL.md](SKILL.md) for the full skill specification.

## What's New in v2

| Feature | Description |
|---------|-------------|
| **Token Budget (L0–L3)** | Progressive loading strategy — agent reads index first, full articles only when confirmed relevant |
| **SHA256 Deduplication** | Content hashing prevents re-ingesting or re-compiling unchanged sources |
| **Hot Cache** | `wiki/hot.md` carries session context (~500 words) across conversations, replacing expensive recaps |
| **Purpose & Scenarios** | `wiki/purpose.md` anchors the knowledge base direction; 4 scenario templates (general, research, reading, project) |
| **Entity Hints** | Lightweight structural guidance (concept/person/tool/event) without rigid schemas |
| **Counter-Arguments** | Articles with 3+ sources automatically gain opposing viewpoints, preventing bias accumulation |
| **Gap Detection** | Query operation flags missing coverage areas; Lint checks purpose.md topics against actual wiki |

## LLM Wiki vs RAG

| Approach | Knowledge lives in | When synthesis happens | Good for |
|----------|--------------------|------------------------|----------|
| **RAG** | Raw chunks and embeddings | At query time | Broad retrieval across large corpora |
| **LLM Wiki** | Curated markdown pages | During ingest and maintenance | Compounding knowledge, summaries, and durable cross-links |

This skill is optimized for the wiki model: knowledge that improves over time instead of re-deriving relationships on every query.

## Examples

See [examples/](examples/) for sample wiki pages, source files, and operation logs.

## Install

```bash
npx add-skill ChaosJu/karpathy-llm-wiki
```

Works with any tool that supports the [Agent Skills](https://agentskills.io) standard.

## Quick Start

### 1. Initialize with a purpose

On your first ingest, the skill asks which scenario fits your use case:

- **General** — open-ended knowledge building
- **Research** — academic paper tracking and methodology mapping
- **Reading** — book-based knowledge synthesis
- **Project** — software architecture decisions and team knowledge

### 2. Ingest your first source

Give the skill a URL, a file, or pasted text:

> "Ingest this article: https://example.com/attention-is-all-you-need"

The skill stores the source in `raw/` (with SHA256 hash for dedup), then compiles or updates the right knowledge pages in `wiki/`.

### 3. Ask your wiki a question

> "What do I know about attention mechanisms?"

The skill follows L0→L3 token budget: reads index first, scans overviews, then loads only relevant full articles before answering with citations.

### 4. Keep the wiki healthy

> "Lint my wiki"

Checks for broken links, missing index entries, stale cross-references, orphan pages, missing counter-arguments, and knowledge gaps relative to your purpose.

## How the Workflow Works

The core idea from Karpathy: the LLM maintains the wiki while the human focuses on choosing sources and asking good questions.

```text
your-project/
├── raw/            ← Immutable source material (with content hashes)
│   └── topic/
│       └── 2026-04-03-source-article.md
├── wiki/           ← Compiled knowledge pages maintained by the LLM
│   ├── topic/
│   │   └── concept-name.md
│   ├── index.md    ← Global table of contents
│   ├── log.md      ← Append-only operation log
│   ├── hot.md      ← Session context cache (~500 words)
│   └── purpose.md  ← Knowledge base goals and scope
```

Each new source can update multiple pages, strengthen cross-references, trigger counter-arguments, and record contradictions. That is what makes the wiki compound over time.

## Token Budget Strategy

The skill minimizes LLM costs with progressive loading:

```
L0: SKILL.md frontmatter        (~50 tokens, auto-loaded)
L1: purpose + hot + index       (~1-2K tokens, session start)
L2: Article Overview sections   (scan for relevance)
L3: Full article bodies         (only confirmed-relevant articles)
```

This means a query touching 3 articles out of a large wiki reads ~3K tokens instead of loading everything.

## Tool Compatibility

This skill follows the [agentskills.io](https://agentskills.io) open standard:

| Tool | Install method |
|------|----------------|
| Claude Code | `npx add-skill ChaosJu/karpathy-llm-wiki` |
| Cursor | `npx add-skill ChaosJu/karpathy-llm-wiki` |
| Codex CLI | Copy to `.agents/skills/karpathy-llm-wiki/` |
| OpenCode | `npx add-skill ChaosJu/karpathy-llm-wiki` |
| Other tools | Copy `SKILL.md` and `references/` into the tool's skill directory |

## FAQ

### What is the difference between an LLM wiki and a personal wiki?

An LLM wiki is maintained by the model. It updates summaries, cross-links, index entries, counter-arguments, and contradictions as new material arrives. A normal personal wiki depends on manual editing.

### What sources can I ingest?

Web pages, papers, blog posts, PDFs, markdown files, text files, and pasted text. The skill converts everything into markdown under `raw/` and compiles it into `wiki/`.

### What are the scenario templates?

Four presets for `wiki/purpose.md` that tailor the wiki's direction: **general** (open exploration), **research** (paper tracking), **reading** (book synthesis), **project** (software docs). Choose during initialization or switch later.

### How does deduplication work?

Every raw file stores a SHA256 hash of its body content. Before ingesting, the skill checks if that hash already exists. If so, it skips the duplicate and tells you which file already has the content.

### What is hot.md?

A ~500-word session context cache that lets the agent pick up where you left off without re-reading your entire wiki. Updated at session end, read silently at session start.

### Is this production-ready?

The workflow design is based on analysis of multiple production LLM wiki implementations. The repo includes examples, templates, and a full design spec.

## Inspired By

Unofficial community implementation of the workflow from [Karpathy's LLM Wiki idea](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f). Design informed by analysis of [lucasastorian/llmwiki](https://github.com/lucasastorian/llmwiki), [SherwinQ/karpathy-wiki](https://github.com/SherwinQ/karpathy-wiki), and [ScrapingArt/Karpathy-LLM-Wiki-Stack](https://github.com/ScrapingArt/Karpathy-LLM-Wiki-Stack).

## License

[MIT](LICENSE)
