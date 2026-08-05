# Module README Template (`<module>/README.md`)

> [!NOTE]
> Module README files are **not** part of the OKF docs and do **not** use OKF frontmatter. They provide an initial starting point detailing scope and usage, and always link to `docs/index.md`.

---

## Decoupling Rules

- `README.md` files must **never** include OKF YAML frontmatter (no `---` frontmatter blocks).
- Every module `README.md` must contain a valid relative link to `docs/index.md`.
- `README.md` content is limited to: module scope & purpose, usage & integration guidance, and the link to the module docs index.
- Respect existing `README.md` structure; only append the docs index link if necessary.

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
