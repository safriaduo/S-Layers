# Angel System

> **Status**: Designed — CD-GDD-ALIGN: CONCERNS addressed (2026-05-25)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-25
> **Implements Pillar**: Agency Over Dice (primary); Fast and Focused (secondary); Cooperative Ownership (supporting)

## Overview

The Angel System defines the adversary side of every combat encounter in S-Layers. Each Angel is represented by a physical board: a stack of **layer cards**, each carrying its own **face map** (six die faces, each mapped to a unique action for that layer) and an explicit **HP value**. The number of layers varies by encounter type — from a single-layer skirmish Angel to a four-layer boss — and as the party strips each layer by meeting its HP threshold, the Angel's face map changes: the outer layer's calculated cruelty gives way to whatever is underneath. The Angel rolls three six-sided dice each round; the middle die (the 3D6 median roll defined by the Dice Economy) determines which action fires. This result is revealed to all players **before** the placement phase begins — the Angel is not a random hazard but a known, committed threat whose next move is visible and whose consequences the party must plan around or absorb. Actions include party hits, targeted hits, and non-damage effects; the mix is unique per Angel per layer. The Angel System owns the board structure, the face-map assignments, the HP values on each layer, and the enemy action resolution order. It does not define how players respond to hits (Integrity System owns routing), how dice are placed to deal damage (Equipment System), or how the turn phases sequence (Combat System).

## Player Fantasy

An Angel is not a monster. A monster can be killed and forgotten. An Angel is an *instrument* — something Heaven built and posted to hold this door, this stair, this hour against you. It rolls its three dice in plain sight and shows you what it will do before a single one of yours has left your hand: foreknowledge offered as a courtesy, because the outcome was never in doubt. Your work is not to be surprised. Your work is to look at what was promised, and route the harm through the bodies and the gear and the dice you brought, until the instrument is quieter than it was. Then you climb to the next door.

The layer beneath is not weaker. It is different. A new face map, a new geometry of actions — the Angel does not diminish as it is damaged; it *changes*. Stripping a layer is not a victory lap. It is a revelation. You learn something about the instrument. It teaches you what it is made of. You were afraid of the outer face. Now you know there was something underneath, and it has been watching you since the encounter started.

**Design test**: If a player says — mid-encounter, after the Angel's middle die settles — *"okay, that's manageable"* and then begins telling their teammates exactly how to route the coming hits: the system has worked. The foreknowledge created decision, not helplessness. If the players react to the Angel's revealed action with confusion, silence, or resigned acceptance rather than active deliberation: the action is too complex (violating *Fast and Focused*) or the actions feel arbitrary rather than readable (violating this fantasy — the instrument must be legible). If stripping a layer produces no visible change in the party's strategy (they play the next round exactly the same way), the face-map escalation has failed to deliver.

## Detailed Design

### Core Rules

**1. Angel Board Physical Structure**

