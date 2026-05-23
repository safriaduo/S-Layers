# Dice Economy

> **Status**: Complete
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-23
> **Implements Pillar**: Agency Over Dice (primary); Fast and Focused (secondary)

## Overview

The Dice Economy defines the two probability models that power every round of S-Layers: the **player's 2D6 placement pool** and the **Angel's 3D6 middle-die action roll**. These two models produce every die result in the game. Every other system — equipment activation, Angel actions, character abilities — draws from these two outputs and nothing else.

**Player pool**: Each turn, a player rolls two standard six-sided dice. Each die may then be placed onto a slot — typically an equipment slot (Head, Body, or Weapon), though some items also accept dice. Placement is voluntary: a player may place both dice, one die, or neither. When a die is placed, its face value is the direct input to that slot's effect. An effect reading "Deal [N] damage" deals damage equal to the placed die's value; "Heal [N] Integrity" heals that many Integrity points. Most effects consume the die's value directly. Some effects instead accumulate die values across multiple turns — notably the **Accumulate** keyword, where placed die values sum toward a trigger threshold.

**Enemy roll**: The Angel rolls three six-sided dice simultaneously, sorts them from lowest to highest, and resolves the middle die (second position). The 3D6 middle-die distribution is centered and bowl-shaped: faces 3 and 4 are most common (~24% each), faces 2 and 5 occur at ~18.5% each, and faces 1 and 6 are the rarest at ~7.4% each. Angel board face assignments must account for this distribution — effects assigned to faces 1 and 6 will rarely trigger under normal play.

**Die value notation**: Effects that accept a placed die reference its value as `[N]`, where N is the die face showing. This symbol appears in all equipment, item, and ability text across the game. The Dice Economy owns this notation; what each system does with N is owned by that system.

The tactile experience of rolling 2D6 and choosing which slots — if any — to activate, knowing the Angel's committed action is already on the board, is the moment-to-moment game. The probability model is infrastructure; the placement decision is what the player feels.

## Player Fantasy

Heaven keeps ledgers. Damage is bounded; rolls are fair; Angels die on schedule. **You are going to break the ledger.**

Two dice in your hand each turn, and most of the time you will place them the way the ledger expects — activating the obvious effect, dealing the obvious damage. But the dice do not know what you know. They do not know that three turns of placements that looked wrong were part of a sentence finishing itself. They do not know that the 4 you just placed on a slot that "does nothing visible" is the last accumulation the degraded helm needed.

The Dice Economy serves one emotional promise: **no roll takes the decision away from you.** A 1 and a 2 are not a disaster. They are the turn you build for the turn after. A 6 placed on a slot that currently deals 6 damage is interesting. A 6 placed on the right slot at the right moment — when the item attached to your gauntlet is counting, when your teammate's build is one die short of completing — is the thing you came here to find.

The dice are Heaven's instruments. They fall impartially. They do not care that you have been watching the Angel board for three rounds, calculating which face will show, preparing the slot that answers it. **This is the mistake Heaven made.** It gave you randomness and called it control. The placement is yours. The compounding is yours. When the chain fires and the table goes quiet, the ledger has been broken — and you did it on purpose, with tools they thought were keeping you in line.

## Detailed Design

### Core Rules

**Player Dice Pool**

1. At the start of a player's action phase, that player rolls two standard six-sided dice. This is the player's pool for the turn.
2. Each die may be placed onto an available slot. Placement is voluntary — a player may place both dice, one die, or neither.
3. Unplaced dice are discarded at end of placement phase with no effect.
4. **Placement procedure**: The player physically places their dice on their chosen slots. No effects fire during this placement. All effects resolve only after every player has placed all their dice for this round.
5. **Slot capacity**: Each slot accepts one die per turn by default. Accumulate slots are an inherent exception — they accept any number of dice per turn, each adding its N value to the running total. Other multi-die exceptions are stated on the card.
6. **Die value (N)**: The face value of a placed die is N. N is always the raw, unmodified die face. Items may modify the *output* of an effect, but never N itself before the effect reads it.

