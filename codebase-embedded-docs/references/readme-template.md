# Module README Template (`<module>/README.md`)

> [!NOTE]
> Module READMEs are **not** OKF docs — no frontmatter. They're an entry point for scope and usage, always linking to `docs/index.md`.

---

## Decoupling Rules

- Never include OKF YAML frontmatter (no `---` blocks).
- Always include a valid relative link to `docs/index.md`.
- Content limited to: scope & purpose, usage & integration, and the link to the module docs index.
- Respect existing README structure; only append the docs index link if necessary.

---

## Standard Module README Template (`<module>/README.md`)

```markdown
# `<Module Name>`

## Scope & Purpose
Concise overview of what this module does, its business boundaries, and key responsibilities.

## Usage & Integration
How other modules interact with this module, key interfaces exposed, and environment configuration required.

## Module Docs Index
For detailed architectural rationale, design decisions, and system mechanics, see the module docs:
- 📖 **[Module Docs Index](docs/index.md)**
```
