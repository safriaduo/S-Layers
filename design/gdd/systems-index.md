# Systems Index: S-Layers

> **Status**: Draft
> **Created**: 2026-05-23
> **Last Updated**: 2026-05-23
> **Source Concept**: design/gdd/game-concept.md

> **Technical Director Review (TD-SYSTEM-BOUNDARY)**: CONCERNS resolved 2026-05-23 — four boundary fixes applied (Combat scope narrowed, Dice Economy owns activation cost notation, Integrity System decoupled from Equipment, Items discard with card on degradation).
> **Producer Review (PR-SCOPE)**: UNREALISTIC noted 2026-05-23 — 9 MVP GDDs in 3 months is aggressive for a solo designer doing parallel work. Designer accepted the risk. Recommendation to move Game Setup and Communication Protocol to MVP was declined. Mitigation: draft informal playtest setup notes before first external playtest session regardless of GDD status.
> **Creative Director Review (CD-SYSTEMS)**: CONCERNS 2026-05-23 — Item System promoted to MVP (Concern 1, HIGH). Concerns 2-4 recorded as GDD-level notes below.

---

## Overview

S-Layers is a cooperative board game built on five interlocking rule modules: dice placement, equipment degradation, item modification, enemy resolution, and session structure. The game's mechanical depth emerges from the intersection of these systems — no single system delivers the core fantasy alone. The design priority is Foundation-first: the dice economy and integrity model are the deepest invariants in the game, and every other system references them. Eight systems are required for a working MVP prototype; four more complete the full game. The core pillars — Agency Over Dice, The Build is Never Finished, Fast and Focused, Cooperative Ownership, Combos Must Be Discoverable — constrain every design decision in every GDD.

---

## Systems Enumeration

| # | System Name | Category | Priority | Status | Design Doc | Depends On |
|---|-------------|----------|----------|--------|------------|------------|
| 1 | Dice Economy | Foundation | MVP | Designed | [design/gdd/dice-economy.md](dice-economy.md) | — |
| 2 | Integrity System | Foundation | MVP | Designed | [design/gdd/integrity-system.md](integrity-system.md) | — |
| 3 | Win/Loss Conditions | Foundation | MVP | Designed | [design/gdd/win-loss-conditions.md](win-loss-conditions.md) | — |
| 4 | Equipment System | Core | MVP | Designed | [design/gdd/equipment-system.md](equipment-system.md) | Dice Economy |
| 5 | Angel System | Core | MVP | Not Started | — | Dice Economy, Integrity System |
| 6 | Character System | Core | MVP | Not Started | — | Equipment System, Dice Economy |
| 7 | Equipment Degradation | Core | MVP | Not Started | — | Equipment System, Integrity System |
| 8 | Combat System | Feature | MVP | Not Started | — | Dice Economy, Integrity System, Equipment System, Equipment Degradation, Character System, Angel System |
| 9 | Item System | Feature | MVP | Not Started | — | Equipment System, Equipment Degradation |
| 10 | Events System | Feature | Vertical Slice | Not Started | — | Combat System, Item System, Win/Loss Conditions |
| 11 | Game Setup | Polish | Alpha | Not Started | — | Character System, Equipment System, Events System |
| 12 | Communication Protocol | Polish | Alpha | Not Started | — | Combat System, Character System |

---

## Categories

| Category | Description | S-Layers Systems |
|----------|-------------|-----------------|
| **Foundation** | Core rules that all other systems reference | Dice Economy, Integrity System, Win/Loss Conditions |
| **Core** | The mechanical building blocks of every encounter | Equipment System, Equipment Degradation, Character System, Angel System |
| **Feature** | Systems that orchestrate or extend the core mechanics | Combat System, Item System, Events System |
| **Polish** | Setup, table protocol, and player-facing meta-rules | Game Setup, Communication Protocol |

---

## Ownership Statements (post-TD-SYSTEM-BOUNDARY review)

These ownership boundaries were tightened after the TD-SYSTEM-BOUNDARY review. Each GDD must stay within its stated ownership — any rule that falls outside it belongs in another system's GDD.

