---
description: Run the full automated spec-driven development workflow with best-in-class specifications
agent: build
---

# Speckit Autopilot Command

You are running the automated spec-driven development workflow. This will guide through the complete process from concept to implementation using best-in-class specification standards.

## Phase 1: Check Prerequisites

First, let's verify the environment:

```bash
# Check project structure
ls -la .specify/ 2>/dev/null || echo "No .specify directory"

# Check git status
git status --short 2>/dev/null || echo "Not a git repository"

# Check if in a project directory
ls -la 2>/dev/null | head -5
```

## Phase 2: Constitution (Automated)

### Step 2.1: Check Existing Constitution

```bash
cat .specify/memory/constitution.md 2>/dev/null && echo "Constitution exists" || echo "Need to create constitution"
```

### Step 2.2: Create Constitution

If no constitution exists, create one with best-in-class default principles:

```markdown
# Constitution

## Overview
This project uses spec-driven development for all workflows and automations. Specs are operational contracts that drive validation, implementation, and operations.

## Core Principles

### 1. Spec as Contract
- **Standard**: Every workflow MUST have a spec that defines inputs, outputs, behavior, and operational boundaries
- **Rationale**: Ambiguous scopes and underspecified I/O contracts are primary causes of production failures

### 2. Validation Over Code Review
- **Standard**: Specs must be validated automatically; conformance tests must verify acceptance criteria
- **Rationale**: Human review cannot catch all edge cases; automated validation is reproducible and scalable

### 3. Required Specification Sections
All specs MUST include: Overview, Purpose/Scope, Audience/Roles, Goals, Non-goals, Requirements (MUST/SHOULD/MAY), Architecture, Data Flows, Inputs/Outputs (JSON Schema), Preconditions/Postconditions, Workflow Steps, Error Handling, Retries, Idempotency, Security, Secrets Handling, Observability, SLAs/SLOs, Rollout/Rollback, Testing, Acceptance Criteria

### 4. RFC 2119 Keyword Semantics
- **MUST**: Required, no deviation
- **SHOULD**: Strong recommendation, deviation requires justification
- **MAY**: Optional, implementation-dependent

### 5. Schema Validation
- **Standard**: Specs MUST validate against JSON Schema (Draft 2020-12)
- **Rationale**: Catch structural errors at PR time

### 6. Secrets Discipline
- **Standard**: No secrets in code, logs, or spec artifacts
- **Standard**: Secrets MUST come from external secrets management (Vault, AWS Secrets Manager, K8s secrets)

### 7. Observability as Design Requirement
- **Standard**: Every workflow MUST define metrics, logging, and correlation IDs
- **Standard**: SLAs/SLOs MUST be explicit with alerting policy

### 8. Idempotency Requirements
- **Standard**: Steps with side effects MUST declare idempotency keys
- **Standard**: Retry policies MUST be bounded with explicit backoff

### 9. Rollout Governance
- **Standard**: All production workflows MUST have documented rollback procedures
- **Standard**: Operational sign-off REQUIRED before production rollout

### 10. Versioning Discipline
- **Standard**: Specs use SemVer; breaking changes require version bump

---

**Version**: 1.0.0
**Ratification Date**: $(date +%Y-%m-%d)
**Last Amended**: $(date +%Y-%m-%d)
```

## Phase 3: Specification (Automated)

### Step 3.1: Gather Workflow Requirements

Ask the user:
> **What workflow/pipeline/automation do you want to build?** Please describe it in detail.

Collect:
- Workflow name and purpose
- Trigger events (manual, scheduled, webhook, etc.)
- Steps involved
- Inputs and expected outputs
- Error handling requirements
- Security/secrets needs
- Observability requirements
- Deployment targets

### Step 3.2: Create Feature Directory

```bash
FEATURE_NAME="[FEATURE-NAME]"
mkdir -p ".specify/specs/$FEATURE_NAME"
```

### Step 3.3: Generate Best-in-Class Specification

Create `.specify/specs/[FEATURE_NAME]/spec.md` with this template:

