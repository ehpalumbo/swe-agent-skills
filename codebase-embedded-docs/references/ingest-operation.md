# Operation: `ingest` (Ingest External Insights, PRs, or Commits)

The `ingest` operation updates or expands the codebase-embedded documentation using external sources, pull requests, commit ranges, or provided documentation (RFCs, design docs, onboarding notes, incident postmortems). It distills raw delta information into high-level architectural rationales (*why*) and conceptual mechanics (*how*) within target module `docs/` directories, targeted for agent consumption.

---

## When to Run `ingest`

- Merging or reviewing a Pull Request (PR) that introduces new concepts, architecture, or design decisions.
- Ingesting a commit range (e.g., `git diff v1.0..v1.2` or feature branch).
- Incorporating an external text document, RFC, architectural design document, or user notes into the codebase knowledge base.
- Generating architectural design documentation from the current codebase state.

---

## Detailed Step-by-Step Procedure

### Precondition: Bootstrap Docs Skeleton for Affected Modules Only

If an affected module (identified in Step 1) has not been bootstrapped, perform the [init operation](init-operation.md) **scoped to that module only**, creating just the `docs/` branches where the ingested content will be placed. Do not bootstrap the whole repository and do not create structure for modules that the input source does not touch.

### Step 1: Parse the Input Source & Identify Affected Modules

1. Determine the source type:
   - **Pull Request / Commit Range:** Extract modified files, diffs, PR description, and commit messages using `git log` or `git diff`.
   - **External Document / RFC / Design Doc:** Read the provided file or text.
   - **Current Codebase State:** Generate an architectural design documentation from the current codebase state.
2. Identify the target modules affected by the input source based on file paths or domain topics.
3. **Keep the scope limited to these affected modules.** Modules not implicated by the input source are out of scope — do not create, modify, or re-index their documentation.

### Step 2: Map Insights to Modules & High-Level Docs Topics

1. For each **affected** module, locate its local `README.md` and `docs/` subdirectory.
2. Check existing OKF pages in `<module>/docs/**/*.md` and inspect `<module>/docs/index.md`.
3. Distill input into high-level architectural concepts that reflect **why** decisions were made and **how** systems interact. **Do NOT copy code snippets, commit messages, or raw PR diffs into Markdown**.
4. Decide whether to:
   - **Update an existing OKF page**: Add new architectural insights, update `timestamp`.
   - **Create a new OKF page**: If the input introduces a new architectural concept or design decision. Place the new page inside `<module>/docs/<category>/<topic-slug>.md` (e.g. `concepts/`, `decisions/`, etc.), reflecting the upper levels of the docs structure in the directory tree.

### Step 2.5: Confirm New Pages & Branches with the User

1. Before creating any **new** OKF page or `docs/<category>/` subdirectory, present the proposed additions to the user (which category, which page title, which module).
2. **Confirm the placement is acceptable** — especially whether the new branch belongs in the affected module and the category is appropriate. Ask for the missing context if the input source does not already supply it (e.g., "What is the boundary of this new concept?").
3. Do not create a new branch or destination directory that will end up empty; only create a branch when it will actually hold a page.

### Step 3: Write or Update OKF Docs Pages & Update `docs/index.md` TOC

> See [okf-spec.md](okf-spec.md) for the OKF frontmatter schema and docs page template, and [index-templates.md](index-templates.md) for the `docs/index.md` TOC template.

1. Format all new/updated pages with standard OKF YAML frontmatter:
   - Set `timestamp` to current ISO-8601 string.
   - Set `type` to one of: `architecture`, `decision-record`, `concept`, `guide`.
   - Update `tags` to match OKF taxonomy.
   - Add or update `related` links to other OKF pages using relative Markdown paths.
   - Set `resource` to the source/configuration file(s) the page documents, when it tracks specific files.
   - Set `stale_after` when the rationale has a known expiry (e.g., a temporary workaround or time-sensitive decision).
2. Synthesize clear, durable technical descriptions focusing on high-level system behavior.
3. **Update `<module>/docs/index.md` for affected modules only**: Whenever new docs pages or major section headers (`# Title`, `## Section`, `### Sub-section`) are added or modified, update the **3-level deep Table of Contents** in `<module>/docs/index.md`. Leave the indexes of unaffected modules untouched.

### Step 4: Ensure README Files Remain Simple Entry Points

1. Ensure module `README.md` files are **not** updated with OKF frontmatter.
2. Verify affected module `README.md` files contain a link to `docs/index.md`.
3. Do not modify README files of modules that were not affected by the input source.

---

### Verification Criteria for `ingest`

- [ ] Only **affected modules** (identified in Step 1) had any files created, modified, or re-indexed.
- [ ] New OKF pages and `docs/<category>/` branches were confirmed with the user and only created where a page will actually be placed (no empty branches).
- [ ] All new/modified OKF pages live strictly inside `<module>/docs/**/*.md` (or global `docs/**/*.md`), reflecting upper docs levels in directory subdirectories.
- [ ] Technical content reflects high-level architectural rationale (**why/how**) rather than temporary PR chatter or code snippet copies.
- [ ] `docs/index.md` in affected directories is updated with a **3-level deep Table of Contents**; unaffected indexes are unchanged.
- [ ] `README.md` files do **not** use OKF frontmatter and link to `docs/index.md`.