| System | Owns | Does NOT Own |
|--------|------|-------------|
| **Dice Economy** | Probability model (2D6 player pool, 3D6→middle enemy roll), die value distributions, activation cost notation AND meaning ([Dice] syntax) | Which equipment uses which costs (Equipment owns that) |
| **Integrity System** | Abstract health/damage/destruction model — integrity points, damage routing rules (cannot split), destruction trigger | Equipment reveal event (Degradation owns that); Angel-specific thresholds (Angel owns that) |
| **Win/Loss Conditions** | Game termination triggers — victory (defeat God), defeat (any player board destroyed) | How "destroyed" is mechanically achieved (Integrity owns that) |
| **Equipment System** | Slot structure (Head/Body/Weapon), equipment card anatomy, dice-placement-onto-equipment rules, how placed dice activate effects | Activation cost notation (Dice Economy owns that); layer structure (Degradation owns that) |
| **Equipment Degradation** | Layer reveal rules, what each layer must contain, layer ordering design principles, equipment reveal event, item-discard-with-card rule | Integrity point mechanics (Integrity System owns that) |
| **Character System** | Player board design, divine gifts (once-per-turn), starting equipment assignment, character differentiation | Equipment card content (Equipment owns that) |
| **Angel System** | Angel board structure, die-face→action mapping, Angel integrity thresholds, enemy action resolution order, multiple-Angel rules | Player-side dice placement (Equipment owns that) |
| **Combat System** | Turn phase sequence and inter-system handoffs only: enemy roll phase → action assignment phase → player roll phase → dice placement phase → player resolution phase → enemy resolution phase | Dice-placement rules (Equipment owns that); enemy action order (Angel owns that) |
| **Item System** | Item acquisition rules, attachment rules (item → specific equipment), modifier syntax, stacking rules | What happens to items on degradation (Degradation owns that: items are discarded with the card) |
| **Events System** | Event deck composition rules, draw and resolution rules, event type taxonomy (boon/bad/encounter), read-aloud format, God card as last-card trigger | Combat rules (Combat owns that); item rules (Item owns that) |
| **Game Setup** | Deck construction, starting equipment dealing, character selection, first-turn structure | Ongoing game rules (other systems own those) |
| **Communication Protocol** | What information players may freely share vs. must keep private; any communication constraints | When players take actions (Combat owns that) |

---

## Priority Tiers

| Tier | Definition | S-Layers Milestone | System Count |
|------|------------|-------------------|--------------|
| **MVP** | Required for the core loop to function — without these, cannot test "is this fun?" | Physical prototype, internal playtest | 9 |
| **Vertical Slice** | Required for a complete 60-90 min session with full session structure | Kickstarter Demo (months 8-10) | 1 |
| **Alpha** | Setup and table protocol — needed before external blind playtests | Pre-KS external playtests | 2 |
| **Full Vision** | N/A — all systems fall into earlier tiers | — | 0 |

---

## Dependency Map

### Foundation Layer (no dependencies — design first)

1. **Dice Economy** — The game's deepest invariant. Die value distributions and activation cost notation anchor every other system. Highest downstream risk: changing this after Equipment or Character are designed requires cascading rewrites.
2. **Integrity System** — The abstract health model. Decoupled from equipment by design — integrity works for players, Angels, and any future objects with HP.
3. **Win/Loss Conditions** — Termination rules. Simple and self-contained; designed early to make every playtest interpretable.

### Core Layer (depends on Foundation only)

4. **Equipment System** — depends on: Dice Economy (activation costs reference die values)
5. **Angel System** — depends on: Dice Economy (die-face action mapping), Integrity System (Angel integrity thresholds)
6. **Character System** — depends on: Equipment System (characters have slots), Dice Economy (divine gifts interact with dice)
7. **Equipment Degradation** — depends on: Equipment System (layers exist within slots), Integrity System (integrity loss triggers reveal)

*Note: Systems 5, 6, and 7 can be designed in parallel after Foundation is complete.*

### Feature Layer (depends on Core)

8. **Combat System** — depends on: all Foundation + all Core systems (it orchestrates them). Owns ONLY the turn phase sequence — does not redefine dice-placement or enemy action ordering rules.
9. **Item System** — depends on: Equipment System (items modify equipment), Equipment Degradation (items discard with card on reveal)
10. **Events System** — depends on: Combat System (encounter events trigger combat), Item System (treasure events give items), Win/Loss Conditions (last-card boss defeat = win)

### Polish Layer (depends on Features)

11. **Game Setup** — depends on: Character System, Equipment System, Events System
12. **Communication Protocol** — depends on: Combat System, Character System

---

## Recommended Design Order

| Order | System | Priority | Layer | Est. Effort | Notes |
|-------|--------|----------|-------|-------------|-------|
| 1 | Dice Economy | MVP | Foundation | S | Design first — highest downstream risk |
| 2 | Integrity System | MVP | Foundation | S | Can be parallel with #1 |
| 3 | Win/Loss Conditions | MVP | Foundation | S | Can be parallel with #1-2 |
| 4 | Equipment System | MVP | Core | M | Requires Dice Economy complete |
| 5 | Angel System | MVP | Core | M | Can be parallel with #6, #7 |
| 6 | Character System | MVP | Core | M | Can be parallel with #5, #7 |
| 7 | Equipment Degradation | MVP | Core | M | Can be parallel with #5, #6 |
| 8 | Item System | MVP | Feature | M | Can be parallel with Combat — requires #4, #7 |
| 9 | Combat System | MVP | Feature | L | Requires all Core complete; can be parallel with #8 |
| 10 | Events System | Vertical Slice | Feature | M | Requires #9, #8, #3 |
| 11 | Game Setup | Alpha | Polish | S | Requires #6, #4, #10 |
| 12 | Communication Protocol | Alpha | Polish | S | Requires #8, #5 |

