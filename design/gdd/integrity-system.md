# Integrity System

> **Status**: Needs Revision — propagation update from angel-system.md redesign (2026-05-27)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-27
> **Implements Pillar**: Fast and Focused (primary); The Build is Never Finished (secondary); Cooperative Ownership (supporting)
> **Creative Director Review (CD-GDD-ALIGN)**: CONCERNS (accepted) 2026-05-24 — three LOW concerns resolved: routing authority clarified (rule 3), near-miss AC added (AC-25), tonal register copy note added to Player Fantasy.

## Overview

The Integrity System defines the two durability models in S-Layers: how players absorb damage and how Angels are defeated. Both models are represented physically as card stacks — the integrity of any entity is the number of card layers remaining in its pile. For **players**, integrity is qualitative: a single "hit" removes the top card from a chosen equipment slot, revealing the layer beneath. There is no HP number — the card's presence in the slot *is* the state. For **Angels**, integrity is quantitative: each layer carries an explicit HP value, and the party must collectively deal damage equal to or exceeding that value within a single turn to strip the layer. Damage against an Angel layer does not carry over between turns, cannot be split across multiple layers, and any excess above the threshold is discarded.

The system serves two design functions simultaneously. First, it defines the destruction event that triggers Equipment Degradation's layer-reveal mechanic — the pivot moment that drives each combat encounter. Second, it defines the quantitative wall the party must break through turn by turn to win. Despite their physical similarity, the two models are deliberately asymmetric: players are fragile and mutable (one hit changes their state), Angels are structured obstacles that require coordinated effort to crack (but only within a single turn's momentum, never across turns).

A player board is destroyed when all three of its equipment slots have been emptied — all layers stripped. This is the loss condition. Losing a single slot leaves two functioning, and a slot reaching zero layers does not immediately end the game. The routing decision — the party collectively choosing which slot absorbs each incoming hit — is the cooperative heartbeat of this system and the moment Pillar 4 (Cooperative Ownership) is most visibly expressed in play.

## Player Fantasy

The Angels are stripping you. This is not a metaphor: each blow removes a layer of what you came in with, and what the dungeon has been holding for you is the card beneath the one that just went. The helmet's second face. The breastplate you didn't know you were wearing until the outer one was taken.

Heaven's logic is that damage diminishes you. This is its mistake. **You have been waiting to see what's underneath.**

The routing decision is the ceremony. The party leans over the table. Three slots, each with something remaining, each with something beneath. Someone says: *take my helm.* The rest consider. Someone else says: *no — take my chest. I know what's there.* Or: *I don't know what's there, but I need to find out.* The Angel's blow, when it lands, is not just damage. It is an answer.

The Angel works differently. Each of its layers is a wall with a number on it — the cost of taking it apart. The party watches its damage totals and watches the threshold. *We need six. We can get to five, but five doesn't count.* The no-carryover rule makes the Angel feel like a real obstacle, not a resource to drain slowly but an edifice that only falls under concentrated effort. When a layer drops, it is not an arithmetic victory. It is a wall coming down.

**Design test**: If a player says mid-encounter, *"take my chest — I need to see what's under it"* — the system has worked. If they only ever say *"take mine, I have the most layers left"* — integrity is being played as a hit point tracker. It is not. It is an excavation.

**Copy note**: Downstream text (layer-reveal flavor copy, item names, event card text) must honor this register. "Ceremonial / prisoner-of-Heaven" is the voice. Generic dark-fantasy descriptors or ironic humor will erode it. Flag this requirement for the narrative-director when copywriting begins.

## Detailed Design

### Core Rules

#### Player Integrity

