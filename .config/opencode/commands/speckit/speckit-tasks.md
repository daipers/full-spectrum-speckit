---
description: Generate actionable task breakdown from implementation plan with validation and conformance tasks
---

# Tasks Command

You are creating a task breakdown from the implementation plan at `.specify/specs/[FEATURE_NAME]/tasks.md`.

**Philosophy**: Tasks must include not just implementation steps but also validation, conformance testing, and operational readiness.

## Step 1: Identify the Feature

From user input:
```
$ARGUMENTS
```

Check available plans:
```bash
ls .specify/specs/*/plan.md
```

## Step 2: Read the Plan

Read the plan file:
```bash
cat .specify/specs/[FEATURE_NAME]/plan.md
```

Also read the spec to ensure tasks map to acceptance criteria:
```bash
cat .specify/specs/[FEATURE_NAME]/spec.md
```

## Step 3: Create Tasks Document

Create `.specify/specs/[FEATURE_NAME]/tasks.md`:

```markdown
# Tasks: [Feature Name]

## Task Breakdown

### Phase 1: Foundation

#### Task 1.1: Executor Setup
**Description**: Configure workflow executor and credentials
**File**: [executor configuration]
**Acceptance Criteria**: AC-1, AC-2
**Dependencies**: None
**Verification**: Executor can authenticate and create test workflow

#### Task 1.2: Secrets Integration
**Description**: Configure secrets backend and secret injection
**File**: [secret configuration]
**Acceptance Criteria**: Secrets accessible at runtime, not in logs
**Dependencies**: Task 1.1
**Verification**: Workflow runs with secrets loaded

#### Task 1.3: Observability Setup
**Description**: Configure metrics, logging, and tracing
**File**: [observability configuration]
**Acceptance Criteria**: Metrics visible, logs structured with correlation IDs
**Dependencies**: Task 1.1
**Verification**: Dashboard shows workflow metrics

### Phase 2: Core Implementation

#### Task 2.1: [Step Name] Implementation
**Description**: Implement step logic per spec
**File**: [implementation files]
**Acceptance Criteria**: [Mapped to ACs]
**Dependencies**: Task 1.1
**Verification**: Step executes successfully with correct outputs

#### Task 2.2: Retry & Timeout Configuration
**Description**: Configure per-step retry policies and timeouts
**File**: [workflow definition]
**Acceptance Criteria**: R1 (MUST) - Retry policy enforced
**Dependencies**: Task 2.1
**Verification**: Simulated failure triggers retry

#### Task 2.3: Idempotency Implementation
**Description**: Implement idempotency keys and deduplication
**File**: [idempotency logic]
**Acceptance Criteria**: R2 (MUST) - Re-running produces same result
**Dependencies**: Task 2.1
**Verification**: Duplicate run returns same output

[... Additional step tasks ...]

### Phase 3: Validation

#### Task 3.1: Schema Validation in CI
**Description**: Add JSON Schema validation to CI pipeline
**File**: [.github/workflows/validate-spec.yml]
**Acceptance Criteria**: Invalid specs rejected at PR time
**Dependencies**: Tasks 2.x
**Verification**: PR with invalid spec fails CI

#### Task 3.2: Unit Tests
**Description**: Write unit tests for step logic
**File**: [test files]
**Acceptance Criteria**: [Coverage target]
**Dependencies**: Tasks 2.x
**Verification**: Tests pass

#### Task 3.3: Integration Tests
**Description**: Test external dependencies with mocks
**File**: [integration test files]
**Acceptance Criteria**: External calls mocked, behavior verified
**Dependencies**: Task 3.2
**Verification**: Integration tests pass

#### Task 3.4: End-to-End Tests
**Description**: Run complete workflow in test environment
**File**: [e2e test files]
**Acceptance Criteria**: All acceptance criteria verified
**Dependencies**: Task 3.3
**Verification**: E2E tests pass

#### Task 3.5: Chaos/Failure Injection Tests
**Description**: Test failure scenarios
**File**: [chaos test files]
**Acceptance Criteria**: System handles failures per error taxonomy
**Dependencies**: Task 3.4
**Verification**: Identified failures handled correctly

### Phase 4: Operational Readiness

#### Task 4.1: Metrics Dashboard
**Description**: Create operational dashboard
**File**: [dashboard config]
**Acceptance Criteria**: Dashboard shows SLIs
**Dependencies**: Task 1.3
**Verification**: Dashboard renders with sample data

#### Task 4.2: Alert Rules
**Description**: Configure alert rules per spec
**File**: [alert config]
**Acceptance Criteria**: Alerts fire on defined conditions
**Dependencies**: Task 4.1
**Verification**: Test alert fires correctly

#### Task 4.3: Runbook Documentation
**Description**: Document operational procedures
**File**: [runbook.md]
**Acceptance Criteria**: Common issues documented
**Dependencies**: Task 4.2
**Verification**: Runbook reviewed by operator

#### Task 4.4: Rollback Procedure Test
**Description**: Test rollback procedure in staging
**File**: N/A (procedure)
**Acceptance Criteria**: Rollback verified in non-production
**Dependencies**: Task 4.3
**Verification**: Rollback executed successfully

### Phase 5: Deployment

#### Task 5.1: Staging Deployment
**Description**: Deploy to staging environment
**File**: [deployment config]
**Acceptance Criteria**: All tests pass in staging
**Dependencies**: Tasks 4.x
**Verification**: Workflow runs successfully in staging

#### Task 5.2: Production Rollout
**Description**: Deploy to production with rollout strategy
**File**: [deployment config]
**Acceptance Criteria**: Production deployment succeeds
**Dependencies**: Task 5.1
**Verification**: Production workflow operational

## Task Dependencies Graph

```
Phase 1:     1.1 → 1.2 → 1.3
Phase 2:     2.1 → 2.2 → 2.3 ...
Phase 3:     3.1 ← 2.x → 3.2 → 3.3 → 3.4 → 3.5
Phase 4:     4.1 → 4.2 → 4.3 → 4.4
Phase 5:     5.1 → 5.2
```

## Step 3b: Parallel Task Drafting (Subagents)

Draft tasks by phase in parallel, then merge into a single `tasks.md`:
- Subagent A: Phase 1 (foundation)
- Subagent B: Phase 2 (core implementation)
- Subagent C: Phase 3 (validation)
- Subagent D: Phase 4 (operational readiness)
- Subagent E: Phase 5 (deployment)

Merge rules:
- Preserve dependencies and acceptance criteria mapping
- Ensure each task lists file paths and verification
- Validate dependency graph after merge

## Acceptance Criteria Mapping

| Task | Acceptance Criteria |
|------|---------------------|
| 1.1 | - |
| 1.2 | AC-SEC-1 |
| 1.3 | AC-OBS-1, AC-OBS-2 |
| 2.1 | AC-1, AC-2 |
| 2.2 | R1 |
| 2.3 | R2 |
| ... | ... |

## Notes

- [Any additional notes]
```

## Step 4: Break Down the Plan

For each section in the plan:
1. Identify specific, actionable tasks
2. Determine file paths
3. Note dependencies
4. Map to acceptance criteria from spec
5. Add validation tasks (schema, conformance, chaos)

## Step 5: Ensure Validation Coverage

Verify tasks include:
- [ ] Schema validation (CI gate)
- [ ] Unit tests
- [ ] Integration tests
- [ ] End-to-end tests
- [ ] Chaos/failure injection
- [ ] Observability verification
- [ ] Rollback testing

## Step 6: Link to Next Step

> Tasks created! Now use `/speckit.implement` to execute the tasks:
> ```
> /speckit.implement
> ```

## Output

Provide:
1. Number of tasks created
2. Task phases
3. Dependencies identified
4. Validation/conformance tasks highlighted
