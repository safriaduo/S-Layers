# Combat System

> **Status**: Needs Revision — propagation update from angel-system.md redesign (2026-05-27) — re-review pending
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-27
> **Revision notes**: Win/Loss collision reverted (Loss prevails for non-God encounters); AC-02, AC-18, AC-22, AC-27 corrected; AC-30 split; AC-31–AC-38 added (edge case coverage); encounter-card order defined; SRG/BUFF_value cross-references added; T_round range corrected; EPC guard clause added; Phase 2 full-magnitude requirement; Phase 4 Pillar 3 advisory; PRS spectator-state edge case and Player Fantasy note added; OQ-2 coefficients corrected; OQ-3 resolved.
> **Implements Pillar**: Agency Over Dice (primary); Cooperative Ownership (secondary); Fast and Focused (third)

## Overview

The Combat System is the sequencer of S-Layers. It owns one thing: the order in which the game's other systems fire during a combat encounter, and the formal handoffs between them. It does not define dice-placement rules (Equipment System), hit-routing rules (Integrity System), Angel action content (Angel System), or layer-reveal behavior (Equipment Degradation) — it calls those systems in the order their rules require and ensures they receive the correct context at the right moment.

An encounter begins when an Event card resolves as a combat encounter, placing one or more Angel boards in the Angel Zone. The Combat System then runs six phases in order until the encounter ends — either the Angel is cleared (all layers stripped) or a player board is destroyed (all slots inert). Each phase is owned by the system doing the work inside it:

**Phase 1 — Enemy Roll:** Angels collectively roll 3D6. The median face is identified and revealed to all players. The Angel's committed action for this round is visible on the face map before any player picks up a die.

**Phase 2 — Action Assignment:** The committed face is read against the current layer's face map. Action types are publicly identified: HIT/SRG (will fire in Phase 6); BUFF/PRS (will activate at the start of Phase 5). No game-state changes occur. This phase exists to ensure full information is established before placement begins.

**Phase 3 — Player Roll:** Each player picks up their 2D6 and rolls. Die values are visible to all players.

**Phase 4 — Dice Placement:** Players discuss and place their dice on slots. No effects fire. This phase is complete when every player has placed or discarded all their dice.

**Phase 5 — Player Resolution:** BUFF and PRS actions activate first, before any placed-die effects resolve. Placed-die effects then fire in party-chosen order. Damage is tracked against the current Angel layer. At phase end, a single damage check fires: if total ≥ layer HP, the layer is stripped and Equipment Degradation resolves the reveal. Underspent damage is discarded — no carryover.

**Phase 6 — Enemy Resolution:** HIT and SRG actions fire. The party routes each hit per Integrity System rules. Player board destruction or Angel clearing is evaluated at phase end, triggering the Win/Loss check.

If the encounter is not over after Phase 6, the sequence repeats from Phase 1.

At the table, a combat encounter is five to eight minutes of the build proving itself. The Angel's committed action is always visible when the placement decision is made — no die is placed blind. The drama arrives on schedule: the damage check at Phase 5, the hit routing in Phase 6, the layer reveal that changes the board for the next round. Whether a layer strips, who takes the hit, and what order effects resolve are all cooperative table decisions. The Combat System guarantees the sequence. What happens inside each phase is owned by the systems that designed it.

## Player Fantasy

The Angel declares its intent before you touch your dice. A blow, a surge, a constraint — revealed, face-up, known. The round becomes a small trial: the verdict is read aloud, and the party must answer it together, in full sight of what is coming. There is no blind corner in this system. That is the gift Heaven offers its prisoners, and the gift is also a trap: when you fail, you cannot point at uncertainty. You saw it. You chose anyway.

Every die placed in Phase 4 is a sentence you are signing. When the damage check fires at Phase 5 and the number falls short by one — the table is quiet not with frustration but with recognition. The failure was authored. Every decision point from Phase 2 onward was visible; you made them all. When the layer strips and the reveal comes up, the satisfaction is precise: not luck, not relief, but the small clean feeling of a problem solved in the open, together, with the answer already on the table before any of you reached for it.

The choice was never yours alone. *That* is the second thing the trial teaches. The placement debate in Phase 4, the hit-routing in Phase 6, the order of resolution the party calls in Phase 5 — these are not logistics. They are the cooperative sentence being written. One player holding the die, three more watching the Angel board, someone saying *"if you route that hit to Body, I can fire the burst next turn."* The round has no private moves. When it works, the whole table built it. When it fails, the whole table signed it.

Sometimes the build reaches the place where all that remains are the slots Heaven has locked — and your dice sit still. This too is authored. The degradation and the constraint together wrote this round. Being a witness to the table you helped build is not a failure of the system; it is a consequence the system was designed to make legible. It will not last. Next round, the dice come back.

**Design test:** If a player says, after a round that went wrong, *"we should have routed that differently"* — and they are already planning Phase 4 of the next round — the Combat System has worked. If they say *"I had no idea that would happen"* — something failed at Phase 2 information transparency, and the framing must be investigated.

## Detailed Design

### Core Rules

---

#### 1. Encounter Entry

When an Event card resolves as a combat encounter, the following setup executes immediately:

1. The encounter card is read aloud. It specifies: Angel identity, party size scaling (1–2 players: [N] Angel(s); 3–4 players: [M] Angel(s)), and any encounter-specific special rules.
2. The appropriate Angel board(s) are retrieved from the Angel pool and placed in the Angel Zone. Each Angel has **three slot columns** (Head, Body, Weapon) with their own layer stacks. Each slot column is placed with its outermost layer (L1) face-up and deeper layers face-down beneath it.
3. The face-up Exposed layer of each slot column is readable by all players: trigger faces, action, HP, and Layer Index are visible.
4. A **Slot Tracker Die** (spare d6) is placed below each slot column, set to 0. Each slot has its own independent tracker. If a tracker needs to exceed 6, a second die acts as the tens digit (Angel System edge case rule).
5. **Encounter-card order is established:** Angel boards are placed left-to-right in the Angel Zone and this left-to-right sequence is the **encounter-card order** for all phase sequencing in this encounter (Phase 2 announcements, Step 5a BUFF/PRS activation, Phase 6 resolution). Encounter-card order does not change mid-encounter. When an Angel is Cleared, it is skipped in the sequencing loop — its slot is not removed, it is simply passed over in subsequent phases.
6. Phase 1 begins immediately.

No other setup steps occur. All player board states (active layers, inert slots, Accumulate totals, item configurations) carry forward from prior encounters.

---

#### 2. Encounter End Conditions

An encounter ends when one of two conditions is met:

- **Win — Angel Cleared:** All three slots (Head, Body, Weapon) of all Angels in the encounter are **Inert** (all layers stripped from each slot). Evaluated at Phase 5 end (per-slot strip checks). *(2026-05-27: "Cleared" = all 3 slots Inert, not all layers of one stack.)*
- **Loss — Player Board Destroyed:** Any player's board has all three equipment slots in inert state. Evaluated at Phase 5 end (after committed dice resolve) or Phase 6 end (after hit routing resolves).

