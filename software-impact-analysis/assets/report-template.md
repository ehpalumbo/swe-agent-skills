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

[Describe the problem statement providing sufficient background information for readers to understand the proposal. Summarize what was discovered during codebase exploration, documentation review, or external spec analysis. Note any existing patterns, helper modules, or libraries that should be reused. Include references to any material used during the analysis.]

---

## 3. Proposed Solution Approaches

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

## 4. Recommended Way Forward & Design Decisions

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

## 5. Affected Components

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

## 6. Verification & Testing Plan

[How the implementation will be verified to prevent regressions and ensure correctness.]

### Automated Tests

- **Existing Tests to Run:** [e.g., `npm run test`, `mvn verify`]
- **New Tests to Add:** [Describe unit/integration tests that need to be written]

### Manual Verification Steps

1. [Step 1, e.g., Start local server and navigate to /endpoint]
2. [Step 2, e.g., Verify visual alignment/behavior]

### Potential Regression Risks

- [Describe any potential side effects or areas of the application that could be inadvertently affected and how to monitor/test them.]
