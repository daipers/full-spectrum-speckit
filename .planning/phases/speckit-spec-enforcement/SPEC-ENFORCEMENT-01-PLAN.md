---
phase: speckit-spec-enforcement
plan: 01
type: execute
wave: 1
depends_on: []
files_modified:
  - ~/.config/opencode/commands/speckit/speckit-specify.md
  - ~/.config/opencode/commands/speckit/speckit-autopilot.md
  - ~/.config/opencode/commands/speckit/speckit-lint.md
  - ~/.config/opencode/commands/speckit/speckit-review.md
autonomous: true

must_haves:
  truths:
    - "Every generated spec has all required sections filled"
    - "Spec Lint catches missing sections before implementation"
    - "Human reviewers have explicit checklist for missing decisions"
    - "Thin sections trigger refinement prompts"
    - "Implemented features can be traced back to spec sections"
  artifacts:
    - path: ~/.config/opencode/commands/speckit/speckit-specify.md
      provides: "Spec generation with template gate and refinement prompts"
    - path: ~/.config/opencode/commands/speckit/speckit-lint.md
      provides: "Spec validation script (automated lint)"
    - path: ~/.config/opencode/commands/speckit/speckit-review.md
      provides: "Human review checklist with explicit questions"
    - path: ~/.config/opencode/commands/speckit/speckit-autopilot.md
      provides: "Updated workflow with lint + review steps"
  key_links:
    - from: "speckit-specify.md"
      to: "speckit-lint.md"
      via: "auto-runs lint after generation"
    - from: "speckit-autopilot.md"
      to: "speckit-review.md"
      via: "calls review step in workflow"
---

<objective>
Add spec enforcement to Speckit autopilot: Full Spec rubric, template gate, automated lint, human review checklist, refinement prompts, and verification loop.

Purpose: Eliminate thin specs that cause implementation drift and rework. Ensure every spec has required sections with meaningful content before implementation begins.
</objective>

<execution_context>
@./.opencode/get-shit-done/workflows/execute-plan.md
@./.opencode/get-shit-done/templates/summary.md
</execution_context>

<context>
# Current Speckit State
- speckit-specify.md: Creates spec using template (lines 41-297)
- speckit-autopilot.md: Full workflow (lines 1-608)
- Existing constitution already defines required sections (line 54)
- No validation that required sections are present
- No lint or review step in workflow
</context>

<tasks>

<task type="auto">
  <name>Task 1: Create Full Spec Rubric and Checklist</name>
  <files>~/.config/opencode/commands/speckit/speckit-rubric.md</files>
  <action>
Create new file `speckit-rubric.md` with:

**Required Sections (Non-negotiable):**
1. Overview - Problem, solution, outcomes, why now (3-8 sentences)
2. User Stories + Acceptance Criteria - At least 1 user story with testable ACs
3. Functional Requirements - Inputs, processing, outputs, errors (MUST/SHOULD/MAY)
4. Non-Functional Requirements - Performance, security, accessibility (at least 1)
5. Dependencies - External systems, libraries, services (explicit list)
6. Out of Scope - What is explicitly excluded

**Recommended Sections (Validation warns if missing):**
7. Data Model - Schema, entities, relationships
8. API Endpoints - Request/response contracts
9. UI/UX Guidelines - If user-facing

**Minimum Content Thresholds:**
- Overview: >=3 sentences
- User Stories: >=1 with >=2 acceptance criteria each
- Functional Requirements: >=1 MUST requirement
- Non-Functional: >=1 (performance OR security OR accessibility)
- Dependencies: >=0 (empty allowed but must exist as section)
- Out of Scope: >=1 item

**Checklist format:** Use checkbox format for easy tracking.
  </action>
  <verify>File exists with all required sections defined</verify>
  <done>Rubric document created with required sections, minimum thresholds, and checklist format</done>
</task>

<task type="auto">
  <name>Task 2: Create Spec Lint Command</name>
  <files>~/.config/opencode/commands/speckit/speckit-lint.md</files>
  <action>
Create new file `speckit-lint.md` as a spec validation command:

**Validation Logic:**
1. Parse SPEC.md and extract all H2 sections
2. Check each required section exists (presence check)
3. Check each section has minimum content (length/count check)
4. Output structured report: PASS, WARN, FAIL

