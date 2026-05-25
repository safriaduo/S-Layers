# Session State — S-Layers

**Last updated**: 2026-05-25
**Stage**: Concept

## Current Task

Item System GDD — COMPLETE (2026-05-25). 34 ACs, 7 tuning knobs, CD-GDD-ALIGN: CONCERNS accepted. 3 revisions applied. Registry and systems index updated. Ready for `/design-review` in fresh session.

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
- **3 DEFERREDs**: D-1 restoration rules, D-2 multi-player strip arbitration, D-3 Martyr S_encounter balance (playtest-dependent)

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
4. ⚠️ ED-D-1: Restoration rules — DEFERRED to Combat System or Item System GDD
5. ⚠️ ED-D-2: Multi-player strip arbitration — DEFERRED to Combat System GDD
6. ⚠️ ED-D-3: Martyr Weapon S_encounter balance — DEFERRED until playtests confirm P_strip values

## Next Steps
1. Run `/design-review design/gdd/item-system.md` in a FRESH session (mandatory)
2. Run `/design-review design/gdd/equipment-degradation.md` in a FRESH session (mandatory)
3. Run `/design-review design/gdd/character-system.md` in a FRESH session (mandatory)
4. Run `/design-review design/gdd/angel-system.md` in a FRESH session (mandatory)
5. Run `/design-review design/gdd/equipment-system.md` in a FRESH session (mandatory)
6. Run `/design-review design/gdd/dice-economy.md` in a FRESH session (mandatory)
7. Run `/design-review design/gdd/integrity-system.md` in a FRESH session (mandatory)
8. Run `/design-review design/gdd/win-loss-conditions.md` in a FRESH session (mandatory)
9. Design Combat System — last MVP GDD (System #9 in design order; depends on all Foundation + Core + Item System)
10. Build MVP physical prototype (months 1-3)
11. Begin audience building in parallel (month 1+)
12. Search for woodcut illustrator (month 2)
13. Run /art-bible — visual identity specification

## Open Questions
- Does the dice economy balance? (Monte Carlo simulation)
- Does combat hit 5-8 min with items? (MVP playtests)
- Does woodcut art read at 63x88mm? (grayscale prototype test)
- Self-publish vs. publisher partnership?
- CD forward note: confirm Martyr routing is sometimes wrong (not autopilot to Martyr Body)
- CD forward note: verify SR-5 contract at Equipment/Item GDD review (name 3 non-obvious synergies per character) — PARTIALLY ADDRESSED in Item System Tuning Knob 7; verify at design-review
- CD forward note: Equipment Degradation Player Fantasy makes canonical claim about Heaven's design philosophy ("built to be operated") — flag to narrative-director when authoring world lore
- Item System OQ-2: First Player token rotation rule — must be defined in Combat System or Game Setup GDD (HIGH priority)
- Item System OQ-6: Restore + re-equip loop — verify no perverse loop exists when layer is restored and item is available for re-attachment
