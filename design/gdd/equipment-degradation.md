# Equipment Degradation

> **Status**: Designed
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-25
> **Implements Pillar**: The Build is Never Finished (primary); Agency Over Dice (secondary); Combos Must Be Discoverable (supporting)
> **CD-GDD-ALIGN**: APPROVE (2026-05-25) — five non-blocking notes: (1) anti-example added to Principle 2; (2) cross-table indicator legibility spec added; (3) last-layer emotional beat added to Player Fantasy; (4) strip-event directorial note added; (5) flag to narrative-director that Player Fantasy makes a canonical claim about Heaven's design philosophy.

## Overview

The Equipment Degradation system defines what happens when an equipment layer is stripped: the removed card goes face-down beside the player board, and the card beneath — the next layer — becomes the active face of that slot. This reveal event is the central transformation engine of S-Layers. Every hit the party chooses to absorb is simultaneously an act of damage and an act of build progression: stripping the outer shell exposes the layer the player designed their session around finding, or the one they didn't know was there.

Equipment Degradation owns four things: the **layer reveal rules** (exactly what happens at the moment a card is stripped), the **layer ordering design principles** (what each position in a stack must contain and why), the **equipment reveal event** (the physical card flip and its game-state consequences), and the **item disposition rule** (an item attached to a stripped layer departs with it and is discarded). This system does not own the hit trigger — that belongs to the Integrity System, which sends a "layer stripped" event with a slot and player identifier. Degradation receives that event and owns everything that follows: the flip, the new active state, the item departure, and the design rules governing what card was placed beneath.

The layer stack is not a random progression. Each character's layer ordering is co-designed with the Character System: the outermost layer is the character's baseline; deeper layers are more demanding, more powerful, or more dangerous — designed to be discovered only after the outer shell has been earned away. A card author placing a layer beneath another is making a specific promise to the player: *what you find here is worth the hit it took to reach it, and it will change how your next five turns work*. The Equipment Degradation system is the guarantee that this promise is structurally enforced, not left to chance.

## Player Fantasy

You did not lose armor. You spent it.

Heaven did not build this equipment to last. It was built to be operated — wound up and aimed and run until it gave out. The hit that strips the outer layer is not the Angels doing damage to you. It is the machine completing one more cycle of the job it was made for. You are the operator. The failure is yours to direct.

Every layer you spend is a routing decision. The party leans in before the Angel's turn and reads the boards: *whose card can we afford to flip, and into what?* The answer depends on what each player knows is beneath their own card — and what they're willing to tell the table. The flip is not damage absorbed. It is a consequence you scheduled.

When the card goes face-down, it goes cleanly. No grief. The card below it comes face-up. The table reads the new effect, recalculates routes, updates the plan. Three cards into the session, the board you started with is not the board you are playing. This is not depletion. This is the build arriving at the configuration you were steering toward — or the one you discovered was possible two layers in, which is better.

The equipment does not know who is wearing it. Heaven built the routing prayer into the card, calibrated to a die value, aimed at a threshold. You are saying the prayer wrong on purpose, with dice that cooperate badly and in orders the guard at the door cannot classify. What the layer underneath asks for is usually harder. What it offers in return is usually worth it.

The card you discarded is not waste. It is receipt.

When a player reaches the last layer, something shifts in the room. Not panic — recognition. *This is all I have left, and I chose to be here.* The outermost cards were protection; they are gone because you spent them. What remains is the card that was always underneath: the one that was there at session start, face-down and waiting, for the moment the others had done their work. The final layer is not an emergency. It is the statement the session has been building toward.

**Directorial note (art/audio):** A strip event should feel like a mechanism completing a cycle, not breaking. The card leaves cleanly. The flip is not a destruction sound — it is a *click*, a *turn*, a gear engaging the next position. The machine is still running.

**Design test:** When a player says "I'll take that hit" — before the party has routed it — the system has worked. When they say "route it to me, I know what's under there," the system has worked twice. If the flip ever feels like subtraction rather than transformation, a layer is under-designed: the card beneath did not keep the promise the card above implied.

## Detailed Design

### Core Rules

---

**Rule 1 — Layer Reveal Procedure**

When the Integrity System sends a "layer stripped" event for a specific slot on a specific player board, the following sequence executes in order:

1. The player takes the top card from the targeted slot.
2. The stripped card is placed **face-down** beside the player board in the **per-slot discard pile** for that zone (Head pile, Body pile, or Weapon pile — kept separate). The card is accessible for restoration effects.
3. If the slot had an **item attached** to the stripped layer: the item card is removed from the Item Anchor Zone and placed in a separate discard. The Item System owns what happens to the item next — it may be discarded permanently, or its own card text may specify otherwise. Equipment Degradation's contract: the item leaves the board with its host layer, immediately, without exception.
4. If a **next layer** exists beneath the stripped card: that card is **flipped face-up** and becomes the active layer of the slot. Its effects are live immediately for any subsequent resolution events in the same phase.
5. If **no next layer** exists (the stripped card was the last one): the slot becomes **inert** per Integrity System Rule 9. No card is face-up. The slot accepts no dice, produces no effects, and cannot receive routing until an explicit restoration effect fires.
6. Any **Accumulate total** on the stripped card is lost at step 2. The newly revealed layer (if any) begins with a fresh Accumulate total of zero.

