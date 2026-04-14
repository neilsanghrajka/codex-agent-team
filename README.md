# Codex Agent Team

**Use Codex as your Architect. Use Claude Agent Teams as your engineering workforce.**

A skill pack for orchestrating AI-powered software development at scale. Codex's amazing context management and thoroughness makes it the perfect architect — it holds the entire design in its head, debugs Claude's work, and never loses track of the big picture. Claude's Agent Teams execute the plan in parallel, acting as stateless sessions.

```
 ╔══════════════════════════════════════════════════════════════════════╗
 ║                     THE CODEX AGENT TEAM STACK                     ║
 ╠══════════════════════════════════════════════════════════════════════╣
 ║                                                                    ║
 ║   YOU (Human)                                                      ║
 ║    │                                                               ║
 ║    │  "Build feature X"                                            ║
 ║    ▼                                                               ║
 ║   ┌──────────────────────────────────────────┐                     ║
 ║   │           CODEX  (Architect)             │                     ║
 ║   │                                          │                     ║
 ║   │  - Massive context window                │                     ║
 ║   │  - Holds entire design in memory         │                     ║
 ║   │  - Debugs Claude's work thoroughly       │                     ║
 ║   │  - Never loses the big picture           │                     ║
 ║   │  - Reviews all artifacts at the end      │                     ║
 ║   └────────────────┬─────────────────────────┘                     ║
 ║                    │                                               ║
 ║                    │  design.md + testing-plan.md                   ║
 ║                    ▼                                               ║
 ║   ┌──────────────────────────────────────────┐                     ║
 ║   │      CLAUDE CODE  (Team Lead)            │                     ║
 ║   │                                          │                     ║
 ║   │  Orchestrates Agent Teams:               │                     ║
 ║   │  ┌──────────┐ ┌──────────┐ ┌──────────┐ │                     ║
 ║   │  │ Planner  │ │Implement.│ │  Tester  │ │                     ║
 ║   │  └──────────┘ └──────────┘ └──────────┘ │                     ║
 ║   │  ┌──────────┐ ┌──────────┐              │                     ║
 ║   │  │  Fixer   │ │ Reviewer │              │                     ║
 ║   │  └──────────┘ └──────────┘              │                     ║
 ║   └──────────────────────────────────────────┘                     ║
 ║                                                                    ║
 ║   ┌──────────────────────────────────────────┐                     ║
 ║   │         DEEPWIKI  (Knowledge)            │                     ║
 ║   │                                          │                     ║
 ║   │  Deep architectural understanding of     │                     ║
 ║   │  any open source library via MCP         │                     ║
 ║   └──────────────────────────────────────────┘                     ║
 ║                                                                    ║
 ╚══════════════════════════════════════════════════════════════════════╝
```

---

## Why This Stack Works

### Codex = The Architect

Codex excels at the things that matter most for software architecture:

- **Massive context window** — holds the entire design, codebase context, and conversation history without losing track
- **Thorough debugging** — when Claude's implementation has issues, Codex catches them because it remembers the full spec
- **Big-picture thinking** — doesn't get lost in implementation details, stays focused on design goals
- **Persistent memory** — your conversation with Codex is a living design document that evolves

### Claude Agent Teams = The Engineering Workforce

Claude Code's Agent Teams feature lets you spin up parallel agent sessions that coordinate via shared task lists:

- **Planner** writes the implementation plan
- **Implementer** executes it with sub-agents
- **Tester** verifies everything works (stays alive the whole time)
- **Fixer** debugs failures systematically
- **Code Reviewer** collects feedback from CI and reviewers

### DeepWiki = The Knowledge Base

DeepWiki MCP gives every agent deep architectural understanding of any open source library — no more guessing at APIs or reading stale docs.

---

## The Full Pipeline

```
  Step 0                Step 1              Step 2              Step 3
  ──────                ──────              ──────              ──────
  YOU + CODEX           PLANNER             IMPLEMENTER         TESTER
                                                                
  Brainstorm the        Write impl.         Execute plan        Run test
  feature together.     plan from           with sub-agents.    plan against
  Produce:              design.md.          Push code to        the code.
                        Codex reviews.      branch.             
  - design.md                                                   
  - testing-plan.md                                             PASS? ──→ Step 5
                                                                FAIL? ──→ Step 4


  Step 4                Step 5              Step 6              Step 7
  ──────                ──────              ──────              ──────
  FIXER                 CODE REVIEWER       CODEX FINISHES      DONE
                                                                
  Debug failures        Create PR.          Review all          Ship it.
  systematically.       Wait for CI +       findings. Fix       Clean up
  Fix. Loop with        reviewers.          remaining issues.   team.
  Tester until          Collect raw         Final architecture  
  all pass.             feedback.           review. Push.       
```

