# Item System

> **Status**: In Design — CD-GDD-ALIGN: CONCERNS accepted (2026-05-25, 3 revisions applied: Rule 3 split-authority distribution, Tuning Knob 7 synergy scope boundary); design-review revisions applied 2026-05-28 (formula corrections, pool rebalance, rotation rule resolution)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-28
> **Implements Pillar**: The Build is Never Finished (primary); Combos Must Be Discoverable (secondary); Agency Over Dice (supporting)

## Overview

The Item System is the third axis of build construction in S-Layers. Every player begins with a board of equipment layers and a routing strategy — items are the modifiers that arrive mid-session and rewrite what those layers do. An item attaches to any equipment zone the player chooses, sits there for as long as the host layer remains active, and departs the moment that layer is stripped. While it is live, it changes the zone's constraint, effect, output, or rules in the way its text specifies.

Items enter play through two channels: **treasure events** in the Event deck and **post-combat rewards** following a defeated Angel. The party distributes acquired items at the moment they are earned; each item immediately attaches to a chosen zone on a chosen player's board, with one item per zone as a hard limit. The Equipment System owns the attachment anchor and the one-item cap; this GDD owns acquisition, targeting, modifier syntax, and stacking rules.

The Item System does not own what happens to items when a layer is stripped — that contract is established by Equipment Degradation (item departs with the layer) and is referenced here, not redefined. It also does not modify the placed die's raw face value `[N]` — that is a Dice Economy rule — nor does it own the layer structure, placement rules, or degradation reveal sequence. What it owns is the input side: getting an item into play, choosing where it goes, specifying how it modifies an active zone, and defining what happens when two items interact with the same zone across layers.

The player fantasy is the broken build assembling itself mid-dungeon: a second-layer reveal that asks for `[N≥5]`, combined with an item that feeds those high dice into a burst threshold that wasn't worth building toward before. You did not plan this at session start. You built toward it one hit, one treasure, one routing decision at a time. The item does not change what you have — it changes what the thing you have is *for*.

## Player Fantasy

You are climbing through Heaven's machinery and you keep finding *things*.

A tooth ripped from a creature that didn't belong here. A marker made from charcoal and certainty. A thorn from a plant that grows only where something was executed. You do not know what most of them mean. You bring them because leaving them behind is a second kind of erasure — something was here, it left a mark, and carrying the mark is how you witness it. When you slide one into the anchor on your armor, you are not upgrading. You are installing a testimony.

The attachment is the act of misuse. Heaven designed that slot for a die. You are also putting a relic of unclear provenance in there, next to the die, and watching the layer change its mind about what it's for. The layer does not fight it. The layer is Heaven's equipment — built to be operated — and it doesn't know who is operating it or what they're running through its circuits. The finding goes in. You call the zone aloud — the choice is the announcement. The rest of the table has not finished reading what you just said when you are already nodding.

The moment of recognition is the moment you wanted.

Not when the synergy fires — though that matters too. The moment is when you name the zone and the table starts decoding it in real time: someone says *"wait — if that goes there while you still have the second layer—"* and you are already nodding before they finish. You found something. You brought it. You named the zone out loud and let the table catch up. The announcement is the move. The broken build is the evidence.

And then the layer goes. You absorb the hit because absorbing this specific hit was the correct move three turns ago when you decided it would be. The finding goes with the card. It is gone. It did its job — which was to make that layer into something Heaven didn't design. The receipt is face-down in the discard pile. The next layer comes up and the anchor is empty and you are already looking at the new card, figuring out which finding goes here next.

The build is never finished. You are still reading the evidence.

## Detailed Design

### Core Rules

---

#### 1. Item Card Anatomy

An item card (standard 63×88mm) contains the following elements:

| Element | Description |
|---|---|
| **Item Name** | A proper name identifying this item. Each item is unique (no duplicates in the 10-card MVP pool). |
| **Modifier Type** | One of four types: Constraint, Amplification, Threshold, or Mutation. Stated explicitly on the card. |
| **Modifier Text** | The rule this item imposes on its host zone while attached. **Hard cap: 12 words.** Uses the standard modifier syntax defined in Rule 5. |
| **Trigger Clause** | Optional. A condition under which the item fires an additional effect (see Rule 6). Printed below the modifier text, separated by a dividing line. |
| **Anchor Target Reminder** | Small icon or text confirming which constraint indicator types this item interacts with most effectively. Design guidance — not a restriction. Items may attach to any zone. |

---

#### 2. Acquisition

Items enter play through exactly two channels:

**Channel A — Treasure Events (Event Deck)**

When a player draws a treasure event card from the Event deck, they read the card aloud, then draw one item from the face-down item pool. The drawn item is revealed to all players. The Event deck contains 3 treasure events at MVP, shuffled freely.

**Channel B — Post-Combat Reward**

When the party defeats an Angel (reduces its Integrity to zero), one item is drawn from the item pool immediately following the conclusion of the combat phase. This draw is guaranteed — it is not a probability, not a conditional, and not skippable. One item per Angel defeat, regardless of party size.

Both channels draw from the same shared item pool (face-down; unordered). The pool contains 10 items at MVP.

**Exhausted pool**: If the item pool has no cards remaining, acquisition attempts produce nothing. No replacement mechanism exists at MVP.

---

#### 3. Distribution Procedure

When an item enters play (from either channel), the following procedure resolves immediately — before any other game action:

1. The **First Player** (the player currently holding the First Player token) may briefly ask the party: *"Does anyone need this?"* The party may respond in a few words — no formal vote, no veto. This discussion is advisory only and lasts no more than a few seconds.
2. The **First Player** announces aloud which player receives the item. This decision is **unilateral** — no player may call for a vote or override it. Other players may suggest; the First Player's word is final on *who receives it*.
3. The **receiving player** announces which of their eligible zones the item attaches to, per Rule 4. This is **their decision alone** — no one may override the receiving player's zone choice. They choose immediately.
4. If the receiving player has no eligible zone (all zones inert or all zones already occupied by items), the item is discarded immediately without attaching. No departure trigger fires.
5. If the First Player's own board already has items on all three zones, they must announce a different player as the recipient. They may not hold an item unassigned or give it to a player with no eligible zones.

**Why split authority**: The First Player reads the party's board state to identify who benefits most from the item (cooperative board-read beat; Pillar 4). The receiving player reads their own board to choose the best zone (individual agency — the "findings" contract; Pillar 2). Together these decisions require two players to understand the current board without committee-style deliberation (Pillar 3). The First Player token rotates after each event card resolves (per Events System Rule 7), ensuring assignment authority distributes across the party over the session.

**Consecutive self-assignment rule**: The First Player may not self-assign the current item if they received the most recently distributed item this session. If they would be the optimal recipient again, they must offer to another eligible player first; if no other player has an eligible zone, the self-assignment proceeds normally.

---

#### 4. Attachment Rules

**4.1 — Eligibility**

An item may only be attached to an **active zone** — a zone with at least one layer remaining (i.e., a face-up equipment card occupying the slot). Items may not be attached to:
- Inert zones (0 layers — no card in the slot, no anchor visible)
- Zones that already have an item attached (one item per zone maximum)

**4.2 — Targeting**

Items may be attached to any zone on any player's board, subject to eligibility. There is no slot-type restriction — a damage-amplification item may go on a Head zone, a Body zone, or a Weapon zone. **Who receives the item** is the First Player's call; **which zone it attaches to** is the receiving player's call (Rule 3).

**4.3 — One Item Per Zone**

Each zone has exactly one Item Anchor Zone (Equipment System Rule 1). A second item cannot attach to a zone that already has an item. This limit is physical — the anchor is full. The Equipment System owns the anchor; this GDD owns the attachment rule.

**4.4 — Permanence**

