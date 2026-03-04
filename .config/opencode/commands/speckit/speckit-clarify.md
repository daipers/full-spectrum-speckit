---
description: Clarify ambiguous areas in specifications before planning
---

# Clarify Command

You are helping clarify ambiguous or incomplete areas in a specification.

## Step 1: Identify the Feature

From user input or check for current feature:
```bash
ls .specify/specs/
```

## Step 2: Read the Specification

```bash
cat .specify/specs/[FEATURE_NAME]/spec.md
```

## Step 3: Identify Ambiguities

Look for:
- Vague requirements ("user-friendly", "fast")
- Missing details
- Contradictory statements
- Unclear acceptance criteria
- Edge cases not covered

## Step 4: Ask Clarifying Questions

For each ambiguity, ask specific questions:

### Example Questions

1. **About functionality**:
   - "Should users be able to X or only Y?"
   - "What happens when [edge case] occurs?"

2. **About data**:
   - "What is the maximum length for [field]?"
   - "How should we handle [invalid input]?"

3. **About behavior**:
   - "What should happen if [error condition]?"
   - "Is this required or optional?"

4. **About scope**:
   - "Does this include [related feature]?"
   - "Are there any platform requirements?"

## Step 5: Document Clarifications

Create `.specify/specs/[FEATURE_NAME]/clarifications.md`:

```markdown
# Clarifications: [Feature Name]

## Questions & Answers

### Q1: [Question]
**A**: [Answer]
**Decision Made**: [Date]

### Q2: [Question]
**A**: [Answer]
**Decision Made**: [Date]

## Updated Requirements

Based on clarifications, update the spec.md with:
- [Requirement change 1]
- [Requirement change 2]

## Out of Scope (Confirmed)

- [Item 1]
- [Item 2]
```

## Step 6: Update Specification

After getting answers, update spec.md with:
- Clarified acceptance criteria
- Additional edge cases
- Specific constraints

## Output

Provide:
1. List of clarifications sought
2. Answers received
3. Changes made to the spec

## Clarification Checklist (Prescriptive)

Before closing the clarify loop:
- [ ] Every vague term is replaced with measurable criteria
- [ ] Each MUST requirement has explicit acceptance criteria
- [ ] Edge cases are listed with expected behavior
- [ ] Security, observability, and rollback questions answered

## Example Clarification Entry

```markdown
### Q3: What should happen if dependency X is down?
**A**: Retry twice with exponential backoff; if still failing, mark run failed and page on-call.
**Decision Made**: 2026-03-03
```