1. Each encounter places one or more **Angel boards** — physical stacks of layer cards — in a dedicated **Angel Zone** at the center of the table, visible to all players simultaneously.
2. The number of Angels in play is determined by the encounter event card, scaled to party size. Each encounter card states: *"1–2 players: [N] Angel(s). 3–4 players: [M] Angel(s)."*
3. Each Angel's board is a stack of **layer cards** arranged in encounter order: Layer 1 on top (outermost, first to be fought), deepest layer on the bottom (last to be stripped).
4. **Exposed layer**: The top card of the stack — the currently active layer. Its HP value and face map are face-up and visible to all players.
5. **Hidden layers**: All cards beneath the Exposed layer are face-down. HP values and face maps are not visible until the layer above is stripped.
6. Each Angel has a dedicated **Damage Tracker Die** — a spare d6 placed beside its stack in the Angel Zone. It tracks cumulative damage dealt to that Angel this turn. If the running total exceeds 6, a second die serves as the tens digit (per the Integrity System's two-die tracking rule).
7. **All Angels in an encounter share one 3D6 roll per round.** A single set of three dice is rolled in the Angel Roll Phase. Every Angel applies the same median face value to its own Exposed layer's face map.

---

**2. Layer Card Anatomy**

| Field | Description | Visibility |
|---|---|---|
| **HP value** | Integer. Printed prominently. The damage threshold per turn to strip this layer. | Face-up when Exposed; face-down when Hidden |
| **Face Map** | Six entries: one action per face (1–6). Each entry ≤ 8 words. One action type code (HIT / SRG / PRS / BUFF) per entry. | Face-up when Exposed; face-down when Hidden |
| **Layer Index** | Ordinal label: "Layer 1 / 3", "Layer 2 / 3", etc. Printed on card back. | Always visible (card back) |
| **Angel Name / Tag** | Confirms which Angel this layer belongs to. | Face-up when Exposed |
| **Target Party Size** | The party size range this layer was balanced for (e.g., "1–2" or "3–4"). | Face-up when Exposed |

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

**4. Face Map Design Rules**

**Rule 4.1 — Maximum 3 action types per layer.** No single layer may use more than 3 distinct action categories across its 6 faces. A player learning a new layer sees at most 3 types of threat.

**Rule 4.2 — At least one face per layer must be HIT or SRG.** An Angel that cannot deal hits is not a credible obstacle.

**Rule 4.3 — Severity Calibration.** Define a Severity Score per face action:

| Severity | Score | Example |
|---|---|---|
| 0 | Light BUFF or light PRS | BUFF +2; "Placements on Weapon slots resolve last" |
| 1 | Significant PRS | "Head slots may not receive dice this turn" |
| 2 | HIT: 1 party hit | 1 freely-routed hit |
| 3 | HIT: 1 targeted hit or heavy BUFF | "Hit: the player with the fewest layers"; BUFF +5 |
| 4 | HIT: 2 party hits | 2 freely-routed hits |
| 5 | SRG | Escalating 2+ hits |

**Weighted Average Severity (WAS):** `WAS = Σ(P_i × S_i)` where P_i is P_angel(i) from the Dice Economy.

| WAS Band | Design Intent |
|---|---|
| < 1.5 | Passive — light pressure. Valid for outer layer of a boss. |
| 1.5–2.5 | Balanced — sustained threat. Target for most MVP layers. |
| 2.5–3.5 | Aggressive — valid for inner layers and hard Angels. |
| > 3.5 | Degenerate — party cannot plan. Do not publish. |

**Rule 4.4 — Common-face constraint.** Faces 3 and 4 (each ~24.1%) may not both carry Severity ≥ 4 on the same layer. At least one of them must be Severity ≤ 3. This ensures the most frequent faces are never simultaneously the most punishing.

**Rule 4.5 — Rare-face latitude.** Faces 1 and 6 (each ~7.4%) may carry SRG or any Severity ≤ 5, provided WAS stays within band. A high-severity rare face is more dramatic because it fires infrequently.

**Rule 4.6 — BUFF faces must carry a guaranteed secondary component.** Every face map entry that includes a BUFF action must also include a secondary component that fires unconditionally — regardless of whether the party is dealing damage to that Angel this turn. The secondary component must be one of:

- A **PRS constraint**: e.g., *"BUFF +4. PRS: Weapon slots resolve last."*
- A **HIT or SRG**: e.g., *"BUFF +3. HIT: 1 party hit."*

The secondary component fires exactly as its action type specifies — PRS at the start of the Effect Resolution Phase, HIT/SRG in the Angel Resolution Phase. A BUFF face carrying only a threshold adjustment and no secondary component is an **illegal face map entry**. Card authors must verify:
1. The secondary component is present (the entry has two parts).
2. The secondary component is not vacuously satisfied — a PRS restricting a slot type absent from all expected character builds in the target encounter does not satisfy this rule. Choose a slot type that is structurally present in all expected character configurations.

*Rationale*: Without a guaranteed secondary component, a BUFF face on an Angel the party is not currently targeting produces zero table effect — the threshold is raised on a tracker that will not be tested. This is not a dramatic tension moment; it is a wasted face. The secondary component ensures every BUFF face creates a table event regardless of party targeting decisions.

---

**5. Action Resolution Order**

**Rule 5.1 — PRS and BUFF fire at the start of the Effect Resolution Phase** — before any player dice resolve. Players see both the constraint and the modified threshold before committing to die resolution order.

**Rule 5.2 — HIT and SRG fire in the Angel Resolution Phase** — after all player effects have resolved, per the Dice Economy's timing lock.

**Rule 5.3 — Compound action sequence.** If a face entry contains multiple components (e.g., "PRS: Head slots disabled. HIT: 1 party hit"), components resolve in printed order: PRS/BUFF at phase start, HIT/SRG in the Angel Resolution Phase.

**Rule 5.4 — Hit routing is party-chosen.** The face map states hit count and type. The party decides the routing order, subject to Integrity System routing rules.

**Rule 5.5 — Surge sequence.** Base hits resolve first. Then the escalation clause is evaluated. If met, additional hits fire immediately in party-chosen routing order.

**Rule 5.6 — Multiple-Angel announcement order.** When multiple Angels are in play, all BUFF/PRS announcements happen together at phase start (Angel A then Angel B, in encounter-card order). All constraints apply simultaneously for the turn.

---

**6. Layer Transition Rules**

**Rule 6.1 — Strip trigger.** At the end of the Effect Resolution Phase, each Angel independently checks: if tracker total ≥ effective HP threshold (HP_L + any active BUFF) → layer stripped.

**Rule 6.2 — Physical strip procedure:**
1. Party announces the tracker total.
2. Compare to effective threshold (HP_L + any active BUFF).
3. **If total ≥ threshold:** Remove the Exposed layer card. Place it face-up in the **Stripped Pile** beside the Angel Zone (kept for reference — players may consult stripped layers at any time).
4. Flip the next card in the stack face-up. Its HP and face map are immediately visible.
5. Reset the Damage Tracker Die to zero.
6. **If no cards remain:** Angel is **Cleared** → Win/Loss check fires immediately.
7. **If total < threshold:** Layer survives. Reset tracker to zero. No state change.

**Rule 6.3 — Hidden-until-strip.** Hidden layer cards are face-down until the moment of strip. Players cannot plan around the new face map before it is revealed.

**Rule 6.4 — Layer transition is a public event.** All players see the new face map simultaneously at the moment of strip.

**Rule 6.5 — One strip check per turn per Angel.** No mid-phase checks. The single check fires at end of Effect Resolution Phase only.

**Rule 6.6 — BUFF does not persist.** If a BUFF fires and the layer survives, next turn's threshold reverts to HP_L. BUFF does not stack across turns.

---

**7. Multi-Angel Rules**

**Rule 7.1 — All Angels share one 3D6 roll.** The single median face value is applied to every Angel's Exposed layer face map this round. One roll, multiple readings.

**Rule 7.2 — Each Angel reads its own face map.** The same face may produce different actions on different Angels. All actions are announced simultaneously at the Angel Roll Phase announcement step.

**Rule 7.3 — Damage is targeted per Angel.** When a damage-dealing effect fires, the active player declares which Angel receives that damage before advancing any tracker. A single effect's output cannot be split between two Angels — it fully targets one.

**Rule 7.4 — Strip checks are independent per Angel.** Both Angels can be stripped in the same turn. Each tracker resets to zero independently after its check.

**Rule 7.5 — Hit resolution order with multiple Angels.** BUFF/PRS announcements from all Angels happen together at phase start. HIT/SRG actions resolve in the Angel Resolution Phase: encounter-card order (Angel A's hits, then Angel B's). If order is not printed, the party declares it before the encounter begins — this choice is binding for the full encounter.

**Rule 7.6 — Encounter continues until all Angels are Cleared.** Clearing one Angel does not end the encounter.

**Rule 7.7 — Board destruction applies normally.** If any player's board is destroyed during a multi-Angel encounter, the Win/Loss check fires immediately, regardless of Angel states.

**Rule 7.8 — Multi-Angel Resolution Ritual.** When multiple Angels are in play, follow this named sequence to prevent mis-reads and ensure all actions are acknowledged before placement begins:

1. The **Lead Player** (the player whose turn it is to lead declarations, or any agreed-upon consistent role) reads the median die face aloud: *"The die shows [N]."*
2. Starting with Angel A (encounter-card order), the Lead Player points to each Angel's Exposed layer card and reads the action on face [N] aloud: *"Angel A — face [N]: [action text]."* Then *"Angel B — face [N]: [action text]."* Continue for all Angels in order.
3. All other players confirm or challenge before any placement occurs. If any player disputes the read, compare against the printed face map immediately and correct before proceeding.
4. Once all actions are confirmed aloud by the table, proceed to step 1 of the placement phase.

The ritual is mandatory when 2 or more Angels are in play. It may be abbreviated by experienced groups ("Two [N]s — Shield Surge on A, Pressure on B — confirmed?") provided all players explicitly confirm before placement begins. No dice are placed until confirmation is given.

**Physical board UX note (downstream requirement for component design):** When the Component Design spec for Angel boards is authored, it must accommodate the multi-Angel resolution ritual. Required considerations: (a) face index numbers must be large enough to cross-reference across multiple boards from a seated position; (b) a reference card or rulebook sidebar should print the ritual as a step-by-step prompt; (c) consider a player-aid insert for encounters with 3+ Angels. This requirement is owned by the Component Design spec — the Angel System defines the requirement; that spec defines the solution.

---

### States and Transitions

**Angel Layer States**

| State | Condition | Face Map Active? | HP Visible? |
|---|---|---|---|
| **Hidden** | In stack beneath the Exposed layer | No | No — face-down |
| **Exposed** | Currently active top card | Yes | Yes |
| **Stripped** | Tracker met effective threshold; card removed to Stripped Pile | No | Yes — Stripped Pile, face-up |
| **Cleared** | All layers Stripped | — | — |

**Per-Turn Flow (Angel System scope)**

```
Angel Roll Phase
  → Roll 3D6. Sort low→high. Median face committed and revealed to all.
  → For each Angel: read Exposed layer face map at committed face.
  → Announce all face-map actions (Angel A, then Angel B, in encounter-card order).

Start of Effect Resolution Phase
  → Announce all active BUFF values (effective threshold = HP_L + BUFF for this turn).
  → Announce all active PRS constraints (apply for this phase only).
  → Players resolve placed-die effects; damage targets declared per effect.
  → Each Angel's Damage Tracker Die advances as targeted damage accrues.

End of Effect Resolution Phase (strip checks — one per Angel, independent)
  → For each Angel:
       If tracker ≥ effective threshold → Stripped: card to Stripped Pile; next layer revealed.
       If tracker < threshold → layer survives. Tracker resets to zero.
  → Angel Cleared check: if no layers remain → Cleared → Win/Loss check immediately.

Angel Resolution Phase
  → For each Angel in resolution order:
       Resolve HIT: party routes hits per Integrity System routing rules.
       Resolve SRG: resolve base hits; evaluate escalation clause; resolve additional hits if met.
  → Round ends.
```

---

### Interactions with Other Systems

| System | Direction | Data / Event | Notes |
|---|---|---|---|
| **Dice Economy** | Inbound | Median face value from 3D6 roll (P_angel) | Dice Economy owns the roll and probability model. Angel System maps the median to an action via the face map. |
| **Integrity System** | Inbound | HP bounds formula: HP_min = ⌈0.6 × D_agg⌉, HP_max = ⌊1.2 × D_agg⌋ | All Angel layer HP values must satisfy these bounds for their target party size. MVP bounds (n=2): [10, 19]. |
| **Integrity System** | Inbound | Hit type definitions: Party hit / Targeted hit | Angel face maps use exactly these two hit types. No other hit grammar is valid. |
| **Integrity System** | Outbound | Hit count and type per face-map action | Integrity System receives and applies routing rules. Angel System does not own routing. |
| **Integrity System** | Outbound | "Layer stripped" event (which layer, which Angel) | Strip trigger per Integrity System Rule 18. |
| **Integrity System** | Outbound | "Angel Cleared" event (per Angel) | Final strip triggers this per Angel. Win/Loss check fires via Integrity System. |
| **Combat System** | Inbound | Phase sequence (Angel Roll → Effect Resolution → Angel Resolution) | Angel System rules execute within Combat System's phase structure. |
| **Win/Loss Conditions** | Outbound | "Angel Cleared" trigger (via Integrity System) | Win/Loss owns the consequence. |
| **Equipment System** | Indirect via PRS | PRS may restrict placement on specific slot types for one turn | Equipment System must handle an externally-imposed placement restriction. |
| **Character System** | Outbound (indirect) | H_pt (expected hits per turn per player) | Derivable from WAS formula; used by Integrity System's T_board verification when authoring Character System. |

## Formulas

*These formulas are design tools for Angel card authors and encounter designers. None are computed at the table. All derive from locked constants in the Dice Economy and Integrity System GDDs.*

**Locked upstream inputs:**

| Symbol | Value | Source GDD |
|---|---|---|
| P_angel(1), P_angel(6) | 0.074 | Dice Economy |
| P_angel(2), P_angel(5) | 0.185 | Dice Economy |
| P_angel(3), P_angel(4) | 0.241 | Dice Economy |
| D_agg (MVP, n=2) | 16 | Integrity System |
| D_player_base | 8 | Integrity System |
| HP_min (MVP) | 10 | Integrity System |
| HP_max (MVP) | 19 | Integrity System |

---

### Formula 1 — WAS: Weighted Average Severity

The `WAS` formula is defined as:

`WAS = Σ (P_angel(i) × S_i)` for i = 1 to 6

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Angel face index | i | int | {1–6} | The die face being evaluated |
| Face probability | P_angel(i) | float | {0.074, 0.185, 0.241, 0.241, 0.185, 0.074} | Probability the Angel's 3D6 median shows face i; locked from Dice Economy |
| Severity score | S_i | int | {0–5} | Severity of the action on face i; see scale below |
| Weighted Average Severity | WAS | float | [0.0, 5.0] | Expected severity per turn for this layer |

**Severity scale:**

| Score | Action class | Example |
|---|---|---|
| 0 | Light BUFF or light PRS | BUFF +2; "Weapon slots resolve last" |
| 1 | Significant PRS | "Head slots may not receive dice this turn" |
| 2 | HIT: 1 party hit | 1 freely-routed hit |
| 3 | HIT: 1 targeted hit or heavy BUFF | "Hit: player with fewest layers"; BUFF +5 |
| 4 | HIT: 2 party hits | 2 freely-routed hits |
| 5 | SRG | Escalating 2+ hits |

**WAS design bands:**

| WAS Band | Design Intent |
|---|---|
| < 1.5 | Passive — light pressure. Valid for outer layer of a boss Angel. |
| 1.5–2.5 | Balanced — sustained threat. Target for most MVP layers. |
| 2.5–3.5 | Aggressive — valid for inner layers and hard Angels. |
| > 3.5 | Degenerate — party cannot plan. Do not publish. |

**Output Range:** WAS ∈ [0, 5] theoretical; publishable band [1.5, 3.5].

**Example:** Face map with BUFF (face 1, S=0), PRS (face 2, S=1), HIT-1-party (faces 3+4, S=2), HIT-1-targeted (face 5, S=3), SRG-2+1 (face 6, S=5):

`WAS = (0.074×0) + (0.185×1) + (0.241×2) + (0.241×2) + (0.185×3) + (0.074×5) = 2.074` → Balanced ✓

---

### Formula 2 — H_pt: Expected Hits per Player per Turn

The `H_pt` formula is defined as:

`H_pt = [Σ (P_angel(i) × C_i)] / n` for i = 1 to 6

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Face probability | P_angel(i) | float | see above | Probability of median face i; locked from Dice Economy |
| Expected hits for face i | C_i | float | [0, ~4] | Hit count delivered when face i fires; derived from action type (see below) |
| Party size | n | int | {1–4} | Active players; assume uniform hit distribution for the baseline formula |
| Hits per player per turn | H_pt | float | [0, ~2] | Expected hits received by one player per turn at uniform routing |

**Deriving C_i by action type:**

| Action | C_i |
|---|---|
| BUFF (any value) | 0 |
| PRS (any type) | 0 |
| HIT: 1 party or targeted hit | 1 |
| HIT: 2 party hits | 2 |
| SRG | base_hits + P(escalation) × escalation_hits; use P(escalation) = 0.5 unless structurally known |

**Output Range:** H_pt ∈ [0.3, 1.5] for publishable layers at MVP (n=2). Use H_pt (not WAS) when verifying T_board in the Character System GDD — BUFF and PRS contribute to WAS but deliver 0 hits.

**Example** (same face map as Formula 1, n=2, inner layer where SRG always escalates):

E_hits = (0.074×0) + (0.185×0) + (0.241×1) + (0.241×1) + (0.185×1) + (0.074×2) = 0.815 hits/turn (party total)

`H_pt = 0.815 / 2 = 0.408 hits per player per turn`

T_board cross-check: character with L_total = 6 → `T_board = 6 / 0.408 = 14.7 turns` — survives well beyond a 6–8 turn encounter. Inner layers with heavier face maps push H_pt toward 1.0–1.5.

**[PENDING: Character System]** — When Character System GDD defines L_total per character, verify T_board = L_total / H_pt ≥ encounter length at each Angel's H_pt. Evaluate at both uniform routing (baseline) and worst-case routing (all hits directed at one player).

---

### Formula 3 — BUFF Effective Threshold

The effective HP threshold for this turn when a BUFF fires is defined as:

`HP_eff = HP_L + BUFF_value` (this turn only)

The layer is stripped this turn if and only if: `D_turn ≥ HP_eff`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Printed layer HP | HP_L | int | [10, 19] (MVP) | HP value printed on the card — does not change |
| BUFF value | BUFF_value | int | [1, HP_max − HP_L] | Amount added to HP_L for this turn only; printed on card as "+X" |
| Effective threshold | HP_eff | int | [HP_min+1, HP_max] | The threshold the party's damage total must meet or exceed to strip this turn |

**Safe BUFF range:** `BUFF_value ≤ HP_max − HP_L`

| HP_L | Max safe BUFF | Notes |
|---|---|---|
| 10 | ≤ 9 | Wide room |
| 14 | ≤ 5 | Meaningful tension |
| 16 | ≤ 3 | Significantly impedes stripping |
| 18 | ≤ 1 | BUFF +1 only |
| 19 | 0 | No BUFF permitted — already at HP_max |

**Design rule:** Do not assign BUFF to a layer where HP_L = HP_max (19 at MVP). Any BUFF on such a layer is degenerate when it fires.

**Output Range:** HP_eff ∈ [HP_min+1, HP_max] = [11, 19] at MVP. HP_eff is a per-turn value — it never persists to the next round.

**Example:** HP_L = 14, face 2 carries BUFF +4 → HP_eff = 18 ≤ 19 ✓. On BUFF turns T_angel(18, 16) ≈ 2.5 turns; on non-BUFF turns T_angel(14, 16) ≈ 1.3 turns. Contrast with degenerate: HP_L = 14, BUFF +8 → HP_eff = 22 > 19. Do not publish.

---

### Formula 4 — Multi-Angel Damage Split Efficiency

*Design calibration tool for encounter designers. Not computed at the table.*

**Setup:** 2 Angels, both HP_L = H. MVP (n=2, D_agg = 16, D_split = 8).

**The `T_focus` formula:**

`T_focus = 2 × T_angel(H, D_agg)`

**The `T_split` formula:**

`T_split = T_angel(H, D_agg / 2)`

**Variables:**

| Variable | Symbol | Type | Range | Description |
|---|---|---|---|---|
| Layer HP (both Angels) | H | int | [10, 19] (MVP) | HP on each Angel's Exposed layer |
| Full party damage | D_agg | float | 16 (MVP) | Party damage per turn; locked from Integrity System |
| Split damage per Angel | D_split | float | D_agg / 2 = 8 (MVP) | Each Angel's share under even split |
| Focus total turns | T_focus | float | [2.0, 6.0] | Expected turns to clear both Angels via focus |
| Split total turns | T_split | float | [T_focus, ∞) | Expected turns under even split — never better than focus |

**Comparison table (MVP, D_agg = 16):**

| H | T_focus | H/D_split | T_split | Verdict |
|---|---|---|---|---|
| 10 | ≈ 2.2 | 1.25 | ≈ 3–4 turns | Focus wins |
| 14 | ≈ 2.6 | 1.75 | Degenerate (>3t) | Focus wins decisively |
| 16 | ≈ 3.0 | 2.00 | Impossible range | Focus wins decisively |

**Key design result:** Within all valid MVP HP bounds, **focus always beats even-split.** The no-carryover rule means partial damage contributes nothing — concentrating fire on one Angel is always more efficient.

**Output Range:** T_focus ∈ [2.0, 6.0] within valid HP bounds. T_split ≥ T_focus always within publishable HP bounds.

**Design implication:** HP calibration alone cannot force the party to split attention across two Angels. Encounter designers who want to require sustained split-focus must use one of these tools:
1. Explicit card rule: "While both Angels are in play, all damage to Angel A is halved"
2. Asymmetric HP values: give Angels very different HP_L so the party naturally focuses the lighter Angel first while maintaining pressure on the heavier one
3. SRG escalation on the surviving Angel when the other is cleared (punishes eliminating one Angel too fast)

## Edge Cases

*Format: **If [condition]**: [exact outcome]. Rationale provided where non-obvious.*

---

### Strip Check Edge Cases

- **If the Damage Tracker total exactly equals HP_eff**: the layer is stripped. The strip condition is `D_turn ≥ HP_eff`; equality is a passing condition. This is consistent with the Integrity System (AC-06).

- **If a BUFF fires and the tracker total already equals HP_L at the moment of announcement (the party would have stripped without BUFF)**: the BUFF raises the threshold to HP_eff = HP_L + BUFF_value. The strip check fires against HP_eff only. If the tracker reads HP_L and HP_eff > HP_L, the layer survives this turn. BUFF announcements happen at the start of the Effect Resolution Phase before any player dice resolve — the new threshold is in force for the entire phase. There is no "would have stripped" state prior to the check.

- **If a BUFF announcement was missed and only discovered after player dice have resolved**: this is a sequencing error. BUFF must be announced before any player dice resolve (Rule 5.1). Reconstruct and replay the Effect Resolution Phase with BUFF in force if the physical state can be recovered. If the state cannot be reconstructed, use the tracker value at the moment the error is discovered compared against HP_eff. This produces the most conservative outcome — the party does not benefit from the error.

- **If the Damage Tracker Die is misread mid-phase**: revert to the last value both players agreed on. If no agreed prior value exists, use the lower of the two claimed values. Consistent with the Integrity System's tracker-dispute ruling.

---

### Layer Transition Edge Cases

- **If the stripped layer is the last card in the Angel's stack (final layer) AND that Angel has a HIT or SRG committed for the Angel Resolution Phase of the same round**: the HIT/SRG is **cancelled**. When an Angel is Cleared (all layers stripped), it ceases to act immediately. No actions fire from a Cleared Angel in the current or any future round. The Win/Loss check fires at the end of the strip check sequence, before the Angel Resolution Phase begins.

- **If the newly revealed layer's face map shows an action for the same face value that just fired on the stripped layer**: the new face map does NOT fire this round. The Angel's action for this round was determined at the Angel Roll Phase using the Exposed layer's face map at that time. The new layer's face map becomes active from the next Angel Roll Phase onward.

- **If all Angels in a multi-Angel encounter are Cleared on the same turn**: process each Angel's strip check independently. After all checks complete, evaluate the Cleared state for each Angel. If all Angels are Cleared, trigger the Win/Loss check once. The Win/Loss check fires after all strip checks for the turn are complete — not mid-sequence. No Angel Resolution Phase occurs this round (all Angels are Cleared before it would begin).

---

### BUFF Edge Cases

- **If BUFF fires on a layer where HP_L = HP_max (degenerate BUFF — illegal card)**: this is a card-vetting error. At the table: treat the BUFF as null — announce it, apply nothing. HP_eff = HP_L. Log the defect. The card must be corrected before the next session.

- **If two Angels each have a BUFF face and both fire in the same round**: both BUFFs apply independently to their respective Angels. Each BUFF raises its own Angel's effective threshold for this turn only. Announce both at phase start (Angel A then Angel B, per encounter-card order). Damage targeting remains independent.

- **If BUFF fires but the party's damage total is 0 this turn**: the BUFF is announced, HP_eff is set, and the tracker stays at 0. The end-of-phase check fires (0 ≥ HP_eff is false), the layer survives, and the tracker resets to zero. BUFF does not persist — next turn's threshold reverts to HP_L. **The mandatory secondary component (Rule 4.6) still fires in full** — if the secondary is a PRS, the placement constraint applies for this phase; if the secondary is a HIT/SRG, it resolves in the Angel Resolution Phase. The BUFF threshold component has no effect when damage is 0, but the secondary component always fires regardless.

---

### PRS Edge Cases

- **If a PRS constraint makes it impossible for any player to legally place any die (all slot types are restricted)**: the PRS fires as announced. No die placements occur this Effect Resolution Phase — all players skip placement. The Angel Resolution Phase proceeds normally. A total lockout is legal but extreme. Card authors must verify WAS remains in the publishable band before printing such a constraint.

- **If two PRS constraints from different Angels conflict in a way that creates an impossible placement rule (example: Angel A: "Head slots locked" AND Angel B: "Head slots only")**: both constraints apply simultaneously. The combined effect produces a total lockout — no placements are legal this phase. This specific combination is a degenerate encounter design error; the encounter designer must not publish Angel sets whose PRS faces can combine this way. Log as a design defect if it occurs in playtesting.

- **If a PRS restricts a slot type that no player currently has active (all such slots are inert)**: the PRS fires as announced but has no practical effect. The party proceeds with all remaining legal placements as normal. A constraint that affects no reachable state is vacuously satisfied.

---

### Multi-Angel Edge Cases

- **If one Angel is Cleared during the strip check and the surviving Angel still has a HIT or SRG action to fire**: the surviving Angel fires its hits normally. The Cleared Angel fires nothing (its actions are cancelled on Cleared, per the layer transition ruling above). Hit resolution order continues from the surviving Angel only.

- **If an encounter card's hit resolution order is missing or unclear**: the party declares the resolution order before the encounter begins, binding for the full encounter (Rule 7.5). If the error is discovered mid-encounter, the party agrees on an order immediately and applies it to all remaining-to-resolve actions in the current round and all future rounds. Already-resolved hits are not reversed. Log as a card-vetting defect.

- **If a Targeted hit specifies a constraint that can only be satisfied by targeting a slot that became inert earlier in the same Angel Resolution Phase**: the hit is wasted. Hit eligibility is evaluated at the moment of routing — an inert slot at that moment cannot be targeted, and the hit does not reroute. Owned by the Integrity System (targeted-hit-to-inert-slot ruling); Angel System inherits it without modification.

---

### Face Map / Card Error Edge Cases (Physical Prototype)

- **If an Angel layer card has a face with no action printed**: treat that face as a null action — the Angel does nothing this round. Announce it explicitly: "Face [N]: no action." Log as a card-vetting defect. Do not retroactively assign an action at the table.

- **If a face map contains more than 3 action types (violating Rule 4.1)**: use the face map as printed. Resolve all printed actions in sequence per Rule 5.3. Log as a card-vetting defect. Post-session, reduce the face map to at most 3 action types and re-derive WAS.

- **If HP_L on a card is 0 or negative**: this is an illegal card. Treat it as HP_L = HP_min for the encounter's target party size (10 at MVP). Announce the substitution to all players. Log as a card-vetting defect requiring correction before the next session.

## Dependencies

**Upstream dependencies** (systems this one depends on):

| System | Dependency Type | What the Angel System Consumes |
|---|---|---|
| **Dice Economy** | Hard | 3D6 median roll mechanic and P_angel probability table (used for WAS calibration and face assignment design); E_angel = 3.5; the rule that Angel result is revealed before player placement; the rule that Angel resolution fires after all player effects |
| **Integrity System** | Hard | HP bounds formula (HP_min, HP_max, HP_alpha = 0.6, HP_beta = 1.2); D_agg calibration baseline (D_player_base = 8); T_angel formula (target 1.0–3.0 turns per layer); hit type definitions (Party hit / Targeted hit); no-carryover rule; two-die damage tracker model; targeted-hit-to-inert-slot ruling |

**Downstream dependents** (systems that depend on this one):

| System | Dependency Type | What They Consume |
|---|---|---|
| **Combat System** | Hard | Full Angel action resolution sequence (Angel Roll Phase → BUFF/PRS at Effect Resolution Phase start → strip check at phase end → HIT/SRG in Angel Resolution Phase); multi-Angel shared-roll rule; Cleared-Angel-cancels-actions rule. Combat owns the phase structure; Angel System defines the rules within it. |
| **Character System** | Hard | H_pt (expected hits per player per turn) derived from each Angel's face map via Formula 2. Used in T_board = L_total / H_pt. Must be evaluated at both uniform routing (baseline) and worst-case routing (all hits directed at one player). |
| **Win/Loss Conditions** | Hard | "Angel Cleared" trigger — when all layers of an Angel are stripped, Win/Loss is notified. Win/Loss owns the consequence. Cross-system timing note: the Win/Loss check fires before the Angel Resolution Phase begins in the same round (Cleared Angel cancels remaining actions). |
| **Equipment System** | Soft | PRS actions may restrict dice placement on specific slot types for one turn. Equipment System must handle externally-imposed placement restrictions. |
| **Events System** | Soft (provisional) | The Events System (not yet designed) will pull Angel boards from the Angel deck for encounter events. Encounter cards must state how many Angels per party size (Rule 2 of Detailed Design). This dependency is provisional — confirm when the Events System GDD is authored. |

**Bidirectional consistency notes:**
- When the **Combat System GDD** is authored, it must list the Angel System as a dependency and reference the Angel Roll Phase action sequence as defined here.
- When the **Character System GDD** is authored, it must list the Angel System as a dependency and use the H_pt formula (Formula 2) to verify T_board for each character at each Angel's face map.
- When the **Win/Loss Conditions GDD** is updated, it must reflect the "Cleared Angel cancels remaining actions" timing rule and the pre-Angel-Resolution-Phase Win/Loss check.
- The **Dice Economy GDD** listed the Angel System as a pending `referenced_by` entry in the entity registry — update after this GDD is complete.
- The **Integrity System GDD** listed the Angel System as a pending `referenced_by` entry in the entity registry — update after this GDD is complete.

## Tuning Knobs

**1. Angel Layer HP (HP_L)**
- **Adjusts**: How long a layer takes to strip; encounter pacing tension
- **Safe range**: HP_min to HP_max for the target party size. At MVP (n=2): [10, 19]
- **Too low (HP_L < HP_min)**: Layer strips trivially on most turns; no tension; layers feel like speed bumps
- **Too high (HP_L > HP_max)**: T_angel > 3 turns; encounter runs long; violates 5–8 minute target
- **Source formula**: HP bounds formula (Integrity System GDD)
- **Owned by**: Angel System. Affects: Integrity System (T_angel), Character System (T_board via H_pt)

**2. Layer Count per Angel (1–4 layers)**
- **Adjusts**: Total encounter length; number of face-map-change moments per fight
- **Safe range**: 1–4 layers
- **At 1 layer**: Single-threshold encounter; high-stakes but no escalation arc. Best for skirmish Angels
- **At 3 layers**: Standard — three phases of escalation. Target for most MVP Angels
- **At 4 layers**: Long encounter; requires low HP_L per layer to stay within 5–8 minutes; reserve for the final boss
- **Interaction**: Affects total H_pt accumulation across the encounter; Character System must verify T_board at worst-case H_pt of the Angel's inner layers

**3. WAS per Layer**
- **Adjusts**: How threatening a layer is on average; party pressure per turn
- **Safe range**: [1.5, 3.5]
- **Too low (WAS < 1.5)**: Layer feels passive; party faces little pressure; encounters drag
- **Too high (WAS > 3.5)**: Degenerate — party cannot plan or recover; routing collapses
- **Escalation pattern**: WAS should increase from outer to inner layers on the same Angel. At minimum, the final layer must have the highest WAS.
- **Source formula**: Formula 1 (this GDD)

**4. BUFF Value (BUFF_value)**
- **Adjusts**: How effectively a BUFF face impedes stripping on that turn
- **Safe range**: [1, HP_max − HP_L]. At MVP: max BUFF_value = 19 − HP_L
- **Too low (BUFF_value = 1)**: Near-negligible effect; the face feels wasted
- **Too high (BUFF_value > HP_max − HP_L)**: Degenerate — layer cannot be stripped on BUFF turns
- **Interaction**: Frequency matters. BUFF on faces 3/4 (24.1% each) fires often; BUFF on faces 1/6 (7.4% each) fires rarely. A large BUFF on a common face is more punishing than an equivalent BUFF on a rare face.

**5. Number of Angels per Encounter (scaled by party size)**
- **Adjusts**: Complexity, coordination load, and damage-split decisions per encounter
- **Safe range**: 1–2 Angels for MVP. 3+ Angels deferred to Feature-layer.
- **At 1 Angel**: Standard cooperative focus; clean and fast
- **At 2 Angels**: Forces damage-split decision each turn; adds coordination load and strategic depth
- **Design constraint**: HP bounds are calibrated to party size, not total Angel count. A two-Angel encounter with both Angels at HP_max creates effectively double the HP to clear without any increase in D_agg. Reduce HP_L on multi-Angel encounters to compensate. See Formula 4 for the focus vs. split efficiency analysis.

**6. Action Type Distribution per Layer (HIT / SRG / PRS / BUFF)**
- **Adjusts**: Whether a layer feels aggressive (hit-heavy), defensive (BUFF-heavy), or tactical (PRS-heavy)
- **Safe range**: Maximum 3 distinct action types per layer; at least 1 HIT or SRG face
- **All HIT**: Maximum raw hit pressure; party routing is everything; minimal tactical variety
- **HIT + BUFF heavy**: Encounters where the party must overkill to strip; frustrating if BUFF is on common faces with high BUFF_value
- **HIT + PRS heavy**: Tactical encounters that punish specific build archetypes; add friction for certain equipment configurations
- **Too many action types (>3)**: Cognitive overload; violates Fast and Focused

**7. Common Face Severity (Faces 3 and 4)**
- **Adjusts**: The baseline pressure the party faces on ~48% of rounds
- **Locked constraint**: Faces 3 and 4 may not both carry Severity ≥ 4 on the same layer (Rule 4.4)
- **Setting both at Severity 2–3 (HIT-1)**: Steady, readable pressure; party knows the baseline each round
- **Setting one at Severity 3 and one at Severity 0–1**: Creates a "relief/threat" alternation on the most frequent faces; more variance in the mid-game feel
- **Setting one at Severity 4 (HIT-2)**: Permitted only if the other is ≤ 3; creates a "usually hard, sometimes brutal" feel on common faces

**Tuning knobs owned by other systems (referenced here for completeness):**

| Knob | Owned By | Relevance to Angel System |
|---|---|---|
| D_player_base (8) | Integrity System | Recalibrate all HP_L values if this changes |
| HP_alpha (0.6) | Integrity System | Changes HP_min for all Angels; recalibrate on update |
| HP_beta (1.2) | Integrity System | Changes HP_max for all Angels; recalibrate on update |
| L_total per character | Character System | Affects T_board at this Angel's H_pt; verify after each new Angel is designed |

## Visual/Audio Requirements

[To be designed]

## UI Requirements

[To be designed]

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [measurable outcome]. Items marked DEFERRED are design-tool validations, not table-play checks.*

---

**Board Structure**

- **AC-01** — GIVEN an encounter card states "1–2 players: 1 Angel, 3–4 players: 2 Angels" and the party has 3 players, WHEN the encounter is set up, THEN 2 Angel boards are placed in the Angel Zone and 1 Angel board is not.

- **AC-02** — GIVEN two Angels are in play, WHEN the Angel Roll Phase begins, THEN exactly one set of 3D6 is rolled. No second dice roll occurs. Each Angel's action for this round is determined by reading face [median] on that Angel's own Exposed layer face map — not by a separate roll.

- **AC-03** — GIVEN two Angels are in play, WHEN a player declares damage against Angel A, THEN the tracker die beside Angel A advances. The tracker die beside Angel B does not advance. Trackers are never shared.

- **AC-04** — GIVEN an Angel has received damage totaling 7 in one turn, WHEN the tracker die reaches 6 and more damage accrues, THEN a second spare die is placed alongside it as a tens digit. The combined representation correctly displays the total (tens die = 1, units die = 1 for 11; tens die = 1, units die = 4 for 14, etc.). Both dice reset to zero at end of Effect Resolution Phase.

---

**Action Timing**

- **AC-05** — GIVEN an Angel layer has BUFF +4 on face 2 and the median die shows 2, WHEN the Effect Resolution Phase begins, THEN the BUFF is announced and the effective HP threshold (HP_eff = HP_L + 4) is stated aloud before any player picks up or resolves any placed die.

- **AC-06** — GIVEN an Angel layer has PRS ("Head slots may not receive dice") on face 3 and the median die shows 3, WHEN the Effect Resolution Phase begins, THEN the PRS constraint is announced before any player picks up or resolves any placed die. No die is placed on a Head slot during this phase.

- **AC-07** — GIVEN an Angel layer has HIT: 1 party hit on face 4 and the median die shows 4, WHEN all player dice effects have resolved at the end of the Effect Resolution Phase, THEN the HIT has not yet fired. The HIT fires in the subsequent Angel Resolution Phase only.

- **AC-08** — GIVEN a face entry reads "PRS: Head slots locked. HIT: 1 party hit", WHEN that face fires this round, THEN the PRS constraint is announced at the start of the Effect Resolution Phase AND the HIT fires in the Angel Resolution Phase — the two components do not fire simultaneously in the same phase.

---

**Strip Check**

- **AC-09** — GIVEN an Angel layer has HP_L = 14 and the party deals exactly 14 damage, WHEN the end-of-phase strip check fires, THEN the layer is stripped. (The strip condition is `D_turn ≥ HP_eff`; equality is a passing condition.)

- **AC-10** — GIVEN an Angel layer has HP_L = 14 and a BUFF +4 fires this turn (HP_eff = 18) and the party deals exactly 14 damage, WHEN the end-of-phase check fires, THEN the layer survives (14 < 18). The BUFF raised the threshold; the damage was not prevented.

- **AC-11** — GIVEN an Angel layer has HP_L = 14 and a BUFF +4 fires this turn (HP_eff = 18) and the party deals exactly 18 damage, WHEN the end-of-phase check fires, THEN the layer is stripped.

- **AC-12** — GIVEN the party deals 14 damage in turn 1 against a layer with HP_L = 16 (layer survives), WHEN turn 2's Angel Roll Phase begins, THEN the Damage Tracker Die reads 0. (No carryover. The reset occurs at the end of turn 1's Effect Resolution Phase regardless of whether the layer survived or was stripped.)

