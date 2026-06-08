# devteam Architecture

## Purpose

`devteam` is a reusable multi-agent development framework for coordinating planning, implementation, review, and strategic brainstorming across different software projects.

It is not tied to a single repository. A project opts in by adding a `.devteam/` directory containing configuration, context, run state, and generated artifacts.

## Layers

```text
Global Team Framework
        |
        v
Project Adapter
        |
        v
Runtime Orchestrator
```

### Global Team Framework

Defines shared defaults:

- role definitions
- role prompts
- workflow rules
- default model assignments
- artifact formats
- CLI conventions

### Project Adapter

Each project provides:

- `.devteam/project.yaml` — project-specific configuration
- `.devteam/context.md` — long-lived project context
- optional project-specific overrides for commands, paths, and workflow policies

### Runtime Orchestrator

The orchestrator coordinates roles, model calls, artifacts, git status, and workflow state.

The intended orchestrator is Hermes. The default worker mapping is:

| Role | Default runner | Default model |
|---|---|---|
| Leader | Claude Code CLI | Claude Opus 4.7 |
| Developer | Claude Code CLI | Claude Sonnet 4.6 |
| Reviewer | Hermes/OpenAI | GPT-5.4 |
| Brainstormer | Gemini CLI/API | Gemini 3 Flash Preview |

## Responsibilities

### Leader

The Leader produces plans and acceptance criteria. It may inspect context and git history but should not implement code by default.

### Developer

The Developer implements one planned task at a time, validates the result, and commits when validation passes.

### Reviewer

The Reviewer reviews plans and code. It is the quality gate for progression and merge readiness.

### Brainstormer

The Brainstormer generates non-blocking strategic and architectural insights.

### Orchestrator

The Orchestrator:

- loads project configuration
- builds prompts
- calls the appropriate model runners
- saves raw and structured outputs
- maintains `.devteam/runs/<run-id>/state.yaml`
- checks git status
- enforces workflow gates

## Artifact Strategy

All durable outputs use Markdown with YAML frontmatter. This keeps artifacts human-readable and machine-parseable.

Artifacts live under:

```text
.devteam/plans/
.devteam/reviews/
.devteam/brainstorms/
.devteam/runs/
.devteam/logs/
```

## Git Strategy

Developer work must not occur directly on `main` unless explicitly allowed.

v0.1 default:

```text
main
  |
  +-- feat/<run-id>
```

v0.2 target:

```text
git worktree per task
```

## Safety Defaults

- `auto_commit: true`
- `auto_push: false`
- `require_plan_review: true`
- `require_code_review: true`
- `require_review_before_merge: true`
- `devteam run` deferred until single-step commands are stable
