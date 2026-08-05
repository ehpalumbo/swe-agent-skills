# Implementation Plan: [Feature/Requirement Title or ID]

## Metadata

- **Author:** [Agent Name / ID]
- **Date:** [YYYY-MM-DD]
- **Phased Implementation:** [Yes / No]

### References [Links to supporting documents / analysis reports]

- e.g., [Supporting Document / Analysis Report Title](path/to/document_name)

---

## Executive Summary & Architecture

[Provide a summary of the proposed changes, the confirmed architecture, and any system constraints.]

---

<!-- If Phased Implementation is Yes, include the Phase Index below. Otherwise, remove it. -->
## Phase Index

- **[Phase 1: Title]** - [Succinct description] - [Link to phase_1.md](phases/phase_1_plan.md)
- **[Phase 2: Title]** - [Succinct description] - [Link to phase_2.md](phases/phase_2_plan.md)

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
  - [file_basename](relative/path/to/affected_file)
- **Affected Symbols:**
  - `ClassName` or `method_name` or `database_table`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Criterion 1, e.g., Middleware returns 401 Unauthorized for invalid tokens]
- [ ] [Criterion 2, e.g., Middleware attaches user object to request context]

#### 2. [Imperative Task Title]

- **Prerequisites / Dependencies:** [e.g., None]
- **Affected Files:**
  - [file_basename](relative/file/path)
- **Affected Symbols:**
  - `SymbolName`
- **Description:** [Concise description of what to do and how to implement it.]
- **Acceptance Criteria:**
  - [ ] [Criterion 1]

---

## Verification Plan (Whole Feature Verification)
<!-- Note: This verification plan is for checking the whole feature. Every individual task in the plan must also be verified by checking that it meets its specific acceptance criteria. -->

### Automated Tests

[Describe unit, integration, or E2E tests to create/update]

### Manual Verification Steps

1. [Step 1, e.g., Start local environment and login]
2. [Step 2, e.g., Verify network payload contains new fields]

### Definition of Done (DoD)

- [ ] Code is formatted and linted (no warnings/errors)
- [ ] All automated unit & integration tests pass successfully
- [ ] No regression introduced to existing components
- [ ] Documentation is updated if applicable
