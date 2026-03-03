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

## Step 4: Generate Phase PLAN.md

Create `${PHASE_DIR}/001-PLAN.md`:

```markdown
---
phase: ${PHASE_NAME}
plan: "01"
type: execute
wave: 1
depends_on: []
autonomous: true

must_haves:
  truths:
    - "[Truth 1 from spec]"
    - "[Truth 2 from spec]"
  artifacts:
    - path: "[path to implement]"
      provides: "[what it provides]"
  key_links:
    - from: "spec.md"
      to: "implementation"
      via: "GSD execution"
---

<objective>
[Copy Overview from SPEC.md - what this feature does]
</objective>

<context>
# Source Specification
- Spec file: .specify/specs/${FEATURE}/spec.md
- Phase derived from Speckit specification

## Key Requirements from Spec
[MUST requirements extracted from spec]

## Architecture
[Architecture section from spec]
</context>

<tasks>

<task type="auto">
  <name>Task 1: [Task name from requirements]</name>
  <files>[files to create/modify]</files>
  <action>
[Action derived from spec requirements]
  </action>
  <verify>[How to verify this task]</verify>
  <done>[Completion criteria]</done>
</task>

[tasks for each MUST requirement]

</tasks>

<verification>
- [ ] [Verification item from spec]
- [ ] [Verification item from spec]
</verification>

<success_criteria>
- [Success criteria from spec's Acceptance Criteria]
</success_criteria>

<output>
After completion, create summary in `${PHASE_DIR}/001-SUMMARY.md`
</output>
```

## Step 5: Generate Verification Template

Create `${PHASE_DIR}/001-VERIFICATION.md` template:

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

## Step 6: Update ROADMAP.md

Add the new phase to ROADMAP.md:
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

## Step 7: Offer Execution

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
/speckit.gsd [feature]        → Creates .planning/phases/[feature]/
    ↓
/gsd-execute-phase [feature]  → Executes phase with GSD
    ↓
/gsd-verify-work [feature]    → Verifies implementation against spec
```

## Example

User types: `/speckit.gsd 001-ci-pipeline`

This reads `.specify/specs/001-ci-pipeline/spec.md` and creates:
- `.planning/phases/001-ci-pipeline/001-PLAN.md`
- `.planning/phases/001-ci-pipeline/001-VERIFICATION.md`
- Adds phase to ROADMAP.md

Then offers: `/gsd-execute-phase 001-ci-pipeline`

---

## Notes

- This command converts SPEC → GSD phase format
- GSD handles the execution, verification, and gap closure
- Speckit lint/review can be run before bridging to ensure spec quality
- The verification uses SPEC.md acceptance criteria as source of truth
