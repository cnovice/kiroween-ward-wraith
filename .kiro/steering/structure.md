---
inclusion: always
---

# Project Structure

```
ward-and-wraith/
├── cmd/
│   └── wraith/              # main binary: serve | run-nights | replay | mcp
├── internal/
│   ├── arena/               # deterministic world: map, fog of war, move resolution
│   ├── manor/               # Localhost Manor map data + vulnerability catalog
│   ├── gm/                  # Game Master: turn loop, legality checks, adjudication
│   ├── sar/                 # SAR-lite: agent registry, lifecycle, envelopes, guardrails
│   ├── ledger/              # append-only hash-chained event log
│   ├── crew/                # red/blue agent roles, prompts, move parsing
│   ├── evolve/              # debrief, coach rewrite, hall-of-fame gate, Elo
│   ├── referee/             # human whistle points + escalation rules
│   ├── commentary/          # narrator + caster agents
│   ├── mcpserver/           # Arena exposed as MCP tools
│   ├── llm/                 # provider interface: bedrock, mock
│   └── web/                 # HTTP, SSE, htmx templates, static canvas JS
├── playbooks/
│   ├── red/                 # versioned red playbooks (markdown + front matter)
│   └── blue/                # versioned blue playbooks
├── nights/                  # night theme definitions (YAML)
├── testdata/                # golden ledgers for replay tests
└── .kiro/                   # specs, steering, hooks, agents (never gitignored)
```

## Conventions
- Package names are single lowercase words; no `util`/`common` packages.
- Engine packages (`arena`, `manor`, `ledger`, `gm`) must not import `llm`, `web` or `crew`. Dependency direction: `web → gm → arena`.
- All IDs are typed (`type AgentID string`, `type NodeID string`), never bare strings across package boundaries.
- Errors: wrap with `%w`; engine never panics on agent input — illegal moves become `Rejected` ledger events.
- Themed text and real meaning travel together: every narratable event has `Themed` and `Real` fields.
- Playbook files: `playbooks/<team>/v<N>.md`, immutable once written; promotion is recorded in the ledger, not by renaming files.