- **AC-13** — GIVEN a BUFF +4 fires in turn 1 (HP_eff = 18) and the layer survives, WHEN turn 2's Angel Roll Phase begins, THEN the effective threshold is HP_L only. BUFF does not persist. HP_eff is recalculated fresh each turn.

- **AC-14** — GIVEN an Angel layer with HP_L = 14 is Exposed and a player's damage effect resolves during the Effect Resolution Phase bringing the tracker to 14, WHEN that effect resolves, THEN the layer does not strip mid-phase. The strip check fires exactly once: at the end of the Effect Resolution Phase, after all placed-die effects have resolved.

---

**Layer Transition**

- **AC-15** — GIVEN an Angel layer is stripped at the end of the Effect Resolution Phase, WHEN the card is removed from the stack, THEN it is placed face-up in the Stripped Pile beside the Angel Zone. Players may consult it at any time.

- **AC-16** — GIVEN a layer is stripped and a Hidden layer exists beneath it, WHEN the stripped card is placed in the Stripped Pile, THEN the next card in the stack is physically turned face-up so its HP value and full face map are readable by all players before the Angel Resolution Phase begins.

- **AC-17** — GIVEN the old Exposed layer's face 3 action was HIT-1 and the newly revealed layer's face 3 action is SRG, WHEN the newly revealed layer becomes Exposed this round, THEN the SRG does NOT fire this round. The new face map is active from the next Angel Roll Phase onward.

