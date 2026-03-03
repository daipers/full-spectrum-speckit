# Architecture

**Analysis Date:** 2026-03-03

## Pattern Overview

**Overall:** Multi-layer command system with autonomous agents and phase-based execution

**Key Characteristics:**
- Command-based workflow execution (OpenCode commands)
- Agent-driven autonomous execution (subagents)
- Phase-based project management (GSD system)
- Spec-driven development with validation gates
- Wave-based parallel plan execution

## Layers

**Layer 1: OpenCode Infrastructure:**
- Purpose: Base execution platform providing agent orchestration
- Location: `~/.config/opencode/`
- Contains: Commands, agents, workflows, templates, references
- Key files:
  - `opencode.jsonc` - Provider configuration (OpenAI, Anthropic)
  - `agents/*.md` - Agent definitions
  - `command/*.md` - CLI command definitions

**Layer 2: Speckit Commands:**
- Purpose: Spec-driven development workflow system
- Location: `~/.config/opencode/commands/speckit/`
- Contains: Specification, planning, implementation commands
- Key files:
  - `speckit-autopilot.md` - Full automated workflow (lines 1-707)
  - `speckit-specify.md` - Spec generation
  - `speckit-plan.md` - Implementation planning
  - `speckit-implement.md` - Code implementation
  - `speckit-constitution.md` - Core principles
  - `speckit-lint.md` - Spec validation (new)
  - `speckit-review.md` - Human review checklist (new)
  - `speckit-rubric.md` - Quality standards (new)

**Layer 3: GSD (Get Sh*t Done) Workflows:**
- Purpose: Project and milestone management system
- Location: `~/.config/opencode/get-shit-done/`
- Contains: Workflows for planning, execution, verification
- Key directories:
  - `workflows/` - Phase execution workflows
  - `templates/` - Plan/summary/verification templates
  - `references/` - Documentation references
  - `bin/` - CLI tools (gsd-tools.cjs)

**Layer 4: Execution Layer:**
- Purpose: Run plans in phases with wave-based parallelization
- Location: `.planning/`
- Contains: Phase plans, summaries, verification reports
- Key files:
  - `STATE.md` - Current execution state
  - `ROADMAP.md` - Project roadmap
  - `phases/*/` - Phase directories

## Data Flow

**Speckit Workflow:**

1. `/speckit.specify` → Creates SPEC.md from requirements
2. `/speckit.plan` → Creates implementation plan from spec
3. `/speckit.tasks` → Breaks plan into executable tasks
4. `/speckit.implement` → Executes tasks and writes code
5. `/speckit.lint` → Validates spec completeness (new)
6. `/speckit.review` → Human review checklist (new)

**GSD Phase Execution:**

1. `init` → Load phase context, models, config
2. `discover_plans` → Find incomplete plans, group by wave
3. `execute_waves` → Spawn executor agents per wave
4. `verify_phase` → Run verifier agent to check goals
5. `update_roadmap` → Mark phase complete, advance state

## Key Abstractions

**Command:**
- Purpose: User-invokable workflow unit
- Examples: `speckit-autopilot.md`, `speckit-specify.md`
- Pattern: Markdown with frontmatter, step-by-step instructions

**Agent:**
- Purpose: Autonomous subagent for specialized tasks
- Examples: `gsd-executor`, `gsd-verifier`, `speckit-autopilot`
- Pattern: Subagent definition with tools and model

**Phase:**
- Purpose: Logical grouping of related plans
- Examples: `speckit-spec-enforcement`
- Pattern: Directory with PLAN.md, SUMMARY.md, VERIFICATION.md

**Wave:**
- Purpose: Group of plans that can execute in parallel
- Pattern: Defined in phase plan frontmatter (`wave: N`)

## Entry Points

**User Commands:**
- `/speckit.*` - Speckit development commands
- `/gsd-*` - GSD management commands

**Orchestrator Entry:**
- `gsd-tools.cjs init` - Initialize phase context
- `gsd-tools.cjs phase-plan-index` - Get plan inventory
- `gsd-tools.cjs phase complete` - Mark phase done

## Error Handling

**Strategy:** Checkpoint-based with user interaction

**Patterns:**
- Checkpoint plans (`autonomous: false`) pause for user input
- Agent returns structured state on checkpoint
- Continuation agent resumes from last completed task
- Verification creates gap plans for failed items

## Cross-Cutting Concerns

**Logging:** Structured markdown in phase directories
**Validation:** Spec lint, verification agents, gap detection
**Authentication:** OAuth-based provider models (configured in opencode.jsonc)
**State Management:** STATE.md tracks position, GSD tools update atomically

---

*Architecture analysis: 2026-03-03*
