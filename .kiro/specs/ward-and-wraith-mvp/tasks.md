# Implementation Plan — Ward & Wraith MVP

Target: submission 2026-12-04. Week markers are guidance for a solo builder.

## Week 1 — Engine you can trust

- [ ] 1. Project skeleton
  - Go module, `cmd/wraith` with subcommands `serve | run-nights | replay | mcp`, MIT license, README stub
  - Add `pgregory.net/rapid` and pure-Go SQLite driver
  - _Requirements: 10.1_

- [ ] 2. Arena core
  - [ ] 2.1 World, nodes, edges, actors, seeded RNG; `Apply(Move)` and `ViewFor(team)`
    - _Requirements: 1.1, 1.3, 1.4, 3.1_
  - [ ]* 2.2 Property test P1 Determinism
  - [ ]* 2.3 Property test P2 Reachability
  - [ ]* 2.4 Property test P3 Fog integrity

- [ ] 3. Localhost Manor map + vulnerability catalog (all 8 vulnerabilities, themed + real labels)
  - _Requirements: 1.1, 1.2_

- [ ] 4. Ledger
  - [ ] 4.1 Hash-chained append-only store on SQLite with `Verify()`
    - _Requirements: 4.1, 4.2, 4.3_
  - [ ]* 4.2 Property test P5 Ledger chain
  - [ ] 4.3 Replay command reproducing final state hash
    - _Requirements: 4.4_
  - [ ]* 4.4 Property test P6 Replay + golden ledger in `testdata/`

## Week 2 — Agents on rails

- [ ] 5. SAR-lite runtime
  - [ ] 5.1 Manifest, lifecycle state machine (GM-only transitions), envelopes
    - _Requirements: 2.1, 2.2, 2.3_
  - [ ] 5.2 Guardrails: allowed moves, budget, rate limit → `Rejected` events
    - _Requirements: 2.4, 2.5_
  - [ ]* 5.3 Property test P4 Guardrail closure

- [ ] 6. Game Master turn loop, win/limit conditions, novelty detection
  - _Requirements: 3.1, 3.2, 3.3_

- [ ] 7. LLM provider interface with Bedrock + mock (scripted naive/greedy/cautious policies)
  - _Requirements: 10.1_

- [ ] 8. Arena MCP server (`look`, `move`, `probe`, `act`, `report`) + `.kiro/settings/mcp.json` registration
  - _Requirements: 3.4_

- [ ] 9. Red and Blue crews (3 roles each) with v1 playbooks
  - _Requirements: 2.1, 3.4_

- [ ] 10. Night themes: 5+ YAML definitions with cumulative unlocks
  - _Requirements: 5.1, 5.2, 5.3_

## Week 3 — Evolution and the whistle

- [ ] 11. Morning loop
  - [ ] 11.1 Debrief summaries per team
    - _Requirements: 6.1_
  - [ ] 11.2 Coach agents produce candidate playbooks (immutable files)
    - _Requirements: 6.2, 6.4_
  - [ ] 11.3 Gate vs current + hall of fame; promotion events
    - _Requirements: 6.3_
  - [ ] 11.4 Elo per team version
    - _Requirements: 6.5_
  - [ ]* 11.5 Property tests P7 Promotion safety, P8 Immutability, P9 Elo conservation

- [ ] 12. Referee
  - [ ] 12.1 PAUSE/RESUME, VETO, APPROVE, RULING + ledger actor `human`
    - _Requirements: 7.1, 7.2, 7.7_
  - [ ] 12.2 Escalations: novel move, large diff, Elo spike; absent-referee defaults
    - _Requirements: 7.3, 7.4, 7.5, 7.6_

## Week 4 — Make it watchable

- [ ] 13. Web UI
  - [ ] 13.1 Manor canvas: floor plan, fog, flashlight, breaches
    - _Requirements: 8.1_
  - [ ] 13.2 SSE live stream + htmx referee controls + whistle freeze
    - _Requirements: 8.2, 8.4_
  - [ ] 13.3 Elo chart
    - _Requirements: 8.3_

- [ ] 14. Commentary: narrator + caster with themed/real pairs, templated fallback, silence on whistle
  - _Requirements: 9.1, 9.2, 9.3, 9.4_

- [ ] 15. Demo mode: 7 nights end-to-end under 5 minutes, no credentials required
  - _Requirements: 10.1, 10.2, 10.3_

- [ ] 16. Submission package
  - Public GitHub repo with visible license, `.kiro/` committed
  - Kiro usage write-up (specs, steering, hooks, custom agents, MCP, PBT)
  - Blog post + #KiroweenHackathon26 social post
  - 3-minute video built around the three must-capture moments
  - _Requirements: 10.3_

## Stretch
- [ ]* 17. Narrator/caster voice
- [ ]* 18. Playbook diff view in UI
- [ ]* 19. Referee INJECT scenarios
- [ ]* 20. Custom maps ("bring your own system")
- [ ]* 21. Kiro Crew app wrapper in a separate repo (nightly cron = nights)
