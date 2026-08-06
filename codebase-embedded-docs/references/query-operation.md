# Operation: `query` (Navigate & Query Documentation Context)

The `query` operation lets agents or developers navigate the codebase-embedded documentation to answer technical questions, build context for engineering tasks, and understand domain concepts without reading raw source.

---

## When to Run `query`

- Preparing context before an engineering or implementation task.
- Answering architecture, design-rationale, or domain-logic questions.
- Onboarding to a module or tracing component boundaries.

---

## Step-by-Step Procedure

### Step 1: Parse Query Intent & Identify Scope

1. Analyze the question or task topic; extract key domain terms, module names, or concepts.
2. Determine the query type: **Module-Specific**, **Architecture / Cross-Cutting**, or **Design Record Inquiry**.

### Step 2: Fast-Path Discovery via `docs/index.md`

1. Read `<module>/README.md` for scope/usage; follow the link to `docs/index.md`.
2. Scan the 3-level TOC in `<module>/docs/index.md` (or root `docs/index.md`).
3. Jump to the target OKF page without parsing unrelated files.
4. Follow OKF metadata and links (`tags`, `type`, `description`, `related`) to complete context.

### Step 3: Verify Freshness & Staleness

1. Check `stale_after` on candidate pages.
2. If passed, inspect the `resource` file(s) (per [okf-spec.md](okf-spec.md)) or run `git log` to confirm the rationale still holds.

### Step 4: Synthesize the Answer

1. Answer directly, highlighting *why* decisions were made and high-level *how*.
2. Avoid dumping code blocks or verbatim source.
3. Provide relative links to relevant OKF pages and key source files.

---

## Verification Criteria

- [ ] Leverages `docs/index.md` (3-level TOC) for fast-path discovery.
- [ ] Synthesizes high-level *why/how*, not raw code snippets.
- [ ] Includes relative links to OKF pages and source code.
- [ ] Flags any stale pages encountered.