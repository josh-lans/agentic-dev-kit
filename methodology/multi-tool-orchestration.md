# Multi-Tool Orchestration

No single AI model is best at everything. Treating one tool as a hammer for every nail leaves capability — and quality — on the table. The Conductor method is deliberately **tool-agnostic**: route each task to the strongest available player, and let them coordinate through the shared documentation layer.

## The roster

Models and tools I orchestrate, each assigned by strength:

- **Claude** — primary reasoning, architecture, code authoring, orchestration (the Conductor itself).
- **OpenAI Codex** — gap-filling where another model struggles; in-repo generation tasks; a second independent reviewer.
- **Gemini / Grok** — alternate perspectives, cross-checking, and capability coverage where they lead.
- **Generative pipelines (Meshy, Scenario)** — text-to-3D and image/texture generation, driven via API.
- **Persistent ops agents (e.g., OpenClaw)** — long-running agents embedded *in the real environment* (not a chat window) that execute commands and diagnostics against live systems and coordinate findings back.

## Patterns

### Assign by strength, not by habit
Pick the tool that's actually best for the task in front of you. Writing async backend logic, generating a 3D asset, and running a diagnostic against a live database are three different jobs for three different players.

### Brief → generate → integrate (cross-tool pipeline)
One model can author a precise brief for another, then consume its output. *Real example:* Claude writes an asset brief → Codex generates → Claude renders the result through Meshy (text-to-3D) and Scenario (image-gen) into game-ready assets. This covers image/3D generation that the reasoning model can't do alone.

### Agents in the real environment
The most powerful agents aren't in a chat tab — they live where the work is. *Real example:* a persistent agent running on a home-lab server (WSL, alongside live SAP systems and databases) executes diagnostics across systems, databases, and monitoring collectors, and coordinates with the authoring model on R&D. The loop closes against reality, not a mock.

### A second model as an independent reviewer
Using a *different* provider to review or refute the first model's work catches failure modes that redundancy within one model can't. Diversity of model is a cheap, effective quality gate.

## What makes it cohere

The shared `.claude-docs/` layer (STANDARDS, MEMORY, DEPENDENCIES, ADRs) is provider-neutral plain text. Any tool that can read project files can pick up the same context and the same rules — so switching or combining tools doesn't reset the project's standards. The documentation is the lingua franca; the human conductor is the integration point.