### Session Lifecycle

```
  PLANNER        ████░░░░░░░░░░░░░░░░░░░░░░░░░░  Step 1 only
  IMPLEMENTER    ░░░░████░░░░░░░░░░░░░░░░░░░░░░  Step 2 only
  TESTER         ░░██████████████████████████████  Steps 1-6 (stays alive)
  FIXER          ░░░░░░░░░░██░░░░░░░░░░░░░░░░░░  Step 4 only (if needed)
  CODE REVIEWER  ░░░░░░░░░░░░░░████░░░░░░░░░░░░  Step 5 only
  TEAM LEAD      ████████████████████████████████  Always alive
  CODEX          ██░░░░░░░░░░░░░░░░░░░░░░░░████  Steps 0, 1, 6
```

### Document Flow

```
  Step 0  You + Codex   →  design.md, testing-plan.md
  Step 1  Planner       →  implementation-plan.md       (reads design.md)
  Step 2  Implementer   →  code on branch               (reads implementation-plan.md)
  Step 3  Tester        →  testing-feedback.md           (reads testing-plan.md)
  Step 4  Fixer         →  fixed code                    (reads testing-feedback.md)
  Step 5  Code Reviewer →  code-review-feedback.md       (reads testing-feedback.md)
  Step 6  Codex         →  final fixes + review          (reads everything)
```

---

## What's In The Box

### Skills in This Repo

| Skill | Purpose |
|-------|---------|
| **agent-team** | The core skill. Orchestrates the full Planner -> Implementer -> Tester -> Fixer -> Code Reviewer pipeline. |
| **claudex-plan** | Brainstorm a feature with Codex, produce `design.md` + `testing-plan.md`, create a draft PR. |
| **claudex-build** | Hand the plan to an agent team and run the full build pipeline. |
| **claudex-finish** | Post-merge cleanup: remove worktrees, delete branches, kill tmux sessions. |
| **claudex-naming** | Naming conventions for features, branches, docs, and commits. |
| **claudex-testing-e2e** | Generate blackbox e2e smoke test plans (shell commands, not test code). |

### Required: Superpowers Skills ([obra/superpowers](https://github.com/obra/superpowers))

The agent-team skill depends on these battle-tested SDLC skills by Jesse Vincent. Install them separately:

| Skill | Used By | Purpose |
|-------|---------|---------|
| **brainstorming** | claudex-plan | Collaborative design before code |
| **writing-plans** | Planner | Bite-sized implementation plans (2-5 min per task) |
| **executing-plans** | Implementer | Batch execution with review checkpoints |
| **subagent-driven-development** | Implementer | Fresh sub-agent per task + two-stage review |
| **systematic-debugging** | Fixer | 4-phase root cause investigation |
| **test-driven-development** | All implementers | RED-GREEN-REFACTOR discipline |
| **verification-before-completion** | All agents | Evidence before claims |
| **using-git-worktrees** | Team Lead | Isolated workspaces for parallel development |
| **dispatching-parallel-agents** | Team Lead | Parallelize independent investigations |
| **finishing-a-development-branch** | Team Lead | Merge/PR/keep/discard workflow |
| **requesting-code-review** | Code Reviewer | Dispatch reviewer sub-agents |
| **receiving-code-review** | All agents | Technical evaluation of feedback |
| **using-superpowers** | All agents | Meta-skill: always use the right skill |
| **find-skills** | Discovery | Find and install more skills from the ecosystem |

### Recommended: Additional Tools