- **AC-18** — GIVEN the last layer of an Angel is stripped during the strip check AND that Angel has a HIT committed for the Angel Resolution Phase this round, WHEN the Angel is Cleared, THEN the HIT is cancelled. No hits are dealt from a Cleared Angel this round or any future round.

- **AC-19** — GIVEN the last layer of an Angel is stripped and the Angel is Cleared, WHEN the strip check resolves, THEN the Win/Loss check fires. If the encounter ends, the Angel Resolution Phase does not occur for this round. If other Angels remain active, those Angels' Angel Resolution Phase actions still resolve.

- **AC-20** — GIVEN an Angel has Hidden layers below the current Exposed layer, WHEN a player attempts to view a Hidden layer card during play, THEN the card remains face-down. Hidden layers may not be read by any player until the layer above them is stripped.

- **AC-21** — GIVEN Angel A is Cleared and Angel B has 1 layer remaining, WHEN Angel A's strip check resolves, THEN the encounter continues. The party must strip Angel B's remaining layer to trigger the Win/Loss check.

---

**Multi-Angel**

- **AC-22** — GIVEN two Angels are in play and a player's damage effect deals 5 damage, WHEN that effect fires, THEN the player declares which Angel receives the 5 damage before either tracker advances. The 5 damage cannot be split between Angels — the full effect targets one Angel only.