7. **Activation constraints**: Equipment effects may restrict which die values activate them. These constraint notations are defined by the Dice Economy and used by the Equipment System:

   | Notation | Triggers when… | Example |
   |----------|----------------|---------|
   | `[N≥X]` | Placed die ≥ X | `[N≥4]: Deal [N] damage` — activates on 4, 5, or 6 only |
   | `[N≤X]` | Placed die ≤ X | `[N≤2]: Heal [N] Integrity` — activates on 1 or 2 only |
   | `[N=X]` | Placed die = X exactly | `[N=6]: Draw an item card` — activates on 6 only |
   | `[N:odd]` | Placed die is odd (1, 3, 5) | `[N:odd]: Gain a shield token` |
   | `[N:even]` | Placed die is even (2, 4, 6) | `[N:even]: Accumulate [N]` |

   A constrained slot that receives an out-of-constraint die is **not activated** — the die is placed and consumed but the effect does not fire.

**Accumulate Keyword**

8. Effects with the **Accumulate** keyword use the format: `Accumulate [X]: [effect]`. When a die is placed on an Accumulate slot, N is added to a running total tracked on that card (with a token or cube).
9. The running total persists between turns until the threshold is reached.
10. When the running total equals or exceeds X, the effect fires immediately and the total **resets to zero**.
11. If a placement pushes the total past X (e.g., threshold 8, current total 6, player places a 5 → total 11), the effect fires once and the total resets to zero. The excess does not carry over.

**Angel Roll**

12. The Angel rolls three six-sided dice simultaneously.
13. The three dice are sorted from lowest to highest. The middle die (second position) determines the Angel's action for this round. Ties resolve naturally through sorting — no special rule is needed (e.g., 2, 2, 5 → middle = 2; 1, 5, 5 → middle = 5).
14. The Angel's result is revealed to all players **before** the player placement phase begins. Players know which Angel action is committed before placing any dice.
15. The Angel's action resolves **after** all players have placed all their dice and all player effects have resolved.

**Decision Presentation**

16. At the moment of placement, every player has full information about: (a) their own board (equipment, items, Accumulate totals, slot states), (b) the Angel's committed face and its mapped action, (c) all teammates' boards.
17. Players may freely discuss placements with teammates before and during the placement phase. No effects fire until all players have finished placing.
18. This information availability is a design invariant — no effect may conceal a player's own board state or the Angel's resolved face during the placement phase.

**Cleanup**

19. After all placed-die effects resolve, placed dice are removed from their slots. Dice belong to no individual player between turns.

### States and Transitions

| State | Tracked On | Persists | Changes When |
|-------|-----------|----------|-------------|
| Accumulate running total | Equipment card (marker) | Between turns | Die placed on slot; resets to 0 on threshold |
| Angel middle-die result | Angel board | Current round | Angel roll phase |
| Player 2D6 pool | Player's hand | Current turn | Dice placed or discarded |

**Turn flow (Dice Economy scope only):**

```
Angel Roll Phase
  ↓ Angel sorts 3D6 low to high — middle die revealed to all players

Player Placement Phase (all players, no effects fire)
  ↓ Each player rolls 2D6 and physically places dice on their slots
  ↓ All players complete their placements before any effect fires
  ↓ Unplaced dice discarded

Effect Resolution Phase
  ↓ All placed-die effects resolve in party-chosen order
      [each placement: N = raw die face → slot effect fires]
  ↓ Accumulate totals update; thresholds checked and triggered here
  ↓ Cleanup: placed dice removed from slots after all effects resolve

Angel Resolution Phase
  ↓ Angel's committed action resolves
  ↓ If Angel action causes equipment integrity loss → layer revealed
      → If new layer activates on the current Angel die face: fires immediately
```

*The full turn-phase sequence (party order, multiple-player coordination within a round) is owned by the Combat System. The Dice Economy defines only the rules governing dice within the placement and effect resolution phases.*

### Interactions with Other Systems

| System | Data Out from Dice Economy | Interface Notes |
|--------|---------------------------|-----------------|
| **Equipment System** | N (placed die face value) + placement signal (slot + value) + Accumulate total updates | Equipment System enforces placement legality — which slots exist, how many dice per turn, what card effects do with N. Dice Economy produces values only. |
| **Angel System** | "Player phase complete" signal | Angel independently resolves its 3D6 roll. Dice Economy does not pass player die values to the Angel System. |
| **Character System** | None | Divine gifts activate independently — they do not consume dice from the 2D6 pool. No interface. |
| **Item System** | N (placed die face value), when a die is placed on an item that accepts dice | Items receive raw N, same as equipment slots. Items may modify effect output; they cannot modify N. |

