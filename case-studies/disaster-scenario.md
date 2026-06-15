# Case Study — Disaster Scenario

*A cross-platform multiplayer game, built with the Conductor method — proof it travels beyond enterprise software.*

## Why it matters here
The first two case studies are enterprise platforms. This one is a **comedic multiplayer deckbuilder game** for Steam and mobile. Completely different domain, completely different tech — same methodology. That's the point: the Conductor method isn't tied to one kind of software.

## What was built
A polished party game built in **Godot 4 (GDScript)** — roughly **39,000 lines of code** — with a clean separation between a pure rules engine, presentation, and networking.

**Notable engineering:**
- **A deterministic, unit-tested rules core** cleanly isolated from rendering and networking, guarded by **14 CI test gates** (hundreds of assertions each).
- **Online multiplayer over ENet** with masked state snapshots and a turn timer; a deliberate **zero-AI-at-runtime** design keeps cost and latency out of gameplay (humans judge each other; the app just scores).
- **A multi-agent asset pipeline** — Claude authors asset briefs, Codex generates, and Claude renders them through Meshy (text-to-3D) and Scenario (image generation) — producing 21 fully-realized 3D scenes.
- **Cross-platform export** to Steam and mobile from a single codebase.

## How the method showed up
Same `.claude-docs/` discipline (architecture decision records, per-session handoffs, standards), same human-gated workflow — adapted to game development. It's also the clearest example of **multi-tool orchestration**: the asset pipeline only works because each tool is assigned to what it does best, coordinated by the human conductor. When AI-driven visual placement proved unreliable, the response was to build a live in-game alignment editor — a reminder that the method values human judgment over forcing imperfect AI output.
