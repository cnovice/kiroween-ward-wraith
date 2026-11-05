---
inclusion: always
---

# Tech Stack

## Languages and runtime
- **Go** (latest stable) for the engine, Game Master, ledger, MCP server and HTTP server. Single static binary is the goal.
- **Web UI**: server-rendered Go templates + **htmx** for controls, plain **Canvas 2D** (vanilla JS, no framework) for the manor view. Live updates over **SSE**.
- **Storage**: SQLite (pure-Go driver) for ledger, playbook versions and Elo history. Append-only tables for the ledger.

## AI agents (runtime, inside the app)
- Red/Blue crew members, coaches and commentators are LLM agents called from Go through a provider interface (`llm.Provider`).
- Default provider: **Amazon Bedrock** (Claude models). A deterministic **mock provider** must exist so the whole game runs offline and in tests.
- Agents never touch the Arena directly. They act only through the Arena **MCP tools** (`look`, `move`, `probe`, `act`, `report`) brokered by the Game Master.

## Kiro usage (dev time) — this is judged, keep it deliberate
- **Specs** for every feature; property-based tests for engine invariants.
- **Steering** (this folder) for standards. Team playbooks at runtime live in `playbooks/` and use the same front-matter style so they read like steering docs.
- **Hooks** in `.kiro/hooks/` (v1 JSON schema, PascalCase triggers).
- **Custom agents** in `.kiro/agents/` for dev roles (engine builder, rules auditor).
- **MCP**: the Arena MCP server is also registered in `.kiro/settings/mcp.json` so Kiro itself can play/inspect the game during development.

## Testing
- Unit tests with the standard `testing` package.
- Property-based tests with `pgregory.net/rapid`.
- Golden-replay tests: a recorded ledger must replay to the identical final Arena state.

## Constraints
- Deterministic engine: same seed + same move list ⇒ same state. No wall-clock or map-iteration nondeterminism in engine code.
- Every external LLM call has a timeout, a token budget and a fallback to the mock provider.
- No secrets in the repo. Credentials come from env vars only.
- Repository must be public with an OSI license (MIT). `.kiro/` must never be gitignored.

## Pre-existing work disclosure
Architecture concepts (Game Master routing, guardrails, append-only ledger, module lifecycle) are inspired by the author's internal platform design. **No code is reused**; all code is written during the Kiroween submission period.