## Formulas

### Formula 1 — Angel Action Frequency

**The `P_angel` formula is defined as:**

`P_angel(k) = F(k) − F(k−1)` where `F(k) = 3·(k/6)² − 2·(k/6)³`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Angel face | k | int | {1, 2, 3, 4, 5, 6} | The face value whose probability is being computed |
| CDF | F(k) | float | [0.0, 1.0] | Cumulative probability that the median ≤ k |
| Result | P_angel(k) | float | [0.074, 0.241] | Probability that the Angel's action shows face k |

**Complete probability table:**

| Face | Fraction | % | Expected triggers / 10 rounds | Expected triggers / 20 rounds |
|------|----------|---|-------------------------------|-------------------------------|
| 1 | 16/216 | 7.4% | 0.74 | 1.48 |
| 2 | 40/216 | 18.5% | 1.85 | 3.70 |
| 3 | 52/216 | 24.1% | 2.41 | 4.81 |
| 4 | 52/216 | 24.1% | 2.41 | 4.81 |
| 5 | 40/216 | 18.5% | 1.85 | 3.70 |
| 6 | 16/216 | 7.4% | 0.74 | 1.48 |

**Output Range:** P_angel(k) ∈ [7.4%, 24.1%]. Minimum at faces 1 and 6; maximum at faces 3 and 4. E[Angel] = 3.5.

**Example:** Over a 12-round encounter, face 3 triggers approximately 2.89 times; face 6 triggers approximately 0.89 times. Face 1/6 effects can be more punishing than face 3/4 effects precisely because they will rarely fire.

---

### Formula 2 — Accumulate Expected Trigger Time

**The `T_accum` formula is defined as:**

`T_accum = X / (p · E[N])` where `E[N] = 3.5` for a fair d6

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Threshold | X | int | [4, 20] | Accumulate total at which the effect fires |
| Die expected value | E[N] | float | 3.5 (fixed) | Expected value of a single placed die on a fair d6 |
| Feeding probability | p | float | (0.0, 1.0] | Fraction of turns the player places at least one die on this slot |
| Trigger time | T_accum | float | [X/6, ∞) | Expected number of turns to trigger; unbounded as p → 0 |

**Output Range:** T_accum ≥ X/6 (fastest: all 6s every turn). At honest competitive play (~65% feeding consistency), effective p·E[N] ≈ 2.75 per turn, giving T_accum ≈ X/2.75.

**Example:** Threshold X = 10, feeding p = 0.65:
`T_accum = 10 / (0.65 × 3.5) = 10 / 2.275 ≈ 4.4 turns` — fires once per typical encounter.

---

### Formula 3 — 2D6 Placement Decision Space

**The `D_space` formula is defined as:**

`D_space(s, m) = (s + m + 1)² − s`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Single-die slots | s | int | [0, ∞) | Slots that accept at most one die per turn |
| Multi-die slots | m | int | [0, ∞) | Slots that accept any number of dice per turn (Accumulate slots and cards with explicit multi-die capacity) |
| Decision space | D_space | int | [1, ∞) | Number of distinct ordered die-assignment choices available |

**Derivation:** Each die has (s + m + 1) options: any slot or discard. Total ordered pairs = (s+m+1)². Subtract s invalid cases where both dice are assigned to the same single-die slot. Multi-die slots never produce an invalid case — both dice landing there is legal.

**Output Range:** D_space ≥ 1 always (s=0, m=0 gives exactly 1 choice: discard both). Grows quadratically with total slots.

**Reference table:**

| Setup | s | m | D_space | Notes |
|-------|---|---|---------|-------|
| Base game (no items) | 2 | 1 | 14 | 3 equipment slots, 1 Accumulate |
| Base + 1 single-die item | 3 | 1 | 22 | |
| Base + 1 multi-die item | 2 | 2 | 23 | |
| All single-die | 3 | 0 | 13 | Accumulate-free build |

**Example:** Standard 3-slot game (s=2, m=1): `D_space = (2+1+1)² − 2 = 16 − 2 = 14 choices`. Fast and Focused compliant — 14 choices collapse quickly once the board state is read.

