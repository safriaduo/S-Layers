# Cross-GDD Review Report — 2026-05-27

**Date**: 2026-05-27
**GDDs Reviewed**: 12 (10 system GDDs + game-concept.md + systems-index.md)
**Systems Covered**: Dice Economy, Integrity, Win/Loss Conditions, Equipment, Equipment Degradation, Character, Angel, Combat, Item, Events
**Prior Review**: design/gdd/gdd-cross-review-2026-05-26.md (FAIL, 7 blockers)
**Trigger for re-run**: Verify post-fix state after 7-blocker resolution + Angel System redesign propagation (2026-05-27)

---

## Verification of Prior Blockers (2026-05-26)

All 7 prior blockers are **CONFIRMED-FIXED**:

| ID | Blocker | Status | Evidence |
|----|---------|--------|----------|
| B-CON-1 | Penetrating multi-strip cascade | ✓ FIXED | Integrity Rule 20 + Angel Rule 6.6 + Character Rule 11 + Combat AC-30a all agree: within-slot, outermost-first |
| B-CON-2 | Win/Loss Rule 6 God-only qualifier | ✓ FIXED | win-loss-conditions.md Rule 6 explicit; Lost prevails for non-God simultaneous triggers |
| B-CON-3 | T_round registry + Formula 2 | ✓ FIXED | entities.yaml and combat-system.md Formula 1 table aligned (T_base=25, α_4=α_5=10, α_6=5, β=15; range [37,193]) |
| B-CON-4 | Pillar 5 undeliverable at MVP | ⚠ ACCEPTED | User accepted risk; flagged for playtest validation |
| B-CON-5 | Martyr dump-dice home | ⚠ DOWNGRADED | First-playtest observation flag |
| B-CON-6 | Penetrating flag + item departure | ✓ FIXED | character-system.md Rule 11 Edge Case explicit |
| B-CON-7 | On-destroy passive vs item departure order | ✓ FIXED | equipment-degradation.md Rule 1 timing note: (1) on-destroy, (2) item departure, (3) reveal |

---

## Consistency Issues (Phase 2)

### Blocking
None.

### Warnings

#### ⚠️ W-1 — Stale Angel rule number reference
- **GDDs**: combat-system.md (line ~631, OQ-1)
- **Issue**: Combat OQ-1 references "Angel Rule 6.5 updated" but the Penetrating cascade rule is at **Angel Rule 6.6** in the redesigned angel-system.md. Rule 6.5 is the *opposite* rule ("No damage carryover, non-Penetrating"). Substance of Penetrating is correct elsewhere in combat-system.md.
- **Fix**: One-line edit — update "Angel Rule 6.5" → "Angel Rule 6.6".

#### ⚠️ W-2 — Integrity Dependencies table uses pre-redesign wording
- **GDDs**: integrity-system.md (line ~330)
- **Issue**: Win/Loss row reads `"'Angel cleared' trigger (all Angel layers stripped)"`. Under the 3-slot redesign, "Angel Cleared" = "all 3 Angel slots Inert" (consistent in body Rule 22, but not in this summary table).
- **Fix**: Update wording to "all 3 Angel slots Inert".

#### ⚠️ W-3 — Pilgrim S_encounter recalculation pending (also Phase 3 blocker — see B-DT-1)
- **GDDs**: character-system.md (OQ-05), angel-system.md (OQ-05)
- **Issue**: Pilgrim S_encounter ≈ 65 was computed under the deprecated cross-layer cascade model. Within-slot model + HP_L ∈ [4,14] + max 3 layers requires recomputation.
- **Fix**: Perform the recompute; update Pilgrim row in S_encounter Comparison table; close OQ-05.

#### ⚠️ W-4 — Integrity Formula 4 SUPERSEDED block could be collapsed
- **GDDs**: integrity-system.md (Formula 4 block, lines ~223-265)
- **Issue**: The full deprecated table ([10,19], [15,29], etc.) is retained with a SUPERSEDED banner. A reader scanning without the banner could be misled.
- **Fix**: Optional documentation hygiene — collapse to one-line "DEPRECATED. See Angel System Formula 2."