*Effort estimates: S = 1 design session, M = 2-3 sessions, L = 3-4 sessions. A session produces a complete GDD section or a complete GDD for small systems.*

---

## Circular Dependencies

None found. The Foundation → Core → Feature → Polish dependency flow is strictly one-directional.

---

## High-Risk Systems

| System | Risk Type | Risk Description | Mitigation |
|--------|-----------|-----------------|------------|
| Dice Economy | Design | 3D6-middle distribution weights toward 3-4; Angel faces 1 and 6 will rarely trigger. If activation costs are calibrated to a wrong distribution, balance fails across all downstream GDDs. | Build Monte Carlo simulation (1 week) before writing this GDD. Do not write Equipment or Angel GDDs until probability model is validated. |
| Combat System | Scope | God Object risk flagged by TD-SYSTEM-BOUNDARY. Ownership has been tightened (Combat owns turn sequence only), but it still orchestrates 6 upstream systems. GDD may surface undocumented interactions. | Write Combat GDD last among MVP systems. All 6 dependencies must be complete and reviewed before Combat authoring begins. |
| Equipment Degradation | Design | Layer ordering principles — what makes a lower layer "interesting" vs. "just different" — require design taste, not just rules. Risk: layers feel arbitrary rather than meaningful. | Prototype 2-3 complete equipment stacks before writing this GDD. Physical playtest evidence should inform the layer design principles section. |

---

## Creative Director Notes (CD-SYSTEMS, 2026-05-23)

> **Concern 2 — Pillar 4 (Cooperative Ownership) needs MVP mechanical hooks.**
> Communication Protocol is in Alpha, but the decisions that make teammate boards relevant must exist in MVP mechanics. When authoring the **Character System** and **Combat System** GDDs, explicitly name at least 2-3 decisions in those systems that require a player to look at a teammate's board. If no such decisions exist in the MVP mechanics, Pillar 4 is unrepresented in the first playtest.

> **Concern 3 — Pillar 5 (Combos Must Be Discoverable) needs a named owner for synergy surface design.**
> The deliberate construction of intersection points where emergent combos can occur is currently implicit across Character, Equipment, Degradation, and Items. When authoring the **Character System** GDD, include an explicit "Synergy Surface Design" subsection that defines how divine gifts are designed to create non-obvious intersections with equipment layers and items. This ensures combos are designed rather than emergent by accident.

> **Concern 4 — Pillar 1 (Agency Over Dice) needs a named system.**
> The "decision at roll-time" surface — what the player sees, what options are surfaced, how the choice is framed — has no explicit home. When authoring **Dice Economy** or **Combat System**, one of these must explicitly own the decision presentation: what information is visible to the player at the moment of dice placement, and how the available options are communicated.

---

## Producer Risk Notice

> **PR-SCOPE (2026-05-23)**: UNREALISTIC verdict on 8 MVP GDDs in 3 months for a solo designer doing parallel work. Designer accepted the risk. Recommendation to move Game Setup and Communication Protocol to MVP was declined.
>
> **Mitigation**: Before the first external playtest session, draft informal setup and protocol notes (not full GDDs) to make the prototype runnable by strangers. Formalize into GDDs during the Alpha tier.

---

## Progress Tracker

| Metric | Count |
|--------|-------|
| Total systems identified | 12 |
| Design docs started | 4 |
| Design docs reviewed | 0 (pending /design-review in fresh sessions) |
| Design docs approved | 0 (pending) |
| MVP systems designed | 4 / 9 |
| Vertical Slice systems designed | 0 / 1 |
| Alpha systems designed | 0 / 2 |

---

## Next Steps

- [x] Build Monte Carlo dice simulation before writing Dice Economy GDD (validated; distribution confirmed)
- [x] Design Dice Economy (complete — CD-GDD-ALIGN: APPROVE 2026-05-23)
- [ ] Run `/design-review design/gdd/dice-economy.md` in a FRESH session (not the same session as /design-system)
- [x] Design Integrity System (complete — CD-GDD-ALIGN: CONCERNS accepted 2026-05-24)
- [x] Design Win/Loss Conditions (complete — CD-GDD-ALIGN: APPROVE 2026-05-24)
- [x] Design Equipment System (complete — CD-GDD-ALIGN: CONCERNS accepted 2026-05-25; two revisions applied)
- [ ] Run `/design-review design/gdd/equipment-system.md` in a FRESH session (mandatory)
- [ ] Run `/design-review design/gdd/win-loss-conditions.md` in a FRESH session
- [ ] Design Angel System, Character System, or Equipment Degradation next (parallel candidates)
- [ ] Run `/gate-check systems-design` when all MVP GDDs are complete
