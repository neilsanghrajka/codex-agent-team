---
name: claudex-plan
description: Brainstorm a feature, produce design + e2e test plan, and create a draft PR
---

# Claudex — Plan

Plan a feature: brainstorm design, generate an e2e test plan, create a draft PR, and record all state in `config.json`.

## Prerequisites

This skill expects `claudex new <name>` has already been run. That creates:
- A git worktree on `feature/<id>-<slug>` branch
- `docs/features/<id>-<slug>/config.json` with feature metadata

## Gate Check

Before starting, verify:
```bash
FULL_NAME=$(basename "$(git rev-parse --show-toplevel)")
CONFIG="docs/features/${FULL_NAME}/config.json"
if [[ ! -f "$CONFIG" ]]; then
  echo "No config.json found. Run 'claudex new <name>' first." >&2
  exit 1
fi
```

Read `config.json` and use its fields for all paths below. If not currently in the worktree directory, `cd` into `worktreePath`.

## Steps

### Step 1 — Brainstorm -> design.md

**REQUIRED**: Use the `brainstorming` skill (invoke `/brainstorming`).

Brainstorm the feature with the user. Save the output as `design.md` in the feature's docs directory (`docsDir` from config.json).

### Step 2 — E2E Test Plan -> testing-plan.md

**REQUIRED**: Use the `claudex-testing-e2e` skill (invoke `/claudex-testing-e2e`).

Generate a blackbox e2e smoke test plan based on `design.md`. Save as `testing-plan.md` in the same docs directory.

### Step 3 — Create Draft PR

```bash
git add "<docsDir>/design.md" "<docsDir>/testing-plan.md" "<docsDir>/config.json"
git commit -m "feat: plan <slug> (#<featureId>)"
git push -u origin "<gitBranch>"
gh pr create --draft \
  --title "feat: <slug> (#<featureId>)" \
  --body "## Summary
Design and test plan for feature <fullName>.

## Documents
- \`<docsDir>/design.md\` — Feature design
- \`<docsDir>/testing-plan.md\` — E2E smoke test plan
- \`<docsDir>/config.json\` — Feature config

## Status
Planning complete. Ready for /claudex-build."
```

Capture the PR URL from `gh pr create` output.

### Step 4 — Architect Session ID

Ask the user:
> "Paste the Codex architect session ID (or press Enter to skip):"

### Step 5 — Update config.json

Update `config.json` with the new fields:
- `prUrl` — the draft PR URL from Step 3
- `architectSessionId` — from Step 4 (or keep `null` if skipped)

Commit and push:
```bash
git add "<docsDir>/config.json"
git commit -m "feat: update config with PR and architect (#<featureId>)"
git push
```

### Step 6 — Done

Print:
```
Planning complete for <fullName>.
  PR: <prUrl>
  Docs: <docsDir>/

Next: run /claudex-build (in a separate session)
```