---

### Formula 4 — Activated N Distribution (Selective Placement)

**The `P_select` formula is defined as:**

- Single die (random): `P_random(v) = 1/6`
- Higher of 2d6 (max-selection): `P_max(v) = (2v − 1) / 36`
- Lower of 2d6 (min-selection): `P_min(v) = (13 − 2v) / 36`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Die face | v | int | {1, 2, 3, 4, 5, 6} | The placed die's face value |
| Random placement | P_random(v) | float | 1/6 ≈ 16.7% | Baseline: placing without regard to value |
| Max-selection | P_max(v) | float | [2.8%, 30.6%] | Player places the higher of their two dice |
| Min-selection | P_min(v) | float | [2.8%, 30.6%] | Player places the lower of their two dice |

**Expected values:**

| Strategy | E[N] | Notes |
|----------|------|-------|
| Random | 3.50 | Baseline |
| Max-selection | 4.47 | +27.7% above baseline — the mechanical reward for Agency Over Dice |
| Min-selection | 2.53 | Used when a slot rewards low values |

**Output Range:** E[N_max] ∈ [3.50, 6.00] depending on available dice and board state. E[N_max] + E[N_min] = 7.0 (identity: both dice average to 7.0).

**Example:** A slot reading "Deal [N] damage." Player rolls 3 and 5. Max-selection: places the 5, deals 5 damage. Over many turns, expected damage per placement = 4.47. A player placing randomly would expect 3.50 — choice alone adds ~1 damage per turn.

---

### Accumulate Threshold Design Tiers

These tiers are design constraints for all Accumulate cards across the game. All thresholds assume ~65% feeding consistency (E[N/turn] ≈ 2.75).

| Tier | Threshold Range | Expected Fires / 6-Round Encounter | Design Role |
|------|----------------|-------------------------------------|-------------|
| **Tick** | 5–6 | 2–4× | Minor recurring effects; effect must be weak |
| **Charge** | 8–12 | 1–2× | Primary design space; meaningful payoffs |
| **Ritual** | 15–18 | ~once | Game-bending effects; build anchors |

**Hard floor**: 4 — below this, use an immediate effect instead.
**Hard ceiling**: 20 — above this, unreachable at honest feeding rates.
**Use even numbers only** in physical play (4, 6, 8, 10, 12, 15, 18, 20). Odd thresholds add token-tracking friction.

*All thresholds assume a single Accumulate slot per build. Multiple competing Accumulate slots reduce effective feeding consistency — recalibrate if two or more Accumulate cards are present in the same build.*

## Edge Cases

**Placement Phase (Effect-Free)**

- **If a player has no available slots on their turn**: both dice must be discarded. Legal, no effect.

- **If a player places a die on a slot and then changes their mind before all placements are complete**: rules for physical commitment are a table protocol matter owned by the Communication Protocol GDD. The Dice Economy rule is: effects do not fire until all players have placed all dice, regardless of when dice are physically set down.

- **If two players each want to place on the same party-shared slot in the same round**: each player places on their own equipment. If a shared party slot is introduced by another system, the first player to place occupies the slot for that round; subsequent players cannot place on it that round. This rule is owned by whichever system introduces shared slots.

**Angel Roll**

- **If two or three Angel dice share the same face value** (e.g., 2, 2, 5 or 3, 3, 3 or 1, 5, 5): sort all three dice from lowest to highest and take the middle position. No special handling is needed for ties — the sort resolves all cases unambiguously (2, 2, 5 → middle = 2; 1, 5, 5 → middle = 5; 3, 3, 3 → middle = 3).

- **If the Angel's resolved face maps to an action that has been removed or altered on the Angel board**: the Dice Economy produces a valid face integer 1–6. What the Angel System does if that face has no mapped action is the Angel System's responsibility.

- **If the Angel's action causes a player's equipment to lose Integrity, triggering a layer reveal, and the newly revealed layer would activate given the current Angel die face**: the new layer fires immediately as part of Angel resolution, before the next player turn. The Angel die face serves as both the source of the integrity damage and the activation trigger for the new layer.

**Effect Resolution Phase**

- **If two dice were placed on the same Accumulate slot**: during the Effect Resolution Phase, the player adds each die's N to the running total in their chosen order. If the first addition crosses the threshold, the Accumulate effect fires and the total resets to zero before the second N is added. The second die then adds to the now-zero total normally.

