# Session State — S-Layers

**Last updated**: 2026-05-26
**Stage**: Concept

## Current Task

Combat System GDD — REVIEWED AND APPROVED (2026-05-26). 11 blockers resolved. Review log: design/gdd/reviews/combat-system-review-log.md.

## Progress
- [x] /start — project initialized, stage set to Concept, review mode set to full
- [x] /brainstorm — S-Layers game concept created
- [x] /map-systems — 12 systems decomposed, index written to design/gdd/systems-index.md
  - [x] Phase 1: Creative Discovery
  - [x] Phase 2: Concept selection (Concept 1 — S-Layers core vision)
  - [x] Phase 3: Core loop design
  - [x] Phase 4: Pillars locked (CD-PILLARS: CONCERNS resolved, Pillar 5 rewritten)
  - [x] Phase 4: Visual anchor selected (Woodcut Witness — AD-CONCEPT-VISUAL: APPROVE)
  - [x] Phase 5: Player type validation
  - [x] Phase 6: Scope and feasibility (TD-FEASIBILITY: CONCERNS, PR-SCOPE: UNREALISTIC resolved with 20-month timeline)
  - [x] Game concept written to design/gdd/game-concept.md
- [x] /design-system — all 9 MVP GDDs designed

## Key Decisions Made
- **Game**: S-Layers — cooperative dark fantasy board game, 1-4 players, 60-90 min sessions
- **Core mechanic**: Dice placement + equipment degradation + item modification
- **Visual identity**: Woodcut Witness — black on cream, red-brown for Angels, gold for boons
- **Target**: Kickstarter (physical product), 20-month timeline
- **Review mode**: full

## Files Created This Session
- production/stage.txt — "Concept"
- production/review-mode.txt — "full"
- design/gdd/game-concept.md — complete game concept document
- design/gdd/dice-economy.md — complete GDD (CD-GDD-ALIGN: APPROVE)
- design/gdd/integrity-system.md — COMPLETE (2026-05-24) — all 8 sections ✓, CD-GDD-ALIGN: CONCERNS (accepted)
- design/gdd/win-loss-conditions.md — COMPLETE (2026-05-24) — all 8 sections ✓, CD-GDD-ALIGN: APPROVE
- design/gdd/equipment-system.md — COMPLETE (2026-05-25) — all 8 sections ✓, CD-GDD-ALIGN: CONCERNS (accepted, 2 revisions applied)
- design/gdd/angel-system.md — COMPLETE (2026-05-25) — all 8 sections ✓, 34 ACs, CD-GDD-ALIGN: CONCERNS addressed (Rule 7.8 ritual added, Rule 4.6 BUFF mandate added)
- design/gdd/character-system.md — COMPLETE (2026-05-25) — all 8 sections ✓, 41 ACs, CD-GDD-ALIGN: APPROVE
- design/gdd/equipment-degradation.md — COMPLETE (2026-05-25) — all 8 sections ✓, 29 ACs, CD-GDD-ALIGN: APPROVE
- design/gdd/item-system.md — COMPLETE (2026-05-25) — all 8 sections ✓, 34 ACs, 7 tuning knobs, CD-GDD-ALIGN: CONCERNS accepted (3 revisions: split-authority Rule 3, Tuning Knob 7 synergy scope, AC language updates)
- design/gdd/combat-system.md — COMPLETE (2026-05-26) — all 8 sections ✓, 30 ACs, 7 tuning knobs, CD-GDD-ALIGN: APPROVE