Once attached, an item stays until:
- Its host layer is stripped (item departs with the layer — Equipment Degradation Rule 1)
- The session ends (item is discarded; no cross-session persistence)

Items may not be voluntarily detached. Placement is a commitment. This is the "findings" contract: you installed a testimony; you operate under it.

**4.5 — Maximum Items Per Board**

A player may have at most **3 items attached simultaneously** — one per zone. This is the structural cap, not a rule that needs tracking: three zones × one item per zone = three items maximum.

---

#### 5. Modifier Types and Syntax

All four modifier types use standard syntax. Card authors must use only the forms below. New modifier forms require approval before use in card production.

---

**Type 1 — Constraint Modification**

An item of this type overwrites the zone's activation constraint while attached. The item text prints the complete new constraint — the base card's constraint is suspended.

*Syntax*: `This zone activates on [N≥X].`
- Replaces the zone's printed constraint entirely. No arithmetic at the table — the item states the full new threshold.
- Applies from the moment of attachment; takes effect at the start of the next Placement Phase.
- Does not apply retroactively to the current phase.

*Example*: An item reading `This zone activates on [N≥3].` attached to a zone printed `[N≥5]` means the zone fires on any die showing 3 or higher for the rest of the session (or until the layer strips).

*Interaction with Accumulate zones*: If the zone has both an Accumulate keyword and an additional constraint (e.g., `Accumulate [N≥5]`), the item replaces the `[N≥5]` part only. The Accumulate mechanic itself (banking totals, firing on threshold) is unchanged. The item modifies what gates the accumulation, not the accumulation itself.

---

**Type 2 — Output Amplification**

An item of this type adds a flat bonus to the resolved output of the host zone's effect, applied after the base effect formula resolves.