**Section Checks:**
- Overview: exists AND >=100 chars
- User Stories: exists AND has >=1 "Story:" or "- [ ]" pattern
- Acceptance Criteria: exists AND has >=1 "AC-" pattern
- Functional Requirements: exists AND has >=1 "R1" or "MUST" pattern
- Non-Functional: exists AND has >=1 "performance" OR "security" OR "accessibility"
- Dependencies: exists (content optional)
- Out of Scope: exists AND has >=1 bullet point

**Output Format:**
```
## Spec Lint Report: [spec-name]
Status: [PASS|WARN|FAIL]

Required Sections:
  [✓] Overview - OK (150 chars)
  [✓] User Stories - OK (2 stories)
  [✓] Acceptance Criteria - OK (4 criteria)
  [✓] Functional Requirements - OK (3 MUST)
  [✓] Non-Functional Requirements - OK (2, security)
  [✓: performance] Dependencies - OK (section exists)
  [✓] Out of Scope - OK (3 items)

Warnings:
  - Data Model section missing (recommended)

Result: [PASS with warnings / FAIL - missing sections]
```

**Implementation:** Write as a shell script that can be run locally OR integrated into CI.
  </action>
  <verify>Creates valid markdown report, catches missing sections, validates content thresholds</verify>
  <done>Spec Lint command created that validates all required sections and outputs structured report</done>
</task>

<task type="auto">
  <name>Task 3: Create Human Review Checklist Command</name>
  <files>~/.config/opencode/commands/speckit/speckit-review.md</files>
  <action>
Create new file `speckit-review.md` as a human review step:

**5-Minute Review Checklist:**

**1. Key Requirements Missing?** (30 seconds)
- [ ] Can I state the core problem being solved in one sentence?
- [ ] Are there clear success metrics?
- [ ] Does each MUST requirement have acceptance criteria?

**2. Out-of-Scope Defined?** (30 seconds)
- [ ] Are there explicit non-goals?
- [ ] Is boundary with other systems clear?
- [ ] Is there a "what this is NOT" statement?

**3. Errors and Security Covered?** (1 minute)
- [ ] Are retriable vs non-retriable errors defined?
- [ ] Is the threat model or security approach stated?
- [ ] Are secrets handling requirements clear?
- [ ] Is there error handling for each step?

**4. Dependencies Called Out?** (1 minute)
- [ ] Are all external systems/APIs listed?
- [ ] Are version constraints specified?
- [ ] Is failure behavior when dependencies fail defined?
- [ ] Are there fallback/degradation strategies?

**5. Observability & Operations** (1 minute)
- [ ] Are metrics/SLIs defined?
- [ ] Is alerting policy stated?
- [ ] Is rollback procedure documented or referenced?

**6. Edge Cases** (30 seconds)
- [ ] What happens on partial failure?
- [ ] What happens on timeout?
- [ ] What happens on invalid input?

**Output:** After checklist, output:
```
## Review Summary
- [ ] Ready for implementation
- [ ] Needs refinement (spec section to improve)
- [ ] Needs clarification (question for PM)
```
  </action>
  <verify>Checklist is comprehensive, covers all critical areas, fits 5-minute timebox</verify>
  <done>Human review checklist created with explicit questions and decision points</done>
</task>

<task type="auto">
  <name>Task 4: Update speckit-specify.md with Template Gate and Refinement Prompts</name>
  <files>~/.config/opencode/commands/speckit/speckit-specify.md</files>
  <action>
Modify `speckit-specify.md` to add:

**A. Template Gate (in template, lines 41-297):**
Add section headers with explicit placeholders that FAIL if empty:
- Keep existing template but add inline validation hints
- Add comment blocks: "<!-- REQUIRED: Minimum X -->" for each required section

**B. Refinement Prompts (after template generation):**
Add new step after spec generation (around line 310):

