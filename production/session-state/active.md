# Session State — S-Layers

**Last updated**: 2026-05-27
**Stage**: Concept

## Current Task

Angel System redesign propagation — COMPLETE (2026-05-27). /propagate-design-change angel-system.md finished. All 6 affected files updated. Change impact report written. Next: individual /design-review passes or /review-all-gdds.

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
- docs/architecture/change-impact-2026-05-27-angel-system.md — CREATED (2026-05-27) — propagation report for angel-system.md redesign

## Angel System Redesign Propagation — COMPLETE (2026-05-27)
All 6 files updated by /propagate-design-change angel-system.md:
1. integrity-system.md — 3-slot rules, Penetrating within-slot, HP bounds deprecated
2. character-system.md — Penetrating rewritten, CS-AC-30 resolved, OQ-05 flagged
3. win-loss-conditions.md — Cleared = all 3 slots Inert
4. combat-system.md — slot targeting, EPC HP range [4,14], cross-ref table updated
5. events-system.md — encounter card format clarified (angel count only, not layers/slot)
6. entities.yaml — HP_alpha/HP_beta deprecated; angel-system.md added to referenced_by for D_agg, D_player_base, P_angel, E_angel, EPC, T_angel

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

### ✅ BLOCKERS RESOLVED (2026-05-26) — cross-review verdict upgraded to CONCERNS
Before any individual /design-review runs, resolve these 7 blockers (see report: design/gdd/gdd-cross-review-2026-05-26.md):

**BLOCKER 1 — Penetrating cascade — ✅ RESOLVED 2026-05-26**
- Decision: Option A (true cascade). Integrity Rule 20 + Angel Rule 6.5 updated with Penetrating exception. CS-AC-30 resolved. Combat OQ-1 resolved. Outermost-first passive ordering locked.

**BLOCKER 2 — Win/Loss Rule 6 God-only qualifier — ✅ RESOLVED 2026-05-26**
- Rule 6 rewritten to open with "God card only" scope. Non-God simultaneous triggers: Lost takes priority.

**BLOCKER 3 — T_round registry + Formula 2 table — ✅ RESOLVED 2026-05-26**
- Registry updated: baselines T_base=25, α_4=10, α_5=10, α_6=5, β=15; output_range=[37,193]. Combat Formula 2 table corrected. Combat OQ-2 updated.

**BLOCKER 4 — Pillar 5 — ⚠️ USER ACCEPTED AS-IS (2026-05-26)**
- User decision: no change to game-concept.md. Will validate in playtests. Risk accepted.

**BLOCKER 5 — Martyr dump-dice — ⚠️ DOWNGRADED TO WARNING (user decision, 2026-05-26)**
- No change now. Flag for first playtest observation: watch for low-roll frustration on Martyr.

**BLOCKER 6 — Penetrating flag + item departure trigger — ✅ RESOLVED 2026-05-26**
- Added to character-system.md Rule 11 Edge Cases: item departure-trigger damage is not Penetrating.

**BLOCKER 7 — On-destroy vs. item departure ordering — ✅ RESOLVED 2026-05-26**
- Added to equipment-degradation.md Rule 1 timing note: (1) on-destroy passive, (2) item departure trigger, (3) new layer reveals.

### After blockers resolved:
1. ✅ Run `/design-review design/gdd/combat-system.md` — originally APPROVED (2026-05-26); NOW Needs Revision (cross-review B-CON-1, B-SCN-1)
2. Resolve all 7 blockers above across affected GDDs (can do in one session)
3. Re-run `/review-all-gdds` OR proceed directly to individual design-reviews:
   - `/design-review design/gdd/integrity-system.md`
   - `/design-review design/gdd/character-system.md`
   - `/design-review design/gdd/angel-system.md`
   - `/design-review design/gdd/combat-system.md`
   - `/design-review design/gdd/win-loss-conditions.md`
   - `/design-review design/gdd/equipment-system.md`
   - `/design-review design/gdd/item-system.md`
   - `/design-review design/gdd/dice-economy.md`
   - `/design-review design/gdd/equipment-degradation.md`
4. Run `/gate-check systems-design` after all reviews pass
5. Build MVP physical prototype (months 1-3)
6. Begin audience building in parallel (month 1+)
7. Search for woodcut illustrator (month 2)
8. Run /art-bible — visual identity specification

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

## Session Extract — /review-all-gdds 2026-05-26
- Verdict: FAIL
- GDDs reviewed: 12 (10 system GDDs + game-concept.md + systems-index.md)
- Flagged for revision (Blocking): integrity-system.md, win-loss-conditions.md, equipment-system.md, angel-system.md, character-system.md, combat-system.md, item-system.md, game-concept.md
- Flagged for revision (Warning): dice-economy.md, events-system.md, systems-index.md, design/registry/entities.yaml
- Blocking issues: 7 — (1) Penetrating multi-strip contradiction across 4 GDDs, affects Pilgrim balance; (2) Win/Loss Rule 6 missing God-only qualifier; (3) T_round registry out-of-date + internal Formula 2 mismatch; (4) Pillar 5 undeliverable at MVP; (5) Martyr no dump-dice home; (6) Penetrating flag + item departure trigger undefined; (7) on-destroy passive vs. item departure ordering undefined
- Recommended next: Resolve all 7 blockers (can be done in one focused design session), then re-run /review-all-gdds or proceed to individual /design-review passes
- Report: design/gdd/gdd-cross-review-2026-05-26.md

