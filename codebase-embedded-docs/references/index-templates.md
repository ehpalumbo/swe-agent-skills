# Docs Index Templates (`docs/index.md`)

Every `docs/` directory MUST contain an `index.md` — a **3-level deep** Table of Contents for fast agent discovery. Entries give just enough context to infer relevance — no more.

Top-level sections mirror the `docs/` subdirectory structure. Whenever pages or section headers change, update the corresponding `docs/index.md`. No OKF frontmatter in `index.md`.

---

## Module Docs Index Template (`<module>/docs/index.md`)

```markdown
# <Module Name> Docs

> Quick discovery index. Load individual pages for full details.

## <Section> (`<section>/`)
<Description of what this section covers. Focus on scope, not on line-by-line details.>

### [<Page Title>](<section>/<page>.md)
<One-sentence description of what this page covers and when it might be useful to load it.>
- <Key heading or sub-topic>
- <Key heading or sub-topic>
```

---

## Global Repository Docs Index Template (`docs/index.md` at root)

```markdown
# Repository Docs

> Cross-cutting knowledge base index. Load individual pages for full details.

## <Section> (`<section>/`)
<Description of what this section covers. Focus on scope, not on line-by-line details.>

### [<Page Title>](<section>/<page>.md)
<One-sentence description of what this page covers and when to load it.>
- <Key heading or sub-topic>
- <Key heading or sub-topic>

## Module Docs

### [<Module Name>](../<module>/docs/index.md)
<One-sentence description of the module's domain and what its docs covers.>
```
