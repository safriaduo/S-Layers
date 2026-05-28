# Design Change Impact Report
**GDD**: `design/gdd/angel-system.md`
**Date**: 2026-05-27
**Propagated by**: `/propagate-design-change design/gdd/angel-system.md`
**Review mode**: lean (TD-CHANGE-IMPACT skipped)
**ADRs at time of propagation**: 0 (no architecture decision records written yet)

---

## Change Summary

The Angel System GDD underwent a complete structural redesign between its committed
version (2026-05-25) and the working version (2026-05-27).

### Core Architecture Change

| Dimension | Old (2026-05-25) | New (2026-05-27) |
|---|---|---|
| **Board model** | Monolithic layer stack per Angel | 3-slot board (Head / Body / Weapon) |
| **Tracker** | Single Angel Damage Tracker Die per Angel | One Slot Tracker Die per slot column |
| **Damage declaration** | Declare target Angel | Declare target Angel AND slot |
| **Cleared definition** | All layers of one stack stripped | All three slots Inert |
| **Penetrating** | Hits all current layers simultaneously | Within-slot cascade only |
| **HP range** | [HP_min, HP_max] = [10, 19] at n=2 via HP_alpha/HP_beta bounds | [4, 14] per slot layer via WAS-based calibration |
| **Party-size scaling** | Different HP card variants per party size | Fixed HP on cards; scaling via layer count per slot (Target Party Size field on layer cards) + angel count on encounter card |
| **HP formula** | HP_min = ⌈HP_alpha × D_agg⌉; HP_max = ⌊HP_beta × D_agg⌋ | WAS_slot_Lk = F_slot × S_slot; HP calibrated to keep T_angel ∈ [1.0, 3.0] |

### New Formulas Added

| Formula | Description |
|---|---|
| **Formula 1: WAS_slot_Lk** | Weighted Action Score per slot layer — trigger probability × severity |
| **Formula 2: HP calibration** | Per-slot HP calibrated against D_agg and T_angel validity |
| **Formula 3: Party-size scaling** | Layer depth per slot × angel count from encounter card |
| **Formula 4: H_pt** | Expected hits per player per turn (consumes slot structure) |
| **Formula 5: BUFF threshold** | HP_eff = HP_L + BUFF_value (unchanged, now per-slot) |

### Key Design Clarifications (from session)

- **Players have no HP.** HP_alpha and HP_beta were always Angel-only. With the
  WAS-based redesign, both coefficients are fully deprecated.
- **Layer cards carry "Target Party Size".** Each slot layer card has a 2+/3+/4+
  field. The encounter card specifies how many Angels appear — it does NOT specify
  layers per slot.

---

## ADR Impact

**0 ADRs exist at this time. ADR impact analysis: N/A.**

No architecture decision records have been written for this project yet. When ADRs
are authored in future sprints, they should be checked against the 3-slot Angel
System architecture described above.

---

## Files Updated

### 1. `design/gdd/integrity-system.md`
**Impact**: Needs Review → Updated in place

Key changes applied:
- Status updated to "Needs Revision"
- Rule 14: Updated to describe 3-slot structure (Head/Body/Weapon)
- Rule 15: Changed from "no layer targeting" → "declare target slot (Head/Body/Weapon)"
- Rule 16: Changed from "single Damage Tracker Die per Angel" → "Slot Tracker Die per slot column"
- Rule 17: Changed from "single check per Angel" → "single check per slot"
- Rule 20 (Penetrating): Changed from "strips all current layers simultaneously" → "within-slot cascade only"
- Rule 22 (Angel Cleared): Changed from "all layers stripped" → "all three slots Inert"
- Angel States table: Replaced with 3-tier table (slot states / layer states / board state)
- Formula 4 (HP Bounds): Marked SUPERSEDED — players have no HP; WAS-based calibration supersedes
- Tuning Knobs 2 (HP_alpha) and 3 (HP_beta): Marked SUPERSEDED
- Dependencies table: Updated Angel System entry (WAS-based calibration)
- Edge case HP_L=0: Updated floor from HP_min → 4

### 2. `design/gdd/character-system.md`
**Impact**: Needs Review → Updated in place

Key changes applied:
- Status updated to "Needs Revision"
- Rule 11 (Penetrating): Rewritten — within-slot cascade only; no cross-slot penetration
- "Penetrating multi-layer strip" edge case: Updated to within-slot cascade
- CS-AC-29: Updated — "hits all current layers" → "cascades within the declared slot only"
- CS-AC-30: Resolved (was DEFERRED) — within-slot cascade confirmed; cascade order outermost-first; HP_eff recalculated per layer; no cross-slot
- S_encounter table: Added OQ-05 flag for Pilgrim recalculation at HP ∈ [4, 14]
- Formula 3 (Penetrating multiplier): Added note that n_L recalculation needed at new HP range

