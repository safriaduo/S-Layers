# Equipment System

> **Status**: In Design
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-25
> **Implements Pillar**: Agency Over Dice (primary); The Build is Never Finished (secondary)

## Overview

The Equipment System defines each player's active board: three equipment slots — **Head**, **Body**, and **Weapon** — each occupied by an equipment card whose text specifies what happens when a player places a die there. This is the primary decision surface of S-Layers: every turn, a player holds two dice and decides which slots to activate, in what order, and which die (if any) to route where. The Equipment System governs slot structure, card anatomy, and the placement rules that turn a die's face value into a fired effect.

Each equipment card specifies: the slot it occupies; the activation constraint (a die-value rule written in the Dice Economy's `[N]` notation); and the effect — what fires when a legal die lands there. Some cards use the **Accumulate** keyword, banking placed die values across turns toward a threshold trigger. Equipment cards may also host items, which modify their rules or effects; item attachment rules are owned by the Item System, not this GDD.

Equipment slots hold stacks of cards called layers — only the top card in each slot is active at any time. When a slot is damaged, the active card is stripped and the card beneath becomes the new active layer. That layer-reveal mechanic — what triggers it, what each layer must contain, how layers are designed — is entirely owned by **Equipment Degradation**. This GDD governs only the rules that apply to whichever card is currently live at the top of a slot.

The fantasy is narrow by design: three slots and two dice is a small stage. The depth comes from the specificity of each slot's constraint, the Accumulate progress banking silently across turns, and the knowledge that the Angel's committed action is already visible before a single die is placed. The moment a player says *"I've been routing dice here for three turns and this is the one"* — that is the Equipment System delivering on its pillar.

## Player Fantasy

Every piece of equipment in front of you was made by Heaven, for Heaven, to be worn by Heaven's instruments. The Crown came off an Angel. The Breastplate was standard-issue for the eighth tier of the guard. The Blade was forged to kill things like you, which means it was forged to kill the specific weight and shape of your resistance. You are not equipped. You are *wearing evidence*.

The activation constraints are not mechanics — they are the original prayers. `[N≥4]` is what the Angel needed to intone before the Crown would open its power for the faithful. You are saying the prayer wrong, with dice that rolled wrong, and the Crown is opening anyway. It does not know who is wearing it. Heaven built this system to be operated, and you are operating it. That is the whole crime.

Every die placement is a small profanation. The slot was designed for someone who knows how to use it properly — who activates the right effect at the right die value and moves on. You activate the wrong effect at the wrong value in the right *order*, and three turns later something fires that the guard at the door cannot classify. The table has no word for what just happened. *"Oh, that's not allowed,"* someone says. That is the Equipment System working.

And then the Angel's blow lands. The top card slides off the Body slot and something you forgot about is suddenly face-up: a card you placed there at the start of the session, before you knew what you were building, before you understood what the session was going to become. It is worse than what you had — hungrier, meaner, asking for higher numbers. But you have a 6 in your cup. The build did not break. The build *molted*. What Heaven stripped away was already spent. What's underneath is the thing that survives.

The Equipment System promises this: **the cards in front of you are the only things in the dungeon that remember your choices.** The dice are neutral. The Angels are on schedule. But the slot configuration — the Accumulate marker at seven, the item attached to the Weapon card, the newly revealed layer that responds to exactly the constraint you've been playing toward — that is yours. Not because you own it. Because you made it into something Heaven didn't design.

## Detailed Design

### Core Rules

---

#### 1. Player Board Anatomy

The **player board** (70×120mm) is divided into three horizontal zones by visual dividing lines: **Head** (top), **Body** (middle), **Weapon** (bottom). This is the player's entire active state — everything they can do each turn lives on this board.

Each zone contains the following elements:

| Element | Description | Status |
|---|---|---|
| **Constraint Indicator** | Small icon in the top-left of each zone's text block. Indicates the dice behavior for this slot: *Single Die*, *Multi Die*, or absent (no dice — passive effect). This is the primary constraint type. Optional additional constraints (e.g., `[N≥4]`) appear as part of the effect text when present. | In mockup |
| **Effect Text** | What this slot does. Hard cap: **10–12 words**. Any effect requiring more than 12 words must be redesigned, not abbreviated. | In mockup |
| **Item Anchor Zone** | Designated space within the zone where an item card can attach to this slot. Position to be finalized in card production, but must exist in every zone. An item occupies this space for as long as its host layer is active. | **Not yet in mockup — required** |
| **Degradation Indicator** | A graphic element (icon or symbol) on the zone boundary communicating: *this layer is stronger than the one above*, *weaker*, or *this is the last layer*. No text — read at a glance. | In mockup (Angel card's heart icons are the equivalent on the enemy side) |

**Effect text budget**: 10–12 words is a hard design constraint, not a formatting preference. If an effect cannot be stated in 12 words, the effect is too complex and must be simplified or compressed into a keyword. The Accumulate keyword exists precisely to compress "sum placed dice toward threshold X, then fire" into one word + one number.

**Passive effects**: Some zones have no dice interaction at all — their effects trigger on conditions other than die placement (e.g., "When this layer is destroyed, deal 6 damage"). These zones have no constraint indicator. The absence of a constraint indicator is itself informative: no die goes here.

---

#### 2. Die-to-Slot Assignment Procedure

Effects do not fire during placement. The physical procedure:

1. The Angel's committed face is visible before any player picks up dice.
2. Each player rolls their two dice. Both values are visible to all players.
3. The player reads the active zone effects and constraint indicators, considers options, places dice. Party discussion is allowed — nothing fires during placement.
4. A placed die sits physically on the zone it targets. Unplaced dice are set aside visibly.
5. A placed die on a constraint-failing zone is legal — the die is consumed without effect.
6. When all players have finished placing, the Effect Resolution Phase begins.

---

#### 3. Slot Capacity

The **constraint indicator** is the primary die-capacity rule:

| Constraint Indicator | Meaning | Rule |
|---|---|---|
| **Single Die** | One die per turn | Player may not place a second die on this zone in the same turn |
| **Multi Die** | Any number of dice per turn | Accumulate zones always have this; some immediate effects may also |
| **Absent** | No dice | Zone has a passive or conditional trigger — dice cannot be placed here |

No other capacity exceptions are implicit. If a card does not show a constraint indicator, no die may be placed on it. Items may modify a zone's die capacity — that rule is owned by the Item System GDD.

---

#### 4. Slot Occupancy and Placement Legality

A player may:
- Place one die on one zone and the other on a different zone
- Place both dice on the same **Multi Die** zone (each die contributes independently)
- Place one die and discard the other
- Discard both dice (legal — sometimes the correct play)

A player may not:
- Place two dice on the same **Single Die** zone in one turn
- Place any die on a zone with no constraint indicator (passive zone)
- Place any die on an **inert** zone (zero layers remaining — per Integrity System rules)

**Constraint-failing placement on Accumulate zones**: If a zone has both an Accumulate keyword AND an additional constraint (e.g., `Accumulate [N≥4]`), a die placed on that zone that fails the constraint is consumed without effect and adds **nothing** to the Accumulate total. The constraint gates everything — not just the threshold trigger.

---

#### 5. Effect Resolution Order

After all players have completed placements:

1. The party collectively decides the order in which all placed-die effects resolve across all players and zones.
2. **Default order**: If the party does not actively propose a different sequence, effects resolve in turn order (clockwise), and within a player's turn in zone order — Head → Body → Weapon. Collective re-sequencing is opt-in: use the default for fast turns; negotiate only when the order matters.
3. Any order is legal. The party may change the proposed order at any point during discussion — **there is no lock on the resolution order**. Reordering is permitted until an effect actually fires.
4. Once an effect fires, it resolves fully before the next effect begins.
5. **For Multi Die / Accumulate zones with two dice placed**: the placing player decides which die is added to the running total first. If the first die crosses the threshold, the Accumulate effect fires and the total resets to zero; the second die adds to the reset total.
   > ⚠️ **Test note**: This sub-ordering rule may generate tracking friction at the table. Validate in playtest — if players find it confusing, simplify to "add both dice simultaneously and check threshold once."
6. **If a zone's active layer is stripped during the Effect Resolution Phase** (by a card effect or passive trigger) and a die was also placed on that zone for later resolution in the same phase: the committed die **fizzles** — it is consumed without effect when its resolution slot arrives. The stripped layer takes the die with it. This creates meaningful stakes around resolution order: effects that may strip a zone should resolve *after* any die on that zone that the party wants to fire.

---

#### 6. Slot Role Philosophy (Design Guidance — Not Player-Facing Rules)

There is no hard rule tying effect types to slots. Head, Body, and Weapon are **labels for zones on the player board**, not mechanical restrictions on what effects can appear there. Any slot can deal damage, heal, Accumulate, trigger on destruction, or do anything else the design requires.

What matters is not what each slot does in isolation — it is what the **set** of three slots does together. The set is the design unit. A well-designed equipment set creates a strategy that requires all three zones to execute.

**The Martyrdom set illustrates this:**
- **Head** `Accumulate 8: Restore 1 Integrity` — banks healing across turns; rewards consistent play
- **Body** `When this layer is destroyed, deal 6 damage` — turns damage received into offense; rewards routing decisions
- **Weapon** `Deal [N]×2 damage, then discard this layer` — massive burst at the cost of self-destruction

No zone works alone. The strategy is: deal damage with Weapon (destroying it), route the resulting hit to Body (triggering its damage spike), while Head accumulates a heal for the aftermath. The "broken build" emerges from reading all three zones as one sentence.

**Design guardrail — dump dice**: Every well-designed equipment set must have an intentional home for dice the player does not want to spend on their primary strategy. These are **dump dice** — not necessarily low-value dice. A build that wants only `[N=6]` may dump a 3 somewhere useful; a build that wants only `[N=1]` may dump a 5. The specific values are build-dependent. The requirement is:

> **At least one zone in any starting build must produce a meaningful result with the dice the other two zones reject.**

If a build has no dump sink, some rolls leave the player with nothing to do. That violates Pillar 1 (Agency Over Dice). The dump sink is often a passive zone or a low-constraint Accumulate zone — but the card designer must identify it explicitly when creating a set.

---

### States and Transitions

| State | Condition | Accepts die? | Effect fires? | Accumulate total | Notes |
|---|---|---|---|---|---|
| **Active** | ≥1 layer remaining; no die placed this turn | Per constraint indicator | Yes, at Effect Resolution | Persists between turns | Standard state |
| **Occupied** | ≥1 layer remaining; die placed, constraint met | Multi Die only | Yes, at Effect Resolution | Accumulates N this turn | Die committed; resolution pending |
| **Constrained-Out** | ≥1 layer remaining; die placed, constraint NOT met | — | No — die consumed, no effect | Nothing added | Looks identical to Occupied (die sits on zone). Distinction is in resolution outcome only. |
| **Inert** | 0 layers remaining | No | No | Lost with stripped layer | Cannot receive dice. Permanent unless explicit restoration effect fires. |

**Transitions:**
```
Active
  → Occupied          [Player places die; constraint met]
  → Constrained-Out   [Player places die; constraint NOT met]
  → Inert             [Hit routed here during Angel Resolution — layer stripped]

Occupied / Constrained-Out
  → Active            [Effect Resolution Phase ends — die removed, zone ready next turn]
  → Inert + die fizzles [Layer stripped DURING Effect Resolution by a card effect:
                        the committed die is consumed without effect; inert from this moment.
                        Resolution order determines which effects fire before the strip happens.]

Inert
  → Active            [Restoration effect fires: explicit "restore a layer to [slot type]"]
  → Inert             [Default — permanent]
```

**Turn phase sequence (Equipment System scope):**
```
PLACEMENT PHASE
  ↓ Angel face visible — players read all three active zone effects
  ↓ Each player: roll 2D6; place dice on zones (no effects fire); set aside unplaced dice
  ↓ All players complete placements

EFFECT RESOLUTION PHASE
  ↓ Party decides global resolution order — default: turn order (clockwise), zone order Head→Body→Weapon
  ↓ May be changed until first effect fires; once first fires, sequence is locked
  ↓ For each placed die (in decided order):
       Is zone still active? → No → die fizzles (zone stripped earlier this phase)
       Check constraint → met? → fire effect
                       → not met? → die consumed; Accumulate total unchanged
       Multi Die / Accumulate zone → add N to total → check threshold
           → total ≥ threshold → Accumulate effect fires; total resets to 0
           → total < threshold → total persists to next turn
  ↓ All effects resolved; placed dice removed from zones

ANGEL RESOLUTION PHASE (interface only)
  ↓ Angel action resolves (Angel System owns)
  ↓ Hits routed to player zones (Integrity System owns routing)
  ↓ Equipment: per hit — active layer stripped; next layer becomes active
  ↓ Zone → Inert if 0 layers remain after strip
```

---

### Interactions with Other Systems

| System | Direction | Interface |
|---|---|---|
| **Dice Economy** | Upstream | `N` (placed die face value); `[N]` notation syntax; Accumulate threshold design tiers; rule that all effects fire after all players place all dice |
| **Integrity System** | Upstream | Hit routing to zones; "hit removes one layer" trigger; inert zone rule; mid-phase layer-strip rule: a committed die fizzles if its zone's layer is stripped before the die resolves (confirm consistency with Integrity GDD) |
| **Equipment Degradation** | Downstream | Equipment System defines the zone and its active layer. Degradation owns what each layer must contain, the layer reveal event, and the degradation indicator graphic design. Interface: "active layer card = current top of stack." |
| **Character System** | Downstream | Character System assigns starting equipment cards to zones; divine gifts must not consume from the 2D6 pool. Equipment System defines zone structure; Character System populates it. |
| **Item System** | Downstream | Items attach to zones via the Item Anchor Zone. Equipment System owns the attachment point location; Item System owns item acquisition, effect rules, and disposition when the host layer is stripped. |
| **Combat System** | Downstream | Combat owns the full turn phase sequence. Equipment System provides placement legality, effect resolution logic, and zone state transitions. |

## Formulas

### Effect Math Grammar

Before the formulas: since the effect modifier language is still being explored, this section defines what the Equipment System **permits** in effect text. Card authors must use only these forms.

| Form | Meaning | Example |
|---|---|---|
| `[N]` | The placed die's raw face value | `Deal [N] damage` |
| `[N]×k` | N multiplied by a whole-number constant k ≥ 2 | `Deal [N]×2 damage` |
| `[N]+k` | N plus a flat constant | `Restore [N]+1 Integrity` |
| Fixed value | A constant, no die value involved | `Deal 6 damage` |
| `Accumulate [X]: [effect]` | Bank N across turns; fire [effect] when total ≥ X | `Accumulate 8: Restore 1 Integrity` |

**Not permitted**: any form where N is modified *before* the effect reads it. N is always the raw placed-die face. Items may modify the output of an effect, never N itself — that rule is owned by the Dice Economy GDD (Rule 6). New modifier forms require a registry update before use in card design.

---

### Formula 1 — Slot Output Per Turn (`E_slot`)

**`E_slot` is defined as:**

`E_slot = p_c × k × E[N | constraint]`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Activation probability | p_c | float | (0.0, 1.0] | Probability the placed die meets the slot's constraint at max-selection or min-selection play |
| Output multiplier | k | float | [1, ∞) | Scalar in the effect text. `[N]` → k=1; `[N]×2` → k=2; fixed value → k=0 (formula doesn't apply — use the fixed value directly) |
| Conditional die mean | E[N \| constraint] | float | [1.45, 6.00] | Expected face value of the placed die, given it met the constraint (see lookup table below) |
| Slot output | E_slot | float | [0, ~14] | Expected output per turn in the effect's unit (damage, healing, etc.) |

**Conditional E[N] lookup table (max-selection strategy):**

| Constraint | p_c | E[N \| constraint] | E_slot (k=1) |
|---|---|---|---|
| None | 1.00 | 4.47 | 4.47 |
| `[N≥3]` | 0.89 | 4.81 | 4.28 |
| `[N≥4]` | 0.75 | 5.15 | 3.86 |
| `[N≥5]` | 0.56 | 5.55 | 3.08 |
| `[N≥6]` | 0.31 | 6.00 | 1.84 |
| `[N≤3]`* | 0.75 | 1.85 | 1.39 |
| `[N≤2]`* | 0.56 | 1.45 | 0.81 |
| `[N:odd]` | ~0.42 | ~4.07 | ~1.70 |
| `[N:even]` | ~0.58 | ~4.76 | ~2.77 |

*For `[N≤X]` slots, the player routes their lower die — min-selection strategy applies. p_c and E[N] values use min(2D6) conditional distribution.

**Output Range:** 0 (die not placed) to approximately 14 at k=2 with no constraint. Upper bound is enforced by Formula 3.

**Example — Martyrdom Weapon zone** (`Deal [N]×2 damage, then discard this layer`):
- p_c = 1.0 (no constraint), k = 2, E[N] = 4.47 → `E_slot = 1.0 × 2 × 4.47 = 8.94`
- Expected one-shot spike: ~9 damage. Balance this against the permanent loss of the Weapon zone.

**Example — "Deal `[N]` damage, `[N≥4]`"** at max-selection:
- p_c = 0.75, k = 1, E[N|N≥4] = 5.15 → `E_slot = 0.75 × 1 × 5.15 = 3.86 damage/turn`

---

### Formula 2 — Accumulate Trigger Timing (`T_accum`, by reference)

The `T_accum` formula is owned by the Dice Economy GDD. The Equipment System references it here for card design calibration.

`T_accum = X / (p × E[N])` where at honest play: **p ≈ 0.65, E[N] = 3.5, effective input rate = 2.275/turn**

> *Note: The Dice Economy GDD also cites ~2.75/turn as a working approximation. The strict formula (0.65 × 3.5 = 2.275) is used here for conservative calibration. Reconcile against playtest data after MVP sessions.*

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Accumulate threshold | X | int | [4, 20] | The sum at which the effect fires. Must be even. Must respect `accumulate_floor`=4 and `accumulate_ceiling`=20. |
| Feeding probability | p | float | (0.0, 1.0] | Fraction of turns a die is routed to this slot. Honest play: ~0.65 |
| Placed die mean | E[N] | float | [3.5, 4.47] | Expected face of die routed to this slot. Use 3.5 (baseline) unless the slot has a constraint, then use the lookup table in Formula 1 |
| Expected trigger time | T_accum | float | [1.75, ∞) | Expected turns until threshold is reached and effect fires |

**Tier guidance — translating X to gameplay timing:**

| Tier | T_accum target | X range | Design intent |
|---|---|---|---|
| **Tick** | 2–3 turns | 5–7 | Minor recurring effect; fires almost every other round |
| **Charge** | 4–6 turns | 9–14 | Mid-tempo payoff; once or twice per encounter |
| **Ritual** | 7–10 turns | 16–20 | Encounter-defining burst; fires only with deliberate commitment |

**Deriving X from a target T_accum:**
`X = T_accum × 2.275` → rounded to nearest even number

**Example — Martyrdom Head** (`Accumulate 8: Restore 1 Integrity`):
- X = 8 → T_accum = 8 / 2.275 ≈ **3.5 turns** → upper Tick / lower Charge boundary
- Fires approximately twice per 8-turn encounter at honest feeding rate ✓

**Example — "Accumulate 14: Draw 1 item card"** (hypothetical Charge effect):
- T_accum = 14 / 2.275 ≈ **6.2 turns** → Charge tier; fires once per encounter if fed consistently ✓

---

### Formula 3 — Set Output Ceiling (`S_ceiling`)

**`S_encounter` is defined as:**

`S_encounter = (S_throughput × T_encounter) + B_burst`

**Balance principle**: Every equipment set — regardless of its style — should produce approximately the same total output over a standard encounter. The *shape* differs (steady throughput vs. burst spikes), but the total does not. No set should be clearly stronger or weaker than another.

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Per-turn throughput | S_throughput | float | [0, ~8] | Sum of E_slot[i] for all zones that produce consistent output each turn (Formula 1). One-shot and passive zones contribute 0 here. |
| Encounter length | T_encounter | float | [6, 10] | Expected turns in a standard encounter. Use 8 turns for MVP balance work. |
| Burst credit | B_burst | float | [0, ∞) | Total output from zones that fire once or conditionally (one-shot, passive on-destroy). Computed as: sum of (trigger probability × effect value) per burst zone, over the full encounter. |
| Encounter total | S_encounter | float | — | Total expected output across the full encounter: throughput + burst combined. |
| Baseline target | T_target | float | 64 | `D_player_base × T_encounter = 8 × 8 = 64`. The encounter-level output every set should approximately match. |

**Balance check:** A set is within budget when:

`S_encounter ∈ [54, 74]`  (T_target ± 15%, i.e. ±~10 units)

Outside this range: the set is over- or under-powered and must be redesigned. Every set targets the same band — style and playstyle are what differentiate sets, not raw power.

**Example — Martyrdom set at T_encounter = 8 turns:**

| Zone | Type | Contribution |
|---|---|---|
| Head | Accumulate 8 (1 heal / 3.5 turns) | 0.29/turn × 8 = **2.3 units** |
| Body | Passive on destroy (deal 6 damage) | Fires ~once per encounter: **6 units** burst |
| Weapon | One-shot `[N]×2` (≈8.94 dmg) | Fires once: **8.94 units** burst |

`S_encounter = (0.29 × 8) + 6 + 8.94 = 2.3 + 14.94 = **17.24**`

This is well below the target band [54, 74]. The Martyrdom values are a design starting point, not a finished calibration — the formula correctly identifies that the effect magnitudes need to increase significantly before the set is balanced against others.

**Note on burst vs. throughput comparability**: Treating 1 damage and 1 heal as equal units is a simplification. If playtests reveal that healing is systematically over- or under-valued, split the target into separate bands. For now, 1 effect-unit = 1 regardless of type.

---

### Formula 4 — Dump Dice Coverage Check (Design Heuristic)

This is a design tool for card authors, not a formula. A set passes the dump dice requirement from Section C if every die value 1–6 has at least one zone that accepts it.

**Coverage check table — fill this for every set under design:**

| Die face | Head accepts? | Body accepts? | Weapon accepts? | Has a home? |
|---|---|---|---|---|
| 1 | — | — | — | — |
| 2 | — | — | — | — |
| 3 | — | — | — | — |
| 4 | — | — | — | — |
| 5 | — | — | — | — |
| 6 | — | — | — | — |

A zone "accepts" a face value if: (a) it has no additional constraint (accepts all), (b) the face meets its `[N≥X]`, `[N≤X]`, or parity constraint, or (c) it is an Accumulate zone with no additional constraint.

**Any row where all three zones show "No" is a design failure** — that face value produces a zero-agency turn when both dice show it. Revise at least one zone's constraint to absorb it.

**Example — Martyrdom coverage check:**
- Head: `Accumulate 8` (no additional constraint) → accepts faces 1–6 ✓
- Body: Passive (no dice) → accepts no faces
- Weapon: `Deal [N]×2` (no additional constraint) → accepts faces 1–6 ✓

Every face has at least one home (Head or Weapon). Coverage passes ✓.

## Edge Cases

### Placement Phase

**If a player attempts to place both dice on the same Single Die zone**: the second die cannot be placed there. The player must redirect it to a different zone or discard it. The first die remains committed. This must be caught during the Placement Phase, before Effect Resolution begins.

**If a player discards both dice**: legal. The Effect Resolution Phase for that player produces no die effects. Accumulate totals on all zones are unchanged. Passive zone triggers may still fire if their conditions are met by other events.

**If a player places a die on an inert zone**: illegal placement. The die must be redirected to an active zone or discarded. An inert zone cannot receive dice under any circumstances unless an explicit restoration effect first makes it active again.

---

### Multi-Die and Accumulate

**If two dice are placed on the same Accumulate zone and neither die meets the zone's additional constraint** (e.g., both show 3 on `Accumulate [N≥4]`): both dice are consumed without effect. The Accumulate total is unchanged. The constraint gates each die independently — a near-threshold total does not override the constraint.

**If two dice are placed on the same Accumulate zone and the first die crosses the threshold**: the Accumulate effect fires once and the total resets to zero. The second die is then added to the reset total (starting from 0), producing a new running total equal to the second die's face value. The placing player decides which die resolves first; this choice is locked once the first die is added.

**If two dice are placed on the same Accumulate zone and each die alone would not cross the threshold, but together they do**: the first die raises the total without firing. The second die is added and the threshold is checked. If crossed, the effect fires once, total resets to zero. One crossing, one fire, one reset — regardless of combined magnitude.

**If a Multi Die zone has an immediate effect (not Accumulate) and receives two dice**: each die resolves as a separate, independent effect in party-chosen order. Two dice produce two separate effect resolutions, each reading its own N. There is no pooling of values.

**If the Accumulate running total is one point below the threshold and a constraint-failing die is placed**: the die adds nothing. The total remains at threshold − 1. The Accumulate effect does not fire. A constraint-failing die cannot complete an Accumulate trigger regardless of proximity to threshold.

**If a placed die pushes the Accumulate total well past the threshold** (e.g., total is 6, threshold is 8, die shows 6, new total would be 12): the effect fires once when the total reaches or exceeds 8. The total resets to zero. The overage (4) is discarded. Accumulate does not carry overflow to the next turn.

---

### Effect Resolution Order and Mid-Phase State Changes

**If a card effect fires during Effect Resolution and strips its own layer, and a die was also placed on that same zone earlier in the resolution sequence**: what happens depends on resolution order. If the die resolves *before* the self-strip effect: the die fires, then the self-strip fires, and the zone becomes inert. If the self-strip fires *first*: the zone's layer is gone, and when the die's slot in the sequence arrives, it **fizzles** — consumed without effect. This makes resolution order consequential: the party should position the die's resolution before the self-stripping effect if they want it to fire.

**If a fired effect strips a zone mid-phase, and a die was already placed on that zone for later resolution**: the die **fizzles** when its slot in the resolution sequence arrives. The stripped layer takes the die with it. The party can prevent this by placing the die's resolution slot before any effect that might strip the zone.

**If the party proposes a resolution order, discusses it, and then wishes to change it**: legal at any point before the first effect fires. Once the first effect fires, the remaining sequence is locked. No retroactive reordering of effects that have already resolved.

**If all dice placed across all players produce no valid activations** (all constrained-out, all zones passive, no threshold crossings): the Effect Resolution Phase begins and ends with no effects firing. All placed dice are removed. Accumulate totals are unchanged. This is a legal turn — sometimes the correct play.

---

### Passive Zone Triggers

**If a passive zone reads "When this layer is destroyed" and the layer is destroyed during the Angel Resolution Phase**: the passive effect fires at the moment of destruction, within the Angel Resolution Phase. It does not wait for the next Effect Resolution Phase.

**If a passive zone reads "When this layer is destroyed" and a card effect triggers the destruction during the Effect Resolution Phase**: the passive trigger fires immediately at the destruction event. Resolution continues from that point. Triggered passive effects resolve at the moment their condition is met, not at the end of the phase.

**If a passive zone's condition references another player's die value or board state** (e.g., "When any ally places a 6"): this violates the Equipment System's scoping rule — all effects reference only the current player's own board. A zone with this text is an illegal card design and must be redesigned before card production.

**If two passive zones on different player boards share the same trigger condition and both fire simultaneously** (e.g., both boards have "When this layer is destroyed" and both are stripped by a multi-hit in the same phase): each fires independently, once per triggering event. The party chooses the order in which the two passive effects resolve.

---

### Item Anchor Zone

**If a layer with an attached item is stripped** (during any phase): the item is removed with the host layer. The newly revealed layer's Item Anchor Zone is vacant from the moment the new layer becomes active. The Equipment System's contract: the anchor is gone when the layer is gone. The Item System owns what happens to the stripped item.

**If a layer is stripped during the Effect Resolution Phase by a card effect, and an item was attached to that layer**: the item departs immediately when the layer is stripped. Any effects resolving later in the same phase that would reference "the item on this zone" find no item. Item presence is not locked at phase start — it tracks the physical layer.

**If a restoration effect returns a stripped layer to an inert slot**: the restored layer arrives with its Item Anchor Zone empty. Items do not return automatically with the layer unless the restoration effect explicitly states they do.

**If two items somehow attempt to occupy the same Item Anchor Zone**: illegal. Each zone has exactly one anchor and may host at most one item at a time. The second item cannot attach. Item System owns how this is prevented at acquisition; Equipment System states the hard limit.

---

### Formula Boundary Cases

**If N = 1 and the effect is `[N]×2`**: the output is 2. Valid. No special handling.

**If N = 6 and the effect is `[N]×2`**: the output is 12. Valid and within the stated output range. No clamping.

**If a card is produced with an Accumulate threshold below 4** (violating `accumulate_floor`): illegal card design. At the table, treat it as threshold = 4 and flag the card for correction before the next print run.

**If a zone's Constrained-Out state from one turn seems to "carry over"**: it does not. Constrained-Out describes a single die placement event within one turn's Effect Resolution. At the start of each Placement Phase, all zones with at least one layer return to Active state. There is no Constrained-Out state between turns.

## Dependencies

**Upstream dependencies** (systems this one depends on):

| System | Type | What Equipment System consumes |
|---|---|---|
| **Dice Economy** | Hard | `N` (placed die face value); `[N]` notation syntax; constraint notations (`[N≥X]`, `[N≤X]`, `[N=X]`, `[N:odd]`, `[N:even]`); Accumulate keyword and threshold design tiers (Tick/Charge/Ritual); the rule that effects fire only after all players place all dice; the Single Die / Multi Die capacity model |
| **Integrity System** | Hard | The "hit removes one layer" trigger; inert zone rule (0 layers = no placement); slot state lock at start of Effect Resolution Phase (Rule 12); routing rules that determine which zone receives each hit |

**Downstream dependents** (systems that depend on this one):

| System | Type | What they consume from Equipment System |
|---|---|---|
| **Equipment Degradation** | Hard | The zone structure (Head/Body/Weapon) and active layer model. Degradation owns what each layer must contain, the layer reveal event, and the degradation indicator graphic. Interface: "active layer card = current top of stack." When a layer is stripped, Equipment System updates slot state; Degradation owns the reveal logic. |
| **Character System** | Hard | Zone structure (three slots per player board). Character System assigns starting equipment sets to zones; divine gifts must not consume from the 2D6 player pool. Equipment System defines what zones are; Character System populates them. |
| **Item System** | Hard | Item Anchor Zone — the designated attachment point on each zone. Equipment System owns the anchor's existence and the one-item-per-anchor limit; Item System owns item acquisition, attachment targeting, effect rules, and item disposition when the host layer is stripped. |
| **Combat System** | Hard | The full Equipment System rule set — placement legality, effect resolution logic, zone state transitions, and the turn phase sequence scope (Placement Phase → Effect Resolution Phase). Combat owns the sequence; Equipment owns what happens within those phases. |
| **Angel System** | Soft | The Equipment System's output values (damage, healing) affect how quickly Angel layers are stripped and how long players survive. The Angel System uses `D_player_base` and `D_agg` from the registry for HP calibration. Equipment System does not call into the Angel System directly — the relationship is one of balance calibration, not rules interface. |

**Bidirectional consistency requirements:**

- When **Equipment Degradation** GDD is authored: must list Equipment System as a dependency and reference the active layer model. Must confirm the degradation indicator graphic is owned by Degradation, not Equipment.
- When **Character System** GDD is authored: must list Equipment System as a dependency; must confirm divine gifts do not consume 2D6 pool dice; must add itself to the `T_board` formula's `referenced_by` in the registry.
- When **Item System** GDD is authored: must list Equipment System as a dependency; must define item disposition on strip; must declare whether items create new zones or only modify existing ones.
- When **Combat System** GDD is authored: must list Equipment System as a dependency and reproduce the turn phase sequence without contradicting the effect-timing rule (all effects fire after all players place all dice).
- When **Angel System** GDD is authored: must add itself to `referenced_by` for `D_agg` and `D_player_base` in the registry.

## Tuning Knobs

**1. Accumulate Threshold (X)**
- **Adjusts**: How many turns before an Accumulate effect fires
- **Safe range**: 4–20 (even numbers only — odd thresholds add token-tracking friction)
- **Tiers**: Tick 5–7; Charge 9–14; Ritual 16–20
- **Too low (< 4)**: Effect fires nearly every turn; use an immediate effect instead
- **Too high (> 20)**: Unreachable at honest feeding rates (~65% consistency); zone becomes a dead slot
- **Interaction**: A build with two competing Accumulate zones splits feeding between them — effective p drops, both T_accum values increase. Recalibrate thresholds downward if two Accumulate zones share the same build.

**2. Effect Multiplier (k in `[N]×k`)**
- **Adjusts**: Output magnitude of immediate die-scaled effects
- **Safe range**: k = 1 (baseline) to k = 2 (strong). k = 3 or higher requires a compensating constraint or a self-limiting condition (e.g., "then discard this layer").
- **Too high without a constraint**: A `[N]×3` slot at max-selection produces ~13.4 output/turn — approaching the full encounter baseline by itself. Always pair k ≥ 3 with either a strict constraint (`[N≥5]`) or a one-shot discard.
- **At k = 1 with no constraint**: baseline damage slot; safe starting point for any zone.

**3. Activation Constraint (`[N≥X]`, `[N≤X]`, `[N=X]`)**
- **Adjusts**: How frequently a slot activates and the conditional die mean when it does
- **Safe ranges** (from Formula 1 lookup table):
  - `[N≥3]`: 89% activation rate, E[N|activates] = 4.81 — nearly unconditional
  - `[N≥4]`: 75% activation rate, E[N|activates] = 5.15 — balanced constraint; primary routing tension
  - `[N≥5]`: 56% activation rate, E[N|activates] = 5.55 — demanding; effect should be strong
  - `[N≥6]`: 31% activation rate — spike-play only; effect must be high value
  - `[N≤3]`: 75% activation rate for low-die builds; primary tool for dump dice design
- **Too restrictive without payoff**: A `[N=5]` constraint on a weak effect is a dead zone. Exact-value constraints require proportionally high payoffs.
- **Interaction with dump dice**: Every set needs at least one zone that accepts the faces the other zones reject. Use the Formula 4 coverage check after adjusting any constraint.

**4. Fixed Effect Value (flat numbers in effect text)**
- **Adjusts**: Output of effects that don't scale with N
- **Safe range**: 1–8 per activation for immediate effects. Passive on-destroy effects: 4–12 (fired rarely, higher ceiling acceptable).
- **Too low (< 2)**: The effect produces less output than a baseline `[N]` die placement on average. Consider replacing with a scaled `[N]` effect instead.
- **Too high (> 8 on a recurring effect)**: Surpasses `D_player_base` from a single zone; balance using Formula 3 encounter-level check.
- **Interaction**: A fixed-value effect on a passive (no-dice) zone fires 0 or 1 times per encounter. Its contribution to S_encounter via B_burst must be verified against the [54, 74] target band.

**5. Number of Layers per Zone (L)**
- **Current value**: 3 layers per zone (9 total per player board)
- **Recommended status**: Locked at MVP. Changes require full T_board recalculation (Integrity System formula) and replaying all set balance checks.
- **At 2 layers**: Board is destroyed faster; strategic degradation is less meaningful (fewer interesting reveals per session).
- **At 4 layers**: Longer survival; encounters may lose tension if the board never approaches destruction.
- **Interaction with T_board**: `T_board = L_total / H_pt`. Changing L affects expected survival time directly — validate against the 5–8 minute encounter target.

**6. Set Balance Target (`T_target` = 64)**
- **Adjusts**: The encounter-level output band all sets are balanced against
- **Current value**: `D_player_base (8) × T_encounter (8) = 64`; tolerance ±~10 units → band [54, 74]
- **If D_player_base shifts in playtests**: recalibrate T_target accordingly. All set balances must be re-checked against the new target.
- **If T_encounter shifts** (encounters run shorter or longer than 8 turns): recalibrate T_target. A 6-turn encounter target gives T_target = 48 — existing sets may become over-budget.
- **Interaction**: This is a derived constant. The two tuning inputs are D_player_base (Integrity System registry) and T_encounter (from playtests). Both must be validated before finalizing set balance.

## Visual/Audio Requirements

[To be designed]

## UI Requirements

[To be designed]

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Gate levels: BLOCKING = must pass before production; ADVISORY = flag for review; DEFERRED = requires simulation or playtest data.*

---

### Core Rules

**AC-01** — GIVEN a player board in any game state, WHEN the board is placed on the table, THEN it displays exactly three labelled zones — Head (top), Body (middle), Weapon (bottom) — separated by visible dividing lines, with no other active zones present.

**AC-02** — GIVEN an equipment card in any zone, WHEN a player reads the card, THEN the card displays exactly one of three constraint indicator states: Single Die icon, Multi Die icon, or absent — no other indicator state is valid.

**AC-03** — GIVEN a zone with a Multi Die indicator and additional constraint `[N≥4]`, WHEN a player places a die showing 3 on that zone, THEN the die is consumed with no effect and nothing is added to any Accumulate total; a die showing 4 or higher on the same zone in the same turn produces its effect normally.

**AC-04** — GIVEN a zone with no constraint indicator (passive), WHEN a player attempts to place a die on that zone, THEN the placement is illegal; the die must be redirected to an eligible zone or discarded.

**AC-05** — GIVEN a passive zone whose effect reads "When this layer is destroyed, deal 6 damage", WHEN that layer is stripped by any hit during any phase, THEN 6 damage fires at the moment of destruction without requiring a die placement.

**AC-06** — GIVEN a zone with a Single Die indicator, WHEN a player places one die on that zone during the Placement Phase, THEN a second die cannot be placed on the same zone this turn; the second die must go elsewhere or be discarded.

**AC-07** — GIVEN a zone with a Multi Die indicator and no additional constraint, WHEN a player places two dice (e.g., values 2 and 5) on that zone in one turn, THEN both dice are accepted; each die resolves as a separate, independent effect in party-chosen order; each die's N is read individually.

**AC-08** — GIVEN an Accumulate zone (threshold 8, running total 0), WHEN a player places two dice showing 3 and 5 in one turn, THEN the placing player decides which die is added first; the first die raises the total without triggering; the second raises it to 8, the effect fires once, the total resets to 0.
> ⚠️ *Playtest note: sub-ordering friction flagged — validate in session 1; simplify to "add both simultaneously, check threshold once" if players find it confusing.*

**AC-09** — GIVEN multiple players each with at least one die placed, WHEN the last player finishes placing, THEN no effect has fired yet; all effects begin resolving only when all players have completed placements.

**AC-10** — GIVEN an Accumulate zone with constraint `[N≥4]` and running total 7 (threshold = 8), WHEN a player places a die showing 2, THEN the die is consumed; the total remains at 7; the Accumulate effect does not fire.

**AC-11** — GIVEN multiple placed dice about to resolve and a proposed resolution order, WHEN the party wishes to change the order before any effect fires, THEN the change is legal; once the first effect fires, the remaining sequence is locked.

**AC-12** — GIVEN a locked resolution order [Effect A, Effect B, Effect C] where A has already fired, WHEN a player attempts to resolve C before B, THEN the action is illegal; B must resolve before C.

**AC-13** — GIVEN a zone with an item already in its Item Anchor Zone, WHEN a second item attempts to attach to the same zone, THEN the second item cannot attach; one item per zone maximum.

**AC-14** — GIVEN a zone with an item attached to its active layer, WHEN that layer is stripped by any hit or card effect, THEN the item departs with the stripped layer; the newly revealed layer's Item Anchor Zone is vacant.

**AC-15** — GIVEN an inert zone (0 layers), WHEN a player attempts to place a die on it, THEN the placement is illegal; the die must be redirected or discarded.

**AC-16** — GIVEN an Accumulate zone (threshold 8, running total 3), WHEN a player places a die showing 6 (new total = 9), THEN the effect fires exactly once; the total resets to 0; the overage of 1 is discarded.

**AC-17** — GIVEN an Accumulate zone (threshold 8, running total 6), WHEN a player places a die showing 6 (potential total 12), THEN the effect fires once; the total resets to 0; the start-of-next-turn total is 0, not 4.

---

### Formula Criteria

**AC-18** *(BLOCKING)* — GIVEN a zone with no additional constraint (p_c = 1.00, k = 1), WHEN E_slot is computed using E[N] = 4.47, THEN E_slot = 4.47; this is the baseline output reference for an unconstrained single-die zone.

**AC-19** *(BLOCKING)* — GIVEN a zone with constraint `[N≥4]`, k = 1, WHEN E_slot is computed (p_c = 0.75, E[N|N≥4] = 5.15), THEN E_slot = 3.86 damage/turn at max-selection play.

**AC-20** *(BLOCKING)* — GIVEN a zone with no additional constraint and effect `[N]×2` (k = 2, p_c = 1.00, E[N] = 4.47), WHEN E_slot is computed, THEN E_slot = 8.94; this is the expected one-shot peak output for that zone.

**AC-21** *(BLOCKING)* — GIVEN an Accumulate zone with threshold X = 6, WHEN T_accum is computed (X / 2.275), THEN T_accum ≈ 2.64 turns — Tick tier; valid threshold.

**AC-22** *(BLOCKING)* — GIVEN an Accumulate zone with threshold X = 12, WHEN T_accum is computed, THEN T_accum ≈ 5.27 turns — Charge tier; valid threshold.

**AC-23** *(BLOCKING)* — GIVEN an Accumulate zone with threshold X = 18, WHEN T_accum is computed, THEN T_accum ≈ 7.91 turns — Ritual tier; valid threshold.

> ⚠️ *AC-21–23: Use 2.275/turn (strict formula). Reconcile against the Dice Economy GDD's ~2.75/turn figure after first playtests before finalizing tier thresholds.*

**AC-24** *(BLOCKING)* — GIVEN an Accumulate zone with threshold X = 3 (below floor of 4), WHEN a QA tester reviews the card, THEN the card is flagged as illegal card design; treat as threshold = 4 at the table until corrected.

**AC-25** *(ADVISORY — DEFERRED for conditional burst sets)* — GIVEN a completed equipment set with all three zones specified, WHEN a card author computes S_encounter = (S_throughput × 8) + B_burst, THEN S_encounter must fall within [54, 74]; any set outside this range is flagged for redesign. Sets with conditional burst effects whose trigger probability is not explicitly stated are DEFERRED pending a playtest-derived estimate.

**AC-26** *(ADVISORY — known open item)* — GIVEN the Martyrdom set as specified (Head: Accumulate 8, Body: passive deal 6, Weapon: `[N]×2` one-shot), WHEN S_encounter is computed at T_encounter = 8, THEN S_encounter = 17.24 — below the [54, 74] target band; the Martyrdom set must be redesigned before production. No card goes to print until this is resolved.

**AC-27** *(BLOCKING)* — GIVEN any starting equipment set, WHEN the Formula 4 coverage table is filled for die faces 1–6 across all three zones, THEN no row shows "No" for all three zones; every face from 1–6 must be accepted by at least one zone.

**AC-28** *(BLOCKING)* — GIVEN a set where die values 1 and 2 are rejected by all three zones (e.g., all zones `[N≥3]` or higher), WHEN both dice roll 1 and 2, THEN the player has no legal die placements producing an effect — coverage failure; the set must be revised.

**AC-29** *(BLOCKING)* — GIVEN the Martyrdom set (Head: Accumulate 8 no additional constraint; Body: passive; Weapon: no additional constraint), WHEN the coverage table is filled, THEN Head and Weapon each accept faces 1–6; every face has at least one home; coverage passes.

---

### Edge Case Verification

**AC-30** — GIVEN an Accumulate zone (`[N≥4]`, running total 5), WHEN a player places two dice both showing 2, THEN both are consumed without effect; total remains at 5; Accumulate does not fire.

**AC-31** — GIVEN an Accumulate zone (threshold 8, running total 5), WHEN a player places dice showing 4 and 3 and declares 4 resolves first, THEN the 4 raises total to 9 → effect fires once → total resets to 0; the 3 adds to the reset total leaving it at 3; no second triggering occurs.

**AC-32** — GIVEN a zone that had a die placed during the Placement Phase and a card effect strips that zone's layer earlier in the same Effect Resolution Phase, WHEN the turn reaches the committed die's resolution slot, THEN the die **fizzles** — it is consumed without effect; the stripped layer takes it. GIVEN the die's slot in the resolution sequence was positioned *before* the stripping effect, THEN the die fires normally; the strip resolves after.
> *Resolution order controls this outcome — party should position the die's slot accordingly. Confirm consistency with Integrity GDD.*

**AC-33** — GIVEN a player who discards both dice without placing either, WHEN the Effect Resolution Phase begins, THEN no die effects fire for that player; all Accumulate totals are unchanged; the turn is legal with no penalty.

**AC-34** — GIVEN a zone whose effect reads "When any ally places a 6 on their zone", WHEN a card author or QA tester reviews it, THEN the card is flagged as illegal card design; effects may only reference the current player's own board.

---

### Open Flags

| ID | Flag | Action Required |
|---|---|---|
| AC-08 | Sub-ordering of two dice on same Accumulate zone may cause table friction | Validate in session 1; simplify if needed |
| AC-21–23 | 2.275 vs 2.75/turn inconsistency with Dice Economy GDD | Reconcile after first playtests; re-check tier thresholds |
| AC-25 | Conditional burst sets require explicit trigger probability in card design docs | Mark probability assumptions on card docs before balance sign-off |
| AC-26 | Martyrdom set is under-budget (known) | Redesign before production |
| AC-32 | Depends on Integrity System Rule 12 | Confirm Integrity GDD Rule 12 is consistent before testing |

## Open Questions

[To be designed]
