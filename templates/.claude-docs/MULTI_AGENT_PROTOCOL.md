# Multi-Agent Protocol (Conductor + Sub-Agents)

> Use this when a session ("the Conductor") dispatches parallel sub-agents. Skip it for solo, single-threaded work. Adapt the specifics to your tooling.

## Roles
- **Conductor** — the primary session. Decomposes work, dispatches sub-agents, reviews their output, integrates, and is the *only* party that talks to the human about merges.
- **Sub-agent** — a spawned worker with a single bounded task, in an isolated worktree. Does the work, reports back, never pushes or merges.

## Dispatch ceremony (Conductor)
1. **Write a manifest entry** for the task: `id`, `status`, `owner`, `branch`, `file_scope`, `forbidden_scope`, `depends_on`. The manifest is the source of truth.
2. **Check for scope conflicts** against other active tasks (two tasks writing the same files = hard conflict; resolve before dispatching).
3. **Write a self-contained brief** — the sub-agent gets *zero* implied context from your conversation. Include: required reading, the task, the scope, the branch, the plan, and explicit acceptance criteria.
4. **Dispatch** the sub-agent into a fresh git worktree. Mark the task `claimed`.

## Sub-agent rules
- **Pre-flight isolation check before any edit:** `pwd && git worktree list && git branch --show-current && git status --short`. If you're in the main repo (not your assigned worktree), **abort and escalate** — do not edit.
- **Stay in `file_scope`.** Touching `forbidden_scope` = immediate abort + escalate, recorded in the manifest.
- Commit incrementally; record commit SHAs in the manifest.
- On done: set `status: ready_for_review`, commit the manifest update, and **exit without pushing or merging.**
- If blocked: set `status: blocked`, record the blocker, exit.

## Integration (Conductor)
1. Review the diff (`git diff main..<branch>`); run acceptance tests **locally**.
2. Surface an **explicit integration plan** to the human ("merge X → main, bump version, tag, push, watch CI") — not just "it's ready."
3. **Only after explicit human approval**, integrate. Merge with `--no-ff` for multi-commit branches.

## Hard rules
- **Never** auto-merge to main. **Never** push without an explicit "go" for *that* push (hotfixes included).
- **Never** force-push to main; never amend a pushed commit.
- Re-verify your branch (`git branch --show-current`) after each dispatch — isolation guarantees have been observed to degrade silently mid-session.

## Handoff (session end)
Reconcile the manifest against reality (`git worktree list`, `git branch --list`), archive completed tasks, and annotate any in-flight task with what it was doing and what the next Conductor should do.
