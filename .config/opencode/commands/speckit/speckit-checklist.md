---
description: Generate and manage quality checklists for best-in-class specifications
---

# Checklist Command

You are creating or reviewing quality checklists for a specification based on best-in-class standards.

## Step 1: Identify the Feature

From user input:
```
$ARGUMENTS
```

Or check current feature:
```bash
ls .specify/specs/
```

## Step 2: Read the Specification

```bash
cat .specify/specs/[FEATURE_NAME]/spec.md
```

## Step 3: Generate Best-in-Class Checklist

Create `.specify/specs/[FEATURE_NAME]/checklist.md`:

```markdown
# Quality Checklist: [Feature Name]

## Spec Fundamentals

### Purpose and Scope
- [ ] Purpose is clear (problem, solution, outcomes, why now)
- [ ] In-scope explicitly defined
- [ ] Out-of-scope explicitly defined
- [ ] Environments / blast radius specified

### Audience and Roles
- [ ] Authors defined with responsibilities
- [ ] Reviewers defined
- [ ] Implementers defined
- [ ] Operators defined with escalation path

### Requirements (RFC 2119)
- [ ] Every MUST has traceability (ticket + AC)
- [ ] Every SHOULD has traceability
- [ ] Requirements are unambiguous

## Workflow Contract

### Inputs/Outputs
- [ ] Input schema defined (JSON Schema Draft 2020-12)
- [ ] Input examples provided
- [ ] Output schema defined
- [ ] Output examples provided

### Preconditions/Postconditions
- [ ] Preconditions documented (secrets, permissions, deps)
- [ ] Postconditions on success defined
- [ ] Postconditions on failure defined
- [ ] Postconditions on cancel defined

### Workflow Steps
- [ ] Step graph explicit (ordering/DAG)
- [ ] Each step has timeout defined
- [ ] Each step has retry policy defined
- [ ] Step dependencies clear

## Safety and Reliability

### Error Handling
- [ ] Failure taxonomy defined
- [ ] Retriable errors enumerated
- [ ] Non-retriable errors enumerated
- [ ] Human notification triggers defined

### Retries and Backoff
- [ ] Max attempts per step defined
- [ ] Backoff strategy defined
- [ ] Timeout per attempt defined

### Idempotency
- [ ] Idempotency requirements stated
- [ ] Idempotency keys defined
- [ ] Deduplication rules specified

## Security

### Identity Model
- [ ] Service account / identity defined
- [ ] RBAC / permissions least privilege
- [ ] No wildcard permissions

### Secrets Handling
- [ ] Secrets sources identified
- [ ] Injection method specified
- [ ] Rotation expectations defined
- [ ] "No secrets in code/logs" invariant clear

### Compliance
- [ ] Control families referenced
- [ ] Verification standards referenced (e.g., OWASP ASVS)

## Observability

### Logs
- [ ] Log format defined (json/structured)
- [ ] Correlation IDs specified
- [ ] What to log enumerated

### Metrics (SLIs)
- [ ] Workflow duration metric defined
- [ ] Success ratio metric defined
- [ ] Step-level metrics defined
- [ ] Labels and cardinality considered

### SLAs/SLOs
- [ ] SLOs explicit with targets
- [ ] Windows defined
- [ ] Alert conditions defined

### Monitoring
- [ ] Dashboards specified
- [ ] Runbooks documented
- [ ] Alert ownership clear

## Operations

### Performance
- [ ] Expected load defined
- [ ] Concurrency limits specified
- [ ] Resource sizing guidance

### Rollout/Rollback
- [ ] Rollout strategy defined
- [ ] Rollback triggers enumerated
- [ ] Rollback steps documented

### Migration
- [ ] State migration steps defined (if applicable)
- [ ] Backfill strategy defined (if applicable)
- [ ] In-flight run handling defined (if applicable)

## Versioning

- [ ] Spec version defined
- [ ] Changelog exists
- [ ] Breaking changes documented

## Testing and Validation

### Test Strategy
- [ ] Unit tests planned
- [ ] Integration tests planned
- [ ] E2E tests planned
- [ ] Chaos/failure injection planned

### Validation
- [ ] Schema validation in CI
- [ ] Acceptance criteria mapped to tests

## Examples

- [ ] Golden path example provided
- [ ] Failure path example provided

## Review Status

| Category | Status | Notes |
|----------|--------|-------|
| Spec Fundamentals | [ ] | |
| Workflow Contract | [ ] | |
| Safety & Reliability | [ ] | |
| Security | [ ] | |
| Observability | [ ] | |
| Operations | [ ] | |
| Versioning | [ ] | |
| Testing | [ ] | |

**Overall Status**: [ ] Ready for Planning / [ ] Needs Work
```

## Step 4: Review Checklist

Go through each item and:
1. Mark as checked if addressed in spec
2. Note any gaps or issues
3. Provide recommendations for improvements

## Step 5: Address Gaps

For any unchecked items:
1. Identify what's missing
2. Propose additions to the spec
3. Update spec.md accordingly

## Output

Provide:
1. Checklist completion status
2. Items needing attention
3. Recommendations

## Usage Checklist (Prescriptive)

When using this checklist:
- [ ] Work top-to-bottom; do not skip sections
- [ ] If any item is unchecked, add a spec change note
- [ ] Require at least one example in Inputs/Outputs and Examples sections

## Example Review Summary

```markdown
## Review Summary: auth-pipeline
- Overall: Needs Work
- Missing: Idempotency keys for deploy step, rollback triggers
- Next: Update spec sections and re-run /speckit.lint
```
