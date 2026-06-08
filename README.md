# devteam

`devteam` is a reusable multi-agent project development framework. It defines a portable team mechanism that can be attached to different software projects through a `.devteam/` directory.

## Roles

- **Leader** — analyzes requests, breaks work into tasks, writes plans. Default model: Claude Opus 4.7 via Claude Code CLI.
- **Developer** — implements planned tasks, writes tests, validates, and commits. Default model: Claude Sonnet 4.6 via Claude Code CLI.
- **Reviewer** — reviews plans and code as a quality gate. Default model: GPT-5.4 through Hermes.
- **Brainstormer** — proposes strategic, product, architecture, and roadmap ideas. Default model: Gemini 3 Flash Preview, fallback Gemini 3.1 Flash Lite.
- **Orchestrator** — coordinates runs, state, artifacts, prompts, and git workflow. In the intended setup, Hermes is the orchestrator.

## Project Integration

Each target project opts in with:

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

Initialize a project from this repository with the MVP script:

```bash
./scripts/devteam init /path/to/project
```

## MVP Scope

This repository currently contains a documentation-first MVP:

- `docs/architecture.md` — framework architecture
- `docs/workflow.md` — workflow and state machine
- `docs/config-schema.md` — `.devteam/project.yaml` schema
- `docs/role-contracts.md` — structured role input/output contracts
- `docs/cli.md` — proposed CLI surface
- `prompts/` — role prompt templates
- `templates/` — project integration templates
- `scripts/devteam` — lightweight bootstrap/status helper

## Design Principles

1. Framework is project-agnostic.
2. Roles have strict responsibilities.
3. Outputs are Markdown with YAML frontmatter.
4. Developers work on isolated branches or worktrees.
5. Reviewer approval gates merge to main.
6. Brainstormer informs strategy but does not block delivery.

## Suggested v0.1 Flow

```bash
devteam init
devteam plan "Add user authentication"
devteam review-plan <run-id>
devteam develop <run-id> T001
devteam review <run-id> T001
devteam brainstorm <run-id>
```

`devteam run` is intentionally left for a later, safer automation phase.