```markdown
# <Workflow Name> Spec

## Overview
[Problem, solution, outcomes, and why now. 3-8 sentences.]

## Purpose and scope
- **In scope**: [What this spec governs]
- **Out of scope**: [What it explicitly excludes]
- **Environments / blast radius**: [Where it runs, what's affected]

## Audience and roles
- **Authors**:]
- **Reviewers**: [Who [Who defines intent validates design]
- **Implementers**: [Who builds]
- **Operators**: [Who runs/monitors]
- **Escalation path**: [How issues are escalated]

## Goals
- G1: [Measurable outcome with success metrics]
- G2: ...

## Non-goals
- NG1: [Explicit behaviors not supported]
- NG2: ...

## Requirements and traceability
- R1 (MUST): [Requirement] (Ticket: <ID>) (Acceptance: AC-1)
- R2 (SHOULD): [Requirement] (Ticket: <ID>) (Acceptance: AC-2)

## Architecture
- **Components**: [Component diagram + responsibilities]
- **Deployment model**: [Where it runs]
- **Trust boundaries**: [Identity model, permissions]

## Data flows
- **Parameters vs artifacts vs external stores**: [How data moves]
- **Sensitivity classification**: [Public/internal/confidential]

## Inputs and outputs
### Inputs
- Input schema (JSON Schema Draft 2020-12):
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "object",
  "required": ["field1"],
  "properties": {
    "field1": { "type": "string", "description": "..." }
  }
}
```

### Step 3.3b: Parallel Section Drafting (Subagents)

After the template skeleton exists, split section drafting across subagents, then merge into `spec.md` + `sections/` files.

Suggested splits:
- Subagent A: Overview, Purpose and scope, Goals, Non-goals
- Subagent B: Architecture, Data flows, Inputs/Outputs
- Subagent C: Workflow steps, Error handling, Idempotency
- Subagent D: Security, Secrets handling, Observability, SLAs/SLOs
- Subagent E: Testing strategy, Rollout/Rollback, Versioning, Acceptance criteria, Examples

Rules:
- All sections must follow the template headings exactly
- Keep RFC 2119 language for requirements
- Merge by section; do not overwrite other sections
- Example input: ...

### Outputs
- Output schema: ...
- Example output: ...

## Preconditions
- [ ] Required secrets exist in configured secrets backend
- [ ] Executor identity has least-privilege permissions
- [ ] Dependencies reachable
- [ ] Migrations applied (if applicable)

## Postconditions
### On Success
- [Guaranteed state after successful completion]

### On Failure
- [Guaranteed state after failure]

### On Cancel
- [Cleanup responsibilities, partial state]

## Workflow steps
### Step Graph
[Diagram showing step ordering/DAG]

### Step Definitions

#### Step 1: [step-name]
- **Responsibilities**: [What this step does]
- **Inputs/Outputs**: [What it consumes/produces]
- **Timeout**: [seconds/minutes]
- **Retries**: Max attempts: [n], Backoff: [none/fixed/exponential]
- **Idempotency**: Key pattern: [key pattern]
- **Secrets used**: [what secrets this step needs]
- **Observability**: Events: [step_started, step_completed, step_failed]

## Error handling
### Failure Taxonomy
- **Retriable errors**: [list with retry policy]
- **Non-retriable errors**: [list that causes workflow failure]
- **Human-notification triggers**: [when to page]

### Retry Policy Summary
| Step | Max Attempts | Backoff | Timeout |
|------|-------------|---------|---------|
| step1 | 2 | exponential | 300s |

## Idempotency
- **Idempotency keys**: [pattern for each step with side effects]
- **Deduplication strategy**: [how duplicates are handled]

## Security
### Identity Model
- **Service account**: [name]
- **Roles/permissions**: [RBAC policy]

### Threat Model Summary
- [Key threats and mitigations]

## Secrets handling
- **Sources**: [where secrets come from]
- **Injection method**: [env vars, volume mounts]
- **Rotation**: [how often, process]
- **Invariant**: NO secrets in code, logs, or spec artifacts

## Observability
### Logs
- **Format**: [json/structured]
- **Correlation IDs**: [run_id, step_id, spec_version]

### Metrics (SLIs)
| Metric | Type | Labels |
|--------|------|--------|
| workflow_duration_seconds | histogram | workflow_name, status |
| workflow_success_ratio | gauge | workflow_name |