- **AC-23** — GIVEN two Angels are in play and the party allocates damage such that Angel A's tracker reaches its HP_eff but Angel B's tracker does not, WHEN the end-of-phase strip checks fire, THEN Angel A's layer is stripped and Angel B's layer survives. The checks are independent; clearing Angel A does not affect Angel B's tracker or threshold.

- **AC-24** — GIVEN Angel A is Cleared during the strip check and Angel B has a HIT action committed this round, WHEN the Angel Resolution Phase begins, THEN Angel B resolves its HIT per Integrity System routing rules. Angel A resolves nothing.

- **AC-25** — GIVEN Angel A's face map shows BUFF on the committed face and Angel B's face map shows PRS on the same committed face, WHEN the Effect Resolution Phase begins, THEN Angel A's BUFF is announced first, then Angel B's PRS, and both are in effect simultaneously before any player resolves a die.

- **AC-26** — GIVEN an Angel layer has SRG [2 base hits + 1 hit if condition met] and the condition IS met, WHEN the SRG fires in the Angel Resolution Phase, THEN the party routes the 2 base hits first. Only after the base hits are fully routed is the escalation condition evaluated, and only then do the additional hits fire and are routed. The party does not receive all 3 hits simultaneously.

- **AC-27** — GIVEN an Angel fires HIT: 2 party hits, WHEN the Angel Resolution Phase resolves, THEN the party collectively decides how to distribute the 2 hits across eligible routing targets per Integrity System routing rules. The Angel System does not dictate a specific target.

