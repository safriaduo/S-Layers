# Win/Loss Conditions

> **Status**: Designed (pending /design-review in fresh session)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-24
> **Implements Pillar**: Fast and Focused (primary); Cooperative Ownership (supporting)
> **Creative Director Review (CD-GDD-ALIGN)**: APPROVED 2026-05-24

## Overview

The Win/Loss Conditions system defines the two game termination events in S-Layers: the party **wins** when God is defeated; the party **loses** when any player's board is destroyed. These two triggers are the contract that makes every other rule in the game meaningful — encounters have stakes because the Angels can end runs, and the Events deck has weight because the God card is always its final card. Both termination events are received from upstream systems (the Integrity System raises "Angel cleared" and "player board destroyed" signals) and resolved here. This GDD owns what happens when those signals fire, not how they are produced. Despite being infrastructure, both ending moments are felt directly at the table: a win is the whole party's construction coming to its conclusion; a loss is a routing decision from three turns ago finally landing. The rules are simple; the weight is earned.

## Player Fantasy

God is the only card in the dungeon with no cracks — bilateral symmetry, complete, unbroken. The party has been staring at it, piece by piece, for the last hour. **The win moment is the first crack.** When the final layer strips, the unbroken thing finally breaks symmetry, and the table sees it happen together. This is what players will describe to someone else afterward: not their strategy, not their build — the image of the thing that wasn't supposed to crack, cracking.

**The loss moment is the half-beat.** A prisoner's board goes inert. The remaining players keep rolling for a few seconds before they understand. The run ends in that delay — the brief interval where the table realizes one of the witnesses has stopped witnessing. The loss belongs to all of them, because the realization happens to all of them at once.

**Design test**: If the win moment produces a table photo or a sentence beginning "and then we actually—", it has worked. If the loss moment produces silence before anyone speaks, it has worked. Win/Loss Conditions does not own the hour of play. It owns the last two seconds of it.

## Detailed Design

### Core Rules

**1. Victory condition**
The party wins when the "Angel cleared" trigger fires for the God card — meaning all of God's layers have been stripped. God is always the final card in the Events deck. When God is cleared, the game state transitions to **Won** immediately. No further actions, effects, or phases resolve after the transition fires, except as specified in rule 6 (simultaneous termination).

**2. Defeat condition**
The party loses when the "player board destroyed" trigger fires for any player — meaning all three equipment slots on one player's board are inert. When that trigger fires, the game state transitions to **Lost** immediately, except as specified in rule 6 (simultaneous termination) and rule 4 (timing window).

**3. Timing of the Win/Loss check**
The Win/Loss check fires at two points in the turn flow:
- **Check A — end of Angel Resolution Phase**: Evaluates "player board destroyed" only. If any board was destroyed by hit routing this phase, the transition fires before the Effect Resolution Phase begins.
- **Check B — end of Effect Resolution Phase**: Evaluates both triggers — first "Angel cleared" (God stripped?), then "player board destroyed." Evaluated after all effects in the phase have fully resolved.

**4. Remaining effects fire before Check B**
A player whose board is destroyed during Effect Resolution Phase has all remaining placed die effects fire for the current phase before Check B runs. This rule is defined by the Integrity System (rule 13) and honored here.

**5. "Angel cleared" can only fire at Check B**
God cannot be cleared during the Angel Resolution Phase. Damage to Angel layers resolves exclusively during Effect Resolution Phase. The "Angel cleared" trigger never fires at Check A.

**6. Simultaneous termination**
If Check B finds both "Angel cleared" and "player board destroyed" active in the same Effect Resolution Phase, **Victory prevails**. The game transitions to **Won**. The defeat trigger is discarded. *(Rationale: the party dealt the killing blow within the same phase window; the dungeon ends and its remaining threat does not outlast it.)*

**7. Termination is final**
Once Won or Lost, no further rules, effects, card text, or player actions apply. The session ends.

**8. Victory requires only that God be cleared**
The party wins if any number of boards remain active when God is cleared, including exactly one surviving board. Survival of individual boards has no bearing on the victory condition.