### Traces
- **Trace ID**: [run_id]
- **Span hierarchy**: [workflow → step → activity]

## SLAs and SLOs
- **SLO name**: [e.g., prod_deploy_success]
- **Target**: [e.g., 0.995]
- **Window**: [e.g., 30d]

## Monitoring and alerting
### Alerts
| Alert | Condition | Severity | Paging |
|-------|-----------|----------|--------|
| SLO burn rate | burn rate > 10 for 1h | critical | true |

- **Runbook**: [link or content]
- **Ownership**: [team/on-call]

## Performance and scalability
- **Expected load**: [concurrent runs, frequency]
- **Resource sizing**: [CPU/memory per step]
- **Concurrency limits**: [max parallel]

## Testing strategy
- **Unit tests**: [coverage scope]
- **Integration tests**: [external dependencies]
- **End-to-end tests**: [real execution paths]
- **Chaos/failure injection**: [top risks to test]

## Rollout and rollback
### Rollout Plan
- **Strategy**: [e.g., progressive, canary, blue-green]
- **Stages**: [staging → canary → production]

### Rollback Triggers
- [Conditions that trigger rollback]

### Rollback Steps
- [How to revert]

## Versioning
- **Spec version**: [e.g., 1.0.0]
- **Changelog**:
  - v1.0.0 (2026-03-02): Initial version

## Acceptance criteria
- AC-1 (MUST): [Testable condition]
- AC-2 (SHOULD): [Testable condition]

## Examples
### Golden Path Example
[Complete example with inputs, expected outputs]

### Failure Path Example
[Example showing error handling behavior]
```

### Step 3.4: Spec Lint Validation

After generating spec, run lint validation to ensure all required sections are present:

```bash
# Run spec lint validation
/speckit.lint [FEATURE_NAME]
```

Or manually verify using the rubric:
- [ ] Overview: >= 3 sentences
- [ ] User Stories: >= 1 story with >= 2 acceptance criteria
- [ ] Functional Requirements: >= 1 MUST requirement
- [ ] Non-Functional: >= 1 (performance/security/accessibility)
- [ ] Dependencies: Section exists
- [ ] Out of Scope: >= 1 item

**Validation rules:**
- If **FAIL**: Go back to Step 3.3, fix missing sections
- If **WARN**: Review warnings, decide if acceptable to proceed
- If **PASS**: Proceed to Step 3.5

### Step 3.5: Human Review (5 min)

Run the review checklist to catch issues automated tools miss:

```bash
# Run human review checklist
/speckit.review [FEATURE_NAME]
```

Or manually complete the 5-minute checklist from `speckit-review.md`:

**Key questions to answer:**
1. Can you state the core problem in one sentence?
2. Are there clear success metrics?
3. Does each MUST requirement have acceptance criteria?
4. Is out-of-scope defined?
5. Are retriable vs non-retriable errors defined?
6. Is the threat model stated?
7. Are dependencies and their failure behaviors clear?
8. Is rollback procedure documented?

**Review outcome:**
- If **Needs Refinement**: Return to Step 3.3
- If **Ready**: Proceed to Phase 4

### Step 3.5b: Parallel Review Support (Subagents)

Run `/speckit.lint` and the human review checklist in parallel. If lint FAIL or review says Needs Refinement, reconcile and loop back to Step 3.3.

## Phase 4: Planning (Automated)

### Step 4.1: Ask Executor/Platform

Ask the user:
> **What workflow executor/platform should we use?** (e.g., Argo Workflows, Tekton, Airflow, Temporal, GitHub Actions, GitLab CI, etc.)

If no preference, ask about:
- Target infrastructure (K8s, cloud, self-hosted)
- Specific security/observability requirements

### Step 4.2: Generate Plan

Create `.specify/specs/[FEATURE_NAME]/plan.md`:

```markdown
# Implementation Plan: [Feature Name]

## Executor Selection
- **Platform**: [Argo Workflows / Tekton / Airflow / Temporal / GitHub Actions / other]
- **Rationale**: [Why this executor fits the spec requirements]

## Executor-Specific Configuration
### Workflow/Pipeline Definition
[Code-defined or YAML-based workflow definition]