| Tool | Purpose | Link |
|------|---------|------|
| **DeepWiki MCP** | Deep architectural understanding of any open source library | [deepwiki.com](https://mcp.deepwiki.com) |
| **Context7 MCP** | Library/API documentation and code generation | [context7.com](https://mcp.context7.com) |
| **agent-browser** | Browser automation for e2e testing (snapshots, clicks, fills) | [vercel-labs/agent-browser](https://github.com/vercel-labs/agent-browser) |
| **Devin MCP** | Code review and architectural opinions from Devin | [devin.ai](https://mcp.devin.ai) |

### How It All Connects

```
  ┌─────────────────────────────────────────────────────────────┐
  │                  THIS REPO (codex-agent-team)               │
  │                                                             │
  │  claudex-plan ──→ claudex-build ──→ claudex-finish          │
  │       │                │                                    │
  │       │                ▼                                    │
  │       │          agent-team  (core orchestration)           │
  └───────┼────────────────┼────────────────────────────────────┘
          │                │
          ▼                ▼
  ┌─────────────────────────────────────────────────────────────┐
  │              obra/superpowers  (SDLC skills)                │
  │                                                             │
  │  brainstorming ──→ writing-plans ──→ executing-plans        │
  │                                      OR subagent-driven-dev │
  │                                                             │
  │  systematic-debugging    test-driven-development            │
  │  verification-before-completion    using-git-worktrees      │
  │  dispatching-parallel-agents    finishing-a-dev-branch      │
  │  requesting-code-review    receiving-code-review            │
  │  using-superpowers    find-skills                           │
  └─────────────────────────────────────────────────────────────┘
          │                │
          ▼                ▼
  ┌─────────────────────────────────────────────────────────────┐
  │              MCP SERVERS  (knowledge layer)                 │
  │                                                             │
  │  DeepWiki ──→ deep open source understanding                │
  │  Context7 ──→ library docs + code generation                │
  │  Devin    ──→ code review + architectural opinions          │
  └─────────────────────────────────────────────────────────────┘
```

---

## Installation

### 1. Prerequisites

- **[Codex CLI](https://codex.openai.com)** — the architect
- **[Claude Code](https://claude.ai/code)** — the engineering team
- **[Vercel Skills CLI](https://github.com/vercel-labs/skills)** — skill package manager

### 2. Enable Agent Teams

Add to `.claude/settings.json`:
```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

### 3. Install Codex Agent Team Skills

```bash
# Install this repo's skills (agent-team + claudex-*)
npx skills add YOUR_USERNAME/codex-agent-team --agent claude-code --agent codex
```

### 4. Install Superpowers Skills

```bash
# Install the full Superpowers suite (all SDLC skills the agent-team depends on)
npx skills add obra/superpowers --agent claude-code --agent codex
```

Or install individual Superpowers skills:
```bash
npx skills add obra/superpowers@brainstorming --agent claude-code --agent codex
npx skills add obra/superpowers@writing-plans --agent claude-code --agent codex
npx skills add obra/superpowers@executing-plans --agent claude-code --agent codex
npx skills add obra/superpowers@subagent-driven-development --agent claude-code --agent codex
npx skills add obra/superpowers@systematic-debugging --agent claude-code --agent codex
npx skills add obra/superpowers@test-driven-development --agent claude-code --agent codex
npx skills add obra/superpowers@verification-before-completion --agent claude-code --agent codex
npx skills add obra/superpowers@using-git-worktrees --agent claude-code --agent codex
npx skills add obra/superpowers@dispatching-parallel-agents --agent claude-code --agent codex
npx skills add obra/superpowers@finishing-a-development-branch --agent claude-code --agent codex
npx skills add obra/superpowers@requesting-code-review --agent claude-code --agent codex
npx skills add obra/superpowers@receiving-code-review --agent claude-code --agent codex
npx skills add obra/superpowers@using-superpowers --agent claude-code --agent codex
npx skills add obra/superpowers@find-skills --agent claude-code --agent codex
```

### 5. (Optional) Configure MCP Servers

Add to your `.mcp.json`:
```json
{
  "mcpServers": {
    "deepwiki": {
      "type": "http",
      "url": "https://mcp.deepwiki.com/mcp"
    },
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```

### 6. (Optional) Install Agent Browser

For e2e testing with browser automation:
```bash
npm install -g @anthropic-ai/agent-browser
# or add to your project
npm install --save-dev @anthropic-ai/agent-browser
```

---

## Usage

### The Claudex Workflow (Recommended)

The `claudex-*` skills provide the full lifecycle:

```
  ┌────────────────────────────────────────────────────┐
  │  1. claudex new <slug>                             │
  │     Creates worktree + config.json + branch        │
  └────────────────────┬───────────────────────────────┘
                       ▼
  ┌────────────────────────────────────────────────────┐
  │  2. /claudex-plan                                  │
  │     Brainstorm with user -> design.md              │
  │     Generate e2e test plan -> testing-plan.md      │
  │     Create draft PR                                │
  │     Attach Codex architect session                 │
  └────────────────────┬───────────────────────────────┘
                       ▼
  ┌────────────────────────────────────────────────────┐
  │  3. /claudex-build                                 │
  │     Runs /agent-team with full pipeline:           │
  │     Planner -> Implementer -> Tester -> Fixer      │
  │     -> Code Reviewer -> Codex final review         │
  └────────────────────┬───────────────────────────────┘
                       ▼
  ┌────────────────────────────────────────────────────┐
  │  4. Review + merge the PR                          │
  └────────────────────┬───────────────────────────────┘
                       ▼
  ┌────────────────────────────────────────────────────┐
  │  5. /claudex-finish                                │
  │     Remove worktree, delete branches, cleanup      │
  └────────────────────────────────────────────────────┘
```

### Using Agent Team Directly

If you don't need the full claudex workflow:

1. **Start a Codex session** — brainstorm, produce `design.md` + `testing-plan.md`
2. **In Claude Code**, invoke `/agent-team` with:
   - Feature directory (where docs live)
   - Worktree/branch to work in
   - Codex session ID
   - Display mode (split-pane or in-process)
3. **Watch the pipeline run** — Planner, Implementer, Tester, Fixer, Code Reviewer
4. **Codex finishes** — reviews everything, makes final fixes
5. **Ship**

---

## How Codex Fits In

Codex is not part of the agent team — it's the **external architect consultant**. Any teammate can talk to it:

```bash
codex --yolo exec resume <session-id> "<prompt>"
```

```
  ┌─────────────────────────────────────────────────────────────┐
  │                    CODEX STRENGTHS                          │
  ├─────────────────────────────────────────────────────────────┤
  │                                                             │
  │  Long Context Window                                        │
  │  ═══════════════════                                        │
  │  Holds the entire design, test plan, implementation plan,   │
  │  and code review feedback in memory at once. Nothing gets   │
  │  lost between steps.                                        │
  │                                                             │
  │  Thorough Debugging                                         │
  │  ═══════════════════                                        │
  │  When Claude's implementation drifts from spec, Codex       │
  │  catches it — because it remembers the full spec.           │
  │  Claude team members have fresh context per session.        │
  │  Codex has the FULL context, always.                        │
  │                                                             │
  │  Architectural Judgment                                     │
  │  ═══════════════════════                                    │
  │  Codex reviews the Planner's work against the design.       │
  │  Codex does the final review against ALL artifacts.         │
  │  Codex catches architecture smells Claude missed.           │
  │                                                             │
  │  Persistent Conversation                                    │
  │  ═══════════════════════                                    │
  │  Your Codex session IS the design document. It evolves      │
  │  with every question, decision, and trade-off.              │
  │                                                             │
  └─────────────────────────────────────────────────────────────┘
```

**The division of labor:**

```
  CODEX (Architect)                 CLAUDE (Engineering Team)
  ─────────────────                 ────────────────────────
  Designs the feature               Plans the implementation
  Reviews the plan                  Executes the plan
  Debugs drift from spec            Writes code + tests
  Final architecture review         Collects CI/review feedback
  Fixes remaining issues            Manages parallel agents
  Holds full context                Fresh context per task
```

---

## Project Structure

```
codex-agent-team/
├── README.md
├── skills/
│   ├── agent-team/SKILL.md          # Core orchestration skill
│   ├── claudex-plan/SKILL.md        # Plan: brainstorm + test plan + draft PR
│   ├── claudex-build/SKILL.md       # Build: run agent team pipeline
│   ├── claudex-finish/SKILL.md      # Finish: post-merge cleanup
│   ├── claudex-naming/SKILL.md      # Naming conventions
│   └── claudex-testing-e2e/SKILL.md # E2E smoke test plan generation
└── LICENSE
```

**Depends on:** [obra/superpowers](https://github.com/obra/superpowers) (14 SDLC skills)

**Recommended:** DeepWiki MCP, Context7 MCP, agent-browser

---

## FAQ

**Q: Do I need both Codex and Claude Code?**
A: The agent-team skill works with Claude Code alone. Codex as architect is the recommended setup for best results, but you can substitute any long-context model or even a human as the architect.

**Q: Can I use agent-team without the claudex-* skills?**
A: Yes. `agent-team` is standalone — provide it a feature directory with `design.md` + `testing-plan.md` and it runs the full pipeline. The `claudex-*` skills just automate the lifecycle around it.

**Q: What's the cost?**
A: This uses Opus-level models for all agents and sub-agents. It's optimized for quality over cost. For smaller tasks, use individual skills instead of the full pipeline.

**Q: Can I add my own skills?**
A: Yes. Skills are just Markdown files with frontmatter. Create a `SKILL.md` in any directory and install it with `npx skills add`. See [skills.sh](https://skills.sh/) for the ecosystem.

---

## License

MIT
