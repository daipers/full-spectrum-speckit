# Codebase Structure

**Analysis Date:** 2026-03-03

## Directory Layout

```
~/.config/opencode/
├── opencode.jsonc          # Provider configuration
├── opencode.json           # Permissions config
├── config.json             # Plugin config
├── settings.json           # User settings
├── agents/                 # Agent definitions
│   ├── gsd-*.md           # GSD system agents
│   └── speckit-autopilot.md
├── command/                # CLI commands
│   └── gsd-*.md
├── commands/
│   ├── gsd/               # GSD commands
│   └── speckit/           # Speckit commands
│       ├── speckit-autopilot.md
│       ├── speckit-specify.md
│       ├── speckit-plan.md
│       ├── speckit-implement.md
│       ├── speckit-constitution.md
│       ├── speckit-lint.md      # NEW
│       ├── speckit-review.md    # NEW
│       ├── speckit-rubric.md    # NEW
│       └── ...
├── get-shit-done/
│   ├── workflows/         # Phase execution workflows
│   ├── templates/         # Plan/summary templates
│   ├── references/       # Documentation
│   └── bin/              # CLI tools
│       └── gsd-tools.cjs
└── hooks/                # OpenCode hooks

.planning/                 # Project execution state
├── STATE.md              # Current state
├── ROADMAP.md            # Project roadmap
├── REQUIREMENTS.md       # Traceability
└── phases/
    └── speckit-spec-enforcement/
        ├── SPEC-ENFORCEMENT-01-PLAN.md
        ├── SPEC-ENFORCEMENT-01-SUMMARY.md
        └── SPEC-ENFORCEMENT-01-VERIFICATION.md
```

## Directory Purposes

**`~/.config/opencode/commands/speckit/`:**
- Purpose: Spec-driven development commands
- Contains: Markdown command files with workflow instructions
- Key files: `speckit-autopilot.md`, `speckit-specify.md`, `speckit-plan.md`, `speckit-lint.md`, `speckit-review.md`, `speckit-rubric.md`

**`~/.config/opencode/get-shit-done/`:**
- Purpose: GSD project management system
- Contains: Workflows, templates, CLI tools
- Key files: `execute-phase.md`, `execute-plan.md`, `verify-phase.md`

**`~/.config/opencode/agents/`:**
- Purpose: Subagent definitions
- Contains: Agent config with tools and model
- Key files: `gsd-executor.md`, `gsd-verifier.md`, `speckit-autopilot.md`

**`.planning/`:**
- Purpose: Project execution tracking
- Contains: Phase plans, summaries, verification
- Key files: `STATE.md`, `ROADMAP.md`

## Key File Locations

**Entry Points:**
- `~/.config/opencode/commands/speckit/speckit-autopilot.md` - Main workflow
- `~/.config/opencode/get-shit-done/workflows/execute-phase.md` - Phase execution

**Configuration:**
- `~/.config/opencode/opencode.jsonc` - Models and providers
- `~/.config/opencode/agents/speckit-autopilot.md` - Agent config

**Core Logic:**
- `~/.config/opencode/get-shit-done/bin/gsd-tools.cjs` - CLI tools

**Testing:**
- No formal test directory - execution verified via verification agents

## Naming Conventions

**Files:**
- Speckit commands: `speckit-{command}.md`
- GSD commands: `gsd-{command}.md`
- GSD agents: `gsd-{agent}.md`
- Phase plans: `{PHASE}-{NN}-PLAN.md`
- Phase summaries: `{PHASE}-{NN}-SUMMARY.md`
- Phase verification: `{PHASE}-{NN}-VERIFICATION.md`

**Directories:**
- Commands: lowercase with hyphens
- Phases: kebab-case phase name

## Where to Add New Code

**New Speckit Command:**
- Location: `~/.config/opencode/commands/speckit/speckit-{name}.md`
- Template: Use `speckit-plan.md` as reference

**New GSD Workflow:**
- Location: `~/.config/opencode/get-shit-done/workflows/{name}.md`
- Template: Use `execute-plan.md` as reference

**New Agent:**
- Location: `~/.config/opencode/agents/{name}.md`
- Frontmatter: Define `description`, `mode`, `tools`

**New Phase:**
- Location: `.planning/phases/{phase-name}/`
- Files: `{plan-id}-PLAN.md`, summary, verification

## Special Directories

**`.planning/`:**
- Purpose: Project execution state
- Generated: Yes (by GSD tools)
- Committed: Yes (to git)

**`~/.config/opencode/get-shit-done/bin/`:**
- Purpose: Node.js CLI tools
- Generated: Yes
- Committed: Yes

---

*Structure analysis: 2026-03-03*
