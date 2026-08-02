# Operation: `ingest` (Ingest External Insights, PRs, or Commits)

The `ingest` operation updates or expands the codebase-embedded wiki using external sources, pull requests, commit ranges, or provided documentation (RFCs, design docs, onboarding notes, incident postmortems). It distills raw delta information into high-level architectural rationales (*why*) and conceptual mechanics (*how*) within target module `wiki/` directories.

---

## When to Run `ingest`

- Merging or reviewing a Pull Request (PR) that introduces new concepts, architecture, or design decisions.
- Ingesting a commit range (e.g., `git diff v1.0..v1.2` or feature branch).
- Incorporating an external text document, RFC, architectural design document, or user notes into the codebase knowledge base.
- Generating architectural design documentation from the current codebase state.

---

## Detailed Step-by-Step Procedure

### Precondition: Bootstrap Wiki Skeleton If Needed

If the codebase has not been bootstrapped, perform the [init operation](init-operation.md) first to ensure the minimal wiki structure exists.

### Step 1: Parse the Input Source

1. Determine the source type:
   - **Pull Request / Commit Range:** Extract modified files, diffs, PR description, and commit messages using `git log` or `git diff`.
   - **External Document / RFC / Design Doc:** Read the provided file or text.
   - **Current Codebase State:** Generate an architectural design documentation from the current codebase state.
2. Identify the target modules affected by the input source based on file paths or domain topics.

### Step 2: Map Insights to Modules & High-Level Wiki Topics

1. For each affected module, locate its local `README.md` and `wiki/` subdirectory.
2. Check existing OKF pages in `<module>/wiki/**/*.md` and inspect `<module>/wiki/index.md`.
3. Distill input into high-level architectural concepts (**why** decisions were made, **how** systems interact). **Do NOT copy code snippets, commit messages, or raw PR diffs into Markdown**.
4. Decide whether to:
   - **Update an existing OKF page**: Add new architectural insights, update `timestamp`.
   - **Create a new OKF page**: If the input introduces a new architectural concept or design decision. Place the new page inside `<module>/wiki/<category>/<topic-slug>.md` (e.g. `concepts/`, `decisions/`, etc.), reflecting the upper levels of the wiki structure in the directory tree.

### Step 3: Write or Update OKF Wiki Pages & Update `wiki/index.md` TOC

> See [okf-spec.md](okf-spec.md) for the OKF frontmatter schema and wiki page template, and [index-templates.md](index-templates.md) for the `wiki/index.md` TOC template.

1. Format all new/updated pages with standard OKF YAML frontmatter:
   - Set `timestamp` to current ISO-8601 string.
   - Set `type` to one of: `architecture`, `decision-record`, `concept`, `guide`.
   - Update `tags` to match OKF taxonomy.
   - Add or update `related` links to other OKF pages using relative Markdown paths.
2. Synthesize clear, durable technical descriptions focusing on high-level system behavior.
3. **Update `<module>/wiki/index.md`**: Whenever new wiki pages or major section headers (`# Title`, `## Section`, `### Sub-section`) are added or modified, update the **3-level deep Table of Contents** in `<module>/wiki/index.md`.

### Step 4: Ensure README Files Remain Simple Entry Points

1. Ensure module `README.md` files are **not** updated with OKF frontmatter.
2. Verify module `README.md` files contain a link to `wiki/index.md`.

---

## Verification Criteria for `ingest`

- [ ] All new/modified OKF pages live strictly inside `<module>/wiki/**/*.md` (or global `wiki/**/*.md`), reflecting upper wiki levels in directory subdirectories.
- [ ] Technical content reflects high-level architectural rationale (**why/how**) rather than temporary PR chatter or code snippet copies.
- [ ] `wiki/index.md` in affected directories is updated with a **3-level deep Table of Contents**.
- [ ] `README.md` files do **not** use OKF frontmatter and link to `wiki/index.md`.
