---
description: Full specification rubric and checklist for validating spec completeness
---

# Speckit Rubric: Full Spec Quality Standards

This document defines what makes a spec complete and actionable. Use this rubric to evaluate any specification before implementation begins.

## Required Sections (Non-negotiable)

Every spec MUST have all of these sections with meaningful content:

### 1. Overview
- **Purpose**: Problem statement, solution summary, expected outcomes, why this matters now
- **Length**: 3-8 sentences minimum
- **Check**: Can you explain what problem this solves in one sentence?

### 2. User Stories + Acceptance Criteria
- **Requirement**: At least 1 user story with testable acceptance criteria
- **Format**: 
  ```
  Story: [As a user], [I want to], [so that]
  AC-1: [Testable condition]
  AC-2: [Testable condition]
  ```
- **Check**: Each MUST requirement should have corresponding AC

### 3. Functional Requirements
- **Requirement**: At least 1 MUST requirement per RFC 2119
- **Format**:
  ```
  R1 (MUST): [Requirement description]
  R2 (SHOULD): [Requirement description]
  R3 (MAY): [Requirement description]
  ```
- **Check**: Are all MUST requirements traceable to acceptance criteria?

### 4. Non-Functional Requirements
- **Requirement**: At least 1 non-functional requirement
- **Categories**: Performance, Security, Accessibility, Reliability, Scalability
- **Check**: Are SLAs/SLOs defined with specific targets?

### 5. Dependencies
- **Requirement**: Explicit list of external systems, libraries, services
- **Format**: List with version constraints where applicable
- **Check**: Are failure behaviors defined when dependencies fail?

### 6. Out of Scope
- **Requirement**: At least 1 item explicitly excluded
- **Purpose**: Clarifies boundaries and prevents scope creep
- **Check**: Is there a "what this is NOT" statement?

---

## Recommended Sections (Validation warns if missing)

These sections are highly recommended for production-quality specs:

### 7. Data Model
- Schema definitions for entities
- Entity relationships
- State management approach

### 8. API Endpoints
- Request/response contracts
- HTTP methods and status codes
- Error response formats

### 9. UI/UX Guidelines (if user-facing)
- User flows
- Component specifications
- Accessibility requirements

---

## Minimum Content Thresholds

| Section | Minimum Threshold | Validation Method |
|---------|------------------|-------------------|
| Overview | >= 3 sentences | Count sentences |
| User Stories | >= 1 story with >= 2 ACs each | Count "Story:" and "AC-" |
| Functional Requirements | >= 1 MUST requirement | Count "MUST" keywords |
| Non-Functional | >= 1 (performance/security/accessibility) | Search keywords |
| Dependencies | Section exists (content optional) | Check H2 exists |
| Out of Scope | >= 1 bullet point | Count list items |

---

## Quick Checklist (Use Before Implementation)

Before proceeding to planning/implementation, verify:

- [ ] **Overview**: Can state problem in 1 sentence, 3-8 sentences total
- [ ] **User Stories**: At least 1 story with clear actor, action, benefit
- [ ] **Acceptance Criteria**: At least 2 testable criteria (AC-1, AC-2 format)
- [ ] **Functional Requirements**: At least 1 MUST requirement defined
- [ ] **Non-Functional**: At least 1 of performance/security/accessibility
- [ ] **Dependencies**: External systems listed with version constraints
- [ ] **Out of Scope**: At least 1 explicit non-goal defined

---

## Deep Review Questions

For thorough spec review, also verify:

### Completeness
- [ ] Are all inputs defined with schemas?
- [ ] Are all outputs defined with schemas?
- [ ] Are preconditions documented?
- [ ] Are postconditions (success/failure/cancel) documented?

### Traceability
- [ ] Does each MUST requirement link to an AC?
- [ ] Does each user story have ACs?
- [ ] Can every requirement be tested?

### Operational Readiness
- [ ] Is error handling defined (retriable vs non-retriable)?
- [ ] Is retry policy specified?
- [ ] Are secrets handling requirements clear?
- [ ] Is observability defined (metrics, logging, tracing)?
- [ ] Is rollback procedure documented?

### Security
- [ ] Is identity/permissions model defined?
- [ ] Is threat model or security approach stated?
- [ ] Are secrets handled externally (no hardcoded secrets)?

---

## Spec Quality Score

Rate the spec on these dimensions (1-5):

| Dimension | Score (1-5) | Notes |
|-----------|-------------|-------|
| Problem Clarity | | |
| Solution Completeness | | |
| Requirements Testability | | |
| Operational Clarity | | |
| Security Thoroughness | | |
| Traceability | | |

**Minimum passing score**: 20/30

## Examples (Pass/Fail Signals)

### Pass Example (Overview)
"This workflow builds, tests, and deploys services on every merge. It reduces manual releases, improves reliability through automated gates, and targets 99.5% success over 30 days. It is needed now to support increased release cadence and reduce incident rate."

### Fail Example (Overview)
"We need a better pipeline." (Too short, lacks outcomes and context.)

### Pass Example (Acceptance Criteria)
- AC-1: Invalid input fails schema validation with error message
- AC-2: Successful run emits `workflow_completed` log and metric

### Fail Example (Acceptance Criteria)
- AC-1: It works (not testable)

---

## References

- RFC 2119 (MUST/SHOULD/MAY): https://datatracker.ietf.org/doc/html/rfc2119
- JSON Schema: https://json-schema.org/
- speckit-lint.md: Automated validation tool
- speckit-review.md: Human review checklist