### Step-to-Executor Mapping
| Spec Step | Executor Step | Type |
|-----------|--------------|------|
| step1 | build | container |
| step2 | test | container |

## Input/Output Binding
- How parameters/artifacts are passed to workflow
- Schema validation approach

## Retry & Timeout Implementation
```yaml
steps:
  - id: build
    retries:
      limit: 2
      backoff:
        strategy: exponential
        duration: 10s
    timeout: 1800s
```

### Step 4.2b: Parallel Plan Drafting (Subagents)

Split plan sections across subagents, then merge into a single `plan.md`:
- Subagent A: Executor selection, step mapping, input/output binding
- Subagent B: Retry/timeout, idempotency, artifact/state management
- Subagent C: Security, secrets, compliance mapping
- Subagent D: Observability, alerting, testing implementation
- Subagent E: Phases, risks/mitigations, research notes, tech stack

Merge rules:
- Keep headings as in template
- Resolve duplicates by choosing the most specific, spec-aligned content

## Idempotency Implementation
- How idempotency keys are generated
- Where they're stored/checked

## Security Implementation
- Service account / IAM role
- Secrets backend integration

## Observability Implementation
- Custom metrics definition
- Structured logging with correlation IDs
- Alert rules

## Testing Implementation
- Unit tests framework
- Integration tests (mocking)
- E2E tests
- Chaos tests

## Rollout & Rollback
- Deployment strategy (canary/blue-green/progressive)
- Automatic/manual rollback triggers

## Implementation Phases

### Phase 1: Foundation
- [ ] Executor setup / credentials
- [ ] Base workflow structure
- [ ] Secrets integration
- [ ] Basic observability

### Phase 2: Core Steps
- [ ] Implement each step per spec
- [ ] Configure timeouts/retry
- [ ] Add idempotency

### Phase 3: Validation
- [ ] Schema validation in CI
- [ ] Unit tests
- [ ] Integration tests
- [ ] E2E tests

### Phase 4: Observability & Operations
- [ ] Metrics dashboards
- [ ] Alert rules
- [ ] Runbooks

### Phase 5: Rollout
- [ ] Staging deployment
- [ ] Canary/progressive rollout
- [ ] Production deployment
```

## Phase 5: Task Generation (Automated)

### Step 5.1: Generate Tasks

Create `.specify/specs/[FEATURE_NAME]/tasks.md`:

```markdown
# Tasks: [Feature Name]

## Task Breakdown

### Phase 1: Foundation

#### Task 1.1: Executor Setup
**Description**: Configure workflow executor and credentials
**Acceptance Criteria**: AC-1, AC-2
**Dependencies**: None
**Verification**: Executor can authenticate and create test workflow

#### Task 1.2: Secrets Integration
**Description**: Configure secrets backend and secret injection
**Acceptance Criteria**: Secrets accessible at runtime, not in logs
**Dependencies**: Task 1.1
**Verification**: Workflow runs with secrets loaded

#### Task 1.3: Observability Setup
**Description**: Configure metrics, logging, and tracing
**Acceptance Criteria**: Metrics visible, logs structured with correlation IDs
**Dependencies**: Task 1.1
**Verification**: Dashboard shows workflow metrics

### Phase 2: Core Implementation

#### Task 2.1: [Step Name] Implementation
**Description**: Implement step logic per spec
**Acceptance Criteria**: AC-1, AC-2
**Dependencies**: Task 1.1
**Verification**: Step executes successfully with correct outputs

#### Task 2.2: Retry & Timeout Configuration
**Description**: Configure per-step retry policies and timeouts
**Acceptance Criteria**: R1 (MUST) - Retry policy enforced
**Dependencies**: Task 2.1
**Verification**: Simulated failure triggers retry

#### Task 2.3: Idempotency Implementation
**Description**: Implement idempotency keys and deduplication
**Acceptance Criteria**: R2 (MUST) - Re-running produces same result
**Dependencies**: Task 2.1
**Verification**: Duplicate run returns same output

### Phase 3: Validation

#### Task 3.1: Schema Validation in CI
**Description**: Add JSON Schema validation to CI pipeline
**Acceptance Criteria**: Invalid specs rejected at PR time
**Dependencies**: Tasks 2.x
**Verification**: PR with invalid spec fails CI

