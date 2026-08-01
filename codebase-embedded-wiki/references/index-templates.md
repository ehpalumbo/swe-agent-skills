# Wiki Index Templates (`wiki/index.md`)

Every `wiki/` directory MUST contain an `index.md` Table of Contents indexed **up to 3 levels deep**. This allows AI agents to rapidly discover wiki structure without reading individual pages. Entries must provide just enough context for an agent to infer when it is relevant to load each file — no more.

> [!IMPORTANT]
> Subdirectories inside `wiki/` reflect top-level TOC sections via Markdown heading levels. Whenever wiki pages or major section headers are added or modified, the corresponding `wiki/index.md` must be updated to stay current.

---

## Module Wiki Index Template (`<module>/wiki/index.md`)

```markdown
# <Module Name> Wiki

> Quick discovery index. Load individual pages for full details.

## <Section> (`<section>/`)

### [<Page Title>](<section>/<page>.md)
<One-sentence description of what this page covers and when to load it.>
- <Key heading or sub-topic>
- <Key heading or sub-topic>

### [<Page Title>](<section>/<page>.md)
<One-sentence description.>
- <Key heading or sub-topic>

## <Section> (`<section>/`)

### [<Page Title>](<section>/<page>.md)
<One-sentence description.>
- <Key heading or sub-topic>
- <Key heading or sub-topic>
```

---

## Global Repository Wiki Index Template (`wiki/index.md` at root)

```markdown
# Repository Wiki

> Cross-cutting knowledge base index. Load individual pages for full details.

## <Section> (`<section>/`)

### [<Page Title>](<section>/<page>.md)
<One-sentence description of what this page covers and when to load it.>
- <Key heading or sub-topic>
- <Key heading or sub-topic>

## Module Wikis

### [<Module Name>](../<module>/wiki/index.md)
<One-sentence description of the module's domain and what its wiki covers.>
```
