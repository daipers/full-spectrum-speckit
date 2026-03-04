---
description: Initialize a GitHub repository with Speckit structure for spec-driven development
---

# GitHub Repo Init Command

You are helping the user create a new GitHub repository with Speckit structure.

## Step 1: Check Prerequisites

Verify GitHub CLI is available:
```bash
which gh || echo "GitHub CLI not found"
gh --version
```

## Step 2: Gather Information

Ask the user for:
1. Repository name
2. Description
3. Visibility (public/private)
4. GitHub username/org

## Step 3: Create GitHub Repository

```bash
# Example:
gh repo create my-spec-project --public --description "A spec-driven development project" --clone
```

Or using GitHub token approach:
```bash
# Set up token if needed
export GH_TOKEN="your_token"

# Create repo (without cloning)
gh repo create $REPO_NAME --$VISIBILITY --description "$DESCRIPTION" --source=. --push
```

## Step 4: Initialize Speckit Structure

After creating the repo, set up Speckit:

```bash
cd $REPO_NAME

# Create Speckit directories
mkdir -p .specify/memory
mkdir -p .specify/specs
mkdir -p .specify/scripts

# Create constitution
cat > .specify/memory/constitution.md << 'EOF'
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

---

**Version**: 1.0.0
**Ratification Date**: [DATE]
**Last Amended**: [DATE]
EOF

# Create quickstart
cat > .specify/quickstart.md << 'EOF'
# Quickstart

## Workflow

1. `/speckit.constitution` - Define principles
2. `/speckit.specify` - Create specs
3. `/speckit.plan` - Plan implementation
4. `/speckit.tasks` - Break into tasks
5. `/speckit.implement` - Build it!

## Commands

| Command | Purpose |
|---------|---------|
| /speckit.constitution | Define project principles |
| /speckit.specify | Create feature specification |
| /speckit.plan | Create implementation plan |
| /speckit.tasks | Generate task breakdown |
| /speckit.implement | Execute implementation |
| /speckit.clarify | Clarify ambiguities |
| /speckit.analyze | Analyze consistency |
| /speckit.checklist | Quality checklist |
EOF

# Create .gitignore
cat > .gitignore << 'EOF'
node_modules/
.env
.env.local
dist/
build/
*.log
.DS_Store
EOF

# Create README
cat > README.md << 'EOF'
# [PROJECT_NAME]

[Description]

## Development

This project uses Spec-Driven Development. See `.specify/quickstart.md` for workflow.
EOF
```

## Step 5: Commit and Push

```bash
git add .
git commit -m "feat: initialize Speckit project structure"
git push origin main
```

## Output

Provide:
1. Repository URL
2. Next steps
3. How to use Speckit commands

## GitHub Setup Checklist (Prescriptive)

Before finishing:
- [ ] `gh` is authenticated and repo created
- [ ] `.specify/` structure exists in repo
- [ ] Constitution and quickstart created
- [ ] README references the Speckit workflow
- [ ] Initial commit pushed to `main`

## Example Follow-up

```markdown
Repo ready. Next:
1. /speckit.constitution
2. /speckit.specify Build a CI pipeline
```