#### Task 3.2: Unit Tests
**Description**: Write unit tests for step logic
**Acceptance Criteria**: [Coverage target]
**Dependencies**: Tasks 2.x
**Verification**: Tests pass

#### Task 3.3: Integration Tests
**Description**: Test external dependencies with mocks
**Acceptance Criteria**: External calls mocked, behavior verified
**Dependencies**: Task 3.2
**Verification**: Integration tests pass

#### Task 3.4: End-to-End Tests
**Description**: Run complete workflow in test environment
**Acceptance Criteria**: All acceptance criteria verified
**Dependencies**: Task 3.3
**Verification**: E2E tests pass

#### Task 3.5: Chaos/Failure Injection Tests
**Description**: Test failure scenarios
**Acceptance Criteria**: System handles failures per error taxonomy
**Dependencies**: Task 3.4
**Verification**: Identified failures handled correctly

### Phase 4: Operational Readiness

#### Task 4.1: Metrics Dashboard
**Description**: Create operational dashboard
**Acceptance Criteria**: Dashboard shows SLIs
**Dependencies**: Task 1.3
**Verification**: Dashboard renders with sample data

#### Task 4.2: Alert Rules
**Description**: Configure alert rules per spec
**Acceptance Criteria**: Alerts fire on defined conditions
**Dependencies**: Task 4.1
**Verification**: Test alert fires correctly

#### Task 4.3: Runbook Documentation
**Description**: Document operational procedures
**Acceptance Criteria**: Common issues documented
**Dependencies**: Task 4.2
**Verification**: Runbook reviewed by operator

#### Task 4.4: Rollback Procedure Test
**Description**: Test rollback procedure in staging
**Acceptance Criteria**: Rollback verified in non-production
**Dependencies**: Task 4.3
**Verification**: Rollback executed successfully

### Phase 5: Deployment

#### Task 5.1: Staging Deployment
**Description**: Deploy to staging environment
**Acceptance Criteria**: All tests pass in staging
**Dependencies**: Tasks 4.x
**Verification**: Workflow runs successfully in staging

#### Task 5.2: Production Rollout
**Description**: Deploy to production with rollout strategy
**Acceptance Criteria**: Production deployment succeeds
**Dependencies**: Task 5.1
**Verification**: Production workflow operational

## Acceptance Criteria Mapping
| Task | Acceptance Criteria |
|------|---------------------|
| 1.1 | - |
| 1.2 | AC-SEC-1 |
| 1.3 | AC-OBS-1, AC-OBS-2 |
| 2.1 | AC-1, AC-2 |
| 2.2 | R1 |
| 2.3 | R2 |
```

### Step 5.1b: Parallel Task Drafting (Subagents)

Draft tasks by phase in parallel, then merge into a single `tasks.md`:
- Subagent A: Phase 1 (foundation)
- Subagent B: Phase 2 (core implementation)
- Subagent C: Phase 3 (validation)
- Subagent D: Phase 4 (operational readiness)
- Subagent E: Phase 5 (deployment)

Merge rules:
- Maintain sequential dependencies
- Ensure each task has file paths, acceptance criteria, and verification
- Validate the dependency graph after merge

## Phase 6: Implementation (Automated)

### Step 6.1: Execute Tasks in Order

For each task, follow this workflow:
1. **Read task details** - Understand what's needed
2. **Implement** - Write the code
3. **Test** - Verify it works
4. **Commit** - Save progress

### Step 6.1b: Parallel Task Execution (Subagents)

If tasks have no dependencies and touch separate files/subsystems, execute them in parallel waves using subagents. Define waves from the dependency graph and only parallelize within a wave.

### Step 6.2: Track Progress

Update `tasks.md` as tasks complete:
```markdown
#### Task X.X: [Name]
- [ ] Not started
- [x] Complete - [Date]: [Verification result]
```

## Phase 7: Completion

When all tasks are complete:
1. Run final validation: schema check + all tests pass
2. Verify functionality end-to-end
3. Confirm observability (metrics, alerts)
4. Verify rollback procedure documented
5. Create summary of what was built

### Step 7b: Spec Verification Loop

After implementation completes, trace spec to code to ensure completeness:

**1. Acceptance Criteria Traceability**
For each ACCEPTANCE CRITERIA (AC-1, AC-2, etc.):
- [ ] Find test(s) that verify this criterion
- [ ] Mark criterion as "validated: [test file:line]"

**2. Requirements Traceability**
For each MUST requirement:
- [ ] Find implementation that satisfies this requirement
- [ ] Mark requirement as "implemented: [file:function]"

**3. User Story Coverage**
For each USER STORY:
- [ ] Verify acceptance criteria cover the story
- [ ] Mark story as "covered by: [AC-1, AC-2]"

**Verification Table:**

| Spec Section | Status | Evidence |
|--------------|--------|----------|
| AC-1 | validated | tests/integration.spec.ts:42 |
| AC-2 | validated | tests/unit/auth.spec.ts:15 |
| R1-MUST | implemented | src/auth/login.ts:23 |
| Security | validated | tests/security.spec.ts |

**If any section is NOT validated/implemented:**
- Create backlog item to address gap
- Do not consider feature complete until full traceability achieved

**Output verification report:**
```markdown
## Spec-to-Code Verification: [FEATURE_NAME]

