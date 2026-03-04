---
description: Quick spec - generate complete spec from a single prompt
---

# Quick Spec Command

Generate a complete specification package from a single description. This is the fastest way to get started.

## Step 1: Get User Input

The user wants to build something. Collect their idea:

```
$ARGUMENTS
```

If no input provided, ask:
> **What do you want to build?** Describe your project or feature.

## Step 2: Parse the Idea

Extract:
1. **Project/Feature name** - What are we building?
2. **Core functionality** - What does it do?
3. **Target users** - Who is it for?
4. **Key features** - What's included?

## Step 3: Set Up Structure

```bash
# Create project name from description
PROJECT_NAME=$(echo "$ARGUMENTS" | head -n1 | slugify)

# Create directories
mkdir -p .specify/memory
mkdir -p .specify/specs/$PROJECT_NAME

echo "Created .specify/specs/$PROJECT_NAME/"
```

## Step 4: Generate Constitution

Create `.specify/memory/constitution.md`:

```markdown
# Constitution

## Overview
This project follows spec-driven development principles with emphasis on code quality, testing, and user experience.

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
**Ratification Date**: $(date +%Y-%m-%d)
**Last Amended**: $(date +%Y-%m-%d)
```

## Step 5: Generate Specification

Create `.specify/specs/$PROJECT_NAME/spec.md`:

```markdown
# [PROJECT_NAME]

## Overview
[Brief description from user's input]

## User Stories

### Story 1: Core Functionality
**As a** user  
**I want to** [use the main feature]  
**So that** [benefit]

**Acceptance Criteria:**
- [ ] Feature works as expected
- [ ] Error states are handled
- [ ] User feedback is provided

[Additional stories based on the idea...]

## Functional Requirements

### REQ-001: Core Feature
**Description**: The main functionality of the application

**Inputs**: User actions, data

**Processing**: Business logic

**Outputs**: Results displayed to user

**Error Handling**: Graceful error messages

## UI/UX Guidelines

- Simple, intuitive interface
- Responsive design for all devices
- Loading states for async operations
- Clear error messages

## Technical Considerations

- Modern web technologies
- Component-based architecture
- API-driven data flow
```

## Step 6: Generate Plan

Create `.specify/specs/$PROJECT_NAME/plan.md`:

```markdown
# Implementation Plan: [PROJECT_NAME]

## Tech Stack Recommendation

### Option A: Modern Web Stack
- Frontend: React + TypeScript
- Styling: Tailwind CSS
- Build: Vite
- State: React Context

### Option B: Full-Stack
- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: SQLite (dev) / PostgreSQL (prod)

## Implementation Steps

### Week 1: Foundation
- Project setup
- Core components
- Basic styling

### Week 2: Core Features
- Main functionality
- Data handling
- API integration

### Week 3: Polish
- Testing
- Error handling
- Performance

## Files to Create

```
src/
├── components/
│   ├── App.tsx
│   └── [Feature].tsx
├── hooks/
├── services/
├── types/
├── App.css
├── main.tsx
└── index.html
```
```

## Step 7: Generate Tasks

Create `.specify/specs/$PROJECT_NAME/tasks.md`:

```markdown
# Tasks: [PROJECT_NAME]

## Setup (Day 1)

### Task 1.1: Initialize Project
- Set up React + TypeScript + Vite
- Configure Tailwind CSS
- Set up project structure

### Task 1.2: Create Base Components
- App shell
- Header/Footer
- Basic layout

## Core Development (Days 2-4)

### Task 2.1: Implement Features
- Build core functionality
- Create components
- Add styling

### Task 2.2: Data Handling
- Set up state management
- Handle user input
- Process data

## Testing & Polish (Day 5)

### Task 3.1: Testing
- Write unit tests
- Test components
- Fix bugs

### Task 3.2: Polish
- Error handling
- Loading states
- Final styling
```

## Step 8: Offer Implementation

After generating all files:

> **Specification complete!** I've created:
> - `.specify/memory/constitution.md` - Project principles
> - `.specify/specs/$PROJECT_NAME/spec.md` - Feature specification
> - `.specify/specs/$PROJECT_NAME/plan.md` - Implementation plan  
> - `.specify/specs/$PROJECT_NAME/tasks.md` - Task breakdown
>
> **Next steps:**
> - Run `/speckit.implement` to start building
> - Or `/speckit.plan` to customize the tech stack

## Example Usage

Input:
```
/quick-spec A task management app with kanban boards
```

Output:
- Creates constitution
- Creates "task-management-app" spec
- Generates spec, plan, and tasks

## Quick Spec Checklist (Prescriptive)

Before presenting results:
- [ ] Constitution includes version and dates
- [ ] Spec includes overview, user stories, acceptance criteria
- [ ] Plan includes executor/stack decision and phases
- [ ] Tasks include dependencies and verification steps

## Example Output Summary

```markdown
Created:
- .specify/memory/constitution.md
- .specify/specs/task-management-app/spec.md
- .specify/specs/task-management-app/plan.md
- .specify/specs/task-management-app/tasks.md
Next: /speckit.plan to refine executor choices
```
