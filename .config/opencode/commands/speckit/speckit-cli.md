---
description: Use Speckit CLI directly if installed
---

# Speckit CLI Command

This command integrates with the official Speckit CLI if it's installed on your system.

## Step 1: Check for Speckit CLI

```bash
# Check if Speckit CLI is installed
which specify 2>/dev/null && echo "Speckit CLI found" || echo "Speckit CLI not installed"

# Check uv for one-time usage
which uv 2>/dev/null && echo "uv found" || echo "uv not installed"
```

## Step 2: Installation Instructions

If Speckit CLI is not installed, provide instructions:

### Option 1: Install with uv (Recommended)
```bash
# Persistent installation
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# Verify installation
specify --help
```

### Option 2: One-time usage
```bash
# Run without installing
uvx --from git+https://github.com/github/spec-kit.git specify --help
```

## Step 3: Use Speckit CLI

If installed, you can use these commands:

### Initialize a new project:
```bash
specify init my-project --ai opencode
cd my-project
```

### Then use these commands in OpenCode:
```
/speckit.constitution Create principles for code quality
/specify.specify Build a photo gallery app
/specify.plan Use React + TypeScript + Tailwind
/specify.tasks
/specify.implement
```

## Step 4: Manual Setup (If CLI not available)

If you don't want to install the CLI, you can set up manually:

### Create project structure:
```bash
mkdir -p .specify/memory
mkdir -p .specify/specs
mkdir -p .specify/templates
mkdir -p .specify/scripts
```

### Then use OpenCode commands:
- `/speckit.constitution` - Define principles
- `/speckit.specify` - Create specs
- `/speckit.plan` - Plan implementation
- `/speckit.tasks` - Generate tasks
- `/speckit.implement` - Build it

## Step 5: Choose Your Path

Ask the user:
> **Choose your approach:**
> 
> - **A** - Install Speckit CLI (recommended for full experience)
> - **B** - Use OpenCode commands only (manual setup)
> - **C** - Run `/speckit.autopilot` for automated workflow

## Output

Based on user's choice, guide them to the appropriate next step.

## CLI Setup Checklist (Prescriptive)

If using the CLI:
- [ ] `specify --help` runs successfully
- [ ] New project initializes `.specify/` structure
- [ ] OpenCode commands work in the project directory

## Example Flow

```bash
specify init my-project --ai opencode
cd my-project
/speckit.constitution
```
