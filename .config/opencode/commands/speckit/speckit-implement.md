---
description: Execute implementation tasks from the task breakdown
---

# Implement Command

You are executing the implementation tasks for a feature.

## Step 1: Identify the Feature

From user input:
```
$ARGUMENTS
```

Check available tasks:
```bash
ls .specify/specs/*/tasks.md
```

## Step 2: Read the Tasks

Read the tasks file:
```bash
cat .specify/specs/[FEATURE_NAME]/tasks.md
```

Also read the plan and spec for context:
```bash
cat .specify/specs/[FEATURE_NAME]/plan.md
cat .specify/specs/[FEATURE_NAME]/spec.md
```

## Step 3: Execute Tasks in Order

For each task, follow this process:

### Before Starting a Task
1. Read the task description carefully
2. Check dependencies are complete
3. Understand the file structure

### During Implementation
1. Create/modify the specified files
2. Follow the tech stack from the plan
3. Implement according to the spec
4. Write tests as you go (TDD recommended)

### After Completing a Task
1. Verify the implementation against the task
2. Test the functionality
3. Check for errors/warnings

### Parallel Execution (Subagents)

If tasks have no dependencies and touch separate files/subsystems, execute them in parallel waves using subagents.

Rules:
- Build execution waves from the dependency graph in `tasks.md`
- Only parallelize within a wave
- Avoid parallel work that modifies the same files or closely-coupled modules

## Step 4: Progress Tracking

Mark tasks as complete by updating the tasks.md:

```markdown
#### Task X.X: [Task Name]
**Status**: ✅ Complete
**Completed**: [Date]
```

## Step 5: Handle Issues

If you encounter problems:
1. Document the issue
2. Research solutions
3. Try alternative approaches
4. If blocked, ask user for guidance

## Execution Guidelines

### Code Quality
- Follow consistent formatting
- Add comments for complex logic
- Use meaningful variable names

### Testing
- Write unit tests for new functionality
- Test edge cases
- Verify error handling

### Git Workflow
- Create a feature branch: `git checkout -b feature/[name]`
- Commit regularly with clear messages
- Push and create PR when complete

## Step 6: Completion

When all tasks are complete:
1. Verify the feature works end-to-end
2. Run the full test suite
3. Check for any remaining issues
4. Summarize what was built

## Output

Provide progress updates:
- Tasks completed: X/Y
- Current task: [Name]
- Issues encountered (if any)
