# Character System

> **Status**: Needs Revision — propagation update from angel-system.md redesign (2026-05-27)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-27
> **Implements Pillar**: Agency Over Dice (primary); Cooperative Ownership (secondary); Combos Must Be Discoverable (supporting)
> **CD-GDD-ALIGN**: APPROVE (2026-05-25) — three forward notes: (1) confirm Martyr routing is sometimes wrong in playtest; (2) verify SR-5 contract at Equipment/Item GDD review; (3) keep "held argument" framing fenced to Player Fantasy section only.

## Overview

The Character System defines each player's position in S-Layers: a **player board** with three equipment slots (Head, Body, Weapon), a **divine gift** usable once per turn at the player's discretion, and a **starting equipment set** — three layered equipment stacks pre-assigned to those slots at session start. These three elements are designed as a unit: each character is a named strategy, with a gift that pushes one play style and a starting set calibrated to reinforce it. From turn one, every player at the table has a distinct angle on how to build, route damage, and engage with the dice pool — not because they have different stats, but because their gift and opening board point in the same direction.

The divine gift is a once-per-turn ability that resolves independently of dice placement — it does not draw from the 2D6 player pool (established by Dice Economy GDD) and therefore adds a decision layer on top of, not in competition with, the placement choices. The gift's activation is entirely voluntary: the player chooses each turn whether to use it and when, relative to their die effects' resolution order. Starting equipment sets are assigned at session start according to each character's design; the three-slot structure, placement legality, and effect resolution rules that govern those sets are owned by the Equipment System GDD, which the Character System populates but does not redefine.

Characters do not advance between sessions. Growth within a session happens through equipment degradation (layers reveal) and item acquisition — the divine gift and player board structure are fixed. The character is the starting condition; the session is the transformation.

## Player Fantasy

You did not pick a class. You picked a sentence Heaven had already begun writing about you.

Your board arrives arranged. Three equipment stacks, one gift — read together, they spell out a strategy the designers laid like a trap: *this one is built to be hit; this one is built to accumulate; this one wants to go fast and burn out*. Read separately, they spell out three more strategies. And then the session begins, and the items arrive, and the layers strip, and the thing you were designed to be is no longer the only thing you are.

The moment of character selection is the moment the game makes its first offer. It says: *here is a starting sentence. Here is a gift that rhymes with your opening board. Here is the play we imagined you making.* You feel the intended read immediately — the gift and the slots and the first three cards point at each other like a diagram. The first time you sit down with a new character, that feeling is the gift. The second time, you already see the subversion.

The divine gift is small. Once per turn. It does not take your dice, because Heaven did not think to build a version of the gift that competes with what it handed you. You decide each turn whether to spend it — and most turns, you do not. You are learning to hold it. You are learning which moment it was made for and whether that moment is the one in front of you right now, or three turns away, or on a teammate's board instead of yours. The gift is not a cooldown. It is an argument you are preparing to make.

The broken build is what the game promises from the beginning. The character is the specific broken build they thought you would build. Your job, for the next sixty minutes, is to find out if they were right — and to be more interested in the answer if they were wrong.

**Design test**: At character selection, every player at the table should be able to read their gift + their three starting stacks and identify the intended strategy within two minutes — without reading a rulebook. If the strategy is invisible, the set needs redesigning. If the strategy is the *only* strategy, the gift needs redesigning. The target: legible from card one, subvertible by card five.

## Detailed Design

### Core Rules

---

**Rule 1 — Player Board Physical Structure**

The player board is a portrait-format physical board divided into five named areas, top-to-bottom:

| Area | Contents | Governed by |
|---|---|---|
| **Identity Area** | Character name, illustration, flavor tag (≤12 words) | Character System |
| **Gift Area** | Gift name, effect text (≤12 words), activation token socket | Character System |
| **Head Zone** | Equipment stack (layer count per character) | Equipment System |
| **Body Zone** | Equipment stack (layer count per character) | Equipment System |
| **Weapon Zone** | Equipment stack (layer count per character) | Equipment System |

The Gift Area hosts one **activation token socket**. Token in socket = gift available this turn. Token removed = gift spent. The token is placed at the start of each player's turn and removed on activation.

---

**Rule 2 — Variable Layer Count Per Zone**

Each character defines its own layer count per zone. The default is 3 per zone. Character-specific exceptions are printed on the board and define the character's resilience profile.

| Character | Head | Body | Weapon | L_total |
|---|---|---|---|---|
| The Pilgrim | 3 | 2 | 3 | 8 |
| The Martyr | 3 | 4 | 3 | 10 |
| Default reference | 3 | 3 | 3 | 9 |

`L_total` feeds `T_board = L_total / H_pt`. All layer configurations must be validated against T_board during character design.

> *The Martyr's 4-layer Body is intentional: nearly all hits route to Body, and each destroy trigger is valuable. The Pilgrim's 2-layer Body reflects a fragile, fast-burn character who extracts value before the board collapses.*

---

**Rule 3 — Starting Equipment Assignment**

MVP: fixed starting sets. Each character has a predefined stack for each zone — Layer 1 on top, sorted in layer order. Assignment is public.

Setup procedure:
1. Player selects a character board.
2. They receive three pre-sorted face-down card stacks (Head, Body, Weapon).
3. Flip the top card of each stack face-up. Layer 1 of all three zones is now visible.
4. Cards beneath remain face-down until a layer strip reveals them. *(Face-down = reduced visual noise, not hidden information. Players may inspect any stack on request.)*

---

**Rule 4 — Divine Gift: General Rules**

1. Each character has exactly one divine gift, printed in the Gift Area.
2. Activatable **once per player turn**, fully voluntary.
3. Does **not** consume from the 2D6 player dice pool.
4. May be activated even if it produces no measurable game-state change — the token is spent regardless.

---

**Rule 5 — Divine Gift: Activation Timing**

Gifts activate during the **Placement Phase**, at one of these moments:

- Before rolling the 2D6
- After rolling, before placing any die
- After placing the first die, before placing the second
- After placing all dice, before Effect Resolution begins

> *Both MVP gifts operate during the Placement Phase. Future characters may define different timing — any such exception is stated on the character board.*

---

**Rule 6 — Divine Gift: N-Modification Ruling**

Gifts operate within the same boundary as items: they modify **the output of effects**, never N itself (Dice Economy Rule 6).

Two exceptions, both inherited from Dice Economy edge cases:

- **Re-roll gifts**: A re-roll produces a new raw face value; that new face becomes N for all subsequent reads.
- **Chosen-face die gifts**: A die created with a player-chosen face has that face as its raw N — the chosen face IS the raw face. No prior value is being modified.

Neither exception breaks Rule 6. N is always the raw face of the die as it enters play.

---

**Rule 7 — The Pilgrim's Gift: "Discard for Future Die"**

