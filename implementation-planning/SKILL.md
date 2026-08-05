---
name: implementation-planning
description: Prepares a detailed, actionable, and phased implementation plan from software requirement specifications and codebase analysis. Use this skill after performing a software impact analysis or when provided with explicit requirements.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.0.0"
---

# Implementation Planning

Use this skill to create a concrete, step-by-step implementation plan once a software requirement specification is defined or a software impact analysis has been completed. This ensures that coding proceeds with clear direction, minimizes integration risks, and enables incremental, reviewable commits.

---

## When to Use

- **Post-Impact Analysis:** Immediately after a Software Impact Analysis is approved to translate the recommended approach into actionable tasks.
- **Explicit Requirements Provided:** When a user provides explicit, well-defined requirements (SRS) and requests a development plan directly.
- **Complex Feature Rollouts:** Before writing code for features that span multiple modules, layers, or require phased execution.

---

## Procedure

Follow these sequential steps to prepare the implementation plan:

### 1. Review Previous Analysis & Requirements

- Review any previous Software Impact Analysis or requirements documents.
- Understand the scope, success criteria, and given constraints (e.g., performance, security, technology stack).

### 2. Verify Current Codebase State

- **Never guess or make assumptions** about the current state of the codebase.
- Explore the codebase if not already thoroughly done in the previous analysis phase. Verify directory structures, configuration files, APIs, database models, and existing components to confirm alignment with your proposed changes.

### 3. Clarify Uncertainties

- Identify any ambiguities, conflicting requirements, or gaps in information.
- Formulate clear questions to clarify these items with the user. Propose concrete options/alternatives when asking, but always allow the user to provide custom text input.

### 4. Confirm Architecture & Solution Design

- **Propose Solution Design if Missing:** If the input documents (such as requirements or impact analysis) do not include a concrete direction for the solution design:
  - Actively formulate and propose a solution design based on all available codebase information, requirements, and constraints.
  - Explain the architectural rationale, highlighting any trade-offs made or alternative designs considered.
  - Address performance, security, scaling, and integration impacts of the proposed design.
- **Get User Approval:** Propose the design/approach clearly and **stop to ask the user to confirm it** before proceeding to write the detailed implementation plan.

### 5. Evaluate Phasing & Slicing

- Assess whether the implementation can be divided into smaller, reviewable increments.
- Aim for **phased implementation plans** to produce small, cohesive increments, ideally where each phase can be delivered as a single commit.
- **Tests Are Part of Every Increment:** Each increment (task or phase) must include writing or updating its tests. Schedule tests as early as possible — a test should only be deferred if it genuinely cannot run until later changes land (e.g., some end-to-end or integration tests). Do not batch test writing into a final phase.
- **First Define Vertical Slices:** Start by defining vertical slices of functionality (e.g., end-to-end flow for a subset of requirements); if vertical slices are not feasible, consider splitting by deliverable component.
- **Then Split by Deliverable Component:** Further split tasks by deliverable component (e.g., API service, user interface). Use this only as a secondary strategy when vertical slices are not feasible.
- **Phasing Structure:**
  - If phasing is needed: Create a main plan file acting as an index with succinct descriptions of each phase and links to the phase files. Each phase must go in a separate file.
    - **Per-Phase Overview:** Each phase plan file must include an **Overview** section at the very top. This section provides minimal context to place the phase's work in the big picture (e.g., what the phase accomplishes, how it connects to the overall feature, and which subsequent phases or components it prepares).
  - If the changes are simple and self-contained: Do not phase; document all tasks in a single plan file.

### 6. Write Implementation Tasks

- Define the specific tasks required to complete the implementation.
- Use **imperative titles** (e.g., "Create database migration for user table").
- Specify the **affected components and files** (including exact paths where possible).
- Reference **code symbols** (classes, methods, interfaces, schemas) when applicable.
- Include a concise description of the implementation details.
- Provide clear, testable **Acceptance Criteria** for each task.
- **Plan Tests Alongside Code:** Write or update tests as an explicit, early task in the plan, not a final step. In the task list, pair each feature/change task with the test task that verifies it (or include test updates directly within the same task). Only push tests later when they unavoidably depend on changes introduced by a subsequent step — for example, unit tests for a new module can run immediately, while full integration/E2E tests may need the module wired end-to-end first. Call out such dependencies explicitly in the task's **Prerequisites / Dependencies**.

---

## Output Format

The final deliverable of this skill is an Implementation Plan (or an index plan linking to individual phase plans).

### Template

Load the report template from [`assets/report-template.md`](assets/report-template.md) **only when you are ready to write the final deliverable**, then fill it in as described below:

1. Fill out all sections of the template based on the requirements and codebase verification.
2. If phasing is used, create separate markdown files for each phase. Each phase file should follow this structure:
   - **Overview:** A concise section at the top providing minimal context to place the phase work in the big picture (e.g., how it integrates, what it unlocks, and the overall goals of this phase).
   - **Task Details:** The specific tasks scheduled for this phase, using the same task format as the main template (imperative title, affected files/symbols, description, and acceptance criteria). Include the test-writing/update tasks in the phase with the code they verify — never defer all tests to a later phase.
   - **Verification Plan:** Step-by-step instructions for verifying this phase's incremental changes.
3. Save the plan(s) or present them to the user for final approval.

### Self-Check (before delivering)

Before presenting the plan:

- Verify that every affected file and code symbol listed per task actually resolves on disk in the current codebase. Re-check any path or symbol that was assumed rather than confirmed.
- Verify each acceptance criterion is objectively testable — it names a specific, observable outcome (e.g., "returns 401 for invalid tokens") rather than vague wording ("works correctly", "handles errors").
- Verify each phase corresponds to a single reviewable commit; if a task is too large to review independently, split it.
- Confirm the phase index links (if used) reference real plan files with valid relative paths and that each phase file has an `Overview` section at the top.
- Verify the tests are planned as early as possible: every feature/change task has its test task in the same or an earlier phase, unless that test genuinely requires later changes. Flag and justify any test that is deferred to the end of the plan.

Fix any issues and repeat until the plan passes all checks before finalizing.

---

## Gotchas

Agent corrections worth remembering on every run:

- **Never guess the current state of the codebase.** If symbol or file references were verified in the impact-analysis phase, do not assume they are still valid — confirm affected paths exist on disk before baking them into the plan.
- **Acceptance criteria must be objectively testable.** Replace vague wording ("works correctly", "handles errors") with specific observable outcomes.
- **A single phase should correspond to a single reviewable commit.** If a task is too large to review independently, split it rather than merging giant, monolithic phases.
- **Write tests as early as possible.** Never schedule all test writing at the end of the plan. Defer a test only when it requires changes that land later (e.g., integration/E2E tests that need the full flow wired up); state that dependency explicitly in the affected task. A plan that leaves testing to a final phase has failed review.
