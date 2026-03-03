---
description: Create a detailed best-in-class specification for a workflow/pipeline/automation system
---

# Specify Command

You are creating a best-in-class workflow specification at `.specify/specs/[FEATURE_NAME]/spec.md`.

**Philosophy**: Specs are operational contracts that can be validated automatically. If a spec can't drive scenario-based validation, it is not complete.

## Step 1: Identify the Feature

Determine the feature name from user input:
```
$ARGUMENTS
```

If no feature name is provided, ask the user what they want to build.

## Step 2: Check Project Structure

Ensure the spec directory exists:

```bash
mkdir -p .specify/specs
```

## Step 3: Create Feature Directory

```bash
FEATURE_NAME="[YOUR-FEATURE-NAME]"
mkdir -p ".specify/specs/$FEATURE_NAME"
```

Replace `[YOUR-FEATURE-NAME]` with a descriptive name (e.g., `001-ci-pipeline`, `deploy-workflow`).

## Step 4: Create Best-in-Class Specification Document

Create `.specify/specs/[FEATURE_NAME]/spec.md` with this comprehensive template:

```markdown
# <Workflow Name> Spec

## Overview
[Problem, solution, outcomes, and why now. 3-8 sentences.]

## Purpose and scope
- **In scope**: [What this spec governs - workflow/pipeline/automation]
- **Out of scope**: [What it explicitly excludes]
- **Environments / blast radius**: [Where it runs, what's affected]

## Audience and roles
- **Authors**: [Who defines intent, constraints, acceptance criteria]
- **Reviewers**: [Who validates design correctness, security, operability]
- **Implementers**: [Who builds workflow logic and integrations]
- **Operators**: [Who runs and monitors the workflow]
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
- R3 (MAY): [Requirement]

**Use MUST/SHOULD/MAY keywords per RFC 2119 semantics.**

## Architecture
- **Components**: [Component diagram + responsibilities]
- **Deployment model**: [Where it runs - cluster/account/namespace]
- **Trust boundaries**: [Identity model, permission boundaries]
- **State**: [Durable state vs ephemeral]

## Data flows
- **Parameters vs artifacts vs external stores**: [How data moves between steps]
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
    "field1": { "type": "string", "description": "..." },
    "field2": { "type": "integer", "default": 10 }
  }
}
```
- Example input:
```json
{ "field1": "example", "field2": 10 }
```

### Outputs
- Output schema:
```json
{
  "type": "object",
  "properties": {
    "result": { "type": "string" }
  }
}
```
- Example output:
```json
{ "result": "success" }
```

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
- **Inputs**: [What it consumes]
- **Outputs**: [What it produces]
- **Timeout**: [seconds/minutes]
- **Retries**:
  - Max attempts: [n]
  - Backoff: [none/fixed/exponential]
  - Non-retriable errors: [list]
- **Idempotency**:
  - Idempotency key: [key pattern]
  - Deduplication rules: [how to handle re-runs]
- **Secrets used**: [what secrets this step needs]
- **Observability**:
  - Events emitted: [step_started, step_completed, step_failed]
  - Metrics: [custom metrics if any]

#### Step 2: [step-name]
...

## Error handling
### Failure Taxonomy
- **Retriable errors**: [list with retry policy]
- **Non-retriable errors**: [list that causes workflow failure]
- **Human-notification triggers**: [when to page]

### Retry Policy Summary
| Step | Max Attempts | Backoff | Timeout |
|------|-------------|---------|---------|
| step1 | 2 | exponential | 300s |
| step2 | 1 | none | 60s |

## Idempotency
- **Idempotency keys**: [pattern for each step with side effects]
- **Deduplication strategy**: [how duplicates are handled]
- **Side-effect boundaries**: [what can be safely re-run]

## Security
### Identity Model
- **Service account**: [name]
- **Roles/permissions**: [RBAC policy]

### Threat Model Summary
- [Key threats and mitigations]

### Compliance
- **Control families**: [e.g., Access Control, Audit Logging]
- **Verification standards**: [e.g., OWASP ASVS if applicable]

## Secrets handling
- **Sources**: [where secrets come from - Vault, AWS Secrets Manager, K8s secrets]
- **Injection method**: [env vars, volume mounts]
- **Rotation**: [how often, process]
- **Invariant**: NO secrets in code, logs, or spec artifacts

## Observability
### Logs
- **Format**: [json/structured]
- **Correlation IDs**: [run_id, step_id, spec_version]
- **What to log**: [key events]

### Metrics (SLIs)
| Metric | Type | Labels |
|--------|------|--------|
| workflow_duration_seconds | histogram | workflow_name, status |
| workflow_success_ratio | gauge | workflow_name |
| step_execution_seconds | histogram | step_name |

### Traces
- **Trace ID**: [run_id]
- **Span hierarchy**: [workflow → step → activity]

## SLAs and SLOs
- **SLO name**: [e.g., prod_deploy_success]
- **Target**: [e.g., 0.995]
- **Window**: [e.g., 30d]
- **Error budget**: [implied]

