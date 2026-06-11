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
- **Define Vertical Slices:** Start by defining vertical slices of functionality (e.g., end-to-end flow for a subset of requirements) if applicable.
- **Split by Deliverable Component:** Further split tasks by deliverable component (e.g., API service, user interface).
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

---

## Output Format

The final deliverable of this skill must be an Implementation Plan (or an index plan linking to individual phase plans).

### Template

```markdown
# Implementation Plan: [Feature/Requirement Title or ID]

## Metadata
- **Author:** [Agent Name / ID]
- **Date:** [YYYY-MM-DD]
- **Phased Implementation:** [Yes / No]

### References [Links to supporting documents / analysis reports]
- e.g., [Supporting Document / Analysis Report Title](path/to/document_name)

---

## Executive Summary & Architecture
[Provide a brief, one-paragraph summary of the proposed changes, the confirmed architecture, and any system constraints.]

---

<!-- If Phased Implementation is Yes, include the Phase Index below. Otherwise, remove it. -->
## Phase Index
- **[Phase 1: Title]** - [Succinct description] - [Link to phase_1.md](file:///path/to/phase_1_plan.md)
- **[Phase 2: Title]** - [Succinct description] - [Link to phase_2.md](file:///path/to/phase_2_plan.md)

---

## Configuration & Environment Updates
- **Environment Variables:** [State new/modified .env variables, e.g., API_KEY, DB_PASSWORD. If none, write "None".]
- **Feature Flags:** [State if any feature toggles are required to gate the changes. If none, write "None".]
- **External Dependencies:** [List any new npm packages, pip packages, or library upgrades needed. If none, write "None".]

---

## Task Details
<!-- If Phased, this section is represented in each phase's separate file. If Not Phased, list the tasks here. -->

### [Component / Layer Name, e.g., Database, Backend, Frontend]

#### 1. [Imperative Task Title, e.g., Implement authentication middleware]
- **Prerequisites / Dependencies:** [e.g., None, or "Database migration in Task X"]
- **Affected Files:**
  - [file_basename](file:///path/to/affected_file)
- **Affected Symbols:**
  - `ClassName` or `method_name` or `database_table`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Criterion 1, e.g., Middleware returns 401 Unauthorized for invalid tokens]
  - [ ] [Criterion 2, e.g., Middleware attaches user object to request context]

#### 2. [Imperative Task Title]
- **Prerequisites / Dependencies:** [e.g., None]
- **Affected Files:**
  - [file_basename](file:///path/to/affected_file)
- **Affected Symbols:**
  - `SymbolName`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Criterion 1]

---

## Verification Plan (Whole Feature Verification)
<!-- Note: This verification plan is for checking the whole feature. Every individual task must also be verified by checking that it meets its specific acceptance criteria. -->

### Automated Tests
- **Test Commands:** [e.g., `npm run test`, `pytest path/to/tests`]
- **New Tests to Add:** [Describe new unit, integration, or E2E tests to write]

### Manual Verification Steps
1. [Step 1, e.g., Start local environment and login]
2. [Step 2, e.g., Verify network payload contains new fields]

### Definition of Done (DoD)
- [ ] Code is formatted and linted (no warnings/errors)
- [ ] All automated unit & integration tests pass successfully
- [ ] No regression introduced to existing components
- [ ] Documentation / README is updated if applicable
```

1. Fill out all sections of the template based on the requirements and codebase verification.
2. If phasing is used, create separate markdown files for each phase. Each phase file should follow this structure:
   - **Overview:** A concise section at the top providing minimal context to place the phase work in the big picture (e.g., how it integrates, what it unlocks, and the overall goals of this phase).
   - **Task Details:** The specific tasks scheduled for this phase, using the same task format as the main template (imperative title, affected files/symbols, description, and acceptance criteria).
   - **Verification Plan:** Step-by-step instructions for verifying this phase's incremental changes.
3. Save the plan(s) or present them to the user for final approval.
