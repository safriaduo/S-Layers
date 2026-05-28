# Cross-GDD Review Report — S-Layers

**Date:** 2026-05-26
**Skill:** `/review-all-gdds` (full mode)
**GDDs Reviewed:** 12 documents (10 system GDDs + game-concept.md + systems-index.md)
**Systems Covered:** Dice Economy, Integrity, Win/Loss Conditions, Equipment, Equipment Degradation, Character, Angel, Combat, Item, Events
**Entity Registry:** 2 entities, 0 items, 11 formulas, 7 constants — used as conflict baseline

---

## Consistency Issues *(Phase 2: Cross-GDD Consistency)*

### Blocking 🔴

**B-CON-1 — T_round registry catastrophically out of date; internal Formula 2 range mismatch**
- `combat-system.md` (Formula 1 + Formula 2), `design/registry/entities.yaml` (T_round entry)
- **Problem:** Registry `T_round` lists `output_range: [30, 1800]` with coefficients `T_base=30, α_4=120, α_5=90, α_6=60, β=30`. The GDD body uses completely different values (`T_base=25` provisional; corrected range `[37, 193]`). Additionally, inside `combat-system.md` itself: Formula 1 corrected the range to `[37, 193]` but Formula 2's variables table still shows the stale `[55, 175]`. The GDD flagged this as OQ-2 but it was never resolved.
- **Must change:** (1) Update registry `T_round` entry with current GDD-body coefficients and range `[37, 193]`. (2) Fix `combat-system.md` Formula 2 variables table to read `[37, 193]`.

**B-CON-2 — Win/Loss Rule 6 missing "God card only" qualifier**
- `win-loss-conditions.md` (Rule 6), `combat-system.md` (Rule 2, AC-22a/22b)
- **Problem:** Rule 6 reads universally: *"If Check B finds both 'Angel cleared' and 'player board destroyed'… Victory prevails."* The God-only scope is buried in the Edge Cases section. Combat System Rule 2 silently relies on the narrow reading. A reader of Win/Loss Rule 6 in isolation — including a playtester — draws the wrong conclusion for non-God encounters.
- **Must change:** Rewrite Win/Loss Rule 6 to open with: *"Applies exclusively to the God card encounter. For non-God Angels: if both conditions occur in the same phase, Lost takes priority."*

---

### Warnings ⚠️

**W-CON-1 — T_accum honest-feeding constant: 2.275 vs. 2.75 across three GDDs**
- `dice-economy.md` (Accumulate Threshold Design Tiers), `equipment-system.md` (Formula 2 note), `item-system.md` (Formula 2 example)
- Dice Economy (owner of T_accum) uses 2.75. Equipment System and Item System use 2.275 (strict: 0.65 × 3.5) for conservative calibration, with notes flagging the conflict. No GDD has adopted a canonical position.
- **Action:** Dice Economy should publish two values formally: *gameplay tier guidance* = 2.75 (rounded); *design-time calibration* = 2.275 (strict). Each downstream GDD cites the appropriate one.

**W-CON-2 — Entity registry `referenced_by` fields stale across multiple entries**
- `entities.yaml` (P_angel, D_agg, D_player_base, HP_alpha, HP_beta, T_angel, E_angel, EPC, T_round, T_accum, the_pilgrim, the_martyr)
- All entries with comments *"add when authored"* for angel-system.md and events-system.md are now stale — both GDDs are authored and reference these values. Combat System references Pilgrim/Martyr layer counts in Phase 4.
- **Action:** Add `angel-system.md`, `events-system.md`, and `combat-system.md` to `referenced_by` lists across all applicable registry entries.

**W-CON-3 — Layer ordering ownership split ambiguous in systems-index**
- `systems-index.md` (Ownership Statements), `character-system.md` (Dependencies), `equipment-degradation.md` (Dependencies)
- Both GDDs' prose correctly split it (principles vs. application) but the systems-index table could be misread as Equipment Degradation owning all of layer ordering.
- **Action:** Add parenthetical in systems-index: Character System "owns layer ordering *per character*"; Equipment Degradation "owns layer ordering *design principles that govern them*."

**W-CON-4 — D_agg_max unpublished; three downstream GDDs waiting**
- `integrity-system.md`, `character-system.md`, `angel-system.md`
- Integrity System defines D_agg open-ended; Character System OQ-01 and Angel System dependencies both flag they need D_agg_max. Computable but never formally published.
- **Action:** Character System or Integrity System to add: *D_agg_max = 32 (at n=4, D_player_base=8, honest play)*. Register as a constant.

