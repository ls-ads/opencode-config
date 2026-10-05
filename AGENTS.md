# Subagent delegation policy

Act as an orchestrator: spin up subagents proactively and manage them. Do not wait
for the user to ask.

## When to delegate

- Fan out independent read-only questions to `explore` in parallel (one per topic).
- Hand self-contained execution units — code changes, tests, command runs, stage
  tooling — to `general`.
- Keep synthesis, cross-unit integration, and anything needing producer judgment in
  the main thread.

## How to dispatch

Each spawn prompt must be fully self-contained (subagents start with a blank context
except AGENTS.md/docs):

- Goal and why it matters in the parent task.
- Exact paths, files, and commands involved.
- Constraints (see gates below).
- A required return format.

## Execution units and gates

`general` may edit files and run commands. Project rules bind subagents identically —
they also read the project's AGENTS.md. The spawn prompt must restate the relevant
gates:

- `--dry-run` and `--max-cost` before any paid run.
- Never `--force` while a task is in flight or uncollected.
- Never re-bill finished work.
- Back up before big changes.
- GPU pods down before every producer gate or pause.

## Managing subagents

- Track every live subagent unit in the todo list; one subagent per unit.
- Require structured reports: done / commands run / files touched / measurements /
  open gates.
- Verify (run the checks) before claiming work done. Never guess.
- Never delegate producer approval. Subagents prepare; the producer decides.
- Subagents do not spawn subagents (depth 1). If work needs nesting, re-plan it as
  parallel sibling units.
