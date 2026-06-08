# devteam Workflow

## v0.1 Workflow

The first supported workflow is deliberately semi-automatic:

```text
User Request
    |
    v
Leader Plan
    |
    v
Reviewer Plan Review
    |
    v
Developer Task Implementation
    |
    v
Developer Validation
    |
    v
Reviewer Code Review
    |
    +--> Changes Requested --> Developer Fix
    |
    v
Task Approved
    |
    v
All Tasks Done
    |
    v
Brainstormer Retrospective / Strategy Notes
```

## State Machine

Each run writes state to:

```text
.devteam/runs/<run-id>/state.yaml
```

Allowed run statuses:

```text
new_request
planning
plan_review
plan_revision
ready_for_development
implementing_task
testing
code_review
changes_requested
task_approved
all_tasks_done
brainstorming
done
failed
blocked
```

## Example State File

```yaml
version: 0.1
run_id: 20260608-add-auth
request: Add user authentication
status: code_review
created_at: 2026-06-08T10:30:00Z
updated_at: 2026-06-08T11:20:00Z

project:
  root: /path/to/project
  config: .devteam/project.yaml

git:
  main_branch: main
  branch: feat/20260608-add-auth
  base_commit: abc1234
  current_commit: def5678

artifacts:
  plan: .devteam/plans/20260608-add-auth.md
  plan_review: .devteam/reviews/20260608-add-auth-plan-review.md
  brainstorm: null

tasks:
  - id: T001
    title: Create User model
    status: approved
    implementation: .devteam/runs/20260608-add-auth/tasks/T001/implementation.md
    review: .devteam/reviews/20260608-add-auth-T001-code-review.md
  - id: T002
    title: Add login endpoint
    status: implementing
    implementation: null
    review: null
```

## Gate Rules

### Plan Gate

If `workflow.require_plan_review` is true, Developer must not begin until Reviewer returns `APPROVE` for the plan.

### Code Gate

If `workflow.require_code_review` is true, each task must pass code review before it is considered approved.

### Merge Gate

If `workflow.require_review_before_merge` is true, no merge to `main` happens without Reviewer approval.

## Fix Loop

`workflow.max_fix_iterations` controls how many times Developer may attempt to fix review findings before the run becomes `blocked` and asks for user guidance.

Recommended default:

```yaml
workflow:
  max_fix_iterations: 3
```

## Brainstorming

Brainstormer runs after completion by default if:

```yaml
workflow:
  brainstorm_after_completion: true
```

Brainstorming is advisory and non-blocking.
