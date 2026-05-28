# Angel System

> **Status**: Needs Revision — Structural redesign in progress (2026-05-27)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-27
> **Implements Pillar**: Agency Over Dice (primary); Fast and Focused (secondary); Cooperative Ownership (supporting)
> **Redesign note**: 2026-05-27 — Monolithic layer-stack model replaced with 3-slot board architecture (Head/Body/Weapon) mirroring player board. Per-slot HP, per-slot damage trackers, escalating trigger sets per layer, party-size scaling via layer count and angel count. All formulas, ACs, tuning knobs, and edge cases rewritten.

## Overview

The Angel System defines the adversary side of every combat encounter in S-Layers. Each Angel is represented by a physical board: a stack of **layer cards**, each carrying its own **face map** (six die faces, each mapped to a unique action for that layer) and an explicit **HP value**. The number of layers varies by encounter type — from a single-layer skirmish Angel to a four-layer boss — and as the party strips each layer by meeting its HP threshold, the Angel's face map changes: the outer layer's calculated cruelty gives way to whatever is underneath. The Angel rolls three six-sided dice each round; the middle die (the 3D6 median roll defined by the Dice Economy) determines which action fires. This result is revealed to all players **before** the placement phase begins — the Angel is not a random hazard but a known, committed threat whose next move is visible and whose consequences the party must plan around or absorb. Actions include party hits, targeted hits, and non-damage effects; the mix is unique per Angel per layer. The Angel System owns the board structure, the face-map assignments, the HP values on each layer, and the enemy action resolution order. It does not define how players respond to hits (Integrity System owns routing), how dice are placed to deal damage (Equipment System), or how the turn phases sequence (Combat System).

## Player Fantasy

An Angel is not a monster. A monster can be killed and forgotten. An Angel is an *instrument* — something Heaven built and posted to hold this door, this stair, this hour against you. It rolls its three dice in plain sight and shows you what it will do before a single one of yours has left your hand: foreknowledge offered as a courtesy, because the outcome was never in doubt. Your work is not to be surprised. Your work is to look at what was promised, and route the harm through the bodies and the gear and the dice you brought, until the instrument is quieter than it was. Then you climb to the next door.

The layer beneath is not weaker. It is different. A new face map, a new geometry of actions — the Angel does not diminish as it is damaged; it *changes*. Stripping a layer is not a victory lap. It is a revelation. You learn something about the instrument. It teaches you what it is made of. You were afraid of the outer face. Now you know there was something underneath, and it has been watching you since the encounter started.

**Design test**: If a player says — mid-encounter, after the Angel's middle die settles — *"okay, that's manageable"* and then begins telling their teammates exactly how to route the coming hits: the system has worked. The foreknowledge created decision, not helplessness. If the players react to the Angel's revealed action with confusion, silence, or resigned acceptance rather than active deliberation: the action is too complex (violating *Fast and Focused*) or the actions feel arbitrary rather than readable (violating this fantasy — the instrument must be legible). If stripping a layer produces no visible change in the party's strategy (they play the next round exactly the same way), the face-map escalation has failed to deliver.

## Detailed Design

### Core Rules

**1. Angel Board Physical Structure**

1. Each encounter places one or more **Angel boards** in a dedicated **Angel Zone** at the center of the table, visible to all players simultaneously.

2. Each Angel board has **three parallel slot columns**: **Head**, **Body**, and **Weapon** — left to right in that order. This mirrors the player board structure. Each column is physically distinct and independently managed.

3. Each slot column contains a **layer stack** of face-down cards, with the **Exposed layer** (current active layer) placed face-up at the top of its column. Hidden layers remain face-down beneath it.

   *Standard slot depth for a reference MVP angel: Head = 1 layer, Body = 2 layers, Weapon = 3 layers. The encounter card specifies exact layer counts for each Angel.*

4. Each slot column has its own dedicated **Slot Tracker Die** — a spare d6 placed below the column's Exposed layer. It tracks cumulative damage dealt to that slot during the current turn. If the running total exceeds 6, a second die serves as the tens digit.

5. **Slots are independent.** Damage tracked on the Head tracker never affects the Body or Weapon trackers. Each slot strips on its own terms, reveals its own next layer, and becomes inert on its own timeline.

6. The number of Angels in play is determined by the encounter event card, scaled to party size. Each encounter card states: *"1–2 players: [N] Angel(s). 3–4 players: [M] Angel(s)."*

7. **All Angels in an encounter share one 3D6 roll per round.** A single set of three dice is rolled in the Angel Roll Phase. Every active slot on every Angel reads the same median face value against its own face map.

8. When a slot's last layer is stripped, that slot is **Inert**. The column remains at the table (for reference) but is marked inert — its actions no longer fire on any die result. The Slot Tracker Die is removed.

9. When all three slots of an Angel are Inert, that Angel is **Cleared** → Win/Loss check fires immediately.

*Physical note (downstream requirement for component design):* The Component Design spec must accommodate the 3-column layout at standard play distance. Required: each slot column's HP value and trigger faces must be table-readable at 60–100 cm; each Slot Tracker Die must be unambiguously associated with its column. Reference player board layout as the visual precedent.

---

**2. Slot Card Anatomy**

Each Angel slot consists of **layer cards** stacked within the slot column. Each layer card contains:

| Field | Description | Visibility |
|---|---|---|
| **Slot Type** | Printed prominently: HEAD / BODY / WEAPON. Identifies the column this card belongs to. | Always visible (card back) |
| **Trigger Faces** | The die face values (subset of 1–6) that activate this slot's action. Example: "2 · 4 · 5". Faces not listed produce no action from this slot. | Face-up when Exposed; face-down when Hidden |
| **Action** | What fires when a trigger face shows: one action entry using HIT / SRG / PRS / BUFF codes. ≤ 8 words. | Face-up when Exposed; face-down when Hidden |
| **HP** | Integer. Printed prominently. The damage threshold this slot layer requires to strip. | Face-up when Exposed; face-down when Hidden |
| **Layer Index** | Ordinal label: "L1 / 3", "L2 / 3", etc. Printed on both card front and card back. | Always visible |
| **Angel Name / Tag** | Confirms which Angel this card belongs to. | Face-up when Exposed |
| **Target Party Size** | The party size range this layer was balanced for: "1–2" or "3–4". | Face-up when Exposed |

**Key structural rule:** Each slot column's layer cards share the same **Slot Type** (Head, Body, or Weapon) and are stacked in layer order (L1 on top, highest L at the bottom). Each layer within a slot may have a different action and a different HP value — deeper layers escalate in both threat and durability.

**Trigger set notation:** A slot card lists only its active trigger faces. A face unlisted is silent — it produces no action from that slot. Multiple slots may share a trigger face (e.g., both Body and Weapon trigger on face 4): when the die shows 4, both slots fire their respective actions simultaneously.

