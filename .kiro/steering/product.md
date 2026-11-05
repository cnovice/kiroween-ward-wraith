---
inclusion: always
---

# Product: Ward & Wraith

> *Every night, the wraiths of Localhost Manor return — smarter.*

## One-liner
Red-team AI agents (wraiths) raid a deliberately shabby system every night. Blue-team AI agents (hunters) explore, defend, and learn by morning. Both teams evolve. A human referee holds the whistle.

## Problem
- Security is the #1 pain point for teams shipping AI agents, and the area where tooling delivers the least value today.
- Small teams (startups, solo CTOs) cannot staff a red team and a blue team.
- AI-assisted attackers keep bringing new weapons; static checklists do not keep up.

## Target user
A small team with no dedicated security staff. They describe (or pick) a system layout, let the wargame run overnight, and walk away in the morning with an **evolved defense playbook**.

## Core loop
1. **Night** — Red wraiths enter from outside with that night's themed weapon. Blue hunters explore a fog-of-war map and defend.
2. **Morning** — The ledger is autopsied. Each team's coach rewrites its own playbook.
3. **Gate** — A new playbook must beat past versions (hall of fame) before promotion.
4. **Whistle** — A human referee can pause, veto, approve, rule, or inject at any time.

## World (Halloween theme is structural, not cosmetic)
- **Localhost Manor** = the system to defend. Fixed map, intentionally shabby (open ports, keys under the mat, forgotten annex, secret debug passage).
- **Rooms** = services, **corridors** = network paths, **cellar** = database, **vault** = customer data (win condition for Red).
- **Night N** = one round, each with a named attack theme (e.g. "The Open Window", "The False Courier", "The Poisoned Gift", "The Whispering Mirror").
- **Commentary**: a horror narrator speaks only at dusk/dawn; a sports caster calls live action. Silence on the referee whistle.
- Every themed line can be expanded to its real security meaning (e.g. "2nd-floor window breached" → "admin console port exposed").

## Hackathon positioning
- Kiroween 2026, category **Frankenstein**: security wargame + horror exploration game + sports broadcast + self-evolving agents + human referee.
- Also eligible: Best Startup Project (WINGSIDE uses this internally).
- Judging is 5 × 20%: Impact, Kiro implementation depth, Design, Innovation, Presentation (3-min video).

## Non-goals
- No real exploits, no real network attacks. The Arena is a deterministic simulation.
- No model training. "Evolution" = rewriting playbooks (prompt/steering-style documents) and module catalogs.
- No custom user maps in the MVP (stretch goal).

## Must-capture demo moments
1. **Night 1 massacre** — Blue knows nothing; the vault falls.
2. **The whistle** — Red invents a move not in the catalog; the screen freezes; the referee rules.
3. **Night 7** — the same attack returns and is stopped; both Elo curves have risen.
