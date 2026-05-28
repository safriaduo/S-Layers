# Events System

> **Status**: Needs Revision — propagation update from angel-system.md redesign (2026-05-27)
> **Author**: Federico Gallucci + Claude Code agents
> **Last Updated**: 2026-05-27
> **Implements Pillar**: Fast and Focused (primary); The Build is Never Finished (secondary); Cooperative Ownership (supporting)

## Overview

The Events System is the structural spine of a S-Layers session. It owns three things: the composition and construction of the Event deck, the rules for drawing and resolving each card, and the taxonomy of event types that governs what each card does. It does not own combat (Combat System), item acquisition procedures (Item System), or session termination (Win/Loss Conditions) — it feeds those systems the triggers they need, in the order the deck determines, until the deck is exhausted.

The Event deck is a countdown. At session start, it is a stack of shuffled cards with God sealed at the bottom. The party does not know exactly how many cards remain — only that every card drawn brings God one step closer. Boon events deliver items and recovery. Bad events deal damage or impose conditions without an enemy to fight. Encounter events trigger combat. The deck never grows: each card resolved is a card removed. When God appears, the run is decided.

Every draw is a miniature event horizon: the party knows a card is coming but not which category. They may have already survived three boons and be deep in a bad run — or they may be at peak power having drawn well. The distance to God is visible (cards remaining) and felt differently from one draw to the next. This is the system's player-facing effect: the dungeon as a structural experience of *shortening*, not expanding, toward the thing the party came to face.

At MVP: 15 cards total — God sealed at the bottom, with 3 treasure events, at least 3 bad events, and the remainder a mix of encounters. The exact composition is a tuning knob.

## Player Fantasy

The deck is the dungeon, and the dungeon is shortening.

Every card drawn is one fewer card between the party and God. Players can see this — the remaining stack is on the table, a physical column of paper that visibly diminishes. Early in the session, the stack is tall and the distance feels abstract. By the middle, it does not. By the end, players count the remaining cards silently before anyone draws, because the column makes the gravity of God visible before any rule is consulted.

The fantasy is not the thrill of the unknown. The events themselves are categorically familiar — the party has seen a boon, a bad event, an encounter before. They know the *space* of possibilities. What they do not know is which door this draw opens. The fantasy lives in the breath between the flip and the reveal: the moment the prisoners brace for a verdict. Encounters are confrontations. Bad events are punishments resumed. **Boons are not rewards.** They are the absence of a worse card — relief that this door, this time, was the merciful one.

**Design test for boons:** If a player flips a Boon and the table cheers, the Events System has failed. If they exhale quietly — if someone says "okay" — it has worked. A boon is not a gift from the dungeon. It is the dungeon not delivering the thing they were bracing for. The unclenching is the intended response.

**Design test for bad events:** If a player flips a bad event and complains about randomness, something is broken in the framing or the card text. The bad event was always in the deck. It is a page in a sentence that was written before the prisoners arrived. The correct response is *recognition*, not surprise: "there it is."

**Design test for the diminishing stack:** If, at some point in the session, a player glances at the remaining cards and adjusts a decision they were otherwise settled on — the deck has exerted ambient pressure without a rule. That is the Events System working at its highest level.

## Detailed Design

### Core Rules

---

#### 1. Deck Construction

The Event deck is assembled once at session start. It does not change during play — no cards are added, no shuffles occur mid-session.

1. **God is sealed last.** Set the God card aside before shuffling. It is never part of the shuffle.
2. **Shuffle all remaining cards.** All non-God Event cards are shuffled together as a single pool, then placed face-down as a stack.
3. **Place God at the bottom.** The shuffled stack is placed in the Event Zone. God is slid beneath it, face-down, as the final step.
4. **No seeding rule.** The deck is not pre-sorted to guarantee any card type early. Early-game pacing is governed by deck composition ratios (see Tuning Knobs), not structural sequencing. A seeding rule adds setup overhead; composition achieves the same pacing goal without a table-time cost.
5. **The stack is always visible and countable.** Players may count remaining cards at any time. This is intentional: the pressure of a shortening column is a designed mechanic, not a hidden state.

---

#### 2. Draw Procedure

2.1. **The First Player draws.** The player currently holding the First Player token is the draw authority. They draw the top card of the Event deck.

2.2. **Draw timing is automatic.** A draw happens immediately after the previous Event card is fully resolved. No player decision or explicit action is required — the dungeon does not wait. "Fully resolved" is defined per card type in Rule 7.

2.3. **One card per draw.** Exactly one card is drawn per resolution. There are no multi-draw events, no optional skips, and no mechanism to delay a draw once the previous card is fully resolved.

2.4. **Read aloud before resolution.** The drawing player reads the card aloud in full before any effect resolves. All players must hear the complete card text before the resolution procedure for that card type begins. Reading the card aloud begins resolution immediately — clarification questions are answered during resolution, not before. There is no deliberation step between reading and resolving.

---

#### 3. Event Type Taxonomy

Every Event card belongs to exactly one of four types. The type is printed on the card (top margin). The type determines the structural resolution procedure. Card content (the specific effect text) is distinct from card type (the structural procedure) — this GDD specifies the procedure; card content is authored per-card.

