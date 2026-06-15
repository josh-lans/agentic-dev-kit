# `.claude-docs` Templates

> Drop-in templates for maintaining project context, standards, and control across AI coding sessions — with any tool (Claude, Codex, Cursor, Copilot, Gemini, …).

## Quick start

**Automated (recommended):** give your AI assistant [`AI_BOOTSTRAP.md`](AI_BOOTSTRAP.md) and say *"Set up .claude-docs for my project."* It will explore your codebase and generate project-specific docs.

**Manual:**
```bash
cp -r .claude-docs your-project/.claude-docs
cp .claude-docs/CLAUDE.md your-project/CLAUDE.md   # or merge into your existing root config
```
Then point your tool's config at them (Claude Code reads `CLAUDE.md` automatically; for Cursor add to `.cursorrules`; for others, paste `MEMORY.md` at session start).

## What's in here

| File | Role | Update cadence |
|---|---|---|
| `CLAUDE.md` | Project root config — first-action reading list + critical rules | Once, then as rules emerge |
| `.claude-docs/MEMORY.md` | Current state, environment, gotchas — read first every session | Every session |
| `.claude-docs/STANDARDS.md` | Numbered (`§`) mandatory patterns the AI can cite | When patterns emerge |
| `.claude-docs/DEPENDENCIES.md` | Change-impact chains ("change X → also change Y") | When features are wired |
| `.claude-docs/SESSION_LOG.md` | Session history for continuity | End of every session |
| `.claude-docs/SESSION_PROTOCOL.md` | Session start/end discipline | Once, then rarely |
| `.claude-docs/LESSONS.md` | Mistakes that caused rework — never repeat | When mistakes happen |
| `.claude-docs/MULTI_AGENT_PROTOCOL.md` | Conductor + sub-agent orchestration (parallel work) | Once; use when dispatching sub-agents |

## How to adapt

The templates are intentionally generic — they ship with placeholders, not someone else's project. Customize:
1. `MEMORY.md` — your real project context, environment, versions.
2. `STANDARDS.md` — start with 5–10 patterns, number them, grow over time.
3. `DEPENDENCIES.md` — map your own change-impact chains.
4. `LESSONS.md` — start empty; populate as you learn.
5. `CLAUDE.md` — adapt the critical-rules and project-identity sections.

See [`../methodology/the-conductor-method.md`](../methodology/the-conductor-method.md) for the thinking behind all of this.
