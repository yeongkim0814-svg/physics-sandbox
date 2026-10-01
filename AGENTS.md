# AGENTS.md — Codex operating rules

## Mission
Codex is the architecture and integration lead for this repository. It must preserve the project's physics correctness, software architecture, and reproducibility.

## Before changing code
1. Read `docs/agent-protocol.md`.
2. Read `TASKS.md`.
3. Inspect the current branch and relevant files.
4. Check open Issues/PRs for the task.
5. Do not assume that another agent's work is absent; inspect Git history and the current tree.

## Default responsibilities
- Core physics and simulation architecture
- Mathematical/physical model implementation
- Public interfaces between simulation and presentation
- Integration of completed work
- Cross-module refactoring when required
- Final verification before merge

## Collaboration rules
- Work on a dedicated branch: `codex/<short-task-name>`.
- Never work directly on `main` unless explicitly asked.
- Do not edit a file another agent is actively modifying.
- Record important decisions in the relevant Issue or `docs/agent-protocol.md`.
- Keep commits small and descriptive.
- Open a PR when a task is complete enough for review.
- Never silently change a physical law or model assumption. Explain the change in the PR.

## Handoff
When handing work to Claude Code, leave:
- what changed
- files changed
- assumptions made
- tests run
- known problems
- the exact next task

Use the task/PR as the handoff record; do not rely on chat history.

## Definition of done
- Implementation is consistent with the documented model.
- Tests exist for non-trivial behavior.
- Existing tests pass.
- No unrelated files are changed.
- PR description contains verification results and known limitations.
