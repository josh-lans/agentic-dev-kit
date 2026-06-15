# agentic-dev-kit

**A battle-tested methodology and template set for building production software with AI agents — without losing quality, context, or control.**

I used this approach to independently ship **three production-grade systems in about a year**, alongside a full-time job — including an enterprise observability platform built to replace a seven-figure commercial contract. Same method each time, across three completely different domains. This repo is that method, generalized so you can use it.

> It's deliberately **tool-agnostic.** I orchestrate Claude, OpenAI Codex, Gemini, and Grok — plus generative pipelines (Meshy, Scenario) and a persistent ops agent — assigning each to the work it does best. The templates work with any AI coding assistant that can read your project files.

---

## Why most AI-assisted development stalls

AI coding tools are powerful but fail in predictable ways once a project outgrows a single session:

1. **Context amnesia** — every session starts fresh and forgets architectural decisions made days ago.
2. **Shortcut bias** — left unconstrained, the model takes the *simplest* path, not the *correct* one.
3. **Scale blindness** — it doesn't inherently know that code hitting 3,000 production systems needs different patterns than a prototype.
4. **Forgotten wiring** — it adds the feature but misses three of the eight files that needed to change.
5. **False confidence** — "done!" when the thing was never actually run and verified.

The fix isn't a better prompt. It's an **operating system for the work.**

---

## The Conductor method

Treat the human as a **conductor** and the AI tools as a section of players. The conductor never plays every instrument — they decompose the work, assign each part to the right player, and hold the standard. Four pillars make it work:

### 1. Multi-agent orchestration
A primary "Conductor" session decomposes a feature into independent sub-tasks and dispatches each to a **sub-agent working in an isolated git worktree** with a bounded file scope and a self-contained brief. Task state lives in a **manifest** (one file per task: status, owner, scope, dependencies). The Conductor reviews each diff, runs acceptance tests, and integrates — and **nothing merges to main without explicit human approval.** Parallel work, zero collisions, one set of hands on the wheel.

### 2. Multi-tool orchestration
No single model is best at everything, so don't pretend otherwise. Route each task to the strongest tool and let them coordinate. (Real example: in a game project, Claude authors asset briefs, Codex generates, and Claude renders them through Meshy and Scenario — covering image/3D generation that a single model can't.) → [`methodology/multi-tool-orchestration.md`](methodology/multi-tool-orchestration.md)

### 3. Living documentation as the system of record
A `.claude-docs/` knowledge layer survives across sessions and tools:
- **MEMORY** — current state, environment, gotchas (read first, every session)
- **STANDARDS** — numbered (`§`) patterns the AI must follow and can cite
- **DEPENDENCIES** — "if you change X, also change Y" impact chains
- **SESSION_LOG / HANDOFFs** — what changed, why, and what's next
- **ADRs** — binding decisions, never re-litigated
- **LESSONS** — real mistakes, documented once so they never recur

### 4. Quality gates that don't bend
- **No partial fixes** — a fix covers every code path, or it isn't done.
- **Pre-merge self-verification** — prove claims by *running the command*, not re-asserting from memory.
- **Automated gates** — tests, lint, and security scanning on every change; human-gated merges.
- **Cross-project intelligence** — proven patterns get ported between codebases instead of reinvented.

→ Full write-up: [`methodology/the-conductor-method.md`](methodology/the-conductor-method.md)

---

## Proven across three domains

The same method produced three very different production systems:

| Project | What it is | Why it's evidence |
|---|---|---|
| **[MonLite](case-studies/monlite.md)** | Enterprise observability platform (SAP, 6 databases, hosts, web) | Replaces a seven-figure contract; scale-validated to 3,000+ systems; embedded air-gapped AI assistant |
| **[Pathfinder](case-studies/pathfinder.md)** | AI-augmented SAP-to-cloud migration decision engine | Multi-LLM authoring + a cumulative-intelligence loop; tamper-evident audit |
| **[Disaster Scenario](case-studies/disaster-scenario.md)** | Cross-platform multiplayer game (Godot) | Multi-agent asset pipeline; deterministic tested core; proves the method travels beyond enterprise software |

*(Case studies are capability-level — architecture and outcomes, no proprietary code.)*

---

## Use the kit

The `templates/` directory is a drop-in `.claude-docs/` system.

**Automated setup** — give your AI assistant [`templates/AI_BOOTSTRAP.md`](templates/AI_BOOTSTRAP.md) and say *"Set up .claude-docs for my project."* It will explore your codebase and generate project-specific docs.

**Manual setup:**
```bash
cp -r templates/.claude-docs your-project/.claude-docs
cp templates/.claude-docs/CLAUDE.md your-project/CLAUDE.md   # or merge into your existing one
```
Then customize `MEMORY.md`, `STANDARDS.md`, and `DEPENDENCIES.md` for your project, and point your tool's config (CLAUDE.md, `.cursorrules`, etc.) at them.

→ Details: [`templates/README.md`](templates/README.md)

---

## About

Built by **Joshua Lans** — a 12-year enterprise infrastructure engineer who turned AI-assisted development into a repeatable way to ship production software.
[github.com/josh-lans](https://github.com/josh-lans) · [linkedin.com/in/joshualans](https://www.linkedin.com/in/joshualans/)

*License: [MIT](LICENSE) — use it, adapt it, ship something.*