**Win/Loss collision:** If a player board is destroyed and an encounter-level Angel (non-God) is cleared in the same turn, **loss takes priority**. A board-destroyed party does not receive an encounter win result. *(Exception: if the Angel cleared is the God card, Victory prevails — Win/Loss Conditions GDD Rule 6 governs session-level termination.)*

---

#### 3. The Six-Phase Sequence

The following sequence repeats each round until an encounter end condition is met.

---

**Phase 1 — Enemy Roll**

1. All Angels currently in the encounter share one roll. One player rolls three standard six-sided dice on behalf of all Angels.
2. The three dice are sorted lowest to highest. The middle die (second position) is the resolved face for this round. Ties resolve through sorting without special rules (2,2,5 → 2; 1,5,5 → 5; 3,3,3 → 3).
3. The median face value is announced and visible to all players before any player picks up dice.
4. Phase 1 is complete when the median face is identified and confirmed visible.

*The probability distribution of this roll is defined by the Dice Economy GDD, Formula 1 (P_angel). Face 3 and 4 fire most frequently (~24% each); faces 1 and 6 fire least (~7.4% each).*

---

**Phase 2 — Action Assignment**

1. Starting with the first Angel in encounter-card order, each Angel's Exposed layer face map is read aloud.
2. The action mapped to the median face is publicly identified. **The full action text is read aloud** — category identification alone is insufficient for the full-information invariant. State both category and magnitude:
   - **HIT**: announce hit count and target type (party hit or targeted hit with constraint).
   - **SRG**: announce base hit count and escalation condition text.
   - **BUFF**: announce BUFF_value explicitly (e.g., "BUFF +4 — HP_eff this turn is 16").
   - **PRS**: announce restricted slot type(s) explicitly (e.g., "PRS — Weapon slots locked").
   No die is placed blind. No action detail is withheld at Phase 2.
3. Repeat for each additional Angel in encounter-card order.
4. No game-state changes occur in Phase 2. Information only.
5. Phase 2 is complete when all Angels' committed actions have been identified and announced.

*Phase 2 is the information guarantee: every placement decision in Phase 4 is made with full knowledge of all Angel actions, including all BUFF and PRS constraints. No die is placed blind. This is a design invariant for Pillar 1 (Agency Over Dice).*

---

**Phase 3 — Player Roll**

1. All players pick up their 2D6 simultaneously.
2. All players roll simultaneously. Die values are immediately public and visible to all players before any placement decision is made.

---

**Phase 4 — Dice Placement**

1. Players discuss placement options freely. Party discussion is not time-limited in the base rules. **Pillar 3 advisory (Fast and Focused):** For groups prone to over-deliberation, a table norm of 60 seconds for Phase 4 discussion is strongly recommended. This expectation belongs in the rulebook, not as a hard rule. If T_round consistently exceeds 150 s (see Formula 1 threshold note), the group should self-impose a soft hourglass for Phase 4.
2. Each player places or discards their dice on their available slots. Placement procedure follows Equipment System Rules 2–4.
3. No effects fire during this phase.
4. **Divine gifts** activate at one of four windows during this phase, per each character's board text. See Character System Rule 5 for window definitions.
5. Phase 4 is complete when every player has placed or discarded all dice.

*BUFF and PRS actions were identified in Phase 2. Players' placement decisions already account for them. Dice placed on PRS-restricted slots do not fire in Phase 5 (see Step 5a). No retroactive placement correction is allowed.*

---

**Phase 5 — Effect Resolution** *(= "Effect Resolution Phase" in all dependency GDDs)*

Phase 5 proceeds in three steps in order.

**Step 5a — BUFF and PRS Activation**

Before any placed-die effects resolve, all BUFF and PRS actions activate in encounter-card order.

- **BUFF activation**: State the effective HP threshold: `HP_eff = HP_L + BUFF_value`. The HP value printed on the card is unchanged. `HP_eff` applies to this turn's strip check only; it reverts to `HP_L` next round. *(BUFF_value range: owned and bounded by the Angel System GDD. The Angel System must define the valid range of BUFF_value — this formula consumes that value. Cross-reference Angel System Rule [TBD].)*
- **PRS activation**: State the restricted slot type(s). Dice already placed on restricted slots **do not fire** their effects this phase. The die is consumed and produces no output. No retroactive placement correction is permitted — the party was informed of the PRS constraint in Phase 2.

*If no BUFF or PRS are active this turn, Step 5a passes without announcement.*

**Step 5b — Placed-Die Effect Resolution**

All placed-die effects fire in **party-chosen order**. The party may interleave effects from different player boards in any sequence. Deliberation before declaring the order is permitted.

