---
description: Initialize a new Speckit project with spec-driven development structure
---

# Speckit Init Command

You are setting up a new Speckit project for spec-driven development.

## Step 1: Gather Project Information

Ask the user for:
1. Project name
2. Project description/goal
3. Tech stack preferences (optional for now)

## Step 2: Create Project Structure

Create the directory structure:

```bash
PROJECT_NAME="[YOUR_PROJECT]"

# Create main directories
mkdir -p "$PROJECT_NAME"
cd "$PROJECT_NAME"

# Create .specify structure
mkdir -p .specify/memory
mkdir -p .specify/specs
mkdir -p .specify/templates
mkdir -p .specify/scripts
```

## Step 3: Initialize Git Repository

```bash
git init
git checkout -b main
```

## Step 4: Create Constitution Template

Create `.specify/memory/constitution.md`:

```markdown
# Constitution

## Overview
[PROJECT_NAME] is committed to [mission]. This constitution establishes the principles that guide all development.

## Principles

### 1. Code Quality
- **Standard**: All code must be readable, testable, and maintainable
- **Rationale**: Technical debt accumulates quickly without clear standards

### 2. Testing  
- **Standard**: All features require automated tests before merge
- **Rationale**: Tests provide confidence and enable safe refactoring

### 3. User Experience
- **Standard**: Consistent, accessible, and performant UI/UX
- **Rationale**: Users expect professional, polished experiences

### 4. Performance
- **Standard**: Optimize for responsiveness and efficiency
- **Rationale**: Performance directly impacts user satisfaction

### 5. Security
- **Standard**: Security is never optional
- **Rationale**: Protecting user data is non-negotiable

## Governance

### Amendment Process
1. Propose change with rationale
2. Gather team feedback
3. Update version number
4. Document the change

### Versioning
- MAJOR: Backward incompatible changes
- MINOR: New principles or expanded guidance
- PATCH: Clarifications and refinements

---

**Version**: 1.0.0
**Ratification Date**: [DATE]
**Last Amended**: [DATE]
```

## Step 5: Create Quickstart Guide

Create `.specify/quickstart.md`:

```markdown
# Quickstart: [PROJECT_NAME]

## Getting Started

1. **Establish Principles**: Use `/speckit.constitution`
2. **Define Feature**: Use `/speckit.specify`
3. **Plan Implementation**: Use `/speckit.plan`
4. **Break into Tasks**: Use `/speckit.tasks`
5. **Implement**: Use `/speckit.implement`

## Available Commands

| Command | Purpose |
|---------|---------|
| `/speckit.constitution` | Define project principles |
| `/speckit.specify` | Create feature specification |
| `/speckit.clarify` | Clarify ambiguities |
| `/speckit.plan` | Create implementation plan |
| `/speckit.tasks` | Generate task breakdown |
| `/speckit.implement` | Execute implementation |
| `/speckit.analyze` | Analyze consistency |
| `/speckit.checklist` | Quality checklist |

## Project Structure

```
.
├── .specify/
│   ├── memory/
│   │   └── constitution.md
│   ├── specs/
│   │   └── [feature-name]/
│   │       ├── spec.md
│   │       ├── plan.md
│   │       └── tasks.md
│   └── quickstart.md
└── [project files]
```

## Next Steps

1. Run `/speckit.constitution` to establish project principles
2. Run `/speckit.specify` to define your first feature
```

## Step 6: Update OpenCode Configuration

Create or update `.opencode/SPEC.md` in the project:

```markdown
# [PROJECT_NAME] - OpenCode Spec-Driven Development Project

This project uses Spec-Driven Development with GitHub Speckit.

## Workflow

1. Constitution → 2. Specify → 3. Plan → 4. Tasks → 5. Implement

## Available Commands

Use these commands in OpenCode:
- `/speckit.constitution` - Define principles
- `/speckit.specify` - Create specs
- `/speckit.plan` - Plan implementation
- `/speckit.tasks` - Break into tasks
- `/speckit.implement` - Build it
```

## Step 7: Create README

Create initial `README.md`:

```markdown
# [PROJECT_NAME]

[Description]

## Development

This project uses Spec-Driven Development. See `.specify/quickstart.md` for workflow.
```

## Step 8: Commit Initial State

```bash
git add .
git commit -m "feat: initialize Speckit project structure"
```

## Output

Provide:
1. Project created at `./[PROJECT_NAME]/`
2. Directory structure explained
3. Next steps for user
4. How to use Speckit commands

## Init Checklist (Prescriptive)

Before finishing init:
- [ ] `.specify/memory/constitution.md` exists and has version metadata
- [ ] `.specify/specs/` exists and is empty (ready for first spec)
- [ ] `.specify/quickstart.md` links to all core commands
- [ ] README references `.specify/quickstart.md`
- [ ] Git repo initialized on main branch

## Example Quickstart Snippet

```markdown
1. /speckit.constitution
2. /speckit.specify Build a deploy pipeline
3. /speckit.plan Use GitHub Actions
4. /speckit.tasks
5. /speckit.implement
```