1. Each player has three equipment slots: **Head**, **Body**, and **Weapon**. Each slot contains a stack of cards called **layers**. The number of layers remaining in a slot is that slot's integrity.
2. A **hit** to a slot removes its top card — the currently active layer — and reveals the card beneath. A hit always removes exactly one layer. There is no HP value to exceed; the hit event itself causes the removal.
3. Hits to a player are assigned to a specific slot during the **Angel Resolution Phase**. The party cooperatively chooses which slot absorbs each hit, subject to the routing rules below. Routing is a **party-table decision** — the player whose slot absorbs the hit does not have unilateral authority. The entire party deliberates. This is load-bearing for Pillar 4 (Cooperative Ownership): routing forces every player to look at every teammate's board.
4. **Routing rules**: A hit must be routed to a slot with at least one layer remaining. Routing to an empty slot is illegal.
5. **Multi-hit ordering**: When an Angel action deals multiple hits, each hit is routed individually in party-chosen order. Slot eligibility is evaluated at the moment each hit is assigned — a slot emptied by hit N is immediately ineligible for hit N+1.
6. **No-route condition**: If a hit cannot be legally routed (no eligible slots remain on any player board), the board destruction condition is met. The Integrity System marks this state; the **Win/Loss Conditions GDD** owns the game-end resolution.
7. **Accumulate loss on strip**: When a slot's layer is stripped, any Accumulate total on that card is lost. The newly revealed layer begins with a fresh Accumulate total of zero.
8. **Routing tiebreaker**: Routing is a cooperative table decision. If the party cannot reach consensus, the Communication Protocol GDD defines the resolution process. The Integrity System takes no position on this.

#### Empty Slots

9. A slot is **inert** when it has zero layers remaining. An inert slot cannot receive dice placements and produces no effects. No effect, item, or ability may restore a card to an inert slot unless that specific card or effect explicitly states "restore a layer to [slot type]." The exception must be printed on the card.
10. An inert slot is permanent within the encounter unless explicitly overridden by a restoration effect.
11. **Board destroyed**: A player's board is destroyed when all three of its equipment slots are inert. This triggers the Win/Loss Conditions check.

#### Slot State During Effect Resolution

