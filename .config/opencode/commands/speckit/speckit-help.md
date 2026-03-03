---
description: Overview and help for Speckit spec-driven development commands
---

# Speckit Help

You are helping the user understand the Speckit spec-driven development workflow.

## Overview

Speckit is GitHub's toolkit for Spec-Driven Development (SDD). It provides a structured workflow for building software through:

1. **Constitution** - Define project principles
2. **Specification** - Describe what to build
3. **Planning** - Design the implementation
4. **Tasks** - Break into actionable items
5. **Implementation** - Build the feature

## Available Commands

### Core Workflow

| Command | Description |
|---------|-------------|
| `/speckit.init` | Initialize a new Speckit project |
| `/speckit.constitution` | Create/update project principles |
| `/speckit.specify` | Define a feature specification |
| `/speckit.plan` | Create technical implementation plan |
| `/speckit.tasks` | Generate actionable task list |
| `/speckit.implement` | Execute implementation |

### Quality Assurance

| Command | Description |
|---------|-------------|
| `/speckit.lint` | Validate spec completeness |
| `/speckit.review` | Human review checklist (5-min) |
| `/speckit.rubric` | Quality standards reference |

### GSD Integration

| Command | Description |
|---------|-------------|
| `/speckit.gsd` | Bridge spec to GSD phase execution |

### Supporting Commands

| Command | Description |
|---------|-------------|
| `/speckit.clarify` | Clarify ambiguous requirements |
| `/speckit.analyze` | Check spec/plan/task consistency |
| `/speckit.checklist` | Quality checklist for specs |

## Getting Started

### Option 1: New Project

```bash
# Create new project directory
mkdir my-project
cd my-project

# Initialize git
git init

# Then use OpenCode commands:
/speckit.constitution
```

### Option 2: Existing Project

Add Speckit structure to any project:

```bash
mkdir -p .specify/memory
mkdir -p .specify/specs

# Then start with:
/speckit.constitution
```

## Workflow Example

```
1. /speckit.constitution
   → Define: "We prioritize code quality and testing"

2. /speckit.specify
   → Describe: "Build a photo album app with drag-drop reordering"

3. /speckit.plan  
   → Tech stack: "React + TypeScript + SQLite"

4. /speckit.tasks
   → Break into: Setup, Models, UI, Integration, Tests

5. /speckit.implement
   → Execute each task
```

## Project Structure

```
project/
├── .specify/
│   ├── memory/
│   │   └── constitution.md    # Project principles
│   ├── specs/
│   │   └── 001-feature/
│   │       ├── spec.md         # Feature specification
│   │       ├── plan.md         # Implementation plan
│   │       ├── tasks.md        # Task breakdown
│   │       ├── clarifications.md
│   │       ├── analysis.md
│   │       └── checklist.md
│   └── quickstart.md
├── src/
└── README.md
```

## Tips

- **Start with constitution** - Principles guide all decisions
- **Be specific in specs** - Clear requirements = better code
- **Use clarify** - Resolve ambiguities before planning
- **Plan first** - Technical decisions affect everything
- **Task small** - Smaller tasks = faster progress

## Learn More

- [Spec-Driven Development](https://github.github.com/spec-kit/)
- [Speckit GitHub Repo](https://github.com/github/spec-kit)

## Next Step

To start a new Speckit project:
```
/speckit.init
```

To add Speckit to existing project:
```
/speckit.constitution
```
