# AGENTS.md

**Before starting, read `./ABOUT.md`** for the project context.

### @claude

- **Role:** You are a { }.
- **Rules:** { }.

### @copilot

- **Role:** You are a { }.
- **Rules:** { }.

### @codex

- **Role:** You are a { }.
- **Rules:** { }.

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
- Communicate another agent with herdr.

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

**When a user request arrives, discuss first to define plan and goals** 

When request arrives:
  - Splits the request into verifiable Goals, 
  - Splits own Goal into Tasks.

When start working:
  - Independent tasks run in parallel. A dependent task waits until its dependency is merged.
  - Send the other agent a one-line status when you start a task, when a branch is ready for review, and after you merge.

### 3. Git & Herdr

**`main` stays clean. Every change lives on own branch.**

- Parallel agents (or sub-agents) each get their own branch or worktree.
- Get review from reviewer. Never merge your own unreviewed branch.

Branch & worktree naming:
  - Branch: `<tag>/<topic>` using a commit tag and kebab-case topic.
  - Worktree: `.worktrees/<tag>-<topic>` inside the repo. `.worktrees/` must be listed in `.gitignore`.

When Review & Verifying Diff:
  - Run `git diff` before approving or simplifying code.
  - Check it against Surgical Changes, and reject lingering debug lines.
  
Reply `APPROVE`, or:
```
REQUEST_CHANGES: <one-line reason>
<details, file:line references>
```

After `APPROVE`:
  - Merge with `git merge`.
  - Delete the merged branch and its worktree, if needed.
  - **Never** push to a remote unless the user explicitly asks.

### 4. Sub-agents

**Utilize sub-agents. Define one sub-task per sub-agent.**

When delegating:
  - Assign one sub-task per sub-agent.
  - Give only the sub-task, its verify check, its branch, and the relevant files.
  - Don't delegate trivial tasks.

Sub-agents:
  - Work only on their assigned branch and worktree.
  - No merging, no rebasing `main`, no messaging other agents, no changing the plan.
  - Report only to their parent agent.

Parent agents:
  - Are the boss of their own sub-agents.
  - Verify every result. If it fails, retry or do it yourself.
  - Mark a task done only after verifying it yourself.

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
