---
name: codebase-embedded-docs
description: Operate codebase-embedded documentation (init, ingest, query, lint) following Google's Open Knowledge Format (OKF). Use when initializing repository docs, ingesting PRs or design docs, gathering architecture context, or auditing documentation health for agentic development.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.0.0-SNAPSHOT"
---

# Codebase-Embedded Documentation

Use this skill to create, maintain, query, and health-check **codebase-embedded documentation** that lives alongside code and helps agents understand how and why the code is implemented.

## Definition

Codebase-embedded documentation stores curated, structured knowledge directly inside the repository alongside target modules that are meant to complement, not duplicate, implementation-level documentation. It is intended primarily to be consumed by AI agents and human developers when gathering context for software engineering tasks.

Documents are never mixed directly with source files; instead, each project module has a module-level `README.md` as an initial entry point, and a separate `docs/` subdirectory containing an `index.md` as a table of contents for the documentation pages that live inside the `docs/` directory. This structure is repeated recursively for each module, splitting the documentation between the global `docs/` directory for cross-cutting concerns and module-specific `docs/` directories for module-specific concerns.

## Objective & Philosophy

- **Complementary, Not Redundant**: The docs MUST complement, not duplicate, implementation-level documentation (e.g., Swagger/OpenAPI specs, Protocol Buffer definitions, JavaDocs or Python docstrings). Agents can always scan source code to learn implementation details, signatures, and syntax.
- **Focus on High-Level Constructs**: The docs must explain **why** things were done the way they were done (design decisions, trade-offs, constraints, rationale, motivation) and **how** things work at a high level (architecture, patterns, system boundaries, data flows, mental models, workflows).
- **Persistent Curator Pattern**: Formalizing Andrej Karpathy's [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) for documentation, act as a persistent curator building a compounding, interlinked knowledge graph embedded close to the code.

## Documentation Architecture & Directory Conventions

A codebase-embedded docs layout places documentation in dedicated `docs/` subdirectories alongside target module `README.md` files. Docs must NOT be mixed directly into source code folders:

```text
<repository-root>/
├── README.md                      # Repository Overview (scope, usage, links to docs/index.md)
├── docs/                          # Global / Cross-Cutting Architecture Docs Directory
│   ├── index.md                   # Global Docs Table of Contents
│   ├── architecture/              # Subdirectory reflecting upper-level docs section
│   │   └── overview.md            # OKF Docs Page (type: architecture)
│   └── guides/                    # Subdirectory reflecting upper-level docs section
│       └── coding-standards.md    # OKF Docs Page (type: guide)
└── <module-name>/                 # e.g., auth-service, billing-service (module root)
    ├── README.md                  # Module Entry Point (scope, usage, links to docs/index.md)
    ├── docs/                      # Module Docs Directory (supports subdirectories reflecting upper docs levels)
    │   ├── index.md               # Module Docs Table of Contents
    │   ├── concepts/              # Docs subdirectory for concepts section
    │   │   └── topic-a.md         # OKF Docs Page (type: concept)
    │   └── decisions/             # Docs subdirectory for decision records section
    │       └── adr-001.md         # OKF Docs Page (type: decision-record)
    └── src/                       # Standard source root
        ├── main/                  # Production application sources
        └── test/                  # Unit and integration tests
```

**Open Knowledge Format (OKF)**: An open, vendor-neutral standard share organizational knowledge as a simple directory of Markdown files with YAML frontmatter. Docs pages are concept-based files and must follow this format.

**Single vs Multiple Modules**: For single-module codebases, the global `docs/` directory should capture all architectural knowledge. When a codebase contains more than one module, the global `docs/` directory should be used for cross-cutting architecture concerns (e.g., shared libraries, common patterns, global design decisions), while each module should maintain its own `docs/` subdirectory for module-specific concerns.

**README Files vs Docs Pages**: `README.md` files do **not** follow OKF format. They provide a quick introduction to the scope and usage of the module and point directly to `docs/index.md`.

**Docs Index (`index.md`)**: Every `docs/` directory MUST contain an `index.md` file. It serves as an optimized Table of Contents indexed **up to 3 levels deep** to allow AI agents to rapidly discover documentation structure without reading all individual pages. Index files do **not** follow OKF format.

### Recommended Structure

The `docs/` directory should be organized into **categories** (select what applies to each module/level):

- `specs/`: High-level specifications for the project. This includes things like project goals, functional and non-functional requirements, and other specifications that may not be captured in the codebase itself.
- `architecture/`: High-level architecture, system overview, technology stack, and architectural patterns.
- `decisions/`: Architecture Decision Records (ADRs). Each ADR should document a significant architectural decision, including the context, the decision made, the trade-offs considered, and the consequences of the decision. ADRs should be atomic and focused on a single decision.
- `domain/`: High-level concepts and mental models implemented in the codebase. This includes explanations of key abstractions, business rules, constraints, stakeholders.
- `workflows/`: High-level documentation of repeatable workflows and process patterns relevant to the codebase — development cycle flows, testing strategies, CI/CD pipeline overviews, and deployment sequences.
- `guides/`: Practical instructions for working with the codebase. This includes coding standards, contribution guidelines, environment setup, and other how-to references.

## Core Operations

Detailed operational procedures and templates are modularized in the `references/` directory of this agent skill. Based on the detected trigger, follow the appropriate guide:

### `init` — Bootstrap Documentation Structure

- **Trigger**: When initializing the codebase-embedded documentation for a repository by creating required `docs/` directories, `docs/index.md` files, module `README.md` entry points, and minimal tracking metadata.
- **Procedure**: See [init-operation.md](references/init-operation.md).

### `ingest` — Ingest PRs, Commits, & External Docs

- **Trigger**: When incorporating new architectural decisions, PR diffs, commit ranges, or external documentation (RFCs, design docs, onboarding notes) into module `docs/` pages and updating `docs/index.md`.
- **Procedure**: See [ingest-operation.md](references/ingest-operation.md).

### `query` — Knowledge Graph Traversal & Context Building

- **Trigger**: When gathering context to understand the codebase and prepare for development tasks.
- **Procedure**: See [query-operation.md](references/query-operation.md).

### `lint` — Documentation Health Check & Audit

- **Trigger**: When performing a health check on the docs to ensure they are well-organized, up-to-date, and free of errors.
- **Procedure**: See [lint-operation.md](references/lint-operation.md).

## Execution Guidelines

1. **Focus on High-Level Rationale**: Explain *why* design decisions were made and *how* components interact at a high level. Never copy code blocks, function signatures, or verbatim logic into docs pages.
2. **Include 3-Level Deep `index.md`**: Ensure every `docs/` folder has an `index.md` acting as a 3-level TOC for fast agent discovery.
3. **Decouple README Files from OKF**: Do not add OKF frontmatter to `README.md` files. Treat READMEs strictly as entry points (scope + usage) that link to `docs/index.md`.
4. **Use Relative Links Exclusively**: Use relative paths for all cross-page links (`[text](page.md)`) and source code references (`[symbol](../../src/<path/to/file.ext)#L10-L25)`). Avoid absolute `file:///` URLs.
5. **Isolate Docs Pages in `docs/`**: Ensure docs pages live exclusively inside module `docs/` subdirectories (`docs/**/*.md`). Never place documentation Markdown files directly among source code files.

## References

For complete schema definitions, Markdown templates, and additional reference material, refer to the following documentation:

- [okf-spec.md](references/okf-spec.md) (frontmatter schema & docs page template)
- [index-templates.md](references/index-templates.md) (TOC templates)
- [readme-template.md](references/readme-template.md) (module README template)
