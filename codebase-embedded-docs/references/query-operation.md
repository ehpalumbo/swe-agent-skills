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

Analyze the question or task topic; extract key domain terms, module names, or concepts.

### Step 2: Fast-Path Discovery via `docs/index.md`

1. Read `<module>/README.md` for scope/usage; follow the link to `docs/index.md`.
2. Scan the directory-heading TOC in `<module>/docs/index.md` (or root `docs/index.md`).
3. Jump to the target OKF pages without reading unrelated files.
4. Follow OKF metadata and links (`tags`, `type`, `description`, `related`) to complete context.

### Step 3: Synthesize the Answer

Answer directly, highlighting *why* decisions were made and high-level *how*. Avoid dumping code blocks or verbatim source. Provide relative links to relevant OKF pages and key source files.

---

## Verification Criteria

- [ ] Leverages `docs/index.md` (directory-heading TOC) for fast-path discovery.
- [ ] Synthesizes high-level *what/why/how*, not raw code snippets.
- [ ] Includes relative links to OKF pages and source code.