```
## Step 5b: Refinement Prompts (Auto-Check)

After generating spec, run quick content check:

1. **Overview Check:**
   - Count sentences (should be >=3)
   - If <3 sentences: "Your Overview is [N] sentences. Add more context about the problem and why this matters now."

2. **User Stories Check:**
   - Count "Story:" occurrences (should be >=1)
   - If 0: "Add at least 1 user story with clear actor, action, and benefit."

3. **Acceptance Criteria Check:**
   - Count "AC-" occurrences (should be >=2)
   - If <2: "Add at least 2 acceptance criteria in format 'AC-N: [testable condition]'."

4. **Non-Functional Check:**
   - Search for "performance" OR "security" OR "accessibility" (should find >=1)
   - If 0: "You have 0 Non-Functional requirements. Add at least one: performance (latency/throughput), security (auth/encryption), or accessibility (compliance)."

5. **Out of Scope Check:**
   - Count bullet points under Out of Scope / Non-goals (should be >=1)
   - If 0: "Define at least 1 item in Out of Scope to clarify boundaries."

6. **Dependencies Check:**
   - Check Dependencies section exists (section must exist even if empty)
   - If section missing: "Add a Dependencies section listing external systems."

For each failed check, prompt user with specific question. Do not proceed to planning until all required sections pass.
```

**C. Add Lint Integration:**
Add at end of specify command:
```
## Step 6: Run Spec Lint

After refinement, run:
/speckit.lint [spec-name]

Only proceed to planning if lint returns PASS or WARN.
```
  </action>
  <verify>speckit-specify.md now includes refinement prompts and lint check step</verify>
  <done>Spec generation now enforces complete sections with auto-prompts for thin content</done>
</task>

<task type="auto">
  <name>Task 5: Update speckit-autopilot.md with Lint + Review Steps</name>
  <files>~/.config/opencode/commands/speckit/speckit-autopilot.md</files>
  <action>
Modify `speckit-autopilot.md` to integrate lint and review:

**A. Update Phase 3 (Specification) - Add after Step 3.3:**

```
### Step 3.4: Spec Lint Validation

After generating spec, run lint validation:

```bash
# Run spec lint
speckit.lint [FEATURE_NAME]
```

- If **FAIL**: Go back to Step 3.3, fix missing sections
- If **WARN**: Review warnings, decide if acceptable
- If **PASS**: Proceed to Step 3.5

### Step 3.5: Human Review (5 min)

Run the review checklist:

```bash
speckit.review [FEATURE_NAME]
```

Or manually complete the 5-minute checklist from speckit-review.md.

- If **Needs Refinement**: Return to Step 3.3
- If **Ready**: Proceed to Phase 4
```

**B. Add to Phase 7 (Completion) - Verification Loop:**

```
### Step 7b: Spec Verification Loop

After implementation completes, trace spec to code:

1. For each ACCEPTANCE CRITERIA (AC-1, AC-2, etc.):
   - [ ] Find test(s) that verify this criterion
   - [ ] Mark criterion as "validated: [test file/line]"

2. For each MUST requirement:
   - [ ] Find implementation that satisfies this requirement
   - Mark requirement as "implemented: [file/function]"

3. For each USER STORY:
   - [ ] Verify acceptance criteria cover the story
   - Mark story as "covered by: [AC-1, AC-2]"

Output verification table:

| Spec Section | Status | Evidence |
|--------------|--------|----------|
| AC-1 | validated | tests/integration.spec.ts:42 |
| AC-2 | validated | tests/unit/auth.spec.ts:15 |
| R1-MUST | implemented | src/auth/login.ts:23 |
| Security | validated | tests/security.spec.ts |

If any section is NOT validated/implemented: Backlog item created.
```
  </action>
  <verify>Autopilot workflow now includes lint step, human review, and verification loop</verify>
  <done>Full spec enforcement integrated into autopilot workflow</done>
</task>

</tasks>

<verification>
- [ ] speckit-rubric.md created with required sections and thresholds
- [ ] speckit-lint.md validates all required sections
- [ ] speckit-review.md has 5-minute checklist with explicit questions
- [ ] speckit-specify.md has refinement prompts and lint check
- [ ] speckit-autopilot.md has lint + review + verification in workflow
- [ ] All new commands follow existing speckit command patterns
</verification>

<success_criteria>
- Every generated spec has all 6 required sections
- Spec Lint catches missing sections before planning
- Human reviewers have explicit checklist for missing decisions
- Thin sections trigger specific refinement prompts
- Implemented features can be traced to spec acceptance criteria
</success_criteria>

<output>
After completion, create summary in `~/.config/opencode/commands/speckit/SPEC-ENFORCEMENT-SUMMARY.md`
</output>
