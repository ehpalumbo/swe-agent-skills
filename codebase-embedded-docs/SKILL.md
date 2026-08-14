---
name: codebase-embedded-docs
description: Operate codebase-embedded documentation (ingest, query, lint) following Google's Open Knowledge Format (OKF). Use when managing repository documentation, ingesting PRs or design docs into module docs/, gathering documented architecture or design-rationale context, or auditing documentation health. Not for live source queries (signatures, APIs, implementations) or general-purpose linting — explore the code directly for those.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.0.0-beta.1"
---

# Codebase-Embedded Documentation

Use this skill to create, maintain, query, and health-check **codebase-embedded documentation** — curated, structured knowledge stored beside the code.

## Definition

Codebase-embedded docs **complement, not duplicate**, implementation-level docs (Swagger, Protobuf, JavaDoc — agents can scan source for signatures). They are primarily consumed by AI agents gathering context for engineering tasks.

Docs are never mixed into source files: the global `docs/` covers cross-cutting concerns, module `docs/` covers module-specific ones, and both lean toward consolidated parent branches.

## Objective & Philosophy

- **Complementary, Not Redundant**: Explain what source-level docs may not — the high-level *what*, *why* and *how*.
- **Focus on High-Level Constructs**: Design decisions, trade-offs, constraints, architecture, patterns, boundaries, data flows, and mental models.
- **Persistent Curator Pattern**: Act as a persistent curator (Karpathy's [LLM Wiki Pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)) building a compounding, interlinked knowledge graph near the code.

## Directory Conventions

Docs live in dedicated `docs/` subdirectories beside module `README.md` files, never mixed into source folders:

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

- **Open Knowledge Format (OKF)**: An open, vendor-neutral standard sharing knowledge as a directory of Markdown files with YAML frontmatter.
- **Single vs Multiple Modules**: Single-module codebases put all knowledge in global `docs/`. Multi-module repos use global `docs/` for cross-cutting concerns and per-module `docs/` for module-specific ones. **`docs/` live high in the tree**: parent modules whose submodules are application-stack layers hold one `README.md` + `docs/` covering their children, rather than scattering a `docs/` into every leaf submodule.
- **READMEs vs Docs Pages**: `README.md` files are **not** OKF — quick scope/usage intros that link to `docs/index.md`.
- **Docs Index**: Every `docs/` directory MUST contain an `index.md` — a flat TOC where headings represent subdirectories and bullets list individual pages, each distilled into a one-sentence summary. Not OKF.

### Recommended Structure

Organize `docs/` into documentation **categories**. For example:

- `specs/`: High-level project specifications defining goals, purpose, and functional/non-functional requirements.
- `architecture/`: High-level architecture, system overview, technology stack, architectural patterns.
- `decisions/`: Architecture Decision Records (ADRs) — context, decision, trade-offs, consequences. Atomic, one decision each.
- `domain/`: High-level concepts and mental models — key abstractions, business rules, constraints, stakeholders.
- `workflows/`: Repeatable process patterns — development flows, testing strategy, CI/CD, deployment sequences.
- `guides/`: Practical how-to references — coding standards, contribution guidelines, environment setup.

Considerations:

- **Start Small, Grow on Demand.** Organizing docs into categories is the full roadmap, **not the required layout**. Begin with the minimum a module needs — often a single `docs/index.md` plus a handful of pages — and open new `docs/<category>/` directories only when knowledge genuinely accumulates. Never pre-scaffold category branches.
- **Split Long Pages (>500 lines).** Keep pages focused. When a page approaches ~**500 lines**, propose splitting it into focused pages (one concept/decision/guide per page, `docs/index.md` updated) — never let a doc silently balloon.

## Core Operations

Detailed procedures are modularized in [`references/`](references/). Follow the guide matching the detected trigger.

### `ingest` — Ingest PRs, Commits, & External Docs

- **Trigger**: Incorporating new architectural decisions, conversations, PR diffs, commit ranges, or external docs (RFCs, design docs, onboarding notes) into module `docs/` pages and updating `docs/index.md`.
- **Procedure**: See [ingest-operation.md](references/ingest-operation.md).

### `query` — Knowledge Graph Traversal & Context Building

- **Trigger**: Gathering context to understand the codebase and prepare for development tasks.
- **Procedure**: See [query-operation.md](references/query-operation.md).

### `lint` — Documentation Health Check & Audit

- **Trigger**: Performing a health check to keep docs well-organized, up-to-date, and error-free.
- **Procedure**: See [lint-operation.md](references/lint-operation.md).

## Execution Guidelines

1. **High-Level Rationale**: Explain *why* and high-level *how*. Never copy code blocks, signatures, or verbatim logic into docs pages.
2. **Relative Links Only**: Use relative paths for cross-page links and source references; avoid absolute `file:///` URLs.
3. **Isolate Docs Pages in `docs/`**: Docs pages live exclusively inside `docs/**/*.md`, never among source files.
4. **Confirm-Then-Apply Posture**: Present proposed pages, `docs/<category>/` branches, and re-parented modules up front; apply after a single approval. For unattended runs, collect all changes and require one sign-off, then create.

## References

- [okf-spec.md](references/okf-spec.md) (frontmatter schema & docs page template)
- [index-templates.md](references/index-templates.md) (TOC templates)
- [readme-template.md](references/readme-template.md) (root & module README templates, incl. existing-README link section)