**Gift text**: `Sacrifice 1 die. Next turn: gain 1 extra die with any face you choose.`

- **Activation timing**: During Placement Phase, before the sacrificed die would otherwise be placed.
- The sacrificed die is removed from the current turn — it cannot be placed.
- At the start of the **next** Placement Phase, before rolling 2D6, the player declares a face value (1–6). They receive one additional die showing that face, then roll their 2D6 normally.
- That turn, the player holds **3 dice** (the extra die + rolled 2D6).
- The extra die's N = declared face (raw value). It may be placed on any zone subject to normal rules.
- The extra die expires at end of that Placement Phase. It cannot be carried to a further turn.

> *This gift is a temporal trade: sacrifice output now for perfect N control next turn. The decision: which face to declare, and which zone that face is worth sacrificing a die to feed.*

---

**Rule 8 — The Martyr's Gift: "Re-roll"**

**Gift text**: `Reroll 1 die this turn. You must keep the new result.`

- **Activation timing**: After rolling 2D6, before placing either die.
- Player selects one die from their pool and re-rolls it. New face becomes N. Old value is lost.
- Player must keep the new result — no further re-rolls on this die.
- May not be applied to a die already placed this turn.

> *Not a guarantee — controlled risk. Holding two 1s against a zone needing ≥4: accept the roll, or gamble and potentially get a 2.*

---

