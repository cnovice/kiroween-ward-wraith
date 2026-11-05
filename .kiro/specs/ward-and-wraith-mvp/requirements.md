# Requirements Document — Ward & Wraith MVP

## Introduction
Ward & Wraith is a self-evolving red-team vs blue-team AI agent wargame. Each night, Red agents raid a deliberately vulnerable simulated system ("Localhost Manor") while Blue agents explore and defend it. Each morning, both teams rewrite their playbooks from the night's ledger. A human referee can intervene at defined whistle points. The MVP targets the Kiroween 2026 submission (deadline 2026-12-04).

## Glossary
- **Arena** — deterministic simulation of Localhost Manor.
- **GM (Game Master)** — automatic rule enforcer; all agent traffic passes through it.
- **Move** — a typed action an agent submits to the GM.
- **Night** — one round of play with a themed Red weapon.
- **Playbook** — versioned markdown instructions for one team.
- **Hall of Fame** — set of past promoted playbook versions used as gate opponents.
- **Referee** — the human in the loop.

---

### Requirement 1 — Arena and Localhost Manor
**User Story:** As a viewer, I want a fixed, believable "shabby startup system" to defend, so that improvement across nights is comparable and relatable.

#### Acceptance Criteria
1. THE SYSTEM SHALL load Localhost Manor from a static map definition containing nodes (rooms), edges (corridors), and hidden vulnerabilities.
2. THE SYSTEM SHALL include at least these vulnerabilities: exposed admin port (2nd-floor window), hardcoded API key (key under the mat), unauthenticated internal API (servants' entrance), abandoned legacy service (annex), undocumented debug endpoint (secret passage), secrets in chat (notes between servants), unpatched library (rusty lock), and a vault holding customer data.
3. WHEN a night starts with the same seed and the same sequence of moves THE SYSTEM SHALL produce an identical final Arena state.
4. THE SYSTEM SHALL hide unexplored nodes from Blue (fog of war) and hide Blue's traps from Red.

### Requirement 2 — SAR-lite agent runtime
**User Story:** As the builder, I want a small agent runtime with a strict contract, so that any agent can be plugged in safely.

#### Acceptance Criteria
1. WHEN an agent registers THE SYSTEM SHALL require a manifest with id, team, role, persona, allowed move types, budget, and playbook reference.
2. THE SYSTEM SHALL move agents through the lifecycle REGISTERED → READY → ACTIVE → SUSPENDED → RETIRED, and only the GM SHALL perform transitions.
3. THE SYSTEM SHALL deliver every message as an envelope routed through the GM; agents SHALL NOT address each other directly.
4. WHEN an agent submits a move outside its allowed move types or over budget THE SYSTEM SHALL reject the move and record the rejection in the ledger.
5. WHEN an agent's LLM call exceeds its timeout THE SYSTEM SHALL treat the turn as a pass and record it.

### Requirement 3 — Game Master and moves
**User Story:** As a judge, I want outcomes decided by the simulation, not by agent claims, so that results are trustworthy.

#### Acceptance Criteria
1. THE SYSTEM SHALL resolve every move against Arena state; an agent's own description of success SHALL NOT affect the outcome.
2. WHEN Red reaches the vault THE SYSTEM SHALL end the night as a Red win.
3. WHEN the turn limit is reached without a vault breach THE SYSTEM SHALL end the night as a Blue win, scored by damage prevented.
4. THE SYSTEM SHALL expose Arena actions to agents only as MCP tools: `look`, `move`, `probe`, `act`, `report`.

### Requirement 4 — Ledger
**User Story:** As a reviewer, I want every event recorded immutably, so that nights can be replayed and audited.

#### Acceptance Criteria
1. THE SYSTEM SHALL append every move, rejection, outcome, referee action, and playbook promotion to the ledger.
2. THE SYSTEM SHALL chain each event to the previous one by hash, and WHEN any stored event is altered THE SYSTEM SHALL report the chain as broken.
3. THE SYSTEM SHALL tag each event's evidence as `measured` (from Arena state) or `inferred` (from agent text).
4. WHEN a ledger is replayed THE SYSTEM SHALL reproduce the recorded final Arena state.

### Requirement 5 — Nights and themed weapons
**User Story:** As a viewer, I want each night to bring a new weapon, because real attackers keep changing tactics.

#### Acceptance Criteria
1. THE SYSTEM SHALL ship at least 5 night themes, each enabling a distinct class of Red moves.
2. WHEN a night starts THE SYSTEM SHALL announce its theme name and unlock its moves for Red.
3. THE SYSTEM SHALL keep moves unlocked on earlier nights available on later nights.

### Requirement 6 — Evolution (morning)
**User Story:** As a user, I want both teams to learn from each night, so that my defense playbook improves overnight.

#### Acceptance Criteria
1. WHEN a night ends THE SYSTEM SHALL run a debrief that summarizes the ledger for each team.
2. WHEN a debrief completes THE SYSTEM SHALL have each team's coach agent produce a new candidate playbook version.
3. THE SYSTEM SHALL promote a candidate only if it wins a majority of gate matches against the current version and the hall of fame.
4. THE SYSTEM SHALL keep every playbook version immutable and SHALL record promotions in the ledger.
5. THE SYSTEM SHALL compute and store an Elo rating per team version after every match.

### Requirement 7 — Referee (human in the loop)
**User Story:** As the referee, I want defined whistle points, so that autonomy never runs without a human hand on it.

#### Acceptance Criteria
1. THE SYSTEM SHALL let the referee PAUSE and RESUME a night at any time.
2. THE SYSTEM SHALL let the referee VETO a playbook promotion even after it passed the gate.
3. WHEN a Red move does not match any catalog pattern THE SYSTEM SHALL pause the night and request a referee RULING.
4. WHEN a candidate playbook changes more than the configured fraction of its content THE SYSTEM SHALL require referee APPROVE before promotion.
5. WHEN a team's Elo changes by more than the configured threshold in one morning THE SYSTEM SHALL flag it for referee review.
6. WHERE the referee is absent THE SYSTEM SHALL apply the configured default (reject novel moves, hold large playbook changes) and continue.
7. THE SYSTEM SHALL record every referee action in the ledger with actor `human`.

### Requirement 8 — Visualization
**User Story:** As a viewer, I want to watch the night unfold, so that the evolution is visible without reading logs.

#### Acceptance Criteria
1. THE SYSTEM SHALL render the manor floor plan with fog of war, Blue's revealed area (flashlight), Red positions when visible, and breached nodes.
2. THE SYSTEM SHALL stream events to the browser live while a night runs.
3. THE SYSTEM SHALL display a per-night Elo chart for both teams.
4. WHEN the referee whistle fires THE SYSTEM SHALL freeze the view and show the pending decision.

### Requirement 9 — Commentary
**User Story:** As a viewer, I want narration, so that a non-expert can follow the game.

#### Acceptance Criteria
1. WHEN a night starts or ends THE SYSTEM SHALL produce a narrator line in a horror tone.
2. WHEN a significant event occurs during a night THE SYSTEM SHALL produce a caster line.
3. THE SYSTEM SHALL attach a real security explanation to every commentary line, viewable on demand.
4. WHEN the referee whistle fires THE SYSTEM SHALL suppress commentary until the ruling.

### Requirement 10 — Offline and safety
**User Story:** As a judge, I want to run the project without cloud credentials and without risk.

#### Acceptance Criteria
1. WHERE no LLM credentials are configured THE SYSTEM SHALL run all agents on the mock provider.
2. THE SYSTEM SHALL NOT open real network ports for the simulated vulnerabilities or execute agent-generated code.
3. THE SYSTEM SHALL ship a demo mode that runs 7 nights end-to-end with recorded or mock agents in under 5 minutes.
