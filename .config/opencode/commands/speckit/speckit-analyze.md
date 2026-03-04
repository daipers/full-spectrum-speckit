---
description: Analyze consistency between best-in-class specifications and implementation
---

# Analyze Command

You are analyzing the consistency between specifications, plans, and implementation for workflow specs.

## Step 1: Identify the Feature

From user input:
```
$ARGUMENTS
```

Or check for current work:
```bash
ls .specify/specs/
```

## Step 2: Gather Artifacts

Read all related files:
```bash
cat .specify/specs/[FEATURE_NAME]/spec.md
cat .specify/specs/[FEATURE_NAME]/plan.md  
cat .specify/specs/[FEATURE_NAME]/tasks.md
```

## Step 3: Analyze Coverage

Check each requirement from spec.md against plan and tasks:

### Requirements Coverage
| Requirement (R#) | Level (MUST/SHOULD) | In Plan? | In Tasks? | Implemented? |
|------------------|---------------------|----------|-----------|--------------|
| R1               | MUST                | Yes      | Yes       | [Check]      |
| R2               | MUST                | Yes      | Yes       | [Check]      |
| R3               | SHOULD              | Yes      | No        | [Check]      |

### Best-in-Class Section Coverage
| Section | In Spec? | In Plan? | In Tasks? |
|---------|----------|----------|-----------|
| Inputs/Outputs (with schema) | ✓ | - | - |
| Preconditions/Postconditions | ✓ | - | - |
| Workflow Steps | ✓ | ✓ | ✓ |
| Error Handling | ✓ | - | - |
| Retries & Backoff | ✓ | - | - |
| Idempotency | ✓ | - | - |
| Security | ✓ | - | - |
| Secrets Handling | ✓ | - | - |
| Observability | ✓ | - | - |
| SLAs/SLOs | ✓ | - | - |
| Rollout/Rollback | ✓ | - | - |
| Testing Strategy | ✓ | - | - |
| Acceptance Criteria | ✓ | - | - |

## Step 4: Check Consistency

Look for:
- **Spec-plan alignment**: Does the plan address all spec requirements?
- **Plan-executor mapping**: Are spec steps mapped to executor steps?
- **Task-plan alignment**: Do tasks cover all plan items?
- **Implementation-task alignment**: Are tasks complete?
- **Missing pieces**: Any spec requirements without implementation?

## Step 5: Create Analysis Report

Create `.specify/specs/[FEATURE_NAME]/analysis.md`:

```markdown
# Analysis Report: [Feature Name]

## Coverage Summary

| Category | Total | Covered | Missing |
|----------|-------|---------|---------|
| MUST Requirements | X | X | X |
| SHOULD Requirements | X | X | X |
| Plan Items | X | X | X |
| Tasks | X | X | X |
| Spec Sections | X | X | X |

## Best-in-Class Section Analysis

| Section | Spec | Plan | Tasks | Status |
|---------|------|------|-------|--------|
| Inputs/Outputs | ✓ | - | - | [Complete/Incomplete] |
| Error Handling | ✓ | - | - | [Complete/Incomplete] |
| Retries | ✓ | - | - | [Complete/Incomplete] |
| Idempotency | ✓ | - | - | [Complete/Incomplete] |
| Secrets | ✓ | - | - | [Complete/Incomplete] |
| Observability | ✓ | - | - | [Complete/Incomplete] |
| SLOs | ✓ | - | - | [Complete/Incomplete] |
| Rollback | ✓ | - | - | [Complete/Incomplete] |

## Findings

### ✅ Complete
- [Item that's done]

### ⚠ Needs Attention
- [Issue 1]: [Description]
- [Issue 2]: [Description]

### ❌ Missing
- [Missing item 1 - spec requirement not in plan]
- [Missing item 2 - spec requirement not implemented]

## Recommendations

1. [Recommendation 1 - e.g., add idempotency tasks]
2. [Recommendation 2 - e.g., add observability validation]

## Next Steps

Based on analysis:
- If gaps found: Complete missing items
- If inconsistencies: Fix alignment
- If complete: Ready for `/speckit.implement`
```

## Step 6: Present Findings

Summarize:
1. Coverage percentage (MUST requirements first)
2. Key missing sections (check security, idempotency, observability)
3. Recommended actions

## Output

Provide:
1. Analysis summary (MUST requirements coverage)
2. Issues identified (missing sections)
3. Recommendations (gaps to fill)

## Analysis Checklist (Prescriptive)

Before finalizing analysis.md:
- [ ] Every MUST requirement is marked in plan/tasks/implementation
- [ ] Every acceptance criterion is mapped to a test or verification
- [ ] All required spec sections appear in spec and are addressed in plan/tasks
- [ ] Gaps are actionable and tied to a specific file or task

## Example Gap Recommendation

```markdown
Recommendation: Add observability tasks for metrics + alerting.
Why: Spec defines SLIs but plan/tasks omit implementation.
Suggested fix: Add Task 4.1/4.2 in tasks.md and update plan.md Observability section.
```