12. Slot state is **locked in at the start of the Effect Resolution Phase**. A placed die on a slot that becomes inert during the phase (through self-damage or another card's effect) still fires its effect for the current turn. The newly inert state applies from the next turn forward.
13. A player whose board is destroyed during the Effect Resolution Phase has their remaining placed die effects fire for the current turn before board destruction is processed.

#### Angel Integrity

> **Angel System note (2026-05-27):** The Angel System GDD owns all per-slot Angel mechanics — the 3-column board structure (Head/Body/Weapon), per-slot Slot Tracker Dice, per-slot strip checks, HP calibration, and the multi-slot Cleared definition. Rules 14–22 below state the foundational principles; the Angel System GDD is the authoritative implementation reference.

14. Each Angel has a board structured as three independent **slot columns** (Head, Body, Weapon), each containing a stack of layer cards. Each layer carries an explicit **HP value** and a trigger set. HP is always public for the currently Exposed layer; Hidden layers remain face-down until the layer above is stripped.
15. Damage targets the **exposed top layer of a declared slot** (Head, Body, or Weapon). Players must declare the target Angel and slot before advancing any tracker. Damage to one slot never affects another slot's tracker. The Angel System GDD governs slot declaration rules.
16. **Damage tracking**: Each active slot has its own **Slot Tracker Die**. As each damage-dealing effect resolves, the declared slot's tracker advances. Slot trackers are fully independent of each other. Full tracking rules (two-die overflow, reset timing) are governed by the Angel System GDD.
17. After all player effects have resolved, each slot's tracker total is compared independently to its exposed layer's HP in a **single check per slot**. Strip checks for all slots resolve before any Win/Loss evaluation.
18. If slot tracker total ≥ HP: that slot's layer is **stripped**. The next layer in that slot becomes the Exposed layer. The Slot Tracker Die resets to zero.
19. If slot tracker total < HP: the layer **survives**. The damage total is discarded. No carryover. The Slot Tracker resets to zero.
20. **No cascade (non-Penetrating)**: Outside of Penetrating events, each slot's accumulated damage removes at most one layer per turn. Excess above the threshold is discarded. **Exception — Penetrating (Character System Rule 11; Angel System Rule 6.6):** When the Penetrating flag is active, the damage event cascades **within the targeted slot only** — excess carries to the next layer of the same slot. Strip checks fire sequentially within that slot, outermost first; HP_eff for each newly revealed layer is recalculated fresh (HP_L + active BUFF for this turn) before its check fires. On-destroy passives trigger outermost-first. Penetrating damage does **not** cross slot boundaries — excess stops at the slot boundary even under Penetrating. This is the only exception to the no-cascade rule.
21. **Layer transition timing**: A layer is removed at the end of the Effect Resolution Phase, after all player effects have resolved. For the remainder of the current phase, all references to "the current Angel layer for a slot" refer to the layer active at the start of that phase.
22. **Angel cleared**: When all three slots of an Angel (Head, Body, and Weapon) are **Inert** — all layers in each slot have been stripped — the Angel is **Cleared**. This triggers the Win/Loss Conditions check. Stripping one slot's last layer does not clear the Angel; all three slots must be Inert.

#### Hit Targeting Grammar

23. Angel actions deal one of two hit types. The Angel card text defines which:
    - **Party hit**: The party routes each hit freely to any slot on any player board, within the routing rules above.
    - **Targeted hit**: The Angel card specifies a constraint on the target (e.g., "the player with the fewest layers remaining" or "Head slot only"). The party routes within the stated constraint.
24. Per-player hits (where each player independently must absorb one hit) do not exist in S-Layers. Angel actions always address the party as a unit or specify a target.

---

### States and Transitions

**Player Slot States**

| State | Condition | Effect |
|---|---|---|
| Active | ≥1 layer remaining | Accepts dice; current layer effect is live |
| Inert | 0 layers remaining | No dice placement; no effect; cannot be targeted for routing |

**Player Board States**

| State | Condition |
|---|---|
| Alive | ≥1 slot is Active |
| Destroyed | All 3 slots are Inert → triggers Win/Loss check |

**Angel Slot States**

| State | Condition |
|---|---|
| Active | Slot has ≥ 1 remaining layer; Slot Tracker Die present |
| Inert | All layers stripped; no actions fire from this slot; tracker removed |

**Angel Layer States (within an Active slot)**

| State | Condition |
|---|---|
| Exposed | Current top card of the slot column; HP and trigger set visible |
| Hidden | In stack beneath Exposed; face-down; not targetable |
| Stripped | Tracker met threshold; card removed; next Hidden becomes Exposed |

**Angel Board State**

| State | Condition |
|---|---|
| Active | ≥ 1 slot still Active |
| Cleared | All three slots (Head, Body, Weapon) Inert → triggers Win/Loss check |

---

### Interactions with Other Systems

| System | Interface | Notes |
|---|---|---|
| **Equipment Degradation** | Receives: "layer stripped" event (which slot, which player) | Degradation owns what happens when the new card is exposed — the reveal event and its consequences. Integrity triggers the event; it does not define the consequence. |
| **Win/Loss Conditions** | Sends: "player board destroyed" trigger; "Angel cleared" trigger | Integrity defines the conditions. Win/Loss owns what game-end means. |
| **Angel System** | Receives: hit count, hit type, and targeting constraints from Angel action text | Angel System owns each action's hit definition and all per-slot mechanics (slot trackers, HP calibration via WAS, multi-slot Cleared definition). Integrity System owns the routing rules applied to those hits. D_player_base and D_agg are consumed by Angel System as calibration anchors only. |
| **Dice Economy** | Sends: Accumulate totals are wiped on layer strip (rule 7). The Dice Economy GDD defines Accumulate totals as a property of the card; this rule applies that principle to the layer-strip event. | Cross-system fact: both GDDs must agree that totals are lost on strip. |
| **Item System** | Receives: restoration exception hook (rule 9). An item that restores a layer overrides the "inert is permanent" base rule. | Item System must print the exception explicitly on the card. |

## Formulas

*These formulas are design tools for Character System authors. None are computed at the table. Formula 4 (HP bounds for Angel layers) has been **superseded** by Angel System Formula 2 (WAS-based per-slot HP calibration) — see `angel-system.md`. D_player_base and D_agg remain as anchor constants consumed by the Angel System.*

**Baseline constant (this GDD):**
> **D_player_base = 8** — expected damage dealt by one player in one turn at honest play with basic equipment and items. Derived from 2D6 placed on damage slots with strategic routing (~3.5 per die × 2 dice = 7.0 base, +1 for expected item contribution). This is the calibration reference; actual in-session damage ranges 7–10 depending on build.

---

### Formula 1 — D_agg: Aggregate Party Damage per Turn

**The `D_agg` formula is defined as:**

`D_agg = n × D_player_base`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Party size | n | int | 1–4 | Number of active players |
| Per-player damage | D_player_base | float | 7–10 | Expected damage per player per turn (calibration baseline = 8) |
| Aggregate damage | D_agg | float | 7–40 | Total expected damage the party deals to the Angel layer in one turn |

**Output range:** D_agg ∈ [7, 40] across party sizes 1–4 at the calibration baseline.

**Reference table:**

| Party size | D_agg (baseline) | D_agg (conservative, no items) |
|---|---|---|
| 1 player | 8 | 7 |
| 2 players (MVP) | 16 | 14 |
| 3 players | 24 | 21 |
| 4 players (full) | 32 | 28 |

**Example:** MVP party (n=2): `D_agg = 2 × 8 = 16`. A layer with HP = 16 is expected to be stripped in approximately 1 turn at MVP.

---

### Formula 2 — T_angel: Expected Turns to Strip One Angel Layer

**The `T_angel` formula is defined as:**

`T_angel = 1 / P(D_turn ≥ HP_L)`

where `P(D_turn ≥ HP_L)` is the probability that the party's actual damage output in one turn meets or exceeds the layer's HP. T_angel is the mean of a geometric distribution.

For design calibration, the relationship between HP_L and D_agg governs the band:

| HP_L relative to D_agg | Design intent | Approx. T_angel |
|---|---|---|
| HP_L ≤ 0.6 × D_agg | Trivial — strips on almost every turn | < 1.2 turns |
| HP_L ≈ 0.7–0.9 × D_agg | Easy layer — reliably strips in 1 turn | ~1.1–1.4 turns |
| HP_L ≈ D_agg | Target band — strips in ~1–1.5 turns | ~1.3–1.7 turns |
| HP_L ≈ 1.1–1.2 × D_agg | Hard layer — may require 2 turns | ~2–3 turns |
| HP_L > 1.2 × D_agg | Degenerate ceiling — see HP bounds below | > 3 turns |

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Layer HP | HP_L | int | 1–∞ | HP value printed on the Angel layer card |
| Aggregate damage | D_agg | float | see Formula 1 | Expected party damage per turn |
| Strip probability | P(D_turn ≥ HP_L) | float | (0.0, 1.0] | Probability that one turn's output strips the layer |
| Expected turns | T_angel | float | 1.0–∞ | Expected turns to strip this layer |

**Output range:** T_angel ≥ 1.0. Values > 3 are degenerate for the 5–8 minute encounter target.

**Example:** MVP party (D_agg = 16). HP_L = 14: HP_L / D_agg = 0.875 — easy layer, T_angel ≈ 1.2–1.4 turns. HP_L = 16 — target band, T_angel ≈ 1.5 turns. HP_L = 19 — hard layer (near HP_max), T_angel ≈ 2.5–3 turns.

---

### Formula 3 — T_board: Expected Turns Until Player Board Destroyed

**The `T_board` formula is defined as:**

`T_board = L_total / H_pt`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Total layers | L_total | int | 3–∞ | Sum of layers across all three slots on one player's board (Character System defines per-character values) |
| Hits per turn | H_pt | float | 0.5–3.0 | Expected hits received by this player per turn, from Angel action design and party routing |
| Turns to destroy | T_board | float | 1.0–∞ | Expected turns until this player's board is destroyed |

**Output range:** T_board ≥ 1.0. A viable board must satisfy `T_board ≥ encounter length in turns` under worst-case routing.

**Example:** A character with L_total = 6 (2 layers per slot) facing H_pt = 1.0: `T_board = 6 / 1.0 = 6 turns` — survives a 6-turn encounter. Same character with H_pt = 2.0: `T_board = 6 / 2.0 = 3 turns` — destroyed by turn 3.

**Note on H_pt:** Evaluate T_board at both average routing (hits spread across the party) and worst-case routing (all hits directed at one player). Character System designers must verify both.

---

### ~~Formula 4 — HP Bounds for Angel Layer Design~~ *(DEPRECATED 2026-05-27)*

> **Removed.** Superseded by Angel System Formula 2 (WAS-based per-slot HP calibration). HP_alpha (0.6) and HP_beta (1.2) no longer apply. Angel slot HP ∈ [4, 14]; see `angel-system.md` Formula 2.

## Edge Cases

*Format: **If [condition]**: [exact outcome].*

---

**Damage Resolution (Angel Layer)**

- **If D_turn exactly equals HP_L (damage total equals the layer's HP precisely)**: the layer is stripped. The check is `total ≥ HP_L`; equality is a passing condition, not a tie.

- **If D_turn = 0 (no player effects deal damage this turn)**: the damage tracker stays at zero. The end-of-phase check fires (0 ≥ HP_L is false for any valid layer), the layer survives, and the tracker resets to zero. No state change occurs.

- **If total party damage exceeds 6 and the tracking die cannot advance further**: use a second spare die alongside the first as a tens-digit die. Display 14 damage as: tens die = 1, units die = 4. Both dice reset to zero at end of Effect Resolution Phase.

- **If the damage tracker value is disputed mid-phase (die moved, misread, or unclear)**: revert to the last value both players agreed on. If no agreement is possible, use the lower of the two claimed values. This is conservative and avoids unfair layer strips.

- **If a card effect deals damage to the Angel outside the Effect Resolution Phase (e.g., a start-of-turn triggered effect)**: that damage is added to the current turn's tracker and counted in the end-of-phase check. The no-carryover rule resets the tracker at the end of the phase — not at the start of the turn.

---

**Hit Routing (Player Layers)**

- **If a Targeted hit specifies a slot that is inert (zero layers remaining)**: the hit is wasted. It cannot be rerouted to another slot. The Angel action proceeds; no integrity damage is dealt for this hit.

- **If a Targeted hit specifies a slot that becomes inert earlier in the same multi-hit sequence**: the hit is wasted for the same reason. Eligibility is evaluated at moment of assignment; the slot is inert at that moment.

- **If a Targeted hit specifies "the player with the fewest layers remaining" and two or more players are tied**: the tie resolution is owned by the **Communication Protocol GDD**. This GDD flags the dependency; the table's tiebreaker is defined there.

- **If a Targeted hit specifies "another player" and only one player is present (party size = 1)**: the hit is skipped. Solo play is an unsupported configuration (Anti-Pillar: "Cooperative dynamic is load-bearing, not optional"). No ruling is defined for solo play.

---

**Layer Strip and Stripped Card State**

- **If a player slot's layer is stripped**: the removed card is placed face-down beside the player's board in a per-slot discard pile. Stripped cards from the Head slot form one pile, Body another, Weapon another. Piles are accessible for restoration effects.

- **If a restoration effect targets an inert slot and specifies "restore a layer to [slot type]"**: the top card from that slot's face-down discard pile is returned to the slot, face-up. The slot is immediately active. The restored card's effects are live from the next player turn onward. Its Accumulate total starts at zero.

- **If a restoration effect fires during the Effect Resolution Phase**: the restored slot becomes active immediately. However, since no die was placed on it during the current placement phase, it fires no die effects this phase. It is eligible for Angel hit routing during the subsequent Angel Resolution Phase (Angel hit eligibility uses current state at moment of routing, not phase-start-locked state).

- **If a restoration effect specifies "restore to full" but only a partial discard pile exists**: restore only the top card from the pile — one layer per restoration instance. "Restore to full" means restore the most-recently-stripped layer, not the entire original stack.

- **If two restoration effects target the same inert slot in the same phase**: the first effect restores 1 layer (slot is now active). The second restoration, if its text says "restore a layer to an inert slot," finds the slot is no longer inert and is wasted. If the second effect says "restore 1 additional layer to [slot type]" without requiring inert state, it stacks and adds a second layer.

- **If a slot with a running Accumulate total is stripped during the Effect Resolution Phase**: the die placed on that slot still resolves (slot state locked at phase start). The Accumulate total advances, and if the threshold is crossed, the Accumulate effect fires. After the phase ends, the slot is inert and the Accumulate total is lost. There is no contradiction: the effect fires, then the slot becomes inert.

---

**Angel Design Errors (Prevention Rules)**

- **If an Angel card has HP_L = 0 on any layer**: this is an illegal card. Per Angel System bounds, HP_slot_Lk ∈ [4, 14]. Flag as a card-vetting error. At the table, treat it as HP_L = 4 (Angel System HP floor).

- **If an Angel enters play with zero layers (malformed card, all layers missing)**: treat the Angel as immediately cleared. No encounter occurs. Advance to the next Event card. Award no victory progress. Log as a physical prototype defect.

## Dependencies

**Upstream dependencies**: None. The Integrity System is a Foundation-layer system. It does not depend on any other system in S-Layers.

**Downstream dependents** (systems that depend on this one):

| System | Dependency Type | What They Consume |
|---|---|---|
| **Equipment Degradation** | Hard | The "layer stripped" event (trigger + which slot, which player). Degradation owns the reveal consequence; Integrity owns the trigger. |
| **Win/Loss Conditions** | Hard | "Player board destroyed" trigger (all 3 slots inert) and "Angel cleared" trigger (all 3 Angel slots Inert). Win/Loss owns what game-end means; Integrity defines the conditions. |
| **Angel System** | Hard | Hit count, hit type (party/targeted), and targeting constraints produced by Angel action text. Angel System owns what each slot/face does and all per-slot HP calibration (WAS-based Formula 2); Integrity owns the routing rules applied to those hits. D_player_base and D_agg are consumed by Angel System as calibration anchors; HP bounds formula (HP_min/HP_max) no longer governs Angel slot HP. |
| **Character System** | Hard | Layer count per slot (L_total per player) — defined on the character board. Integrity System specifies that layer counts are per-character; Character System sets the actual values. Also consumes: T_board formula to verify board survivability. |
| **Item System** | Soft | Restoration effect hook (rule 9) — an item may restore a stripped layer if its card text explicitly says so. Item System must use the face-down discard pile model defined here. |
| **Combat System** | Hard | The full integrity resolution sequence (hit routing in Angel Resolution Phase, damage check in Effect Resolution Phase) is part of the turn flow that Combat System orchestrates. Combat owns the phase sequence; Integrity owns the rules within each phase. |

**Bidirectional consistency notes**:
- When the **Equipment Degradation GDD** is authored, it must list the Integrity System as a dependency and reference the "layer stripped" event as its trigger.
- When the **Win/Loss Conditions GDD** is authored, it must reference "player board destroyed" and "Angel cleared" as defined here.
- The **Angel System GDD** lists Integrity System as a dependency for D_player_base, D_agg, and hit type definitions. Angel slot HP is calibrated by Angel System's own WAS-based Formula 2 — not by the HP bounds formula in this GDD. *(2026-05-27 update: HP bounds formula superseded.)*
- When the **Character System GDD** is authored, it must list the Integrity System as a dependency and verify T_board for each character at both average and worst-case routing.

## Tuning Knobs

**1. D_player_base (Calibration Baseline: 8)**
- **Adjusts**: The effective difficulty of all Angel layer HP values across the whole game
- **Safe range**: 7–10 (conservative no-items baseline to strong-synergy baseline)
- **How to change**: If playtest data shows layers consistently strip in < 1.2 turns (too easy), lower the HP design assumption. If layers survive 4+ turns, raise it.
- **Impact**: Every Angel HP value in the Angel System GDD must be re-derived if this assumption shifts. This is the highest-leverage knob in the system.
- **Note**: D_player_base is an authoring-time design constant. It is not tracked at the table.

**~~2. HP Trivial-Floor Coefficient (α = 0.6)~~** *(SUPERSEDED 2026-05-27 — Angel System now owns HP calibration via WAS-based Formula 2. HP_alpha does not apply to Angel slot HP. Players have no HP. Remove from design tooling.)*

**~~3. HP Hard-Ceiling Coefficient (β = 1.2)~~** *(SUPERSEDED 2026-05-27 — same reason as above. HP_beta does not apply to Angel slot HP. Angel slot HP is calibrated by protection level relative to WAS_inner — see Angel System Tuning Knobs 1–3.)*

**4. Player Layer Count per Slot (L_total — owned by Character System)**
- **Adjusts**: How long a player board survives under hit pressure; how often the reveal drama occurs
- **Safe range**: 2–4 layers per slot (L_total = 6–12 per player board)
- **At 2 layers per slot**: Fragile — reveals happen frequently; build changes rapidly
- **At 3 layers per slot**: Standard — board survives a full encounter under moderate hit pressure
- **At 4 layers per slot**: Robust — less frequent reveal drama; higher component count
- **Owned by**: Character System GDD (actual values). Integrity System provides the T_board formula to verify each character.

**5. Angel Layer Count per Angel (owned by Angel System)**
- **Adjusts**: Total encounter duration and number of "wall breaks" per fight
- **Safe range**: 1–4 layers per Angel
- **At 1 layer**: Single threshold; high-stakes one-shot encounter
- **At 3 layers**: Standard — three phases of encounter with escalating feel
- **At 4+ layers**: Long encounter; requires low HP values per layer to stay within 5–8 minutes
- **Owned by**: Angel System GDD (actual values). Integrity System provides T_angel per layer for verification.

**6. No-Carryover Rule (LOCKED)**
- **Current state**: Damage against an Angel layer resets to zero each turn.
- **If changed**: Enabling carryover fundamentally alters T_angel and breaks the "wall, not a drain" feel. Changing this requires recalibrating every Angel HP value in the game.
- **Do not change without full formula re-derivation.**

## Visual/Audio Requirements

*Not applicable — Foundation layer system. Physical board game; no digital audio or visual rendering. Physical component requirements (spare dice for damage tracking, per-slot discard piles beside player boards) are covered in Detailed Design and Dependencies.*

## UI Requirements

*Not applicable — Foundation layer system. Player-facing information display (damage tracker die, layer discard piles, Angel HP face-up card) is a physical component design concern owned by the Art/Production pipeline.*

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [specific measurable outcome].*

---

**Player Integrity — Layer Removal**

- **AC-01** — GIVEN a player slot has 1 layer remaining, WHEN the party routes a hit to that slot, THEN the layer is removed and the slot becomes inert.

- **AC-13** — GIVEN a player slot has 3 layers remaining, WHEN the party routes a hit to it, THEN exactly one layer (the top card) is removed; the slot now has 2 layers remaining and remains active.

**Player Integrity — Routing Rules**

- **AC-02** — GIVEN a player slot is inert, WHEN the party attempts to route a hit to it, THEN the routing is illegal and a legal slot must be chosen instead.

- **AC-03** — GIVEN a player slot is inert, WHEN a player attempts to place a die on it, THEN the placement is illegal.

- **AC-05** — GIVEN an Angel action deals 2 hits, WHEN the first hit empties the last layer of a slot, THEN the second hit cannot be routed to that now-inert slot.

**Player Integrity — Board Destruction**

- **AC-04** — GIVEN a player board has exactly one slot with 1 layer remaining and the other two are inert, WHEN the party routes a hit to the remaining active slot, THEN that slot becomes inert, the board state is marked Destroyed, and the Win/Loss Conditions system is notified.

**Player Integrity — Slot State During Resolution**

- **AC-15** — GIVEN a player slot is inert and no card in play explicitly reads "restore a layer to [slot type]", WHEN any player attempts to restore a layer to that slot, THEN the restoration is illegal and the slot remains inert.

- **AC-16** — GIVEN a player has placed a die on a slot during Placement Phase, and that slot's layer is stripped during Effect Resolution Phase by another card's effect, WHEN that die's turn to resolve arrives in the same phase, THEN its effect fires normally. The inert state applies from the next turn forward.

- **AC-17** — GIVEN a player has placed dice on multiple slots and their board is destroyed during Effect Resolution Phase, WHEN board destruction is triggered, THEN all remaining placed dice on that board still fire their effects before board destruction is processed.

**Player Integrity — Accumulate Interaction**

- **AC-12** — GIVEN a slot has an Accumulate running total of 5 and its layer is stripped during Effect Resolution Phase, WHEN the die placed on it this phase resolves, THEN the Accumulate total advances normally and the threshold fires if met.

- **AC-14** — GIVEN a player slot has a running Accumulate total and its top layer is stripped, WHEN the new layer is revealed, THEN the Accumulate total on the new layer begins at zero regardless of the previous layer's total.

**Stripped Card Physical State**

- **AC-11** — GIVEN a player layer is stripped, WHEN the card is removed from the slot, THEN it is placed face-down beside the player board in a per-slot discard pile (Head / Body / Weapon piles kept separate).

**Angel Integrity — Damage Aggregation**

- **AC-06** — GIVEN an Angel layer has HP = 16 and the party deals exactly 16 total damage in one turn, WHEN the end-of-phase check fires, THEN the layer is stripped.

- **AC-07** — GIVEN an Angel layer has HP = 16 and the party deals 15 total damage, WHEN the end-of-phase check fires, THEN the layer survives and the tracker resets to 0.

- **AC-08** — GIVEN an Angel layer has HP = 12 and the party deals 20 total damage in one turn, WHEN the end-of-phase check fires, THEN the layer is stripped, the tracker resets to zero, and the newly exposed layer retains its full printed HP value unmodified.

- **AC-09** — GIVEN the party deals 15 damage in turn 1 (layer survives), WHEN turn 2 begins, THEN the damage tracker starts at 0 (no carryover from turn 1).

- **AC-21** — GIVEN an Angel layer has any HP value and no player effect deals damage this turn, WHEN the end-of-phase check fires, THEN the layer survives, the tracker reads zero, and the tracker resets to zero. No state change to the Angel layer.

**Angel Integrity — Layer Transition Timing**

- **AC-18** — GIVEN an Angel layer is stripped during Effect Resolution Phase, WHEN any card effect that resolves later in the same phase references "the current Angel layer", THEN it refers to the layer active at the start of that phase, not the newly exposed layer.

- **AC-19** — GIVEN an Angel has exactly one layer remaining with HP = 10 and the party deals 10 or more total damage, WHEN the end-of-phase check fires, THEN the final layer is stripped, the Angel is marked Cleared, and the Win/Loss Conditions system is notified.

**Angel Integrity — Hit Targeting**

- **AC-20** — GIVEN an Angel action deals a Targeted hit specifying a slot that is inert, WHEN the hit resolves, THEN the hit is wasted — it cannot be rerouted — and no integrity damage is dealt. Remaining hits in the action continue normally.

- **AC-24a** — GIVEN an Angel action text reads "Party hit: deal 2 hits", WHEN the hits are resolved, THEN the party may freely route each hit to any slot on any player board with at least one layer remaining.

- **AC-24b** — GIVEN an Angel action text specifies a targeting constraint (e.g., "Head slot only"), WHEN the hit is resolved, THEN routing outside the stated constraint is illegal.

**Damage Tracking**

- **AC-10** — GIVEN the running damage total reaches 7 or higher in one phase, WHEN the tester advances the tracking die past 6, THEN a second spare die is placed alongside it as a tens digit, and the two-die combination correctly displays the total (e.g., 14 = tens die shows 1, units die shows 4). Both dice reset to zero at end of Effect Resolution Phase.

- **AC-22** — GIVEN the running damage total is disputed mid-phase and two different values are claimed, WHEN the party cannot reach consensus, THEN the lower of the two claimed values is used for the end-of-phase check.

**Restoration**

- **AC-23** — GIVEN a slot is inert at the start of Effect Resolution Phase and a restoration effect fires during that phase, WHEN the restoration resolves, THEN the slot is immediately active; it fires no die effects this phase (no die was placed). The restored slot IS eligible for Angel hit routing in the subsequent Angel Resolution Phase of the same round.

**No-Carryover Near-Miss Bound**

- **AC-25** *(DEFERRED — playtest data required)* — GIVEN MVP prototype play (2 players, D_player_base ≈ 8, Angel layer HP in the range [10, 19]), WHEN the party's damage total falls short of the Angel layer's HP in a turn (layer survives), THEN the shortfall (HP_L − D_turn) is ≤2 in fewer than 30% of such "failed" turns. If this rate is exceeded in playtest, the no-carryover rule should be re-evaluated and a partial-credit tuning lever (e.g., "damage within 2 of threshold is carried over to the next turn only") may be introduced. Document playtest observations in Open Questions.

## Open Questions

*None identified at time of authoring. Add during playtesting if new edge cases or balance questions arise.*