- **If the Accumulate threshold is crossed during Effect Resolution and the fired effect adds a new slot**: the new slot is available from the next player turn. No dice from the current placement phase can redirect to it.

- **If a card effect lowers the Accumulate threshold below the current running total**: the Accumulate effect fires immediately when the condition is recognized. Total resets to zero.

- **If the card bearing an Accumulate slot leaves the active board**: the running total is lost. Accumulate totals are a property of the card. If the card re-enters play, it starts at zero.

**Die Value and Re-rolls**

- **If an effect forces a die to be re-rolled before it is placed**: the re-roll produces a new raw face value. This new face becomes N. Re-roll effects are the only mechanism that may change N before placement and do not violate Rule 6, which applies to post-placement modifications only.

- **If a forced placement effect instructs a player to place a specific die on a specific slot outside the normal placement phase**: the die's N feeds the slot normally. Forced placements are not voluntary and resolve at the point the effect fires, not in the player-chosen effect order.

- **If any mechanism produces N = 0** (impossible on a standard d6, possible through item interaction): N = 0 adds nothing to an Accumulate total and produces zero output on any immediate effect. Flag this to the Item System.

## Dependencies

**Upstream dependencies**: None. The Dice Economy is a Foundation-layer system. It does not depend on any other system in S-Layers.

**Downstream dependents** (systems that depend on this one):

| System | Dependency Type | What They Consume |
|--------|----------------|-------------------|
| **Equipment System** | Hard | N (placed die face value); Accumulate total updates; placement signal (which slot received which die); the `[N]` notation syntax; Accumulate threshold design tiers (Tick/Charge/Ritual) |
| **Angel System** | Hard | The 3D6 median probability model (P_angel formula and full probability table); E[Angel] = 3.5; the middle-die rule (sort low to high, take position 2) |
| **Character System** | Soft | The 2D6 player pool structure (divine gifts must not consume from this pool); the existence of the voluntary discard (some gifts may interact with discarded dice) |
| **Item System** | Hard | N (raw face value) when items accept dice; the "N is raw, items modify output" boundary rule |
| **Combat System** | Hard | The full turn flow (Placement Phase → Effect Resolution Phase → Angel Resolution Phase); the rule that effects fire only after all players place all dice |

**Bidirectional consistency notes**:
- When the **Equipment System** GDD is authored, it must list the Dice Economy as a dependency and reference the `[N]` notation and Accumulate tier constraints.
- When the **Angel System** GDD is authored, it must list the Dice Economy as a dependency and reference the P_angel probability table when assigning actions to die faces.
- When the **Combat System** GDD is authored, it must list the Dice Economy as a dependency and reproduce the turn flow phase sequence without contradicting the effect-timing rule.

**Physical component dependency**: The game requires a minimum of **6 standard six-sided dice** in the box (3 for the Angel roll + 2 per player for the player pool, with at least 2 player dice shared as a common pool). Final component count is a production concern; the rules assume standard d6 dice with faces 1–6.

## Tuning Knobs

**1. Accumulate Threshold (X)**
- **Adjusts**: How often an Accumulate effect fires per encounter
- **Safe range**: 4–20 (Tick: 5–6, Charge: 8–12, Ritual: 15–18)
- **Too low (< 4)**: Effect fires nearly every turn; redesign as an immediate effect instead
- **Too high (> 20)**: Unreachable at honest feeding rates; card becomes a dead slot
- **Interaction**: Multiple competing Accumulate slots on one build reduce per-slot feeding rate — recalibrate thresholds downward

**2. Minimum N Constraint (`[N≥X]`)**
- **Adjusts**: The floor die value required to trigger an effect
- **Safe range**: N≥2 to N≥5
- **N≥2**: Mild gating — 5 of 6 faces qualify (83%)
- **N≥4**: Balanced — 3 of 6 faces qualify (50%); clear tension with max-selection strategy
- **N≥5**: Demanding — 2 of 6 faces qualify (33%); effect should be proportionally powerful
- **N≥6**: Extreme — 1 in 6 faces (17%); treat as Ritual-tier power level
- **Too restrictive (N≥6 on a weak effect)**: Slot becomes useless; player will always discard rather than waste a valid die

