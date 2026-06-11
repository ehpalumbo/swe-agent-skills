---
name: software-impact-analysis
description: Performs a comprehensive Software Impact Analysis for a feature request or bug fix on an existing codebase. Helps identify affected components, explore solution alternatives, evaluate risks, ask clarifying questions, and document the recommended way forward.
license: Apache-2.0
metadata:
  author: ehpalumbo
  version: "1.0.0"
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

- Search the codebase using grep, find, or symbol searches for related logic, utilities, or configurations.
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

```markdown
# Software Impact Analysis: [Feature Request / Bug Fix Title or ID]

## Metadata
- **Author:** [Agent Name / ID]
- **Date:** [YYYY-MM-DD]
- **Target Branch / Environment:** [e.g., main, staging]
- **Overall Risk Level:** [Low / Medium / High]
  - *Low:* Isolated changes, low regression risk, minimal dependencies.
  - *Medium:* Affects multiple components or modules, moderate regression risk.
  - *High:* Core systems, breaking changes, data migration, or high risk of regression.

---

## Executive Summary
[A concise, one-paragraph summary of the proposed changes, the recommended approach, and any critical risks or dependencies identified.]

---

## 1. Requirement & Problem Space Analysis
- **Objectives:** [What is the goal of this change?]
- **Success Criteria:** [How do we know the change is successful?]
- **Key Constraints:** [Any architectural, performance, security, or business constraints?]

---

## 2. Context & Findings
[Summarize what was discovered during codebase exploration, documentation review, or external spec analysis. Note any existing patterns, helper modules, or libraries that should be reused.]

---

## 3. Affected Components
Detail the specific codebase components that are expected to be modified, created, or deleted.

| File / Component Path | Action | Description of Change |
| :--- | :--- | :--- |
| `[file/path]` | [NEW / MODIFY / DELETE] | [Brief description of what changes] |
| `[file/path]` | [NEW / MODIFY / DELETE] | [Brief description of what changes] |

### Database & Schema Changes
- [State if any database migrations, schema updates, or new tables are required. If none, skip this section.]

### API & Contract Changes
- [State if any public or internal API endpoints, payloads, or contracts are modified or added. If none, skip this section.]

### Backward Compatibility & Migration Strategy
- [Assess if changes are backward-compatible. Explain how existing data, clients, or APIs will be handled during rollout. If none, skip this section.]

### Rollback Plan
- [Outline the concrete steps to revert changes in production if a critical issue is discovered post-deployment (e.g., SQL migration rollback, feature flag toggle). If none, skip this section.]

---

## 4. Proposed Solution Approaches
Evaluate the possible approaches to address the feature request or bug fix.

### Approach A: [Name of Approach A, e.g., Direct Implementation]
- **Description:** [How it works]
- **Pros:**
  - [Pro 1]
  - [Pro 2]
- **Cons:**
  - [Con 1]
  - [Con 2]
- **Risk Level & Complexity:** [Low / Medium / High - explain why]

### Approach B: [Name of Approach B, e.g., Refactoring First] (If applicable)
- **Description:** [How it works]
- **Pros:**
  - [Pro 1]
- **Cons:**
  - [Con 1]
- **Risk Level & Complexity:** [Low / Medium / High - explain why]

---

## 5. Recommended Way Forward & Design Decisions
- **Chosen Approach:** [Approach A / Approach B]
- **Rationale:** [Why is this the best path forward? Address trade-offs, scalability, and maintenance.]
- **Security & Performance Impact Evaluation:**
  - **Security:** [Assess vulnerability vectors, data exposure risks, authentication/authorization changes, or new third-party dependencies.]
  - **Performance:** [Assess potential database query latency, resource overhead, caching changes, or scaling issues.]
- **Clarifying Questions Resolved:**
  - [List any questions asked to the user and their responses that helped shape this decision.]
- **Remaining Open Questions:**
  - [List any questions still outstanding that need user input before execution.]

---

## 6. Verification & Testing Plan
How the implementation will be verified to prevent regressions and ensure correctness.

### Automated Tests
- **Existing Tests to Run:** [e.g., `npm run test`, `mvn verify`]
- **New Tests to Add:** [Describe unit/integration tests that need to be written]

### Manual Verification Steps
1. [Step 1, e.g., Start local server and navigate to /endpoint]
2. [Step 2, e.g., Verify visual alignment/behavior]

### Potential Regression Risks
- [Describe any potential side effects or areas of the application that could be inadvertently affected and how to monitor/test them.]
```

1. Fill out all sections of the template based on your findings and analysis.
2. Save or present the report to the user as requested.
