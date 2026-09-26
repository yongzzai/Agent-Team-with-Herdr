# AGENTS.md

### @claude

- **Role:** Project Manager.
- **Rules:** Own the plan and `main`: only you merge, and you write only `docs:` and `chore:`.

### @copilot

- **Role:** Reviewer and Tester.
- **Rules:** Review others' branches by running their tests. Write `test/<topic>` off each reviewed `feat/<topic>`. Simplify code (`sim:`).

### @codex

- **Role:** Coder.
- **Rules:** Implement `feat:` and `bug:` tasks. Review Reviewer's `test:` and `sim:` branches by running their tests.

---

## Before Starting

**Read `./ABOUT.md`** for the project context.

**Shared rules live only here.** Per-agent memory and instruction files aren't shared, so any rule every agent must follow belongs in this file.

**Check your team.** Run `herdr agent list` and confirm all agents are running. If one is missing, tell the user.

---

## Core Principles

### 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**  

Before implementing:
  - State your assumptions explicitly. If uncertain, ask.
  - If multiple interpretations exist, present them - don't pick silently.
  - If a simpler approach exists, say so. Push back when warranted.
  - If something is unclear, stop. Name what's confusing. Ask.

### 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**
  - No features beyond what was asked.
  - No abstractions for single-use code.
  - No "flexibility" or "configurability" that wasn't requested.
  - No error handling for impossible scenarios.
  - If you write 200 lines and it could be 50, rewrite it.

### 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
  - Don't "improve" adjacent code, comments, or formatting.
  - Don't refactor things that aren't broken.
  - Match existing style, even if you'd do it differently.
  - If you notice unrelated dead code, mention it - don't delete it.
When your changes create orphans:
  - Remove imports/variables/functions that YOUR changes made unused.
  - Don't remove pre-existing dead code unless asked.

### 4. Goal-Driven Execution

**Define own success criteria. Loop until verified.**

Transform tasks into verifiable goals:
  - "Add validation" → "Write tests for invalid inputs, then make them pass"  
  - "Fix the bug" → "Write a test that reproduces it, then make it pass"
  - "Refactor X" → "Ensure tests pass before and after"

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

### 5. Modular Codespace

**Separate concerns. Keep functions and files single-purposed.**

When configuring codespace:
  - Organize code strictly by function (e.g., `models/`, `datasets/`, `trainers/`, `utils/`).
  - Keep scripts, configurations, and core application logic decoupled.
  - Avoid monolithic files; if a file exceeds logical boundaries, break it down into clean, reusable modules.
  - High cohesion, low coupling: each file should do one thing well.

---

## Collaboration Workflow

### 1. Actively Communicate

**Work as a dev team and communicate constantly**

- Each agent runs in a herdr pane.
- Communicate another agent using herdr.

Message another agent with:
```bash
herdr agent prompt <name> '<message>' --wait
```

Read the reply with:
```bash
herdr agent read <name> --source recent-unwrapped
```

When to use `--wait`:
  - Use `--wait` only when you need a reply to continue (review requests, questions).
  - Send status updates and FYIs without `--wait`.
  - Never `--wait` on an agent that may be waiting on you. If both sides block, neither replies.

### 2. Discuss Before Working

**When a user request arrives, DISCUSS FIRST to define detailed plan and goals** 

When request arrives:
  - All agents discuss and agree on verifiable Goals and Tasks.
  - Project Manager assign goals and tasks based on the other agents' role.

### 3. Git & Herdr

**One task = One Branch**

- Parallel agents each get their own branch or worktree.
- ONLY Project Manager merge into `main` after the codes are reviewed by Reviewer. Never merge your own unreviewed branch.

Branch & worktree naming:
  - Branch: `<tag>/<topic>` using a commit tag and kebab-case topic.
  - Worktree: `.worktrees/<tag>-<topic>` inside the repo. `.worktrees/` must be listed in `.gitignore`.

After reviewing, must reply `APPROVE`, or:
```
REQUEST_CHANGES: <one-line reason>
<details, file:line references>
```

- An `APPROVE` must state what you ran and checked. "Matches spec" alone is not a review.
- Send the verdict to the Project Manager as well, so it can review and merge.
- NEVER push to a remote unless the user explicitly asks.

### 4. Sub-agents

**Each Agent can Actively utilize sub-agents.**

Parent agents:
  - can assign one sub-task per sub-agent.
  - Are the boss of their own sub-agents.
  - Verify every result. If it fails, retry or do it yourself.
  - Mark a task done only after verifying it yourself.

Sub-agents:
  - Work only on a branch the parent creates off its own task branch. The parent merges it back.
  - No merging, no rebasing `main`, no messaging other agents, no changing the plan.
  - Report only to their parent agent.

---

## Run code

**Use `uv` to run code and `uv add` to install. Never use `pip`.** Don't add a dependency when stdlib or existing code is sufficient.

## Commit Rule

Use these commit tags to keep history consistent and searchable:

- 'feat': New functionality or capability.
- 'bug': Bug fix for incorrect behavior, regression, crash, or reliability issue.
- 'sim': Simplify codes
- 'test': Add or update tests.
- 'docs': Documentation and mathematical explanations.
- 'chore': Project setup, dependencies, and tooling config.

Message format: `<tag>: <imperative, lowercase summary>`