---

**Type A — Encounter**

A card that places one or more Angels in the Angel Zone, triggering a combat encounter.

*Structural resolution procedure:*
1. Read the card aloud. The card specifies: Angel identity, party-size scaling (1–2 players: N Angel(s); 3–4 players: M Angel(s)), and any encounter-specific special rules. *(2026-05-27: The encounter card governs only how many Angels appear per party size. It does NOT specify layers per slot — layer count scaling is owned by the layer cards themselves: each slot layer card carries a "Target Party Size" field [e.g., 2+, 3+, 4+] indicating the minimum party size for that layer to be included. Encounter card format does not change with the Angel System 3-slot redesign.)*
2. Transfer control to the Combat System (see Rule 4).
3. The Combat System runs the full 6-phase sequence until the encounter ends.
4. Control returns to the Events System after the Combat System's cleanup phase completes (see Rule 4).
5. The encounter card is moved to the Event discard, face-up.

---

**Type B — Treasure**

A card that delivers one item to the party from the item pool.

*Structural resolution procedure:*
1. Read the card aloud.
2. Draw one item from the shared item pool (face-down; unordered).
3. Execute Item System Distribution Procedure (Rule 3) in full — First Player assigns recipient; receiving player chooses zone.
4. If the item pool is exhausted, the card still resolves fully. No item is distributed. The card is not skipped or re-drawn.
5. The card is moved to the Event discard, face-up.
6. The First Player token rotates clockwise (Rule 7).

---

**Type C — Bad Event**

A card that applies a negative effect to one or more players without an Angel encounter.

*Structural resolution procedure:*
1. Read the card aloud.
2. Apply the stated effect(s) in the order listed on the card. Effects execute through the appropriate upstream system (Integrity System for slot damage).
3. **Effect scope constraint**: Bad events may only apply effects from the following set — direct slot damage (reducing Integrity on one or more equipment slots), round-long constraints (conditions that expire at end of the next Phase 6 or at end of the current encounter, whichever comes first). Bad events may NOT destroy a layer outright or impose permanent state. Outright layer destruction is reserved for combat-sequence Integrity loss.
4. The card is moved to the Event discard, face-up.
5. **Win/Loss Check C fires**: After all effects on the bad event card have been applied, Win/Loss Conditions evaluates "player board destroyed." If any board is inert, the game transitions to Lost immediately. *(Win/Loss Conditions GDD must be updated to add Check C — bad-event board destruction check — to its timing model.)*
6. If no Loss: the First Player token rotates clockwise (Rule 7).

---

**Type D — Boon**

A card that delivers a positive non-item effect to one or more players.