*Syntax*: `This zone's [output type] +[k].`
- Applies after the base effect formula completes — never before.
- The item does not modify `N` (the placed die's raw face value). It modifies the output of the effect that used `N`.
- Additive only at MVP. Multiplicative amplification (`×k`) is not permitted on items — it remains a property of equipment cards only.

*Resolution order*: Die placed → constraint check → base effect resolves → item modifier adds flat bonus to result.

*Example*: A zone printed `Deal [N]×2 damage` resolves: N=5 → 5×2 = 10 damage from base. An amplification item `This zone's damage +2` adds 2 after the base resolves. Total output: 12 damage. The item does not make the zone `Deal [N]×2 +2 damage` in a single formula — it is an additive step after resolution, always.

*E_item contribution*: Items of this type add a flat value to the zone's E_slot. An item with `+k` on a zone with p_c activation probability adds `p_c × k` to effective E_slot per turn. This must be included in the combined S_encounter calculation (Rule 8).

---

**Type 3 — Accumulate Threshold Reduction**

An item of this type lowers the threshold at which an Accumulate zone fires.

*Syntax*: `Reduce threshold by [k] (cannot go below 4).`
- The floor (4) is printed on the item card. Players do not need to know the `accumulate_floor` registry constant — it is stated on the card.
- Applies from the moment of attachment; the reduced threshold is in effect from the current turn forward.
- Applies to the zone's current threshold as printed on the equipment card.
- If the zone's threshold is already at 4 and this item is attached: the item attaches legally, occupies the anchor zone, departs with the layer normally — but the modifier produces no change. The attachment was a suboptimal choice; it is not an error.

*Example*: A zone printed `Accumulate 12: Restore 1 Integrity` (Charge-tier, T_accum ≈ 5.3 turns) with an item `Reduce threshold by 4 (cannot go below 4)` fires at threshold 8 — Charge/Tick boundary at T_accum ≈ 3.5 turns. Near-doubling of firing frequency.

*Synergy design note*: This item type is specifically powerful on high-threshold (Ritual-tier, X = 16–20) zones, where a reduction of 4–6 crosses an entire tier. The discovery is in recognizing the zone's threshold tier and knowing which item crosses a meaningful threshold.

---

**Type 4 — Effect Type Mutation**

An item of this type changes the output type of the host zone's effect. The effect grammar (formula, constraint, `N` usage) is unchanged; only the declared output type noun changes.

*Permitted mutations at MVP*:

| From | To | Permitted? |
|---|---|---|
| Damage | Healing (Integrity restore) | Yes |
| Healing (Integrity restore) | Damage | Yes |
| Damage or Healing | Accumulate credit to another zone | Yes — treat as Type 3 interaction |

*Syntax*: `This zone deals [new output type] instead of [old output type].`
- The base formula grammar is unchanged — the number and notation come from the equipment card.
- The item provides only the output type substitution.

*Forbidden mutations at MVP (do not design, do not print)*:

| Mutation | Forbidden reason |
|---|---|
| Accumulate → immediate fire | 3–4× encounter value multiplier; breaks S_encounter band |
| Immediate → Accumulate | Requires Accumulate token tracking on a zone not designed for it |
| Passive (no dice) → die-accepting | Structural indicator type change; rules overhead too high |
| Die-accepting → passive | Same |
| Self → party (target expansion) | Violates Equipment System scoping rule (AC-34); breaks balance |
| Multiplicative amplification | Items add flat bonuses only; ×k stays on equipment cards |

Any card reaching production that violates this list is a design error and must be revised before print.

---

#### 6. Triggered Item Effects

Some items have a trigger clause in addition to (or instead of) a passive modifier. The trigger clause fires when its stated condition is met.

**6.1 — Trigger Formats**

Two trigger types are permitted at MVP:

| Trigger Type | Example Text | Timing |
|---|---|---|
| **Departure trigger** | `When this item is removed, deal 3 damage.` | Fires at step 3 of Equipment Degradation Rule 1 (item leaves the board), before the new layer is revealed. |
| **Zone-state trigger** | `When this zone's layer is stripped, deal 4 damage.` | Fires as part of the strip event — same timing as the departure trigger. In practice: identical timing for items on the stripped layer. |

**6.2 — Resolution Order for Triggers**

When the host layer strips:
1. Equipment Degradation Rule 1 begins (layer strip sequence)
2. Item departs (step 3 of Rule 1) → **departure / zone-state trigger fires here**, if present
3. New layer revealed (step 4 of Rule 1)
4. The trigger fires before the new layer is live. Triggered effects cannot reference the newly revealed layer's state.

**6.3 — Scoping**

Triggered effects must reference only game elements the triggering player can observe: damage to the Angel (shared), their own Integrity, their own zone states. Triggered effects may not reference another player's board state. Equipment System scoping rule (AC-34) applies to triggered item effects.

---

#### 7. Balance Constraint (`E_item`)

Items are not free additions. Every item attached to a zone contributes to that player's effective S_encounter. The combined S_encounter — base set output plus all attached items — must remain within the [54, 74] band.

**For Type 2 items (amplification)**: `E_item = p_c × k_bonus × T_active`
- `p_c` = activation probability of the host zone (from E_slot lookup table in Equipment System)
- `k_bonus` = the flat bonus value (`+k` in the item text)
- `T_active` = expected turns this item is live ≈ T_encounter / (H_pt × N_layers_per_zone) = 8 / (1.0 × 3) ≈ **2.67 turns** at MVP. Do not use the shorthand "1/H_pt" — that formula gives per-player hits, not per-zone active time.

**For Type 3 items (threshold reduction)**: Compute new T_accum at the reduced threshold; the encounter output increase is approximately:

`ΔS ≈ E_accumulate_effect × (T_encounter / T_accum_new − T_encounter / T_accum_original)`

This is a design-time calculation, not a table check. Card designers must verify combined S_encounter before any item goes to print.

**Balance gate**: No item shall be approved for production if it pushes any single player's S_combined (base set + all attached items at maximum reasonable attachment) above 85 advisory, or above 110 hard gate (recalibrated against T_active ≈ 2.67). When evaluating multi-item scenarios, Type 1 + Type 2 same-zone combinations must be computed together — p_c changes from a Type 1 item multiply the Type 2 item's contribution. See Formula 3 for the interaction rule.

---

#### 8. Forbidden Modifier Forms (Production Gate)

The following modifier forms may not appear on any item card at MVP. Violations are a hard production block:

1. Multiplicative amplification (`×k` on an item)
2. Accumulate-to-immediate trigger conversion
3. Immediate-to-Accumulate conversion
4. Any effect that changes the constraint indicator type (Single Die ↔ Multi Die ↔ Passive)
5. Any effect that changes the target from the current player's board to another player's board or to all players
6. Any effect that modifies `N` (the raw placed-die face value) before it is read by the equipment card's formula

---

### States and Transitions

**Item lifecycle states:**

| State | Condition | Notes |
|---|---|---|
| **Unacquired** | In the face-down item pool | Not in play; no effect on any board |
| **Revealed** | Drawn from pool; Distribution Procedure active | Visible to all; First Player resolving destination |
| **Attached** | In an Item Anchor Zone on an active layer | Modifier is live; affects host zone per its text |
| **Departed** | Host layer stripped; item removed from board | Departure trigger fires (if any); item goes to discard |
| **Discarded** | In the item discard pile | No further effect; cannot re-enter play |

**Lifecycle state machine:**

```
Unacquired
  ↓ Treasure event drawn OR Angel defeated
  → Revealed

Revealed
  ↓ First Player announces recipient; receiving player chooses zone
  → Attached (item placed in receiving player's chosen anchor zone)

Attached
  ↓ Host layer is stripped (any cause: hit, self-strip, card effect)
  → Departed (item leaves board per Degradation Rule 1 step 3)

Attached
  ↓ Session ends (party wins or loses)
  → Discarded directly (departure trigger does NOT fire on session-end removal)

Departed
  ↓ Departure trigger resolved (if present)
  → Discarded (enters item discard pile)

Discarded
  → [Final state — no re-entry]
```

**Session-end departure note**: When the session ends with items still attached, those items go directly to discard. Departure triggers are in-play strip triggers only — they do not fire on session-end cleanup.

---

### Interactions with Other Systems

| System | Direction | Interface |
|---|---|---|
| **Equipment System** | Upstream | Item Anchor Zone (one per zone; items attach here); one-item-per-zone hard limit; constraint indicator types (items do not change these); effect output grammar (items modify output values, never `N`); AC-34 scoping rule (effects reference current player's board only) |
| **Equipment Degradation** | Upstream | Layer Reveal Procedure Rule 1 step 3 (item departs when its host layer strips); departure trigger timing (fires at step 3, before new layer reveal); items do not return with restored layers; if item card text says "when this item is removed, [effect]," that effect fires at step 3 |
| **Dice Economy** | Upstream | `[N]` notation; `accumulate_floor` = 4 and `accumulate_ceiling` = 20 (items cannot exceed these bounds when modifying thresholds); rule that `N` is never modified by items before it is read by the effect formula |
| **Events System** | Upstream | Treasure event cards trigger item draw from the pool (3 treasure events at MVP, shuffled freely). Events System owns the event card structure; Item System owns what happens when a treasure event resolves: draw 1 item → Distribution Procedure |
| **Combat System** | Downstream | Post-combat item reward: Item System grants 1 item per Angel defeat, immediately after combat concludes. Combat System owns the "combat concluded" event trigger; Item System owns the draw and Distribution Procedure that follows |
| **Character System** | Soft | Items may create specific synergies with character layer ordering (e.g., a Type 3 item on the Pilgrim's Reservoir zone is a high-value interaction). No rules interface — synergy is a design-time concern, not a runtime rule |

## Formulas

> *Note: Item System formulas are design-time checks — they are not computed at the table. Card designers use them to verify that items are balanced before print. No formula in this section requires player calculation during play.*

---

### Formula 1 — Item Output Contribution (`E_item`)

The `E_item` formula quantifies the per-turn output contribution of a Type 2 (Output Amplification) item attached to a zone.

`E_item = p_c × k_bonus × T_active`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Host zone activation probability | p_c | float | (0.0, 1.0] | Probability the placed die meets the host zone's constraint. From E_slot lookup table (Equipment System Formula 1). Use the modified constraint if a Type 1 item is also present. |
| Item bonus value | k_bonus | int | [1, 6] | The flat value `k` in the item text (`+k`). Additive only — no multiplicative items at MVP. |
| Turns item is live | T_active | float | [0.5, T_encounter] | Expected turns this item remains attached. Derived as T_encounter / (H_pt × N_layers_per_zone). At MVP (H_pt = 1.0, N_layers = 3, T_encounter = 8): T_active ≈ 8 / (1.0 × 3) ≈ **2.67 turns per layer**. Use this value for design-time calculations unless the host layer's specific active duration (from Equipment Degradation Formula 2) is available. |
| Item output contribution | E_item | float | [0, ~16] | Expected encounter output contribution from this item = p_c × k_bonus × T_active. This is the encounter-level contribution, not a per-turn rate. |

**Output Range:** 0 (item never activates or T_active = 0) to approximately 16 at `k_bonus = 6, p_c = 1.0, T_active = 2.67`.

**Note on formula structure:** E_item is the complete encounter-level contribution (one multiplication by T_active). There is no separate "encounter contribution" step requiring a second T_active multiplication — that was a formula error in prior versions. Rate per turn while active = p_c × k_bonus; multiply once by T_active to get encounter contribution.

At MVP (T_active ≈ 2.67 turns per layer): an amplification item contributes approximately `p_c × k_bonus × 2.67` encounter output units.

**Example — "+2 damage" item on a `[N≥4]` damage zone (mid-layer)**:
- p_c = 0.75, k_bonus = 2, T_active = 2.67 turns
- Rate = 0.75 × 2 = 1.5 damage/turn while active
- E_item = 0.75 × 2 × 2.67 = **4.0 units** added to S_encounter

**Example — "+3 damage" item on an unconstrained zone (T_active ≈ 2.67)**:
- p_c = 1.0, k_bonus = 3, T_active = 2.67
- E_item = 1.0 × 3 × 2.67 = **8.0 units**

---

### Formula 2 — Threshold Reduction Output Delta (`ΔS_threshold`)

The `ΔS_threshold` formula quantifies the encounter output change when a Type 3 (Accumulate Threshold Reduction) item is applied to an Accumulate zone.

`ΔS_threshold = E_effect × (T_encounter / T_accum_new − T_encounter / T_accum_base)`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Accumulate effect value | E_effect | float | [0, ∞) | The value the effect delivers when the threshold is crossed. For heal or damage: the fixed number on the card. |
| Encounter length | T_encounter | float | [6, 10] | Expected turns per encounter. Use 8 for MVP. |
| Original trigger time | T_accum_base | float | (0, ∞) | Expected turns to fire at the unmodified threshold. From T_accum formula: `X_base / 2.275`. |
| New trigger time | T_accum_new | float | (0, ∞) | Expected turns to fire at the reduced threshold. From T_accum formula: `max(X_base − k, 4) / 2.275`. |
| Output change | ΔS_threshold | float | [0, ~12] | Total additional encounter output provided by the threshold reduction, compared to the unmodified zone. |

**Output Range:** 0 (threshold already at floor) to approximately 12 (Ritual-tier zone reduced by 6 with a high-value effect).

**Example — threshold reduction item (k=4) on `Accumulate 12: Restore 1 Integrity` (Charge tier)**:
- E_effect = 1, T_encounter = 8
- T_accum_base = 12 / 2.275 ≈ 5.27 turns → fires 8/5.27 ≈ 1.52 times per encounter
- T_accum_new = 8 / 2.275 ≈ 3.52 turns → fires 8/3.52 ≈ 2.27 times per encounter
- ΔS_threshold = 1 × (2.27 − 1.52) = **0.75 units**

**Example — threshold reduction item (k=6) on `Accumulate 18: Deal 5 damage` (Ritual tier)**:
- E_effect = 5, T_encounter = 8
- T_accum_base = 18 / 2.275 ≈ 7.91 turns → fires ≈ 1.01 times per encounter
- T_accum_new = 12 / 2.275 ≈ 5.27 turns → fires ≈ 1.52 times per encounter
- ΔS_threshold = 5 × (1.52 − 1.01) = **2.55 units**

*Larger base thresholds yield more ΔS from the same reduction — Type 3 items are most impactful on Ritual-tier zones.*

---

### Formula 3 — Combined Set Balance Check (`S_combined`)

The `S_combined` formula verifies that attaching items to a player's equipment set does not push combined encounter output outside the balance band.

`S_combined = S_encounter_base + Σ E_item_encounter(i) + Σ ΔS_threshold(j)`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Base set encounter output | S_encounter_base | float | [54, 74] | S_encounter of the equipment set without any items (Equipment System Formula 3). |
| Amplification item contributions | Σ E_item(i) | float | [0, ~48] | Sum of encounter contributions from all Type 2 items attached (Formula 1). At 3 items with max values (p_c=1.0, k_bonus=6, T_active=2.67): max ≈ 3 × 16 = 48. Typical session values are substantially lower. |
| Threshold reduction deltas | Σ ΔS_threshold(j) | float | [0, ~8] | Sum of output deltas from all Type 3 items attached (Formula 2). Practical max at realistic card values (E_effect ≤ 5) is ~4 units per item. |
| Type 1 note | (implicit) | — | — | Constraint modification changes p_c — re-run E_slot with the modified constraint and recompute S_encounter_base. **Important:** if a Type 2 item is also attached to the same zone as a Type 1 item, re-run Formula 1 for the Type 2 item using the modified p_c. A Type 1 item that raises p_c from 0.33 to 1.0 multiplies the Type 2 item's E_item by ~3×. This interaction must be checked explicitly. |
| Type 4 note | (implicit) | — | — | Mutation does not change numeric value — only output type. No formula adjustment required for S_combined. Check D_agg separately (see Edge Cases, Type 4). |
| Restoration item note | (implicit) | — | — | Restoration trigger items (departure trigger: restore k Integrity) contribute 0 to S_combined. Track expected session restoration separately: E_restore ≈ P_strip × k_restore. |
| Combined encounter output | S_combined | float | — | Total expected encounter output with all attached items. |

**Balance gates:**

| Gate | Threshold | Status |
|---|---|---|
| Lower bound | S_combined ≥ 54 | Items should not reduce output. Net-negative items are misdesigns. |
| Advisory | S_combined ≤ 85 | Any single player's combined output above 85 triggers design review. |
| Hard production gate | S_combined ≤ 110 | Above 110: item combination is degenerate. Redesign the item or remove from pool. Recalibrated from 105 against corrected T_active ≈ 2.67. |

**Type 1 + Type 2 same-zone example** (the highest-risk two-item interaction):
- Zone with p_c_base = 0.33 ([N≥5] constraint), Type 1 item raises to p_c = 1.0, Type 2 item has k_bonus = 4
- Without Type 1: E_item(Type 2) = 0.33 × 4 × 2.67 = 3.5 units
- With Type 1 on same zone: E_item(Type 2) = 1.00 × 4 × 2.67 = 10.7 units — a 3× multiplier
- Both items must be evaluated together; Formula 1 alone on the Type 2 item gives the wrong result if p_c has changed

**Example — Pilgrim starting set (S_encounter_base ≈ 65) with two items attached**:
- Item 1 (Type 2, +2 damage on Weapon, p_c = 1.0, T_active ≈ 2.67): E_item = 1.0 × 2 × 2.67 = 5.3 units
- Item 2 (Type 3, threshold −4 on Accumulate 8 Heal zone, ΔS ≈ 0.3 units)
- S_combined = 65 + 5.3 + 0.3 = **70.6** — within advisory range ✓

---

### Formula 4 — MVP Item Pool Composition (Design Calibration)

Not a runtime formula. A design allocation tool confirming the 10-item pool delivers meaningful choices across modifier types.

**Revised MVP pool allocation (11 items):**

| Item Type | Count | Rationale |
|---|---|---|
| Type 1 — Constraint Modification | 3 | Broadly applicable; reduced from 4 to limit type glut and make room for Type 4 and Restoration entries |
| Type 2 — Output Amplification | 3 | Most valuable when the player knows their build direction; arrives mid-session |
| Type 3 — Threshold Reduction | 2 | High-synergy with Accumulate zones; rarity prevents runaway threshold stacking |
| Type 4 — Effect Type Mutation | 2 | The broken-build item; 2 copies with different mutation targets (e.g., Damage→Healing and Damage→Accumulate-credit); session appearance rate ~62% at 4 draws from 11-item pool |
| Restoration — Departure Trigger Heal | 1 | Addresses recovery deficit (B-REST-1). Text: "When this item is removed, restore 2 Integrity." Departure trigger per Rule 6 — fires at layer strip, before new layer reveal. Does not count toward S_combined; provides ~2 Integrity resilience per strip event. |
| **Total** | **11** | — |

**Restoration item balance note**: The two Type 4 items must be distinct — identical mutation targets produce duplicate plays. Recommended pair: one mutates Damage→Integrity restore; one mutates Damage→Accumulate credit (feeding a target zone's running total). The restoration departure-trigger item and the Type 4 Damage→Healing mutation are two different healing vectors; having both ensures at least one healing path in most sessions.

**Expected session distribution**: 4 items drawn on average (3 treasure events + 1 post-combat reward). At 4 draws from a shuffled 11-item pool (hypergeometric): ~1.1 Type 1, ~1.1 Type 2, ~0.7 Type 3, ~0.7 Type 4, ~0.4 Restoration per session. At least one Type 4 appears in ~62% of sessions (up from ~40% at 1/10). Restoration item appears in ~36% of sessions.

**Tuning lever**: See Tuning Knob 4. More Type 1 → more accessible game. More Type 4 → broken-build moments more common. More Restoration → more survivability; lowers tension of the loss-of-resources arc.

## Edge Cases

### Acquisition and Distribution

**If the item pool is empty when a treasure event or post-combat reward triggers acquisition**: no item is drawn. The acquisition event resolves with no output. The party continues without an item from that trigger. No replacement or rain-check mechanism exists at MVP.

**If an item is drawn but no eligible anchor zone exists on any player board** (all zones either inert or already occupied by items): the item is discarded immediately without attaching. The Distribution Procedure ends with no placement. This is distinct from pool exhaustion — an item was revealed and goes face-up, but finds no legal home. It enters the discard pile without triggering a departure event (it never attached).

**If both a treasure event (Channel A) and a post-combat reward (Channel B) trigger in the same moment when exactly one item remains in the pool**: Channel B (post-combat reward) always draws first. That draw receives the item and the Distribution Procedure executes normally. Channel A's draw finds an empty pool and produces nothing. Channel B takes priority because the post-combat reward is a guaranteed contract; treasure events are opportunistic. No discussion or First Player decision is required.

**If the First Player's board has items on all three zones** (no eligible zones for themselves): they must assign the drawn item to another player's eligible zone. They may not hold the item unassigned or pass to a player who also has all zones occupied. If no other player has an eligible zone, the item is discarded (per the "no eligible anchor" rule above).

---

### Type 1 — Constraint Modification Edge Cases

**If a Type 1 item attaches to a zone with no printed additional constraint** (e.g., an Accumulate zone that accepts all dice): the item text applies as written. The zone now has the item's stated constraint (`[N≥X]`) applied to it. This makes the zone *more* restrictive than its base — dice that previously accumulated freely are now gated. The attachment is legal. The First Player has inadvertently reduced the zone's activation rate. This is a valid but suboptimal placement; no rule violation occurs.

**If a Type 1 item attaches to a passive zone** (no constraint indicator, no dice): the attachment is legal. The item's constraint modifier is live but permanently inert — passive zones never check a die-value constraint because no die is ever placed there. The item occupies the anchor, departs with the layer normally, and its departure trigger (if any) fires at strip time. Its modifier text produces no gameplay effect for the duration of its attachment.

**If a Type 1 item modifies the constraint and a die is already committed to that zone** (placed during this Placement Phase but not yet resolved): the Type 1 item's constraint change applies starting from the next Placement Phase (per Rule 5, Type 1). The die placed this turn is checked against the constraint that was live when the die was placed, not against the new item constraint. The item takes effect from the next turn.

**If a Type 1 item is attached mid-session to an Accumulate zone that already has a running total**: the running total is unchanged. No dice are retroactively re-evaluated. From the next Placement Phase, accumulation is gated by the new constraint. Previous turns' contributions stand.

---

### Type 2 — Output Amplification Edge Cases

**If a Type 2 item is attached to a one-shot (self-stripping) zone** (e.g., `Deal [N]×2, then discard this layer`): the item bonus applies to the effect that fires on activation. Resolution order: die placed → constraint check → base effect resolves (`[N]×2` damage) → item modifier adds `+k` to the result. The total output fires, then the self-strip executes. The item departs with the stripped layer. For S_combined calculation: T_active = 1 activation (not `1/H_pt`), because the self-strip removes the layer after the first activation regardless of hit rate.

**If a Type 2 item is attached to a passive zone** (no dice, on-destroy trigger): Ruling A applies — the flat bonus applies to the zone's effect output when the passive trigger fires. A passive zone that reads `When this layer is destroyed, deal 6 damage` with a `+2 damage` item attached fires 8 damage at strip time. For S_combined calculation: the item's passive contribution is `E_passive_item = P_strip × k_bonus`, where `P_strip` is the probability this layer is stripped in a given encounter (1.0 for top layers under MVP play, decreasing for deeper layers per S_layer formula). Add this term to S_combined when a Type 2 item is on a passive zone.

**If two Type 2 items from different zones combine their outputs**: each item affects only its host zone. There is no cross-zone pooling of bonuses. Both items contribute their `E_item_encounter` values to S_combined independently.

---

### Type 3 — Threshold Reduction Edge Cases

**If a Type 3 item would reduce a threshold below 4** (the `accumulate_floor`): the threshold is treated as 4. The item card prints "cannot go below 4" to avoid requiring players to know the registry constant. The item attaches legally; the modifier produces no change if the threshold is already at or below 4.

**If a Type 3 item is attached and the reduction brings the threshold below the existing running total**: the effect does not fire at the moment of attachment. Thresholds are checked during Effect Resolution Phase only, when a die is placed and evaluated. The player must place another die on the next turn to trigger threshold evaluation, even if the existing total already exceeds the new lower threshold.

> *Playtest note: validate in session 1. Players may expect the Accumulate effect to fire immediately on item attachment. The rule is clear but may feel counterintuitive.*

**If a Type 3 item attaches to a zone whose running total is already at the new threshold boundary**: the effect still does not fire at attachment. The check waits for the next die placement. This is consistent with the Equipment System rule that effects fire during Effect Resolution Phase, not during item attachment.

---

### Type 4 — Effect Type Mutation Edge Cases

**If a Type 4 item changes a damage zone to healing**: S_combined is unaffected (same numeric output). However, D_agg (expected damage per player per turn) is reduced by the zone's E_slot value. A party relying on the mutated zone for Angel combat damage may find Angel HP targets harder to reach. When a Type 4 mutation converts a damage zone to healing, verify that the party's D_agg across all players remains sufficient: `party_D_agg ≥ Angel_HP_layer / T_encounter`.

**If a Type 4 item mutates a passive zone** (on-destroy: `deal 6 damage` → `restore 6 Integrity`): the mutation is legal and coherent. The passive trigger fires healing instead of damage at strip time. The numeric value is unchanged. D_agg implications apply: the zone no longer contributes to Angel combat damage.

**If S_combined appears unchanged after a Type 4 mutation but D_agg drops**: this is expected and correct behavior. S_combined and D_agg measure different things. Do not adjust item values to compensate — check D_agg separately.

---

### Departure and Trigger Edge Cases

**If a departure trigger fires and a die was committed to the departing zone this turn**: the die fizzles per Equipment System Rule 5.6 (a die committed to a zone whose layer stripped mid-phase is consumed without effect). The departure trigger fires; the die does not resolve.

**If a departure trigger fires after the Angel is already at zero Integrity** (Angel defeated this turn): the damage applies to the combat resolution. Angel defeat is processed at end of combat, not at the moment each damage is applied. Departure trigger damage contributes to final totals but does not retroactively alter combat outcome.

**If an item with a departure trigger is removed at session end**: the departure trigger does **not** fire. Session-end removal is cleanup, not a strip event.

**If the same layer is stripped by two events in one turn** and an item is attached: the item departs at step 3 of the first strip event. The second strip event targets an already-inert zone — the item has already departed. The departure trigger fires once, at the first strip only.

**If a departure trigger fires an offensive effect (damage to the Angel) when no Angel is present in the Angel Zone** (e.g., during a Bad Event, or at session end — see Rule 6.1 for context): the trigger fires and is consumed — the departure is complete and the item is removed — but the offensive damage fizzles. No damage is applied, no integrity is lost, and no target is resolved. This is distinct from combat context, where a valid Angel target always exists. *(Rationale: departure triggers are written with combat context as the default. A trigger that fires outside combat has the same "fires unconditionally" timing but the damage effect has no legal target in a no-Angel context. The effect is discarded; the departure is not.)*

---

### Balance Formula Edge Cases

**If S_combined exceeds 85 only when all three items are optimally attached simultaneously**: the balance gate is a design constraint on the potential power ceiling, not an average-case estimate. Items that look balanced in isolation may push S_combined above 85 only in combination — verify multi-item scenarios explicitly during card design.

**If Type 4 mutation is applied and S_combined appears unchanged but D_agg drops**: expected behavior. Check both metrics separately. Do not over-correct S_combined when D_agg is the at-risk metric.

## Dependencies

**Upstream dependencies** (systems Item System depends on):

| System | Type | What Item System consumes |
|---|---|---|
| **Equipment System** | Hard | Item Anchor Zone (one per zone; departs with layer); one-item-per-zone hard limit; constraint indicator types (items do not change these); effect output grammar (items add to output values, never modify `N`); AC-34 scoping rule (effects reference current player's board only); slot capacity rules (items may not modify constraint indicator type at MVP) |
| **Equipment Degradation** | Hard | Layer Reveal Procedure Rule 1 step 3 (item departs when its host layer strips, immediately, before the new layer is revealed); departure trigger timing (fires at step 3); items do not return with restored layers (unless restoration effect explicitly states otherwise); item card text "when this item is removed" fires at step 3 |
| **Dice Economy** | Hard | `[N]` notation syntax; `accumulate_floor` = 4 and `accumulate_ceiling` = 20 (items cannot exceed these bounds when modifying thresholds); rule that `N` is never modified before it is read by the effect formula |
| **Events System** | Hard | Treasure event cards trigger item acquisition (Channel A). Events System owns the event card type, draw procedure, and deck structure; Item System owns what happens after a treasure event resolves: draw 1 item → Distribution Procedure. |
| **Integrity System** | Soft | Integrity Rule 12 (slot state locked at phase start) interacts with item modifiers on Accumulate zones that change mid-phase — the die still fires using the constraint and modifier live at the start of Effect Resolution Phase. |

**Downstream dependents** (systems that depend on Item System):

| System | Type | What they consume |
|---|---|---|
| **Combat System** | Hard | Post-combat item reward: Combat System owns the "Angel defeated" event; Item System owns the draw and Distribution Procedure that follows. Combat System must acknowledge that item attachment may occur immediately after any combat phase conclusion, before the next phase begins. |
| **Events System** | Hard | Treasure event card resolution triggers Item System acquisition. The Events System must define that treasure events resolve by delegating to Item System's Rule 2 (Channel A acquisition). |

**Bidirectional consistency requirements:**
- **Equipment System GDD**: must confirm item anchor zone placement is on each zone's face; item departure confirmed in its Dependencies section ✓
- **Equipment Degradation GDD**: must confirm item departure fires at step 3 of Rule 1 before the new layer reveals ✓; must confirm items do not return with restored layers ✓
- **Events System GDD** (when authored): must define treasure event card type and its resolution hook calling Item System Channel A; must add Item System to its downstream dependents
- **Combat System GDD** (when authored): must define the post-combat item reward trigger (1 item per Angel defeat); must add Item System to its downstream dependents; must confirm Item System distribution resolves before the next combat phase

## Tuning Knobs

**1. Item Pool Size (total item card count)**
- **Adjusts**: Total items available in a session; how often items influence the build
- **Current value**: 11 items at MVP
- **Safe range**: 10–15
- **Too low (< 10)**: Pool exhausts before late encounters; post-Angel rewards produce nothing in 4-player sessions; restoration and Type 4 items disappear too quickly
- **Too high (> 15)**: Items feel less special; attachment decisions become bureaucratic
- **Interaction**: At 11 items and 4 expected draws, zone fill rate ≈ 67% in a 2-player game (4 items / 6 zones). Recalibrate if T_encounter or player count scaling changes.

**2. Treasure Events in the Event Deck (Channel A acquisition rate)**
- **Adjusts**: How often items enter play from treasure events; pacing of item-build evolution
- **Current value**: 3 at MVP (shuffled freely)
- **Safe range**: 2–5
- **Too low (< 2)**: Build-via-items rarely takes shape; item axis feels invisible
- **Too high (> 5)**: Analysis paralysis at distribution; pool exhausts early
- **Interaction**: Combined with Channel B (guaranteed post-combat), total expected items = treasure events + Angel defeats. At 3 + 1 = 4 expected per session.

**3. Post-Combat Reward Rate (Channel B)**
- **Adjusts**: Whether Angel defeats guarantee an item
- **Current value**: 1 guaranteed item per Angel defeat
- **Variants**: 0 (Channel A only), 0.5 (50% chance), 1 (guaranteed), 2 (two items per defeat)
- **At 0**: 3 expected items total; item axis lightly represented; faster play
- **At 2**: More items than zones in multi-Angel sessions; items become common rather than found
- **Do not change** without recalibrating total expected items and zone fill rate

**4. Item Pool Modifier Type Allocation (mix of types)**
- **Adjusts**: What kinds of items the party sees; gameplay feel without changing item count
- **Current value**: 3 constraint / 3 amplification / 2 threshold / 2 mutation / 1 restoration = 11 items (Formula 4)
- **More Type 1**: More accessible; more zones fire more often; reduces time spent with no useful attachment option
- **More Type 2**: Output spikes more common; requires careful S_combined monitoring (especially with Type 1 same-zone stacking)
- **More Type 3**: Accumulate synergies more frequent; rewards players who build toward Accumulate zones
- **More Type 4**: Broken-build moments more common; below 2 copies, reliability drops sharply (1 copy = ~40% session rate)
- **More Restoration**: More survivability; weakens the loss-of-resources pressure arc
- **Interaction**: Keep at least 1 of each type. Dropping any type to 0 removes that modifier category from the session experience. The Restoration item should always remain at ≥1 to maintain the recovery vector (B-REST-1).

**5. Maximum Items Per Player Board**
- **Current value**: 3 (structural — one per zone)
- **Recommended status**: Locked at MVP
- **If raised**: Requires a new multi-item-per-zone anchor model — a design overhaul, not a tuning change
- **If lowered (global cap < 3)**: Item competition is explicit. Viable as a post-MVP variant rule.

**6. S_combined Balance Gate Thresholds**
- **Adjusts**: How powerful combined item + set combinations are allowed to be
- **Current values**: Advisory 85; hard production gate 105
- **If raised**: Items stack to higher combined output; reduce Angel HP calibration to compensate
- **If lowered**: Items feel weaker relative to base equipment; less incentive to build around items
- **Interaction**: This gate only affects item design approval — it does not affect the base set balance band [54, 74] owned by Equipment System.

**7. Synergy Depth**
- **Adjusts**: How surprising and non-obvious the item synergies feel at the table
- **Current scope (MVP)**: Item + layer **TYPE** synergies — a threshold-reduction item is obviously powerful on an Accumulate zone; a constraint-modification item obviously broadens a high-gated zone. Connections are legible on first read. Targets **Pillar 2 (The Build is Never Finished)**.
- **Post-MVP target**: Item + specific layer **PASSIVE** synergies — non-obvious intersections that require players to read both a specific item card and a specific layer's passive text to recognize the combo. Players discover the synergy mid-play, not at setup. Targets **Pillar 5 (Combos Must Be Discoverable)**.
- **Why deferred**: With a 10-item pool and 2 characters, the MVP synergy surface is too small to reliably generate non-obvious intersections in playtests. The design bar for Pillar 5 is *surprising discovery*, not *visible connection* — deep synergy items require the full equipment layer catalog to design against.
- **Design gates for post-MVP upgrade**: IS-AC-12 defines the current bar (items broadly useful by type). Post-MVP upgrade requires new ACs specifying: (a) at least 3 items in the pool that are substantially stronger on 1 specific layer than on any other, and (b) the synergy is not visible from either card alone — it requires reading both.
- **CD-GDD-ALIGN Concern #3 note**: This scope boundary was accepted as a conscious Pillar 3 trade-off (MVP readability over depth). Flag for post-MVP review after the full equipment layer catalog is designed.

## Visual/Audio Requirements

*Physical board game — no digital audio or UI. Visual requirements for item cards will be specified by the Art Director during card production, in accordance with the Woodcut Witness visual identity. Item cards must use the same 63×88mm card stock and hard-edge woodcut style as equipment cards. The Item Anchor Zone on equipment cards must be specified in the art brief for each equipment layer.*

📌 **Asset Spec** — Once the art bible is approved, run `/asset-spec system:item-system` to produce per-card visual descriptions and art direction prompts for each of the 10 MVP item cards.

## UI Requirements

*Physical board game — no digital UI. Player-facing information elements:*
- *Item pool: face-down stack on the table, visible to all players*
- *Discard pile: face-up stack adjacent to pool*
- *Item attached to zone: physically placed in the Item Anchor Zone on the active layer card*
- *No HUD, no tracker, no digital display*

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Gate levels: BLOCKING = must pass before production; ADVISORY = flag for review; DEFERRED = blocked on cross-system data.*

---

### Acquisition and Distribution (Rules 2–3)

**IS-AC-01** *(ADVISORY)* — GIVEN an item pool with 10 items and a 15-card Event deck containing 3 treasure events (shuffled freely), WHEN the event deck is fully drawn, THEN at most 3 Channel A acquisition events have triggered; any deck state with fewer triggers due to pool exhaustion is also valid.

**IS-AC-02** *(BLOCKING)* — GIVEN a treasure event is drawn from the Event deck, WHEN the acquisition procedure begins, THEN the First Player draws one item from the face-down item pool, announces which player receives it, and the receiving player announces their chosen zone — all before any other game action resolves.

**IS-AC-03** *(BLOCKING)* — GIVEN an Angel is defeated (Integrity reaches zero), WHEN the combat phase concludes, THEN exactly one item is drawn from the item pool and the Distribution Procedure executes before the next game phase begins. This draw is guaranteed — it is not skippable.

**IS-AC-04** *(BLOCKING)* — GIVEN the item pool is empty, WHEN a treasure event is drawn OR an Angel is defeated, THEN no item is drawn; the acquisition event resolves with no output; the party proceeds without an item from that trigger.

**IS-AC-05a** *(BLOCKING)* — GIVEN all zones on all player boards are inert (all layers stripped), WHEN an item is drawn and the Distribution Procedure begins, THEN the item is discarded immediately without attaching; no departure trigger fires; the item enters the discard pile as if it were never placed.

**IS-AC-05b** *(BLOCKING)* — GIVEN all zones on all player boards are occupied by items (each player has 3 items attached, all zones filled), WHEN an item is drawn, THEN the item is discarded immediately without attaching; no departure trigger fires.

**IS-AC-06** *(BLOCKING)* — GIVEN the First Player's board has items on all three zones (no eligible anchor zones), WHEN an item is acquired, THEN the First Player must announce another player as the recipient; they may not keep the item unassigned or nominate themselves. The announced player then chooses their zone. If no other player has an eligible zone, IS-AC-05a or IS-AC-05b applies.

**IS-AC-07** *(BLOCKING)* — GIVEN both a treasure event (Channel A) and an Angel defeat (Channel B) trigger simultaneously when exactly one item remains in the pool, WHEN the simultaneous acquisition resolves, THEN Channel B draws first (no announcement or decision required); Channel B receives the item via the normal Distribution Procedure; Channel A's draw produces no item.

---

### Attachment Rules (Rule 4)

**IS-AC-08** *(BLOCKING)* — GIVEN a zone has one item already attached, WHEN a second item is drawn and the receiving player attempts to choose that zone, THEN the choice is illegal; the zone is already at the one-item-per-zone limit; the receiving player must choose a different eligible zone on their board.

**IS-AC-09** *(BLOCKING)* — GIVEN a zone has zero layers remaining (inert), WHEN a player attempts to attach an item to it, THEN the attachment is illegal; inert zones have no anchor card visible and cannot receive items.

---

### Modifier Types — Type 1 (Constraint Modification)

**IS-AC-10** *(BLOCKING)* — GIVEN a Type 1 item is attached to a zone with constraint `[N≥5]`, WHEN the next Placement Phase begins, THEN dice placed on that zone are checked against the item's printed constraint (e.g., `[N≥3]`) rather than the original `[N≥5]`. The change does not affect dice placed during the Placement Phase in which the item was attached.

**IS-AC-11** *(ADVISORY)* — GIVEN a Type 1 item stating `[N≥4]` is attached to an Accumulate zone with no additional constraint (accepts all dice), WHEN the item takes effect, THEN the zone now gates accumulation at `[N≥4]`; dice showing 3 or lower no longer contribute to the running total. This is a legal but restrictive placement.

**IS-AC-12** *(BLOCKING)* — GIVEN a Type 1 item is attached to a passive zone (no constraint indicator), WHEN game turns advance while the item is attached, THEN the item's constraint modifier has no gameplay effect; the passive zone still fires on its stated trigger condition unchanged; the item occupies the anchor and departs normally at strip.

**IS-AC-13** *(BLOCKING)* — GIVEN a Type 1 item is attached mid-session to an Accumulate zone with a running total of 6, WHEN the item takes effect the following Placement Phase, THEN the running total remains 6; previous accumulation is not invalidated or re-evaluated against the new constraint.

---

### Modifier Types — Type 2 (Output Amplification)

**IS-AC-14** *(BLOCKING)* — GIVEN a Type 2 item reading `+2 damage` is attached to a zone with effect `Deal [N]×2 damage`, WHEN a die showing 5 is placed and resolves, THEN the total output is (5×2)+2 = 12 damage. The item's bonus is added after the base formula, not before. The item does not modify `N` and does not change the base multiplier.

**IS-AC-15** *(BLOCKING)* — GIVEN a Type 2 item reading `+3 damage` is attached to a passive zone (effect: `When this layer is destroyed, deal 6 damage`), WHEN that layer is stripped, THEN the passive fires 6+3=9 damage at strip time (Ruling A). The bonus applies to passive trigger output as it would to die-placement-triggered output.

**IS-AC-16** *(BLOCKING)* — GIVEN a Type 2 item is attached to a one-shot self-stripping zone (e.g., `Deal [N]×2, then discard this layer`), WHEN a die is placed and the effect resolves, THEN: (a) the base damage fires, (b) the item bonus is added to the total output, (c) the self-strip fires as the last step. The item's bonus fires before the self-strip.

---

### Modifier Types — Type 3 (Threshold Reduction)

**IS-AC-17** *(BLOCKING)* — GIVEN a Type 3 item reading `Reduce threshold by 4 (cannot go below 4)` is attached to an Accumulate zone with threshold 12 and running total 5, WHEN the item attaches, THEN the zone's active threshold immediately becomes 8; the running total remains 5; the Accumulate effect does not fire at attachment (threshold is checked during Effect Resolution Phase only).

**IS-AC-18** *(BLOCKING)* — GIVEN a Type 3 item is attached and the zone's running total is already 10 with a new threshold of 8, WHEN the item finishes attaching, THEN the Accumulate effect does not fire immediately; it fires only when the next die is placed and the Effect Resolution Phase evaluates the total against the new threshold.
> *Playtest note: validate in session 1 — players may expect immediate firing. The rule is that thresholds are checked during Effect Resolution Phase, not at attachment time.*

**IS-AC-19** *(BLOCKING)* — GIVEN a Type 3 item is attached to an Accumulate zone whose threshold is already at 4 (the minimum), WHEN the modifier would reduce the threshold below 4, THEN the threshold remains at 4; the modifier has no effect; the item occupies the anchor normally and departs with the layer normally.

---

### Modifier Types — Type 4 (Effect Type Mutation)

**IS-AC-20** *(BLOCKING)* — GIVEN a Type 4 mutation item changes a zone from damage to healing, WHEN the zone's effect fires, THEN the output is healing equal to the numeric formula result (e.g., base effect `Deal [N] damage` with N=4 → `Restore 4 Integrity`). The numeric formula is unchanged; only the output type noun changes.

**IS-AC-21** *(DEFERRED — pending Combat System GDD)* — GIVEN a Type 4 mutation converts a damage zone to healing, WHEN the mutation is attached, THEN D_agg (expected party damage per turn) is reduced by the zone's E_slot value. Verify that `party_D_agg ≥ Angel_HP_layer / T_encounter` remains satisfied. **Blocked on:** `Angel_HP_layer` and the `D_agg` computation procedure are undefined until the Combat System GDD is authored. Reclassify to ADVISORY when those inputs are available.

**IS-AC-22** *(ADVISORY)* — GIVEN a Type 4 mutation item changes a zone to `Accumulate credit to [target zone]`, WHEN the zone fires, THEN the output is added to the target Accumulate zone's running total (not dealt as damage); the target zone's threshold check executes normally at its next Effect Resolution evaluation.

---

### Triggered Item Effects (Rule 6)

**IS-AC-23** *(BLOCKING)* — GIVEN an item with a departure trigger reading `When this item is removed, deal 3 damage`, WHEN the host layer is stripped, THEN the 3 damage fires at step 3 of the Layer Reveal Procedure — before the new layer card is revealed (step 4). The departure trigger cannot reference the newly revealed layer's state.

**IS-AC-24** *(BLOCKING — production gate)* — GIVEN a triggered item effect references another player's zone state (e.g., "if an ally has a damage zone, deal +2"), WHEN a card author or QA tester reviews it, THEN the card is flagged as an illegal design; triggered item effects are scoped to the current player's board only (per Equipment System AC-34). The card must be revised before production.

**IS-AC-25** *(BLOCKING)* — GIVEN an item with a departure trigger is still attached when the session ends (win or loss), WHEN session cleanup begins, THEN the departure trigger does not fire; the item is discarded without any trigger effect.

**IS-AC-26** *(BLOCKING)* — GIVEN a zone has two strip events targeting it in the same turn and the zone has one item attached, WHEN the first strip event resolves, THEN the item departs at step 3 of the first strip (departure trigger fires once); the second strip event finds the zone already inert and is wasted; the departure trigger does not fire a second time.

---

### Balance Constraint — Formula Criteria (Rule 7, Formulas 1–3)

**IS-AC-27** *(BLOCKING — design-time calculation required)* — GIVEN a Type 2 item with k_bonus=2 on a zone with p_c=0.75 (`[N≥4]` constraint) and T_active=2.67 turns (MVP value), WHEN E_item is computed using Formula 1, THEN the result = 0.75 × 2 × 2.67 = **4.0 encounter-output units** (±0.1 tolerance). *Test artifact: reproduce this calculation in the item design spreadsheet (column: E_item) before card production approval.*

*Note on 2.275 (Formula 2):* This constant is the mean accumulation-per-turn rate derived from the 2D6 dice distribution (Dice Economy). Source: Dice Economy GDD Formula 2. Card designers must verify this value against the Dice Economy GDD before computing ΔS_threshold.

**IS-AC-28** *(BLOCKING — design-time calculation required)* — GIVEN a Type 3 item (k=4) on an `Accumulate 12: Restore 1 Integrity` zone (E_effect=1, T_encounter=8), WHEN ΔS_threshold is computed using Formula 2 (2.275 from Dice Economy), THEN: T_accum_base = 12/2.275 ≈ 5.27; T_accum_new = 8/2.275 ≈ 3.52; ΔS_threshold = 1 × (8/3.52 − 8/5.27) ≈ **0.75 units** (±0.05 tolerance). *Test artifact: reproduce in item design spreadsheet.*

**IS-AC-29** *(BLOCKING — design-time calculation required)* — GIVEN S_encounter_base=65, E_item sum=4.0 (from IS-AC-27 corrected), ΔS_threshold sum=0.3, WHEN S_combined is computed using Formula 3, THEN S_combined = 65 + 4.0 + 0.3 = **69.3** — within the advisory range (≤85). Pass.

**IS-AC-30** *(BLOCKING — design gate)* — GIVEN a proposed item combination where S_combined = 91 for a player (S_encounter_base=74 + items=17), WHEN S_combined is evaluated at card design time using Formula 3, THEN the combination triggers an advisory design review flag; the item(s) must be reviewed before production approval. *Evaluate Type 1 + Type 2 same-zone combinations explicitly — the compounding p_c effect must be computed, not just individual item values.*

**IS-AC-31** *(BLOCKING — design gate)* — GIVEN a proposed item combination where S_combined = 112 for a player, WHEN S_combined is evaluated at card design time, THEN the combination is blocked from production (hard gate: S_combined > 110; recalibrated from 105 against corrected T_active ≈ 2.67); the item must be redesigned or removed from the pool.

---

### Production Gate Criteria (Rules 1 and 8)

**IS-AC-32** *(ADVISORY — production gate)* — GIVEN any item card entering the production pipeline, WHEN the modifier text word count is checked, THEN the modifier text does not exceed 12 words. Any item card exceeding 12 words is flagged for redesign.

**IS-AC-33** *(BLOCKING — production gate)* — GIVEN any item card entering the production pipeline, WHEN a reviewer checks the card for forbidden modifier forms, THEN none of the following appear: multiplicative amplification (`×k`), Accumulate-to-immediate conversion, immediate-to-Accumulate conversion, constraint indicator type change, target expansion (self → party), or `N` modification before it is read by the effect formula. A card with any forbidden form is a hard production block.

**IS-AC-34** *(ADVISORY — production gate)* — GIVEN the 11-item MVP pool at final composition, WHEN the modifier types are counted, THEN the pool contains 3 Type 1, 3 Type 2, 2 Type 3, 2 Type 4, and 1 Restoration item (or an approved variant recorded in Tuning Knob 4 with the approval date). Any deviation from the approved composition is flagged for producer review before print.

---

### Coverage Gaps — Additional Criteria

**IS-AC-35** *(BLOCKING)* — GIVEN a zone has one item attached with **no departure trigger**, WHEN the host layer is stripped during play, THEN the item is removed at step 3 of the Layer Reveal Procedure; no trigger effect fires; the item enters the discard pile; the zone's anchor is now empty and eligible for a new item if a new layer is revealed.

**IS-AC-36** *(BLOCKING)* — GIVEN a die is committed to a zone during Placement Phase AND that zone's layer is stripped before the die resolves (triggering item departure), WHEN the strip event resolves, THEN the placed die fizzles (is consumed without effect per Equipment System Rule 5.6); the departure trigger fires normally at step 3 if one is present; the die result does not apply to the zone's effect.

**IS-AC-37** *(BLOCKING)* — GIVEN a Type 3 (threshold-reduction) item is attached to a zone **without the Accumulate keyword**, WHEN game turns advance while the item is attached, THEN the item's modifier has no gameplay effect (there is no threshold to reduce); the item occupies the anchor, departs with the layer normally, and its departure trigger fires (if any); no error condition occurs and no player action is required.

**IS-AC-38** *(ADVISORY)* — GIVEN an item is currently attached to an active zone, WHEN a player attempts to voluntarily detach the item (not through a layer strip event), THEN the action is illegal; the item remains attached; no game-state change occurs. Voluntary detachment is not a legal action at any point in the session.

## Open Questions

| ID | Question | Owner | Priority |
|---|---|---|---|
| OQ-1 | How many items should the Events System generate across a full session at production scope (full 50-card event deck)? The MVP calibration (3 treasure events) was set for a 15-card deck. The rate may need to scale differently with deck size. | Item System + Events System co-authoring | MEDIUM |
| OQ-2 | ~~What is the First Player token rotation rule?~~ **RESOLVED 2026-05-28**: Per-card rotation is authoritative (events-system.md Rule 7 — token rotates after each event card resolves, including treasure events). Rule 3 rationale updated to reflect this. events-system.md is the authoritative source; item-system.md references it. | — | CLOSED |
| OQ-3 | Can post-combat item rewards be drawn after a Boss defeat (God card)? The Boss is the last card — the session ends on defeat or win. If the party defeats the God card, is there a post-combat reward before the session-end? | Events System + Item System | LOW |
| OQ-4 | Should MVP synergistic pairs be documented explicitly in the item design sheet before card production begins? The three confirmed synergy types (item + Accumulate layer, item + constraint layer, item + passive on-destroy) need three specific card designs that instantiate them. | Card designer + Item System | HIGH |
| OQ-5 | At what point in the session does the item pool visibly deplete to signal scarcity to players? Should there be a physical indicator (e.g., a small counter or marker) for items remaining, or is the face-down pile's thinness sufficient? | Art Director + Game Setup | LOW |
| OQ-6 | Do items interact with restoration effects? If a layer is restored via an explicit card effect, can items be re-attached to the restored layer? The current rule states items do not return automatically with restored layers — but nothing prevents immediate re-attachment during the following Placement Phase if an item is somehow available. Verify this creates no perverse restore-and-re-equip loop. | Item System + Equipment Degradation | MEDIUM |
