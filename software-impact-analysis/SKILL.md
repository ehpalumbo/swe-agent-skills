---
name: software-impact-analysis
description: Performs a comprehensive Software Impact Analysis for a feature request or bug fix on an existing codebase. Helps identify affected components, explore solution alternatives, evaluate risks, ask clarifying questions, and document the recommended way forward.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.1.0"
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

Follow these sequential steps during the analysis:

### 1. Understand the Requirement

- Analyze the user request, bug report, or issue description.
- Extract the core objective, success criteria, and any explicit constraints (e.g., performance, technology stack, security).
- Identify any assumptions or ambiguities in the requirement that need validation.

### 2. Explore the Problem Space

- Locate the relevant entry points in the codebase.
- Analyze how the system currently handles the scenario described in the requirement.
- Identify the modules, APIs, or data flows involved in the execution path.

### 3. Collect Sufficient Context

- Search the codebase for related logic, utilities, or configurations and their callers.
- Read existing documentation, READMEs, API specifications, and database schemas.
- Identify existing code patterns, library dependencies, or helper functions that should be reused to maintain consistency.
- Explore existing automated test suites and coverage for the affected codebase areas to understand how regressions are currently caught.

### 4. Identify Affected Components

- Map out the files, packages, database tables, or third-party APIs that will require modification, creation, or deletion.
- Assess the scope of the change: is it local (single module) or global (cross-cutting, changing public API contracts)?
- Document any potential side-effects or regression risks on other parts of the application.

### 5. Ask Clarifying Questions to the User

- Compile a list of specific, clear, and non-trivial questions to resolve open questions, ambiguity, or design trade-offs.
- Avoid asking questions that can be answered by studying the codebase; focus on product behavior, design choices, or business logic.

### 6. Evaluate Solution Approaches

- Define at least two implementation strategies (e.g., a direct/minimal change vs. a more robust/refactored design).
- For each approach, document:
  - High-level design and how it works.
  - Pros (simplicity, execution speed, performance, etc.).
  - Cons (technical debt, complexity, maintenance effort, etc.).
  - Risk Level (Low/Medium/High) and potential regression points.

### 7. Recommend Way Forward

- Select the best solution approach based on the trade-offs evaluated.
- Provide a clear, technical rationale for why this approach was chosen.
- Outline the high-level sequence of steps to execute the plan.

---

## Output Format

The final deliverable of this skill must be a Software Impact Analysis Report.

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) **only when you are ready to write the final report**, then:

1. Fill out all sections of the template based on your findings and analysis.
2. Save or present the report to the user as requested.

### Self-Check (before delivering)

Before presenting the report:

- Verify that every file, symbol, and table listed in the "Affected Components" table actually exists in the codebase.
- Verify that every "Remaining Open Question" is genuinely unanswerable from code — if it can be resolved by studying the codebase, resolve it instead of deferring it.
- Re-walk the codebase for any component that was not directly verified (e.g., relying on a symbol name without confirming its callers or importers).

Fix any discrepancies and repeat until the report passes all checks before finalizing.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Trusting symbol names is not enough.** A function may be called from many places — always hunt down its callers before declaring a change isolated.
- **A shared/module-level util can ripple across multiple features.** Check all importers, not just the one named in the request.
- **Do not skip the clarifying-questions step for ambiguous requirements just because the request is short.** Product behavior and trade-offs are rarely inferable from code alone.
