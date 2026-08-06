# Open Knowledge Format (OKF) Specification

Every Markdown docs page inside a `docs/` directory (except for `docs/index.md` files) MUST begin with a standard YAML frontmatter block adhering to the OKF specification. (Module `README.md` files do NOT include YAML frontmatter).

> [!NOTE]
> `type` is **metadata** and independent of directory structure. The `docs/<category>/` subdirectory a page lives in (e.g., `decisions/`) is purely structural; it does not constrain the `type` field a page declares.

## Frontmatter Schema

The following frontmatter template defines the basic information the docs pages should declare. Required fields: `type`, `title`, `description`, `tags`, `timestamp`. Optional fields: `related`, `resource`, `stale_after`.

```yaml
---
type: <decision-record | constraint | guide | workflow | entity | stakeholder | environment> # just to name a few common types 
title: "<Human-readable concise title>" # descriptive but short
description: "<1-2 sentence summary of what this docs page covers and why it exists>" # concise summary of the page contents
tags: # 2 to 5 relevant taxonomy tags for indexing and querying
  - "tag-1" 
  - "tag-2"
timestamp: "<YYYY-MM-DDTHH:MM:SSZ>" # last update timestamp
related: # links to other docs pages or source code / configuration files
  - "[Title](relative/path/to/other-docs-page.md)"
  - "<fully-qualified-name-of-source-file>"
  - "path/to/configuration"
resource: # optional: underlying source code or configuration files this page documents
  - "<path/to/source/file.ext>"
stale_after: "<YYYY-MM-DD>" # optional: date after which the page should be re-verified for accuracy
---
```

## Staleness & Resource Tracking

- `stale_after`: An optional ISO-8601 date (`YYYY-MM-DD`) after which the page is considered potentially stale. Used by the [`query`](query-operation.md) and [`lint`](lint-operation.md) operations to flag pages that should be re-verified against the codebase.
- `resource`: An optional list of relative paths to the source code or configuration files the page documents. When a page has passed its `stale_after` date, `query` inspects the `resource` file(s) (or runs `git log`) to confirm the documented rationale is still accurate.