**W-CON-5 — Item System Dependencies lists `accumulate_ceiling` as consumed; no Type can violate it**
- `item-system.md` (Dependencies)
- Type 3 items only reduce thresholds; no type raises them. The ceiling constraint is vacuously satisfied — the Dependencies note is misleading.
- **Action:** Trim to: *"Type 3 items must not reduce below accumulate_floor=4. The accumulate_ceiling=20 constrains future Type design that could raise thresholds."*

---

### Info ℹ️

- **I-CON-1:** All critical numerical constants verified consistent: Pilgrim L_total=8, Martyr L_total=10, D_player_base=8, T_encounter=8, T_target=64, band [54,74], accumulate_floor=4, accumulate_ceiling=20. ✓
- **I-CON-2:** T_round (Combat) vs. T_encounter (Equipment) distinction: no conflation found in any GDD. ✓
- **I-CON-3:** First Player rotation cadence (every Event card): events-system.md Rule 7 and combat-system.md Cleanup Step 6 agree. ✓
- **I-CON-4:** Item discard on layer strip: item-system.md Rule 4.4 and equipment-degradation.md Rule 1 Step 3 agree; items do not return on restoration unless card text says so. ✓
- **I-CON-5:** Win/Loss Check C (post-bad-event board destruction): bidirectionally updated between events-system.md and win-loss-conditions.md. ✓
- **I-CON-6:** Multi-Angel Phase 1 roll (session state OQ-3): angel-system.md Rule 7.1 resolves (shared 3D6); confirmed by combat-system.md OQ-3 [RESOLVED]. ✓

---

## Game Design Issues *(Phase 3: Design Theory)*

### Blocking 🔴

**B-DES-1 — Pillar 5 (Combos Must Be Discoverable) structurally undeliverable at MVP**
- `game-concept.md` (Pillar 5), `item-system.md` (Tuning Knob 7), `character-system.md` (SR-3)
- **Problem:** Item System Tuning Knob 7 explicitly defers *non-obvious* synergies to post-MVP — only *legible* item + layer TYPE combos ship in MVP. Equipment Degradation Principle 3 mandates layers "rhyme" with the outermost, making deeper layers predictable. Character System SR-3 mandates one non-obvious synergy per gift, but with 10 items and 2 characters, the combinatorial surface is tiny. The MVP ships without the mechanic its fifth pillar describes. Playtests cannot validate it.
- **This is a design credibility failure:** Pillar 5 appears in marketing language; the GDD authoring work quietly makes it undeliverable.
- **Recommendation:** Choose one of: (a) downgrade Pillar 5 to a post-MVP design goal in game-concept.md with explicit note it cannot be validated in the prototype; (b) commit the MVP item pool to shipping three SR-3 non-obvious synergies as a hard requirement, documented in a synergy design sheet before card production; (c) redesign one MVP item to create a single showcase "aha" combo that anchors the pillar.

