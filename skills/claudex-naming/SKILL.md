---
name: claudex-naming
description: Naming conventions for features, branches, docs, and commits
---

# Feature Naming Conventions

## Feature IDs

Features are identified by a zero-padded 4-digit ID allocated from `.claudex/counter` at repo root.

| Example ID | Counter file value before |
|-----------|--------------------------|
| `0001`    | 1                        |
| `0012`    | 12                       |

The ID is allocated by `claudex new`. Allocation is a pure file write to `.claudex/counter` — no git commit.

## Naming Table

| Asset         | Pattern                                | Example                          |
|---------------|----------------------------------------|----------------------------------|
| Full name     | `<id>-<slug>`                          | `0003-auth-webhooks`             |
| Git branch    | `feature/<id>-<slug>`                  | `feature/0003-auth-webhooks`     |
| Docs dir      | `docs/features/<id>-<slug>/`           | `docs/features/0003-auth-webhooks/` |
| Config file   | `docs/features/<id>-<slug>/config.json`| (see below)                      |
| Commit prefix | `feat: <description> (#<id>)`          | `feat: plan auth-webhooks (#0003)` |
| PR title      | `feat: <slug> (#<id>)`                 | `feat: auth-webhooks (#0003)`    |

## Slug Rules

- kebab-case only: `[a-z0-9]+(-[a-z0-9]+)*`
- Provided by the user when running `claudex new <slug>`
- Never auto-derived — the user picks the name

## Commit Convention

- Default to `feat:` without scope
- Never prompt for scope — keep it simple
- Include feature ID in parentheses at end: `(#<id>)`

## config.json

Created by `claudex new` inside `docs/features/<id>-<slug>/`. This is the single source of truth for all downstream skills.

```json
{
  "featureId": "0003",
  "slug": "auth-webhooks",
  "fullName": "0003-auth-webhooks",
  "worktreePath": "/path/to/worktrees/feature/0003-auth-webhooks",
  "gitBranch": "feature/0003-auth-webhooks",
  "docsDir": "docs/features/0003-auth-webhooks",
  "tmux": {
    "session": "myproject",
    "window": "f0003",
    "panes": {
      "architect": "%42",
      "implementor": "%43",
      "shell": "%44"
    }
  },
  "architectSessionId": null,
  "prUrl": null
}
```

`tmux` is `null` when `--tmux` was not used. `architectSessionId` and `prUrl` are filled in by `/claudex-plan`.
