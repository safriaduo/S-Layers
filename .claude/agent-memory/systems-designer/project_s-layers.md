---
name: project-s-layers
description: Core facts about S-Layers board game — physical cooperative, dice economy, equipment degradation, active GDD authoring
metadata:
  type: project
---

S-Layers is a 1-4 player cooperative physical board game (Kickstarter target). Players are prisoners of Heaven fighting through a card-drawn dungeon of Angels to defeat God. Core fantasy: the "broken build" — equipment degrades to reveal new layers, items modify equipment rules, intersection creates emergent power.

**Why:** Designer is Federico Gallucci, sole designer and publisher. GDDs are being authored incrementally. Physical production target (~200+ components), Kickstarter launch month 11 of a 20-month plan.

**How to apply:** This is a physical board game — no code, no engine. All systems must resolve at a table without computation. "Fast and Focused" pillar is a strict constraint: every mechanic must earn its 30 seconds. Formulas must be human-operable (no calculators required mid-game).

**Game Pillars:**
1. Agency Over Dice — no roll leaves the player helpless
2. The Build is Never Finished — degradation + items = live system
3. Fast and Focused — max one extra lookup per encounter per new rule
4. Cooperative Ownership — party build is collective puzzle
5. Combos Must Be Discoverable — synergies must be non-obvious

**Dice Economy core facts (validated/decided):**
- Player pool: 2D6, place voluntarily per slot (Head, Body, Weapon)
- Angel: 3D6 middle-die — distribution: 1→7.4%, 2→18.5%, 3→24.1%, 4→24.1%, 5→18.5%, 6→7.4%
- [N] notation: placed die face value is direct input to effect (not a cost threshold)
- Accumulate keyword: die values sum toward threshold across turns; total persists between turns
- Unplaced dice: voluntarily discarded, no effect, no reserve/hold
- Slot limit: one die per slot per turn by default; some cards may allow more
- Angel face visible before player places dice; Angel action resolves AFTER player actions

**Active work:** Authoring Section C (Detailed Design — Core Rules, States, Interactions) of dice-economy.md. Pre-draft validation pass in progress.

**How to apply:** When authoring dice economy or any connected system, treat above decisions as locked. Flag conflicts for designer confirmation before proposing alternatives.