## Monitoring and alerting
### Alerts
| Alert | Condition | Severity | Paging |
|-------|-----------|----------|--------|
| SLO burn rate | burn rate > 10 for 1h | critical | true |
| Step timeout | step exceeds 2x timeout | warning | false |

- **Runbook**: [link or content]
- **Ownership**: [team/on-call]

## Performance and scalability
- **Expected load**: [concurrent runs, frequency]
- **Resource sizing**: [CPU/memory per step]
- **Concurrency limits**: [max parallel]
- **Backpressure behavior**: [what happens at limit]
- **Capacity assumptions**: [based on what]

## Dependency management
- **External systems**: [list with versions]
- **Compatibility constraints**: [version matrix]
- **Failure behavior**: [what happens when dependency degrades]

## Testing strategy
- **Unit tests**: [coverage scope]
- **Integration tests**: [external dependencies]
- **End-to-end tests**: [real execution paths]
- **Chaos/failure injection**: [top risks to test]

## Validation plan
- **Schema validation**: [JSON Schema validation in CI]
- **Conformance tests**: [DoD-based verification]
- **Manual verification**: [what requires human check]

## Rollout and rollback
### Rollout Plan
- **Strategy**: [e.g., progressive, canary, blue-green]
- **Stages**: [staging → canary → production]
- **Feature flags**: [if applicable]

### Rollback Triggers
- [Conditions that trigger rollback]

### Rollback Steps
- [How to revert]

## Migration (if applicable)
- **State/data migration**: [steps for existing data]
- **Backfill strategy**: [how to handle historical data]
- **In-flight runs**: [how to handle during migration]

## Compatibility
- **Backward compatibility**: [input/output guarantees]
- **Forward compatibility**: [client version matrix]
- **Deprecation policy**: [how breaking changes are introduced]

## Versioning
- **Scheme**: [SemVer or API-style alpha/beta/stable]
- **Spec version**: [e.g., 1.2.0]
- **Changelog**:
  - v1.2.0 (2026-03-02): [summary]
  - v1.1.0 (2026-01-15): [summary]

## Acceptance criteria
- AC-1 (MUST): [Testable condition]
- AC-2 (SHOULD): [Testable condition]

## Examples
### Golden Path Example
[Complete example with inputs, expected outputs, observability signals]

### Failure Path Example
[Example showing error handling behavior]

## Appendices
### YAML/JSON Examples
[Executor-specific examples - K8s CRD, CI YAML, code-defined]

### Code Snippets
[Minimal reference implementations]
```

## Step 5: Prompt for Details

Ask the user clarifying questions to fill in the template. Key questions:

1. **Purpose**: What problem does this workflow solve? What's the blast radius if it fails?
2. **Inputs/Outputs**: What data enters? What data exits? What's the schema?
3. **Steps**: What's the step graph? What are timeouts and retry policies?
4. **Security**: What secrets are needed? What identity/permissions?
5. **Observability**: What metrics? What's the SLO?
6. **Failure**: What's retriable vs not? What's the rollback plan?

## Step 5b: Refinement Prompts (Auto-Check)

After generating spec, run quick content check before proceeding:

### 1. Overview Check
- Count sentences in Overview section (should be >= 3)
- If < 3 sentences, prompt: "Your Overview is [N] sentences. Add more context about the problem and why this matters now."

### 2. User Stories Check
- Count "Story:" occurrences (should be >= 1)
- If 0 stories: "Add at least 1 user story with clear actor, action, and benefit. Format: Story: As a [user], I want to [action], so that [benefit]."

### 3. Acceptance Criteria Check
- Count "AC-" occurrences (should be >= 2)
- If < 2: "Add at least 2 acceptance criteria in format 'AC-N: [testable condition]'."

### 4. Non-Functional Check
- Search for "performance" OR "security" OR "accessibility" (should find >= 1)
- If 0: "You have 0 Non-Functional requirements. Add at least one: performance (latency/throughput), security (auth/encryption), or accessibility (compliance)."

### 5. Out of Scope Check
- Count bullet points under "Out of Scope" or "Non-goals" (should be >= 1)
- If 0: "Define at least 1 item in Out of Scope to clarify boundaries."

### 6. Dependencies Check
- Check Dependencies section exists (section must exist even if empty)
- If section missing: "Add a Dependencies section listing external systems."

**For each failed check, prompt user with specific question. Do not proceed to planning until all required sections pass.**

## Step 6: Run Spec Lint

After refinement, run validation to confirm spec completeness:

```bash
# Run spec lint (if using CLI)
/speckit.lint [FEATURE_NAME]
```

Or manually verify:
- All 6 required sections present (Overview, User Stories, Functional Requirements, Non-Functional, Dependencies, Out of Scope)
- Minimum content thresholds met

**Only proceed to planning if spec passes lint (PASS or WARN acceptable).**

## Step 7: Link to Next Step

After creating the specification, inform the user:

> Specification created! Now use `/speckit.plan` to define the technical implementation. Example:
> ```
> /speckit.plan Use Argo Workflows with Kubernetes...
> ```

## Output

Provide a summary of:
1. The workflow name and directory
2. Key sections completed (inputs/outputs, steps, security, observability)
3. Next steps for the user
