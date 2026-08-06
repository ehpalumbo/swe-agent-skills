# Docs Index Templates (`docs/index.md`)

Every `docs/` directory MUST contain an `index.md` — a flat Table of Contents for fast agent discovery. **Headings represent subdirectories; bullet-list entries represent individual docs files.** Entries give just enough context to infer relevance — no more.

Top-level sections mirror the `docs/` subdirectory structure. Whenever pages or section headers change, update the corresponding `docs/index.md`. No OKF frontmatter in `index.md`.

---

## Index Structure Rules

1. **Headings = Directories.** Each `docs/` subdirectory with content gets a `## <Short Name>` heading. The short name is a plain, human-readable form of the directory name with abbreviations expanded (e.g., directory `decisions/` → `## Decision Records`).
2. **Directory Description.** A one-to-two-sentence description of the contents expected under that directory MUST follow each heading. It states the directory's intended scope so agents can judge relevance without opening files. It must not start with a verb — describe the content, don't label it.
3. **Bullets = Files.** Every docs file appears as a single-line bullet: `- [<Title>](<relative/path>.md) - <one-sentence summary>`. No line breaks.
4. **One-Sentence Summary Rule.** A summary that synthesizes the page's content, not its purpose label.
5. **Directory Link (Optional).** To make the "directory, not file" nature explicit, link the heading to its directory with a trailing slash: `## [Decision Records](decisions/)`.
6. **Module Links at Root.** The root index lists modules as bullets too: `- [<Module Name>](../<module>/docs/index.md) - <one-sentence summary>`.

---

## Module Docs Index Template (`<module>/docs/index.md`)

```markdown
# <Module Name> Docs

> Quick discovery index. Load individual pages for full context.

## <Short Name>

<How the topics in this directory relate to the module: the boundaries, data flows, or mental models an agent should look for here behind the source signatures.>

- [<Page Title>](<directory>/<page>.md) - <The one-sentence content summary>.
- [<Page Title>](<directory>/<page>.md) - <How the one-sentence content summary reads>.
```

---

## Global Repository Docs Index Template (`docs/index.md` at root)

```markdown
# Repository Docs

> Cross-cutting knowledge base index. Load individual pages for full context.

## <Short Name>

<One-to-two-sentence description of what this section covers, scoped to repository-wide concerns.>

- [<Page Title>](<directory>/<page>.md) - <One-sentence content summary>.

## Module Docs

<The module indexes collected here, each scoped to a single module's domain.>

- [<Module Name>](../<module>/docs/index.md) - <One-sentence summary of the module's domain and what its docs cover>.
```