---

**3. MVP Action Taxonomy**

All face map entries use exactly one of these four action categories. No other action types are valid for MVP.

| Category | Code | What It Does | When It Fires |
|---|---|---|---|
| **Hit** | HIT | Deals 1 or 2 hits. Type is stated: **Party hit** (freely routed per Integrity System) or **Targeted hit** (routing constrained by printed text). | Angel Resolution Phase |
| **Surge** | SRG | A HIT with an escalation clause. Base hits are stated; the clause adds hits if a condition is met (e.g., "if this is an inner layer", "if any player has fewer than 2 layers remaining"). Base hits resolve first; escalation is evaluated and resolved after. | Angel Resolution Phase |
| **Pressure** | PRS | Imposes a constraint on player die placement for the current Effect Resolution Phase. Announced at phase start, before any dice resolve. Lifted at phase end. | Announced start of Effect Resolution Phase |
| **Self-Buff** | BUFF | Raises the effective strip threshold for this turn's damage check only. The BUFF value (e.g., "+4") is added to HP_L for the current turn only. Announced at phase start, before any dice resolve. HP_L printed on the card is unchanged. **BUFF changes the threshold — it does not reduce or prevent player damage.** | Announced start of Effect Resolution Phase |

---

**4. Slot Design Rules**

**Rule 4.1 — Each slot layer has one action type.** A layer card's action field contains exactly one action entry (HIT, SRG, PRS, or BUFF). The trigger faces determine *when* it fires; the action determines *what* it does. A slot with a HIT action always deals the same hit — the frequency of threat comes from how many faces are in its trigger set.

**Rule 4.2 — At least one slot per Angel must carry HIT or SRG.** An Angel with no damage-dealing slot is not a credible obstacle.

**Rule 4.3 — Trigger set design and frequency.** The trigger faces assigned to a slot determine `F_slot` — the probability the slot fires on any given roll:

`F_slot = Σ P_angel(i)` for i in the slot's trigger set

| Trigger Set Example | Faces | F_slot | Design Role |
|---|---|---|---|
| {1, 6} | 2 faces | ≈ 0.148 | Rare — high drama when it fires; slot can carry SRG or high-severity action |
| {3, 4} | 2 common faces | ≈ 0.482 | Moderate — reliable threat; mid-weight action |
| {2, 4, 5} | 3 faces | ≈ 0.611 | Frequent — steady pressure; calibrate WAS carefully |
| {1, 2, 5, 6} | 4 faces | ≈ 0.518 | Moderate-high; wide spread across rarity bands |

**Rule 4.4 — Shared trigger faces create compound rounds.** If a face value appears in two or more slots' trigger sets, all matching active slots fire simultaneously when that face shows. Encounter designers must check compound-fire rounds (same die value → multiple actions) for WAS_total to stay within band. A face that triggers both a HIT and a SRG simultaneously is high severity and must be weighted accordingly.

**Rule 4.5 — Severity calibration (per slot).** Use the same severity scale as the action taxonomy. The **Slot Weighted Average Severity (WAS_slot)** is:

`WAS_slot = F_slot × S_slot`

where `S_slot` is the severity score of the slot's action.

**Total Angel WAS:** `WAS_total = Σ WAS_slot` across all active slots.

| WAS_total Band | Design Intent |
|---|---|
| < 1.5 | Passive — light pressure. Valid for a skirmish or outer-layer encounter. |
| 1.5–2.5 | Balanced — sustained threat. Target for most standard MVP encounters. |
| 2.5–3.5 | Aggressive — valid for inner-layer encounters and boss faces. |
| > 3.5 | Degenerate — party cannot plan. Do not publish. |

**Rule 4.6 — Rare trigger sets may carry high-severity actions.** Faces with F_slot < 0.2 (e.g., trigger set {1, 6}) may carry SRG or Severity 4–5 actions, provided WAS_total stays in band. High severity on a rare trigger is dramatic, not degenerate.

**Rule 4.7 — BUFF actions must carry a guaranteed secondary component.** Every slot layer with a BUFF action must also include a secondary component that fires unconditionally (a PRS constraint or a HIT/SRG). A BUFF entry with no secondary component is an **illegal slot card** — the secondary ensures every BUFF trigger creates a table event even when the party is not targeting that slot.

**Action economy arc.** As slots become Inert, WAS_total decreases: the angel becomes measurably less dangerous as the party strips it. Encounter designers should set outer layers (first encountered) to have the highest combined fire frequency, so early rounds feel most threatening. Inner layers can compensate with higher severity-per-fire.

---

**5. Action Resolution Order**

**Rule 5.1 — PRS and BUFF fire at the start of the Effect Resolution Phase** — before any player dice resolve. For each active slot whose trigger set includes the committed face, check: if that slot's action is PRS or BUFF, announce it now.

- Announce all active-slot BUFF values first (effective threshold raised per slot).
- Announce all active-slot PRS constraints second (placement restrictions applied).
- Players see all constraints and modified thresholds before any placement resolution begins.

**Rule 5.2 — Multiple-slot BUFF/PRS at phase start.** If two slots both trigger on the same face and both carry BUFF or PRS:

- All BUFFs are announced before any PRS.
- Within each category, announce in slot order: Head → Body → Weapon.
- All effects are simultaneously in force once announced.

**Rule 5.3 — HIT and SRG fire in the Angel Resolution Phase** — after all player effects have resolved, per the Dice Economy's timing lock.

**Rule 5.4 — Multiple-slot HIT/SRG in the Angel Resolution Phase.** All slots with HIT or SRG that triggered this round resolve in slot order: **Head → Body → Weapon**. If both slots carry HIT, Head's hits are fully routed before Body's hits begin.

**Rule 5.5 — Hit routing is party-chosen.** Each HIT or SRG effect names hit count and type. The party decides routing per Integrity System routing rules, independently for each slot's hits.

**Rule 5.6 — Surge sequence.** Base hits for a SRG resolve first. Then the escalation clause is evaluated. Additional hits fire immediately after, in party-chosen routing order.

**Rule 5.7 — Inert slots are silent.** If the committed face appears in an Inert slot's trigger set, that slot produces no action. The face is effectively safe for that slot's former threat. Inert slots are never announced.

**Rule 5.8 — Compound action sequence (BUFF + HIT on same slot).** If a slot layer card contains both BUFF and a secondary HIT/SRG (per Rule 4.7), components fire in printed order: BUFF announced at phase start; HIT/SRG resolves in the Angel Resolution Phase.

---

**6. Layer Transition Rules**