**Rule 9 — "Gain 1 Die" Effect (The Pilgrim's Body)**

**Effect text**: `[N=1]: Gain 1 die to place this turn.`

- When a die showing 1 is placed on this zone and the `[N=1]` constraint passes, the effect fires during Effect Resolution.
- Player receives 1 additional die, rolled immediately as a standard d6.
- The additional die may be placed on any eligible zone this turn, after it is rolled.
- Follows all normal placement rules (constraint checks, single-die limits).
- If all zones already have a die, the additional die may still be placed on a Multi Die zone or discarded.
- The additional die does **not** carry to future turns.
- Pool expansion is temporary: 3 placed dice this turn only. The 2D6 base pool is unchanged.

---

**Rule 10 — Reservoir Mechanic ("Accumulate ∞", The Pilgrim's Weapon)**

**Effect text**: `Accumulate ∞: Deal damage equal to accumulated total.`

`Accumulate ∞` is a **Reservoir** — Accumulate with no threshold and player-controlled release.

**Banking:**
- Each die placed on a Reservoir zone adds its N to the running total. No automatic threshold. No ceiling.
- Total persists across turns.
- A constraint-failing die adds nothing (same as standard Accumulate).

**Release:**
- During Effect Resolution, when the Reservoir zone's slot arrives in the sequence, the player may declare "Release."
- On release: deal damage equal to current total. Total resets to 0.
- Release is optional — the total may continue accumulating.
- Release requires a non-zero total; cannot release 0.

**Multiple dice on Reservoir in one turn:**
- Both N values are added to the total. If the player also declares Release that turn, the combined total is the damage dealt.

> *The Reservoir's core decision: release now for reliable damage, or hold for an encounter-defining burst. The party often negotiates this around the Penetrating flag (Rule 11).*

---

**Rule 11 — Penetrating Damage Effect (The Pilgrim's Head)**

**Effect text**: `Accumulate 8: Next attack deals damage to all enemy layers.`

"Deals damage to all enemy layers" = **Penetrating** flag.

- When the Accumulate 8 effect fires, the Penetrating flag is set on the **next** damage-dealing event from **any** player (this turn or the following turn).
- A Penetrating damage event cascades **within the targeted slot only** — excess damage above the layer's HP carries to the next layer of the same slot (Head, Body, or Weapon). Strip checks fire sequentially within the slot, outermost first. HP_eff for each newly revealed layer is recalculated fresh before its check fires. On-destroy passives trigger outermost-first.
- **Penetrating does not cross slot boundaries.** Excess stops at the slot boundary even if damage exceeds all remaining layers in that slot. The slot becomes Inert; damage does not carry to Head, Body, or Weapon neighbours.
- The Penetrating flag is consumed after one damage event. It does not persist further.
- *(Angel System Rule 6.6 governs full Penetrating cascade resolution. Character System Rule 11 sets the flag; Angel System owns the mechanics.)*

> ✅ **RESOLVED 2026-05-27**: Penetrating is within-slot cascade only (Angel System GDD redesign). On-destroy passives fire individually in outermost-first order. Items with departure triggers on stripped layers fire separately and do not receive the Penetrating flag (Angel System edge case Rule 6.6).

---

### States and Transitions

| State | Tracked on | Persists | Resets when |
|---|---|---|---|
| Gift: Available / Spent | Activation token (socket = available; removed = spent) | Current turn only | Start of player's next turn |
| Reservoir total | Physical token/cube on Weapon zone | Across turns | Player declares Release |
| Penetrating flag | Table marker (agreed signal) | Until next damage event | One damage event resolves with Penetrating |
| Future Die pending | Side marker (token held off board) | Until start of next turn | Extra die enters play at next Placement Phase |

**Gift state transitions:**
```
Available (token in socket)
  → Spent (token removed)     [Player activates gift; token removed immediately]

Spent
  → Available (token returned) [Start of player's next turn]
```

**Reservoir transitions:**
```
Running total (0 → N)
  ↓ Die placed, constraint met  [+N to total]
  ↓ Die placed, constraint fails [total unchanged]
  ↓ Player declares Release     [damage = total; total → 0]
```

---

### Interactions with Other Systems

| System | Direction | Interface |
|---|---|---|
| **Equipment System** | Upstream | Zone structure (Head/Body/Weapon), placement legality, effect resolution. Character System assigns starting stacks; does not redefine zone rules. |
| **Dice Economy** | Upstream | 2D6 pool (gift does not consume it); `[N]` notation; re-roll edge case (new face becomes N); placement legality rules. |
| **Integrity System** | Upstream | Hit routing rules (owned by Integrity). L_total feeds T_board. Character System validates T_board per character before production. |
| **Angel System** | Bidirectional | Penetrating damage is a new input to Angel layer-strip logic. Angel System GDD must confirm Penetrating behavior (passive trigger stacking, item trigger count). Angel System's H_pt value feeds T_board validation. |
| **Equipment Degradation** | Downstream | Layer ordering per zone is Character System's design responsibility. Degradation owns the reveal mechanic; Character System defines what each layer holds and in what order. |
| **Item System** | Downstream | "Gain 1 die" creates a third die this turn — Item System must confirm that items accepting dice process the third die using identical rules as the first two. |
| **Combat System** | Downstream | Gift activation occurs within the Placement Phase. Combat System's phase sequence must accommodate gift events at any of the four Placement Phase insertion points (Rule 5). |

---

### Synergy Surface Design (Card Author Mandate — CD-SYSTEMS Concern 3)

The following are mandatory requirements for every divine gift before production. A gift failing any requirement must be redesigned.

**SR-1 — Legibility without context.** Gift text (≤12 words) must be parseable by a first-time reader without reading the character's equipment or the rulebook.

**SR-2 — Gift rhymes with starting set; does not complete it.** The gift reinforces the character's opening strategy but is not the piece that makes it function. Test: can the character play five turns without the gift and not be broken? Answer must be yes, non-trivially.

**SR-3 — At least one non-obvious synergy with non-starting equipment.** The gift must produce a materially different result with at least one item or equipment card outside the starting set, reachable through normal play. Document this interaction in the gift's design notes — reviewed at production gate.

**SR-4 — Meaningful timing decision.** At least two distinct moments must exist where activation produces different outcomes. A timing-agnostic gift should be redesigned as a passive equipment effect.

**SR-5 — Cooperative use is optional, not required.** If the gift can target a teammate's board, it must also be effective on the caster's own board as the default case. A gift only valuable on teammates undermines Pillar 1.

---

### Cooperative Ownership: Three Mandatory Cross-Board Decisions (CD-SYSTEMS Concern 2)

**Decision 1 — Character Selection: Party Composition as Gift Synergy**
At character selection, the party evaluates all starting sets and gifts as a collective. The question is not "which character do I want?" but "which gift combination does this table need?" The Pilgrim and The Martyr are designed as a pair — their cooperative identity is only legible when both boards are in view simultaneously.

**Decision 2 — Hit Routing: Variable Layer Counts as Stakes**
The Pilgrim's 2-layer Body and The Martyr's 4-layer Body create asymmetric routing incentives. Routing a hit to The Pilgrim's Body risks early destruction of the Gain-1-Die effect. Routing the same hit to The Martyr's Body triggers a passive damage chain (5/6/7/8 damage per strip). This decision requires cross-board visibility and cannot be made without knowing the teammate's current active layer.

**Decision 3 — Reservoir Release Timing: Party Coordination of the Pilgrim's Burst**
When The Pilgrim's Reservoir total is high — particularly after a Penetrating flag is set — the party must coordinate when the release fires in the global resolution order. This requires every player to know the Pilgrim's current Reservoir total (visible on their board) and to factor it into the resolution order negotiation.

## Formulas

### Formula 1 — T_board Validation per Character

The `T_board` formula is defined as:

`T_board = L_total / H_pt`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Total layers | L_total | int | {8, 10} (MVP) | Sum of all layer counts across Head, Body, and Weapon zones |
| Expected hits per player per turn | H_pt | float | [0.3, 1.5] | Expected hits one player receives per turn; derived from Angel face map via Angel System |
| Board survival time | T_board | float | [1.0, ∞) | Expected turns before a single player's board is fully destroyed |

**Output Range:** T_board must satisfy T_board ≥ T_encounter = 8 at expected H_pt. Values below 4 represent design failure (board destroyed in first half of encounter under concentrated fire).

**Validation table:**

| Character | L_total | H_pt = 1.0 | T_board | H_pt = 1.5 | T_board | Judgment |
|-----------|---------|-----------|---------|-----------|---------|----------|
| The Pilgrim | 8 | 1.0 | 8.0 turns | 1.5 | 5.3 turns | BORDERLINE at H_pt = 1.5 |
| The Martyr | 10 | 1.0 | 10.0 turns | 1.5 | 6.7 turns | ACCEPTABLE at both checkpoints |

**Interpretation:** The Pilgrim at H_pt = 1.5 falls below the 8-turn target. This is only acceptable if the party actively routes hits away from the Pilgrim — the Martyr's 4-layer Body is the primary routing defense. Worst-case (all hits directed at one player in a 2-player party): effective H_pt doubles. Pilgrim at H_pt = 1.0 directed entirely = 4.0 turns — destroyed at the encounter's halfway point. Routing negotiation is load-bearing, not optional.

**Design constraint:** Any future character with L_total ≤ 6 requires H_pt ≤ 0.75 or explicit routing support from a companion character. Flag any L_total below 8 for mandatory T_board verification before production.

**Example:** The Pilgrim, Angel with H_pt = 1.2. With active routing support (Martyr absorbs 40% of hits): effective H_pt for Pilgrim ≈ 0.72. `T_board = 8 / 0.72 ≈ 11.1 turns` — safely exceeds encounter window.

---

### Formula 2 — Gain-1-Die Probability Impact (The Pilgrim's Body)

The `E_body_gain` formula is defined as:

`E_body_gain = P_min(1) × E[N_extra]`

where `P_min(1) = (13 − 2×1) / 36 = 11/36 ≈ 0.306`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Probability of placing a 1 | P_min(1) | float | 11/36 ≈ 0.306 | Probability the lower of 2D6 shows exactly 1 (min-selection strategy; Dice Economy Formula 4) |
| Expected value of gained die | E[N_extra] | float | 3.5 | Extra die rolled as a fresh d6 — random; E[N] = 3.5 |
| Expected additional output per turn | E_body_gain | float | [0, 3.5] | Expected damage-equivalent from the extra die per turn |

**Output Range:** At honest min-selection routing: ~0.9–1.1 additional damage-equivalent per turn. Additional dice gained per turn: `P_min(1) × 1 = 0.306` extra dice per turn in expectation.

**Example:** Over 10 turns, approximately 3 turns produce the extra die. Expected total extra output: 3.06 × 3.5 ≈ 10.7 damage-equivalent. The mechanical value of the extra die exceeds the raw number — it can feed the Reservoir, complete an Accumulate total, or satisfy a constrained slot the player couldn't otherwise reach.

*DEFERRED: Precise extra-die value depends on routing decisions at time of gain. Playtest observation required to narrow.*

---

### Formula 3 — Reservoir Release Timing (The Pilgrim's Weapon)

The `T_reservoir` formula is defined as:

`T_reservoir(R) = R / (f_w × E[N])`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Release total | R | int | [1, ∞) | Accumulated total at time of release (player-chosen) |
| Die mean routed to Reservoir | E[N] | float | 2.53–4.47 | Expected value of die routed to Weapon; 3.5 if random |
| Reservoir feeding fraction | f_w | float | (0.0, 1.0] | Fraction of turns player routes a die to Weapon; ≈ 0.40 in 40/60 Head-Weapon split |
| Expected turns to reach R | T_reservoir | float | [1, ∞) | Expected turns to accumulate total ≥ R from 0 |

**At 40/60 split (40% Weapon, 60% Head):** Effective input per turn = 0.40 × 3.5 = **1.4 damage/turn**.

**Key design signal:** The damage-per-turn-invested is **constant at 1.4 regardless of release timing**. Releasing early (R=8) or late (R=28) produces the same ratio. Holding is not more efficient — it is only more powerful when combined with the Penetrating flag.

**Penetrating cascade (excess-carries, within-slot — Angel System Rule 6.6):**

The damage total R is applied to the Exposed layer first. If R ≥ HP_L1, L1 strips and the **excess (R − HP_L1) carries to L2**. If excess ≥ HP_L2, L2 strips and remaining carries to L3 (if any). Each newly revealed layer's HP_eff is recalculated fresh before its strip check. Partial excess that does not clear a layer is added to that layer's Slot Tracker Die; the tracker resets to 0 at end of phase (per AC-16).

This is **not** an AOE — R is applied once, cascading through layers. Total damage output = R (unchanged from a non-Penetrating release).

`n_strips(R) = max{ j : R ≥ Σᵢ₌₁ʲ HP_Lᵢ }`

The formula `D_penetrating = R × n_L` (old, pre-2026-05-27 redesign) assumed an AOE model and **is no longer valid**. Retire it.

**Cascade worked examples (n=2, 2-layer slot):**

| R | HP_L1 / HP_L2 | L1 strips? | L2 strips? | Cascade result |
|---|---|---|---|---|
| 8 | 4 / 6 | ✓ (excess=4) | ✓ (4 ≥ 6? No) → survives | L1 only; L2 tracker = 4 |
| 11 | 4 / 6 | ✓ (excess=7) | ✓ (7 ≥ 6) | **Both layers** — slot inerted |
| 11 | 6 / 10 | ✓ (excess=5) | ✗ (5 < 10) | L1 only; L2 tracker = 5 |
| 16 | 6 / 10 | ✓ (excess=10) | ✓ (10 ≥ 10) | **Both layers** — slot inerted |
| 11 | 8 / 12 | ✓ (excess=3) | ✗ (3 < 12) | L1 only; L2 tracker = 3 |

**Cooperative cascade bonus:** Partial excess deposited on L2's tracker does not strip L2 alone, but cooperating players' damage to the same slot in the same turn accumulates on top of it. The Martyr dealing 8 damage to Body on the same turn as a Pilgrim cascade leaving 5 on L2's tracker (HP=10): 5+8=13 ≥ 10 → L2 also strips. This is the intended Pilgrim+Martyr design coupling.

**Release timing rule:** Release is favorable when `R ≥ HP_L_remaining` (guarantees the Angel layer is stripped this turn). For a two-layer strip in one event: hold until `R ≥ HP_L1 + HP_L2`.

**Example:** Encounter turn 5. Reservoir total ≈ 7. Head Accumulate total = 6; will cross 8 in ~2 turns. Medium-protection slot in play (HP_L1=6, HP_L2=10). Hold: in 2 turns, R ≈ 9.8. Pen fires. Cascade: L1 strips (excess=3.8, not enough for L2=10). L2 tracker = 3.8. If Martyr also hits Body for 7 that turn: tracker = 10.8 ≥ 10 → L2 also strips. Party effectively clears the slot in one round via coordination. Solo release at R=9.8 only strips L1.

---

### Formula 4 — Gift Contribution to S_encounter (per character)

**The Pilgrim's Gift** (`G_pilgrim`):

`G_pilgrim = k_gift × (E[N_chosen] − E[N_sacrificed])`

| Variable | Symbol | Type | Range | Description |
|----------|--------|------|-------|-------------|
| Gift activations | k_gift | int | [0, T_encounter] | Turns the gift is used (typically 1–2 per encounter) |
| Chosen-face die | E[N_chosen] | float | [1, 6] | Declared face of extra die; 6 under damage-maximizing play |
| Sacrificed die | E[N_sacrificed] | float | 2.53 | Min-selected (lower die sacrificed; E = 2.53) |

Net per activation (optimal play): `6 − 2.53 = +3.47`
`G_pilgrim (k=1) = 3.47` | `G_pilgrim (k=2) = 6.94`

*DEFERRED: k_gift (activations per encounter) from playtest.*

---

**The Martyr's Gift** (`G_martyr`):

`G_martyr = P_activate × k_gift_turns × E[gain | reroll, constraint]`

P(max of 2d6 ≤ 3 when needing ≥ 4) = `(3/6)² = 0.25` → gift activates ~25% of turns.

At unconstrained slot: `E[gain per reroll] ≈ 1.5` → `G_martyr = 0.25 × 8 × 1.5 = 3.0`

At [N≥4]×2 Weapon slot: `E[gain per reroll] ≈ 5.15` → `G_martyr = 10.3` (upper bound)

**Practical range: G_martyr ≈ 3.0–5.0 per encounter.**

*DEFERRED: P_activate under actual routing from playtest.*

---

### S_encounter Comparison (Base Equipment + Gift)

> ✅ **OQ-05 RESOLVED (2026-05-27):** Penetrating is an excess-carries cascade within one slot (Angel System Rule 6.6). The old `D_penetrating = R × n_L` AOE formula is retired. S_encounter values below reflect the cascade model. Pilgrim's band viability at n=2 is intentionally item-dependent; items complete the arc. See Tuning Knob 6.

| Character | S_encounter | vs. Band [54, 74] | Key variable |
|---|---|---|---|
| The Pilgrim (base, no items, n=2) | ~37–40 | **Below band** | Items required to reach band at n=2 |
| The Pilgrim (1× Type 2 item on Reservoir, n=2) | ~54–58 | **At floor / in band ✓** | Items complete the arc |
| The Pilgrim (n=3–4, R=18, full 2-layer cascade) | ~54–60 | **In band ✓** | Deeper slots allow full-slot Inert |
| The Martyr (pre-Integrity-cost) | ~79.5 | **Above band** | Self-Integrity Weapon cost not yet netted |

*S_encounter (Pilgrim base) ≈ 37–40 derivation: throughput component (Body + Gift) ≈ 23; B_burst = R ≈ 11–14 (Reservoir release, cascade does not change total damage output, only how many layers it clears).*

*S_encounter (Pilgrim + 1 Type 2 item on Reservoir): E_item_encounter = p_c × k_bonus × T_active² ≈ 0.65 × 3 × 3² ≈ 17.6 → S_combined ≈ 37 + 17.6 ≈ 54.6. Confirms item arc is viable.*

**Design flags:**
1. 🚩 **Pilgrim is item-dependent at n=2.** Base S ≈ 37–40. One Type 2 Output Amplification item on the Reservoir zone pushes into band. Encounter designers must not use Pilgrim for n=2 playtest sessions without at least one item in the pool. Captured in Tuning Knobs.
2. 🚩 **Martyr above band until Integrity cost netted.** The Weapon's self-degradation IS the balancing mechanism — must be confirmed after Equipment Degradation GDD defines Weapon layer HP pool.
3. ℹ️ **Reservoir efficiency is flat.** If the Penetrating synergy is ever cut, the Reservoir needs a redesigned holding incentive.
4. ℹ️ **Cascade partial excess enables cooperative play.** At n=2, the Pilgrim's Penetrating cascade deposits partial excess on L2's tracker, which the Martyr's damage can complete in the same turn. This is the designed coupling — solo Pilgrim is below band; Pilgrim+Martyr party is in band.

---

### DEFERRED Items

| ID | Formula | Blocked by |
|---|---|---|
| D-1 | T_board | Angel System — H_pt at actual Angel face maps |
| D-2 | E_body_gain | Playtest — Pilgrim routing decisions |
| D-3 | T_reservoir | Angel System — Penetrating multi-layer passive trigger behavior |
| D-4 | G_pilgrim | Playtest — k_gift activations per encounter |
| D-5 | G_martyr | Playtest — P_activate under actual routing |
| D-6 | Martyr S_encounter | Equipment Degradation — Weapon layer HP pool depth |

## Edge Cases

**Gift — Activation Timing**

**If the Pilgrim activates their gift before rolling 2D6**: illegal — the gift requires sacrificing a die and no die exists yet. The gift activates only at the second through fourth Placement Phase windows (after rolling). Token is not removed.

**If the Martyr attempts to activate their gift after placing one or both dice**: illegal — Rule 8 limits the re-roll to "after rolling, before placing either die." If any die is already placed, the gift cannot be activated this turn. Token remains.

**If the Martyr re-rolls a die and the new result matches the original face**: gift is consumed, token removed. "Must keep the new result" is satisfied. No second re-roll.

**If the Pilgrim activates the gift on the final turn of an encounter**: the future die never enters play. The sacrifice produces no return. Legal but strictly losing.

**If the Pilgrim activates the gift but the next turn begins with all zones inert**: the extra chosen-face die enters play but must be discarded — no eligible zones.

---

**Reservoir**

**If the Pilgrim attempts to release the Reservoir at total 0**: illegal. Release requires a non-zero total. The resolution slot arrives and passes with no effect.

**If the Reservoir zone's layer is stripped while the total is non-zero**: the total is lost with the stripped card. The newly revealed Weapon layer begins at zero. Reservoir totals are tied to the card, not to a global tracker.

**If two dice are placed on the Reservoir and the player declares Release that turn**: both N values are added first, then the full combined total is dealt as damage. One release event.

**If a die placed on the Reservoir would resolve later in the sequence but the Weapon zone is stripped earlier in the same phase**: the die fizzles. Position the die's resolution slot before any effect that could strip the Weapon zone if the player wants it included in the Release.

**If the Reservoir accumulates beyond physical tracker capacity**: use alternate tracking (spare dice, pencil/paper). No mechanical ceiling exists.

---

**Gain 1 Die**

**If the Body "Gain 1 Die" effect fires but all zones are inert**: the extra die enters play and must be discarded immediately — no legal zones. The effect fired legitimately.

**If the extra die from "Gain 1 Die" rolls a 1 and the player wants to place it on the Body zone again**: legal if Body is a Multi Die zone. If Body is Single Die with a die already placed, the extra die cannot go there — redirect or discard.

**If the extra die from "Gain 1 Die" is placed on the Reservoir**: legal. Its N adds to the Reservoir total. If the player also declares Release this turn, the extra die's contribution is included in the burst.

**If the Pilgrim forgets the extra die expires at end of the Placement Phase**: the die is removed. It cannot be held to a future turn.

**If the Body zone's layer is stripped mid-resolution before the "Gain 1 Die" die resolves**: the die fizzles. No extra die is generated.

---

**Penetrating**

**If the Penetrating flag is set and no damage event fires before the encounter ends**: the flag expires at end of encounter. It does not carry to the next encounter.

**If the Penetrating flag is set and the Angel has only one layer remaining**: Penetrating fires at the one layer at full value. Flag is consumed. Outcome is functionally identical to a normal hit.

**If the Penetrating flag is set and the next damage event is from the Martyr** (not the Pilgrim): Penetrating applies. The flag is not character-specific.

**If the Penetrating flag is set and multiple damage events fire in the same sequence**: the flag is consumed by the first damage event in the declared resolution order. Subsequent events are normal. Sequence your highest-value damage first.

**If the Penetrating flag is set and the Martyr's Weapon fires (external damage + self-Integrity-loss)**: Penetrating applies to the external Angel damage only. The self-Integrity-loss component does not receive or consume the Penetrating flag.

**If the Penetrating flag is set and the damage event strips a layer that has an attached item with a departure trigger**: the Penetrating flag is consumed by the die effect's damage — not by the item's departure trigger. The item departure trigger fires after the on-destroy passive of the stripped layer, and is treated as a separate non-Penetrating damage event. Penetrating does not propagate to item departure triggers.

**Penetrating multi-layer strip — RESOLVED 2026-05-27 (within-slot cascade only)**: If Penetrating damage exceeds the HP_eff of successive layers in the **targeted slot**, those layers strip sequentially in the same phase, outermost first. Excess carries within the slot; the slot may be fully Inerted in a single Penetrating event. On-destroy passives fire outermost-first. HP_eff for each newly revealed layer within the slot is recalculated fresh before its check fires. Penetrating does **not** cross slot boundaries (Head/Body/Weapon remain independent). See Integrity System Rule 20 and Angel System Rule 6.6.

---

**Variable Layer Counts**

**If the Pilgrim's 2-layer Body is destroyed before the encounter's midpoint**: Body becomes inert. Pilgrim loses access to "Gain 1 Die" for the rest of the encounter. Intentional design risk — the 2-layer Body is expected to collapse.

**If the Martyr's 4-layer Body receives 4 hits in one Angel Resolution Phase**: all four layers are stripped in sequence. Each on-destroy passive fires in party-chosen order. After 4 strips, Body is inert. The four passives (5, 6, 7, 8 damage) resolve independently — party controls the sequence.

**If all hits in a turn are routed to the Pilgrim's 2-layer Body and 2 hits arrive**: both layers stripped. Body becomes inert. Standard multi-hit routing per Integrity System rules.

---

**Martyr Weapon Self-Damage**

**If the Martyr's Weapon fires and self-strips during Effect Resolution, and a die was placed on that zone for later resolution**: the die fizzles when its slot arrives. Position the second die's resolution before the self-stripping activation if both must fire.

**If the Martyr's Weapon self-strips during Effect Resolution, then the Angel routes a hit to the Weapon zone**: Weapon is inert before the Angel Resolution Phase begins. A targeted hit to an inert zone is wasted; a party hit must be re-routed.

**If all three Martyr Weapon layers are consumed across three turns**: Weapon zone is permanently inert. This is the intended lifespan: 3 activations (×2 → ×3 → ×4), each self-stripping, then exhaustion.

**If the Martyr's Weapon fires**: damage to Angel fires first, self-strip fires second (printed card order). The damage is dealt before the zone becomes inert.

**⚠️ Open ruling — Martyr L4 Body passive "to ALL enemies" timing**: If L4 fires during the Angel Resolution Phase (after the Effect Resolution tracker has reset), the 8-to-all-enemies damage contributes to the next turn's tracker for all Angels. Confirm against Angel System's damage tracker reset rules.

---

**Cross-Character**

**If the Pilgrim's "Gain 1 Die" fires and the Martyr wants to use the extra die**: not possible. The extra die is generated for the Pilgrim's board and cannot be transferred.

**If the Pilgrim's Reservoir Release and the Martyr's Weapon fire in the same sequence**: independent events. Party chooses resolution order. If Penetrating is active, sequence the damage events so the highest-value event consumes the Penetrating flag.

## Dependencies

**Upstream dependencies** (systems the Character System depends on):

| System | Type | What Character System consumes |
|---|---|---|
| **Equipment System** | Hard | Zone structure (Head/Body/Weapon), placement legality, slot capacity rules, effect resolution sequence, inert zone rules, die fizzle rule on mid-phase strip. Character System assigns starting stacks but does not redefine any Equipment System rule. |
| **Dice Economy** | Hard | 2D6 player pool (gift must not consume it); `[N]` notation; re-roll edge case (new face becomes N); `P_min(1)` probability used in E_body_gain formula; all constraint notation syntax. |
| **Integrity System** | Hard | Hit routing rules (owned by Integrity); layer strip triggers; T_board formula (`L_total / H_pt`). Character System defines L_total per character; Integrity System owns the formula. |

**Downstream dependents** (systems that depend on the Character System):

| System | Type | What they consume |
|---|---|---|
| **Angel System** | Bidirectional | Character System introduces Penetrating damage as a new input to Angel layer-strip logic. Angel System must resolve: (a) whether passive on-destroy effects from multiple simultaneous Penetrating strips fire individually or simultaneously; (b) whether items riding layer-strip events trigger once or once per stripped layer. |
| **Equipment Degradation** | Hard | Character System defines layer ordering per zone for each character. Degradation owns the reveal mechanic and layer design principles. Character System must populate starting stacks in conformance with Degradation's layer design rules. |
| **Item System** | Hard | "Gain 1 Die" creates a third die this turn — Item System must confirm that items accepting dice process the third die using identical rules as the first two. Items riding "on fire" triggers on Reservoir Release must be tested against the player-triggered release mechanic. |
| **Combat System** | Hard | Gift activation occurs within the Placement Phase. Combat System's phase sequence must explicitly accommodate gift events at any of the four Placement Phase insertion points (before rolling; after rolling, before placing; between placing; after all placed). |
| **Game Setup** | Hard | Game Setup governs character selection, starting equipment dealing, and first-turn structure — all depend on Character System's defined starting sets and board anatomy. |

**Bidirectional consistency requirements:**
- **Angel System GDD** must resolve the Penetrating multi-strip ruling before Penetrating-capable characters go to production.
- **Equipment Degradation GDD** must define the Martyr's Weapon zone Integrity pool to confirm Martyr S_encounter is within [54, 74] after self-damage netting.
- **Character System** must add itself to `referenced_by` for these registry entries: `T_board`, `D_agg`, `D_player_base`, `E_slot`, `S_encounter`, `T_target`.

## Tuning Knobs

**1. Layer Count per Zone (L_per_zone)**
- **Adjusts**: Board resilience (T_board) and zone-specific ability access duration
- **Safe range**: 2–4 per zone; L_total ∈ [6, 12]
- **Too low (< 2)**: Zone destroyed too early; ability lost before the build comes online. Requires explicit design justification.
- **Too high (> 4)**: T_board exceeds encounter length even under concentrated fire; tension evaporates. Also inflates the Degradation card count per character.
- **Interaction**: Changing L_total requires T_board recalculation at both H_pt = 1.0 and H_pt = 1.5, plus a full Degradation layer design pass.

**2. Reservoir Expected Input Rate (f_w × E[N])**
- **Adjusts**: Pilgrim Reservoir growth speed and release frequency
- **Current baseline**: f_w ≈ 0.40, E[N] = 3.5 → 1.4 damage/turn banked
- **Too high (f_w > 0.80)**: Reservoir dominates build; Head starved; Penetrating never fires; burst without multiplier → chronic underperformance.
- **Too low (f_w < 0.20)**: Reservoir fills too slowly; releases feel weak; mechanic is inert.
- **Interaction**: f_w trades directly against Head feeding. More Reservoir = delayed Penetrating. Validate 40/60 split in playtest.

**3. Head Accumulate Threshold (X = 8 for both characters)**
- **Adjusts**: How often Penetrating fires per encounter (Pilgrim) / how often healing triggers (Martyr)
- **Safe range**: 6–12
- **Too low (< 6)**: Fires nearly every other round — degrades burst to throughput; hold-Reservoir decision becomes trivial.
- **Too high (> 12)**: Fires at most once per encounter; single-trigger-dependent characters become high-variance.

**4. Martyr Weapon Self-Integrity Cost (1 per activation)**
- **Adjusts**: How quickly the Weapon zone burns through its layers (×2 → ×3 → ×4 progression speed)
- **At 0 cost**: Weapon never self-degrades; layer progression is lost.
- **At 2+ cost**: Layer 1 survives at most 1–2 uses; character is front-loaded and collapses mid-encounter.
- **DEFERRED**: Final calibration requires Equipment Degradation GDD to define Weapon layer HP pool.

**5. Martyr Head Accumulate Thresholds (L1=8 / L2=10 / L3=12)**
- **Adjusts**: Healing frequency as layers degrade (deeper layers = harder to fire the heal)
- **T_accum at L1**: ≈ 3.5 turns (fires ~2× per encounter). **At L3**: ≈ 5.3 turns (fires ~1.5×).
- **Too low (< 6 any layer)**: Martyr self-heals through most encounters; Body passive rarely threatened.
- **Too high (> 14 any layer)**: Heal fires so rarely that the 4-layer Body is the sole defensive resource.

**6. ⚠️ Pilgrim Encounter Viability — Item Dependency at n=2**
- **Design constraint (not a tuning knob)**: Pilgrim base S_encounter ≈ 37–40 at n=2 (below band [54, 74]). The Penetrating cascade (excess-carries, Angel Rule 6.6) has total damage output = R; it does not multiply R by layer count. Items complete the arc.
- **Resolution (OQ-05, 2026-05-27)**: Pilgrim is intentionally item-dependent at n=2. One Type 2 Output Amplification item on the Reservoir zone provides ~17.6 encounter-level output (p_c=0.65, k_bonus=3, T_active=3), pushing S_combined to ~54–58 — at or above the [54,74] floor.
- **Required production guidelines**:
  1. Do not run n=2 Pilgrim playtests without ≥1 item in the available pool. Flag this in Game Setup and Events System authoring.
  2. At n=3–4 (3-layer slots), Pilgrim reaches band (~54–60) at R=18 via cascade double-strip; items provide additional headroom.
  3. Cooperative cascade coupling (Penetrating partial excess + Martyr damage to same slot) is the n=2 in-band mechanism. Ensure at least one n=2 encounter per session is designed for this sequence (Angel with medium-protection Body slot, Martyr adjacent).
- **Playtest flag**: Measure Pilgrim S_encounter at first n=2 playtest. If R at Penetrating release is consistently < 8 (cascade provides nothing), the Reservoir feeding split (f_w) needs to increase from 40% → 50%.

**7. Gift Activation Frequency (k_gift)**
- **Target**: 1–2 activations per encounter for the "held weapon" fantasy. More than 3 activations degrades gift to routine action.
- **Pilgrim**: Sacrifice should always be the lower die. Card text must be clear that any die may be sacrificed — the player must understand sacrificing the high die is a mistake, not an option the card prevents.
- **Martyr**: If the gift is activated on nearly every turn, the "must keep result" deterrent isn't working — increase penalty asymmetry or tighten activatable scenarios.

## Visual/Audio Requirements

[To be designed]

## UI Requirements

[To be designed]

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Gate levels: BLOCKING = must pass before production; ADVISORY = flag for review; DEFERRED = blocked on cross-system confirmation.*

---

### Board Structure

**CS-AC-01** *(ADVISORY)* — GIVEN a fully assembled player board, WHEN a player inspects it, THEN exactly five named areas are present top-to-bottom: Identity Area, Gift Area, Head Zone, Body Zone, Weapon Zone — no other areas, no unlabeled regions.

**CS-AC-02** *(ADVISORY)* — GIVEN a player board at the start of any turn, WHEN the gift has not been activated this turn, THEN the activation token is seated in the Gift Area socket. WHEN the gift has been activated, THEN the token is removed. WHEN a new turn begins for that player, THEN the token is returned before the player rolls.

**CS-AC-03** *(BLOCKING)* — GIVEN The Pilgrim's board fully stacked, WHEN a rules judge counts layers zone by zone, THEN Head = 3, Body = 2, Weapon = 3, L_total = 8. GIVEN The Martyr's board fully stacked, THEN Head = 3, Body = 4, Weapon = 3, L_total = 10.

---

### Starting Equipment

**CS-AC-04** *(ADVISORY)* — GIVEN a character selected at session start, WHEN starting equipment is dealt, THEN the player receives exactly three pre-sorted face-down card stacks (Head, Body, Weapon) containing the predefined layer set in layer order (Layer 1 on top).

**CS-AC-05** *(ADVISORY)* — GIVEN the three stacks dealt to a player, WHEN setup is complete, THEN the top card (Layer 1) of each stack is face-up and visible to all players; all remaining cards stay face-down until a layer strip reveals them.

**CS-AC-06** *(ADVISORY)* — GIVEN any face-down card in a player's stack, WHEN a player or opponent requests to inspect it, THEN inspection is allowed — face-down status is visual noise reduction, not hidden information.

---

### Gift — General

**CS-AC-07** *(BLOCKING)* — GIVEN a player who has already activated their gift this turn (token removed), WHEN that player attempts to activate it a second time, THEN the activation is illegal; the effect does not fire; the token state is unchanged.

**CS-AC-08** *(BLOCKING)* — GIVEN a player who activates their gift, WHEN the gift resolves, THEN the number of dice in the 2D6 pool for this turn is unchanged — no die is consumed by gift activation.

**CS-AC-09** *(ADVISORY)* — GIVEN a player who activates their gift but the gift produces no measurable change to game state, WHEN the activation resolves, THEN the token is still removed; a wasted activation is legal and the token is spent regardless.

**CS-AC-10** *(ADVISORY)* — GIVEN a player whose gift token was removed this turn, WHEN that player's next turn begins, THEN the token is returned to the socket before the player rolls.

---

### Gift — Activation Timing

**CS-AC-11** *(BLOCKING)* — GIVEN The Pilgrim has not yet rolled their 2D6 this turn, WHEN The Pilgrim attempts to activate the gift, THEN the activation is illegal — no die exists to sacrifice; token is not removed.

**CS-AC-12** *(BLOCKING)* — GIVEN The Martyr has placed one or both dice on zones this turn, WHEN The Martyr attempts to activate the re-roll gift, THEN the activation is illegal — the window has closed; token is not removed.

**CS-AC-13** *(BLOCKING)* — GIVEN The Martyr rolls 2D6 and re-rolls a die that lands on the same face as the original, THEN the gift is consumed (token removed); that face becomes N; no second re-roll is permitted.

**CS-AC-14** *(ADVISORY)* — GIVEN any MVP gift (Placement Phase only), WHEN a player attempts to activate it during Effect Resolution or Angel Resolution Phase, THEN the activation is illegal; token is not removed.

---

### Pilgrim Gift — Future Die

**CS-AC-15** *(BLOCKING)* — GIVEN The Pilgrim activated their gift on turn N (one die sacrificed), WHEN turn N+1's Placement Phase begins before rolling, THEN The Pilgrim declares a face value (1–6), receives one additional die showing that face, then rolls 2D6 normally — holding 3 dice total this Placement Phase.

**CS-AC-16** *(BLOCKING)* — GIVEN The Pilgrim holds the extra chosen-face die, WHEN effects resolve, THEN the chosen face is the die's raw N — treated identically to a naturally rolled result for all placement and constraint checks.

**CS-AC-17** *(BLOCKING)* — GIVEN The Pilgrim holds the extra die and the Placement Phase ends without it being placed, WHEN Effect Resolution begins, THEN the extra die expires and is removed — it cannot be carried to any future turn.

**CS-AC-18** *(ADVISORY)* — GIVEN The Pilgrim activates the gift on the final turn of the encounter, WHEN the encounter ends, THEN the future die never enters play; the sacrifice produces no return; the activation is legal with no other rule violated.

---

### Pilgrim Body — Gain 1 Die

**CS-AC-19** *(BLOCKING)* — GIVEN The Pilgrim's Body zone is active with `[N=1]` constraint, WHEN a die showing any value other than 1 is placed there, THEN the constraint fails; die consumed with no effect; no extra die is generated.

**CS-AC-20** *(BLOCKING)* — GIVEN The Pilgrim places a die showing 1 on the Body zone and the constraint passes, WHEN the effect resolves during Effect Resolution, THEN one additional die is rolled immediately as a standard d6 (random, not pre-selected).

**CS-AC-21** *(BLOCKING)* — GIVEN The Pilgrim gains the extra die via Body effect, WHEN evaluated this turn, THEN the extra die may be placed on any eligible zone this turn; it does not carry to future turns; the 2D6 base pool remains 2 dice per turn.

**CS-AC-22** *(ADVISORY)* — GIVEN The Pilgrim's Body zone's layer is stripped mid-resolution before the placed die resolves, WHEN the Body zone's resolution slot arrives, THEN the die fizzles — no extra die is generated.

---

### Reservoir

**CS-AC-23** *(BLOCKING)* — GIVEN The Pilgrim's Weapon Reservoir zone is active, WHEN a die is placed on it, THEN the die's N is added to the running total unconditionally — no ceiling, no threshold, no auto-fire; total persists until player-triggered release.

**CS-AC-24** *(BLOCKING)* — GIVEN The Pilgrim's Reservoir running total is 0, WHEN the Weapon zone's resolution slot arrives and the player attempts to declare Release, THEN Release is illegal; no damage is dealt; the slot resolves with no effect.

**CS-AC-25** *(BLOCKING)* — GIVEN The Pilgrim's Reservoir total is any positive integer (e.g., 14), WHEN the player declares Release during Effect Resolution at the Weapon zone's slot, THEN damage equal to 14 is dealt to the target; the total resets to 0 immediately.

**CS-AC-26** *(BLOCKING)* — GIVEN The Pilgrim's Reservoir layer is stripped while the running total is non-zero, WHEN the strip event resolves, THEN the accumulated total is lost; the newly revealed Weapon layer begins at 0.

**CS-AC-27** *(BLOCKING)* — GIVEN The Pilgrim places two dice on the Reservoir (e.g., values 4 and 5) and declares Release that turn, WHEN the Weapon zone resolves, THEN damage = (prior total + 4 + 5); total resets to 0; this is one Release event.

---

### Penetrating

**CS-AC-28** *(BLOCKING)* — GIVEN The Pilgrim's Head Accumulate total reaches or exceeds 8 and the effect fires, WHEN the effect resolves, THEN the Penetrating flag is set; a visible table marker is placed to indicate the flag is active.

**CS-AC-29** *(BLOCKING)* — GIVEN the Penetrating flag is active, WHEN the next damage-dealing event resolves (from any player, any zone, this turn or the following turn), THEN that damage event cascades within the targeted slot: the full damage is applied to the Exposed layer; if it meets or exceeds HP_eff, the layer strips and excess carries to the next layer of the same slot. Penetrating does not cross slot boundaries. *(See Angel System Rule 6.6 for full cascade procedure.)*

**CS-AC-30** *(BLOCKING — RESOLVED 2026-05-27)* — GIVEN a Penetrating damage event resolves against an Angel, WHEN the cascade is evaluated, THEN: (a) damage cascades within the declared slot only — outermost layer strips first, excess carries to next layer of same slot; (b) each layer's on-destroy passive fires individually in outermost-first order; (c) HP_eff for each newly revealed layer is calculated fresh before its strip check; (d) Penetrating does not carry to other slot types (Head/Body/Weapon boundaries are hard stops). *Verified by:* Angel System ACs AC-16, AC-17, AC-18 cover the cascade scenarios.

**CS-AC-31** *(BLOCKING)* — GIVEN the Penetrating flag is active and one damage event fires and resolves, WHEN that event completes, THEN the Penetrating flag is consumed and the table marker is removed; all subsequent damage events are normal.

**CS-AC-32** *(ADVISORY)* — GIVEN the Penetrating flag is set and no damage event fires before the encounter ends, WHEN the encounter ends, THEN the Penetrating flag expires; the marker is removed; the flag does not carry to the next encounter.

---

### Martyr Gift — Re-roll

**CS-AC-33** *(BLOCKING)* — GIVEN The Martyr has rolled 2D6 and placed no dice yet, WHEN The Martyr activates the gift and re-rolls one die, THEN the new face becomes N for that die; the previous face is discarded and has no further effect.

**CS-AC-34** *(BLOCKING)* — GIVEN The Martyr re-rolls a die and the new result is identical to the original (e.g., both 4), THEN the gift is consumed; the new result (4) is kept as N; no second re-roll is permitted.

**CS-AC-35** *(BLOCKING)* — GIVEN The Martyr has placed one die on any zone this turn, WHEN The Martyr attempts to activate the re-roll gift, THEN the activation is illegal; the window has closed; the token is not removed.

---

### Martyr Weapon — Self-Integrity

**CS-AC-36** *(BLOCKING)* — GIVEN The Martyr's Weapon zone is active and a die is placed there, WHEN the effect resolves, THEN the damage to the targeted Angel fires first; after that damage resolves fully, the Weapon zone's layer loses 1 Integrity (self-strip event fires second, per printed card order).

**CS-AC-37** *(BLOCKING)* — GIVEN The Martyr's Weapon zone's active layer self-strips and its layer count reaches 0, WHEN the zone becomes inert, THEN no die may be placed on the Weapon zone for the remainder of the encounter; it is permanently inert.

**CS-AC-38** *(ADVISORY)* — GIVEN The Martyr's Weapon zone is inert and the Angel routes a hit to it, WHEN the hit arrives, THEN the hit is wasted — an inert zone cannot receive hits; the party must re-route the hit per Integrity System routing rules.

---

### Formula Criteria

**CS-AC-39** *(BLOCKING)* — GIVEN The Pilgrim with L_total = 8, WHEN T_board is computed as `L_total / H_pt` at H_pt = 1.0, THEN T_board = 8.0 turns exactly. Must pass before The Pilgrim goes to production.

**CS-AC-40** *(BLOCKING)* — GIVEN The Martyr with L_total = 10, WHEN T_board is computed as `L_total / H_pt` at H_pt = 1.0, THEN T_board = 10.0 turns exactly. Must pass before The Martyr goes to production.

**CS-AC-41** *(BLOCKING)* — GIVEN The Pilgrim's Body zone formula `P_min(1) = (13 − 2×1) / 36`, WHEN computed, THEN P_min(1) = 11/36 ≈ 0.306. This is the base activation rate for E_body_gain under min-selection strategy.

---

### Open Flags

| ID | Flag | Action Required |
|---|---|---|
| CS-AC-30 | Penetrating multi-strip: cascade scope and on-destroy firing order | RESOLVED 2026-05-27 — Within-slot cascade only (Angel System redesign). Outermost layer strips first; on-destroy passives fire outermost-first; HP_eff recalculated fresh for each newly revealed layer within the slot. Penetrating does not cross slot boundaries. Angel System Rule 6.6 is authoritative. |
| CS-AC-29 | Penetrating flag is not character-specific — confirm transfer to Martyr damage events | Verify in first table playtest with both characters |
| CS-AC-38 | Inert-zone hit re-routing when Martyr Weapon self-strips | Confirm Integrity System routing rules handle the inert re-route case unambiguously |

## Open Questions

[To be designed]
