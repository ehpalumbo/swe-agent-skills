# Open Knowledge Format (OKF) Specification

Every Markdown docs page inside a `docs/` directory (except `docs/index.md`) MUST begin with standard OKF YAML frontmatter. Module `README.md` files do NOT include frontmatter.

## Frontmatter Schema

The following frontmatter template defines the fields docs pages should declare. Required: `type`, `title`, `description`, `tags`, `timestamp`. Optional: `related`, `resource`.

```yaml
---
type: <architecture | specification | decision-record | concept | guide | constraint | workflow | entity | stakeholder | environment> # canonical list — see "Canonical Types" below 
title: "<Human-readable concise title>" # descriptive but short
description: "<1-2 sentence summary of what this docs page covers and why it exists>" # concise summary of the page contents
tags: # 2 to 5 relevant taxonomy tags for indexing and querying
  - "tag-1" 
  - "tag-2"
timestamp: "<YYYY-MM-DDTHH:MM:SSZ>" # last update timestamp
related: # optional: links to other docs pages or source code / configuration files
  - "[Title](relative/path/to/other-docs-page.md)"
  - "<fully-qualified-name-of-source-file>"
  - "path/to/configuration"
resource: # optional: underlying source code or configuration files this page documents
  - "<path/to/source/file.ext>"
---
```

> [!NOTE]
> `type` is **metadata** and independent of directory structure. The `docs/<category>/` subdirectory a page lives in is purely structural; it doesn't constrain the declared `type`.

## Canonical Types

This is the recommended list of `type` values. Every docs page, and every operation (`ingest`, `query`, `lint`), should prefer using these values — but other types may be proposed by the agent as needed.

| `type` | Meaning |
| --- | --- |
| `architecture` | System/component structure, technology stack, and how parts interact. |
| `specification` | High-level specification: goals, functional/non-functional requirements, scope boundaries not captured in code. |
| `decision-record` | An ADR-style record: context, decision, trade-offs, consequences. One decision per page. |
| `concept` | Domain mental model: key abstractions, business rules, constraints. |
| `guide` | Practical how-to: coding standards, setup, contribution process. |
| `constraint` | A non-negotiable boundary or technical requirement. |
| `workflow` | A repeatable process sequence: testing strategy, CI/CD, deployment. |
| `entity` | A domain entity or data model. |
| `stakeholder` | A person, team, or system with interests in the codebase. |
| `environment` | An operating environment or configuration context. |
