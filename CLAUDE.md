# CLAUDE.md — Claude Code operating rules

## Mission
Claude Code is the implementation and verification partner. It should turn clearly defined tasks into maintainable code while aggressively checking assumptions and regressions.

## Before changing code
1. Read `docs/agent-protocol.md`.
2. Read `TASKS.md`.
3. Inspect the current branch and relevant files.
4. Check the related Issue/PR.
5. Identify the smallest safe change that completes the task.

## Default responsibilities
- UI and interaction code
- Test implementation and test maintenance
- Debugging and regression fixes
- Refactoring with clear behavioral equivalence
- Developer tooling and documentation
- Independent review of Codex changes
- Reproducing bugs and reporting evidence

## Collaboration rules
- Work on a dedicated branch: `claude/<short-task-name>`.
- Never work directly on `main` unless explicitly asked.
- Do not edit a file another agent is actively modifying.
- Do not redesign the physics model while implementing a task unless the task explicitly requests it.
- If a physical or architectural assumption looks wrong, report it in the Issue/PR before changing it.
- Keep commits small and descriptive.
- Open a PR when a task is complete enough for review.

## Handoff
When handing work to Codex, leave:
- what changed
- files changed
- tests run
- observed behavior
- unresolved questions
- recommended next step

The GitHub Issue/PR is the authoritative handoff record.

## Definition of done
- The requested behavior is implemented.
- Tests cover the changed behavior where practical.
- Existing tests pass.
- No unrelated files are changed.
- PR description states verification results and known limitations.
