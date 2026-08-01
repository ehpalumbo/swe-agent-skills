# Module README Template (`<module>/README.md`)

> [!NOTE]
> Module README files are **not** part of the wiki and do **not** use OKF frontmatter. They provide an initial starting point detailing scope and usage, and always link to `wiki/index.md`.

---

## Decoupling Rules

- `README.md` files must **never** include OKF YAML frontmatter (no `---` frontmatter blocks).
- Every module `README.md` must contain a valid relative link to `wiki/index.md`.
- `README.md` content is limited to: module scope & purpose, usage & integration guidance, and the link to the module wiki index.

---

## Standard Module README Template (`<module>/README.md`)

```markdown
# `<Module Name>`

## Scope & Purpose
Concise overview of what this module does, its business boundaries, and key responsibilities.

## Usage & Integration
How other modules interact with this module, key interfaces exposed, and environment configuration required.

## Module Wiki Index
For detailed architectural rationale, design decisions, and system mechanics, see the module wiki:
- 📖 **[Module Wiki Index](wiki/index.md)**
```