## Session Extract — /review-all-gdds 2026-05-27
- Verdict: FAIL
- GDDs reviewed: 12 (10 system GDDs + game-concept.md + systems-index.md)
- All 7 prior consistency blockers (2026-05-26) CONFIRMED-FIXED
- Flagged for revision (Blocking): character-system.md (B-DT-1 Pilgrim S_encounter recompute), events-system.md (B-SCN-1 item departure-trigger no-Angel-target undefined)
- Flagged for revision (Warning): item-system.md, angel-system.md, combat-system.md, integrity-system.md, equipment-degradation.md, game-concept.md, win-loss-conditions.md (Info)
- Blocking issues: 2 — (1) B-DT-1: Pilgrim may be unviable at MVP n=2 — within-slot Penetrating + HP [4,14] + max 3-layer slot requires recompute; MVP Angels have 2-layer slots while Pilgrim band-viability requires ≥3; (2) B-SCN-1: item departure-trigger damage during Bad Event has no Angel target — undefined behavior across events/item/integrity GDDs
- Recommended next: Resolve B-DT-1 (Pilgrim S_encounter recompute) and B-SCN-1 (define item departure with no-Angel target — recommend damage fizzles), then re-run /review-all-gdds or proceed to individual /design-review passes
- Report: design/gdd/gdd-cross-review-2026-05-27.md

## B-DT-1 RESOLVED — 2026-05-27
- Decision: Keep cascade / excess-carries Penetrating (Angel Rule 6.6). Retire D_penetrating = R × n_L AOE formula.
- Cascade model: damage = R total; n_strips = max j where R ≥ cumulative HP through layer j.
- Pilgrim S_encounter: ~37–40 base (n=2); ~54–58 with 1× Type 2 item on Reservoir (in band); ~54–60 at n=3-4 (cascade double-strip, in band).
- Design intent locked: Pilgrim is intentionally item-dependent at n=2. Items complete the arc. Not a flaw.
- Files updated: character-system.md (Formula 3 cascade rewrite, S_encounter table, Tuning Knob 6, OQ-05 closed), angel-system.md (OQ-05 closed), entities.yaml (the_pilgrim notes).
- Remaining 2027-05-27 blocker: B-SCN-1 (item departure-trigger with no Angel target — events-system.md + item-system.md)

## B-SCN-1 RESOLVED — 2026-05-27
- Decision: Departure trigger fires unconditionally; offensive damage with no valid Angel target fizzles. Trigger consumed, item departs, no damage applied.
- Files updated: events-system.md (added edge case under "Bad Event + Equipment Degradation"), item-system.md (added edge case under "Departure and Trigger Edge Cases")
- Both blockers from gdd-cross-review-2026-05-27.md now resolved: B-DT-1 ✓, B-SCN-1 ✓
- /review-all-gdds 2026-05-27 verdict: FAIL → all blockers now closed. Ready to re-run review or proceed to individual /design-review passes.

## /review-all-gdds 2026-05-28
- Verdict: FAIL
- GDDs reviewed: 12
- Prior blockers B-DT-1 and B-SCN-1: CONFIRMED RESOLVED & PROPAGATED
- Blocking issues: 2 — (1) B-ATTN-1: player attention budget overflow (9 simultaneous systems during Phase 4 placement; Pillar 3 structurally violated, EPC WARN_LONG at ~115s); (2) B-REST-1: no restoration items in MVP item pool + inter-encounter recovery LOCKED — session is a loss spiral by design
- Consistency warnings (4): W-CON-1 stale HP_L=19 in integrity-system.md; W-CON-2 T_active 1.0 vs 2.7 in item-system.md; W-CON-3 combat Phase 5b vs Integrity Rule 17 ordering; W-CON-4 combat-context departure-trigger target undeclared
- Design warnings (8): W-DT-1–8 (Pilgrim+Martyr mandatory, routing-to-Martyr-Body deterministic, Reservoir hold dominant, Type 2 item prescribed, cross-encounter curve, Pilgrim RNG-conditional, Pillar 5 deferred, Pillar 3 over-claimed)
- Recommended next: Resolve B-ATTN-1 (document 3-slot risk + add Pillar 3 playtest gate) and B-REST-1 (add restoration item to item-system.md MVP pool, close equipment-degradation.md Tuning Knob 5), then re-run /review-all-gdds
- Report: design/gdd/gdd-cross-review-2026-05-28.md