**B-DES-2 — Max-selection is the dominant strategy; Pillar 1 promise contradicted by constraint mix**
- `dice-economy.md` (Formula 4: +27.7%), `equipment-system.md` (constraint distribution), `character-system.md`
- **Problem:** Dice Economy Formula 4 explicitly rewards max-selection. The MVP constraint set is overwhelmingly `[N≥X]` — only one `[N≤X]` use case in the whole MVP (Pilgrim's `[N=1]`, p=0.31). The Martyr has no slot that rewards low rolls. The "dump-dice mandate" (Equipment System Section 6) is structurally violated: rolls of 1–2 for the Martyr are routinely wasted. Pillar 1 says "no roll leaves you helpless"; the math contradicts this for ~22% of Martyr rolls.
- **Recommendation:** Add at least one `[N:odd]`, `[N≤3]`, or `[N=low]`-rewarding slot to the Martyr's starting set, or redesign one Martyr zone so placing a 1 or 2 triggers a distinct, valuable effect.

---

### Warnings ⚠️

**W-DES-1 — Two competing core fantasies; decay loop will dominate item-growth loop**
- `game-concept.md`, `equipment-degradation.md` (Player Fantasy), `item-system.md` (Player Fantasy)
- Degradation events land ~8 times per encounter; item acquisitions number ~4 per full session. The decay loop dominates by frequency and drama. Both GDDs claim co-equal fantasy status.
- **Recommendation:** Reframe items formally as *modifiers to the decay loop* rather than a co-equal axis, or significantly increase item acquisition rate.

**W-DES-2 — Pilgrim's only viable damage path is Penetrating + Reservoir**
- `character-system.md` (Formula 3/4, Tuning Knob 6)
- Formula 4: Pilgrim S_encounter ≈ 23 without Penetrating, ≈ 65 with Penetrating at n_L=3. A "combo" that is the only viable line isn't a build with broken possibilities — it's a puzzle with one solution. Contradicts Pillar 5.
- **Recommendation:** Raise Pilgrim's non-Penetrating output floor so Penetrating becomes a *multiplier* on a viable baseline, not the only path to being in-band.

**W-DES-3 — Bad Events have no decision surface; Pillar 1 risk for ≥3 events per session**
- `events-system.md` (Type C Bad Events)
- Bad Events deal slot damage with zero dice involvement — pure top-down punishment. Only decision: where to route damage. The events-system Player Fantasy accepts this ("the sentence was already written"), but that's a narrative defense for a mechanical agency gap.
- **Recommendation:** Option A: add a mitigation window ("reveal 1D6; on ≥4, reduce incoming damage by 1"). Option B: formally cap Bad Events at ≤2 per Event deck and document as a Pillar 1 compliance constraint in Events Tuning Knobs.

**W-DES-4 — Attention budget: 7–8 simultaneous decisions in full-complexity Pilgrim+Martyr round**
- `combat-system.md`, `character-system.md`, `equipment-system.md`, `integrity-system.md`
- Active decisions: (1) die-to-slot assignment, (2) Accumulate feeding vs. immediate, (3) gift activation timing, (4) Reservoir release, (5) Penetrating flag staging, (6) effect resolution order, (7) multi-Angel targeting, (8) Phase 6 hit routing. Count: 7–8. T_round projects 90–140s/round at this load.
- **Recommendation:** No structural change now. Flag for playtest measurement at n=2 full complexity. If T_round exceeds 150s consistently, simplify one character's decision surface.

---

### Info ℹ️

- **I-DES-1:** Item Type 4 (Mutation) at 1/10 rarity means ~40% of sessions never feature the signature "broken build" emergent moment. Consider 2/10.
- **I-DES-2:** Reservoir total-loss-on-strip creates strong "never route hits to Pilgrim Weapon" pressure. Flag for behavioral playtest observation.
- **I-DES-3:** Item Rule 3 First Player split authority for item distribution is elegant for Pillar 4; observe for power-dynamic formation in playtests.

### Pillar Coverage Matrix

| Pillar | Systems serving it | Strength |
|--------|-------------------|----------|
| 1. Agency Over Dice | Dice Economy (primary), Equipment, Character, Combat | **Moderate** — undermined by max-select dominance and Bad Event zero-agency rounds |
| 2. The Build is Never Finished | Equipment Degradation (primary), Item, Character | **Strong** — decay loop is well-architected and emotionally specific |
| 3. Fast and Focused | Integrity, Win/Loss, Combat, Events | **Moderate** — achievable at n=2 base; at-risk with full character complexity |
| 4. Cooperative Ownership | Integrity (routing), Character (3 cross-board decisions), Item (split distribution) | **Strong** — multiple load-bearing co-op decisions per round |
| 5. Combos Must Be Discoverable | Item Tuning Knob 7 (explicitly deferred to post-MVP) | **Weak — explicitly deferred** |

### Attention Budget Count

Active decisions during Phase 4+5, Pilgrim+Martyr, one item in play:
1. Which die to which equipment slot
2. Accumulate feeding vs. immediate effect
3. Divine gift activation timing (4 windows)
4. Reservoir release decision (Pilgrim)
5. Penetrating flag staging
6. Resolution order of all placed-die effects across both players
7. Damage targeting per Angel if multi-Angel
8. Hit routing across teammate boards (Phase 6)

**Count: 7–8 simultaneous decisions.** Exceeds the 4-decision soft ceiling. Co-op license softens it; calibration must wait for playtesting.

---

## Cross-System Scenario Issues *(Phase 4: Scenario Walkthrough)*

**Scenarios walked:** 5
1. Martyr Weapon final-layer self-strip with attached item
2. Penetrating damage on multi-layered Angel — cascade question
3. Treasure event after board-destruction sequence
4. PRS + BUFF + mid-Phase-5 layer reveal
5. God card drawn with existing non-God Angel still live

### Blockers 🔴

**B-SCN-1 — Penetrating multi-strip: four GDDs contradict each other; Pilgrim balance depends on outcome**
- **Systems:** Integrity (Rule 20), Character (Rule 11, CS-AC-30), Combat (AC-30a), Angel (Rule 6.5)
- **Step:** Phase 5b — Penetrating damage event fires across multiple Angel layers
- `integrity-system.md` Rule 20: *"A single turn's aggregated damage removes at most one Angel layer, regardless of magnitude."* — absolute.
- `angel-system.md` Rule 6.5: *"No mid-phase checks. The single check fires at end of Effect Resolution Phase only."*
- `combat-system.md` AC-30a: *"both layers strip in sequence… newly innermost layer is inert for the remainder of Phase 5."* — asserts cascade.
- `character-system.md` CS-AC-30: DEFERRED with *"conservative interim ruling: physical strip events limited to at most one layer per turn."*
- **Balance dependency:** Character System Formula 4 shows Pilgrim S_encounter ≈ 23 *without* cascade and ≈ 65 *with* it (at n_L=3). Without cascade, Pilgrim is **31 points below the [54,74] band** — unplayable at current design.
- **Must resolve:** (a) Penetrating is the explicit exception to Integrity Rule 20 and Angel Rule 6.5 — update both + specify passive firing order + item departure order + HP_eff carryover on new layer; OR (b) Penetrating "touches" all layers for passive triggers only, strips ≤1/turn — and Pilgrim's S_encounter must be recalculated from scratch.

**B-SCN-2 — BUFF HP_eff carryover undefined when mid-Phase-5 strip reveals new Angel layer**
- **Systems:** Angel (Rule 5.1, HP_eff), Combat (Step 5a, AC-33), Equipment Degradation (ED-AC-24)
- **Step:** Phase 5, after BUFF announcement at 5a, if cascade strip (B-SCN-1 path) reveals new layer mid-5b
- BUFF is announced against active Angel layer's HP_L. If mid-phase strip reveals new layer with different HP_L, no GDD specifies whether HP_eff resets or carries. Angel System assumes one layer per phase.
- **Tightly linked to B-SCN-1.** If cascade is denied, this scenario never occurs. If cascade is permitted: add explicit rule to `angel-system.md` — *"If a new layer is revealed mid-Phase 5, HP_eff is recalculated fresh against the new layer's HP_L plus any active BUFF for this turn."*

---

### Warnings ⚠️

**W-SCN-1 — Penetrating flag + item departure-trigger damage interaction undefined**
- **Scenario:** Martyr Weapon final-layer self-strip with attached item that has departure trigger ("deal 3 damage")
- Character edge case specifies *"Penetrating applies to external Angel damage only."* But whether an item's departure-trigger damage counts as "external Angel damage" for Penetrating purposes is unspecified — could create unintended double-application.
- **Resolve:** Add to Character System Rule 11 Edge Cases: *"Item departure-trigger damage does not receive the Penetrating flag. Only the die effect that carried the Penetrating condition applies Penetrating."*

**W-SCN-2 — Post-God-defeat item award: fires or discarded? (Item OQ-3 unresolved)**
- **Scenario:** God card encounter ends in Victory
- Win/Loss Rule 7 preempts everything after termination. Post-combat item award for God defeat is therefore discarded. This may be intentional but was flagged as OQ-3 in Item System and never closed.
- **Resolve:** Add Edge Case to `item-system.md`: *"Post-combat item awards do not fire after the God card encounter. Termination takes immediate effect per Win/Loss Rule 7."*

**W-SCN-3 — On-destroy passive vs. item departure trigger ordering on same stripped layer**
- **Scenario:** Multi-hit routing strips a layer with both an on-destroy passive AND an attached item with departure trigger
- Rule 1 Step 3 places item departure before new layer reveals (Step 4), but on-destroy passive fires at strip. Order between them on the *same* stripped layer is unspecified.
- **Resolve:** Add to `equipment-degradation.md` Rule 1: *"When a layer with an attached item is stripped, resolution order is: (1) on-destroy passive of stripped layer, (2) item departure trigger, (3) new layer reveals."*

---

### Info ℹ️

- **I-SCN-1:** PRS + BUFF + mid-phase strip: ED-AC-24 (new layer inert this phase) and Equipment Rule 5.6 (die fizzles) interact cleanly. ✓
- **I-SCN-2:** Treasure event + Loss sequence: Combat Rule 4.1 awards items only on encounter Win; no double-trigger possible. ✓
- **I-SCN-3:** God card draw with existing Angel: Events Rule 4.6 freezes deck during combat; God card cannot be drawn mid-encounter. Type D Boon deck peek correctly anchors God to bottom. ✓

---

## GDDs Flagged for Revision

| GDD | Reason | Type | Priority |
|-----|--------|------|----------|
| `design/registry/entities.yaml` | T_round coefficients/range out-of-date; `referenced_by` missing for angel, events, combat-system across 12+ entries | Consistency | Blocking |
| `win-loss-conditions.md` | Rule 6 missing "God card only" qualifier — reads as universal | Consistency | Blocking |
| `combat-system.md` | Formula 2 table stale [55,175]; AC-30a asserts cascade strips contradicting Integrity Rule 20 | Consistency + Scenario | Blocking |
| `integrity-system.md` | Rule 20 "no cascade" must gain Penetrating exception or be defended as absolute | Scenario | Blocking |
| `character-system.md` | CS-AC-30 DEFERRED; Pilgrim single-path violation of Pillar 5; missing dump-dice slot; Penetrating + departure trigger order unspecified | Scenario + Design | Blocking |
| `angel-system.md` | Rule 6.5 vs. cascade; HP_eff carryover undefined on mid-phase reveal | Scenario | Blocking |
| `game-concept.md` | Pillar 5 not deliverable at MVP per Item Tuning Knob 7 — downgrade or commit | Design Theory | Blocking |
| `item-system.md` | Synergy depth deferred contradicts Pillar 5; OQ-3 unresolved; departure trigger + Penetrating order unspecified; on-destroy vs. departure order unspecified | Design + Scenario | Blocking |
| `equipment-system.md` | Constraint distribution all `[N≥X]`; Martyr dump-dice slots missing | Design Theory | Blocking |
| `dice-economy.md` | Publish canonical 2.275 vs. 2.75 split as two formal values | Consistency | Warning |
| `events-system.md` | Bad Events no decision surface (Pillar 1); Bad Event count uncapped; OQ-3 (post-God item award) unresolved | Design + Scenario | Warning |
| `systems-index.md` | Layer ordering ownership wording ambiguous | Consistency | Low |

---

## Verdict: ❌ FAIL

**Blocking issues: 7** (across consistency, design theory, and scenario phases)

The GDD set is structurally strong — formula chains traceable, dependency graph sound, all critical numerical constants consistent, cooperative mechanics structurally anchored. Equipment Degradation is the standout: transformation-through-spending is an unusually specific and well-realized design. This is not a design in trouble. It is a design that has outgrown its open rulings.

**Three root causes of FAIL:**

1. **Penetrating is unresolved and blocks character balance.** Integrity Rule 20 (no cascade) and Combat AC-30a (cascade asserted) directly contradict. Pilgrim is 31 points below the balance band if cascade is denied. Four GDDs disagree. This must be resolved before any architecture, card production, or prototype printing.

2. **Pillar 5 is structurally undeliverable at MVP.** Item Tuning Knob 7 explicitly defers non-obvious synergies. The gap between "Pillar 5: Combos Must Be Discoverable" and "Deferred to post-MVP" will be visible to playtesters and publishers. Either formally downgrade the pillar or commit to a hard synergy deliverable.

3. **Max-selection is the dominant strategy; Martyr has no dump-dice home.** Pillar 1 cannot be claimed when low rolls for the Martyr are systematically useless. Requires one constraint redesign — not a wholesale rewrite.

### Required Actions Before Re-Run

1. **Penetrating ruling (B-SCN-1 + B-SCN-2):** Choose: cascade exception (update Integrity Rule 20 + Angel Rule 6.5 + specify all ordering rules + HP_eff carryover), OR no-cascade with Pilgrim S_encounter recalculation.
2. **Win/Loss Rule 6 (B-CON-2):** Add God-only qualifier to the rule body.
3. **T_round registry (B-CON-1):** Update coefficients and `output_range: [37, 193]`. Fix combat-system.md Formula 2 table.
4. **Pillar 5 (B-DES-1):** Downgrade in game-concept.md, or commit to 3 SR-3 synergies as hard MVP deliverable.
5. **Martyr dump-dice (B-DES-2):** Add ≥1 `[N≤3]` or `[N:odd]` slot to Martyr's starting set.
6. **Penetrating flag + item departure trigger (W-SCN-1):** Add one-line ruling to character-system.md Rule 11 Edge Cases.
7. **Post-God item award (W-SCN-2):** Close OQ-3 in item-system.md with explicit Edge Case.
8. **On-destroy vs. item departure ordering (W-SCN-3):** Add ordering rule to equipment-degradation.md Rule 1.

---

*Report generated by `/review-all-gdds` — three parallel review agents (Consistency, Design Theory, Cross-System Scenarios). Next step: resolve all 7 blockers, then re-run `/review-all-gdds` or run `/gate-check systems-design`.*
