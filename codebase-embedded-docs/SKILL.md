---
name: codebase-embedded-docs
description: Operate codebase-embedded documentation (init, ingest, query, lint) following Google's Open Knowledge Format (OKF). Use when initializing repository docs, ingesting PRs or design docs, gathering architecture context, or auditing documentation health for agentic development.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.0.0-SNAPSHOT"
---

# Codebase-Embedded Documentation

Use this skill to create, maintain, query, and health-check **codebase-embedded documentation** — curated, structured knowledge stored beside the code so agents understand how and why it is implemented.

## Definition

Codebase-embedded docs **complement, not duplicate**, implementation-level docs (Swagger, Protobuf, JavaDoc — agents can scan source for signatures). They are primarily consumed by AI agents gathering context for engineering tasks. Docs are never mixed into source files: each module has a `README.md` entry point plus a `docs/` subdirectory containing an `index.md` table of contents. The global `docs/` covers cross-cutting concerns; module `docs/` covers module-specific ones.

## Objective & Philosophy

- **Complementary, Not Redundant**: Explain what source-level docs don't — *why* and high-level *how*.
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
- **Single vs Multiple Modules**: Single-module codebases put all knowledge in global `docs/`. Multi-module repos use global `docs/` for cross-cutting concerns and per-module `docs/` for module-specific ones.
- **READMEs vs Docs Pages**: `README.md` files are **not** OKF — quick scope/usage intros that link to `docs/index.md`.
- **Docs Index**: Every `docs/` directory MUST contain an `index.md` — a TOC up to **3 levels deep** for fast agent discovery. Not OKF.

### Recommended Structure

Organize `docs/` into **categories** (select what applies per module/level):

- `specs/`: High-level project specifications — goals, functional/non-functional requirements not captured in code.
- `architecture/`: High-level architecture, system overview, technology stack, architectural patterns.
- `decisions/`: Architecture Decision Records (ADRs) — context, decision, trade-offs, consequences. Atomic, one decision each.
- `domain/`: High-level concepts and mental models — key abstractions, business rules, constraints, stakeholders.
- `workflows/`: Repeatable process patterns — development flows, testing strategy, CI/CD, deployment sequences.
- `guides/`: Practical how-to references — coding standards, contribution guidelines, environment setup.

> **Start Small, Grow on Demand.** Organizing docs into categories are the full roadmap, **not the required layout**. Begin with the minimum a module needs — often a single `docs/index.md` plus a handful of pages — and open new `docs/<category>/` directories only when knowledge genuinely accumulates. Never pre-scaffold category branches.

> **Split Long Pages (>500 lines).** Keep pages focused. When a page approaches ~**500 lines**, assess splitting into focused pages (one concept/decision/guide per page) and **propose** the split — never let a doc silently balloon.

## Core Operations

Detailed procedures are modularized in `references/`. Follow the guide matching the detected trigger:

### `init` — Bootstrap Documentation Structure

- **Trigger**: Initializing repository docs — creating required `docs/` directories, `docs/index.md` files, and module `README.md` entry points.
- **Procedure**: See [init-operation.md](references/init-operation.md). Minimalistic: infer module structure read-only, confirm scope with the user, seed each `docs/index.md` from context, and scaffold only branches where docs pages will land.

### `ingest` — Ingest PRs, Commits, & External Docs

- **Trigger**: Incorporating new architectural decisions, PR diffs, commit ranges, or external docs (RFCs, design docs, onboarding notes) into module `docs/` pages and updating `docs/index.md`.
- **Procedure**: See [ingest-operation.md](references/ingest-operation.md). Scoped to affected modules: new pages and `docs/<category>/` branches are confirmed with the user and opened on demand; ~500-line splits are proposed, not performed silently.

### `query` — Knowledge Graph Traversal & Context Building

- **Trigger**: Gathering context to understand the codebase and prepare for development tasks.
- **Procedure**: See [query-operation.md](references/query-operation.md).

### `lint` — Documentation Health Check & Audit

- **Trigger**: Performing a health check to keep docs well-organized, up-to-date, and error-free.
- **Procedure**: See [lint-operation.md](references/lint-operation.md). Flags oversized pages (>~500 lines) with a split proposal.

## Execution Guidelines

1. **High-Level Rationale**: Explain *why* and high-level *how*. Never copy code blocks, signatures, or verbatim logic into docs pages.
2. **3-Level Deep `index.md`**: Every `docs/` folder has an `index.md` as a 3-level TOC.
3. **Decouple READMEs from OKF**: No OKF frontmatter in `README.md`; treat as entry points linking to `docs/index.md`.
4. **Relative Links Only**: Use relative paths for cross-page links and source references; avoid absolute `file:///` URLs.
5. **Isolate Docs Pages in `docs/`**: Docs pages live exclusively inside `docs/**/*.md`, never among source files.
6. **Start Small, Grow on Demand**: Begin with the minimal structure; open new `docs/<category>/` directories only when content justifies them.
7. **Split Long Pages (>500 lines)**: Pages approaching 500 lines must be evaluated and a split proposed, with `docs/index.md` updated.

## References

- [okf-spec.md](references/okf-spec.md) (frontmatter schema & docs page template)
- [index-templates.md](references/index-templates.md) (TOC templates)
- [readme-template.md](references/readme-template.md) (module README template)