---

**Face Map Design (card-level validation)**

- **AC-28** — GIVEN a completed Angel layer face map, WHEN a tester lists the distinct action categories used (HIT, SRG, PRS, BUFF — each counted once regardless of how many faces carry it), THEN the count of distinct categories is ≤ 3.

- **AC-29** — GIVEN a completed Angel layer face map, WHEN a tester checks all 6 faces for action type, THEN at least one face carries HIT or SRG.

- **AC-30** — GIVEN a completed Angel layer face map, WHEN a tester checks faces 3 and 4, THEN at most one of them carries HIT: 2 party hits or SRG. Both faces 3 and 4 may not simultaneously carry actions in these categories on the same layer.

- **AC-31** — GIVEN an Angel layer card with HP_L printed and a BUFF action with BUFF_value, WHEN a tester checks BUFF_value against the constraint BUFF_value ≤ (19 − HP_L) (where 19 is HP_max at MVP), THEN the constraint is satisfied. A BUFF that would make HP_eff > 19 is an illegal card.

---

**Deferred — Design Tools**

- **AC-34** — GIVEN a completed Angel layer face map with one or more BUFF faces, WHEN a tester inspects each BUFF face entry, THEN every BUFF entry contains a secondary component (a PRS constraint or a HIT/SRG action printed in the same entry). A BUFF entry with no secondary component is an illegal face map entry and must be corrected before print.

