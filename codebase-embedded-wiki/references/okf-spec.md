# Open Knowledge Format (OKF) Specification

This document covers the wiki objectives & principles, directory placement rules, OKF YAML frontmatter schema, and the copy-pasteable OKF wiki page template.

---

## 1. High-Level Wiki Objective & Principles

1. **Explain Why & How at a High Level**: The primary objective of the wiki is to support development tasks by explaining **why** architectural decisions were made and **how** components interact conceptually.
2. **Do NOT Replicate Source Code**: AI agents can inspect source code files to read implementation details, function signatures, syntax, and exact parameters. The wiki must **NEVER** duplicate code blocks or clone code logic in Markdown.
3. **README Files are NOT Wiki Pages**: `README.md` files serve as initial entry points summarizing module scope and usage value. They do **not** follow the OKF format and are **not** part of the wiki.
4. **Wiki Index (`index.md`) is Mandatory**: Every `wiki/` directory MUST contain an `index.md` Table of Contents indexed **up to 3 levels deep**, optimized for AI agents to discover knowledge structure rapidly.

---

## 2. Directory Structure & Placement Rules

A codebase-embedded wiki lives directly within the repository alongside target modules. Wiki pages must **NEVER** be mixed directly among source code files. Instead, all wiki pages must reside strictly within a `wiki/` subdirectory located alongside the module's `README.md`. Subdirectories inside `wiki/` are supported to organize pages, reflecting upper-level wiki hierarchy:

```text
<repository-root>/
├── README.md                      # Repository Scope & Usage (Non-OKF, links to wiki/index.md)
├── .wiki-meta.json                # Minimal tracking manifest (commit SHA & timestamp)
├── wiki/                          # Global / Cross-Cutting Architecture Wiki Directory
│   ├── index.md                   # Global Wiki Table of Contents (3 levels deep)
│   ├── architecture/              # Subdirectory reflecting upper-level wiki section
│   │   └── overview.md            # OKF Wiki Page (type: architecture)
│   └── guides/                    # Subdirectory reflecting upper-level wiki section
│       └── coding-standards.md    # OKF Wiki Page (type: guide)
└── <module-name>/                 # e.g., a logical module or service
    ├── README.md                  # Module Scope & Usage (Non-OKF)
    ├── wiki/                      # ALL Module Wiki Pages MUST live inside wiki/ (supports subdirectories)
    │   ├── index.md               # Module Wiki Table of Contents (3 levels deep)
    │   ├── concepts/              # Subdirectory reflecting upper-level wiki section
    │   │   ├── topic-a.md         # OKF Wiki Page (type: concept)
    │   │   └── topic-b.md         # OKF Wiki Page (type: concept)
    │   └── decisions/             # Subdirectory reflecting upper-level wiki section
    │       └── adr-001.md         # OKF Wiki Page (type: decision-record)
    └── src/                       # Standard source root
```

---

## 3. OKF YAML Frontmatter Specification

Every Markdown wiki page inside a `wiki/` directory MUST begin with a standard YAML frontmatter block adhering to OKF v0.2. (Module `README.md` files do NOT include YAML frontmatter).

### Frontmatter Schema

```yaml
---
type: <architecture | decision-record | concept | guide>
title: "<Human-readable concise title>"
description: "<1-2 sentence summary of what this wiki page covers and why it exists>"
module: "<relative/path/to/module or 'root'>"
resource: "<relative/path/to/primary/source/code/file or directory>"
tags:
  - "<tag-1>"
  - "<tag-2>"
timestamp: "<YYYY-MM-DDTHH:MM:SSZ>"
verified: true | false
related:
  - "[Title](relative/path/to/other-wiki-page.md)"
---
```

### Field Definitions

| Field | Required | Type | Description |
| :--- | :---: | :--- | :--- |
| `type` | **Yes** | Enum | Page classification (`architecture`, `decision-record`, `concept`, `guide`). |
| `title` | **Yes** | String | Descriptive title of the wiki page. |
| `description` | **Yes** | String | Concise summary (1-2 sentences) of page contents and architectural rationale. |
| `module` | **Yes** | String | Relative path to the module directory (e.g., `<module-name>` or `root`). |
| `resource` | No | String | Primary code file or directory described (e.g., `<module-name>/src/<path/to/primary/source/file>`). |
| `tags` | **Yes** | Array | 2 to 5 relevant taxonomy tags for indexing and querying. |
| `timestamp` | **Yes** | String | ISO-8601 timestamp of last update/creation. |
| `verified` | No | Boolean | Optional trust signal. `true` if content was explicitly validated against codebase. |
| `stale_after` | No | String | Optional ISO-8601 timestamp indicating when content should be re-checked for staleness. |
| `related` | No | Array | Relative Markdown links to related OKF wiki pages. |

---

## 4. OKF Wiki Page Template (`wiki/**/*.md`)

```markdown
---
type: <architecture | decision-record | concept | guide>
title: "<Descriptive page title>"
description: "<1-2 sentence summary of what this wiki page covers and why it exists.>"
module: "<relative/path/to/module or 'root'>"
resource: "<relative/path/to/primary/source/code/file or directory>"
tags:
  - "<tag-1>"
  - "<tag-2>"
timestamp: "<YYYY-MM-DDTHH:MM:SSZ>"
stale_after: "<YYYY-MM-DDTHH:MM:SSZ>"
related:
  - "[<Related Page Title>](<relative/path/to/other-wiki-page.md>)"
---

# <Page Title>

## Context & Architectural Rationale
[Explain why this design was chosen, trade-offs considered, and what domain problems it solves.]

## System Mechanics & High-Level Flow
[Describe how components interact at a high level. Use Mermaid diagrams or conceptual summaries. Do NOT replicate source code logic.]

- **<Key Component or Concept>**: <High-level description of its role and behavior.>
- **<Key Component or Concept>**: <High-level description of its role and behavior.>

## Known Constraints & Trade-offs
- <Constraint or trade-off relevant to the architectural decision.>
- <Constraint or trade-off relevant to the architectural decision.>

## Primary Code References
- <Description of reference>: [<filename>](<relative/path/to/file.ext>)
- <Description of reference>: [<filename>](<relative/path/to/file.ext>)
```
