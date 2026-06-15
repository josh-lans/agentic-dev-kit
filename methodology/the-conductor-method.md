# The Conductor Method

A way of working with AI coding agents that keeps velocity high *and* quality production-grade, over months and across multiple tools. Developed and refined across three shipped systems and hundreds of structured sessions.

The mental model: **you are the conductor, the AI tools are the players.** A conductor doesn't play every instrument — they decompose the score, assign each part, set the tempo, and hold the standard. Your leverage isn't typing speed; it's orchestration and judgment.

---

## 1. The session is the unit of work

Every session is bounded and disciplined:

**Start** — read the context layer before writing anything: `MEMORY.md` → `STANDARDS.md` → `DEPENDENCIES.md`, then check git state. No code before context.

**Work** — small, validated increments. Plan complex features *before* implementing (the most expensive mistake is building the wrong thing correctly). Decompose anything large into dispatchable sub-tasks.

**Close** — update the docs that changed (SESSION_LOG, MEMORY, ADRs, ROADMAP), regenerate/verify artifacts, and write a handoff so the *next* session starts informed. Continuity is a feature you build deliberately.

---

## 2. Multi-agent orchestration (parallelism without chaos)

When a feature is decomposable, run sub-agents **in parallel** — safely:

- **Isolation:** each sub-agent works in its own git worktree. A mandatory pre-flight check (`pwd && git worktree list && git branch --show-current`) confirms isolation before any edit; if it's in the main repo, it aborts. (This caught real worktree-degradation incidents — so it's now also re-verified mid-session after each dispatch.)
- **Bounded scope:** each sub-agent gets a *self-contained* brief (zero implied context from the parent conversation) and an explicit `file_scope` / `forbidden_scope`. Touching forbidden files = immediate abort and escalate.
- **Manifest-driven state:** one markdown file per task holds the contract — id, status, owner, branch, file scope, dependencies. The manifest *is* the source of truth; sub-agents update it as they work.
- **Conductor integration:** the Conductor reviews each diff, runs acceptance tests locally, and only then proposes an explicit integration plan to the human.

**The hard rule:** never auto-merge to main, never push without an explicit "go" for *that* push. Hotfixes included. The gate is the point.

---

## 3. Living documentation (the system of record)

Memory lives in files, not in the model's head:

| File | Role | Cadence |
|---|---|---|
| `MEMORY.md` | Current state, environment, gotchas — read first | Every session |
| `STANDARDS.md` | Numbered (`§`) mandatory patterns, citable | When patterns emerge |
| `DEPENDENCIES.md` | Change-impact chains ("change X → also change Y") | When features are wired |
| `SESSION_LOG.md` / HANDOFFs | What changed, why, what's next | End of every session |
| `ADRs` | Binding decisions with context + consequences | Per significant decision |
| `LESSONS.md` | Mistakes that caused rework — never repeat | When mistakes happen |

Standards are **numbered** so the AI can cite them ("per §5, I'll batch-fetch instead of looping"). Decisions become ADRs so they're never re-argued. Mistakes become lessons so they're never repeated.

---

## 4. Quality gates that don't bend

- **No partial fixes.** A fix covers the default path *and* every override path, or it isn't shipped. "Load-bearing" comments rationalizing a broken path are a smell, not a solution.
- **Verify by running, not re-asserting.** Before claiming "done," run the command, query the row, watch the worker tick. Passing unit tests + "looks right" is not verification for anything stateful or UI-facing.
- **Automated gates.** Pre-commit hooks (tests, lint, secret-scan) and CI (tests, SAST, dependency scanning). Never bypass them.
- **Human-gated merges.** The conductor proposes; the human approves; only then does it ship.

---

## 5. Cross-project intelligence

Solved problems shouldn't be re-solved. Maintain a references catalog so a new project can lift a proven subsystem (auth, secrets, provider routing, audit) from an existing one — adapting names/paths, not reinventing. The loop runs both ways: porting code is also a chance to catch a bug the source still has.

---

## Why it works

It directly counters the failure modes of AI-assisted development:

| Failure mode | Countered by |
|---|---|
| Context amnesia | Living documentation, read-first protocol |
| Shortcut bias | Numbered STANDARDS + "no partial fixes" |
| Scale blindness | DEPENDENCIES chains + domain rules in MEMORY |
| Forgotten wiring | Feature/metric wiring checklists |
| False confidence | Verify-by-running + human-gated merges |
| Collisions in parallel work | Worktree isolation + manifest scope |

**The core insight: AI doesn't replace expertise — it amplifies it.** The judgment, the standards, and the architecture are the human's. The tools multiply throughput. The method is what keeps the multiplier pointed at quality.
