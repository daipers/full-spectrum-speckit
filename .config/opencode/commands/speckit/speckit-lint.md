---
description: Validate specification completeness - check required sections and content thresholds
agent: build
---

# Speckit Lint Command

You are running spec validation to check if a specification meets all required sections and content thresholds.

## Step 1: Identify the Spec

Determine the spec to validate:
```
$ARGUMENTS
```

If no spec name is provided, ask the user which spec to validate.

## Step 2: Locate Spec Files

```bash
FEATURE_NAME="[SPEC-NAME]"
SPEC_DIR=".specify/specs/$FEATURE_NAME"
SPEC_INDEX="$SPEC_DIR/spec.md"
SECTIONS_DIR="$SPEC_DIR/sections"

if [ -d "$SECTIONS_DIR" ]; then
  echo "Found sections at: $SECTIONS_DIR"
elif [ -f "$SPEC_INDEX" ]; then
  echo "Found spec index at: $SPEC_INDEX"
else
  echo "ERROR: Spec not found at $SPEC_DIR"
  echo "Usage: /speckit.lint [FEATURE_NAME]"
  exit 1
fi
```

## Step 3: Run Validation

Parse the spec and check for required sections and content thresholds.

### Validation Logic

Read the spec files and check these sections. Prefer section files in `sections/` when present; fall back to `spec.md` for legacy single-file specs.

#### 1. Overview Check
- **Required**: Section exists AND has >= 100 characters
- **Pattern**: Look for Overview section file or "## Overview" heading
- **Threshold**: >= 100 chars of content under the heading

#### 2. User Stories Check
- **Required**: Section exists AND has >= 1 "Story:" pattern OR "- [ ]" (checkbox)
- **Pattern**: Look for User Stories section file or "## User Stories" / "### Stories" heading
- **Threshold**: >= 1 occurrence of "Story:" keyword

#### 3. Acceptance Criteria Check
- **Required**: Section exists AND has >= 1 "AC-" pattern
- **Pattern**: Look for "AC-" prefix (e.g., AC-1, AC-2)
- **Threshold**: >= 1 "AC-" occurrence

#### 4. Functional Requirements Check
- **Required**: Section exists AND has >= 1 "R1" or "MUST" pattern
- **Pattern**: Look for "R1" or "MUST" keyword
- **Threshold**: >= 1 MUST requirement

#### 5. Non-Functional Requirements Check
- **Required**: Section exists AND has >= 1 of: "performance", "security", "accessibility"
- **Pattern**: Look for any of these keywords
- **Threshold**: >= 1 non-functional requirement

#### 6. Dependencies Check
- **Required**: Section exists (content optional)
- **Pattern**: Look for Dependencies section file or "## Dependencies" / "## Dependency"
- **Threshold**: Section must exist

#### 7. Out of Scope Check
- **Required**: Section exists AND has >= 1 bullet point
- **Pattern**: Look for Out of Scope section file or "## Out of Scope" / "## Non-goals"
- **Threshold**: >= 1 list item

## Step 4: Generate Report

Output structured validation report:

```markdown
## Spec Lint Report: [FEATURE_NAME]
Status: [PASS|WARN|FAIL]

### Required Sections

| Section | Status | Details |
|---------|--------|---------|
| Overview | [✓/✗] | [char count or "missing"] |
| User Stories | [✓/✗] | [story count or "missing"] |
| Acceptance Criteria | [✓/✗] | [criteria count or "missing"] |
| Functional Requirements | [✓/✗] | [MUST count or "missing"] |
| Non-Functional Requirements | [✓/✗] | [found keywords or "missing"] |
| Dependencies | [✓/✗] | [section exists or "missing"] |
| Out of Scope | [✓/✗] | [item count or "missing"] |

### Warnings

[Recommended sections missing - if any]

- [ ] Data Model section missing (recommended)
- [ ] API Endpoints section missing (recommended)
- [ ] UI/UX Guidelines section missing (recommended)

### Result

[PASS / WARN / FAIL - missing X required sections]
```

## Step 5: Exit Codes

- **0**: PASS or WARN (spec is usable, may have warnings)
- **1**: FAIL (missing required sections, fix before implementation)

## Step 6: Examples

### Example 1: Passing Spec

Input: `/speckit.lint auth-service`

Output:
```
## Spec Lint Report: auth-service
Status: PASS

### Required Sections

| Section | Status | Details |
|---------|--------|---------|
| Overview | ✓ | 245 chars |
| User Stories | ✓ | 2 stories |
| Acceptance Criteria | ✓ | 4 criteria |
| Functional Requirements | ✓ | 3 MUST |
| Non-Functional Requirements | ✓ | security, performance |
| Dependencies | ✓ | section exists |
| Out of Scope | ✓ | 3 items |

### Warnings

- Data Model section missing (recommended)

### Result

PASS with warnings
```

### Example 2: Failing Spec

Input: `/speckit.lint simple-feature`

Output:
```
## Spec Lint Report: simple-feature
Status: FAIL

### Required Sections

| Section | Status | Details |
|---------|--------|---------|
| Overview | ✗ | 45 chars (< 100 required) |
| User Stories | ✗ | missing |
| Acceptance Criteria | ✗ | missing |
| Functional Requirements | ✗ | missing |
| Non-Functional Requirements | ✗ | missing |
| Dependencies | ✓ | section exists |
| Out of Scope | ✗ | missing |

### Warnings

- User Stories section missing (required)
- Acceptance Criteria section missing (required)
- Functional Requirements section missing (required)
- Non-Functional Requirements section missing (required)
- Out of Scope section missing (required)

### Result

FAIL - missing 5 required sections
```

## Usage in Workflow

Integrate into your workflow:

```bash
# After creating a spec
/speckit.lint [FEATURE_NAME]

# In CI/CD
if ! speckit.lint [FEATURE_NAME]; then
  echo "Spec validation failed. Fix required sections."
  exit 1
fi
```

## Integration with speckit-autopilot

This command is called automatically in Phase 3 of the autopilot workflow:
- After `speckit-specify` generates a spec
- Before proceeding to `speckit-plan`
- As a gate to prevent thin specs from reaching implementation

---

## Related Commands

- `speckit-rubric.md`: Full rubric with detailed standards
- `speckit-review.md`: Human review checklist for deeper validation
- `speckit-specify.md`: Spec generation with refinement prompts
