---
name: claudex-build
description: Run an agent team to implement a planned feature
---

# Claudex — Build

Execute a feature's implementation plan using an agent team.

## Gate Check

Read `config.json` from the feature docs directory:
```bash
FULL_NAME=$(basename "$(git rev-parse --show-toplevel)")
CONFIG="docs/features/${FULL_NAME}/config.json"
if [[ ! -f "$CONFIG" ]]; then
  echo "No config.json found. Run 'claudex new <name>' first." >&2
  exit 1
fi
```

Verify these files exist in the feature's `docsDir`:
- `design.md` — feature design (from `/claudex-plan`)
- `testing-plan.md` — e2e test plan (from `/claudex-plan`)
- `config.json` — feature state

If any are missing, stop and tell the user to run `/claudex-plan` first.

## Steps

### Step 1 — Read Config

Parse `config.json` to get:
- `fullName`, `docsDir`, `worktreePath`, `gitBranch`
- `architectSessionId` (may be `null`)
- `tmux` (may be `null`)

### Step 2 — Determine Display Mode

If `tmux` is not `null` in config:
- Use **split-pane** display mode
- Reference pane IDs from `config.tmux.panes`
Else:
- Use **in-process** display mode

Ask the user to confirm display mode if unsure.

### Step 3 — Run Agent Team

**REQUIRED**: Use the `agent-team` skill.

Provide the agent team with:
- **Feature directory**: `<docsDir>` (contains `design.md`, `testing-plan.md`)
- **Where to work**: `<worktreePath>` on branch `<gitBranch>`
- **Architect ID**: `<architectSessionId>` from config (tell team "no architect available" if null)
- **Display Mode**: from Step 2

The agent team handles the full pipeline: Planner -> Implementer -> Tester -> Fixer -> Code Reviewer.

### Step 4 — Post-Build

After the agent team completes:
1. Run your project's verify/build command to confirm everything passes
2. Ensure all changes are committed and pushed
3. Print summary:
```
Build complete for <fullName>.
  Branch: <gitBranch>
  PR: <prUrl>

Next: review the PR, then run /claudex-finish after merge.
```
