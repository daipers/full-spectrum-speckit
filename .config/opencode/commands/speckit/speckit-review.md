---
description: Human review checklist for 5-minute spec review with explicit questions
agent: build
---

# Speckit Review Command

You are conducting a human review of a specification. This checklist takes approximately 5 minutes and ensures critical decisions are not missed.

## Step 1: Identify the Spec

Determine the spec to review:
```
$ARGUMENTS
```

## Step 2: Load the Spec

Open and review: `.specify/specs/[FEATURE_NAME]/spec.md`

---

## 5-Minute Review Checklist

Complete this checklist systematically. Total time: ~5 minutes.

---

### Section 1: Key Requirements Missing? (30 seconds)

- [ ] **Can I state the core problem being solved in one sentence?**
  - If NO: Ask "What is the single most important problem this solves?"

- [ ] **Are there clear success metrics?**
  - Look for: Goals (G1, G2), SLAs, SLOs, or explicit success criteria
  - If NO: Ask "How will we know this succeeded?"

- [ ] **Does each MUST requirement have acceptance criteria?**
  - Look for: R1 (MUST) linked to AC-1, AC-2, etc.
  - If NO: Ask "Which acceptance criteria validate R1?"

---

### Section 2: Out-of-Scope Defined? (30 seconds)

- [ ] **Are there explicit non-goals?**
  - Look for: "Out of Scope" or "Non-goals" section with >= 1 item
  - If NO: Ask "What are we explicitly NOT doing?"

- [ ] **Is boundary with other systems clear?**
  - Look for: Dependencies section, integration points
  - If NO: Ask "What other systems does this touch?"

- [ ] **Is there a "what this is NOT" statement?**
  - Look for: Explicit exclusions in Out of Scope
  - If NO: Ask "What would someone incorrectly assume is included?"

---

### Section 3: Errors and Security Covered? (1 minute)

- [ ] **Are retriable vs non-retriable errors defined?**
  - Look for: Error handling section with taxonomy
  - If NO: Ask "What errors should we retry? What should fail hard?"

- [ ] **Is the threat model or security approach stated?**
  - Look for: Security section, threat model, identity model
  - If NO: Ask "What are the security risks?"

- [ ] **Are secrets handling requirements clear?**
  - Look for: Secrets handling section, external secrets mention
  - If NO: Ask "Where do secrets come from? How are they injected?"

- [ ] **Is there error handling for each step?**
  - Look for: Per-step error handling in workflow steps
  - If NO: Ask "What happens when each step fails?"

---

### Section 4: Dependencies Called Out? (1 minute)

- [ ] **Are all external systems/APIs listed?**
  - Look for: Dependencies section with external services
  - If NO: Ask "What external systems does this depend on?"

- [ ] **Are version constraints specified?**
  - Look for: Version numbers in Dependencies
  - If NO: Ask "What versions are required?"

- [ ] **Is failure behavior when dependencies fail defined?**
  - Look for: Dependency failure handling, fallback strategies
  - If NO: Ask "What happens when dependency X is down?"

- [ ] **Are there fallback/degradation strategies?**
  - Look for: Graceful degradation, fallback paths
  - If NO: Ask "Can this work in reduced capacity?"

---

### Section 5: Observability & Operations (1 minute)

- [ ] **Are metrics/SLIs defined?**
  - Look for: Metrics section, SLIs, success ratio
  - If NO: Ask "What should we measure?"

- [ ] **Is alerting policy stated?**
  - Look for: Alerts, SLO burn rate, thresholds
  - If NO: Ask "What should page someone?"

- [ ] **Is rollback procedure documented or referenced?**
  - Look for: Rollback section, rollback triggers
  - If NO: Ask "How do we undo this?"

---

### Section 6: Edge Cases (30 seconds)

- [ ] **What happens on partial failure?**
  - Look for: Idempotency, partial state handling
  - If unclear: Ask "If step 3 of 5 fails, what state are we in?"

- [ ] **What happens on timeout?**
  - Look for: Timeout settings, timeout behavior
  - If unclear: Ask "How long should we wait?"

- [ ] **What happens on invalid input?**
  - Look for: Input validation, error responses
  - If unclear: Ask "What if someone passes bad data?"

---

## Review Summary

After completing the checklist, output your assessment:

```markdown
## Review Summary: [FEATURE_NAME]

- [ ] **Ready for implementation**
  - All critical sections present
  - No blocking questions
  
- [ ] **Needs refinement** (spec section to improve)
  - Section: [name]
  - Issue: [specific problem]
  - Fix: [what's needed]
  
- [ ] **Needs clarification** (question for PM/stakeholder)
  - Question: [what needs answering]
  - Impact: [why it matters]
```

---

## Questions to Ask (Print These)

If you found issues, ask these questions:

1. "What's the single most important problem this solves?"
2. "How will we know it succeeded?"
3. "What are we explicitly NOT doing?"
4. "What should we retry vs fail hard on?"
5. "What are the security risks?"
6. "What happens when dependency X fails?"
7. "What should page someone?"
8. "How do we undo this?"

---

## Timing Guide

| Section | Time | If Running Over |
|---------|------|-----------------|
| Key Requirements | 30s | Skip edge cases section |
| Out of Scope | 30s | Skip edge cases section |
| Errors & Security | 1m | Skip observability details |
| Dependencies | 1m | Skip observability details |
| Observability | 1m | Skip edge cases |
| Edge Cases | 30s | Can skip if covered above |

---

## Integration with speckit-autopilot

This command is called in Phase 3 of the autopilot workflow:
- After `speckit-lint` passes
- Before proceeding to `speckit-plan`
- As human validation gate

Use manually or run:
```bash
/speckit.review [FEATURE_NAME]
```

---

## Related Commands

- `speckit-rubric.md`: Full rubric with detailed standards
- `speckit-lint.md`: Automated validation tool
- `speckit-specify.md`: Spec generation with refinement prompts

## Review Completion Checklist (Prescriptive)

Before finalizing the review:
- [ ] Answered the 8 key questions (print section)
- [ ] Identified at least one success metric or stated missing
- [ ] Confirmed out-of-scope exists or flagged as missing
- [ ] Verified error handling + rollback coverage

## Example Review Outcome

```markdown
## Review Summary: deploy-pipeline
- Needs refinement
  - Section: Error handling
  - Issue: Retriable vs non-retriable not defined
  - Fix: Add failure taxonomy and retry policy table
```