Rules governing this step:
- N = raw die face. Items may modify effect output; they cannot modify N.
- **Damage targeting**: Before advancing any tracker, the active player declares both **the target Angel** and **the target slot** (Head, Body, or Weapon). A single effect's damage cannot be split between slots or Angels.
- **Damage tracking**: Each damage effect advances the declared slot's **Slot Tracker Die** by the damage amount. Slot trackers are independent — damage declared to Body never advances Head or Weapon trackers. *(2026-05-27: per-slot targeting replaces per-Angel targeting.)*
- **Accumulate effects**: Running totals update on each die placement; thresholds fire immediately when crossed, per Dice Economy Rules 8–11.
- **Self-strip effects** (e.g., Martyr's Weapon): execute the full Layer Reveal Procedure (Equipment Degradation Rule 1) immediately when triggered. The newly revealed layer's effects are inert for the remainder of Phase 5.
- **Mid-step board destruction**: If a player board is destroyed during Step 5b (all slots reach inert state from self-strip or triggered effects), the Win/Loss check fires after all committed dice on that board have fully resolved (Integrity System Rule 13). The encounter ends. Phase 5c and Phase 6 do not execute.

**Step 5c — End-of-Phase Strip Check**

After all placed-die effects have resolved, each **slot of each Angel** is evaluated independently:

- If `D_slot_turn ≥ HP_eff` for a slot's Exposed layer → that layer is stripped. The Layer Reveal Procedure (Equipment Degradation Rule 1) resolves immediately. If no layers remain in that slot, the slot becomes **Inert** (Slot Tracker Die removed).
- If `D_slot_turn < HP_eff` → the layer survives. The slot's tracker resets to 0. No carryover.
- **Penetrating exception**: If the Penetrating flag is active, excess carries within the same slot (cascade within slot only — does not cross slot boundaries). See Angel System Rule 6.6.
- **Angel Cleared**: If the strip check leaves all three slots of an Angel Inert (Head, Body, and Weapon all Inert), the Angel is **Cleared**. A Win/Loss check fires. If all Angels in the encounter are Cleared, the Win condition is met and the encounter ends. *(2026-05-27: Cleared = all 3 slots Inert.)*
- All Slot Tracker Dice reset to 0 after all strip checks resolve.

*(2026-05-27: Strip checks are now per-slot, not per-Angel.)*

---

**Phase 6 — Enemy Resolution** *(= "Angel Resolution Phase" in all dependency GDDs)*

HIT and SRG actions fire in encounter-card order. Each Angel's actions fully resolve before the next Angel begins.

1. For each Angel with a committed HIT or SRG action (only Angels not Cleared in Phase 5):
   a. Announce the action: hit count and type (Party hit or Targeted hit).
   b. For SRG: resolve the base hit count first; then evaluate and resolve the escalation clause. *(SRG escalation clause — its trigger condition, hit count, and resolution — is defined and owned by the Angel System GDD. Cross-reference Angel System Rule [TBD — must be resolved before implementation]. AC-16 tests the escalation path.)*
   c. For each hit: the party cooperatively routes it to a slot on any eligible player board, per Integrity System routing rules. Slot eligibility is evaluated at the moment each hit is assigned. A slot emptied by a prior hit this Phase 6 is immediately ineligible for subsequent hits.
   d. Each routed hit executes the Layer Reveal Procedure if the targeted slot has remaining layers.
2. After all Angels' Phase 6 actions resolve: if any player board is destroyed as a result of hit routing, the Win/Loss check fires. Loss condition.
3. If no Win/Loss condition triggered: encounter continues. Return to Phase 1.

*Hit routing in Phase 6 is a party-table decision — not one player's unilateral choice. Every player must look at every board before routing is declared. This is load-bearing for Pillar 4 (Cooperative Ownership).*

---

#### 4. Encounter End — Cleanup

When an encounter ends (Win or Loss), this procedure resolves before the next Event card is drawn:

1. **Post-combat item award** (Win only): Draw one item from the shared item pool. The current First Player distributes it per Item System Rule 3.
2. **Angel Zone cleanup**: All Angel board(s) and stripped layer piles are removed and placed in a permanent Angel discard area. Damage Tracker Dice return to the shared dice pool.
3. **Player board state persists**: Per-slot stripped card piles remain beside each player board and persist between encounters, accessible for restoration effects. Inert slots remain inert — they do not restore between encounters.
4. **Accumulate totals persist**: Running Accumulate totals on surviving active layers are not reset. Tokens remain on cards.
5. **Active items persist**: Items in Item Anchor Zones remain attached. Items previously stripped with equipment layers remain in item discard permanently and cannot be re-acquired.
6. **First Player token rotation**: After the encounter card is fully resolved (including any post-combat item award), the First Player token rotates one position clockwise. The new First Player holds the token for the next Event card.

---

### States and Transitions

**Encounter-Level States**

| State | Condition | Enter From | Exit To |
|---|---|---|---|
| Inactive | No encounter in progress | — | Encounter Event card drawn → Active |
| Active | ≥1 Angel in play; no board destroyed | Event card | All Angels Cleared (no board destroyed) → Resolved Win; Any board destroyed → Resolved Loss |
| Resolved (Win) | All Angels Cleared, no board destroyed | Phase 5 strip check | Cleanup → next Event card |
| Resolved (Loss) | Any player board destroyed | Phase 5 or Phase 6 | Session ends |

**Round-Level States (within Active)**

| Phase | Name | Ends When |
|---|---|---|
| 1 | Enemy Roll | Median face identified and announced |
| 2 | Action Assignment | All Angels' actions publicly identified |
| 3 | Player Roll | All players have rolled 2D6, values public |
| 4 | Dice Placement | All players have placed or discarded |
| 5 | Effect Resolution | Step 5c complete (all strip checks evaluated) or Win/Loss triggered |
| 6 | Enemy Resolution | All HIT/SRG resolved including Surge escalations, or Win/Loss triggered |

**Angel States (during Active)**

| State | Condition | Transition |
|---|---|---|
| Exposed | Top layer face-up, HP visible | `D_turn ≥ HP_eff` at Step 5c → strip → next Hidden layer becomes Exposed (or Cleared) |
| Cleared | All layers stripped | Win/Loss check fires |

---

### Interactions with Other Systems

| System | What Combat System Provides | What It Receives Back |
|---|---|---|
| **Dice Economy** | Phase timing signals: Placement Phase start/end, Effect Resolution Phase start/end | N values per placed die; Accumulate total updates; phase completion signals |
| **Equipment System** | Effect Resolution Phase start signal (effects may now fire) | Placed-die effects in party-chosen order, per Equipment System rules; slot legality enforcement |
| **Integrity System** | Phase 6 hit count, type, and targeting constraints from each Angel; routing opportunity | Routing decisions (which slot, which player); board destruction trigger; slot state |
| **Equipment Degradation** | "Layer stripped" event (slot, player) when strip check passes or hit routing empties a layer | Full Layer Reveal Procedure (6-step atomic); new Exposed layer state |
| **Angel System** | Phase 1 roll trigger; Phase 2 face-map read trigger; Phase 5 BUFF/PRS activation; Phase 6 HIT/SRG trigger | Action categories, HP values per layer, encounter setup (party size scaling, Angel count); multi-Angel resolution order (Angel System Rule 7.5) |
| **Character System** | Phase 4 activation windows (divine gift timing) | Gift execution per character board text (Character System Rule 5) |
| **Win/Loss Conditions** | "Angel Cleared" trigger (Phase 5 end); "Player Board Destroyed" trigger (Phase 5 or 6) | Game-end determination |
| **Item System** | Encounter-end signal (post-combat item award trigger); current First Player identity | Item drawn and distributed per Item System Rule 3; First Player token rotation |
| **Events System** | Encounter-end signal (Event card fully resolved); First Player token rotates | Next Event card drawn |

---

> **Registry note — T_encounter:** The MVP combat encounter is calibrated to `T_encounter = 8 turns`. This constant anchors `S_encounter`, `N_strips`, `S_layer`, and `S_combined` across all dependency GDDs. If playtests show consistent encounter lengths outside 6–10 turns, recalibrate `T_encounter` and re-derive all downstream formulas before the next playtest cycle.

## Formulas

*The Combat System introduces two new design-tool formulas: a round-duration estimate (T_round) and an encounter pacing validator (EPC). All other numeric values used at the Combat System level are owned by dependency GDDs and are cross-referenced here, not redefined.*

---

### Formula 1 — T_round: Estimated Real-Time Duration per Round

The `T_round` formula is defined as:

`T_round = T_base + (α_4 × n) + (α_5 × n) + (α_6 × n) + (C_item × β)`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Fixed phase baseline | T_base | int (seconds) | 25 | Sum of fixed-cost phases: Phase 1 (~10 s) + Phase 2 (~10 s) + Phase 3 (~5 s) |
| Placement time per player | α_4 | float (seconds) | [5, 15] | Per-player Phase 4 contribution; experienced = 5 s, new players = 15 s; baseline = 10 s |
| Resolution time per player | α_5 | float (seconds) | [5, 15] | Per-player Phase 5 contribution; baseline = 10 s |
| Routing time per player | α_6 | float (seconds) | [2, 7] | Per-player Phase 6 hit-routing contribution; baseline = 5 s |
| Party size | n | int | {1, 2, 3, 4} | Number of active players |
| Item complexity | C_item | int | {0, 1} | 0 = no items attached (early session); 1 = standard item load (≥1 item in play) |
| Item overhead | β | float (seconds) | [10, 20] | Flat overhead when items are active: reading text, checking conditions; baseline = 15 s |
| Round duration | T_round | float (seconds) | [37, 193] | Estimated real-time duration of one complete 6-phase round. *(Previously stated as [55, 175] — corrected: n=1 solo minimum is 37 s; n=4 all-max is 193 s.)* |

**Output Range:** T_round ∈ [37, 193] seconds across full variable range. At typical 2–4 player groups with baseline coefficients: [90, 140] s. Values above 150 s indicate over-deliberation in Phase 4 — apply the Pillar 3 advisory (see Phase 4 Rule 1). Solo play (n=1) can reach as low as 37 s.

**T_round is a calibration estimate, not a hard value.** The α coefficients are tuning knobs. Playtest data should tighten them after the first 5–10 sessions.

**Example (n=2, C_item=1, all baselines — standard MVP play):**
`T_round = 25 + (10×2) + (10×2) + (5×2) + (1×15)`
`T_round = 25 + 20 + 20 + 10 + 15 = 90 seconds` ≈ **1.5 minutes per round**

---

### Formula 2 — EPC: Encounter Pacing Check

The `EPC` formula validates whether a specific Angel design will hit the 5–8 minute encounter target. It runs in three steps.

**Step 1 — Derive expected encounter length:**

`T_encounter_actual = Σ T_angel(HP_L(j), D_agg)` for j = 1 to L_angel

where each T_angel term is the Integrity System Formula 2 result for that layer.

**Step 2 — Convert to real-time minutes:**

`T_encounter_minutes = (T_encounter_actual × T_round) / 60`

**Step 3 — Validate against turn target band:**

`T_lo = ⌈300 / T_round⌉`     `T_hi = ⌊480 / T_round⌋`

PASS if `T_encounter_actual ∈ [T_lo, T_hi]`; WARN_SHORT if below; WARN_LONG if above.

**Guard clause:** If `T_lo ≥ T_hi` (possible when T_round > ~240 s with high α coefficients), EPC returns **DEGENERATE_BAND** — not PASS or WARN. This indicates extreme over-deliberation; revise α estimates before using EPC results.

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Angel layer count | L_angel | int | {1, 2, 3, 4} | Number of layers in this Angel's design |
| Layer j HP | HP_L(j) | int | [4, 14] | Printed HP on layer j (per-slot layer card). *(2026-05-27: updated from [HP_min, HP_max] = [10, 19] to [4, 14] — Angel System redesign moved to 3-slot WAS-based HP calibration; old bounds formula deprecated.)* |
| Party aggregate damage | D_agg | float | [7, 40] | Expected damage per turn (Integrity System Formula 1); at MVP (n=2): 16 |
| Expected turns per layer | T_angel(j) | float | [1.0, 3.0] | Integrity System Formula 2 — not redefined here |
| Expected encounter turns | T_encounter_actual | float | [1.0, ∞) | Sum of T_angel across all layers |
| Round duration | T_round | float (seconds) | [37, 193] | From Formula 1 (this GDD); baseline = 90 s |
| Projected encounter time | T_encounter_minutes | float (minutes) | [0, ∞) | Real-time projection for this Angel design |
| Low turn bound | T_lo | int | [3, 6] | Minimum turns for 5-minute encounter at current T_round |
| High turn bound | T_hi | int | [5, 10] | Maximum turns for 8-minute encounter at current T_round |
| Pacing result | EPC | enum | PASS / WARN_SHORT / WARN_LONG | PASS: within [T_lo, T_hi] |

**Output Range:** T_encounter_actual varies by Angel design. Target band at T_round = 90 s: [4, 5] turns. At T_round = 75 s (experienced group): [4, 6] turns.

**Example (Integrity System example Angel — 3 layers, HP 12/16/18, D_agg=16, T_round=90 s):**

Using T_angel values from the Integrity System GDD (Formula 2 example):
- Layer 1: HP_L=12 → T_angel ≈ 1.2 turns
- Layer 2: HP_L=16 → T_angel ≈ 1.5 turns
- Layer 3: HP_L=18 → T_angel ≈ 2.5 turns

`T_encounter_actual = 1.2 + 1.5 + 2.5 = 5.2 turns`
`T_encounter_minutes = (5.2 × 90) / 60 = 7.8 minutes`

At T_round=90 s: T_lo=4, T_hi=5 → EPC = **WARN_LONG** (5.2 > 5)
At T_round=75 s (experienced players): T_lo=4, T_hi=6 → EPC = **PASS**

The 7.8-minute projection validates the 5–8 minute target for experienced groups. New player groups (T_round ≈ 115–120 s) project to 10–11 minutes — WARN_LONG. Reducing Layer 1 HP from 12 to 10 saves ~18 seconds per encounter for new-player groups.

---

### Cross-Referenced Formulas (owned by dependency GDDs)

| Formula | Owned By | Relevance to Combat |
|---|---|---|
| `T_angel = 1 / P(D_turn ≥ HP_L)` | Integrity System (F2) | T_encounter_actual is a direct sum of T_angel values |
| `D_agg = n × D_player_base` | Integrity System (F1) | Input to T_angel; at MVP (n=2, D_player_base=8): D_agg = 16 |
| `N_strips = H_pt × T_encounter` | Equipment Degradation | T_encounter in N_strips is the calibration constant (8), not T_encounter_actual |
| ~~`HP_min/HP_max` bounds~~ | ~~Integrity System (F4)~~ | *SUPERSEDED 2026-05-27 — HP bounds formula deprecated. Angel layer HP is now calibrated via Angel System Formula 2 (WAS-based, HP ∈ [4, 14] per slot layer). EPC validity is enforced by T_angel ∈ [1.0, 3.0], not old HP_min/HP_max.* |
| `WAS = Σ(P_angel(i) × S_i)` | Angel System | EPC measures timing; WAS measures threat pressure. Always check both. |
| `S_combined ≤ 85` advisory | Item System | C_item=1 shifts T_round upward; validate S_combined before publishing item-heavy encounters |

---

> **⚠️ T_encounter constant vs. T_encounter_actual:**
> `T_encounter = 8` (registry constant, Equipment System) is a balance calibration anchor — it ensures S_encounter, N_strips, and S_layer are conservative. It is not a prediction for any specific Angel. `T_encounter_actual` (EPC output) is the per-Angel design tool. Do not change the registered constant; use T_encounter_actual to evaluate specific Angel designs.

## Edge Cases

*Format: **If [condition]**: [exact outcome].*

---

**Phase 1 — Enemy Roll**

- **If all three Angel dice show the same face value (e.g., 3, 3, 3):** sort lowest to highest, take the middle position. Triple ties are unambiguous — sorted [3, 3, 3], middle = 3. No special rule or re-roll.

---

**Phase 2 — Action Assignment**

- **If two Angels are in play and both produce a BUFF and a PRS on the same round:** both activate at Step 5a in encounter-card order. Neither cancels the other. BUFF raises HP_eff for each Angel independently; PRS constraints from both Angels stack.

---

**Phase 4 — Dice Placement**

- **If the Pilgrim receives their extra die from the prior-turn gift and the Angel's Phase 2 identified a PRS that locks all slot types:** all three dice (extra die + 2D6) are discarded without placement. PRS applies universally — dice from divine gifts are subject to the same placement rules as the 2D6 pool (Character System Rule 7). The prior-turn sacrifice is not recoverable.

---

**Phase 5 — Effect Resolution (Step 5a)**

- **If a PRS restricts a slot type and a player has already placed a die on that slot in Phase 4:** the die is consumed without effect. No retroactive placement correction is permitted. The party was informed of the PRS in Phase 2 and is responsible for their Phase 4 decisions.

---

**Phase 5 — Effect Resolution (Step 5b)**

- **If a player's only active slots are all PRS-restricted (all active slots are of the restricted type, all other slots are inert):** The player's dice are consumed without effect this Phase 5. This is an intended consequence of the degradation arc — the board has reached a state where Heaven's constraint and the player's own degradation together have authored a round of witness rather than action. The player's role this round is to observe and to route hits in Phase 6. This state does not persist: if the PRS constraint changes next round (different Angel face rolled), the player re-enters action normally. *(This edge case is acknowledged as a Pillar 1 / Pillar 2 design tension; the spectator round is accepted as a narrative consequence of full degradation under constraint. See Player Fantasy.)*

- **If a player board is destroyed mid-Step 5b and D_turn on any Angel has accumulated to ≥ HP_eff at that moment (sufficient damage for a strip) but Phase 5c has not yet fired:** Phase 5c does not execute. The accumulated damage is not evaluated against HP_eff. The Angel is not Cleared by this damage. Win/Loss check fires for board destruction only (Loss for non-God Angels; see Win/Loss collision rule). The damage tracker resets at encounter cleanup. *(Exception: if the board destruction was triggered by a self-strip effect that simultaneously stripped the Angel's last layer as part of the same die's effect — per the Martyr edge case above — the strip is evaluated at the moment of the effect, not at Phase 5c.)*

- **If the Pilgrim's Accumulate 8 threshold fires and sets the Penetrating flag, with multiple damage effects still pending in the same Step 5b:** the flag attaches to the next single damage event the party declares after the flag is set. That event is Penetrating and consumes the flag. Subsequent events in the same step are non-Penetrating.

- **If a Penetrating damage event meets or exceeds the HP of two Angel layers simultaneously (e.g., exposed layer HP 12, hidden layer HP 10, Penetrating hit = 15):** both layers strip in sequence, outermost first. Equipment Degradation Rule 1 executes once per stripped layer. Each newly exposed layer's face map is inert for the remainder of Phase 5. Phase 6 reads from the final exposed layer's face map if the Angel was not Cleared.

- **If the Pilgrim's Reservoir release is the next damage event after the Penetrating flag is set:** the Reservoir release is Penetrating. The party may deliberately order resolution to achieve this — this is a cooperative decision point (see Detailed Design). The flag is consumed after the Reservoir's damage resolves.

- **If a self-strip effect (e.g., Martyr's Weapon) reveals a new layer whose passive would modify current-phase damage:** the new layer is inert for the remainder of Phase 5. The passive does not apply to any effect in the current phase. It becomes live from Phase 4 of the next round.

- **If the Martyr's Weapon fires its self-strip and all three slots become inert mid-Step 5b (board destruction triggered):** all dice already committed and unresolved on the Martyr's board still fire (Integrity System Rule 13). After the last committed die resolves, the Win/Loss check fires. If the final Weapon effect also stripped the last layer of the **God card** in the same step: both conditions are active → **Victory prevails** (Win/Loss GDD Rule 6 — God-card-only exception). If the final Weapon effect stripped the last layer of a non-God encounter Angel: both conditions are active → **Loss takes priority** (Win/Loss GDD edge case). If no Angel layer was cleared: **Loss**. Phase 5c and Phase 6 do not execute in all cases.

---

**Phase 5 — Strip Check (Step 5c)**

- **If one Angel is cleared by a Penetrating hit and a surviving second Angel's SRG escalation reads "if this is an inner layer" — and that inner layer was revealed in the same Step 5c:** escalation conditions are evaluated at the moment the SRG fires in Phase 6. The layer exposed during Phase 5 is the active layer by Phase 6 start. If it qualifies as an inner layer per its card text, the escalation fires with full effect.

---

**Phase 6 — Enemy Resolution**

- **If a Targeted hit specifies a slot that was emptied by a prior hit in the same Phase 6:** the hit is wasted. Slot eligibility is evaluated at the moment of assignment; the now-inert slot is not an eligible target. The hit does not redirect to another slot. Remaining hits in the action continue normally.

---

**Encounter End — Win/Loss**

- **If a player board is destroyed and an encounter-level Angel (non-God) is also cleared in the same turn:** Win/Loss collision — **loss takes priority**. A destroyed party does not receive an encounter win result even if their final effect cleared the Angel in the same phase. *(God-card exception: if the simultaneous Angel cleared is the God card, see Win/Loss Conditions GDD Rule 6.)*

- **If the Pilgrim's Penetrating flag was set but never consumed before the encounter ends:** the flag is discarded at encounter cleanup. It does not carry forward to the next encounter. The Accumulate threshold was met; the flag expired without use.

---

**Encounter End — Cleanup**

- **If Accumulate running totals are active on surviving layers when an encounter ends:** the totals persist into the next encounter. They are not reset at encounter end.

- **If inert slots remain at encounter end:** they remain inert at the start of the next encounter. In-session degradation is permanent. No automatic restoration occurs between encounters.

---

**EPC Formula**

- **If T_round is provided outside its defined range [37, 193] seconds:** EPC returns INVALID_INPUT. It does not produce a PASS or WARN result. Correct T_round before re-running.

- **If EPC returns PASS but WAS (Angel System Formula 1) is below 1.5 for all layers:** the encounter is fast but passive — it completes within the time window but delivers insufficient threat. A PASS on EPC with WAS < 1.5 requires Angel redesign. Check both formulas jointly.

## Dependencies

**Upstream dependencies** (systems the Combat System depends on):

| System | Dependency Type | What Combat System Consumes |
|---|---|---|
| **Dice Economy** | Hard | Full turn-flow contract: Placement Phase and Effect Resolution Phase timing; N notation (placed die face value); the "effects fire only after all players have placed all dice" invariant; Accumulate total update rules |
| **Integrity System** | Hard | Hit routing rules (party table decision, slot eligibility); board-destruction trigger (all slots inert); Angel layer damage aggregation (no carryover, single check at phase end); T_angel and D_agg formulas for encounter calibration |
| **Equipment System** | Hard | Slot structure (Head/Body/Weapon); die-to-slot assignment procedure; effect resolution order (party-chosen); slot capacity rules (single-die / multi-die / passive) |
| **Equipment Degradation** | Hard | Layer Reveal Procedure (6-step atomic, triggered by strip event or self-strip); item departure rule; new-layer passive inertness rule (inert for remainder of current phase) |
| **Character System** | Hard | Player board structure; divine gift timing windows (Phase 4, Character System Rule 5); variable layer counts (Pilgrim L_total=8; Martyr L_total=10); starting equipment assignment |
| **Angel System** | Hard | Angel board structure; face map anatomy; action taxonomy (HIT/SRG/PRS/BUFF); multi-Angel shared 3D6 roll; multi-Angel resolution order (Angel System Rule 7.5); BUFF/PRS activation at Phase 5 start; WAS formula for encounter design validation |
| **Item System** | Hard | Distribution procedure (Item System Rule 3); First Player role in distribution; item attachment rules; item departure rule (defers to Equipment Degradation) |

**Downstream dependents** (systems that depend on the Combat System):

| System | Dependency Type | What They Consume |
|---|---|---|
| **Win/Loss Conditions** | Hard | "Angel Cleared" trigger (Phase 5 end); "Player Board Destroyed" trigger (Phase 5 or 6 end). Win/Loss owns game-end; Combat System produces the triggers. |
| **Events System** | Hard | Encounter entry trigger (encounter Event card → Combat begins); encounter-end signal (Combat complete → Event card resolved → Events System advances). |

**Bidirectional consistency notes:**
- Dice Economy GDD must list Combat System in its `referenced_by` for the turn-flow rules.
- Integrity System GDD must agree: hit routing happens in Phase 6; damage checks happen at Phase 5 end.
- Equipment Degradation GDD: add `combat-system.md` to `referenced_by` for N_strips and A_weapon registry entries (Phase 5b registry update). Both GDDs must agree: layer reveals triggered by Phase 5 strip checks and Phase 6 hit routing follow the same 6-step procedure.
- Angel System GDD must agree: BUFF/PRS activates at Phase 5 start; HIT/SRG fires in Phase 6.

**Deferred items resolved in this GDD:**

- ✅ **ED-D-2 (multi-player strip arbitration)**: Hit routing is a party table decision — not any one player's unilateral choice. Confirmed in Phase 6 rules. Communication Protocol GDD owns the formal tiebreaker if the party cannot reach consensus.
- ✅ **Item System OQ-2 (First Player token rotation)**: Token rotates clockwise after each Event card is fully resolved (including non-combat events and the post-combat item award). Confirmed in Encounter Cleanup step 6.
- ✅ **ED-D-1 (restoration rules — partial)**: Integrity System defines restoration via explicit card text. Combat System confirms inert slots persist between encounters (no automatic restoration). Full restoration design is owned by the individual card text — no additional Combat System rule required.

## Tuning Knobs

**1. T_round coefficients (α_4, α_5, α_6, β)**
- **Adjusts**: Expected real-time duration of one combat round
- **α_4** (Placement time per player): baseline = 10 s. Range [5, 15].
- **α_5** (Resolution time per player): baseline = 10 s. Range [5, 15].
- **α_6** (Routing time per player): baseline = 5 s. Range [2, 7].
- **β** (Item overhead): baseline = 15 s. Range [10, 20].
- **Too low**: Underestimates table time; EPC projects encounters as feasible when they're not
- **Too high**: Overestimates; encounter designers over-shrink Angel HP values, producing trivial fights
- **How to update**: After 5+ playtest sessions, average observed round duration by player count and item load; replace baselines with measured values

**2. EPC turn target band [T_lo, T_hi]**
- **Adjusts**: What is considered valid encounter length in turns
- **Current derivation**: 5–8 minutes → at T_round=90 s, valid band = [4, 5] turns
- **If widened** (5–10 minutes): T_hi increases; more complex 6–7 turn Angels permitted for experienced groups
- **If narrowed** (5–6 minutes): Very few Angels PASS — requires either fast T_round or low HP_L values

**3. Post-combat item award**
- **Adjusts**: Item acquisition rate per session
- **Current value**: 1 item per Angel defeated (guaranteed, Win encounters only)
- **If increased (2 items)**: Builds compound faster; S_combined advisory gate (≤85) reached sooner
- **If conditional** (item award only when ≥2 layers stripped): Reduces rate for quick-clear encounters; creates incentive around encounter depth vs. speed
- **Interaction**: Item pool has 10 cards at MVP. Check pool size against expected encounter count before distribution rule changes.

**4. First Player token rotation cadence**
- **Adjusts**: How frequently item distribution authority rotates
- **Current value**: Once per Event card resolved (combat and non-combat)
- **If slowed (every 2 cards)**: One player controls distribution for pairs of events; short-run power concentration
- **If combat-only**: Players drawing treasure events outside combat cannot leverage First Player authority on post-combat items
- **Safe range**: Once per card (current) to once per 2 cards

**5. Angel count scaling (per encounter card)**
- **Adjusts**: Combined threat pressure and Phase 6 complexity
- **Set by**: Individual encounter cards (Events System). This GDD defines multi-Angel combat rules; encounter card authors set the actual counts.
- **MVP recommendation**: 1 Angel at all party sizes for the first prototype. Multi-Angel encounters add Phase 6 complexity before the base loop is validated.
- **If M > N+1 per party tier**: Routing decisions become impossible within the Fast and Focused target

**6. Win/Loss collision priority (LOCKED: Loss prevails for non-God encounters)**
- **Current state**: For encounter-level Angels (non-God), if both "Angel cleared" and "player board destroyed" occur in the same round, **loss takes priority**. A board-destroyed party does not receive an encounter win result. *(Reverted 2026-05-26 — prior update incorrectly applied Win/Loss GDD Rule 6 to encounter-level Angels. Rule 6 applies exclusively to the God card. The authored-failure design pillar supports Loss prevailing: the table traded survival for the kill, and the system honors that signature as a loss.)*
- **God card exception**: If a player board is destroyed and the God card is simultaneously cleared in the same Effect Resolution Phase, **Victory prevails** (Win/Loss Conditions GDD Rule 6 — session-level termination). This exception is fully owned by Win/Loss GDD and does not require a Combat System rule change.
- **If changed to "Victory prevails for all"**: Requires updating the Win/Loss GDD edge case section. Would mean any Angel clear with simultaneous board destruction = Won — undermining the authored-failure risk economy.
- **Recommended status**: Locked for MVP.

**7. Inert slot persistence (LOCKED: permanent within session)**
- **Current state**: Inert slots persist between encounters. No automatic restoration.
- **If partial restoration** (one free layer after each Win): Creates tension between taking restoration and holding item-anchor real estate.
- **If full reset** (boards reset between encounters): Degradation is encounter-scoped only. Session-level arc is removed.
- **Recommended status**: Locked as permanent for MVP. Session-arc degradation is load-bearing for Pillar 2 (The Build is Never Finished).

## Visual/Audio Requirements

*Not applicable — S-Layers is a physical board game. Visual and audio requirements are owned by the Art Bible and component specification documents.*

## UI Requirements

*Not applicable — S-Layers is a physical board game. Table readability requirements (60–100 cm sightlines for Angel boards, degradation indicators, PRS constraint icons) are specified per-component in the Art Bible and component spec.*

## Acceptance Criteria

### Phase 1 — Enemy Roll

**AC-01** Given 1 or more Angels in the Angel Zone, when Phase 1 begins, 3D6 are rolled and the median face is identified and displayed face-up. *Verified by:* set up a single Angel board, roll Phase 1 five times, confirm median is always face-up and accessible to all players.

**AC-02** Given 3D6 yielding two or more identical values (e.g., 2,2,5 or 1,5,5 or 3,3,3), when Phase 1 resolves, the dice are sorted lowest to highest and the middle position is the committed face — **no re-roll occurs**. *Verified by:* manually set three dice to (2,2,5) and confirm committed face = 2; set to (1,5,5) and confirm committed face = 5; set to (3,3,3) and confirm committed face = 3. In all cases confirm no dice are re-rolled.

### Phase 2 — Action Assignment

**AC-03** Given the committed face identified in Phase 1, when Phase 2 executes, the action type (HIT / SRG / BUFF / PRS) is publicly announced before any player touches a die. *Verified by:* playtest debrief — ask players "did you know the action type before placing?" post-round.

**AC-04** Given a PRS action, when Phase 2 executes, the specific slot constraints are readable by all players at 60–100 cm table distance. *Verified by:* prototype readability test at 80 cm.

**AC-05** Given Phase 2 complete, when Phase 3 begins, no game-state change has occurred. *Verified by:* inspect all player boards and Angel boards after Phase 2 — all values unchanged from Phase 1 end.

### Phase 3 — Player Roll

**AC-06** Given Phase 3, each player rolls exactly 2D6 and all results are visible to all players simultaneously. *Verified by:* playtest observation — confirm no secret rolls occur in Phase 3.

### Phase 4 — Dice Placement

**AC-07** Given a PRS constraint announced in Phase 2, when Phase 4 ends and Phase 5 begins, dice placed on restricted slots are consumed and do not fire. *Verified by:* place a die on a PRS-restricted slot, run Phase 5, confirm the die effect does not resolve and the die is discarded.

**AC-08** Given Phase 4 complete, when Phase 5 begins, no effects have fired. *Verified by:* run Phase 4 in isolation and confirm no Angel HP changes, no Integrity changes, no layer reveals occurred.

### Phase 5 — Player Resolution

**AC-09** Given a BUFF action from Phase 2, when Phase 5 begins, HP_eff = HP_L + BUFF_value is announced before any die effect resolves. *Verified by:* set up a BUFF scenario, confirm HP_eff is announced first and used for the damage check at Phase 5 end.

**AC-10** Given a PRS action from Phase 2, when Phase 5 begins, the PRS constraint activates before any die effect resolves. *Verified by:* confirm PRS announcement precedes all die resolution in Phase 5.

**AC-11** Given multiple placed dice, when the party chooses a resolution order in Phase 5, effects resolve in the chosen order. *Verified by:* run Phase 5 with two dice that have order-dependent effects; confirm chosen order is honored.

**AC-12** Given total player damage at Phase 5 end, when damage < HP_eff, no layer is stripped. *Verified by:* set HP_eff = 10, deal 9 damage, confirm layer remains.

**AC-13** Given total player damage at Phase 5 end, when damage ≥ HP_eff, exactly one layer strip and reveal triggers. *Verified by:* set HP_eff = 10, deal 10 damage, confirm Equipment Degradation fires exactly once.

**AC-14** Given a layer stripped in Phase 5, when underspent damage remains (damage > HP_eff), the excess is discarded. *Verified by:* deal 15 damage to a 10-HP layer and confirm no damage carries to the next layer.

### Phase 6 — Enemy Resolution

**AC-15** Given a HIT action, when Phase 6 resolves, the party routes the hit to exactly one slot per the Integrity System routing rules. *Verified by:* run a HIT round and confirm routing decision is called, Integrity decrements correctly, and only one slot is affected.

**AC-16** Given a SRG action with an inner-layer escalation condition, when Phase 6 resolves: (a) the base surge hit count fires first — two routing decisions occur; (b) the escalation clause is evaluated after the base hits resolve; (c) if the escalation condition is met (Angel is on an inner layer per card text), escalation hits fire as additional routing decisions. *Verified by:* run a SRG round with an Angel on an inner layer whose escalation condition is met; confirm base hits fire first; confirm escalation hits fire afterward as additional routing decisions. *(Note: SRG escalation clause definition is owned by Angel System — see AC-35 for multi-Angel interaction.)*

**AC-17** Given a BUFF or PRS action (no HIT/SRG), when Phase 6 resolves, no hit is applied. *Verified by:* run a BUFF round through Phase 6, confirm no player board state changes.

**AC-18** Given Phase 6 complete with a player board destroyed by hit routing and the encounter Angel not cleared, when the Win/Loss check fires, the game transitions to Loss and the encounter ends. *Verified by:* construct a scenario where Phase 6 hit routing empties a player's final active slot; confirm Loss is declared, the encounter ends, and cleanup executes without proceeding to the next round.

**AC-19** Given the encounter not ended after Phase 6, when Phase 1 of the next round begins, round counter increments and sequence restarts. *Verified by:* run a 2-round encounter, confirm second Phase 1 receives correct round count.

### Win / Loss

**AC-20** Given all slots on a player board inert, when that state is reached, Loss is declared and the encounter ends immediately. *Verified by:* reduce a player board to all-inert state during Phase 6, confirm encounter terminates.

**AC-21** Given an Angel with zero layers remaining, when that state is reached, Win is declared and the encounter ends immediately. *Verified by:* strip the final layer in Phase 5, confirm encounter terminates.

**AC-22a** Given a non-God encounter Angel as the active Angel, and a player board is destroyed simultaneously with that Angel's final layer being stripped in the same Effect Resolution Phase, when the Win/Loss check fires, the result is **Loss** (encounter ends; session continues with degraded board state). *Verified by:* construct a Martyr Weapon scenario against a non-God Angel where self-strip simultaneously destroys the Martyr's board and strips the Angel's last layer in the same Step 5b; confirm Loss is declared, not Won. *(Ruling: Loss takes priority for non-God encounters — authored-failure pillar; the table traded survival for the kill.)*

**AC-22b** Given the God card as the active encounter Angel, and a player board is destroyed simultaneously with God's final layer being stripped in the same Effect Resolution Phase, when the Win/Loss check fires, the session transitions to **Won** (Victory prevails — Win/Loss GDD Rule 6). *Verified by:* construct the same simultaneous scenario against the God card; confirm Won is declared, not Lost, and the session ends. *(Ruling: Victory prevails is exclusively the God card exception. The dungeon itself ends the moment God is cleared.)*

### Encounter Cleanup & Persistence

**AC-23** Given an encounter ending, when cleanup executes, all inert slots on player boards remain inert at the start of the next encounter. *Verified by:* end an encounter with one inert slot; begin next encounter and confirm slot is still inert (not restored).

**AC-24** Given an encounter ending, when cleanup executes, Accumulate totals on surviving (non-stripped) layers persist into the next encounter. *Verified by:* bank 3 Reservoir charges mid-encounter; win encounter; begin next encounter and confirm 3 charges remain banked.

**AC-25** Given an Event card resolved (combat or non-combat), when cleanup executes, the First Player token passes clockwise to the next player. *Verified by:* run two consecutive Event cards, confirm the token holder changes after each.

### Formulas

**AC-26 [DEFERRED — playtest]** Given T_round constants (T_base, α_4, α_5, α_6, β), when calibrated against playtests, T_round at n=2, C_item=1 is 90 ± 15 seconds. *Verified by:* stopwatch playtest of 10+ rounds with 2 players and 1 active item; confirm mean within range.

**AC-27** Given T_round = 90 s (T_lo = 4, T_hi = 5), when EPC is run: T_encounter_actual = 4 turns → PASS; T_encounter_actual = 5 turns → PASS; T_encounter_actual = 6 turns → WARN_LONG (6 > T_hi = 5). *Verified by:* compute EPC for T_encounter_actual ∈ {4, 5, 6} at T_round = 90 s; confirm 4 → PASS, 5 → PASS, 6 → WARN_LONG.

**AC-28** Given an encounter design where T_encounter_actual < T_lo or > T_hi, when EPC is run, the formula emits WARN with the actual value. *Verified by:* compute EPC for T_encounter_actual = 2 (below minimum); confirm WARN is emitted.

**AC-29** Given invalid EPC input (T_round ≤ 0 or T_encounter_actual ≤ 0), when EPC is run, the formula returns an error — not a spurious PASS. *Verified by:* input T_round = 0; confirm output is not a valid PASS or WARN result.

### Edge Case

**AC-30a** Given the Penetrating flag active and an Angel with an Exposed layer (HP=8) and one Hidden layer (HP=10), when a Penetrating damage event of 15 is declared in Step 5b, both layers strip in sequence (outermost first) and Equipment Degradation Rule 1 executes once per stripped layer. The newly innermost layer is inert for the remainder of Phase 5. *Verified by:* set up the two-layer Angel and activate Penetrating; fire a 15-damage event; confirm two strip events fire in sequence; confirm the newly exposed layer is inert for the rest of Phase 5.

**AC-30b** Given the Penetrating flag was set mid-Step 5b and consumed by the first damage event declared after it was set, when the next round begins at Phase 3, the Penetrating flag is absent on the Pilgrim's board. *Verified by:* activate Penetrating in Phase 5, fire one damage event to consume the flag, complete the round, begin next Phase 3, confirm flag is absent.

### Edge Case Coverage (AC-31–AC-38)

**AC-31** Given the Pilgrim has an extra die from a prior-turn gift and a PRS action from Phase 2 locks all slot types on the Pilgrim's board, when Phase 4 ends and Phase 5 begins, all three dice (extra die + 2D6) are discarded without placement and produce no effects. *Verified by:* set up Pilgrim with extra die from divine gift; set PRS to restrict all slot types on the Pilgrim's board; confirm all three dice are discarded in Phase 5 with no effect firing.

**AC-32** Given the Pilgrim's Accumulate 8 threshold fires mid-Step 5b setting the Penetrating flag, with two additional damage effects still pending in the same step, when the party declares the next damage event after the flag is set, that single event is Penetrating and the flag is consumed. The subsequent damage event is not Penetrating. *Verified by:* stage a Step 5b with Accumulate 8 crossing mid-step; queue two subsequent damage effects; confirm only the immediately next declared event is Penetrating; confirm the third event is non-Penetrating.

**AC-33** Given a self-strip effect (e.g., Martyr Weapon) strips a layer during Step 5b, revealing a new layer whose passive would modify damage output, when remaining dice in Step 5b continue to fire, the newly revealed layer's passive does not apply for the remainder of Phase 5. *Verified by:* trigger a mid-step self-strip revealing a passive that buffs damage; confirm subsequent dice in the same Step 5b fire at pre-passive output values; confirm the passive is active from Phase 4 of the next round.

**AC-34** Given the Martyr's Weapon fires a self-strip that destroys the Martyr's board mid-Step 5b (all slots reach inert), when board destruction is triggered, any dice already committed and unresolved on the Martyr's board still fire before the Win/Loss check. Phase 5c and Phase 6 do not execute. *Verified by:* set up a Martyr with two committed dice where the first die triggers board destruction via self-strip; confirm the second die fires before the Win/Loss check; confirm Phase 5c is skipped; confirm Phase 6 is skipped.

**AC-35** Given two Angels in play where Angel A is cleared by a Penetrating hit at Step 5c, and Angel B has a SRG action whose escalation condition references "inner layer," when Phase 6 evaluates Angel B's SRG, the escalation condition is evaluated against Angel B's currently exposed layer (which may have changed during Phase 5 if Angel B was also affected). *Verified by:* set up a two-Angel encounter; clear Angel A via Penetrating; ensure Angel B's exposed layer qualifies as inner layer; run Phase 6; confirm Angel B's SRG escalation fires based on the layer state at Phase 6 start, not Phase 2 state.

**AC-36** Given a Targeted hit in Phase 6 that specifies a slot type already emptied (inert) by a prior hit in the same Phase 6, when that hit is assigned, it is wasted and does not redirect to another slot or slot type. *Verified by:* run a Phase 6 with an Angel delivering two consecutive Targeted hits to the same slot; confirm the first hit routes and strips normally; confirm the second hit is wasted with no effect and no redirect.

**AC-37** Given the Penetrating flag was set mid-Step 5b but not consumed before the encounter ends (no remaining damage events declared), when encounter cleanup executes, the Penetrating flag is discarded. It does not carry forward to the start of the next encounter. *Verified by:* set Penetrating flag on Pilgrim; end the encounter before any subsequent damage event fires; begin next encounter at Encounter Entry; confirm Penetrating flag is absent on Pilgrim's board.

**AC-38** Given two Angels in play where Angel A commits BUFF and Angel B commits PRS in the same round, when Step 5a executes: (a) Angel A's BUFF raises HP_eff for Angel A's layer only; (b) Angel B's PRS restricts the stated slot type(s) for all players; (c) both activate in encounter-card order; (d) neither cancels the other. *Verified by:* set up a two-Angel round with BUFF on Angel A and PRS on Angel B; run Step 5a; confirm HP_eff is raised for Angel A only; confirm restricted slots do not fire; confirm both activations occur and neither cancels the other.

## Open Questions

**OQ-1 [RESOLVED 2026-05-26]** Penetrating multi-strip firing order: cascade is permitted (Option A). Outermost layer strips first; on-destroy passives fire in outermost-first sequence; HP_eff for each newly revealed layer is recalculated fresh (HP_L + active BUFF this turn). Integrity Rule 20 updated with Penetrating exception. Angel Rule 6.6 updated with Penetrating exception. CS-AC-30 resolved in Character System Open Flags.

**OQ-2 [PARTIALLY RESOLVED 2026-05-26]** T_round calibration: α_4, α_5, α_6, and β baseline values in the formula table (10 s, 10 s, 5 s, 15 s per-player respectively) are provisional estimates. Actual values must be measured against stopwatch playtests at MVP. AC-26 is deferred until playtest data is available. *(Registry entities.yaml updated 2026-05-26 — cross-review B-CON-1 fix — to match GDD-body coefficients and output_range [37, 193]. Internal Formula 2 table range also corrected to [37, 193]. Coefficient calibration itself remains DEFERRED to playtests.)*

**OQ-3 [RESOLVED in this GDD — confirm in Angel System]** Multi-Angel encounter: all Angels share one 3D6 roll per round (Phase 1 Rule 1). Each Angel reads the median face independently against its own layer's face map (Phase 2). This design decision is made here. The Angel System GDD must confirm this approach and define any Angel-specific exceptions (e.g., a unique Angel that rolls separately). No Combat System rule change required.

**OQ-4 [TRACKING]** Item System OQ-6 (Restore + re-equip loop): verify no perverse loop exists when a layer is restored and the same item is available for re-attachment within the same encounter. Low priority at concept stage; flag for Combat System playtest pass.