### States and Transitions

| State | Description |
|---|---|
| **Playing** | Default state. Game is in progress. All rules and effects apply. |
| **Won** | God has been cleared. Session ends. |
| **Lost** | A player board has been destroyed and Victory has not prevailed. Session ends. |

| Transition | From | To | Trigger | Timing |
|---|---|---|---|---|
| God cleared | Playing | Won | "Angel cleared" fires for God card | Check B (end of Effect Resolution Phase) |
| Board destroyed, no simultaneous win | Playing | Lost | "Player board destroyed" fires; God was NOT cleared in same phase | Check A or Check B |
| Board destroyed, simultaneous win | Playing | Won | Both triggers fire in same phase — Victory prevails | Check B — Won overrides Lost |
| — | Won | — | Terminal | No exit |
| — | Lost | — | Terminal | No exit |

### Interactions with Other Systems

| System | Interface | Notes |
|---|---|---|
| **Integrity System** | Receives: "player board destroyed" (all 3 slots inert) and "Angel cleared" (all Angel layers stripped) | Integrity System defines the conditions; this GDD defines what happens when they fire. |
| **Events System** | God card is always the last Event card in the deck. When God is cleared, Win/Loss receives the "Angel cleared" signal and resolves the win. Events System owns deck structure; Win/Loss owns the termination interpretation of that last card's clearing. |
| **Angel System** | God is an Angel encounter using standard Integrity System rules. No God-specific win mechanics are needed — the "Angel cleared" signal is identical for God and other Angels. The semantic distinction is this GDD's responsibility: God cleared = session win; other Angel cleared = encounter win, continue play. |

## Formulas

