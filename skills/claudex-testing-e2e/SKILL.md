---
name: claudex-testing-e2e
description: Generate blackbox e2e smoke test plans for features
---

# E2E Smoke Test Plan Generation

Generate a blackbox end-to-end smoke test plan for a feature. This is a **foundation skill** — it produces a `testing-plan.md` document, not test code.

## Hard Gate

**NEVER generate:**
- Unit tests
- Integration tests
- Test code in `tests/`
- Vitest/Jest/Mocha specs
- Any `*.test.ts` or `*.spec.ts` files

This skill produces a **human/agent-executable test plan document** — shell commands, curl calls, browser steps, CLI invocations. Not code.

## Input

- A `design.md` describing what the feature does
- The feature's `config.json` for context (paths, branch, etc.)

## Output Structure

Write `testing-plan.md` in the feature's docs directory with these sections:

### 1. Objective
One sentence: what are we verifying works?

### 2. Prerequisites
Concrete list — credentials needed, dependencies to install, test data to create, services to start.

### 3. Live Smoke Procedure
Step-by-step commands that a tester (human or agent) can execute:
- Temporary scripts go in `/tmp/` (outside git-tracked files)
- Use `curl`, CLI commands, browser steps
- Each step has exact commands, not vague descriptions

### 4. Expected Results
Concrete outputs for each step — exact JSON shapes, status codes, UI states. Never write "should work" — describe what success looks like with evidence.

### 5. Negative Checks
Error paths to verify:
- What happens with bad credentials?
- What happens with malformed input?
- What happens when a dependency is down?

### 6. Evidence to Attach in PR
What artifacts to capture as proof:
- Command transcript snippets (sanitized)
- Redacted JSON response shapes
- Screenshots (if UI)
- Error messages from negative checks

### 7. Phased Approach (for complex features)
If the feature is large, break testing into phases:
- **Phase A — Static gates:** Linting, type-check, build passes
- **Phase B — Live acceptance:** Core happy-path smoke tests from the procedure above
- **Phase C — Regression:** Verify existing functionality still works after changes

## Non-Goals Section

Always end the testing plan with a **Non-Goals** section listing what is explicitly NOT tested (to prevent scope creep during the testing phase).
