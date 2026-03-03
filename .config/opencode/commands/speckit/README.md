# Speckit Commands for OpenCode

This directory contains Speckit spec-driven development commands for OpenCode, focused on best-in-class workflow specification development.

## Philosophy

Specs are **operational contracts** that can be validated automatically. Key principles:

- **Validation replaces code review**: If a spec can't drive scenario-based validation, it's incomplete
- **Required sections**: Every spec must include inputs/outputs, error handling, retries, idempotency, security, observability, and rollback plans
- **RFC 2119 semantics**: MUST/SHOULD/MAY keywords are binding
- **Schema validation**: JSON Schema Draft 2020-12 for machine validation

## Available Commands

| Command | Description |
|---------|-------------|
| `/speckit-help` | Overview of Speckit workflow |
| `/speckit-init` | Initialize a new Speckit project |
| `/speckit-constitution` | Define project principles (best-in-class standards) |
| `/speckit-specify` | Create best-in-class workflow specification |
| `/speckit-plan` | Create implementation plan (executor-specific) |
| `/speckit-tasks` | Generate task breakdown with validation tasks |
| `/speckit-implement` | Execute implementation |
| `/speckit-clarify` | Clarify ambiguities |
| `/speckit-analyze` | Analyze spec-implementation consistency |
| `/speckit-checklist` | Best-in-class quality checklist |

## Quick Start

1. `/speckit-init` - Create a new Speckit project
2. `/speckit-constitution` - Define project principles
3. `/speckit-specify` - Describe what to build (workflow/pipeline/automation)
4. `/speckit-plan` - Plan the implementation
5. `/speckit-tasks` - Break into tasks with validation
6. `/speckit-implement` - Build it!

## Required Spec Sections

| Section | Purpose |
|---------|---------|
| Overview | Problem, solution, outcomes |
| Purpose and scope | In/out of scope, blast radius |
| Inputs/outputs | JSON Schema with examples |
| Workflow steps | Step graph, timeouts, retries, idempotency |
| Error handling | Failure taxonomy, retry policies |
| Security | Identity model, least privilege |
| Secrets handling | Sources, injection, rotation |
| Observability | Logs, metrics, traces, correlation |
| SLAs/SLOs | Explicit targets, alerting |
| Rollout/rollback | Deployment strategy, revert procedures |
| Testing | Unit, integration, e2e, chaos |
| Acceptance criteria | Testable conditions |

## Documentation

See [Spec Kit GitHub Repo](https://github.com/github/spec-kit) for additional context.