**Rule 6.1 — Damage targeting declaration.** When a player's damage effect fires, the active player declares both **which Angel** and **which slot** (Head, Body, or Weapon) receives that damage before advancing any Slot Tracker Die. A single effect's output cannot be split between two slots or two Angels — it fully targets one slot on one Angel.

**Rule 6.2 — Slot Tracker accumulation.** Damage accumulates in the targeted slot's tracker throughout the Effect Resolution Phase. All player damage to the same slot in the same turn pools together.

**Rule 6.3 — Strip trigger (per slot).** At the end of the Effect Resolution Phase, each slot independently checks: if `tracker total ≥ HP_L` for the Exposed layer (including any active BUFF on that slot) → the layer is stripped.

**Rule 6.4 — Physical strip procedure (per slot):**
1. Announce the slot tracker total.
2. Compare to effective threshold (`HP_L + BUFF_value` if a BUFF fired on this slot this turn).
3. **If total ≥ threshold:** Remove the Exposed layer card from the slot column. Place it face-up beside the Angel board (kept for reference).
4. Flip the next card in the slot column face-up. Its trigger faces, action, and HP are immediately visible.
5. **Reset the Slot Tracker Die to zero.**
6. **If no next card remains in this slot column:** The slot is **Inert** — remove the Slot Tracker Die.
7. **If total < threshold:** Layer survives. Reset Slot Tracker to zero. No state change.

**Rule 6.5 — No damage carryover (non-Penetrating).** When a layer strips, any damage above the threshold is **discarded**. It does not carry to the newly revealed next layer. Tracker resets to 0 regardless of surplus.

**Rule 6.6 — Penetrating exception (Character System Rule 11 — Pilgrim ability).** When a Penetrating damage event targets a slot: if the damage total meets or exceeds HP_L, the excess carries to the next layer of **the same slot only**. Sequential strip checks fire within the same phase, outermost layer first. On-destroy passives trigger in outermost-first order. Penetrating damage does **not** cross slot boundaries — excess stops at the slot boundary even under Penetrating. The Angel may have a slot fully Inerted in a single Penetrating event if accumulated damage exceeds all remaining layers' HP in that slot.

