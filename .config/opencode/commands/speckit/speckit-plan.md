---
description: Create a technical implementation plan from a best-in-class specification
---

# Plan Command

You are creating a technical implementation plan based on the specification at `.specify/specs/[FEATURE_NAME]/plan.md`.

**Philosophy**: The plan translates the spec's operational contracts into executor-specific definitions (K8s CRD, CI YAML, code-defined workflows).

## Step 1: Identify the Feature

From user input:
```
$ARGUMENTS
```

If no specific feature is mentioned, ask which feature to plan (check `.specify/specs/` for available specifications).

## Step 2: Read the Specification

Read the specification file:
```bash
ls .specify/specs/
cat .specify/specs/[FEATURE_NAME]/spec.md
```

## Step 3: Create Plan Document

Create `.specify/specs/[FEATURE_NAME]/plan.md`:

```markdown
# Implementation Plan: [Feature Name]

## Executor Selection
- **Platform**: [Argo Workflows / Tekton / Airflow / Temporal / GitHub Actions / GitLab CI / other]
- **Rationale**: [Why this executor fits the spec requirements]

## Executor-Specific Configuration

### Workflow/Pipeline Definition
[Code-defined or YAML-based workflow definition]

### Step-to-Executor Mapping
| Spec Step | Executor Step | Type |
|-----------|--------------|------|
| step1 | build | container |
| step2 | test | container |
| step3 | deploy | deploy |

## Input/Output Binding
### Input Binding
- How parameters/artifacts are passed to workflow
- Schema validation approach

### Output Binding
- How outputs are captured and stored
- Artifact storage backend

## Retry & Timeout Implementation
### Per-Step Configuration
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

## Idempotency Implementation
### Idempotency Keys
- How idempotency keys are generated
- Where they're stored/checked

### Idempotency Mechanisms
- [PUT semantics for deploys]
- [Deduplication via external store]
- [Natural idempotency of operation]

## Security Implementation
### Identity
- Service account / IAM role
- RBAC policy

### Secrets
- Secrets backend integration
- Injection method (env/volumes)
- Secret rotation approach

## Observability Implementation
### Metrics Export
- Custom metrics definition
- Scrape endpoint / push gateway

### Structured Logging
- Log format
- Correlation IDs

### Tracing
- Trace context propagation
- OpenTelemetry configuration

### Alerting
- Alert rules (Prometheus/Platform-native)
- PagerDuty/OpsGenie integration

## Testing Implementation
### Unit Tests
- [Test framework]
- [Coverage targets]

### Integration Tests
- [External dependency mocking]
- [Test environment]

### E2E Tests
- [Real execution paths]
- [Verification approach]

### Chaos Tests
- [Failure injection points]
- [Blast radius control]

## Rollout & Rollback Implementation
### Deployment Strategy
- [Canary / Blue-Green / Progressive]
- Feature flags if applicable

### Rollback Mechanism
- Automatic triggers
- Manual rollback procedure

## Artifact & State Management
### Workflow Artifacts
- Storage backend
- Retention policy

### State
- Durable state (if any)
- Cleanup on completion/failure

## Compliance Mapping
- Control families addressed
- Audit trail requirements

## Tech Stack

### Frontend (if applicable)
- [Framework]
- [Language]
- [Styling]

### Backend
- [Runtime]
- [Framework]
- [Database]

### Infrastructure
- [Hosting]
- [CI/CD]

## Component Structure
```

## Step 3b: Parallel Plan Drafting (Subagents)

Draft plan sections in parallel, then merge into a single `plan.md`:
- Subagent A: Executor selection, step mapping, input/output binding
- Subagent B: Retry/timeout, idempotency, artifact/state management
- Subagent C: Security, secrets, compliance mapping
- Subagent D: Observability, alerting, testing implementation
- Subagent E: Phases, risks/mitigations, research notes, tech stack

Merge rules:
- Preserve template headings and order
- Resolve overlaps by keeping the most spec-aligned details
src/
├── workflows/
│   └── [workflow definitions]
├── tasks/
│   └── [reusable task definitions]
├── scripts/
│   └── [step implementations]
└── tests/
    ├── unit/
    ├── integration/
    └── e2e/
```

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

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| [Decision 1] | [Choice] | [Reason] |

## Risks & Mitigations

| Risk | Impact | Mitigation |
|------|--------|------------|
| [Risk 1] | [Impact] | [Mitigation] |

## Research Notes

- [Technology 1]: [Notes about version/usage]
- [Technology 2]: [Notes about version/usage]
```

## Step 4: Gather Executor Preferences

Ask the user:
1. Which workflow executor/platform should we use?
2. What's the target infrastructure (K8s, cloud, self-hosted)?
3. Any specific security/observability requirements?

## Step 5: Validate Against Spec

Before finalizing the plan, verify:
- [ ] All spec steps have corresponding executor steps
- [ ] Retry/timeouts match spec
- [ ] Idempotency mechanisms defined
- [ ] Security (secrets, identity) addressed
- [ ] Observability (metrics, logs, traces) covered

## Step 6: Link to Next Step

> Plan created! Now use `/speckit.tasks` to break this down into actionable tasks:
> ```
> /speckit.tasks
> ```

## Output

Provide:
1. Executor selection and rationale
2. Key implementation decisions
3. Mapping of spec requirements to executor configuration
4. Implementation phases