**3. Maximum N Constraint (`[N≤X]`)**
- **Adjusts**: Rewards low rolls; creates slots that benefit from min-selection strategy
- **Safe range**: N≤2 to N≤4
- **N≤2**: Highly restrictive (33% qualify); reverses max-selection incentive entirely
- **N≤4**: Moderate (67% qualify); gentle counter-pressure to max-selection
- **N≤5**: Near-baseline (83%); barely constraining
- **Interaction**: The primary design tool for making the lower die in a 2D6 roll feel valuable rather than wasted

**4. Exact Value Constraint (`[N=X]`)**
- **Adjusts**: High-power effect frequency; tied to a single face
- **Safe range**: Any face 1–6; calibrate effect power per face frequency
- **Face 3 or 4**: ~16.7% base trigger rate (uniform d6 baseline)
- **Face 6 with max-selection**: Effectively ~30.6% trigger rate — account for selection strategy when calibrating power
- **Face 1 with max-selection**: Effectively ~2.8% trigger rate — near-Ritual power level appropriate
- **Warning**: Too many exact-value slots on one build leaves the player with no valid placement targets on most turns

**5. Odd/Even Constraint (`[N:odd]` / `[N:even]`)**
- **Adjusts**: ~50% baseline trigger rate, independent of specific die value
- **Baseline**: 3 of 6 faces qualify per constraint type
- **Even with max-selection**: Slight advantage (faces 4 and 6 are even, face 5 is odd — higher faces lean even)
- **Design use**: The most balanced constraint for effects that should fire ~half the time without being trivially gated

**6. Player Dice Pool Size (2D6)**
- **Current value**: 2 dice per turn
- **Recommended status**: Locked. The 2-die pool is load-bearing for D_space, max-selection distribution, and Pillar 1 (Agency Over Dice). Changing it requires full pillar review.
- **At 1 die**: No choice between dice — agency collapses, Pillar 1 violated
- **At 3+ dice**: D_space grows rapidly; analysis paralysis risk

**7. Angel Dice Count (3D6)**
- **Current value**: 3 dice → median distribution
- **Recommended status**: Locked. Changing it invalidates the P_angel probability table and all Angel board face assignments calibrated against it.
- **At 2D6**: Flat distribution (each face = 16.7%); loses tail compression that makes faces 1/6 rare
- **At 5D6**: Distribution tightens further; faces 1/6 become nearly unreachable

**8. Integrity-Loss Trigger** *(notation owned by Equipment Degradation GDD)*
- **What it is**: `When this equipment loses Integrity: [effect]` — a reactive trigger fired by integrity damage, not die placement
- **Dice Economy relevance**: Exists on the same cards as die-placement effects; designers must account for both trigger types when calibrating total card power. This trigger does not use N or the placement model.
- **Tuning note**: Integrity loss in S-Layers is often deliberate — these triggers interact with the "strategic self-harm" dynamic and should not be calibrated as pure penalty effects
- **Full ownership**: Equipment Degradation GDD

## Visual/Audio Requirements

*Not applicable — Foundation/Infrastructure layer. Physical board game; no digital audio or visual rendering. Physical component requirements (dice, tokens) are covered in Dependencies.*

## UI Requirements

*Not applicable — Foundation/Infrastructure layer. Player-facing information display (Accumulate tracking tokens, Angel board) is a physical component design concern owned by the Art/Production pipeline, not a UI system.*

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Items marked DEFERRED require a simulation tool, not a playtester.*

**Player Pool and Placement**

- **AC-01** — GIVEN a player rolls 2D6 with available slots, WHEN they choose to place neither die, THEN both dice are discarded and no effect fires.
- **AC-02** — GIVEN Player A has placed both dice and Player B has not yet placed theirs, WHEN Player A's placed die meets its activation constraint, THEN no effect fires until Player B has also completed their placement.
- **AC-03** — GIVEN a slot with effect "Deal [N] damage" and N = 4, WHEN the effect resolves, THEN exactly 4 damage is dealt — not 3, not 5.

**Activation Constraints**