### 3. `design/gdd/win-loss-conditions.md`
**Impact**: Needs Review → Updated in place

Key changes applied:
- Status updated to "Needs Revision"
- Rule 1 (Victory): "all layers stripped" → "all three slots (Head, Body, Weapon) Inert"
- Interactions table: Updated "Angel cleared" definition; added Angel System row
- AC-01: GIVEN condition updated to 3-slot Inert definition
- Bidirectional note: Updated to reference Angel System as owner of Cleared definition

### 4. `design/gdd/combat-system.md`
**Impact**: Needs Review → Updated in place

Key changes applied:
- Status updated to "Needs Revision"
- Encounter Entry step 2: "stack of layer cards" → "three slot columns (Head/Body/Weapon)"
- Encounter Entry step 4: "Damage Tracker Die beside each Angel stack" → "Slot Tracker Die below each slot column"
- Encounter End Win condition: "all layers stripped" → "all three slots Inert"
- Step 5b: Added slot declaration — must declare both Angel AND slot before advancing any tracker
- Step 5c: Rewritten for per-slot strip evaluation; Penetrating note (within-slot only); Cleared = all 3 slots Inert
- EPC variable table: HP_L range updated from [HP_min, HP_max] = [10, 19] → [4, 14]
- Cross-Referenced Formulas table: HP_min/HP_max bounds row marked SUPERSEDED

### 5. `design/gdd/events-system.md`
**Impact**: Needs Review → Updated in place

Key changes applied:
- Status updated to "Needs Revision"
- Type A resolution procedure step 1: Added 2026-05-27 note clarifying encounter card governs Angel count only; layer count per slot lives on layer cards via "Target Party Size" field
- Rule 4.1: Added parenthetical clarifying layer-per-slot scaling is resolved at Angel Zone setup, not declared on encounter card
- OQ-3: Updated from DEFERRED → PARTIALLY RESOLVED — encounter card/layer card separation documented

### 6. `design/registry/entities.yaml`
**Impact**: Needs Review → Updated in place

Key changes applied:
- `last_updated` updated to 2026-05-27
- `HP_alpha`: status → `deprecated`; notes updated with deprecation rationale
- `HP_beta`: status → `deprecated`; notes updated with deprecation rationale
- `T_angel`: scope note updated; HP_L range noted as [4, 14]; `design/gdd/angel-system.md` added to `referenced_by`
- `D_player_base`: `design/gdd/angel-system.md` added to `referenced_by`
- `D_agg`: `design/gdd/angel-system.md` added to `referenced_by` (uncommented)
- `P_angel`: `design/gdd/angel-system.md` added to `referenced_by` (uncommented)
- `E_angel`: `design/gdd/angel-system.md` added to `referenced_by` (uncommented)
- `EPC`: `design/gdd/angel-system.md` and `design/gdd/events-system.md` added to `referenced_by` (uncommented)

---

## Open Flags Left Behind

These items were identified during propagation and require follow-up. They are
documented in the affected GDDs but not yet resolved:

| Flag | GDD | Description |
|---|---|---|
| OQ-05 | character-system.md | Recalculate Pilgrim S_encounter at HP ∈ [4, 14] — verify stays within [54, 74] |
| Formula 3 flag | character-system.md | n_L recalculation for Penetrating multiplier at new HP range |
| EPC example | combat-system.md | Example still uses old HP values (12/16/18 from Integrity System) — update when Integrity System example is revised |
| angel-system.md status | angel-system.md | Still marked "Needs Revision — Structural redesign in progress"; requires final design-review pass before marking Designed |

---

## Traceability Summary

| System | Before | After |
|---|---|---|
| HP calibration ownership | Integrity System (HP_alpha/HP_beta bounds) | Angel System (WAS-based, HP ∈ [4, 14]) |
| Angel Cleared definition | "All layers of stack stripped" | "All 3 slots Inert" |
| Penetrating scope | Cross-layer (all current layers) | Within-slot cascade only |
| Slot targeting at damage declaration | Not required | Required (declare Angel + slot) |
| Party-size scaling location | Encounter card (card variants) | Layer cards (Target Party Size field) |
| HP_alpha / HP_beta | Active constants | Deprecated |

---

*Report written automatically by `/propagate-design-change` — 2026-05-27*