#### ⚠️ W-5 — equipment-degradation.md status header not refreshed
- **GDDs**: equipment-degradation.md (lines 1-3)
- **Issue**: Other propagation-touched GDDs were updated to "Needs Revision — propagation update from angel-system.md redesign (2026-05-27)". equipment-degradation.md Rule 1 timing note carries the 2026-05-26 ordering fix but the status header was not bumped.
- **Fix**: Refresh status note / Last Updated for traceability.

### Info / Minor

- **ℹ️ I-1** — win-loss-conditions.md Formulas section parenthetical still mentions "HP bounds" (now deprecated). Cosmetic.
- **ℹ️ I-2** — Registry T_round arithmetic comment (35+60+60+28+20=203 vs published 193). Acknowledged inline as "GDD-body corrected value." Cross-GDD consistent.
- **ℹ️ I-3** — character-system.md Formula 3 example uses n_L=2 with R=12.8. Math still valid for within-slot model (slot depth 2–3) but narrative was written for cross-layer cascades.
- **ℹ️ I-4** — Dependency bidirectionality (2a): all reciprocal links verified.
- **ℹ️ I-5** — Tuning knob ownership (2d): no conflicts. Integrity TK 2/3 (HP_alpha/HP_beta) properly marked SUPERSEDED with ownership transfer to Angel TK 1-3.
- **ℹ️ I-6** — Formula compatibility (2e): all downstream inputs match upstream output ranges.
- **ℹ️ I-7** — Acceptance criteria cross-check (2f): CS-AC-30, Angel AC-16/17/18, Combat AC-30a form consistent Penetrating chain. AC-22a/b + Win/Loss AC-05/07 form consistent simultaneous-termination chain.

---

## Game Design Issues (Phase 3)

### 🔴 Blocking

#### 🔴 B-DT-1 — Pilgrim may be unviable at MVP party size

**GDDs**: character-system.md, angel-system.md

The Angel System redesign (2026-05-27) made Penetrating **within-slot only**, with HP_L ∈ [4,14] and max 3 layers per slot. Pilgrim's S_encounter ≈ 65 (at band ceiling) was computed under the old **cross-layer** cascade with potentially higher n_L.

Three concurrent issues:

1. **Pilgrim is Penetrating-gated**: Without Penetrating firing, Pilgrim produces ~23 S_encounter — 35% of band-target [54,74]. Only with Penetrating does Pilgrim approach band.
2. **Tuning Knob 6 requires ≥3 active layers**: character-system.md TK 6 states Pilgrim band-viability requires "≥3 active layers" in the targeted slot.
3. **MVP n=2 Angels have 2-layer slots**: angel-system.md Formula 3 specifies n=2 → 2 layers per slot. Only n=3-4 or God-card encounters have 3-layer slots.

**Net**: Pilgrim is structurally not band-viable at MVP party size against standard MVP Angels.

**Resolution options**:
- (a) Recompute Pilgrim S_encounter under within-slot Penetrating with HP_L ∈ [4,14], max 3-layer slot — then confirm if she meets band.
- (b) Increase MVP Angel layer counts so n=2 encounters have 3-layer slots.
- (c) Redesign Pilgrim Penetrating to remain viable against 2-layer slots.
- (d) Ship Martyr-only for first playtest; defer Pilgrim.

### Warnings

#### ⚠️ Phase 5 attention budget exceeded
6-8 simultaneously active systems during Phase 5 (vs. 3-4 target). T_round at n=4 with items projects ~3.1 min/round → 8-round encounter ≈ 25 min (vs. 5-8 min target × 3-5).
**Mitigation pre-playtest**: Enforce n=2 + 1 Angel + 2 layers/slot as the only validated MVP configuration. Defer n=3-4 and 2-Angel encounters until n=2 attention budget is verified.