- **AC-04** — GIVEN a slot with constraint [N≥4] and a die showing 3 is placed, WHEN effects resolve, THEN the effect does not fire and the die is consumed.
- **AC-05** — GIVEN a slot with constraint [N≤3] and a die showing 4 is placed, WHEN effects resolve, THEN the effect does not fire and the die is consumed.
- **AC-06** — GIVEN a slot with constraint [N:odd] and a die showing 4 (even) is placed, WHEN effects resolve, THEN the effect does not fire and the die is consumed.
- **AC-07** — GIVEN a slot with constraint [N:even] and a die showing 3 (odd) is placed, WHEN effects resolve, THEN the effect does not fire and the die is consumed.
- **AC-08** — GIVEN a slot with constraint [N=6] and a die showing 5 is placed, WHEN effects resolve, THEN the effect does not fire and the die is consumed.

**Accumulate**

- **AC-09** — GIVEN an Accumulate slot (threshold X=10, starting total 0) and dice placed across two turns summing to 6 then 4 (total reaches 10), WHEN the second batch resolves, THEN the effect fires exactly once and the total resets to 0.
- **AC-10** — GIVEN an Accumulate slot (threshold X=10) with running total 4 at end of Turn 1, WHEN Turn 2 begins and no die is placed on the slot, THEN the running total is still 4 at the start of Turn 2's effect resolution.
- **AC-11** — GIVEN an Accumulate slot with running total 7 and a die of N=5 is placed (total would reach 12, threshold=10), WHEN the effect fires, THEN the total resets to 0 — not 2.
- **AC-12** — GIVEN two dice (N=4 and N=3) placed on the same Accumulate slot (threshold=10, starting total=6), WHEN the player resolves the N=4 die first (total becomes 10, threshold crossed, fires, resets to 0), THEN the N=3 die is added to the now-zero total (total becomes 3), not to 10.
- **AC-13** — GIVEN an Accumulate slot with a running total of 7 and the card is removed from the active board, WHEN the card re-enters play later, THEN the running total starts at 0, not 7.

**Angel Roll**

- **AC-14** — GIVEN the Angel rolls 2, 2, 5, WHEN the middle die is determined by sorting low to high, THEN the resolved face is 2 (sorted: 2, 2, 5 → position 2 = 2).
- **AC-15** — GIVEN the Angel rolls 1, 5, 5, WHEN the middle die is determined by sorting low to high, THEN the resolved face is 5 (sorted: 1, 5, 5 → position 2 = 5).
- **AC-16** — GIVEN the Angel's 3D6 are sorted and the middle die is resolved, WHEN the player placement phase begins, THEN the Angel face is visible to all players before any player picks up their 2D6.
- **AC-17** — GIVEN all players have placed their dice and all player effects have resolved, WHEN the Angel resolution phase begins, THEN no player effect fires after this point in the same round.
- **AC-18** — GIVEN the Angel face is 3 and the Angel's action causes integrity loss on a player's equipment, triggering a layer reveal, and the newly revealed layer activates on face 3, WHEN Angel resolution fires, THEN the new layer's effect fires during the same Angel resolution phase before the next player turn.

**Re-rolls and Forced Placement**

- **AC-19** — GIVEN a re-roll effect fires before a die is placed and the die shows a new face value (e.g., original face 2, re-rolled to 5), WHEN the die is subsequently placed on a slot, THEN N = 5 (the new face), not 2.
- **AC-20** — GIVEN a forced placement effect fires and directs a die to a specific slot outside the normal placement phase, WHEN the effect resolves, THEN the die's N feeds the slot normally and the placement does not wait for the player-chosen effect order.

**Max-Selection and Distribution**

- **AC-21** *(DEFERRED — simulation tool required)* — GIVEN a simulation of 10,000 player turns applying max-selection strategy, WHEN placed die values are recorded, THEN the distribution matches P_max(v) = (2v−1)/36 within ±0.01 per face and mean placed value is 4.47 ± 0.05.
- **AC-22** — GIVEN a player rolls 2D6 and applies max-selection strategy, WHEN they place a die, THEN the die placed is always the higher-value die of the two rolled (or either die if values are equal).

*Note: Slot enforcement (one die per single-die slot per player turn) is tested by the Equipment System GDD, not this document.*

## Open Questions

*None — all design questions were resolved during the authoring session. AC-21 (max-selection distribution validation) is deferred pending a simulation tool, tracked in the Acceptance Criteria.*
