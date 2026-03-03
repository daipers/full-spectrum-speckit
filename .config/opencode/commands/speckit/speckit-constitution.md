---
description: Create or update the project constitution with best-in-class governance principles for workflow specifications
---

# Constitution Command

You are updating the project constitution at `.specify/memory/constitution.md`. This file contains the foundational principles that guide all workflow/pipeline/automation development decisions.

**Philosophy**: Specs are operational contracts, not just design documents. They must be machine-validatable and drive conformance testing.

## Step 1: Check for Existing Constitution

First, check if a constitution already exists:

```bash
ls -la .specify/memory/constitution.md 2>/dev/null || echo "No constitution found"
```

## Step 2: Gather User Input

The user wants to establish project principles. Consider their input:

```
$ARGUMENTS
```

If no specific principles are provided, present these core principles for their input:

## Step 3: Create or Update Constitution

If no constitution exists, create `.specify/memory/constitution.md` with:

```markdown
# Constitution

## Overview
[Project name] uses spec-driven development for all workflows and automations. Specs are operational contracts that drive validation, implementation, and operations.

## Core Principles

### 1. Spec as Contract
- **Standard**: Every workflow MUST have a spec that defines inputs, outputs, behavior, and operational boundaries
- **Rationale**: Ambiguous scopes and underspecified I/O contracts are primary causes of production failures

### 2. Validation Over Code Review
- **Standard**: Specs must be validated automatically; conformance tests must verify acceptance criteria
- **Rationale**: Human review cannot catch all edge cases; automated validation is reproducible and scalable

### 3. Required Specification Sections
All specs MUST include these sections (or explicitly mark "Not Applicable" with justification):

| Section | Purpose |
|---------|---------|
| Overview | Problem, solution, outcomes |
| Purpose and scope | In-scope, out-of-scope, blast radius |
| Audience and roles | Authors, reviewers, implementers, operators |
| Goals | Measurable outcomes with success metrics |
| Non-goals | Explicitly excluded behaviors |
| Requirements (MUST/SHOULD/MAY) | Traceable to tickets and acceptance criteria |
| Architecture | Components, deployment, trust boundaries |
| Data flows | Parameters vs artifacts, sensitivity classification |
| Inputs/outputs | JSON Schema with examples |
| Preconditions/postconditions | Required state before/after run |
| Workflow steps | Step graph with timeouts, retries, idempotency |
| Error handling | Failure taxonomy, retry policies |
| Idempotency | Keys and deduplication strategy |
| Security | Identity model, least privilege |
| Secrets handling | Sources, injection, rotation |
| Observability | Logs, metrics, traces, correlation IDs |
| SLAs/SLOs | Explicit targets and alerting |
| Rollout/rollback | Deployment strategy and revert procedures |
| Testing strategy | Unit, integration, e2e, chaos |
| Acceptance criteria | Testable conditions mapped to requirements |

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
- **Rationale**: Embedded secrets are a primary security failure mode

### 7. Observability as Design Requirement
- **Standard**: Every workflow MUST define metrics, logging, and correlation IDs
- **Standard**: SLAs/SLOs MUST be explicit with alerting policy
- **Rationale**: Operations cannot debug what they cannot observe

### 8. Idempotency Requirements
- **Standard**: Steps with side effects MUST declare idempotency keys
- **Standard**: Retry policies MUST be bounded with explicit backoff
- **Rationale**: Unbounded retries and non-idempotent operations cause cascading failures

### 9. Rollout Governance
- **Standard**: All production workflows MUST have documented rollback procedures
- **Standard**: Operational sign-off REQUIRED before production rollout
- **Rationale**: Silent failures and un-revertible changes are unacceptable

### 10. Versioning Discipline
- **Standard**: Specs use SemVer; breaking changes require version bump
- **Standard**: Changelog MUST document changes per version
- **Rationale**: Comparability across versions decays without explicit versioning

## Governance

### Spec Review Checklist
Before merging, verify:
- [ ] All MUST requirements have traceability (ticket + acceptance criterion)
- [ ] Inputs/outputs have JSON Schema with examples
- [ ] Steps have explicit timeouts and retry policies
- [ ] Idempotency keys defined for non-idempotent operations
- [ ] Secrets handling is explicit (source, injection, rotation)
- [ ] Metrics and logging defined with correlation IDs
- [ ] SLAs/SLOs explicit with alerting policy
- [ ] Rollback procedure documented

### Amendment Process
1. Propose change with rationale
2. Gather team feedback
3. Update version number
4. Document the change in changelog

### Versioning
- MAJOR: Backward incompatible changes to inputs/outputs/behavior
- MINOR: New requirements, expanded sections, or new optional features
- PATCH: Clarifications, fixes, non-breaking additions

---

**Version**: 1.0.0
**Ratification Date**: [DATE]
**Last Amended**: [DATE]
```

## Step 4: Link to Next Step

After creating the constitution, inform the user about the next step:

> Constitution created! Now use `/speckit.specify` to define what you want to build. Example:
> ```
> /speckit.specify Build a CI pipeline that builds, tests, scans, and deploys...
> ```

## Output

Provide a summary of:
1. What principles were established
2. The version number
3. Key governance requirements (validation, secrets, observability)
4. Next steps for the user