## Combat System Key Decisions (2026-05-26)
- **Phase sequence**: Enemy Roll → Action Assignment → Player Roll → Dice Placement → Effect Resolution → Enemy Resolution
- **Full-information architecture**: Angel commits before players place. PRS/BUFF visible at Phase 2; activates Phase 5 start.
- **PRS ruling**: Constrains effect resolution (not placement). Dice on restricted slots consumed without firing in Phase 5.
- **BUFF ruling**: HP_eff = HP_L + BUFF_value, this turn only, announced at Phase 5 start before any die resolves.
- **Damage check**: Single check at Phase 5 end; underspent damage discarded (no carryover).
- **Win/Loss collision**: Loss takes priority (board destroyed + Angel cleared same turn = Loss).
- **Session persistence**: Inert slots persist between encounters; stripped card piles persist; Accumulate totals on surviving layers persist.
- **First Player token**: Rotates clockwise after each Event card resolved (combat AND non-combat).
- **Multi-player strip arbitration (ED-D-2 resolved)**: Party table decision — no fixed rule; cooperative social contract.
- **Item OQ-2 resolved**: First Player token = after each Event card (confirmed in Section C cleanup).
- **T_round formula**: `T_round = T_base + (α_4 × n) + (α_5 × n) + (α_6 × n) + (C_item × β)` → baseline 90s at n=2, C_item=1.
- **EPC formula**: `T_lo = ⌈300/T_round⌉, T_hi = ⌊480/T_round⌋; PASS if T_encounter_actual ∈ [T_lo, T_hi]`
- **T_encounter constant vs. T_encounter_actual**: T_encounter=8 is Equipment System balance calibration. T_encounter_actual is EPC design-tool output.
- **Pillar 4 audit**: 5 cross-board decisions per round — pillar passes (CD APPROVE).
- **7 Tuning Knobs**: T_round coefficients, EPC turn target band, post-combat item award, First Player rotation cadence, Angel count scaling, Win/Loss priority (LOCKED), inert slot persistence (LOCKED).

## Equipment Degradation Key Decisions (2026-05-25)
- **Layer reveal procedure (Rule 1)**: 6-step atomic sequence — take top card → face-down discard pile → item departs immediately → flip new card face-up (immediately active) → if no next card: slot inert → Accumulate total lost
- **Self-strip (Rule 2)**: "Lose 1 Integrity" = immediate strip of active layer, identical to a hit. Atomic, mandatory, non-interruptible once die placed and constraint met
- **Layer ordering principles**: P1 distinct expression; P2 nuance not escalation (deeper layers work differently, same strategy); P3 outermost establishes strategy, all layers rhyme; P4 innermost is free choice
- **New layer passives mid-phase**: INERT until next turn (Integrity Rule 12 exception applies only to the committed die on the stripped layer)
- **Strip card timing**: Card moves physically to discard immediately; die still fires by Rule 12 exception (physical and logical state may diverge within phase)
- **Degradation indicator**: Must show — layers remaining, stronger/weaker next layer, final-layer alert. Must be table-readable (60-100 cm)
- **N_strips formula**: N_strips = H_pt × T_encounter. At MVP: 8.0. Guardrail: N_strips < L_total
- **S_layer formula**: S_layer(i) = E_slot(i) × T_active(i) + B_burst(i). Sum ∈ [54,74]
- **A_weapon (Martyr)**: A_weapon = N_weapon_layers = 3. B_weapon_total = 8.94 + 13.41 + 17.88 = 40.23
- **3 DEFERREDs**: D-1 restoration rules (partially addressed in Combat Section C cleanup), D-2 multi-player strip arbitration (✅ RESOLVED), D-3 Martyr S_encounter balance (playtest-dependent)

## Character System Key Decisions (2026-05-25)
- **MVP characters**: The Pilgrim (L_total=8, burst accumulator) and The Martyr (L_total=10, self-consuming attacker)
- **Divine gift**: Activation during Placement Phase only, 4 windows; does NOT draw from 2D6 pool; gifts modify effect output, not N itself
- **Reservoir mechanic**: Accumulate ∞ — bank indefinitely, player-triggered Release; total lost if layer stripped
- **Penetrating damage**: Hits all Angel layers; set by Pilgrim Head Accumulate 8; consumed after one damage event
- **Variable layer counts**: Pilgrim Head=3/Body=2/Weapon=3; Martyr Head=3/Body=4/Weapon=3 — character-specific exceptions
- **N-modification ruling (Option A)**: Gifts modify output, not N; re-rolls create new N (Dice Economy edge case)
- **Synergy Surface Design**: SR-1 through SR-5 mandatory for every gift before production
- **3 cooperative decisions**: Character selection, hit routing, Reservoir release timing

