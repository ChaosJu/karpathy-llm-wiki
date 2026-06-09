# Purpose Templates

Choose one scenario during initialization. The user picks; you generate `wiki/purpose.md` accordingly.

---

## General (default)

```markdown
# Purpose

> Scenario: general

## Goal

{What this knowledge base is about — one sentence.}

## Core Questions

- {Question 1 the user wants to answer over time}
- {Question 2}
- {Question 3}

## Scope

**In scope:** {topics, domains, timeframes to cover}
**Out of scope:** {what to deliberately exclude}

## Evolving Thesis

{A working hypothesis that updates as knowledge accumulates. Not a conclusion — a direction.}
```

---

## Research (academic/technical deep-dives)

```markdown
# Purpose

> Scenario: research

## Goal

{Research question or domain to systematically map.}

## Core Questions

- {Primary research question}
- {Sub-question A}
- {Sub-question B}
- {Methodological question}

## Scope

**In scope:** {papers, preprints, technical reports, conference talks in domain X since YYYY}
**Out of scope:** {blog opinions without data, tangential fields}

## Tracking Strategy

- Track key papers by research group
- Note methodology evolution across years
- Identify competing approaches and their tradeoffs
- Flag replication status of major claims

## Evolving Thesis

{Current best understanding of the research landscape — update as new papers are ingested.}
```

---

## Reading (book-based knowledge building)

```markdown
# Purpose

> Scenario: reading

## Goal

{What intellectual territory this reading project covers.}

## Core Questions

- {What am I trying to understand through these books?}
- {What frameworks am I building?}
- {What practical decisions will this inform?}

## Scope

**In scope:** {genres, authors, time periods}
**Out of scope:** {casual reads not worth compiling}

## Reading Strategy

- One article per book (entity: concept or person as appropriate)
- Track cross-book themes via See Also links
- Note contradictions between authors explicitly
- Promote recurring insights into standalone concept articles

## Evolving Thesis

{Current synthesis across books read so far — what patterns emerge?}
```

---

## Project (software/product documentation)

```markdown
# Purpose

> Scenario: project

## Goal

{What project this wiki documents and why a wiki over inline docs.}

## Core Questions

- {Key architectural decisions to document}
- {Recurring team questions to answer once}
- {Integration patterns to codify}

## Scope

**In scope:** {architecture decisions, API contracts, deployment runbooks, post-mortems}
**Out of scope:** {auto-generated API docs, ephemeral sprint notes}

## Documentation Strategy

- ADRs (Architecture Decision Records) → ingest as raw, compile into concept articles
- Post-mortems → ingest, compile into pattern articles
- Meeting decisions → extract action items only, discard chatter
- Code patterns → tool-type articles with usage examples

## Evolving Thesis

{Current architectural direction — what principles guide decisions?}
```
