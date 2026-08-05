# Open Knowledge Format (OKF) Specification

Every Markdown docs page inside a `docs/` directory (except for `docs/index.md` files) MUST begin with a standard YAML frontmatter block adhering to the OKF specification. (Module `README.md` files do NOT include YAML frontmatter).

## Frontmatter Schema

The following frontmatter template defines the basic information the docs pages should declare:

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
---
```