## Open Rulings (must resolve before production)
1. ⚠️ Penetrating multi-strip: passive on-destroy firing order when multiple layers stripped — BLOCKED on Angel System GDD (CS-AC-30)
2. ⚠️ Martyr S_encounter balance confirmation — BLOCKED on Equipment Degradation D-3 (self-Integrity cost calibration, playtest-dependent)
3. ⚠️ Martyr L4 Body passive "to ALL enemies" timing — confirm against Angel System damage tracker reset rules
4. ⚠️ ED-D-1: Restoration rules — PARTIALLY ADDRESSED in Combat Section C cleanup (stripped card piles persist); full restoration rules remain DEFERRED
5. ⚠️ ED-D-3: Martyr Weapon S_encounter balance — DEFERRED until playtests confirm P_strip values
6. ⚠️ Combat OQ-3: Multi-Angel encounter Phase 1 roll — one shared 3D6 or per-Angel? Must be resolved in Angel System GDD.

## Next Steps (IN ORDER)
1. ✅ Run `/design-review design/gdd/combat-system.md` — APPROVED (2026-05-26), 11 blockers resolved
2. Run `/design-review design/gdd/item-system.md` in a FRESH session (mandatory)
3. Run `/design-review design/gdd/equipment-degradation.md` in a FRESH session (mandatory)
4. Run `/design-review design/gdd/character-system.md` in a FRESH session (mandatory)
5. Run `/design-review design/gdd/angel-system.md` in a FRESH session (mandatory)
6. Run `/design-review design/gdd/equipment-system.md` in a FRESH session (mandatory)
7. Run `/design-review design/gdd/dice-economy.md` in a FRESH session (mandatory)
8. Run `/design-review design/gdd/integrity-system.md` in a FRESH session (mandatory)
9. Run `/design-review design/gdd/win-loss-conditions.md` in a FRESH session (mandatory)
10. Run `/gate-check systems-design` after all design-reviews complete
11. Build MVP physical prototype (months 1-3)
12. Begin audience building in parallel (month 1+)
13. Search for woodcut illustrator (month 2)
14. Run /art-bible — visual identity specification

## CD Non-Blocking Notes (from Combat System review)
- Add one-sentence rationale for "Underspent damage discarded" in Phase 5 (design reason: each strike must be authored against a specific layer)
- Design AP-brake mechanic in Integrity/rulebook layer (sand timer, First Player tiebreaker, or clockwise declaration rule for Phase 6 routing)
- Confirm player-facing text (cards, rulebook) uses diegetic names not acronyms (HIT/SRG/BUFF/PRS → Blow/Surge/Boon/Constraint or equivalent)

## Open Questions
- Does the dice economy balance? (Monte Carlo simulation)
- Does combat hit 5-8 min with items? (MVP playtests — T_round coefficient calibration)
- Does woodcut art read at 63x88mm? (grayscale prototype test)
- Self-publish vs. publisher partnership?
- CD forward note: confirm Martyr routing is sometimes wrong (not autopilot to Martyr Body)
- CD forward note: verify SR-5 contract at Equipment/Item GDD review (name 3 non-obvious synergies per character) — PARTIALLY ADDRESSED in Item System Tuning Knob 7; verify at design-review
- CD forward note: Equipment Degradation Player Fantasy makes canonical claim about Heaven's design philosophy ("built to be operated") — flag to narrative-director when authoring world lore
- Item System OQ-6: Restore + re-equip loop — verify no perverse loop exists when layer is restored and item is available for re-attachment (flagged in Combat OQ-4)
