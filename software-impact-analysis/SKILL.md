---
name: software-impact-analysis
description: Performs a comprehensive Software Impact Analysis for a feature request or bug fix on an existing codebase. Helps identify affected components, explore solution alternatives, evaluate risks, ask clarifying questions, and document the recommended way forward.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.2.0"
---

# Software Impact Analysis

Use this skill to conduct a thorough impact analysis when presented with a feature request, bug report, or refactoring proposal. This process ensures all modifications are well-planned, risks are managed, and dependencies or regressions are identified before writing code.

---

## When to Use

- **Feature Requests:** Before implementing new features or extending existing functionality to understand where the changes fit.
- **Bug Fixes:** Before making code modifications to resolve a bug, to verify we fix the root cause and avoid introducing regressions.
- **Refactoring & Architectural Changes:** When modifying internal structures, APIs, or database schemas.
- **Ambiguous Requirements:** When a task is high-level or underspecified, prompting the need to explore the codebase and ask clarifying questions first.

---

## Procedure

Follow these steps during the analysis — iterate and parallelize exploration where possible:

### 1. Understand the Requirement

- Analyze the user request, bug report, or issue description.
- Extract the core objective, success criteria, and any explicit constraints (e.g., performance, technology stack, security).
- Identify any assumptions or ambiguities in the requirement that need validation.

### 2. Evidence-Grounded Exploration (iterate, parallelize where possible)

- Run parallel searches for entry points, related logic, callers/importers, configs, docs/schemas, and covering tests. Use subagents for independent areas in large codebases.
- Trace the execution path: entry point → modules/APIs/data flows → side effects. Record `file:line` evidence; do not declare scope isolated without caller/importer check.
- Produce the affected-components set directly (files, packages, tables, APIs + local vs. global scope + regression risks). Reuse existing patterns/helpers where found.
- Check git history (`log`, `blame`, recent PRs) for churn and hotspots in affected areas to calibrate risk.
- Ground bug fixes in execution: reproduce with a script/logs before proposing a fix. Note existing tests covering affected areas and coverage gaps.
- Stop when the affected set stabilizes (no new callers/importers) or scope is clearly bounded; do not exhaustively read the repo for local changes.

### 3. Ask Clarifying Questions (blockers only)

- Ask only blocking questions that change scope/design and cannot be answered from code (product behavior, design trade-offs). Max 3-5, each with a proposed default.
- Record non-blocking ambiguities as Assumptions in the report and proceed; do not block on nice-to-know.
- Skip questioning entirely for unambiguous bug fixes or small tasks.

### 4. Evaluate Solution Approaches (risk-proportional)

- Trivial / local / low-risk: one approach is enough; note rejected alternatives in one line.
- Non-trivial / cross-cutting / API, DB, or complex bug: define at least two strategies (e.g., direct/minimal vs. robust/refactored).
- For each approach, document:
  - High-level design and how it works.
  - Pros (simplicity, execution speed, performance, etc.).
  - Cons (technical debt, complexity, maintenance effort, etc.).
  - Risk Level (Low/Medium/High) and potential regression points.

### 5. Recommend Way Forward

- Select the best approach with technical rationale (trade-offs, scalability, maintenance).
- Outline the high-level execution sequence only — hand off to the `implementation-planning` skill for phased tasks; do not write detailed tasks here.
- State effort estimate (S/M/L) and rollback/migration needs if applicable.

---

## Output Format

The final deliverable of this skill must be a Software Impact Analysis Report.

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) **only when you are ready to write the final report**, then:

1. Fill out all sections of the template based on your findings and analysis. Scale depth to risk: low-risk/local changes may use a concise report (Summary + Affected Components + Verification); cross-cutting/API/DB/high-risk changes require the full template including Approaches, Security/Performance, and Rollback.
2. Save or present the report to the user as requested. Omit inapplicable subsections instead of filling with placeholders.

### Self-Check (before delivering)

Before presenting the report:

- Every affected file cites a verified `path:line` on disk; every symbol was confirmed via caller/importer search, not inferred from its name.
- Every "Remaining Open Question" is genuinely unanswerable from code/search — if resolvable, resolve it instead of deferring it.
- No placeholders (`[file/path]`, TBD) remain; omitted subsections are intentional per risk tier, not incomplete work.

Fix any discrepancies and repeat until the report passes all checks before finalizing.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Trusting symbol names is not enough.** A function may be called from many places — always hunt down its callers before declaring a change isolated.
- **A shared/module-level util can ripple across multiple features.** Check all importers, not just the one named in the request.
- **Ask blockers, assume the rest.** Product behavior and trade-offs are rarely inferable from code — ask max 3-5 blocking questions with defaults, record the rest as Assumptions and proceed.
