# README Templates (root `README.md` & `<module>/README.md`)

> [!NOTE]
> READMEs are **not** OKF docs — no frontmatter. They're entry points for scope and usage, always linking to `docs/index.md`.

---

## Decoupling Rules

- Never include OKF YAML frontmatter (no `---` blocks).
- Always include a valid relative link to `docs/index.md`.
- Content limited to: scope & purpose, usage & integration, and the link to the docs index.
- Respect the structure of existing README files — never rewrite the whole file; only insert the docs index link section.

---

## Repository (Root) README Template — Agent-Initialized (`README.md` at repo root)

Used when the agent initializes codebase-embedded docs and the repository has no root README to preserve.

```markdown
# <Repository Name>

## Scope & Purpose

Concise overview of the repository, its business boundaries, and key responsibilities.

## Usage & Integration

How to build, run, and integrate with this repository; key interfaces exposed and environment configuration required.

## Documentation

Please refer to the [Repository Docs Index](docs/index.md) for further details.
```

---

## Module README Template (`<module>/README.md`)

```markdown
# `<Module Name>`

## Scope & Purpose

Concise overview of what this module does, its business boundaries, and key responsibilities.

## Usage & Integration

How other modules interact with this module, key interfaces exposed, and environment configuration required.

## Module Docs

Please refer to the [Docs Index](docs/index.md) for further details.
```

---

## Existing READMEs — Link to the Docs Index

When a README already exists, do **not** restructure it. Insert the docs index link section after the usage section (or before any Contributing/License boilerplate).

For an existing module README:

```markdown
## Module Docs

Please refer to the [Docs Index](docs/index.md) for further details.
```

For an existing repository root README:

```markdown
## Documentation

Please refer to the [Repository Docs Index](docs/index.md) for further details.
```
