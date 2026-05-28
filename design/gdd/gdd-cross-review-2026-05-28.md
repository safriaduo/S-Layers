# Cross-GDD Review Report
**Date**: 2026-05-28
**GDDs Reviewed**: 12
**Systems Covered**: Dice Economy, Integrity System, Win/Loss Conditions, Equipment System, Angel System, Character System, Equipment Degradation, Combat System, Item System, Events System (+ game-concept, systems-index)

---

## Prior Blockers — Re-Verification

| Blocker | Status |
|---|---|
| B-DT-1 — Pilgrim cascade Penetrating model | ✅ RESOLVED & PROPAGATED |
| B-SCN-1 — item departure-trigger no-Angel target → fizzles | ✅ RESOLVED & PROPAGATED |
| HP_alpha / HP_beta deprecation | ✅ CLEAN (deprecated in registry; all GDD refs explicitly historical) |

Both prior blockers from `gdd-cross-review-2026-05-27.md` are fully propagated across all dependent GDDs.

---

## Consistency Issues

### Blocking
**None.**

### Warnings

#### ⚠️ W-CON-1 — Stale HP example in `integrity-system.md`
- **File**: `integrity-system.md` line 197
- **Quoted text**: *"HP_L = 19 — hard layer (near HP_max), T_angel ≈ 2.5–3 turns."*
- **Issue**: HP bounds formula (HP_max = floor(HP_beta × D_agg)) deprecated 2026-05-27. Active Angel slot HP range is now [4, 14] per `angel-system.md` Formula 2. HP_L=19 is outside the valid range; "near HP_max" references a deprecated constant.
- **Fix**: Update example to use HP_L ∈ [4, 14] (e.g., HP_L = 14); remove "near HP_max". **XS effort.**

#### ⚠️ W-CON-2 — T_active definition mismatch in `item-system.md`
- **Files**: `item-system.md` Rule 7 (line 235) and Formula 1 variable table (line 332) state `T_active ≈ 1.0 at MVP`; the same GDD's worked examples (line 343), IS-AC-27 (line 700), and Formula 3 example (line 412) all use `T_active = 2.7`.
- **Issue**: The "1.0" figure is a stale draft remnant. The 2.7 value (= T_encounter / N_layers, matching `equipment-degradation.md` Formula 2) is the intended and used interpretation.
- **Fix**: Replace "≈ 1.0 at MVP" in Rule 7 and Formula 1 variable table with "≈ 2.7 turns per layer at MVP (T_encounter / N_layers; matches Equipment Degradation Formula 2)". **S effort.**

#### ⚠️ W-CON-3 — Procedural ordering conflict: `combat-system.md` Phase 5b vs. `integrity-system.md` Rule 17
- **Files**: `combat-system.md` Phase 5b edge case (line 396) states Step 5c does not execute when a self-strip destroys the Martyr board mid-5b. `integrity-system.md` Rule 17 states strip checks for all slots resolve before any Win/Loss evaluation.
- **Issue**: Outcome (Loss prevails for non-God) is identical; procedural record differs. The Angel layer at tracker 20 vs HP 14 may or may not be stripped in the record. This is a genuine 2b rule contradiction.
- **Fix**: Amend `combat-system.md` Phase 5b edge case to: *"Step 5c strip checks fire for all Angel slots BEFORE Win/Loss evaluation, per Integrity Rule 17. If the Angel is thereby Cleared, both conditions are active — for non-God: Loss takes priority; for God: Victory prevails."* **S effort.**

#### ⚠️ W-CON-4 — Combat-context offensive departure trigger has no declared target
- **Files**: `item-system.md` Rule 6.1/6.2; `angel-system.md` Rule 6.1.
- **Issue**: `angel-system.md` Rule 6.1 requires explicit Angel + slot declaration before tracker advance. Offensive departure triggers in combat fire automatically without a player declaration of which Angel + slot receives the damage. Bad Event context is covered (B-SCN-1). Combat context is still implicit.
- **Fix**: Add to `item-system.md` Rule 6.2: *"Offensive departure-trigger damage in combat targets the host Angel and the slot whose strip caused departure; if the source slot is ambiguous (e.g., Bad Event in a multi-Angel encounter), the party declares before the trigger resolves."* **XS effort.**