#### ⚠️ Martyr S_encounter above band, balancing mechanism undefined
Martyr S_encounter ≈ 79.5 (pre-Integrity-cost). The "Integrity-cost netting" calculation that would bring Martyr into [54,74] is not present in any GDD (DEFERRED item D-6 / D-3).

#### ⚠️ Reservoir has no explicit ceiling
character-system.md Rule 10 states "No automatic threshold. No ceiling." Edge case "If the Reservoir accumulates beyond physical tracker capacity" implies unbounded. Combined with Penetrating + n_L=3, a 30+ Reservoir release × within-slot cascade can deal 90+ damage in one event.
**Recommend**: Add explicit ceiling (e.g., 30 = D_agg(4)).

#### ⚠️ Anti-Pillar 1 tension with Angel HP
game-concept.md Anti-Pillar 1: "NOT separate hit point tracking." Angel System now has per-slot HP tracker dice with explicit HP values. Players have no HP; Angels do. Letter of anti-pillar is about parallel HP systems for *players*; spirit of "Integrity IS the health system" is stretched by Angel HP.
**Recommend**: Reconciliation language at next CD review.

#### ⚠️ Slot naming overlap (player Weapon zone vs. Angel Weapon slot)
PRS on player Weapon slots vs. Angel's own Weapon-slot column. Documented in angel-system.md Interactions table but high-friction at first encounter.
**Recommend**: Glossary entry distinguishing the two.

#### ⚠️ Compound-fire WAS validation tool deferred
angel-system.md AC-21 (validate compound-fire rounds keep WAS_total in band) deferred. No design tool yet exists to detect compound-fire band violations on multi-trigger faces.

#### ⚠️ Martyr L4 Body passive "to ALL enemies" self-damage unresolved
character-system.md Open Ruling 3. Question: can the L4 passive damage the Martyr's own board?
**Recommend**: Resolve before Martyr ships to playtest.

---

## Cross-System Scenario Issues (Phase 4)

Scenarios walked: 5
1. Pilgrim Penetrating burst against 3-layer God slot
2. Martyr self-strip reveals new layer mid-Step 5b with item attached
3. 2-Angel encounter at n=3 with compound BUFF+PRS+HIT
4. Bad Event Type C strips last player layer with attached item (departure trigger)
5. God encounter with persisted Boon modifier + simultaneous termination

### 🔴 Blockers

#### 🔴 B-SCN-1 — Undefined behavior: Item departure-trigger damage with no Angel target
**Systems**: Events System (Bad Event Type C), Item System (departure trigger), Equipment Degradation (Rule 1), Integrity System

**Scenario**: A Bad Event Type C deals hits to a player slot. The hit strips a layer with an attached item whose departure trigger is offensive ("deal 3 damage" or similar). Per Equipment Degradation Rule 1 timing note, the item departure trigger fires after the on-destroy passive, before the new layer reveals.

**Problem**: There is no Angel present during a Bad Event. The damage has no target. None of events-system.md, item-system.md, or integrity-system.md defines what happens.

**Resolution options**:
- (a) Damage fizzles (recommended — simplest, matches "the trigger has no legal target").
- (b) Damage redirects to player Integrity (creates a perverse incentive against attaching damage items).
- (c) Departure triggers scoped to combat encounters only (cleanest rule, but reduces item versatility).

### Warnings

