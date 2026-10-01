# Agent Collaboration Protocol

This repository is worked on by a human owner, Codex, and Claude Code.

## 1. Source of truth

Priority order:

1. Current code and tests
2. Accepted GitHub Issues/PR decisions
3. `TASKS.md`
4. Agent instruction files (`AGENTS.md`, `CLAUDE.md`)
5. Chat conversations

If two records conflict, stop and document the conflict before making a risky change.

## 2. Work lifecycle

Every non-trivial task follows:

`Issue -> assignment -> branch -> implementation -> tests -> PR -> review -> merge -> TASKS update`

Use one Issue per coherent task. Link the PR to the Issue.

## 3. Branch ownership

Naming:
- Codex: `codex/<task>`
- Claude Code: `claude/<task>`
- Human experiments: `exp/<task>`

Only one agent should own a task branch.

Never force-push another agent's branch.

## 4. File ownership

Default ownership is responsibility-based, not absolute.

Codex normally owns:
- simulation/core physics
- mathematical models
- shared interfaces
- integration

Claude Code normally owns:
- UI/presentation
- tests
- debugging
- tooling
- documentation

If a task crosses these boundaries, define ownership in the Issue before implementation.

## 5. Avoiding collisions

Before editing:
- inspect `git status`
- inspect recent commits
- inspect open PRs
- inspect the Issue
- check whether another agent is touching the same files

If collision is possible, split the task by file/module or wait for the other branch.

## 6. Communication format

Use this compact format in Issues/PR comments:

### STATUS
- Agent:
- Task:
- Branch:
- State: planning | working | blocked | ready

### CHANGED
- ...

### VERIFIED
- Tests/commands:
- Result:

### NEXT
- ...

### BLOCKERS
- None / ...

## 7. Physics integrity

The sandbox is a physics project. Code must not silently turn an approximation into a physical law.

Every simulation feature should distinguish:
- physical assumption
- numerical method
- rendering/visualization choice

If a result disagrees with known physics, prefer exposing the discrepancy and its cause over tuning constants until it looks correct.

## 8. Merge policy

A PR should normally be reviewed by the other agent before merge.

Suggested sequence:
1. Author opens PR.
2. Other agent reads the diff and tests.
3. Reviewer posts findings on the PR.
4. Author fixes issues.
5. Human owner decides whether to merge when the change affects project direction or physical assumptions.

Do not merge merely because tests pass.

## 9. Task board

`TASKS.md` is a lightweight human-readable board. GitHub Issues remain the detailed task records.

Keep `TASKS.md` synchronized after major task completion or reassignment.
