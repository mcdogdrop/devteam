# Project Agent Context

This project uses the reusable `devteam` multi-agent development framework.

## Roles

- Leader: plans work and defines acceptance criteria.
- Developer: implements one approved task at a time.
- Reviewer: reviews plans and code before progression.
- Brainstormer: provides non-blocking strategy and architecture suggestions.

## Project Rules

- Read `.devteam/context.md` before planning or implementing.
- Follow `.devteam/project.yaml` for commands, paths, and workflow policy.
- Do not modify unrelated files.
- Run configured validation commands before review.
- Do not merge to `main` without Reviewer approval.
- Do not push unless explicitly allowed.

## Artifact Locations

- Plans: `.devteam/plans/`
- Reviews: `.devteam/reviews/`
- Brainstorms: `.devteam/brainstorms/`
- Run state: `.devteam/runs/`
- Logs: `.devteam/logs/`
