---
name: codebase-embedded-wiki
description: Operate a codebase-embedded wiki (init, ingest, sync, query, lint) following Google's Open Knowledge Format (OKF) and Karpathy's LLM wiki pattern. Use when initializing a codebase wiki, ingesting PRs or design docs, syncing git diffs, querying architecture context, or auditing wiki health.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.0.0-SNAPSHOT"
---

# Codebase-Embedded Wiki

Use this skill to create, maintain, query, and health-check a **codebase-embedded wiki**.

## Definition

A codebase-embedded wiki stores curated, structured knowledge directly inside the repository alongside target modules that are meant to complement, not duplicate, implementation-level documentation. It is intended to be consumed by both AI agents and human developers when gathering context for software engineering tasks.

Wiki pages are never mixed directly with source files; instead, each project module has a module-level `README.md` as an initial entry point, and a separate `wiki/` subdirectory containing an `index.md` as a table of contents for the wiki pages that live inside the `wiki/` directory. This structure is repeated recursively for each module, splitting the wiki contents between the global `wiki/` directory for cross-cutting concerns and module-specific `wiki/` directories for module-specific concerns.

## Objective & Philosophy

- **Complementary, Not Redundant**: The wiki MUST complement, not duplicate, implementation-level documentation (e.g., Swagger/OpenAPI specs, Protocol Buffer definitions, JavaDocs or Python docstrings). Agents can always scan source code to learn implementation details, signatures, and syntax. The wiki MUST NOT repeat what is already obvious in the code.
- **Focus on High-Level Constructs**: The wiki must explain **why** things were done the way they were done (design decisions, trade-offs, constraints, rationale, motivation) and **how** things work at a high level (architecture, patterns, system boundaries, data flows, mental models, workflows).
- **Persistent Curator Pattern**: Formalizing Andrej Karpathy's [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), act as a persistent curator building a compounding, interlinked knowledge graph embedded close to the code.

## Wiki Architecture & Directory Conventions

A codebase-embedded wiki places documentation in dedicated `wiki/` subdirectories alongside target module `README.md` files. Wiki pages must NOT be mixed directly into source code folders:

```text
<repository-root>/
├── README.md                      # Repository Overview (Non-OKF: scope, usage, links to wiki/index.md)
├── .wiki-meta.json                # Minimal tracking manifest (last_sync_commit, last_sync_timestamp)
├── wiki/                          # Global / Cross-Cutting Architecture Wiki Directory
│   ├── index.md                   # Global Wiki Table of Contents
│   ├── architecture/              # Subdirectory reflecting upper-level wiki section
│   │   └── overview.md            # OKF Wiki Page (type: architecture)
│   └── guides/                    # Subdirectory reflecting upper-level wiki section
│       └── coding-standards.md    # OKF Wiki Page (type: guide)
└── <module-name>/                 # e.g., auth-service, billing-service (module root)
    ├── README.md                  # Module Entry Point (Non-OKF: scope, usage, links to wiki/index.md)
    ├── wiki/                      # Module Wiki Directory (supports subdirectories reflecting upper wiki levels)
    │   ├── index.md               # Module Wiki Table of Contents
    │   ├── concepts/              # Wiki subdirectory for concepts section
    │   │   └── topic-a.md         # OKF Wiki Page (type: concept)
    │   └── decisions/             # Wiki subdirectory for decision records section
    │       └── adr-001.md         # OKF Wiki Page (type: decision-record)
    └── src/                       # Standard source root
        ├── main/                  # Production application sources
        └── test/                  # Unit and integration tests
```

**Single vs Multiple Modules**: For single-module codebases, the global `wiki/` directory must capture all architectural knowledge. When a codebase contains more than one module, the global `wiki/` directory should be used for cross-cutting architecture concerns (e.g., shared libraries, common patterns, global design decisions), while each module should maintain its own `wiki/` subdirectory for module-specific concerns.

**README Files vs Wiki Pages**: `README.md` files are **not** part of the wiki and do **not** follow OKF format. They provide a quick introduction to the scope and usage of the module and point directly to `wiki/index.md`.

**Wiki Index (`index.md`)**: Every `wiki/` directory MUST contain an `index.md` file. It serves as an optimized Table of Contents indexed **up to 3 levels deep** to allow AI agents to rapidly discover wiki structure without reading all individual pages.

### Recommended Structure

The wiki directory may be organized into the following sections (select what applies to each module/level):

