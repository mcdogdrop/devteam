# Role Contracts

Every devteam role produces Markdown with YAML frontmatter so humans can read it and tooling can parse it.

## Common Frontmatter Fields

```yaml
type: plan | implementation_result | plan_review | code_review | brainstorm
version: 0.1
run_id: example-run-id
author_role: leader | developer | reviewer | brainstormer
model: model-name
status: status-name
```

## Leader Contract

### Input

```yaml
type: leader_request
project_root: /path/to/project
request: User request
context_files:
  - .devteam/context.md
  - README.md
  - CLAUDE.md
constraints:
  - Do not implement code.
  - Produce only a plan.
```

### Output

Saved to:

```text
.devteam/plans/<run-id>.md
```

```md
---
type: plan
version: 0.1
run_id: 20260608-add-auth
status: ready_for_review
author_role: leader
model: claude-opus-4.7
---

# Plan: Add User Authentication

## Goal

...

## Context

...

## Assumptions

...

## Architecture

...

## Tasks

### T001: Create User model

**Owner:** Developer

**Depends on:** None

**Files Expected:**
- `src/models/user.py`
- `tests/test_user_model.py`

**Acceptance Criteria:**
- User has id, email, created_at fields.
- Email is unique.

**Validation Commands:**

```bash
uv run pytest tests/test_user_model.py
```

## Risks

...

## Handoff to Developer

...
```

## Developer Contract

### Input

```yaml
type: developer_task
run_id: 20260608-add-auth
task_id: T001
plan_file: .devteam/plans/20260608-add-auth.md
branch: feat/20260608-add-auth
acceptance_criteria:
  - ...
validation_commands:
  - ...
```

### Output

Saved to:

```text
.devteam/runs/<run-id>/tasks/<task-id>/implementation.md
```

```md
---
type: implementation_result
version: 0.1
run_id: 20260608-add-auth
task_id: T001
status: implemented
author_role: developer
model: claude-sonnet-4.6
commit: 12f0af8
---

# Implementation Result: T001

## Summary

...

## Files Changed

- `src/models/user.py`
- `tests/test_user_model.py`

## Validation

| Command | Result |
|---|---|
| `uv run pytest tests/test_user_model.py` | passed |

## Notes

...
```

## Reviewer Contract

Reviewer decisions must be exactly one of:

```text
APPROVE
REQUEST_CHANGES
BLOCK
```

### Plan Review Output

Saved to:

```text
.devteam/reviews/<run-id>-plan-review.md
```

```md
---
type: plan_review
version: 0.1
run_id: 20260608-add-auth
decision: approve
author_role: reviewer
model: gpt-5.4
---

# Plan Review

## Decision

APPROVE

## Spec Clarity

...

## Missing Requirements

...

## Risks

...

## Required Changes

None.

## Optional Suggestions

...
```

### Code Review Output

Saved to:

```text
.devteam/reviews/<run-id>-<task-id>-code-review.md
```

```md
---
type: code_review
version: 0.1
run_id: 20260608-add-auth
task_id: T001
decision: request_changes
author_role: reviewer
model: gpt-5.4
---

# Code Review: T001

## Decision

REQUEST_CHANGES

## Spec Compliance

...

## Code Quality

...

## Tests

...

## Security

...

## Required Changes

1. Add test for invalid email.

## Optional Suggestions

...
```

## Brainstormer Contract

### Output

Saved to:

```text
.devteam/brainstorms/<run-id>-brainstorm.md
```

```md
---
type: brainstorm
version: 0.1
run_id: 20260608-add-auth
topic: roadmap
author_role: brainstormer
model: gemini-3-flash-preview
---

# Brainstorm: Roadmap

## Strategic Opportunities

...

## Architecture Ideas

...

## Product Risks

...

## Technical Risks

...

## Experiments

...

## Recommended Next Questions

...
```