---

## Game Design Issues

### Blocking

#### 🔴 B-ATTN-1 — Player attention budget overflow
- **Systems involved**: All 10 MVP systems + cooperative scope requirement
- **Issue**: During a single Phase 4 dice-placement decision, a player must actively track **9 simultaneous systems**:
  1. Dice Economy — hand composition (2–3 dice)
  2. Equipment System — own 3 zones, constraints
  3. Equipment Degradation — own layers remaining + indicators
  4. Item System — items modifying constraints/output/thresholds
  5. Character System — gift state, Reservoir total, Penetrating flag
  6. Angel System — 3 slot columns × HP × tracker × trigger faces × BUFF/PRS
  7. Integrity System — own Accumulate totals + teammate states for routing
  8. Combat System — committed face + BUFF/PRS already announced
  9. Cooperative Scope — full teammate board (Pillar 4 requirement)

  Pillar 3 ("Fast and Focused") is declared primary by 6 of 10 MVP systems. Research norm: 3–4 concurrent active systems is the comfortable ceiling. The post-2026-05-27 Angel System 3-slot redesign tripled enemy-side tracking complexity. `combat-system.md` EPC projects new-player T_round ~115–120s → WARN_LONG at 10–11 minute encounters, violating the 5–8 min target.

- **Source**: `angel-system.md` Rules 1–9 (3-slot redesign); `combat-system.md` Formula 1; `character-system.md` Decision 2 (cross-board visibility requirement); `integrity-system.md` Pillar 3 declaration.

- **Required resolution options** (one must be chosen before architecture begins):
  - (A) Run a focused physical prototype pass on Phase 4 complexity before committing to the 3-slot Angel design; use playtest data to validate T_round ≤ 90s. Add a documented "EPC validated by playtest" note to `angel-system.md` clearing this blocker.
  - (B) Add an explicit design-risk acknowledgment to `angel-system.md` and `combat-system.md` accepting that the 3-slot complexity requires iconography and layout discipline to reach the 90s target; commit to a Pillar 3 pass criterion in the MVP playtest brief (*T_round ≤ 90s for 80% of rounds at n=2*).
  - Option (B) does not require GDD rewrites; it documents the risk and gates it against empirical validation.

#### 🔴 B-REST-1 — No equipment layer restoration in MVP item pool; session is a loss spiral
- **Systems involved**: `item-system.md` (Formula 4), `equipment-degradation.md` (Tuning Knob 5), `combat-system.md` (Tuning Knob 7)
- **Issue**:
  - `equipment-degradation.md` Tuning Knob 5: *"How often players can recover stripped layers… This GDD does not set this value — it is owned by Item System."*
  - `item-system.md` Formula 4 MVP pool: 4 Constraint + 3 Amplification + 2 Threshold + 1 Mutation = **0 restoration items**.
  - `combat-system.md` Tuning Knob 7: inter-encounter recovery **LOCKED** (permanent within session).
  - At H_pt = 1.0, each player loses ~8 layers per encounter. ~8 encounters before God. L_total per player = 8–10. The Martyr Head Accumulate heal (2×/encounter, 1 Integrity) is the only in-game recovery — insufficient to replace layer loss.
  - **Result**: Player boards cannot survive past encounter 1 without a restoration source that has not been designed. The 60–90 min cooperative win-eligible session target in `game-concept.md` is incompatible with the current source/sink balance.