#### ⚠️ Scenario 2 — Item departure-trigger slot-targeting gap (combat case)
When the item departs during combat, which Angel slot receives the damage? Implicit assumption is the host slot, but no rule states it. angel-system.md Rule 6.1 requires explicit slot declaration before advancing trackers — but item departure triggers fire automatically without a player declaration.
**Recommend**: Add explicit clause — departure-trigger damage targets the host slot of the item (or the host Angel's first active slot if host slot is now inert).

#### ⚠️ Scenario 3 — Compound-fire WAS spikes undetected
At n=3 with 2 Angels and median-die showing a trigger-shared face, multiple BUFF+PRS+HIT effects fire on the same round. Per angel-system.md Rule 4.4, designers must verify compound-fire rounds keep WAS_total in band — but AC-21 (validation tool) is DEFERRED. No mechanism today detects a band violation.

#### ⚠️ Scenario 4 — Type C Bad Event hit count cap not specified
events-system.md Tuning Knob 4 acknowledges snowball risk at ≥6 hits but no MVP cap is defined. Two-hit Bad Events can destroy a fragile board in one card.
**Recommend**: Limit Type C Bad Events to max 1 hit per player at MVP composition.

### Info

- **ℹ️ Scenario 1** — Pilgrim Penetrating burst: rules consistent across all four GDDs after the 2026-05-27 redesign. Within-slot cascade resolves cleanly.
- **ℹ️ Scenario 2 (atomic ordering)** — On-destroy + item departure + reveal sequence confirmed in equipment-degradation.md Rule 1 timing note. Die fizzle behavior for the second placed die is defined (Equipment Rule 5.6).
- **ℹ️ Scenario 5** — God encounter simultaneous termination: Rule 6 + Combat AC-22b + win-loss-conditions.md align. Victory prevails for God only.

---

## GDDs Flagged for Revision

| GDD | Reason | Type | Priority |
|-----|--------|------|----------|
| character-system.md | OQ-05 — Pilgrim S_encounter recompute under within-slot Penetrating model (B-DT-1); Martyr L4 Body self-damage ruling open; Reservoir ceiling | Design Theory | **Blocking** |
| events-system.md | Item departure-trigger with no Angel target undefined (B-SCN-1); Type C hit cap missing | Cross-System | **Blocking** |
| item-system.md | Departure-trigger slot-targeting gap during combat; pool exhaustion at n=3-4 | Design | Warning |
| angel-system.md | AC-21 compound-fire WAS validation tool deferred; OQ-05 mirrors character-system; slot naming overlap | Design Theory | Warning |
| combat-system.md | Stale "Angel Rule 6.5" → 6.6 reference; T_round at n=3-4 with items unmeasured (AC-26 deferred); attention budget warning | Consistency + Design | Warning |
| integrity-system.md | Dependencies table outdated wording (Angel Cleared); Formula 4 SUPERSEDED hygiene | Consistency | Warning |
| equipment-degradation.md | Status header not refreshed; Martyr B_weapon_total Integrity-cost netting formula missing (D-6) | Consistency + Design | Warning |
| game-concept.md | Anti-Pillar 1 ("NOT separate hit point tracking") tension with Angel per-slot HP | Design Theory | Warning |
| win-loss-conditions.md | "HP bounds" cosmetic reference (deprecated) | Consistency | Info |

---

## Verdict: **FAIL**

**Summary**: All 7 prior consistency blockers are fixed; propagation introduced no new consistency contradictions. However, the design-theory and cross-system scenario passes surfaced **2 new blockers** that must be resolved before architecture begins.

### Required actions before re-running:

1. **B-DT-1** (character-system.md / angel-system.md): Recompute Pilgrim S_encounter under within-slot Penetrating + HP_L ∈ [4,14] + max 3-layer slot. Determine whether Pilgrim meets band [54,74] at MVP n=2 with 2-layer slots. If not: choose between adjusting MVP Angel layer counts, redesigning Pilgrim Penetrating, or shipping Martyr-only.
2. **B-SCN-1** (events-system.md + item-system.md): Define what happens when an item departure-trigger fires with no Angel target (Bad Event context). Recommended: damage fizzles.

After both blockers resolved, the 5+8 warnings can be addressed in a single revision pass or accepted as playtest-deferred risks. Re-run `/review-all-gdds` or proceed directly to individual `/design-review` passes.