- **AC-32** *(DEFERRED — design tool)* — GIVEN a completed Angel layer face map, WHEN WAS is calculated using Formula 1, THEN WAS ∈ [1.5, 3.5] for the layer to be publishable.

- **AC-33** *(DEFERRED — design tool, pending Character System)* — GIVEN a completed Angel layer face map and a known party size n, WHEN H_pt is calculated using Formula 2, THEN the result is used (not WAS) for T_board verification in the Character System GDD. BUFF and PRS faces correctly contribute C_i = 0; HIT and SRG faces contribute C_i = hit count.

## Open Questions

- **OQ-01 (PENDING: Character System)** — What is D_agg_max (maximum achievable party damage per turn at full build)? Required to fully close the BUFF degenerate condition for HP_L values near HP_max. Owner: Character System GDD. Resolve before finalizing Angel cards with BUFF actions on layers with HP_L ≥ 16.

- **OQ-02 (PENDING: Character System)** — What is L_total per character (layers per player board)? Required to verify T_board = L_total / H_pt for each character at each Angel's H_pt. Owner: Character System GDD. Evaluate at both uniform routing (baseline) and worst-case routing (all hits to one player).

- **OQ-03 (PENDING: playtesting)** — Does the hidden-until-strip rule feel fair for the first turn on a newly revealed layer? If playtest shows the "blind first turn" feels arbitrary rather than dramatic, consider the preview-at-50%-damage variant (flip the next layer face-up when the current layer first takes ≥ HP_L/2 cumulative damage). AC-33 deferred pending this data.

- **OQ-04 (PENDING: playtesting)** — Is the damage-split decision for multi-Angel encounters engaging or does it create analysis paralysis? Formula 4 establishes that focus always beats even-split within HP bounds — the strategic question is whether players understand this and find the focus-vs-pressure tradeoff rewarding, or whether two Angels simply doubles cognitive load without adding interesting decisions. Monitor in MVP playtesting.

- **OQ-05 (PENDING: Events System)** — What is the encounter card format? The Angel System specifies that encounter cards must state Angel count per party size, hit resolution order (for multi-Angel), and the Angel boards to use. The Events System GDD must define the encounter card anatomy to accommodate these fields.