- **Required resolution options**:
  - (A) Add ≥1 restoration item to the MVP item pool in `item-system.md` Formula 4. Recommended: replace the 1 Mutation item (Type 4) with 1 Restoration item; add restoration rules to `equipment-degradation.md` Tuning Knob 5. **(Most direct fix.)**
  - (B) Define a partial inter-encounter recovery mechanic in `events-system.md` (e.g., boon events restore 1 layer to the most-depleted slot). Requires `combat-system.md` Tuning Knob 7 to be unlocked for boon-driven restoration.
  - (C) Accept high loss rate and explicitly frame the session arc as designed-failure — but update `game-concept.md` session target to reflect this is a loss-probable arc, not a win-eligible 60–90 min session.

### Warnings

#### ⚠️ W-DT-1 — Pilgrim+Martyr is the only viable MVP party composition
- `character-system.md` Tuning Knob 6: Pilgrim base S_encounter ≈ 37–40 at n=2 (below [54, 74] band); band-reach requires Martyr Penetrating-cascade coupling. At MVP (2 characters), only one party exists. Pillar 5 ("Combos Must Be Discoverable") weakened — the central combo is mandatory, not discovered.
- **Not blocking at MVP. Will become blocking before KS Demo.** Future characters 3–6 must include alternate Penetrating finishers or independent band-reaching archetypes.

#### ⚠️ W-DT-2 — Routing-to-Martyr-Body is deterministic
- Martyr Body 4-layer escalating on-destroy passives (5/6/7/8 damage) dominate every routing scenario. Pilgrim Body L1 (`[N=1]` Gain-1-Die) is strictly weaker as a routing target. The Pillar 4 "cooperative routing debate" reduces to a non-decision.
- **Recommend**: Design Pilgrim Body L1 on-strip effect to be routing-worthy before MVP playtests. The cooperative routing decision is load-bearing for Pillar 4.

#### ⚠️ W-DT-3 — Reservoir hold-for-Penetrating dominates early release
- `character-system.md` Formula 3 confirms damage-per-turn-invested is constant regardless of release timing, but holding until Penetrating achieves cascade coupling. For the optimizer audience, "should I release now?" has a near-deterministic answer: hold.
- **Advisory.** Consider whether Pilgrim needs a competing incentive to release early (e.g., a boon that fires only on Reservoir ≤ 12).

#### ⚠️ W-DT-4 — Type 2 item on Pilgrim Reservoir is the prescribed band-reaching answer
- `character-system.md` Tuning Knob 6 names the solution explicitly. With only 3 Type 2 items in the 10-item pool, the first Type 2 will route to Pilgrim Reservoir as standard play. Discovery is bypassed.
- **Acknowledged in `item-system.md` Tuning Knob 7 as post-MVP. Advisory at MVP scope.**

#### ⚠️ W-DT-5 — Cross-encounter difficulty curve bends sharply upward without restoration
- Angel HP resets each encounter (fresh outer layers). Player board state does not. By encounter 4–5, Pilgrim is likely fully depleted under any non-trivial routing distribution. Compounds B-REST-1.
- **Resolves naturally if B-REST-1 Option (A) or (B) is implemented; monitor in playtest.**

#### ⚠️ W-DT-6 — Pilgrim viability is RNG-conditional on early item draw
- Pilgrim needs a Type 2 item on Reservoir to reach band. No mechanism guarantees one appears before encounter 1.
- **Add to MVP playtest protocol**: track early-item-type distribution. May require seeding a Type 2 in the first treasure event for controlled playtests.

#### ⚠️ W-DT-7 — Pillar 5 (Combos Must Be Discoverable) deferred; MVP cannot validate the central hypothesis
- `item-system.md` Tuning Knob 7: MVP synergies are *"legible on first read"* — which contradicts the Pillar 5 design test. The MVP combinatorial surface is too shallow to seed undiscovered combos.
- **Known scoping decision.** Add to MVP playtest brief: explicitly state what the MVP *can* test (Pillars 1–4, encounter pacing, cooperative routing) and what it *cannot* (Pillar 5 discovery arc).