*Structural resolution procedure:*
1. Read the card aloud.
2. Apply the stated positive effect. Effects may be drawn from the following set: Integrity restoration (heal one or more equipment slots, per card text), one-round combat modifier (applies to the next combat encounter only; expires when that encounter ends), or deck peek (look at the top N cards of the Event deck without drawing them; return them in any order, per card text — **God, if revealed within the peeked window, must be returned to the bottom of the peeked window; God's position within the remaining stack cannot be changed by any deck manipulation effect**).
3. The card is moved to the Event discard, face-up.
4. The First Player token rotates clockwise (Rule 7).

**Design note:** Boon effects are relief, not reward. Card text and art must reflect the mercy framing — these cards deliver absence-of-worse-outcome, not gifts. See Player Fantasy.

---

#### 4. Encounter Resolution Handoff

When a Type A (Encounter) card is drawn, the following sequence defines which system has control at each moment:

4.1. **Events System owns the draw and read.** The First Player draws the card, reads it aloud, confirms party-size scaling (number of Angels active at this party size) and any special encounter rules. This is the Events System's complete responsibility before handoff. *(Layer-per-slot scaling is not declared here — it is resolved during Angel Zone setup by reading the "Target Party Size" field on each slot layer card.)*

4.2. **Events System is suspended at handoff.** After the card is read and acknowledged by the table, control transfers to the Combat System. The Events System is frozen: no draws, no token rotations, no card resolutions occur while the Combat System is active.

4.3. **Combat System runs to completion.** The Combat System executes its 6-phase encounter sequence until the encounter ends (Angel cleared or player board destroyed).

4.4. **Control returns after Combat System cleanup.** When the encounter ends:
- **Encounter ends in Loss**: Win/Loss Conditions fires. Session ends. Events System does not resume.
- **Encounter ends in Win**: Combat System executes its cleanup phase (post-combat item award drawn and distributed per Item System; inert slot states persist; stripped card piles persist; First Player token rotates per Combat System Rule 4 cleanup step). After cleanup is fully complete, the Events System resumes. The encounter card is moved to the Event discard.

4.5. **Next draw is immediate.** After control returns and the encounter card is discarded, the new First Player (post-rotation) draws the next card automatically.

4.6. **No events fire during combat.** An event card cannot interrupt a combat encounter. The Events System is frozen for the duration. There is no "draw mid-combat" mechanic.

---

#### 5. God Card

5.1. **God is structurally a Type A (Encounter) card.** The Events System treats God identically to any other encounter card. Draw procedure, read-aloud, and Combat System handoff are identical. No Events System rule distinguishes the God draw from a standard encounter draw.

5.2. **Win/Loss Conditions owns the semantic distinction.** When God is cleared (all layers stripped), the Combat System fires "Angel cleared" → Win/Loss Conditions transitions to Won. The Events System does not need to know God is special.

5.3. **The God encounter terminates the session.** The Events System does not resume after the God encounter. There is no next draw, no token rotation, no cleanup return. The session ends inside the Combat System's resolution — Won or Lost.

5.4. **No First Player token rotation after God.** The token rotation rule (Rule 7) applies to cards that return control to the Events System. The God card never returns control. No rotation occurs.

5.5. **God's "last card" position is the structural announcement.** When God is the remaining card, the stack above it is empty. Players can see this before the draw. The Events System makes no special announcement — the visible empty column is the cue.

---

#### 6. Deck Exhaustion

6.1. **Standard play cannot exhaust the deck without encountering God.** God is the final card. The stack above God empties only by drawing card by card. When the last non-God card is fully resolved, the next draw is God.

6.2. **If God is absent from the deck (setup error):** The draw produces nothing. The deck is empty. The session cannot reach a Win state. Acknowledge the error; the session result is void. This condition cannot occur in correctly assembled play (Rule 1.1).

6.3. **No refresh mechanism exists.** The deck does not loop, shuffle back, or regenerate. Each card is drawn once and discarded.

---

#### 7. First Player Token Rotation

7.1. **The token rotates after each Event card is fully resolved.** This includes all four card types.

7.2. **"Fully resolved" by card type:**

| Card Type | Fully Resolved When... |
|---|---|
| **Type A — Encounter** | Combat System cleanup complete, including post-combat item award and token rotation. Token rotation is embedded in Combat System cleanup (per Combat System Rule 4) — it fires there, not as a second rotation here. |
| **Type B — Treasure** | Item Distribution Procedure complete (including receiving player's zone choice), regardless of whether an item was available from the pool. |
| **Type C — Bad Event** | All effects applied + Win/Loss Check C evaluated + game did not end. |
| **Type D — Boon** | All positive effects applied. |

7.3. **Exception — God card:** No token rotation. The session ends before the Events System regains control (Rule 5.3).

---

### States and Transitions

| State | Description |
|---|---|
| **Idle** | No card active. Deck is assembled and ready. Awaiting next draw (immediately after previous card is fully resolved). |
| **Drawing** | First Player has flipped the top card. Type being identified. |
| **Resolving — Treasure** | A Type B card is live. Item pool draw and Distribution Procedure in progress. |
| **Resolving — Bad Event** | A Type C card is live. Negative effects are applying to boards. |
| **Resolving — Boon** | A Type D card is live. Positive effects are applying. |
| **Resolving — Encounter** | A Type A card is live. Combat System has control. Events System is frozen. |
| **Session End** | Won or Lost. Terminal. No further Events System transitions. |

| Transition | From | To | Trigger |
|---|---|---|---|
| Deck assembled, session starts | — | Idle | Setup complete |
| Automatic draw fires | Idle | Drawing | Previous card fully resolved |
| Card is Treasure | Drawing | Resolving — Treasure | Card type identified |
| Card is Bad Event | Drawing | Resolving — Bad Event | Card type identified |
| Card is Boon | Drawing | Resolving — Boon | Card type identified |
| Card is Encounter (non-God) | Drawing | Resolving — Encounter | Card type identified |
| Card is God | Drawing | Resolving — Encounter | Card type identified (God is Type A) |
| Treasure / Bad Event / Boon fully resolved, no Loss | Resolving — any non-Encounter | Idle | Resolution procedure complete; token rotated |
| Encounter fully resolved (Win) | Resolving — Encounter | Idle | Combat System cleanup complete; token rotated by Combat System |
| Bad event destroys a board (Check C — Loss) | Resolving — Bad Event | Session End | "Player board destroyed" at Check C → Lost |
| Encounter ends in Loss | Resolving — Encounter | Session End | Win/Loss Conditions → Lost |
| God cleared | Resolving — Encounter (God) | Session End | Win/Loss Conditions → Won |
| God encounter ends in Loss | Resolving — Encounter (God) | Session End | Win/Loss Conditions → Lost |

---

### Interactions with Other Systems

| System | Interface | Direction | Notes |
|---|---|---|---|
| **Combat System** | Encounter card triggers combat; Combat System cleanup returns control to Events System; First Player token rotates in Combat System cleanup | Bidirectional | Events System provides the encounter trigger. Combat System runs the encounter and handles token rotation in cleanup. Events System resumes after cleanup completes. |
| **Item System** | Treasure Events invoke Distribution Procedure (Channel A — Type B cards) | Events → Item System | Events System draws from the item pool and passes to Item System Rule 3. The post-combat award (Channel B) is triggered by Combat System cleanup, not by the Events System directly. |
| **Win/Loss Conditions** | "Angel cleared" (God) → Won; "player board destroyed" (combat or Check C) → Lost | Upstream → Win/Loss | Events System does not own termination. Win/Loss Conditions GDD requires a minor update: add Check C (fires immediately after a bad event's effects resolve, evaluating "player board destroyed"). |
| **Integrity System** | Bad event slot damage routes through Integrity System | Events → Integrity | Events System does not apply slot damage directly. It reads the card text and invokes Integrity System routing rules. |
| **Equipment Degradation** | Bad event damage that reduces a slot's Integrity to zero triggers a layer reveal (indirectly via Integrity System) | Indirect | Equipment Degradation fires as a downstream consequence of Integrity routing, not as a direct Events System invocation. |

## Formulas

The Events System is a sequencing skeleton. It owns no mathematical formulas — all calculations relevant to card design, item output balance, and encounter pacing live in the systems those cards invoke. Card designers must run the appropriate formula gates when authoring each card type:

**Card Design Formula Gates**

| Card Type | Formula to Run | Owned By | Purpose |
|---|---|---|---|
| **Type A — Encounter** | EPC (Encounter Pacing Check) | Combat System | Validates that encounter turn count falls within the 5–8 min window: `PASS if T_encounter_actual ∈ [⌈300/T_round⌉, ⌊480/T_round⌋]` |
| **Type B — Treasure** | S_combined advisory check | Item System | Validates that the item being awarded does not push a player's combined output above the advisory gate (S_combined ≤ 85) |
| **Type C — Bad Event** | None at MVP | — | Bad event effects are calibrated through playtesting. No formula gate exists at MVP. Effects must stay within the scope constraint (Rule 3, Type C, step 3). |
| **Type D — Boon** | None at MVP | — | Non-item positive effects calibrated through playtesting. Magnitude documented as a Tuning Knob. |

**Cross-references to registered formulas**: EPC — `design/registry/entities.yaml` (source: combat-system.md); S_combined — `design/registry/entities.yaml` (source: item-system.md).

## Edge Cases

*Format: **If [condition]**: [exact outcome]. [rationale if non-obvious]*

---

**Item Pool**

**If the item pool is exhausted when a Treasure card (Type B) is drawn**: the card resolves fully. No item is drawn or distributed. The Distribution Procedure is not invoked. The card moves to the Event discard. The First Player token rotates. The session continues.

---

**Bad Event + Equipment Degradation**

**If a bad event's slot damage reduces a slot's Integrity to zero, triggering a layer reveal**: the Events System remains in control throughout. Equipment Degradation's reveal procedure executes synchronously as part of the bad event's effect application (Rule 3, Type C, step 2). When the reveal is complete, the Events System continues: scope check, discard, Check C, and token rotation. Equipment Degradation owns the strip and reveal; the Events System owns the sequence before and after.

**If a bad event's slot damage strips a layer that has an item attached**: Equipment Degradation Rule 1, step 3 fires — the item departs with the stripped card immediately, before Check C evaluates. Check C evaluates board state after all effects (including item departure) are fully settled.

**If a bad event strips a layer whose attached item has a departure trigger that deals offensive damage (e.g., "deal 3 damage to the Angel"), and no Angel is present in the Angel Zone during Bad Event resolution**: the departure trigger fires — the item departs normally and the trigger is consumed — but the offensive damage fizzles. No damage is applied, no target is resolved, and no integrity is lost. Bad Event resolution then continues as normal (scope check, discard, Check C, token rotation). *(Rationale: departure triggers are authored for combat context where a valid Angel target always exists. Bad Events are a non-combat context with no Angel in scope. The trigger fires because departure is always unconditional; the damage effect has no legal target and is discarded.)*

---

**Bad Event + Board Destruction**

**If a bad event's effects destroy a player's board (all 3 slots inert)**: Check C fires immediately after ALL effects on the card have been applied — including any layer reveal and item departure triggered by the final slot damage. Check C does not fire mid-resolution. The transition to Lost is immediate when Check C fires. Token rotation (Rule 3, Type C, step 6) does not occur — the session ended before that step.

---

**Boon — Deck Peek and God**

**If a deck peek (Type D Boon) reveals God within the peeked window**: God must be returned to the bottom position of the peeked window, regardless of how the other peeked cards are reordered. God's position within the remaining stack cannot be changed by any effect. *(Rationale: "sealed at bottom" is a structural invariant of the session, not just a construction rule. Any mechanic that could move God up the stack would allow the party to trivially delay or advance the final encounter, breaking the countdown structure.)*

**If a deck peek window is smaller than the remaining non-God card count (e.g., peek 3 cards, 7 non-God cards remain above God)**: God is not visible in the peeked window. The player sees only the top N non-God cards and may reorder them freely.

**If a deck peek window exactly equals the remaining non-God card count (e.g., peek 5 cards, 5 non-God cards remain, God is card 6)**: God is at the bottom of the peeked window (position N+1, which is below the peek). The player sees 5 non-God cards and may reorder them freely. God is not touched.

**If a deck peek window is larger than the remaining non-God card count (e.g., peek 5 cards, 3 non-God cards remain, God is the 4th card)**: the player sees 3 non-God cards and God at position 4. They may reorder the 3 non-God cards freely among positions 1–3. God must remain at position 4 (its original position as bottom of stack). No reordering may place God above any non-God card.

---

**Boon — One-Round Combat Modifier and God**

**If a one-round combat modifier from a Boon card is active when God is drawn**: the modifier applies to the God encounter. "The next combat encounter only" — God is the next combat encounter if no other combat intervened. The modifier expires when the God encounter ends (consumed by God's encounter regardless of session outcome).

---

**First Player Token — Recipient is the First Player**

**If the First Player assigns distribution to themselves (First Player is the item recipient)**: this is valid. No rule prevents the First Player from naming themselves as recipient. They perform both roles sequentially: announce themselves as recipient, then choose which zone on their own board receives the item.

---

**Deck State — Exactly One Card Remaining (God)**

**If God is the only remaining card in the deck**: the stack above God is empty. The visible column is gone; only the face-down God card sits in the Event Zone. The Events System makes no special announcement. The next automatic draw draws God. The empty column is the only cue — no rule text accompanies it.

---

**Consecutive Non-Combat Cards**

**If a bad event and a Boon are drawn and resolved consecutively with no combat between them**: the First Player token rotates twice — once after the bad event (Rule 3, Type C, step 6), once after the Boon (Rule 3, Type D, step 4). Each card type includes its own rotation step. Two resolved cards = two rotations.

**If a bad event imposes a round-long constraint and the next card drawn is also a bad event (no combat between them)**: both constraints are simultaneously active. A round-long constraint expires at end of the next Phase 6 or end of the current encounter — whichever comes first. If no combat encounter has begun, neither constraint has found its expiration trigger. Both wait for the first subsequent combat Phase 6 to expire. If no combat encounter occurs before the session ends (unusual), both constraints remain active through the God encounter and expire at the end of its Phase 6.

---

**Win/Loss Collision — Non-God Encounter**

**If a Type A Encounter (non-God) ends with "Angel cleared" and "player board destroyed" firing simultaneously at Check B**: the session transitions to **Lost**. Win/Loss Conditions Rule 6 (Victory prevails) applies exclusively to God. Clearing a non-God Angel is an encounter win, not a session win. The simultaneous-termination override is not available for non-God Angels. *(Reference: Win/Loss Conditions Edge Cases — "If 'Angel cleared' fires for a non-God Angel in the same phase a board is destroyed... 'Player board destroyed' fires and produces Lost.")*

## Dependencies

**Upstream dependencies** (systems this one depends on):

| System | Dependency Type | What Events System Consumes |
|---|---|---|
| **Combat System** | Hard | Encounter card resolution handoff — the Combat System runs when a Type A card triggers combat. Combat System cleanup returns control to Events System after the encounter. First Player token rotation during cleanup is owned by Combat System (Rule 4). |
| **Item System** | Hard | Treasure event Distribution Procedure (Channel A) — when a Type B card resolves, Events System invokes Item System Rule 3. Post-combat item award (Channel B) is triggered by Combat System cleanup, not the Events System directly. |
| **Win/Loss Conditions** | Hard | "Player board destroyed" signal → session Loss (Check A, B, and C). "Angel cleared" for God → session Won. Events System does not own termination — it provides triggers and invokes checks. *(Events System GDD added Check C to Win/Loss Conditions timing model, 2026-05-26.)* |
| **Integrity System** | Soft | Bad event slot damage routes through Integrity System. Events System does not apply slot damage directly — it invokes Integrity System routing for effects declared on Type C cards. |
| **Equipment Degradation** | Soft (indirect) | If bad event damage strips a layer (Integrity to zero), Equipment Degradation's reveal procedure fires. Events System does not invoke Equipment Degradation directly — it fires as a downstream consequence of Integrity routing. |

**Downstream dependents** (systems that depend on this one):

| System | Dependency Type | What They Need |
|---|---|---|
| **Game Setup** (Alpha) | Hard | Events System deck construction rules define how the deck is assembled at session start. Game Setup GDD must specify deck assembly as a pre-session step using Events System Rule 1 as the specification. |
| **Communication Protocol** (Alpha) | Soft | The read-aloud rule (Rule 2.4 — drawing player reads the card aloud before resolution) is an Events System rule. Communication Protocol GDD should reference this as an existing constraint on player communication during card resolution. |

**Bidirectional consistency notes:**
- **Combat System** Section F already references Events System as a downstream dependent — its Interactions table lists "Encounter entry trigger (encounter Event card → Combat begins); encounter-end signal (Combat complete → Event card resolved → Events System advances)." ✓ Consistent.
- **Item System** Rule 2 Channel A (Treasure events draw from item pool) and Channel B (post-combat award triggered by Combat System cleanup) are consistent with Events System Rule 3 Type B procedure. ✓ Consistent.
- **Win/Loss Conditions** lists Events System as a downstream dependent and requires God to be the last card (Win/Loss GDD Dependencies section). ✓ Consistent. Win/Loss GDD updated this session to add Check C to its timing model.
- When **Game Setup** GDD is authored, it must reference Events System Rule 1 (deck construction) as the deck assembly specification.
- When **Communication Protocol** GDD is authored, it must not contradict Events System Rule 2.4 (read aloud before resolution is mandatory).

## Tuning Knobs

**1. Deck size (total card count)**
- **Adjusts**: Session length and dungeon depth
- **Current value**: 15 cards at MVP (14 shuffled + 1 God sealed at bottom)
- **Safe range**: 10–20 cards
- **If too low (< 10)**: Session ends before the build matures. Players don't reach critical item+equipment synergies. The countdown collapses into urgency without earned stakes.
- **If too high (> 20)**: Session length exceeds 90 min target. God draw becomes a formality rather than a climax. The stack stops exerting ambient pressure because it never feels close.
- **Interaction**: Tightly coupled with Knob 3 (encounter count) and T_round coefficients (Combat System Tuning Knob 1).

**2. Treasure event count (Type B cards)**
- **Adjusts**: Item acquisition rate; build density by session end
- **Current value**: 3 at MVP
- **Safe range**: 2–5
- **If too low (≤ 1)**: Players rely almost entirely on post-combat awards. Low-encounter draws leave the party under-equipped. Reduces Pillar 2 (The Build is Never Finished) engagement.
- **If too high (≥ 6 in a 15-card deck)**: Too many items enter play before encounters. S_combined advisory gate (≤ 85) becomes harder to maintain.
- **Notes**: Post-combat item awards (guaranteed per encounter, owned by Combat System) are not tuned here. Total expected item acquisition: `N_treasure + N_encounters_completed`.

**3. Encounter event count (Type A cards, excluding God)**
- **Adjusts**: Combat frequency, total post-combat item awards, degradation opportunity rate
- **Current value**: ~8 encounters at MVP (15 cards − 3 Treasure − 3 Bad Events − 1 God = 8 remainder)
- **Safe range**: 5–11 encounters in a 15-card deck
- **If too low (< 5)**: Too few encounters for the build to evolve through degradation. Pillar 2 underserved.
- **If too high (> 11)**: Bad events and Boons crowded out. Session becomes a combat sequence with no pacing relief.
- **Interaction**: Every encounter card must pass EPC (Combat System) before production.

**4. Bad Event count (Type C cards)**
- **Adjusts**: Out-of-combat punishment frequency; dread pressure between encounters
- **Current value**: ≥ 3 at MVP (minimum constraint)
- **Safe range**: 2–5 in a 15-card deck
- **If too low (< 2)**: Non-encounter draws feel safe. The "sentence was already written" Player Fantasy collapses. Boons become the expected outcome rather than the relief outcome.
- **If too high (≥ 6)**: Players may be damaged into inert states before their builds activate. Risk of snowball failure that violates Agency Over Dice.

**5. Boon count (Type D cards)**
- **Adjusts**: Relief frequency; pacing valve between punishment and recovery
- **Current value**: 0 at MVP (no Boon cards; positive non-item events deferred to post-MVP)
- **Safe range**: 0–3
- **If added (1–3)**: Introduce Integrity restoration or combat modifier cards. Playtest to confirm that Boon draws produce the "quiet exhale" table behavior from the Player Fantasy, not celebration.
- **Interaction**: Each Boon card displaces a Bad Event or Encounter card from the 14 shuffled slots.

**6. Deck peek magnitude (Type D Boon — deck peek variant)**
- **Adjusts**: Information advantage granted per peek
- **Current value**: Not set (no Boon cards at MVP; activates when Type D cards are authored)
- **Safe range**: 1–5 cards revealed per peek
- **If too high (> 5 cards)**: High risk of revealing God and triggering the God-anchoring rule repeatedly. Reduces dread of the unknown; the shortening column becomes navigable information rather than a felt pressure.

**7. Round-long constraint duration (Type C Bad Event effects)**
- **Adjusts**: How long bad event penalties persist into combat
- **Current value**: Expires at end of next Phase 6 OR end of the current encounter — whichever comes first
- **Alternative (shorter)**: Expires at end of current Phase 4 only — minimal combat impact
- **Alternative (longer)**: Persists for 2 full encounters — session-defining punishment
- **If shortened**: Bad events become irrelevant to combat. Effect resolves in non-combat context and expires before the next encounter begins.
- **If lengthened to 2 encounters**: Bad events become high-stakes. Risk of snowball failure if two bad events are drawn consecutively before first combat.

## Visual/Audio Requirements

*Not applicable — S-Layers is a physical board game. Visual requirements for the Events System (card type iconography, God card bilateral symmetry, deck position visibility) are specified in the Art Bible (when authored). The Player Fantasy defines the emotional register: boons as quiet exhale, bad events as verdicts recognized, the shortening column as ambient dread. These are communicated through physical component design.*

*Key visual requirement for production (to be formalized in the Art Bible): the Event Zone must allow the remaining deck stack to be clearly visible to all players at 60–100 cm. The physical column's decreasing height is the primary pacing mechanic — if players cannot see the stack from their seat, the shortening-dungeon fantasy is broken.*

## UI Requirements

*Not applicable — S-Layers is a physical board game. The Events System has no digital UI. Table information design (card type icons readable at distance, God card distinctiveness) is owned by the Art Bible and component spec documents.*

## Acceptance Criteria

### Deck Construction

**AC-01** Given a new session is being set up, when the Event deck is assembled, then the God card occupies the bottom position, all other Event cards are randomly shuffled above it, and God was never included in the shuffle. *Verified by: physical inspection after setup — God face-down at bottom; 14 cards in random order above.*

**AC-02** Given the Event deck is assembled, when a player counts the stack, then the full card count is visible and accurate — no cards are hidden or face-up. *Verified by: tester counts the stack and confirms total matches expected deck size (15 at MVP).*

### Draw Procedure

**AC-03** Given the previous Event card has been fully resolved, when the resolution procedure completes, then the next draw fires automatically with no player decision required. *Verified by: observe that no player asks "should we draw?"; draw occurs as the prior card reaches its final step.*

**AC-04** Given the First Player token is held by Player 2, when a draw fires, then Player 2 draws the top card and reads its full text aloud before any effect resolves. *Verified by: confirm no die is rolled, no item is drawn, and no effect applies until Player 2 finishes reading aloud.*

### Type B — Treasure

**AC-05** Given a Type B (Treasure) card is drawn, when the card is read aloud, then one item is drawn from the item pool, First Player assigns the recipient, the recipient places it in a chosen zone, and the card moves to the Event discard face-up. *Verified by: confirm these steps occur in order; confirm no more than one item is drawn.*

**AC-06** Given a Type B (Treasure) card is drawn and the item pool is empty, when the resolution procedure runs, then the card resolves fully — no item is distributed, no re-draw occurs, the card moves to the Event discard, and the First Player token rotates. *Verified by: confirm no item enters play; confirm token rotates exactly once.*

**AC-07** Given a Type B (Treasure) card has completed the full Distribution Procedure including the recipient's zone choice, when that last step completes, then the First Player token rotates clockwise exactly once. *Verified by: confirm token advances to the next player clockwise; confirm no second rotation occurs.*

### Type C — Bad Event

**AC-08** Given a Type C (Bad Event) card is drawn, when the card is read aloud, then its stated effects are applied through the Integrity System in the order listed, the card moves to the Event discard face-up, Win/Loss Check C fires, and — if no Loss — the First Player token rotates. *Verified by: confirm each step occurs in sequence; confirm no step is skipped if no board was destroyed.*

**AC-09** Given a Type C (Bad Event) card applies effects that reduce a slot's Integrity to zero, when Integrity routing fires, then Equipment Degradation's layer reveal executes synchronously as part of effect application — and the Events System continues to Check C only after the reveal fully completes. *Verified by: confirm the layer reveal finishes (including any item departure) before Check C evaluates.*

**AC-10** Given a Type C (Bad Event) card's effects destroy a player's board (all 3 slots inert), when Check C fires, then the game transitions to Lost immediately and the First Player token does not rotate. *Verified by: confirm token position unchanged from before the Bad Event draw; confirm no draw fires after the Loss transition.*

**AC-11** Given a Type C (Bad Event) card is fully resolved and no board was destroyed, when Check C runs, then the First Player token rotates clockwise exactly once and the next draw fires automatically. *Verified by: confirm token advances one position; confirm next draw fires from the new First Player.*

### Type D — Boon

**AC-12** Given a Type D (Boon) card is drawn, when the card is read aloud, then the stated positive effect is applied, the card moves to the Event discard face-up, and the First Player token rotates clockwise. *Verified by: confirm effect resolves, discard occurs, and token advances — in that order.*

**AC-13** Given a Type D (Boon) card grants a deck peek of N cards, when the peek resolves, then the drawing player looks at the top N cards without drawing them, may reorder those cards freely, and returns them to the top of the deck. *Verified by: tester observes peek window; confirms no card is removed; confirms stack card count is unchanged after the peek.*

**AC-14** Given a deck peek reveals God within the peeked window, when the player reorders the peeked cards, then God must be returned to the bottom position of the peeked window — God's position in the remaining stack cannot change. *Verified by: after the peek, tester verifies God is at the same absolute position in the deck as before the peek.*

**AC-15** Given a deck peek window is smaller than the remaining non-God card count, when the peek resolves, then God is not visible in the peeked window and God's position is not affected. *Verified by: confirm the player has not seen or moved God; confirm stack card count is unchanged.*

### Type A — Encounter Handoff

**AC-16** Given a Type A (Encounter) card is drawn, when the card is read aloud and acknowledged, then control transfers to the Combat System — no Events System draws, token rotations, or card resolutions occur for the duration of combat. *Verified by: confirm no Event card is drawn and no Events System token rotation occurs between handoff and combat cleanup completion.*

**AC-17** Given the Combat System has completed an encounter (non-God, win outcome) and its cleanup phase is fully done, when cleanup completes, then the Events System resumes, the encounter card moves to the Event discard, and the new First Player draws the next card automatically. *Verified by: confirm token rotation occurred exactly once during Combat System cleanup; confirm no second rotation fires from the Events System.*

**AC-18** Given the Combat System performs First Player token rotation in its cleanup phase, when the Events System resumes, then the Events System does not rotate the token again — exactly one rotation occurs per encounter resolution. *Verified by: track token position through the full encounter cycle; confirm one advancement total.*

### God Card

**AC-19** Given the God card is the next card to be drawn, when the automatic draw fires, then the Events System treats God identically to any Type A Encounter — draws, reads aloud, and hands control to the Combat System with no special Events System rule. *Verified by: confirm draw and read-aloud steps are identical to a standard Encounter card; confirm no special Events System text fires.*

**AC-20** Given the God encounter ends (Won or Lost), when the session terminates, then the First Player token does not rotate and no next draw fires — the Events System does not resume after the God card. *Verified by: confirm token position unchanged from its state at the start of the God draw; confirm no card is drawn after session end.*

### Combat Modifiers and God

**AC-21** Given a Type D (Boon) granted a one-round combat modifier and no combat encounter occurred between the Boon and the God draw, when the God encounter begins, then the modifier is active and applies to the God encounter — it expires when the God encounter ends. *Verified by: confirm modifier is in effect at Phase 1 of the God encounter; confirm it is consumed by end of that encounter.*

**AC-22** Given a one-round combat modifier from a Boon is active, when a non-God encounter occurs before God is drawn, then the modifier applies to that encounter and expires at its end — it does not carry forward to God or any subsequent encounter. *Verified by: confirm modifier is consumed after the first encounter's Phase 6; confirm it is absent from subsequent tracking.*

### Deck State

**AC-23** Given God is the only remaining card in the deck, when the Events System runs its automatic draw, then God is drawn with no special Events System announcement — the empty column is the only visible cue. *Verified by: confirm no rule text or verbal announcement fires from the Events System; confirm the draw proceeds as a standard Type A Encounter.*

**AC-24** Given a correctly assembled deck (God at bottom) and all non-God cards are drawn and resolved, when the last non-God card is fully resolved, then the next automatic draw draws God — there is no empty-deck state before God appears. *Verified by: run through all 14 non-God cards; confirm the 15th draw is God.*

**AC-28** Given the Event deck was assembled without God (setup error), when all Event cards are drawn and resolved, then the deck is empty, no God draw occurs, and the Won state is unreachable — the session result is void. *Verified by: confirm after the last card resolves, no draw fires and no Won transition is available.*

### Win/Loss Collisions

**AC-25** Given a Type A Encounter (non-God) ends with "Angel cleared" and "player board destroyed" simultaneously at Check B, when Win/Loss Check B resolves, then the game transitions to Lost — Victory prevails applies exclusively to God. *Verified by: confirm session state is Lost; confirm no Victory prevails rule is invoked for a non-God Angel.*

### Token Rotation — Consecutive Non-Combat Cards

**AC-26** Given a Bad Event is fully resolved (no Loss) followed immediately by a Boon fully resolved, when both cards complete, then the First Player token has rotated exactly twice — once after the Bad Event and once after the Boon. *Verified by: track token position through both resolutions; confirm two discrete rotations, each after its respective card's final step.*

**AC-27** Given a Bad Event imposes a round-long constraint and the next Event card is also a Bad Event imposing a round-long constraint (no combat between them), when both cards are fully resolved, then both constraints are simultaneously active and each awaits the first subsequent combat Phase 6 to expire. *Verified by: confirm both constraints are tracked entering the next combat encounter; confirm both expire at end of that encounter's Phase 6.*

### First Player Self-Assignment

**AC-29** Given the First Player is also the chosen recipient for a Treasure item, when they announce themselves as recipient, then this is valid — they perform both roles sequentially and choose which zone on their own board receives the item. *Verified by: confirm no rule challenge is raised; confirm item placement proceeds on the First Player's board; confirm token rotates normally after.*

### Combat Freeze

**AC-30** Given the Events System is suspended during a combat encounter, when any player suggests drawing a card or applying an Event effect mid-combat, then no Event card draw or Event System token rotation occurs — the Events System remains frozen until Combat System cleanup fully completes. *Verified by: confirm that during Phases 1–6 plus Combat cleanup, zero Event cards are drawn and zero Events System token rotations occur.*

## Open Questions

**OQ-1 [DEFERRED → playtest]** MVP deck composition balance: the initial split of ~8 Encounters / 3 Treasure / 3 Bad Events / 0 Boons is provisional. Validate pacing against the 60–90 min session target with stopwatch playtests.

**OQ-2 [DEFERRED → post-MVP]** Boon card design: Type D Boon cards are absent from the MVP deck. When authored, each Boon must be tested against the Player Fantasy design test ("quiet exhale, not cheer"). This is a card authoring constraint, not a GDD rule.

**OQ-3 [PARTIALLY RESOLVED 2026-05-27]** Multi-Angel encounter scaling per card: the encounter card specifies how many Angels appear (N at 1–2 players; M at 3–4 players) — this is the only party-size information on the encounter card. Layer-per-slot scaling is governed by the "Target Party Size" field on each slot layer card (Angel System redesign, 2026-05-27). The Angel System GDD must define the valid range for N and M Angels per tier and verify multi-Angel encounter pacing against EPC.

**OQ-4 [RESOLVED → 2026-05-26]** Win/Loss collision (God + board destroyed simultaneously): Victory prevails. Win/Loss GDD Rule 6 is the authority. Combat System GDD AC-18 and AC-22 updated to match.

**OQ-5 [RESOLVED → 2026-05-26]** Win/Loss Check C (bad event board destruction): added to Win/Loss Conditions GDD Rule 3 as Check C. Fires immediately after all bad event effects resolve.