**Rule 6.7 — HP_eff on newly revealed layer (Penetrating cascade).** When a Penetrating cascade reveals a new layer mid-phase, HP_eff for that new layer is calculated fresh: `HP_eff = HP_L_new + BUFF_value_new` (BUFF for the new layer = 0 unless that slot's face triggered BUFF this turn).

**Rule 6.8 — Hidden-until-strip.** Hidden layer cards in a slot column are face-down until the moment the layer above them is stripped. Players cannot plan around the new layer before it is revealed.

**Rule 6.9 — Layer transition is a public event.** All players see the new layer card simultaneously at the moment of strip.

**Rule 6.10 — One strip check per slot per turn (non-Penetrating).** Outside of the Penetrating exception, each slot fires exactly one strip check at end of Effect Resolution Phase.

**Rule 6.11 — BUFF does not persist between turns.** If a slot's BUFF fires and the layer survives, next turn's threshold reverts to HP_L. BUFF does not stack across turns.

**Rule 6.12 — Angel Cleared check.** After all three slot strip checks resolve for this turn: if all slots of an Angel are Inert → the Angel is **Cleared** → Win/Loss check fires immediately, before the Angel Resolution Phase begins.

---

**7. Multi-Angel Rules**

**Rule 7.1 — All Angels share one 3D6 roll.** The single median face value is applied to every active slot on every Angel this round. One roll, read across all slot columns of all Angels.

**Rule 7.2 — Each slot reads its own trigger set.** The same median face may produce different results per slot and per Angel. A slot fires only if the median face is in its trigger set. Multiple slots across multiple Angels may fire simultaneously.

**Rule 7.3 — Damage is targeted per Angel per slot.** When a damage-dealing effect fires, the active player declares which Angel AND which slot receives that damage before advancing any tracker. A single effect's output cannot be split between slots or Angels.

**Rule 7.4 — Strip checks are independent per slot per Angel.** Each slot's tracker and strip check is fully independent. Both Angels' slots can strip in the same turn. Each Slot Tracker resets to zero independently after its check.

**Rule 7.5 — Announcement order at phase start (multiple Angels).** BUFF/PRS announcements at the start of the Effect Resolution Phase follow this order:
1. Angel A, all triggering slots (Head → Body → Weapon), BUFFs first then PRS
2. Angel B, all triggering slots (Head → Body → Weapon), BUFFs first then PRS
3. All constraints apply simultaneously once announced.

**Rule 7.6 — HIT/SRG resolution order (multiple Angels).** In the Angel Resolution Phase, resolve Angel A's triggering slots (Head → Body → Weapon), then Angel B's triggering slots (Head → Body → Weapon). Encounter-card order governs which Angel is "A". If order is not printed, the party declares it before the encounter begins — binding for the full encounter.

**Rule 7.7 — Encounter continues until all Angels are Cleared.** Clearing one Angel does not end the encounter. All slots of all Angels must be Inert.

**Rule 7.8 — Board destruction applies normally.** If any player's board is destroyed during a multi-Angel encounter, the Win/Loss check fires immediately, regardless of Angel states.

**Rule 7.9 — Multi-Angel Resolution Ritual.** When multiple Angels are in play, follow this named sequence before any placement begins:

1. The **Lead Player** reads the median die face aloud: *"The die shows [N]."*
2. For each Angel in order (A, B…), read each active slot that includes face N in its trigger set: *"Angel A — Head fires: [action]. Body fires: [action]. Weapon: silent."* Then repeat for Angel B.
3. All other players confirm or challenge before any placement occurs.
4. Once all firing slots are confirmed aloud by the table, proceed to placement.

The ritual is mandatory when 2 or more Angels are in play. Experienced groups may abbreviate it provided all players explicitly confirm before placement begins.

---

### States and Transitions

**Slot States**

| State | Condition | Trigger Set Active? | Tracker Present? |
|---|---|---|---|
| **Active** | Slot has ≥ 1 remaining layer card | Yes — current Exposed layer's trigger set and action apply | Yes |
| **Inert** | All layer cards in this slot have been stripped | No — die results never trigger this slot | No — tracker removed |

**Layer States (within an Active slot)**

| State | Condition | Action Readable? | HP Readable? |
|---|---|---|---|
| **Hidden** | In stack beneath the Exposed layer of this slot | No — face-down | No — face-down |
| **Exposed** | Current top card of the slot column | Yes | Yes |
| **Stripped** | Tracker met effective threshold; card removed to face-up reference pile | No | Yes — reference pile, face-up |

**Per-Turn Flow (Angel System scope)**

```
Angel Roll Phase
  → Roll 3D6. Sort low→high. Median face committed and revealed to all.
  → For each Angel, for each active slot: check if median face ∈ slot's trigger set.
  → Announce all triggering slots (Angel A: Head fires / Body fires / Weapon silent, etc.)

Start of Effect Resolution Phase
  → Announce all BUFF values from triggering slots (raise HP_eff for those slots).
  → Announce all PRS constraints from triggering slots (apply for this phase only).
  → Players resolve placed-die effects; damage target declared as [Angel + Slot] per effect.
  → Each slot's Slot Tracker Die advances as targeted damage accrues.

End of Effect Resolution Phase (strip checks — per slot, independent)
  → For each Angel, for each active slot:
       If tracker ≥ HP_eff (HP_L + active BUFF for this slot) → Stripped: remove card; next
       layer revealed; reset Slot Tracker to 0. If no next card: slot is Inert.
       If tracker < HP_eff → layer survives; reset Slot Tracker to 0.
  → Penetrating exception (if active): excess carries within same slot; cascade
       strip checks fire outermost-first within that slot.
  → Angel Cleared check: if all slots Inert → Cleared → Win/Loss check immediately.

Angel Resolution Phase
  → For each Angel in encounter order:
       For each active slot in slot order (Head → Body → Weapon):
         If slot triggered HIT this round: party routes hits per Integrity System.
         If slot triggered SRG: resolve base hits; evaluate escalation; resolve additional.
  → Round ends.
```

---

### Interactions with Other Systems

| System | Direction | Data / Event | Notes |
|---|---|---|---|
| **Dice Economy** | Inbound | Median face value from 3D6 roll (P_angel probability table) | Dice Economy owns the roll. Angel System reads each active slot's trigger set against the median. |
| **Integrity System** | Inbound | HP calibration baseline: D_player_base = 8, D_agg = 16 (n=2) | Old HP_min/HP_max bounds formula no longer governs per-slot HP. Per-slot HP is frequency-weighted (see Formula 2). The Integrity System's two-die tracking rule applies per Slot Tracker Die. |
| **Integrity System** | Inbound | Hit type definitions: Party hit / Targeted hit | Angel slot actions use exactly these two hit types. No other hit grammar is valid. |
| **Integrity System** | Outbound | Hit count and type per triggering slot action | Integrity System receives and applies routing rules. Angel System does not own routing. |
| **Integrity System** | Outbound | "Slot Inerted" event (which slot, which Angel) | When a slot strips its last layer. |
| **Integrity System** | Outbound | "Angel Cleared" event (per Angel) | When all 3 slots of an Angel are Inert. Win/Loss check fires via Integrity System. |
| **Combat System** | Inbound | Phase sequence (Angel Roll → Effect Resolution → Angel Resolution) | Angel System rules execute within Combat System's phase structure. Damage declaration step requires: declare Angel + slot, then advance tracker. |
| **Win/Loss Conditions** | Outbound | "Angel Cleared" trigger | Win/Loss owns the consequence. Cleared = all 3 slots Inert. |
| **Equipment System** | Indirect via PRS | PRS may restrict placement on specific player slot types (Head/Body/Weapon) for one turn | Equipment System must handle externally-imposed placement restrictions. PRS applies to player board slots — these share naming convention with Angel board slots but are structurally separate. |
| **Character System** | Inbound / Outbound | Penetrating (Pilgrim ability): within-slot cascade — excess carries to next layer of the same slot only | Character System Rule 11 defines Penetrating. Angel System Rule 6.6 governs how the cascade resolves within the slot. Cross-slot penetration does not exist. |
| **Character System** | Outbound | H_pt (expected hits per player per turn) derivable from slot WAS calculations | H_pt_total = (1/n) × Σ_i P_angel(i) × Σ_active_slots C_slot(i). Used by Character System for T_board verification. |

## Formulas

*Design tools for Angel card authors and encounter designers. None computed at the table. All derive from locked constants in the Dice Economy and Integrity System GDDs.*

**Locked upstream inputs:**

| Symbol | Value | Source GDD |
|---|---|---|
| P_angel(1), P_angel(6) | 0.074 | Dice Economy |
| P_angel(2), P_angel(5) | 0.185 | Dice Economy |
| P_angel(3), P_angel(4) | 0.241 | Dice Economy |
| D_player_base | 8 | Integrity System |
| D_agg(n) | n × 8 | Integrity System |

---

### Formula 1 — WAS_slot_Lk: Weighted Average Severity per Layer

Each layer card has its own trigger set and action. WAS is calculated per layer:

`WAS_slot_Lk = F_slot_Lk × S_slot_Lk`

where `F_slot_Lk = Σ P_angel(i)` for i in layer k's trigger set.

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Layer trigger probability | F_slot_Lk | float | [0.074, 0.963] | Sum of P_angel(i) for faces in this layer's trigger set |
| Layer severity score | S_slot_Lk | int | {0–5} | Severity of this layer's single action |
| Layer WAS | WAS_slot_Lk | float | [0, 5.0] | Expected severity contribution when this layer is Exposed |

**Severity scale:**

| Score | Action class | Example |
|---|---|---|
| 0 | Light BUFF or PRS | BUFF +2 |
| 1 | Significant PRS | "Head slots may not receive dice this turn" |
| 2 | HIT: 1 party hit | 1 freely-routed hit |
| 3 | HIT: 1 targeted hit or heavy BUFF | "Hit: player with fewest layers"; BUFF +5 |
| 4 | HIT: 2 party hits | 2 freely-routed hits |
| 5 | SRG | Escalating 2+ hits |

**Escalation design guideline (not a hard rule):** Inner layers are encouraged to escalate relative to the layer above — by widening the trigger set, increasing severity, or both. Any single escalation axis is valid; a layer that escalates only in frequency (same action, fires more often) is as legitimate as one that escalates only in severity (same trigger set, harder hit). Flat layers (identical trigger set and severity to the outer layer) are permitted but offer no revelation moment on strip.

**Example (Body slot, 2 layers):**
- L1: trigger {4}, action = HIT:1 party hit → F=0.241, S=2 → `WAS_body_L1 = 0.482`
- L2: trigger {2,4,5}, action = HIT:2 party hits → F=0.611, S=4 → `WAS_body_L2 = 2.444`

L2 is 5× more threatening than L1. Both axes escalated — this is the strongest revelation moment.

---

### Formula 2 — WAS_total and HP Calibration

**Total Angel WAS at a given encounter state:**

`WAS_total = Σ WAS_slot_Lk` for all active slots at their currently Exposed layer k.

WAS_total increases as the encounter progresses (inner layers exposed). Track WAS_total at each encounter state:

| Encounter State | WAS_total Target |
|---|---|
| All outer layers (L1) Exposed | 1.5 – 2.5 (opening pressure) |
| Mix of L1 and inner layers | 2.0 – 3.0 (mid-fight escalation) |
| All inner layers, all slots still active | ≤ 3.5 (hard but not degenerate) |

**HP calibration — severity-weighted, fixed at print:**

HP values are printed on cards and never change. Party-size scaling is handled entirely by encounter card design — not by different card variants or HP adjustments (see Formula 3).

Per-slot HP protection level is set by the slot's **inner layer WAS** (peak threat):

| Protection Level | Inner Layer WAS | HP per layer | Design Role |
|---|---|---|---|
| **High** | WAS_inner ≥ 2.5 | Outer: 7–10 HP, Inner: 10–14 HP | Hardest to silence; rewards sustained investment |
| **Medium** | WAS_inner 1.5–2.4 | Outer: 5–7 HP, Inner: 7–10 HP | Standard threat; focus opportunistically |
| **Low** | WAS_inner < 1.5 | Outer: 4–5 HP, Inner: 5–7 HP | Easiest to inert; quick action economy win |

**Layer depth HP escalation within a slot:**
- L1 (outer): `HP_L1 = protection_mid × 0.6` (rounded up to nearest integer)
- L2: `HP_L2 = protection_mid × 1.0`
- L3 (innermost, if present): `HP_L3 = protection_mid × 1.4`

*Where protection_mid is the midpoint of the slot's protection level HP bracket.*

**Output Range:** HP_slot_Lk ∈ [4, 14] for all published MVP cards.

---

### Formula 3 — Party-Size Scaling (Layers and Angels)

HP values on cards are fixed. Party-size scaling is achieved entirely through encounter card design — by specifying how many layer cards to place per slot, and how many Angels to deploy. No card variants or dynamic HP values are required.

**Layer count per slot (same card pool, different stack depth):**

| Party Size | Layers per Slot | Total Strip Events (1 angel) | Design Effect |
|---|---|---|---|
| n=2 | 2 layers | 6 | Standard encounter depth |
| n=3–4 | 3 layers | 9 | 1 additional inner layer per slot; encounter runs longer; escalation curve more pronounced |

**Angel count (additive complexity):**

| Party Size | Angels in Encounter | Effect |
|---|---|---|
| n=2 | 1 angel | Clean focus-fire decisions |
| n=3–4 | 1–2 angels (encounter-card choice) | More targets; damage-split decisions; coordination load increases |

**Encounter card format for party-size scaling:**

> *"n=2: 1 angel, 2 layers per slot.*
> *n=3–4: 1 angel, 3 layers per slot  OR  2 angels, 2 layers per slot."*

The encounter designer chooses which scaling lever to use:
- **Deeper slots** (more layers): encounter lasts longer; escalation curve more pronounced; same tactical decisions, more turns
- **More angels** (additional board): more simultaneous threats; damage-split decisions required; higher coordination overhead

At n=3–4 with 3 layers per slot, the L3 card is pulled from the same angel card set as L1 and L2. No additional card variants are needed.

---

### Formula 4 — H_pt: Expected Hits per Player per Turn

`H_pt = (1/n) × Σ_{i=1}^{6} P_angel(i) × Σ_{active slots s} C_s_Lk(i)`

where `C_s_Lk(i) = [hit count of slot s's Exposed layer action if i ∈ that layer's trigger set; else 0]`

**Deriving C_s_Lk(i):**

| Action | C value |
|---|---|
| BUFF or PRS | 0 |
| HIT: 1 party hit | 1 |
| HIT: 2 party hits | 2 |
| SRG | base_hits + P(escalation) × escalation_hits |

**H_pt arc:** H_pt increases as inner layers expose (wider triggers + higher severity). H_pt decreases only when a slot becomes Inert. The encounter has a natural pressure curve: low opening → escalating mid-fight → relief only when slots are fully silenced.

**Fire rate stability check:** For each slot-inert scenario (one slot gone, others at inner layers), verify H_pt_total ≥ 0.3. If H_pt drops below this threshold, the encounter risks dead turns — the angel produces no meaningful threat. Remedy: add 1 trigger face to a remaining slot's inner layer, or increase that layer's severity to S≥2.

---

### Formula 5 — BUFF Effective Threshold (per slot)

`HP_eff_slot = HP_slot_Lk + BUFF_value`

Strip check fires if: `D_slot_turn ≥ HP_eff_slot`

**Safe BUFF range:** `BUFF_value ≤ D_agg(n) − HP_slot_Lk`

*(Ensures HP_eff never exceeds D_agg — stripping always remains possible with full cooperative focus at the given party size.)*

**Example (n=2):** Weapon slot L2, HP=12, face {4} carries BUFF+3 → HP_eff=15 ≤ D_agg(2)=16 ✓. On BUFF turns the party needs 15 damage to this slot; on non-BUFF turns 12 suffices.

## Edge Cases

*Format: **If [condition]**: [exact outcome]. Rationale provided where non-obvious.*

---

### Strip Check Edge Cases

- **If the Slot Tracker total exactly equals HP_eff for a slot**: the layer is stripped. The strip condition is `D_slot_turn ≥ HP_eff`; equality is a passing condition.

- **If a BUFF fires on a slot and the tracker already equals HP_slot_L at the moment of announcement (the party would have stripped without BUFF)**: the BUFF raises the threshold to HP_eff = HP_slot_L + BUFF_value. The strip check fires against HP_eff only. If the tracker reads HP_slot_L and HP_eff > HP_slot_L, the layer survives this turn. BUFF announcements happen at the start of the Effect Resolution Phase before any player dice resolve — the new threshold is in force for the entire phase.

- **If a BUFF announcement for a slot was missed and only discovered after player dice have resolved**: reconstruct and replay the Effect Resolution Phase for that slot with BUFF in force if the physical state can be recovered. If not recoverable, compare the tracker value at the moment of discovery against HP_eff. This produces the most conservative outcome for the party.

- **If the Slot Tracker Die is misread mid-phase**: revert to the last value both players agreed on. If no agreed prior value exists, use the lower of the two claimed values.

- **If the party deals damage to a slot but does not declare the target slot before advancing the tracker**: declare immediately before advancing. If already advanced in error, reassign the tracker advancement to the declared target; do not undo other resolved effects.

---

### Damage Targeting Edge Cases

- **If a player attempts to declare a slot that is already Inert as the damage target**: the declaration is invalid. The player must redirect to an active slot before advancing any tracker. If no active slot exists on that Angel, the Angel is already Cleared — this targeting event should not occur.

- **If two players simultaneously declare different target slots for the same damage effect**: a single damage effect has one source. The active player who controls the effect makes the final declaration. Resolve immediately before advancing any tracker.

---

### Layer Transition Edge Cases

- **If the stripped layer is the last in a slot AND that slot has a HIT or SRG committed for the Angel Resolution Phase of the same round**: the slot is now Inert — the HIT/SRG is **cancelled**. A slot cannot fire in the Angel Resolution Phase if it is Inert. Cancelled hits do not reroute to another slot.

- **If the newly revealed layer's trigger set includes the same face that just fired on the stripped layer**: the new layer does NOT fire this round. The angel's action for this round was determined at the Angel Roll Phase using the Exposed layer at that time. The new layer's trigger set becomes active from the next Angel Roll Phase onward.

- **If all three slots of an Angel become Inert in the same turn**: process each slot's strip check independently. After all checks complete, if all three slots are Inert, trigger the Win/Loss check once. The Win/Loss check fires after all strip checks for the turn are complete — not mid-sequence.

---

### Penetrating Edge Cases

- **If a Penetrating damage event targets a slot's last layer (no next layer exists)**: strip the layer normally. Excess damage is discarded — there is no next layer to cascade into. The slot becomes Inert.

- **If a Penetrating cascade reveals a new layer whose BUFF fired this turn**: recalculate HP_eff fresh for the new layer: `HP_eff_new = HP_Lk_new + BUFF_value` (BUFF_value = 0 unless the new layer's trigger set also includes the face that triggered BUFF — which it usually won't, since BUFF fired on the now-stripped layer card).

- **If a Penetrating event fully Inerts a slot in one turn**: the slot immediately enters the Inert state. Any subsequent dice already placed on that slot column fizzle — the Slot Tracker Die has been removed. Announce "slot Inerted" immediately when the last layer strips.

- **If item departure-trigger damage fires and the Penetrating flag is active**: item departure-trigger damage does not receive the Penetrating flag. Only the die effect that carried the Penetrating condition applies Penetrating to the Angel slot. (Per Character System Rule 11.)

---

### BUFF Edge Cases

- **If BUFF fires on a slot the party is not targeting this turn**: the BUFF raises HP_eff on that slot, but with the slot tracker at 0, the strip check trivially fails. The mandatory secondary component (Rule 4.7) still fires in full regardless.

- **If two different slots both have a BUFF face that triggers this round**: both BUFFs are announced separately at phase start (Head → Body → Weapon order). Each BUFF raises only its own slot's effective threshold. They do not interact.

---

### PRS Edge Cases

- **If a PRS constraint makes it impossible for any player to legally place any die**: the PRS fires as announced. No placements occur this Effect Resolution Phase. The Angel Resolution Phase proceeds normally. A total lockout is legal but extreme; card authors must verify WAS_total remains publishable.

- **If two PRS constraints from different slots conflict and produce an impossible combination**: both fire as announced. The combined effect is a total lockout. This is a degenerate encounter design error — the encounter designer must verify that no combination of their slots' PRS faces can create mutual exclusion. Log as a design defect if discovered in playtesting.

- **If a PRS restricts a player slot type that no player currently has active (all such player slots are inert)**: the PRS fires as announced but has no practical effect. The party proceeds with all remaining legal placements.

---

### Multi-Angel Edge Cases

- **If one Angel is Cleared during the strip check and the surviving Angel still has active slots with HIT/SRG committed**: the surviving Angel fires its hits normally. The Cleared Angel fires nothing.

- **If two Angels share a trigger face and stripping one Angel's slot causes the encounter to end before the other Angel's slot resolves**: process all strip checks in full before any Win/Loss check. Only after all strip checks for the turn are complete does the Win/Loss check fire.

---

### Card Error Edge Cases (Physical Prototype)

- **If a slot layer card has a trigger set printed but no action printed**: treat as a null action — announce "Slot fires: no action." Log as a card-vetting defect.

- **If HP on a slot layer card is 0 or negative**: treat as HP = 4 (HP_floor). Announce the substitution. Log as a card-vetting defect.

- **If a slot layer card is placed in the wrong slot column**: correct immediately when discovered. If discovered mid-encounter, move the card to its correct column and layer position, and treat all prior actions as if it had been correctly placed throughout.

## Dependencies

**Upstream dependencies** (systems this one depends on):

| System | Dependency Type | What the Angel System Consumes |
|---|---|---|
| **Dice Economy** | Hard | 3D6 median roll mechanic and P_angel probability table (used for WAS_slot_Lk calculation and trigger set design); E_angel = 3.5; the rule that Angel result is revealed before player placement; the rule that Angel resolution fires after all player effects |
| **Integrity System** | Hard | D_player_base = 8 and D_agg(n) = n×8 (used for HP calibration and BUFF safe-range formula); hit type definitions (Party hit / Targeted hit); no-carryover rule (slot tracker resets each turn); two-die damage tracker model (applies per Slot Tracker Die); targeted-hit-to-inert-slot ruling |

*Note: The old HP bounds formula (HP_min/HP_max based on HP_alpha/HP_beta) no longer governs Angel slot HP. Per-slot HP is calibrated via the severity-weighted Formula 2 in this GDD, using D_player_base and D_agg as anchors only.*

**Downstream dependents** (systems that depend on this one):

| System | Dependency Type | What They Consume |
|---|---|---|
| **Combat System** | Hard | Full Angel action resolution sequence (Angel Roll Phase → BUFF/PRS at Effect Resolution Phase start → per-slot strip checks at phase end → HIT/SRG in Angel Resolution Phase); multi-Angel shared-roll rule; Cleared-Angel-cancels-actions rule; damage declaration format (Angel + slot). Combat owns the phase structure; Angel System defines the rules within it. |
| **Character System** | Hard | H_pt (expected hits per player per turn) derived from each Angel's per-slot trigger sets and layer actions via Formula 4. Penetrating is slot-scoped (Rule 6.6) — Character System must reflect this in Pilgrim's ability description. |
| **Win/Loss Conditions** | Hard | "Angel Cleared" trigger — when all three slots of an Angel are Inert, Win/Loss is notified. Cleared = all slots Inert (not simply all layers stripped of a single stack). Win/Loss check fires before the Angel Resolution Phase begins in the same round. |
| **Equipment System** | Soft | PRS actions may restrict player dice placement on specific slot types (Head/Body/Weapon) for one turn. Equipment System must handle externally-imposed placement restrictions. |
| **Events System** | Soft (provisional) | The Events System pulls Angel boards from the Angel deck for encounter events. Encounter cards must state: Angel count per party size, layers per slot per party size, and hit resolution order for multi-Angel encounters. This dependency is provisional — confirm when the Events System GDD is authored. |

**Bidirectional consistency notes:**
- **Combat System GDD** must update the damage declaration step to require Angel + slot targeting declaration.
- **Character System GDD** must update Penetrating (Pilgrim Rule 11) to reflect within-slot cascade only — cross-slot penetration does not exist.
- **Win/Loss Conditions GDD** must reflect the updated Cleared definition: all three slots Inert (not all layers of a single HP stack).
- **Integrity System GDD**: the old HP bounds formula (HP_min/HP_max) no longer applies to Angels — note this distinction when that GDD is next reviewed.

## Tuning Knobs

**1. HP per slot layer (HP_slot_Lk)**
- **Adjusts**: How long a layer takes to strip; encounter pacing tension
- **Safe range**: [4, 14] for all published MVP cards. Outer layers: [4, 10]. Inner layers: [7, 14].
- **Too low (< 4)**: Layer strips trivially — one player solos it in any turn; no cooperative decision required; action economy disrupted too easily
- **Too high (> 14)**: Layer takes 2+ focused turns to strip; encounter runs long; inner layers feel impassable
- **Source formula**: Formula 2 (HP calibration — severity-weighted)

**2. Trigger set size per layer (F_slot_Lk)**
- **Adjusts**: How often a slot fires; baseline pressure from this slot per turn
- **Safe range**: F ∈ [0.074, 0.963]. Outer layers: F ≤ 0.50. Inner layers: F ≥ outer layer F.
- **Too narrow (F < 0.074)**: Slot effectively never fires; wastes a slot column
- **Too wide on outer layer (F > 0.65)**: Angel fires frequently from turn 1; no escalation arc; inner layers feel flat by comparison
- **Design guideline**: Inner layer F ≥ outer layer F (at least one additional trigger face on each deeper layer)

**3. Severity per layer (S_slot_Lk)**
- **Adjusts**: How punishing a slot's action is when it fires
- **Safe range**: S ∈ {0–5}. Outer layers: S ≤ 3. Inner layers: S ≤ 5.
- **Too low on inner layer (S ≤ 1 on innermost)**: No escalation moment; inner reveal is flat; violates the revelation fantasy
- **Too high on outer layer (S = 5 from round 1)**: Party under maximum pressure immediately; no mid-fight escalation; violates WAS_total starting band [1.5, 2.5]
- **Interaction with trigger set**: WAS_slot_Lk = F × S. High F + high S on the same layer is an escalation spike — ensure WAS_total stays ≤ 3.5

**4. Layer count per slot**
- **Adjusts**: Encounter duration and revelation arc depth
- **Safe range**: 2 layers minimum (1-layer slots are too easily inerted); 3–4 layers for major encounters
- **At 2 layers**: One revelation moment per slot; 6 total strip events per standard angel; clean and fast; standard for n=2
- **At 3 layers**: Two revelation moments per slot; 9 total strip events; longer encounter, more escalation arc; standard for n=3–4 encounters
- **At 4 layers**: Extended encounter; reserve for the God card boss; inner layers must have high WAS or encounter becomes attritional

**5. Number of Angels per Encounter**
- **Adjusts**: Complexity, coordination load, and damage-split decisions
- **Safe range**: 1–2 Angels for MVP
- **At 1 Angel**: Clean focus-fire strategy; cooperative decisions are about slot priority
- **At 2 Angels**: Adds cross-Angel targeting decisions; each slot strip on Angel A trades against progress on Angel B; HP on individual slots should be reduced to keep encounter within time budget
- **Design constraint**: Total angel HP across all slots of all Angels in the encounter should stay within ~8 × D_agg to target ≤ 8 turns

**6. BUFF value (BUFF_value)**
- **Adjusts**: How effectively a BUFF face impedes stripping on that turn
- **Safe range**: [1, D_agg(n) − HP_slot_Lk]. At MVP (n=2): max BUFF_value = 16 − HP_slot_Lk
- **Too low (= 1)**: Near-negligible; the face feels wasted
- **Too high (> D_agg − HP_slot_Lk)**: Slot cannot be stripped on BUFF turns — degenerate
- **Frequency interaction**: BUFF on high-F trigger faces fires often. A large BUFF on a common face is more punishing than the same BUFF on a rare face.

**7. Encounter Layer Scaling (party size)**
- **Adjusts**: Encounter length and difficulty for larger parties
- **Design levers**: Add 1 layer per slot (n=3–4 variant) OR add a second Angel. Both use the same fixed card pool — no card reprints needed.
- **Interaction**: Adding a layer deepens the escalation arc; adding an Angel increases parallel threat. Choose based on the encounter's intended feel: "this angel gets more dangerous as it's damaged" (deeper slots) vs. "this encounter is about coordination across threats" (more angels)

## Visual/Audio Requirements

[To be designed]

## UI Requirements

[To be designed]

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Items marked DEFERRED are design-tool validations, not table-play checks.*

---

**Board Setup**

- **AC-01** — GIVEN an encounter card states "n=2: 1 angel, 2 layers per slot," WHEN the encounter is set up, THEN the angel board has three slot columns (Head, Body, Weapon), each with exactly 2 layer cards (L1 face-up, L2 face-down), and one Slot Tracker Die placed below each column.

- **AC-02** — GIVEN an encounter card states "n=3–4: 1 angel, 3 layers per slot," WHEN the encounter is set up for a 3-player party, THEN each slot column has 3 layer cards (L1 face-up, L2 and L3 face-down) and one Slot Tracker Die per column.

- **AC-03** — GIVEN a 2-angel encounter, WHEN the encounter is set up, THEN two complete angel boards are placed in the Angel Zone, each with their own independent slot columns and Slot Tracker Dice.

---

**Dice Roll and Trigger Reading**

- **AC-04** — GIVEN an angel with Head trigger {1,6}, Body trigger {2,4,5}, Weapon trigger {3,4}, WHEN the median die shows 4, THEN both Body AND Weapon slots fire their respective actions. Head does not fire. The Lead Player announces both firing slots before any placement begins.

- **AC-05** — GIVEN the median die shows a face that is not in any active slot's trigger set, WHEN the Angel Roll Phase resolves, THEN no angel slot fires this round. No BUFF, PRS, HIT, or SRG announces. The party proceeds to placement with no angel pressure this turn.

- **AC-06** — GIVEN a slot is Inert, WHEN the median die shows a face that was in that slot's original trigger set, THEN the Inert slot produces no action. The face is treated as silent for that slot.

---

**Damage Targeting**

- **AC-07** — GIVEN two active slots (Body and Weapon) on an Angel, WHEN a player's damage effect fires for 6 damage, THEN the player declares "Angel [name], Body slot" (or Weapon) before advancing any Slot Tracker Die. The 6 damage cannot be split between the two slots.

- **AC-08** — GIVEN Body slot tracker reads 5 and Weapon slot tracker reads 3, WHEN a player deals 4 damage to Body, THEN Body tracker advances to 9. Weapon tracker stays at 3. No other tracker is affected.

---

**Strip Checks (per slot)**

- **AC-09** — GIVEN Body slot L1 has HP=8 and the Body Slot Tracker reads 8 at end of Effect Resolution Phase, WHEN the strip check fires, THEN Body L1 is stripped. The L2 card is flipped face-up. The Body Slot Tracker Die resets to 0.

- **AC-10** — GIVEN Body slot L1 has HP=8 and Body tracker reads 7, WHEN the strip check fires, THEN Body L1 survives. Body Slot Tracker resets to 0. Head and Weapon trackers also reset independently.

- **AC-11** — GIVEN Body slot L1 has HP=8, a BUFF+3 fires on Body (HP_eff=11), and the Body tracker reads 8 at end of phase, WHEN the strip check fires, THEN Body L1 survives (8 < 11). The BUFF raised the threshold; the damage was not prevented.

- **AC-12** — GIVEN Body slot L1 has HP=8 and the party deals 12 damage to Body, WHEN the strip check fires, THEN Body L1 strips (12 ≥ 8). The excess 4 damage is discarded (non-Penetrating). Body L2 tracker starts at 0.

---

**Inert State**

- **AC-13** — GIVEN Body slot L2 is the last layer and strips, WHEN the strip check resolves, THEN Body Slot Tracker Die is removed. The Body column is marked Inert. On all subsequent turns, rolling a face in Body's former trigger set produces no action from Body.

- **AC-14** — GIVEN Head slot is Inert and Body slot is still Active, WHEN the median die shows face 1 (which was in Head's trigger set only), THEN no slot fires this round. The die result is effectively a free turn for the party.

- **AC-15** — GIVEN all three slots of an Angel become Inert on the same turn, WHEN all three strip checks resolve, THEN the Angel is Cleared. The Win/Loss check fires immediately. The Angel Resolution Phase does not occur for this Angel this round.

---

**Penetrating (within-slot cascade)**

- **AC-16** — GIVEN Body slot L1 has HP=6 and L2 has HP=10, the Penetrating flag is active, and the party deals 9 damage to Body, WHEN the strip check fires, THEN: Body L1 strips (9 ≥ 6), excess 3 carries to Body L2. Body L2 tracker = 3. If 3 < 10, L2 survives this turn. Body L2 tracker resets to 0 at end of phase.

- **AC-17** — GIVEN Body slot L1 has HP=6 and L2 has HP=5, the Penetrating flag is active, and the party deals 12 damage to Body, WHEN the strip check fires, THEN: Body L1 strips (12 ≥ 6), excess 6 carries. Body L2 check: 6 ≥ 5 → Body L2 strips also. Body is Inert in one Penetrating event.

- **AC-18** — GIVEN the Penetrating flag is active and Body slot L2 is the last layer, WHEN the cascade reaches L2 and excess damage exceeds HP_L2, THEN L2 strips and the slot is Inert. Excess is discarded — Penetrating does not cross slot boundaries to Head or Weapon.

---

**Slot Trigger Set Reading (multi-slot compound rounds)**

- **AC-19** — GIVEN Head trigger {1,6} carries HIT:2 and Weapon trigger {3,4} carries HIT:1, and the median die shows 4 (4 ∉ Head trigger set {1,6}), WHEN the Angel Resolution Phase resolves, THEN only Weapon's HIT:1 fires. Head's HIT:2 does not fire. The party routes 1 hit only.

- **AC-20** — GIVEN two slots each carry a BUFF action and both slots trigger on face 4, WHEN the median die shows 4, THEN both BUFFs are announced at phase start in slot order (Head → Body → Weapon). Each BUFF raises only its own slot's HP_eff. They do not interact.

---

**Deferred — Design Tools**

- **AC-21** *(DEFERRED — design tool)* — GIVEN a completed angel slot design, WHEN WAS_total is calculated at each encounter state (all L1 → all L2 → all L3), THEN WAS_total at all-L1 ∈ [1.5, 2.5], at all-L2 ∈ [2.0, 3.0], at all-L3 ≤ 3.5.

- **AC-22** *(DEFERRED — design tool)* — GIVEN one slot becomes Inert and the other two slots are at inner layers, WHEN H_pt_total is calculated for this encounter state, THEN H_pt_total ≥ 0.3 (no dead-turn risk).

- **AC-23** *(DEFERRED — design tool)* — GIVEN a completed slot layer card with HP_slot_Lk and BUFF_value, WHEN verified against `BUFF_value ≤ D_agg(n) − HP_slot_Lk`, THEN the constraint is satisfied. A BUFF that makes HP_eff > D_agg(n) is an illegal card at that party size.

## Open Questions

- **OQ-01 (PENDING: playtesting)** — Does the hidden-until-strip rule feel fair on a newly revealed slot layer? If the "blind first inner-layer turn" feels arbitrary rather than dramatic, consider previewing the inner layer card once the outer layer is below half HP (flip face-up when outer tracker first exceeds HP_L1/2). Deferred pending playtest data.

- **OQ-02 (PENDING: playtesting)** — Is slot-targeting (declare Angel + slot) engaging or does it create analysis paralysis? The core strategic questions — which slot to focus, when to split vs. coordinate — need playtest validation. Watch for: players always defaulting to the same slot (no decision depth), or players spending >30s per turn debating slot priority (cognitive overload). AC-22 (fire rate stability) should be verified at first playtest.

- **OQ-03 (PENDING: Events System)** — What is the encounter card format? Must now include: angel count per party size, layers per slot per party size, trigger set for each slot (Head/Body/Weapon) for each angel, hit resolution order for multi-angel encounters, and the layer card set to use. The Events System GDD must define the encounter card anatomy to accommodate all these fields.

- **OQ-04 (PENDING: component design)** — How should trigger sets be displayed on slot layer cards? Options: printed as face numbers ("2 · 4 · 5"), graphical die faces, or a combination. Must be table-readable at 60–100 cm and unambiguous at a glance. Deferred to Component Design spec.

- **OQ-05 (RESOLVED — 2026-05-27)** — Penetrating cascade (excess-carries, Rule 6.6) confirmed: total damage output = R; does not multiply by layer count. Pilgrim S_encounter at n=2 base ≈ 37–40 (below band). Resolution: Pilgrim is intentionally item-dependent at n=2; one Type 2 Output Amplification item on Reservoir pushes S_combined to ~54–58 (in band). At n=3–4 with R=18, cascade double-strip (full-slot Inert) achieves band unaided. See character-system.md Tuning Knob 6 and S_encounter Comparison table for full derivation. No changes required to Angel System GDD.

- **OQ-06 (PENDING: playtesting)** — Does the action economy arc (WAS_total increasing as inner layers expose) produce the intended escalation feel, or does the encounter peak too early (all inner layers exposed before players have made progress)? Monitor in MVP playtests: if inner layers expose too fast, increase outer layer HP or reduce inner layer trigger set breadth.