#### ⚠️ W-DT-8 — Pillar 3 (Fast and Focused) is the most-claimed and most-violated pillar structurally
- Declared primary by 6 systems. 9-system attention budget and 3-slot Angel complexity make structural weight the highest in the project's history. Iconography discipline and read-aloud rules are countermeasures but cannot be verified on paper.
- **Requires empirical validation.** Add Pillar 3 pass criterion to MVP playtest protocol: *T_round ≤ 90s at n=2 for 80% of rounds.*

---

## Cross-System Scenario Issues

**Scenarios walked**: 5

1. Pilgrim Penetrating cascade into 3-layer Angel Body slot
2. Bad Event strips player layer with attached offensive-departure item, no Angel present
3. Martyr Weapon final self-strip simultaneously clears non-God Angel last slot AND destroys board
4. Pilgrim Head Accumulate fires + Reservoir Release same round, same Angel slot (cascade)
5. Pilgrim gift + PRS lockout — future-die queued but all placement locked

### Blockers
**None.**

### Warnings

**⚠️ Scenario B / W-CON-4 — Combat-context departure trigger target undeclared**
See W-CON-4 above. Bad Event case is resolved (B-SCN-1). Combat case remains implicit.

**⚠️ Scenario C / W-CON-3 — `combat-system.md` Phase 5b vs. `integrity-system.md` Rule 17**
See W-CON-3 above. Outcome identical; procedural record diverges.

### Info

**ℹ️ Scenario A** — B-DT-1 fix confirmed end-to-end. Cascade model (Angel Rule 6.6, Character Rule 11 + Formula 3, Integrity Rule 20, Combat Step 5c) fully consistent. Retired AOE formula referenced only as explicitly-retired in `character-system.md:351,408`. ✓

**ℹ️ Scenario D** — Pilgrim Head Accumulate + Reservoir Penetrating-cascade same-round confirmed clean across `character-system.md` AC-28/29/30, `angel-system.md` AC-16, `combat-system.md` AC-32, `equipment-degradation.md` Rule 1 atomic procedure. ✓

**ℹ️ Scenario E** — Pilgrim gift + PRS lockout confirmed clean. `character-system.md` Edge Case line 452 and `combat-system.md` edge case line 372 consistent: sacrificed prior die, no return under PRS. ✓

---

## GDDs Flagged for Revision

| GDD | Issue | Type | Priority |
|---|---|---|---|
| `integrity-system.md` | W-CON-1: stale HP_L=19 "near HP_max" example | Consistency | Warning |
| `item-system.md` | W-CON-2: T_active=1.0 vs 2.7 prose inconsistency | Consistency | Warning |
| `combat-system.md` | W-CON-3: Phase 5b edge case vs Integrity Rule 17 ordering | Consistency | Warning |
| `item-system.md` | W-CON-4: combat-context departure trigger target undeclared | Consistency | Warning |
| `angel-system.md` / `combat-system.md` | B-ATTN-1: attention budget overflow; EPC needs 3-slot revalidation | Design Theory | **Blocking** |
| `item-system.md` / `equipment-degradation.md` | B-REST-1: no restoration items in MVP pool; loss spiral | Design Theory | **Blocking** |

---

## Verdict: FAIL

**2 blocking design-theory issues must be resolved before architecture begins.**

### Required actions before re-running `/review-all-gdds`:

1. **B-ATTN-1** — Choose resolution path A or B above. Path B (document risk, add Pillar 3 playtest gate) is the lowest-effort option and does not require GDD rewrites. Modify `angel-system.md` and `combat-system.md` to acknowledge the attention budget risk and commit to empirical validation at MVP prototype.

2. **B-REST-1** — Add ≥1 restoration item to `item-system.md` Formula 4 MVP pool allocation, and close the Tuning Knob 5 handoff in `equipment-degradation.md` with a reference to the restoration item. Recommended: replace the 1 Mutation item with 1 Restoration item type; define restoration rules (e.g., "restore 1 stripped layer to a player-chosen slot") and calibration notes.
