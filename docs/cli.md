# CLI Design

The v0.1 CLI is intentionally semi-automatic. Full automation is deferred until the individual steps are reliable.

## Commands

### `devteam init`

Initialize `.devteam/` in a project.

```bash
devteam init
devteam init /path/to/project
```

Creates:

```text
.devteam/
├── project.yaml
├── context.md
├── runs/
├── plans/
├── reviews/
├── brainstorms/
└── logs/
```

### `devteam status`

Show project integration and recent run state.

```bash
devteam status
devteam status <run-id>
```

### `devteam plan`

Ask Leader to produce a plan.

```bash
devteam plan "Add user authentication"
```

Expected artifacts:

```text
.devteam/plans/<run-id>.md
.devteam/runs/<run-id>/state.yaml
```

### `devteam review-plan`

Ask Reviewer to review a plan.

```bash
devteam review-plan <run-id>
```

### `devteam develop`

Ask Developer to implement a task.

```bash
devteam develop <run-id> T001
devteam develop <run-id> --next
```

### `devteam review`

Ask Reviewer to review a task implementation.

```bash
devteam review <run-id> T001
```

### `devteam fix`

Ask Developer to fix review findings.

```bash
devteam fix <run-id> T001
```

### `devteam brainstorm`

Ask Brainstormer for non-blocking strategy or architecture advice.

```bash
devteam brainstorm <run-id>
devteam brainstorm "What should this project become in 3 months?"
```

### `devteam run`

Future full automation command.

```bash
devteam run "Add user authentication"
```

v0.1 should either omit this command or make it print a warning explaining that full automation is not implemented yet.

## Exit Codes

Recommended future exit code meanings:

| Code | Meaning |
|---:|---|
| 0 | Success |
| 1 | User/config error |
| 2 | Missing prerequisite |
| 3 | Workflow blocked |
| 4 | Model runner failed |
| 5 | Validation failed |

## MVP Script

The current `scripts/devteam` helper supports only bootstrap/status behavior. It is not yet the full orchestrator.