### Traceability Matrix
| AC | Test File | Line | Status |
|----|-----------|------|--------|
| AC-1 | tests/api.spec.ts | 15 | ✓ validated |
| AC-2 | tests/api.spec.ts | 28 | ✓ validated |

### Coverage Summary
- Acceptance Criteria: X/Y validated
- MUST Requirements: X/Y implemented
- User Stories: X/Y covered

### Gaps (if any)
- [ ] AC-X: No test found
- [ ] R2-MUST: Implementation incomplete
```

## GSD Integration: Execute with GSD

After creating your spec and plan, you can use **GSD (Get Sh*t Done)** for robust phase-based execution with verification and gap closure:

### Why GSD?
- Wave-based parallel execution
- Built-in verification agents
- Gap detection and closure cycle
- State tracking and progress
- Checkpoint handling for human-in-the-loop

### How to Bridge to GSD

After completing specification and planning:

```bash
# Convert spec to GSD phase
/speckit.gsd [feature-name]

# Then execute with GSD
/gsd-execute-phase [feature-name]

# Verify against spec
/gsd-verify-work [feature-name]
```

This will:
1. Read your SPEC.md
2. Create GSD phase plan with tasks from requirements
3. Execute tasks in waves
4. Verify against acceptance criteria
5. Detect gaps and offer closure

### Flow Diagram

```
/speckit.specify     → SPEC.md (Speckit)
        ↓
/speckit.plan        → plan.md (Speckit)
        ↓
/speckit.gsd         → GSD phase (converts spec → GSD)
        ↓
/gsd-execute-phase   → Implementation with verification
        ↓
/gsd-verify-work     → Gap detection & closure
```

## Automation Options

### Option A: Full Autopilot
This runs through all phases automatically with AI generating content.

### Option B: Step-by-Step
Pause after each phase for user confirmation:
- After constitution
- After specification
- After plan
- After tasks
- Before each implementation phase

### Option C: Ask-Then-Generate
AI asks specific questions at each phase, then generates based on answers.

## User Choice

Ask the user:
> **How would you like to proceed?**
> 
> - **A** - Full autopilot (AI generates everything)
> - **B** - Step-by-step (pause at each phase)
> - **C** - Ask-then-generate (AI asks questions, you answer, it generates)

## Output

Provide progress at each phase:
- ✅ Constitution: [Status]
- 📝 Specification: [Status]
- 📋 Plan: [Status]
- 📝 Tasks: [Status]
- 🔨 Implement: [Progress]

## Post-Completion

After all phases complete, provide:
1. Summary of spec created
2. Plan highlights (executor, key decisions)
3. Task count by phase
4. Next steps for the user

## Autopilot Checklist (Prescriptive)

Before moving to the next phase:
- [ ] Spec lint passes (PASS or WARN) and review completed
- [ ] Plan includes retries, idempotency, security, observability
- [ ] Tasks include verification steps and dependencies
- [ ] Implementation produces evidence for each acceptance criterion