- `specs/`: High-level specifications for the project. This includes things like project goals, functional and non-functional requirements, and other specifications that may not be captured in the codebase itself.
- `architecture/`: High-level architecture, system overview, technology choices, and architectural patterns. Might be replaced with a single `architecture.md` file for small wikis.
- `decisions/`: Architecture Decision Records (ADRs). Each ADR should document a significant architectural decision, including the context, the decision made, the trade-offs considered, and the consequences of the decision. ADRs should be atomic and focused on a single decision.
- `concepts/`: High-level concepts and mental models for understanding the codebase. This includes explanations of key concepts, abstractions, and design patterns used in the codebase. Might be replaced with a single `concepts.md` file for small wikis.
- `workflows/`: High-level documentation of repeatable workflows and process patterns relevant to the codebase — development cycle flows, testing strategies, CI/CD pipeline overviews, and deployment sequences.
- `guides/`: Practical instructions for working with the codebase. This includes coding standards, contribution guidelines, environment setup, and other how-to references.
- `references/`: Reference material for working with the codebase. This includes things like API documentation, configuration details, and other reference material. Might be replaced with a single `references.md` file for small wikis.

## Core Operations

Detailed operational procedures and templates are modularized in the `references/` directory. Based on the detected trigger, follow the appropriate guide:

### 1. `init` — Bootstrap Wiki Structure

- **Trigger**: When initializing the codebase-embedded wiki across an un-documented or newly explored codebase, establishing `wiki/index.md` files and minimal tracking metadata.
- **Procedure**: See [init-operation.md](references/init-operation.md).

### 2. `ingest` — Ingest PRs, Commits, & External Docs

- **Trigger**: When incorporating new architectural decisions, PR diffs, commit ranges, or external documentation (RFCs, design docs, onboarding notes) into module `wiki/` pages and updating `wiki/index.md`.
- **Procedure**: See [ingest-operation.md](references/ingest-operation.md).

### 3. `sync` — Differential Git Synchronization

- **Trigger**: When incrementally synchronizing existing wiki pages with source code changes introduced since the last tracked git commit.
- **Procedure**: See [sync-operation.md](references/sync-operation.md).

### 4. `query` — Knowledge Graph Traversal & Context Building

- **Trigger**: When gathering context to understand the codebase and prepare for development tasks.
- **Procedure**: See [query-operation.md](references/query-operation.md).

### 5. `lint` — Wiki Health Check & Audit

- **Trigger**: When performing a health check on the wiki to ensure it is well-organized, up-to-date, and free of errors.
- **Procedure**: See [lint-operation.md](references/lint-operation.md).

## Execution Guidelines

1. **Focus on High-Level Rationale**: Explain *why* design decisions were made and *how* components interact at a high level. Never copy code blocks, function signatures, or verbatim logic into wiki pages.
2. **Include 3-Level Deep `index.md`**: Ensure every `wiki/` folder has an `index.md` acting as a 3-level TOC for fast agent discovery.
3. **Decouple README Files from OKF**: Do not add OKF frontmatter to `README.md` files. Treat READMEs strictly as entry points (scope + usage) that link to `wiki/index.md`.
4. **Keep `.wiki-meta.json` Minimal**: Store only necessary operational tracking state (`version`, `last_sync_commit`, `last_sync_timestamp`).
5. **Use Relative Links Exclusively**: Use relative paths for all cross-page links (`[text](page.md)`) and source code references (`[symbol](../../src/<path/to/file.ext>#L10-L25)`). Avoid absolute `file:///` URLs.
6. **Isolate Wiki Pages in `wiki/`**: Ensure wiki pages live exclusively inside module `wiki/` subdirectories (`wiki/**/*.md`). Never place wiki Markdown files directly among source code files. When the number of pages grows large, organize them into named subdirectories within `wiki/` (e.g., `concepts/`, `decisions/`, `guides/`), where each subdirectory name mirrors a top-level section heading in `wiki/index.md`.

## References

For complete schema definitions, Markdown templates, and additional reference material, refer to the following documentation:

- [okf-spec.md](references/okf-spec.md) (frontmatter schema & wiki page template)
- [index-templates.md](references/index-templates.md) (TOC templates)
- [readme-template.md](references/readme-template.md) (module README template)
- [wiki-meta-schema.md](references/wiki-meta-schema.md) (`.wiki-meta.json` schema)
