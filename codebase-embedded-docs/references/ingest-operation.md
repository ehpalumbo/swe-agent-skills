# Operation: `ingest` (Ingest External Insights, PRs, or Commits)

The `ingest` operation expands the codebase-embedded documentation from external sources — pull requests, commit ranges, or external documents — distilling raw deltas into high-level *what/why/how* pages within affected module `docs/` directories.

---

## When to Run `ingest`

- Merging or reviewing a PR that introduces new concepts, architecture, or decisions.
- Ingesting a commit range (e.g., `git diff v1.0..v1.2` or a feature branch).
- Incorporating an external document (RFC, design document, user notes, conversations).
- Onboarding an undocumented or partially documented repository.
- Generating architecture docs from the current codebase state.

---

## Step-by-Step Procedure

### Step 1: Read the Input Source

Determine the source type:

- **Pull Request / Commit Range:** extract modified files, diffs, PR description, and commit messages.
- **Current Codebase State:** scan the current state of the codebase to generate architecture documentation.
- **External Document:** read the provided file or text and understand its structure.

### Step 2: Extract Concepts & Ideas

Distill the input into high-level concepts that can be interconnected as a knowledge graph.

**Don't copy code snippets, commit messages, or diffs** — capture the durable rationale, not the raw delta.

### Step 3: Identify Where the Knowledge Should Land

Identify the affected modules from file paths or domain topics. For each affected module, locate its `docs/index.md` and review candidate locations and pages for storing the new distilled information. Decide per piece: **extend/replace an existing page**, **create a new page**, or **create a new `docs/<category>/` branch** — always prefer the smallest fit.

Considerations for knowledge placement:

- **Prefer higher-level branches.** Identify logical components, not merely module folders. Consolidate `docs/` at parent modules so one parent `README.md` + `docs/` covers its layer submodules; never scaffold a `docs/` for a leaf layer that fits under its parent, nor for empty/placeholder modules.
- **Plan minimal skeletons only where content lands** — `docs/` + `docs/index.md` with real, known entries (no fabricated headings), and only the `docs/<category>/` branches that will hold pages. Infer what you can from project manifests and layout.
- **Watch page size.** Propose splitting pages that approach ~**500 lines** into focused pages instead of bloating existing pages or creating very long ones.
- **Remove obsolete pages.** Do not keep stale information for future reference; codebase-embedded documentation is version-controlled.

### Step 4: Present the Plan

Draft a high-level proposal for ingesting distilled information into the docs and discuss it with the user. Iterate on the plan as needed until the user signs it off.

### Step 5: Create/Update Pages

Create the confirmed minimal skeletons from Step 3 — `docs/` directories and only the `docs/<category>/` branches that hold pages. Proceed to create or update pages as previously agreed. Format all new/updated pages with OKF YAML frontmatter (see [okf-spec.md](okf-spec.md)).

Considerations for page writing:

- **Avoid repeating information.** Prefer linking to relevant pages.
- **Enrich content with inline relative links** to enable navigation within and across pages.
- **Leverage hierarchy levels** to support progressive disclosure of information.

### Step 6: Update Indexes

1. Refresh `**/docs/index.md` TOCs for affected modules.
2. Ensure READMEs contain entry points to docs. Verify they link to `docs/index.md`. No OKF frontmatter.

> See [index-templates.md](index-templates.md) and [readme-template.md](readme-template.md).

---

## Verification Criteria

- [ ] Only **affected modules** had files created, modified, or re-indexed.
- [ ] Storage placement was planned **read-only** — no files created until the Step 4 sign-off.
- [ ] `docs/` roots were **consolidated high in the tree**: parent modules with layer submodules hold one `README.md` + `docs/` covering their children; leaf layers have no redundant `docs/`.
- [ ] Missing skeletons planned in Step 3 **only where content will land** — no whole-repo bootstrap for a scoped PR, no pre-scaffolded `docs/<category>/` branches.
- [ ] Every planned `docs/index.md` reflects real context (not fabrications) and lists only branches that hold pages.
- [ ] The storage plan and new pages/`docs/<category>/` branches were confirmed; directories opened **on demand**, only where a page lands (no empty branches).
- [ ] Pages over ~**500 lines** were evaluated and any split was **proposed** before restructuring.
- [ ] Content reflects high-level *what/why/how*, not PR chatter or copied code.
- [ ] `docs/index.md` in affected dirs lists current pages as one-sentence bullets under their directory headings; every affected `docs/` directory contains an `index.md`; unaffected indexes unchanged.
- [ ] `README.md` files have no OKF frontmatter and link to `docs/index.md`.
