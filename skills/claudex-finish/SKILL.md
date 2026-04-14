---
name: claudex-finish
description: Post-merge cleanup for a completed feature
---

# Claudex — Finish

Clean up after a feature has been merged: remove worktree, delete branches, kill tmux, and report summary.

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

If not found, ask the user for the feature's full name (e.g. `0003-my-feature`) and look for `docs/features/<fullName>/config.json`.

## Steps

### Step 1 — Verify PR is Merged

```bash
gh pr view "<prUrl>" --json state -q '.state'
```

If state is not `MERGED`, warn the user and ask if they want to proceed anyway.

### Step 2 — Kill Tmux Window

If `config.tmux` is not `null`:
```bash
tmux kill-window -t "<tmux.window>" 2>/dev/null || true
```

### Step 3 — Remove Worktree

```bash
# From the main repo (NOT from inside the worktree)
cd "$(git rev-parse --show-toplevel 2>/dev/null || git config worktree.base)/.."
git worktree remove "<worktreePath>" --force
```

### Step 4 — Delete Branches

```bash
# Delete local branch
git branch -D "<gitBranch>" 2>/dev/null || true

# Delete remote branch (GitHub auto-deletes on merge, but be safe)
git push origin --delete "<gitBranch>" 2>/dev/null || true
```

### Step 5 — Summary

```
Feature <fullName> cleaned up!
  PR: <prUrl> (MERGED)
  Worktree: removed
  Branch: <gitBranch> deleted
  Tmux: <killed / n/a>

Docs preserved at: docs/features/<fullName>/
```

Note: Feature docs (`design.md`, `testing-plan.md`, etc.) are kept in the repo as traceability artifacts — they were committed to main via the merged PR.
