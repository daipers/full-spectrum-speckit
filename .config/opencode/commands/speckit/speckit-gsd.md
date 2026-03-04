---
description: Convert a Speckit specification into GSD phase plans for execution
---

# Speckit → GSD Bridge Command

This command bridges Speckit's spec-driven development with GSD's phase-based execution. It converts a SPEC.md into executable GSD phase plans.

## Step 1: Identify the Feature

From user input:
```
$ARGUMENTS
```

Extract the feature name (e.g., `001-ci-pipeline`, `user-auth`).

If no feature is specified, ask the user which spec to convert:
```bash
ls -la .specify/specs/
```

## Step 2: Read the Speckit Specification

Read the spec file:
```bash
FEATURE="[FEATURE_NAME]"
cat ".specify/specs/$FEATURE/spec.md"
```

Extract key information:
- **Overview** - What this feature does
- **Goals** - Measurable outcomes
- **Requirements** - MUST/SHOULD/MAY items
- **Acceptance Criteria** - How to verify
- **Architecture** - Components and data flows
- **Non-goals** - What's explicitly out of scope

## Step 3: Create GSD Phase Structure

Create the phase directory:
```bash
PHASE_NAME="[kebab-case-feature-name]"
PHASE_DIR=".planning/phases/${PHASE_NAME}"
mkdir -p "$PHASE_DIR"
```

## Step 4: Create Phase CONTEXT.md for GSD Planner

Create `${PHASE_DIR}/${PHASE_NAME}-CONTEXT.md` so `/gsd-plan-phase` can read the spec directly:

```markdown
# Phase Context: ${PHASE_NAME}

## Source Spec
- Spec file: `.specify/specs/${FEATURE}/spec.md`
- Feature name: ${FEATURE}

## Overview
[Copy Overview from SPEC.md]

## Goals
[Copy Goals from SPEC.md]

## Requirements
[Copy MUST/SHOULD/MAY requirements from SPEC.md]

## Acceptance Criteria
[Copy Acceptance Criteria from SPEC.md]

## Architecture
[Copy Architecture section from SPEC.md]

## Non-goals
[Copy Non-goals from SPEC.md]

## Decisions
- [List any locked decisions from the spec]

## Deferred Ideas
- [List any explicitly deferred ideas from the spec]

## Claude's Discretion
- [List areas where implementation choices are open]
```

## Step 5: Update ROADMAP.md

Add the new phase to ROADMAP.md before planning:

```bash
# Use gsd-tools to add phase
node ~/.config/opencode/get-shit-done/bin/gsd-tools.cjs roadmap add-phase "${PHASE_NAME}" --goal "[goal from spec overview]"
```

Or manually add to ROADMAP.md:
```markdown
## Phase N: ${PHASE_NAME}
- **Goal**: [Overview from spec]
- **Status**: [ ]
- **Plans**: 1
```

## Step 6: Run GSD Plan-Phase

Invoke the GSD planner so it can use the context + spec:

```
/gsd-plan-phase ${PHASE_NAME}
```

## Step 7: Generate Verification Template (Optional)

If you want a verification template before execution, create `${PHASE_DIR}/001-VERIFICATION.md`:

```markdown
---
phase: ${PHASE_NAME}
plan: "01"
status: pending
---

# Verification: ${FEATURE}

## Source
- Spec: `.specify/specs/${FEATURE}/spec.md`
- Plan: `001-PLAN.md`

## Must-Haves

### Truths
[From plan must_haves.truths]

| Truth | Status | Evidence |
|-------|--------|----------|
| [Truth 1] | [✓/✗] | [File/line showing verification] |

### Artifacts
| Artifact | Status | Location |
|----------|--------|----------|
| [Artifact 1] | [✓/✗] | [Path] |

## Acceptance Criteria

[From spec's Acceptance Criteria section]

| Criterion | Status | Evidence |
|-----------|--------|----------|
| [AC-1] | [✓/✗] | [Test file/line] |

## Verification Result

**Status:** [passed / gaps_found / human_needed]

[Summary of verification]
```

## Step 8: Offer Execution

After creating the phase, offer to execute:
```
Phase '${PHASE_NAME}' created from Speckit spec!

To execute:
/gsd-execute-phase ${PHASE_NAME}

Or to verify the plan first:
/gsd-plan-checker ${PHASE_NAME}
```

## Integration Flow

```
/speckit.specify [feature]    → Creates .specify/specs/[feature]/spec.md
    ↓
/speckit.gsd [feature]        → Creates .planning/phases/[feature]/[feature]-CONTEXT.md
    ↓
/gsd-plan-phase [feature]     → Creates PLAN.md from spec context
    ↓
/gsd-execute-phase [feature]  → Executes phase with GSD
    ↓
/gsd-verify-work [feature]    → Verifies implementation against spec
```

## Example

User types: `/speckit.gsd 001-ci-pipeline`

This reads `.specify/specs/001-ci-pipeline/spec.md` and creates:
- `.planning/phases/001-ci-pipeline/001-ci-pipeline-CONTEXT.md`
- Adds phase to ROADMAP.md

Then offers: `/gsd-plan-phase 001-ci-pipeline`

---

## Notes

- This command converts SPEC → GSD phase format
- GSD handles the execution, verification, and gap closure
- Speckit lint/review can be run before bridging to ensure spec quality
- The verification uses SPEC.md acceptance criteria as source of truth

## Bridge Checklist (Prescriptive)

Before running `/gsd-plan-phase`:
- [ ] Spec has Goals, Requirements, Acceptance Criteria, Non-goals
- [ ] CONTEXT.md includes Decisions, Deferred Ideas, Claude's Discretion
- [ ] ROADMAP.md has a new phase entry

## Example CONTEXT.md Snippet

```markdown
## Decisions
- Use GitHub Actions (locked)

## Deferred Ideas
- Terraform automation

## Claude's Discretion
- Choose logging format for CI steps
```