> *Timing note: Step 4 (new layer becomes active) fires immediately. If the strip event occurs during Effect Resolution Phase and a die was placed on that zone for later resolution in the same phase, the die fizzles when its slot arrives — the slot is inert or the new layer is active, but the old layer's effect is gone. See Equipment System Rule 5.6.*

---

**Rule 2 — Self-Strip (Card-Text Mechanic)**

Some equipment cards contain a self-strip instruction (e.g., the Martyr's Weapon layers: "this equipment loses 1 Integrity after firing"). A self-strip resolves identically to a hit-triggered strip:

- The active layer of the specified zone is stripped following the full Layer Reveal Procedure (Rule 1).
- The self-strip is **not a hit** — it does not come from the Angel, does not consume a routing decision, and is not blocked by inert-zone routing rules.
- **Order of operations**: Printed card order governs. If the card reads "deal damage, then lose 1 Integrity," the damage effect fires first and the self-strip fires second. The zone is active when the damage fires; it begins the strip sequence after.
- A self-strip on the last remaining layer of a zone makes that slot inert by the same rules as a hit on a last layer.

---

**Rule 3 — Layer Ordering Design Principles**

These are card-author mandates — not player-facing rules. A card author populating a layer stack must follow all four principles:

**Principle 1 — Each layer is a distinct expression of the same strategy, not a reskin.** Two layers in the same zone must differ in at least one of: constraint, effect type, output shape, or synergy surface. Layers that produce the same play pattern at different magnitudes (e.g., "deal [N] damage" on every layer with only the number changing) fail this principle, unless that escalation is itself the designed progression.

**Principle 2 — Deeper layers nuance the strategy, not escalate it.** Each layer is a different expression of the same play style — not a harder-to-operate version of the layer above it. Constraints, thresholds, and effect types may change between layers, but the character's core strategic identity must remain legible at every depth. A card author must be able to state in one sentence how each layer reinforces the character's named strategy. If the sentence requires "but" or "however," the layer has drifted.

> *Martyr Body example: all four layers serve the same strategy ("absorb hits, convert them to offense"). L1 deals 5 AoE on destroy; L4 deals 8 AoE on destroy — the change is magnitude and synergy surface, not a new strategy. L4 is the most powerful expression of the same sentence, and it creates an emerging combo with the Martyr's Head healing — both layers rhyme with the same build identity.*

> *Anti-example of a principle violation: A Martyr Body where L1 is "on destroy: deal 5 AoE damage" and L3 is "on destroy: heal 2 Integrity to all allies." The strategy changed — L1 serves "convert hits to offense" while L3 serves "convert hits to healing." These do not rhyme; L3 must be redesigned to stay within the absorption-offense identity.*

**Principle 3 — The outermost layer establishes the strategy; all layers rhyme with it.** Layer 1 (the top card) is what the player reads at session start. It sets up the play the character is built around. Deeper layers may offer different angles on that strategy — different output types, different synergy surfaces, different trigger conditions — but they must *rhyme* with Layer 1, not contradict it. A player who has read Layer 1 should, upon seeing any deeper layer, recognize it as belonging to the same character.

> *This is not a constraint on power level. A deeper layer may be significantly more powerful (Martyr L4 Body), more situational, or more synergy-dependent — provided it serves the same named strategy. "More powerful" and "different nuance" are both valid. "Different strategy" is not.*

**Principle 4 — The innermost (last remaining) layer is the card author's choice.** No special rule governs the last layer's power level, effect type, or difficulty. Card authors may make the last layer the most powerful (earned reward), the most desperate (crisis tool), or simply the most honest expression of what the character is built around. The requirement is only that it is a deliberate, documented choice — not a default.

---

**Rule 4 — Degradation Indicator Requirements**

Each zone on the player board must display a **degradation indicator** — a visual element conveying layer state at a glance without requiring the player to read the card. The indicator must communicate three pieces of information:

| Information | How communicated | Read at glance? |
|---|---|---|
| **How many layers remain in this zone** | Icon set (e.g., 3 dots/bars for 3 layers, updated as layers strip) | Yes — count visible before reading card text |
| **Whether the next layer is stronger or weaker** | Directional indicator (arrow up, arrow down, or equal mark) printed at the zone boundary | Yes — after flip |
| **Whether this is the last layer** | Special "final layer" marker (distinct icon, border treatment, or color accent) on the card itself | Yes — alert state, visible before flip |

**Design test for the indicator**: A player who has not looked at their own board this turn must be able to answer "how many layers do I have left in my Body zone?" in under 2 seconds by reading the indicator alone, without lifting or flipping any card. If they cannot, the indicator design has failed.

**Cross-table legibility requirement**: The indicator must be readable from any seat at the table at standard play distance (60–100 cm). The "I know what's under there" cooperative moment requires that a teammate can read your indicator from their seat — not only the owning player from directly above. Physical production must validate this at prototype stage.

**Owner**: The degradation indicator's information requirements are owned by Equipment Degradation. Physical production spec (dimensions, icon set, color usage) is owned by the Art Director pipeline. The requirements above are the design constraint the art director works from.

---

**Rule 5 — Accumulate Total Persistence Across Layer Transitions**

Accumulate zones bank a running total across turns. When a layer strip event fires on an Accumulate zone:

- The stripped layer's running total is **lost immediately** at step 2 of Rule 1. It does not transfer to the next layer.
- The newly revealed layer begins at **total = 0** regardless of what was accrued on the stripped layer.
- **Reservoir zones** (Accumulate ∞): the same rule applies. A Reservoir total is tied to the specific card, not to the zone. If the Pilgrim's Weapon layer is stripped mid-accumulation, the entire reservoir is lost with the card.

> *This is a significant player-board risk for the Pilgrim. The party must account for Reservoir total loss when routing hits — do not route to Weapon if the Pilgrim holds a large reservoir total they have not released.*

---

**Rule 6 — Layer Stack Author Accountability**

For every character, the card author must document the layer stack for each zone in a **layer design sheet** before cards go to art or print. The layer design sheet records, per layer:

- Layer number (1 = outermost, N = innermost)
- Effect text (within 12-word budget)
- Constraint
- E_slot value (from Equipment System Formula 1)
- S_encounter contribution across the set
- Ordering rationale — one sentence explaining why this layer is at this position and how it expresses the character's named strategy

The layer design sheet is the audit document. A character cannot be approved for production without a complete, reviewed layer design sheet for each zone.

---

### States and Transitions

**Zone layer states (extending Equipment System states):**

| State | Condition | Notes |
|---|---|---|
| **Layer 1 active** | 3 layers remain (default at session start) | Outermost card; establishes the strategy |
| **Layer N−1 active** | 2 layers remain (1 strip has occurred) | Middle layer(s); different nuance on the same strategy |
| **Layer N active (final)** | 1 layer remains | Innermost; final-layer marker visible on card |
| **Inert** | 0 layers remain | No dice; no effect; slot discard pile exists but no card is live |

**Layer strip state machine:**

```
Layer 1 active
  ↓ Hit or self-strip fires
  → stripped card → face-down per-slot discard pile
  → item (if attached) → item discard (Item System owns next)
  → Accumulate total → lost
  → Layer 2 active (if card exists beneath)
  → Inert (if no card beneath)

Layer N active (final layer)
  ↓ Hit or self-strip fires
  → stripped card → face-down per-slot discard pile
  → item (if attached) → item discard
  → Inert (no card beneath; Integrity System Rule 9 triggers)

Inert
  ↓ Restoration effect fires explicitly ("restore a layer to [slot type]")
  → top card from per-slot discard pile returns face-up
  → Layer N active (restored; Accumulate total = 0; item anchor vacant)
```

---

### Interactions with Other Systems

| System | Direction | Interface |
|---|---|---|
| **Integrity System** | Upstream | Sends "layer stripped" event (slot, player). Equipment Degradation receives and executes the reveal procedure (Rule 1). Integrity System also owns the inert state definition, face-down discard pile model, and restoration effect hook (Rule 9). |
| **Equipment System** | Upstream | Zone structure (Head/Body/Weapon), active layer model (top of stack), die fizzle rule (die fizzles if zone stripped mid-resolution before die resolves), item anchor zone (one item per zone; departs with layer). Equipment Degradation works within these rules without redefining them. |
| **Character System** | Bidirectional | Character System defines layer ordering per zone for each character; Equipment Degradation defines the design principles those orderings must follow (Rules 3–6). At production gate, both the Character System layer ordering and the Degradation layer design sheet are reviewed together. |
| **Item System** | Downstream | Equipment Degradation defines the item disposition event: the item departs with the stripped layer, immediately. Item System owns what happens to the item after departure — permanent discard, return to shared pool, or card-text exception. Item System must not contradict the "item leaves with layer" contract defined here. |
| **Combat System** | Downstream | The layer reveal procedure (Rule 1) fires within the current resolution phase. Combat System's phase sequence must accommodate strip events mid-phase without interrupting or re-sequencing other committed effects. No changes to Combat System rules are required — strip events are passive within existing phase boundaries. |

## Formulas

### Formula 1 — Expected Layer Strips Per Encounter (`N_strips`)

The `N_strips` formula is defined as:

`N_strips = H_pt × T_encounter`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Hits received per player per turn | H_pt | float | [0.3, 1.5] | Expected hits routed to one player per turn; derived from Angel face map (Angel System) |
| Encounter length in turns | T_encounter | float | [6, 10] | Expected turns per encounter; use 8 for MVP balance work |
| Expected strips per encounter | N_strips | float | [2.4, 12.0] | Expected number of layer reveals one player experiences across the full encounter |

**Output Range:** At MVP design targets (H_pt = 1.0, T_encounter = 8): `N_strips = 8.0` strips per encounter. This would exhaust a 9-layer board (L_total = 9) in exactly one encounter — the ceiling. In practice, routing distributes hits across the party so no single player receives all 8.

**Design constraint table:**

| H_pt | T_encounter = 8 | Strips per zone (÷3) | Interpretation |
|---|---|---|---|
| 0.5 | 4.0 | 1.3 | Rarely strips to layer 2. Degradation feels slow. |
| 1.0 | 8.0 | 2.7 | Reaches layer 3 consistently in one encounter. ✓ Target |
| 1.5 | 12.0 | 4.0 | Exceeds default layer count (3) — board destroyed before encounter ends. |

**Design guardrail:** N_strips must satisfy `N_strips < L_total` at average H_pt, or board destruction is the expected outcome per encounter. Board destruction is the loss condition, not a design target. Ensure all characters satisfy:

`L_total / H_pt ≥ T_encounter` (from T_board formula — Integrity System)

**Example:** The Pilgrim (L_total = 8, H_pt = 1.0): `N_strips = 8.0`. Pilgrim board is expected to reach L_total = 0 in exactly one encounter at H_pt = 1.0 with no routing support. This is confirmed marginal by T_board (see Character System Formula 1). Routing to Martyr is load-bearing.

---

### Formula 2 — Per-Layer Balance Check (`S_layer`)

The `S_layer` check verifies that each individual layer within a zone, when considered as the active face for its portion of the encounter, does not push the set's total S_encounter outside the [54, 74] band. This is a design tool — it is not computed at the table.

`S_layer(i) = E_slot(i) × T_active(i) + B_burst(i)`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Layer index | i | int | [1, N] | 1 = outermost (first active), N = innermost (last) |
| Expected slot output when active | E_slot(i) | float | [0, 14] | Per-turn output of layer i (Equipment System Formula 1) |
| Turns layer i is the active face | T_active(i) | float | [0, T_encounter] | Expected turns this layer is live: approximately 1/H_pt per layer, summing to T_encounter |
| Burst credit of layer i | B_burst(i) | float | [0, ∞) | One-shot or on-destroy contribution (e.g., Martyr Body: 5/6/7/8 damage on strip) |
| Layer output | S_layer(i) | float | [0, ~40] | Expected output from this specific layer during its active window |

**Set validation:** The sum of all layer outputs must satisfy the S_encounter band:

`S_encounter = Σ S_layer(i) ∈ [54, 74]`

At T_encounter = 8 and H_pt = 1.0, each of the 3 layers in a zone is expected to be active for approximately 2–3 turns. If any single layer contributes disproportionately (> 30 units), the set has a "power spike layer" and must be reviewed for balance before print.

**Passive (on-destroy) layer handling:** A passive layer with no die placement has E_slot(i) = 0. Its entire contribution is B_burst(i) = effect_value × P_strip, where P_strip = 1.0 if the strip is guaranteed in an encounter (i.e., T_board suggests the layer will be reached), or < 1.0 if the layer may not be reached.

**Example — Martyr Body layer balance:**

| Layer | E_slot | T_active | B_burst | S_layer |
|---|---|---|---|---|
| L1 (5 dmg on destroy) | 0 | 2.7 | 5 × 1.0 = 5 | **5** |
| L2 (6 dmg on destroy) | 0 | 2.7 | 6 × 1.0 = 6 | **6** |
| L3 (7 dmg on destroy) | 0 | 2.7 | 7 × 0.8 = 5.6 | **5.6** |
| L4 (8 dmg AoE on destroy) | 0 | 2.7 | 8 × 0.6 = 4.8 | **4.8** |
| **Total (Body zone only)** | — | — | — | **21.4** |

*P_strip decreases for deeper layers because they may not be reached in every encounter. Calibrate from playtest observations. The Body zone contributes ~21.4 to S_encounter — the Head and Weapon zones must together contribute ~42–53 units to reach the [54, 74] band. Confirmation DEFERRED to Martyr starting set balance review.*

---

### Formula 3 — Self-Strip Activation Budget (`A_weapon`)

For zones with a self-strip mechanic, the number of activations before the zone goes inert is deterministic:

`A_weapon = N_weapon_layers`

where N_weapon_layers is the number of layers in the Weapon zone.

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Weapon zone layer count | N_weapon_layers | int | [1, 4] | Number of layers in the Weapon zone at session start |
| Total activations before inert | A_weapon | int | [1, 4] | Each activation consumes one layer; A_weapon = N_weapon_layers |

**Martyr Weapon at MVP:** N_weapon_layers = 3, therefore A_weapon = 3. The Weapon zone fires a maximum of 3 times per encounter (×2 → ×3 → ×4 progression), then becomes permanently inert.

**Design implication:** The Martyr Weapon zone's total burst contribution is deterministic and bounded:

`B_weapon_total = E_slot(L1) + E_slot(L2) + E_slot(L3)`

where each E_slot(i) uses the `[N]×k` formula with k = 2, 3, 4 respectively and p_c = 1.0 (no additional constraint).

| Layer | k | E_slot |
|---|---|---|
| L1 (`[N]×2`) | 2 | 1.0 × 2 × 4.47 = **8.94** |
| L2 (`[N]×3`) | 3 | 1.0 × 3 × 4.47 = **13.41** |
| L3 (`[N]×4`) | 4 | 1.0 × 4 × 4.47 = **17.88** |
| **Total** | — | **40.23** |

This is the Weapon zone's entire encounter contribution if all 3 layers are activated before any hit-triggered strip. Martyr S_encounter balance confirmation remains DEFERRED until full set calculation is complete.

---

### DEFERRED Items

| ID | Item | Blocked by |
|---|---|---|
| D-1 | H_pt at actual Angel face maps — feeds N_strips | Angel System GDD |
| D-2 | Martyr Body P_strip values per layer — feeds S_layer balance | Playtest observation |
| D-3 | Martyr Weapon + Body combined S_encounter confirmation | Full set balance review (this GDD + Character System) |

## Edge Cases

**Strip Timing — Die Fizzle**

**If a layer is stripped during Effect Resolution Phase and a die was placed on that zone for later resolution in the same phase**: the die fizzles when its slot arrives in the sequence. The stripped layer takes the die with it. This is consistent with Equipment System Rule 5.6 and unchanged by this system. Resolution order determines which effects fire before the strip occurs — the party should position die resolution before any effect that may strip that zone, if they want the die to fire.

**If a die was placed on a zone and the Martyr's Weapon self-strips after the damage fires**: the die placed on that zone this turn has already resolved (the damage fired first, per printed card order). The self-strip removes the now-spent layer. No fizzle occurs because the die already resolved before the strip event.

---

**Self-Strip — Last Layer**

**If the Martyr's Weapon fires its self-strip effect and it is the last remaining Weapon layer**: the damage fires first (printed card order), then the Weapon layer is stripped, and the zone becomes inert. The Weapon zone is permanently inert for the rest of the encounter. No die may be placed on it from the next turn forward.

**If the Martyr's Weapon is targeted by a hit from the Angel while also scheduled to self-strip this turn**: the Angel hit and the self-strip are separate events. They each strip one layer. If the zone has only one layer remaining, the first strip (whichever fires first in resolution order) strips the layer and makes the zone inert. The second strip event finds an inert zone and is wasted — an inert zone cannot be stripped further.

---

**Restoration Edge Cases**

**If a restoration effect targets a zone whose discard pile is empty** (the zone was never stripped, or all stripped cards are already consumed): the restoration effect is wasted. No layer is returned. The zone state is unchanged.

**If two restoration effects target the same zone in the same phase**: the first effect restores the top card from the discard pile (zone becomes active, one layer restored). The second effect resolves against a now-active zone. If the second effect reads "restore a layer to an inert zone," it is wasted — the zone is no longer inert. If the second effect reads "restore 1 layer to [slot type]" without the inert requirement, it restores a second card from the pile, adding a second layer.

**If a restoration effect fires and no per-slot discard pile exists** (the zone was never stripped this session): the restoration effect is wasted. No layer can be added to a zone that has not lost any.

---

**Item Disposition Timing**

**If a layer with an attached item is stripped during Effect Resolution Phase** (by a card effect or self-strip): the item departs immediately at step 3 of Rule 1, in the same moment the card goes to the discard pile. Any subsequent effect resolving later in the same phase that would reference "the item on this zone" finds no item — item presence is not locked at phase start.

**If a layer with an attached item is stripped during Angel Resolution Phase** (by a hit): the item departs immediately. The newly revealed layer's Item Anchor Zone is vacant from the moment the new layer becomes active.

**If a stripped item's card text reads "when this item is removed, [effect]"**: that effect fires at step 3 of Rule 1, as part of the strip sequence. The Item System owns what the effect does; Equipment Degradation guarantees the item's removal event fires before the new layer becomes active (step 4).

---

**Accumulate Total Loss — Integrity Rule 12 Interaction**

**If a slot has a running Accumulate total, a die is placed on it this turn, and the layer is stripped during Effect Resolution Phase**: Integrity System Rule 12 states slot state is locked at the start of Effect Resolution Phase — the placed die still resolves (fires its Accumulate contribution and checks threshold). After the phase ends, the slot becomes inert and the Accumulate total is lost. There is no contradiction: the total is lost *after* the phase, not at the moment of strip.

**If the Accumulate effect fires (threshold crossed) and then the layer is stripped later in the same phase**: the Accumulate effect already resolved, the total reset to zero. When the layer strips, the post-reset total (zero or whatever was added after the reset) is lost. The fired effect stands — it is not reversed by the strip.

---

**Two Strips to the Same Zone**

**If two events would strip the same zone in the same turn** (e.g., a hit from the Angel routes to the Martyr's Body AND a card effect strips Body in the same phase): each strip removes one layer. If the zone has two or more layers, both strips fire in party-chosen order — two layers are stripped, two cards go to the discard pile, two reveals occur. If the zone has only one layer, the first strip makes it inert; the second strip finds an inert zone and is wasted.

**If the Pilgrim's Weapon zone (Reservoir) is stripped while holding a large accumulated total**: the Reservoir total is lost at step 2 of Rule 1, before the new layer is revealed. The new Weapon layer (if any) begins at total = 0. There is no mechanism to transfer or preserve the Reservoir total across a layer transition. This is an irreversible loss — the party must choose to release before routing a hit to the Pilgrim's Weapon if the total is strategically significant.

## Dependencies

**Upstream dependencies** (systems Equipment Degradation depends on):

| System | Type | What Equipment Degradation consumes |
|---|---|---|
| **Equipment System** | Hard | Zone structure (Head/Body/Weapon), active layer model (top of stack = active), die fizzle rule (Equipment System Rule 5.6), item anchor zone model (one item per zone, anchor departs with layer), 12-word effect text budget for layer cards. Equipment Degradation works within these rules without redefining them. |
| **Integrity System** | Hard | The "layer stripped" event trigger (hit removes top card), inert slot definition (0 layers), face-down discard pile model (per-slot piles), restoration effect hook (Rule 9: explicit card text required to restore a layer), Integrity Rule 12 (slot state locked at Effect Resolution Phase start — Accumulate die still fires even if strip occurs mid-phase). |

**Downstream dependents** (systems that depend on Equipment Degradation):

| System | Type | What they consume |
|---|---|---|
| **Character System** | Bidirectional | Character System defines layer ordering per zone per character; Equipment Degradation defines the design principles that ordering must follow (Rules 3–6). At production gate, both are reviewed together via the layer design sheet. Character System layer counts (L_total) feed N_strips validation. |
| **Item System** | Hard | Item disposition rule: item departs with its stripped layer, immediately, with no exceptions unless the item's own card text specifies otherwise. Item System must not contradict this contract. Item System also consumes the face-down discard pile model for restoration interactions (items do not return with restored layers unless explicitly stated). |
| **Combat System** | Hard | Layer reveal procedure (Rule 1) fires within existing phase boundaries. Combat System's phase sequence must accommodate strip events mid-phase. No new phases or phase interrupts are required — strip is a passive event within the current resolution phase. |

**Bidirectional consistency requirements:**
- **Character System GDD** must reference Equipment Degradation's layer design principles (Principles 1–4) when documenting each character's layer stack. The layer design sheet is the shared artifact.
- **Item System GDD** must state the item disposition rule explicitly and confirm it does not contradict the "item leaves with layer" contract. If Item System introduces any exception (e.g., "this item is indestructible and transfers to the new layer"), it must surface this as a rules override with explicit card text.
- **Equipment System GDD** and **Integrity System GDD** are upstream — they do not depend on this GDD, but their relevant rules (Rule 5.6, Rule 12, Rule 9) are listed here for traceability.

## Tuning Knobs

**1. Layer Count per Zone (N_layers)**
- **Adjusts**: How many reveals occur per encounter; how long each layer's strategy is in play
- **Current values**: 3 per zone for default characters; Pilgrim Body = 2; Martyr Body = 4
- **Safe range**: 2–4 per zone. L_total ∈ [6, 12] per board.
- **Too low (< 2)**: Zone depletes in 2 turns under H_pt = 1.0; the layer design's depth is never experienced
- **Too high (> 4)**: Board survives far beyond a single encounter; degradation rarely matters; component count inflates
- **Interaction**: Changing N_layers requires T_board recalculation (`L_total / H_pt`) and a full N_strips validation. Any change also affects the S_layer balance calculation (T_active per layer shifts).

**2. Layer Ordering (design-time knob — not numeric)**
- **Adjusts**: The pacing of the build evolution — whether the player gets the "interesting" card early or earns it late
- **Current values**: Defined per character in the layer design sheet
- **Too front-loaded** (best layer on top): The build peaks at session start; degradation feels like loss rather than transformation
- **Too back-loaded** (all interesting layers at the bottom): Players may never reach the payoff if the encounter ends early or board destruction occurs
- **Guideline**: At H_pt = 1.0, N_strips ≈ 8 across all zones over an 8-turn encounter. Each zone is expected to be stripped ~2.7 times. Design for layers 1–3 all being live, not only layer 3.

**3. Self-Strip Cost per Activation (Martyr Weapon)**
- **Adjusts**: How quickly the Martyr Weapon zone burns through its layers — determines A_weapon
- **Current value**: 1 layer per activation (A_weapon = N_weapon_layers = 3)
- **At 0 cost**: Weapon never self-strips; the layer progression (×2 → ×3 → ×4) is never reached; the escalating damage structure collapses
- **At 2 layers per activation**: Zone exhausts in 1–2 activations; the ×4 layer is never reached in normal play
- **Interaction**: Changing the cost changes A_weapon, which changes B_weapon_total. Full S_encounter recalculation required.
- **DEFERRED**: Final calibration requires full Martyr S_encounter confirmation (D-3 in Formulas DEFERRED items).

**4. Degradation Indicator Information Density**
- **Adjusts**: How much layer-state information is visible without reading the card
- **Current requirement**: Three signals — layers remaining, stronger/weaker next layer, final-layer alert
- **Too little information**: Players must pick up cards to assess zone state → table friction → slower play
- **Too much information**: Indicator becomes a wall of symbols; no longer readable at a glance
- **Design test**: Validate in playtests that the three-signal indicator is sufficient for routing decisions. If players consistently pick up cards to assess zone state, add a signal. If they ignore the indicator, reduce it.

**5. Restoration Effect Availability (owned by Item System)**
- **Adjusts**: How often players can recover stripped layers — affects how punishing degradation feels
- **This GDD does not set this value** — it is owned by Item System (rarity and availability of restoration-effect items)
- **If restoration is too common**: Degradation never becomes permanent; the build evolution stalls; "The Build is Never Finished" pillar is undermined
- **If restoration is too rare**: Every strip feels irreversible and potentially punishing; risk of loss-spiral
- **Interface**: Equipment Degradation defines the restoration hook (Rule 1 Step 5; Integrity Rule 9). Item System controls how often it fires.

## Visual/Audio Requirements

[To be designed]

## UI Requirements

[To be designed]

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Gate levels: BLOCKING = must pass before production; ADVISORY = flag for review; DEFERRED = blocked on cross-system confirmation.*

---

### Layer Reveal Procedure (Rule 1)

**ED-AC-01** *(BLOCKING)* — GIVEN a zone with 3 layers and a hit routed to it, WHEN the strip event fires, THEN: (a) the top card is removed; (b) it is placed face-down in the correct per-slot discard pile; (c) if an item was attached, it departs with the card; (d) the next card is flipped face-up and becomes active; (e) all in this order, before any other event proceeds.

**ED-AC-02** *(BLOCKING)* — GIVEN a zone with 1 layer remaining and a hit routed to it, WHEN the strip event fires, THEN: (a) the top (and only) card is removed and placed face-down in the discard pile; (b) no card is flipped face-up; (c) the slot is immediately inert — accepts no dice, produces no effects, cannot be targeted for routing.

**ED-AC-03** *(BLOCKING)* — GIVEN a zone with an Accumulate running total of 7 and a hit routed to it, WHEN the strip event fires, THEN the Accumulate total of 7 is lost at the moment the card enters the discard pile; the newly revealed layer (if any) starts with an Accumulate total of 0.

**ED-AC-04** *(BLOCKING)* — GIVEN three separate zones on a player board each stripped at least once, WHEN a tester inspects the discard piles, THEN there are exactly three separate face-down piles — one per zone type (Head, Body, Weapon) — with no cards cross-filed between piles.

**ED-AC-05** *(ADVISORY)* — GIVEN a zone with 2 layers and a layer stripped, WHEN the next layer becomes active, THEN the active layer's effects are live immediately for the remainder of the current phase — any subsequent effect in that same phase may interact with the new layer's effect if relevant.

---

### Self-Strip (Rule 2)

**ED-AC-06** *(BLOCKING)* — GIVEN the Martyr's Weapon zone is active and a die is placed there, WHEN the Weapon effect resolves, THEN damage to the Angel fires first; after that damage resolves fully, the Weapon layer self-strips per the Layer Reveal Procedure (Rule 1). The damage value from the card that was just active is used — the card is not yet stripped when the damage fires.

**ED-AC-07** *(BLOCKING)* — GIVEN the Martyr's Weapon self-strips and it is the zone's last layer, WHEN the self-strip resolves, THEN the zone becomes inert immediately; no die may be placed on the Weapon zone for the remainder of the encounter.

**ED-AC-08** *(BLOCKING)* — GIVEN the Martyr's Weapon zone receives both a self-strip (from card effect) and a hit from the Angel in the same turn, WHEN both events have resolved, THEN each removes one layer independently. If the zone had 2 layers at the start of the turn, both layers are stripped (zone inert). If the zone had 1 layer, the first event strips it (inert); the second event finds an inert zone and is wasted — no second strip.

**ED-AC-09** *(BLOCKING)* — GIVEN a self-strip event fires on any zone, WHEN a tester evaluates routing rules for that same turn, THEN the self-strip is NOT treated as a hit — it does not occupy or consume the party's hit routing decision for that turn.

---

### Layer Design Principles (Rule 3)

**ED-AC-10** *(BLOCKING — production gate)* — GIVEN any character ready for production review, WHEN a reviewer reads all layers in a single zone, THEN each layer differs from adjacent layers in at least one of: constraint, effect type, output shape, or synergy surface. Two identical effect texts at different magnitudes with no other difference is a design failure.

**ED-AC-11** *(BLOCKING — production gate)* — GIVEN any character ready for production review, WHEN a reviewer reads all layers in a single zone, THEN each layer can be described in one sentence that connects it to the character's named strategy without using "but" or "however." Any layer failing this test must be redesigned.

**ED-AC-12** *(BLOCKING — production gate)* — GIVEN any character ready for production review, WHEN a reviewer reads Layer 1 of a zone, THEN the character's core strategy is identifiable from that single card alone within 30 seconds — without reading the rules or any other layer.

---

### Degradation Indicator (Rule 4)

**ED-AC-13** *(BLOCKING — production gate)* — GIVEN a player board with any combination of active and stripped layers, WHEN a player who has not looked at their own board this turn glances at a single zone, THEN they can state (a) how many layers remain, (b) whether the next layer is stronger/weaker, and (c) whether this is the final layer — within 2 seconds — without picking up or flipping any card.

**ED-AC-14** *(ADVISORY)* — GIVEN a zone on its final layer, WHEN a tester reads the zone from across the table at normal play distance (~60 cm), THEN the final-layer marker is distinguishable from the standard layer marker without needing to lean in or pick up the card.

---

### Accumulate Loss — Integrity Rule 12 Interaction

**ED-AC-15** *(BLOCKING)* — GIVEN a zone has a running Accumulate total of 5, a die is placed on it this turn, and the layer is stripped during Effect Resolution Phase by a card effect, WHEN the placed die resolves in the same phase, THEN the die fires normally (Accumulate total advances, threshold checked). After the phase ends, the slot is inert and the total is lost. The die's effect is not reversed.

**ED-AC-16** *(BLOCKING)* — GIVEN the Pilgrim's Weapon Reservoir zone holds a total of 14 and a hit strips the Weapon zone's active layer during Angel Resolution Phase, WHEN the strip resolves, THEN the Reservoir total of 14 is lost with the stripped card. The newly revealed Weapon layer (if any) begins at 0. The 14 cannot be recovered.

---

### Two Strips to Same Zone

**ED-AC-17** *(BLOCKING)* — GIVEN a zone with 2 layers remaining and two strip events targeting it in the same turn (one hit, one card effect), WHEN both events resolve, THEN each removes one layer — two cards go to the discard pile, two reveals occur, zone is now inert. Party chooses the order of the two strip events.

**ED-AC-18** *(BLOCKING)* — GIVEN a zone with 1 layer remaining and two strip events targeting it in the same turn, WHEN the first event resolves, THEN the zone is inert. WHEN the second event attempts to resolve, THEN it is wasted — an inert zone cannot be stripped further; no discard occurs.

---

### Formula Criteria

**ED-AC-19** *(BLOCKING)* — GIVEN H_pt = 1.0 and T_encounter = 8, WHEN N_strips is computed, THEN N_strips = 8.0. GIVEN The Pilgrim (L_total = 8), THEN N_strips = L_total — board is at exact destruction threshold under these conditions. Routing support is required; confirmed marginal per T_board.

**ED-AC-20** *(BLOCKING)* — GIVEN The Martyr's Weapon zone (N_weapon_layers = 3), WHEN A_weapon is computed, THEN A_weapon = 3. The Weapon zone fires a maximum of 3 times (×2, ×3, ×4) and is then inert.

**ED-AC-21** *(BLOCKING)* — GIVEN The Martyr's Weapon zone B_weapon_total computation (k = 2, 3, 4; p_c = 1.0; E[N] = 4.47), WHEN computed, THEN B_weapon_total = 8.94 + 13.41 + 17.88 = 40.23.

**ED-AC-22** *(ADVISORY — DEFERRED)* — GIVEN the Martyr's complete starting set (Head + Body + Weapon layers), WHEN S_encounter is computed using S_layer for each zone, THEN S_encounter must fall within [54, 74]. Currently DEFERRED — Body P_strip values and Head layer balance required. Confirm after first prototype playtests.

---

### Layer Design Sheet (Rule 6)

**ED-AC-23** *(BLOCKING — production gate)* — GIVEN any character entering the production pipeline, WHEN a producer reviews it, THEN a completed layer design sheet exists for each zone (Head, Body, Weapon), containing: layer number, effect text, constraint, E_slot value, S_encounter contribution, and one-sentence ordering rationale per layer. Missing or incomplete sheets are a hard production block.

---

---

### Supplementary ACs (QA-review additions)

**ED-AC-24** *(BLOCKING — ruling: new layer inert until next turn)* — GIVEN a layer strip reveals a new layer mid-Effect-Resolution Phase, WHEN subsequent dice placed this phase resolve against the newly revealed layer's slot, THEN the new layer's passive effects are inert for the remainder of this phase. The new layer is face-up and readable, but its effects are live only from the next Placement Phase onward. The stripped layer's Accumulate die (if committed this phase) still fires per Integrity Rule 12.

**ED-AC-25** *(BLOCKING — ruling: card moves immediately)* — GIVEN a strip event fires mid-Effect-Resolution Phase, WHEN the strip resolves, THEN the stripped card physically moves to the face-down discard pile immediately (not at phase end). For the remainder of the phase: the committed die still fires per Integrity Rule 12 (logical state locked at phase start); the physical zone has no active card; if the die triggers the Accumulate threshold, the effect fires and the total is subsequently lost when the phase ends. Physical and logical state may diverge for the remainder of that phase — this is intended.

**ED-AC-26** *(BLOCKING — ruling: self-strip is atomic and automatic)* — GIVEN a die is placed on a self-stripping zone and the constraint is met, WHEN the effect resolves, THEN the self-strip is mandatory and non-interruptible. The party cannot choose to fire the damage without the self-strip, nor can they cancel the self-strip after placing the die. The choice is at die-placement time only: if the party does not want the self-strip, they do not place a die on the zone.

**ED-AC-27** *(BLOCKING)* — GIVEN a zone with exactly one layer remaining, an item attached, and a hit targeting it, WHEN the strip event fires, THEN (a) the item departs (step 3) before (b) the inert state is established (step 5). A QA tester must verify both steps complete before any subsequent event in the sequence proceeds.

**ED-AC-28** *(BLOCKING)* — GIVEN a player board where all three zones become inert in the same Effect Resolution Phase (e.g., multiple card effects strip the last layer of each zone sequentially), WHEN the last zone's strip resolves, THEN the board destruction condition is triggered per Integrity System Rule 11. Any in-flight effects committed this phase (dice already placed before the strips) still resolve per Integrity Rule 13 before board destruction is processed.

**ED-AC-29** *(ADVISORY)* — GIVEN a degradation indicator where the next layer has equal E_slot to the current layer, WHEN the indicator is read, THEN the "equal" mark is displayed (not "stronger" or "weaker"). E_slot equality is determined by the Formula 1 calculation alone; B_burst is not included in the stronger/weaker comparison for indicator purposes.

---

### Open Flags

| ID | Flag | Action Required |
|---|---|---|
| ED-AC-10–12 | Layer design principle compliance is a production gate, not a table test — requires a defined design review process | Establish the review process in the production pipeline before Alpha |
| ED-AC-22 | Martyr S_encounter balance DEFERRED | Confirm after full set balance review + first playtests |
| ED-AC-13 | 2-second indicator test requires physical prototype | Validate in first internal playtest session |
| ED-AC-24 | New layer passive inert until next turn — confirm no interactions in first playtests that require revisiting this ruling | Observe in playtest session 1 |

## Open Questions

[To be designed]
