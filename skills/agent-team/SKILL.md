---
name: agent-team
description: Create and orchestrate a Claude Code agent team for feature implementation
---

# Agent Team Prompt

> Give this to the team lead session along with an implementation plan and test plan.

Create a Claude Code Agent Team.

## Pre-Checks
---
Before starting ensure the following is true. If not, then terminate early.
- Understand Claude Code agent teams by reading this doc https://code.claude.com/docs/en/agent-teams
- Check if { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"} is enabled in settings.json.
- If display mode is split-pane, check if we are running inside tmux.
- Check you are given all the inputs you need(mentioned below). If not ask for them.
- Verify you are able to talk to architect by telling it "Hi, I am your implementation team, we will start work now. Wait for me, i will send you tasks for review/approval."

## Input for agent team.
---
**Note**: DO NOT START IF THE MANDATORY INPUTS ARE missing
Each agent team requires the following inputs.
- **Feature directory**: `<docsDir>/` — all pipeline documents live here.
  - `design.md` — What we want the agent team to build
  - `testing-plan.md` — How to verify that what the agent team built works
- **Where to work**: Which worktree/directory/branch/cloud environment to work inside.
- **Architect Id**: Codex session Id. See architect session below.
- **Display Mode**: Split-pane or In-Process.

### Document Convention
All documents live in a single feature directory.  Ensure all documents are committed.
```
<docsDir>/
├── 01-design.md                 # Step 0: Human + Architect
├── 02-testing-plan.md           # Step 0: Human + Architect
├── 03-implementation-plan.md    # Step 1: Planner
├── 04-testing-feedback.md       # Step 3: Tester
└── 05-code-review-feedback.md   # Step 5: Code Reviewer
```

## Instructions for team lead (you)
---
- **IMPORTANT**: Agent team is different from subagents. I am not talking about sub-agents. See this for context (https://code.claude.com/docs/en/agent-teams). Each teammember can use subagents, but as team lead your job is to spin up agent-teams.
- **Team Lead**: You are team lead. Your job is to DELEGATE. Wait for teammate to complete their tasks. Do not implement their tasks for them even if they take long.
- **Display**: If input is split pane display, Use agent teams split plane display mode, NOT IN process. Else use in-process
- **Pre-Checks**: Before starting, check if { "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"} is enabled in settings.json. And we are running inside tmux.
- **Plan approvals**:  For complex/long changes, require plan approvals from teammates that "Require Plan".
- **Assigning Tasks**: Only lead can assign tasks. Teammate cannot self-claim.
- **Cleanup**: Cleanup team once done.
- **Worktrees**: Team should operate in a worktree only. If you are not already in a worktree, or have not been given a worktree, create a new one using /using-git-worktrees skill.
- ***Scope discipline**: Members do ONLY what the plan says. No extra refactoring, no "improvements".
- **Model**: Always use Opus. Even for sub-agents.


## Instructions to teammates
---
Team lead should give each teammate this instruction always on startup, in addition to task context and other communication.

```
1. Always use Opus. Also use opus subagents to do things faster where you see appropriate.
2. Only work on tasks that your teammate tells you to . DO NOT self claim tasks. All managed exclusively by the team lead — no member picks up work on its own, ever. Wait idle until the lead assigns work.
3. Don't do beyond what you are told and is in scope of task. You will be punished for over-enthusiasm.
4. You have access to architect.  <INSERT FULL DETAILED ARCHITECT SECTION HERE>
5. If for any reason you cannot exactly do what you were supposed to because you don't have access, or things aren't setup properly it's okay to pause and ask for human intervention. DO NOT ASSUME and do short work arounds.
```


## Architect
---
- Architect is an external consultant, not part of the agent team. Any teammate can access architect and have discussions and ask for opinions on their work.
- Architect is different from team lead. Team lead is an engineering-manager coordinating work between engineers. Architect is an expert Product Manager + Technical lead who has the business context of the change and the expert behind this change.
- To talk to architect run `codex --yolo exec resume <codex session id> <PROMPT>`.
- Architect remembers history of conversation in the order it is invoked. No need to repeat. However. Don't let multiple teammates talk to architect at once. They can talk directly, but not at same time.
- Architect is able to parallelize by working in multiple directories. Always tell it which directory you are calling from, it will know.
- Unlike team-lead, architect cannot access a teammate's claude code session or read discussions. Architect can access the file system, github and all the same tools, but session context must be provided to architect when discussing with it.
- Architect is smarter than you so listen to architect unless there is some context architect does not have, and you are able to prove it. Always give concrete evidence when disagreeing with architect to make it agree. Else listen to it, even if you don't feel like. Don't coerce architect, with anything other than logic and evidence.

## Team Structure
---

### 1. Planner
- **Role:** Write an implementation plan file that an implementer can execute.
- **Input**: `<docsDir>/design.md`
- **Output**: `<docsDir>/implementation-plan.md`
- **Steps:**
  - 1. Read the provided `design.md` and create an implementation plan using `/writing-plans` skill.
  - 2. Use parallel Opus sub-agents to write plan.
  - 3. Once plan is written, review the plan to see if it can be simplified without compromising goals.
- **Feedback**:
  - Consult with `architect` on your plan, ask for feedback. If you have deviated from architect's instructions, explain to architect and see if they have feedback still. If yes - incorporate that in the plan.
- **Guardrails:**
  - DO NOT LOOK AT `testing-plan.md`. That is meant for the tester teammate.
  - Does NOT write code, only output a plan file.
  - Don't do more than 3 rounds of back and forth with architect.
- **When to Stop:**
  - Stops after plan is written and tell team lead you are done.
- **Keep Alive**:
  - Once plan is written, team lead should close this session.


### 2. Implementer
- **Role:** Execute the approved implementation plan and produce working code.
- **Input**: `<docsDir>/implementation-plan.md`
- **Output**: Working code pushed to branch, local change summary (start commit, end commit).
- **Steps:**
  - Execute the approved plan using `/executing-plans` skill with `/subagent-driven-development` skill. Use Opus sub-agents.
  - Run end to end verification to ensure code works.
- **Feedback**:
  - After execution completes: review the changes and simplify where possible, then commit.
- **Guardrails:**
  - DO NOT RAISE PR.
- **When to Stop:**
  - Stops after pushing to branch and reporting change summary to team lead.
- **Keep Alive:**
  - CLOSE session once done.


### 3. Tester
- **Role:** Standalone functional tester — does not know the codebase, just verifies that changes work against the test plan.
- **Input**: `<docsDir>/testing-plan.md`, access to the branch with implemented changes.
- **Output**: `<docsDir>/testing-feedback.md` — pass/fail report with exact command output as evidence. If `testing-feedback.md` already exists, append latest run feedback.
- **Steps:**
  - Can start during Step 1 to create testing strategy using `/plan`, then wait for code changes before running tests.
  - Tests end-to-end using the provided test plan. These are end-to-end tests not unit/integration.
  - Report exact output — no "should work" claims, only evidence.
- **Feedback**:
  - No feedback needed on it's work. Reports failures to team lead only.
- **Guardrails:**
  - Does NOT fix code. Does NOT give code review feedback (only testing feedback). On failure, reports failures in testing-feedback. Team lead spawns a Fixer session. Tester re-tests after fixes are pushed.
- **When to Stop:**
  - Stops after all checks pass with evidence.
- **Keep Alive:**
  - Stays alive throughout (Step 1-6). Re-tests after Fixer (Step 4) and after Finisher (Step 6).


### 4. Fixer
- **Role:** Debug and fix test failures reported by Tester.
- **Input**: `<docsDir>/testing-feedback.md` (the failure report from Tester).
- **Output**: Fixed code pushed to branch.
- **Steps:**
  - Use `/systematic-debugging` to identify issues in plan mode first.
  - Plan bug fixes in plan mode, then execute fixes.
  - Tell Tester to re-test.
  - Loop until Tester reports all pass.
- **Feedback**:
  - Only feedback from Tester.
- **Guardrails:**
  - Only fixes what Tester reported. No refactoring, no improvements. If more than 3 iterations, there is some fundamental bug, ask human for help.
- **When to Stop:**
  - Stops when Tester reports all pass.
- **Keep Alive:**
  - CLOSE session once Tester passes.


### 5. Code Reviewer
- **Role:** Collect raw review feedback — has no own opinion, does NOT fix or coordinate fixes.
- **Input**: `<docsDir>/testing-feedback.md` (pass evidence), branch with implemented changes.
- **Output**: `<docsDir>/code-review-feedback.md` — raw feedback collection, no prioritisation.
- **Steps:**
  - Create PR with full context in description.
  - Wait ~10 minutes for reviewers to comment on GitHub.
  - Wait for all GitHub checks to run.
  - Collect ALL raw feedback: `gh api` for comments + reviews + CI results (no own opinion).
  - Produce `code-review-feedback.md`.
- **Feedback**:
  - None. This teammate just collects.
- **Guardrails:**
  - Does NOT fix code. Does NOT prioritise or filter feedback. Does NOT have opinions. Just raw collection.
- **When to Stop:**
  - Stops after `code-review-feedback.md` is produced.
- **Keep Alive:**
  - CLOSE session once `code-review-feedback.md` is done.



## Flow
---

### Legend
```
██ TEAM MEMBER ██   — Agent team session (Planner, Implementer, Tester, Fixer, Code Reviewer)
── TEAM LEAD ──     — You, the orchestrator
** ARCHITECT **     — External Codex consultant (not a team member)
「skill-name」       — Skill invocation (e.g.「/writing-plans」)
[X] CLOSE           — Session terminated after this step
```

### Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 0 ── TEAM LEAD ── + ** ARCHITECT **                           │
│                                                                     │
│  Plan together. Produce `design.md` + `testing-plan.md`             │
│  in `<docsDir>/`.                                                   │
│  Give team: feature directory path + inputs.                        │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               v
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 1 ██ PLANNER ██                                               │
│                                                                     │
│  1. Write implementation plan using「/writing-plans」                │
│  2. Simplify plan where possible                                    │
│  3. ** ARCHITECT ** reviews plan, Planner incorporates feedback     │
│  [X] CLOSE session                                                  │
│                                                                     │
│  ┌─ NOTE ──────────────────────────────────────────────────────┐    │
│  │ ██ TESTER ██ can start here — create testing strategy,      │    │
│  │ then wait for code changes before running tests.            │    │
│  └─────────────────────────────────────────────────────────────┘    │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               v
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 2 ██ IMPLEMENTER ██                                           │
│                                                                     │
│  1. Execute plan using「/executing-plans」+「/subagent-driven-dev」  │
│  2. Simplify and review changes                                     │
│  3. Does NOT raise PR                                               │
│  [X] CLOSE session                                                  │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               v
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 3 ██ TESTER ██  — runs full verification suite                │
│                                                                     │
│  Only tests changes, using provided test plan                       |
|  no code review feedback.                                           │
│  Acts as standalone functional tester (does not know codebase,      │
│  just verifies).                                                    │
│                                                                     │
│            ┌── PASS ──┐              ┌── FAIL ──┐                   │
│            │           │             │          │                   │
│            v           │             v          │                   │
│         Step 5         │  ┌───────────────────────────────────┐     │
│                        │  │ STEP 4 ██ FIXER ██                │     │
│                        │  │                                   │     │
│                        │  │ 1. Plan using「/systematic-debug」 │     │
│                        │  │ 2. Execute fixes                  │     │
│                        │  │ 3. Tell ██ TESTER ██ to re-test   │     │
│                        │  │ 4. Loop until Tester is happy     │     │
│                        │  │ [X] CLOSE session                 │     │
│                        │  └───────────────┬───────────────────┘     │
│                        │                  │                         │
│                        │                  v                         │
│                        │       ██ TESTER ██ re-tests                │
│                        │                  │                         │
│                        └──────<───────────┘                         │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ PASS
                               v
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 5 ██ CODE REVIEWER ██                                         │
│                                                                     │
│  1. Create PR                                                       │
│  2. Wait ~10 min for reviewers to comment on GitHub                 │
│  3. Wait for all GitHub checks to run                               │
│  4. Collect ALL raw feedback (no own opinion)                       │
│  5. Produce `code-review-feedback.md`                               │
│  [X] CLOSE session                                                  │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               v
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 6  ── TEAM LEAD ── tells ** ARCHITECT **:                     │
│                                                                     │
│  1. Review all findings (`code-review-feedback.md`,                 │
│     `testing-feedback.md`, PR link,                                 │
│     start-end-commit)                                               │
│  2. Prioritize & fix remaining testing feedback                     │
│  3. One last architecture smell review, fix that                    │
│  4. Make a commit                                                   │
│  5. ── TEAM LEAD ── checks and pushes PR                            │
│  6. ██ TESTER ██ RETESTS                                            │
│                                                                     │
│  [Alive: ██ TESTER ██ + ── TEAM LEAD ── + ** ARCHITECT **]          │
└──────────────────────────────┬──────────────────────────────────────┘
                               │
                               v
┌─────────────────────────────────────────────────────────────────────┐
│  STEP 7 ── DONE  ── TEAM LEAD ──                                    │
│                                                                     │
│  Shut down ██ TESTER ██ → clean up team                             │
└─────────────────────────────────────────────────────────────────────┘
```

### Session Lifecycle Summary
```
██ PLANNER ██        ████░░░░░░░░░░░░░░░░░░░░░░░░░░  Step 1 only
██ IMPLEMENTER ██    ░░░░████░░░░░░░░░░░░░░░░░░░░░░  Step 2 only
██ TESTER ██         ░░██████████████████████████████  Step 1→6 (stays alive)
██ FIXER ██          ░░░░░░░░░░██░░░░░░░░░░░░░░░░░░  Step 4 only (if needed)
██ CODE REVIEWER ██  ░░░░░░░░░░░░░░████░░░░░░░░░░░░  Step 5 only
── TEAM LEAD ──      ████████████████████████████████  Always alive
** ARCHITECT **      ██░░░░░░░░░░░░░░░░░░░░░░░░████  Step 0, 1, 6
```

### Document Lifecycle
```
Step 0  Human + Architect  →  design.md, testing-plan.md
Step 1  Planner            →  implementation-plan.md       (reads design.md)
Step 2  Implementer        →  code on branch               (reads implementation-plan.md)
Step 3  Tester             →  testing-feedback.md           (reads testing-plan.md)
Step 4  Fixer              →  fixed code                   (reads testing-feedback.md)
Step 5  Code Reviewer      →  code-review-feedback.md      (reads testing-feedback.md)
Step 6  Architect          →  final fixes                  (reads code-review-feedback.md + testing-feedback.md)
```

### STEP 6 - Clarification
- Team Lead just orchestrates , does not make code changes.
- Provide Architect with context. This is all code review feedback, any outstanding bugs reported by tester, path to PR etc etc. You can give file references, architect can read.
- Architect's job is to finish up this PR. First review all work against our design goals, prioritize and incorporate code reviewer feedback and pending testing feedback. Make a plan and then execute. Tell architect that most of the work is already done, and it just needs to finish it up. If there are some major gaps still, stop work and tell team lead to escalate to human.
- Once done tell team lead to push to GitHub and mark task done.

### STEP 7 - Cleanup (Team Lead)
  1. **Pipeline docs committed**: Commit `implementation-plan.md`, `testing-feedback.md`, `code-review-feedback.md` along with all docs to the branch. These are traceability artifacts.
  2. **PR description updated**: If fixes were made after the PR was created (Fixer, Step 6 architect fixes), update the PR description to reflect the final state — final commit hash, final test results, any bugs found and fixed.
  3. **Stale feedback docs refreshed**: If `code-review-feedback.md` was collected before subsequent fix commits, append a note with the latest review status.
  4. **Temp files deleted**: Remove any temp runners, scripts, or artifacts created during testing. Ask Tester to clean up before shutdown, or do it yourself.
  5. **Background processes killed**: Kill any background processes started during the session (dev servers, watchers). Use `ps aux | grep` to verify. Focus only on things started in this session.
  6. **Working tree clean**: Run `git status` — must show clean working tree, branch up to date with remote.
  7. **PR is mergable**: Verify that CI checks pass on the actual last pushed commit, not an earlier one. No merge conflicts, if any pull latest main and resolve.
  8. **Team resources cleaned**: Run team cleanup (TeamDelete or equivalent) to remove shared team config and task directories.