*No formulas. Win/Loss Conditions is a binary state machine: it receives "Angel cleared" and "player board destroyed" triggers and transitions game state. All mathematical modeling of session length, encounter duration, and God encounter design belongs to the Angel System GDD (for God's layer count and HP values) and the Integrity System GDD (for T_angel, T_board, D_agg, and HP bounds). Those formulas are registered in `design/registry/entities.yaml`. This GDD does not define or own any of them.*

## Edge Cases

*Format: **If [condition]**: [exact outcome]. [rationale if non-obvious]*

---

**Check A vs. Check B Timing**

- **If a board is destroyed during the Angel Resolution Phase (Check A fires)**: the game transitions to Lost immediately. The Effect Resolution Phase does not begin. No restoration effects, remaining dice effects, or any other effects from that phase fire. Rule 7 (termination is final) applies. This is the key asymmetry: Check A termination is instant; Check B termination allows the phase to fully resolve first.

- **If a board is destroyed at Check A and God has not yet been cleared**: there is no simultaneous termination path. Simultaneous termination (rule 6) requires both triggers to fire within the same Effect Resolution Phase. A Check A destruction produces Lost unconditionally.

- **If two or more player boards are simultaneously destroyed in the Angel Resolution Phase**: the game transitions to Lost. Simultaneous termination (rule 6) is not applicable — "Angel cleared" cannot fire at Check A (rule 5).

**Restoration Reversal — the Most Critical Edge Case**

- **If a board reaches full-inert state during Effect Resolution Phase but a restoration effect fires later in the same phase (returning at least one layer to any slot on that board)**: Check B evaluates board state *after all effects in the phase have fully resolved*. If the board has at least one active slot when Check B runs, "player board destroyed" does NOT fire. A mid-phase destruction that is healed within the same phase is not a loss.

- **If a board is destroyed during Effect Resolution Phase and no restoration fires before Check B**: the board is inert when Check B evaluates. "Player board destroyed" fires normally. No grace period exists.

- **If a board is destroyed at Check A and a player argues a restoration item should fire to prevent the loss**: Check A precedes Effect Resolution Phase. No effects fire between Check A and the Effect Resolution Phase. Check A termination is irreversible.

**Dead Player's Die Can Win the Session**

- **If a player whose board is destroyed during Effect Resolution Phase has a placed die that deals damage to God, and that damage strips God's final layer**: the die effect fires (rule 4). Check B then finds both "Angel cleared" and "player board destroyed" active in the same phase. Victory prevails (rule 6). A dead player's final die can win the session.

**Non-God Angel Cleared — No Session Win**

- **If "Angel cleared" fires for a non-God Angel in the same phase a board is destroyed**: rule 6 does not apply. "Angel cleared" on an encounter Angel produces an encounter win (handled by the Events System), not a session win. "Player board destroyed" fires and produces Lost. The simultaneous-termination override is exclusive to the God card.

**Partial Party**

- **If one of four boards is destroyed and three boards remain active**: the game transitions to Lost immediately (rule 2 is unconditional — "any player" board destroyed). There is no "continue with reduced party" mode.

**Events System / God Card Consistency**

- **If the Events deck runs out before God is drawn (setup error — God missing from deck)**: Win/Loss Conditions cannot produce a Won state. The game is unwinnable until the deck is corrected. This GDD flags it as a dependency requirement: the Events System must guarantee God is always present as the final card.

- **If God is cleared before the last card position (accidental early shuffle)**: the session ends in Won immediately when God is cleared. Win/Loss Conditions acts only on the "Angel cleared" signal for God; it does not check deck position. The "God is always last" invariant is the Events System's responsibility to enforce during setup.

## Dependencies

**Upstream dependencies**: None. Win/Loss Conditions is a Foundation-layer system. It does not depend on any other system in S-Layers to function.

**Downstream dependents** (systems that depend on this one):

| System | Dependency Type | What They Consume |
|---|---|---|
| **Events System** | Hard | The session win trigger — "God cleared = party wins." The Events System must guarantee God is the last card in the deck precisely because Win/Loss Conditions connects that card's clearing to session end. Also surfaces: the "Events deck runs out before God appears" failure mode is the Events System's responsibility to prevent. |
| **Integrity System** | Soft (bidirectional reference) | The Integrity System sends triggers to this GDD; this GDD documents the receiving end. The Integrity System does not depend on Win/Loss Conditions to function — it fires triggers regardless of who receives them. |

**Bidirectional consistency notes**:
- The **Integrity System** GDD already lists Win/Loss Conditions as a downstream dependent receiving "player board destroyed" and "Angel cleared." ✓ Consistent.
- When the **Events System** GDD is authored, it must list Win/Loss Conditions as a dependency and reference the "God card is always last" requirement.
- When the **Angel System** GDD is authored, it must note that the semantic distinction between "encounter win" (non-God Angel cleared) and "session win" (God cleared) is owned by this GDD, not the Angel System.

## Tuning Knobs

**1. Simultaneous Termination Resolution (Victory Prevails)**
- **Adjusts**: Whether the party wins or loses when God is cleared and a board is destroyed in the same Effect Resolution Phase
- **Current value**: Victory prevails
- **Alternative**: Defeat prevails — no override; a destroyed board is always a loss
- **If changed to "defeat prevails"**: The edge case where a dead player's die kills God becomes a loss, not a win. Creates a degenerate failure mode where the party kills God and still loses. **Not recommended.**
- **Status**: Effectively locked. Changing requires re-evaluating the Player Fantasy and the "dead player's final die" edge case.

**2. Defeat Scope — "Any Player" vs. "All Players"**
- **Adjusts**: Whether the party loses when one board is destroyed, or only when all boards are destroyed
- **Current value**: Any player board destroyed = session loss (strict cooperative)
- **Alternative**: "All boards destroyed" mode — party continues until all boards fall or God is cleared
- **If changed to "all boards"**: Routing pressure collapses (one destroyed board no longer ends the run). Session length increases. Cooperative Ownership dynamic fundamentally changes — individual boards become less critical.
- **Status**: Locked by the Anti-Pillar ("cooperative dynamic is load-bearing, not optional"). Changing requires Creative Director review and pillar re-evaluation.

**3. God-as-Last-Card (Events System Invariant)**
- **What it is**: The requirement that God is always the final card in the Events deck
- **This GDD's position**: Not a tuning knob here — a hard dependency on the Events System. If the Events deck shuffles, God must be seeded last, not included in the shuffle.
- **If changed (God can appear anywhere)**: The win condition still fires when God is cleared regardless of deck position. Encounter pacing changes fundamentally — the party could face God in the first few turns.
- **Owned by**: Events System GDD.

## Visual/Audio Requirements

*Not applicable — Foundation layer system. Physical board game; no digital audio or visual rendering. The Player Fantasy section defines the emotional register for win/loss moments; physical component and card design (God card bilateral symmetry, uncracked state) is governed by the Visual Identity anchor in the game concept and the Art Bible (when authored).*

## UI Requirements

*Not applicable — Foundation layer system. Win/Loss Conditions produces no player-facing display of its own. The end-of-game state is communicated through physical component state (God card face-down discard pile, inert player boards) and table recognition.*

## Acceptance Criteria

*Format: GIVEN [state], WHEN [action/trigger], THEN [specific measurable outcome].*

---

**AC-01** — GIVEN the party has stripped all of God's layers, WHEN the "Angel cleared" trigger fires for God at the end of Effect Resolution Phase (Check B), THEN the game state transitions to Won and no further actions resolve.

**AC-02** — GIVEN any player's board has all 3 slots inert and no "Angel cleared" fired for God in the same phase, WHEN Check B runs, THEN the game state transitions to Lost and no further actions resolve.

**AC-03** — GIVEN a player board is destroyed by hit routing in the Angel Resolution Phase, WHEN Check A runs at end of that phase, THEN the game transitions to Lost immediately — Effect Resolution Phase does not begin.

**AC-04** — GIVEN a player board is destroyed during Effect Resolution Phase (by a card effect) and that player has a die still to resolve on another slot, WHEN Check B has not yet run, THEN that die effect fires before Check B evaluates.

**AC-05** — GIVEN all of God's layers are stripped in Effect Resolution Phase AND a player's board is also destroyed in the same phase, WHEN Check B runs, THEN the game transitions to Won (not Lost).

**AC-06** — GIVEN 1 of 4 player boards is destroyed and 3 boards remain active, WHEN Check A or Check B fires, THEN the game transitions to Lost — no "continue with reduced party" resolution exists.

**AC-07** — GIVEN a non-God Angel is cleared in the same phase a board is destroyed, WHEN Check B runs, THEN the game transitions to Lost, not Won — "Angel cleared" for a non-God Angel does not trigger session win.

**AC-08** — GIVEN a player board reaches full-inert state mid-Effect Resolution Phase but a restoration effect fires in the same phase returning at least 1 layer to any slot on that board, WHEN Check B runs, THEN "player board destroyed" does NOT fire — the board has an active slot.

**AC-09** — GIVEN the party kills God at Check B with a die placed by a player whose board was destroyed earlier in the same phase, WHEN Check B evaluates, THEN the game transitions to Won.

**AC-10** — GIVEN only 1 of 4 player boards is alive when God is cleared, WHEN Check B fires, THEN the game transitions to Won — survival of all boards is not required for victory.

**AC-11** — GIVEN two or more player boards are destroyed simultaneously in the Angel Resolution Phase, WHEN Check A fires, THEN the game transitions to Lost — simultaneous termination (Victory prevails) does not apply at Check A.

**AC-12** — GIVEN the Angel Resolution Phase ends without God being cleared, WHEN Check A runs, THEN the game state cannot transition to Won — "Angel cleared" is not evaluated at Check A.

**AC-13** — GIVEN God is not present in the Events deck (setup error), WHEN all Event cards are resolved, THEN the Won state is unreachable — this criterion validates the dependency: the Events System must guarantee God is always present as the final card.

## Open Questions

*None identified at time of authoring. The simultaneous termination resolution (Victory prevails) and the two-check-point timing model were resolved during the design session. Add during playtesting if ambiguous table situations arise.*
