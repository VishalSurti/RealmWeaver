# RealmWeaver — V1 Game Rules Specification

**Document Version:** 0.1
**Status:** Draft — M2.1 In Progress
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification & Rules-Engine Boundary
**Last Reviewed:** 12 August 2026

---

# 1. Purpose

This document defines the approved gameplay rules for RealmWeaver V1.

It serves as the authoritative specification for the deterministic game mechanics that will eventually be implemented by the RealmWeaver rules engine.

This document is developed incrementally during M2.1.

Rules marked **Approved** represent accepted V1 design decisions.

Rules marked **Pending** have not yet been finalised and must not be treated as approved implementation requirements.

---

# 2. Rules Design Philosophy

## Status: APPROVED

RealmWeaver V1 will use a **D&D-inspired fantasy tabletop ruleset closely aligned with familiar 5e mechanics**, while allowing simplification and adaptation for AI-driven solo gameplay.

RealmWeaver will not attempt to provide complete 5e compatibility in V1.

The objective is to preserve the familiar tabletop RPG experience while avoiding mechanics that create disproportionate complexity without significantly improving the V1 player experience.

The core design principle is:

> **AI tells the story. Rules decide what happens.**

The AI Dungeon Master may interpret player intent, create narrative content, suggest mechanical actions, and narrate results.

The deterministic RealmWeaver game engine remains authoritative over objective game state and mechanical outcomes.

---

# 3. Ruleset Strategy

## Status: APPROVED

RealmWeaver follows a selective rules implementation strategy.

### V1 principles

* Preserve familiar tabletop mechanics where practical.
* Simplify mechanics that create unnecessary V1 complexity.
* Maintain meaningful player agency.
* Maintain meaningful dice outcomes.
* Keep objective mechanics deterministic.
* Prevent the AI from overriding mechanical truth.
* Design systems so additional 5e-style mechanics can be introduced in future versions.
* Avoid architecture that permanently restricts RealmWeaver to the V1 rules subset.

### Future Expansion

Later versions may progressively introduce additional 5e-style rules and greater mechanical depth.

V1 simplification must therefore not unnecessarily prevent future rules-engine expansion.

---

# 4. Rules Content Strategy

## Status: PRELIMINARY APPROVAL — Architecture Details Pending

RealmWeaver should avoid manually hard-coding large amounts of static rules content when appropriately licensed structured sources are available.

Examples include:

* Monsters
* Equipment
* Weapons
* Armour
* Spells
* Conditions
* Skills
* Class information

The intended architecture is:

**Structured rules source → RealmWeaver importer → validation/transformation → RealmWeaver rules data → game engine**

External rules APIs should not become mandatory runtime dependencies for normal gameplay.

RealmWeaver should maintain its own validated internal representation of supported rules content.

The final sourcing, licensing, import and storage architecture will be defined later in M2.

---

# 5. Character Core

## Status: APPROVED

---

## 5.1 Ability Scores

RealmWeaver V1 uses six primary ability scores:

* Strength (STR)
* Dexterity (DEX)
* Constitution (CON)
* Intelligence (INT)
* Wisdom (WIS)
* Charisma (CHA)

Ability modifiers are calculated deterministically by the rules engine.

The AI must not calculate or override ability modifiers.

---

## 5.2 Ability Score Generation

V1 supports two character-generation methods:

### Standard Array

The player assigns predefined balanced ability values.

### Rolled Stats

The player generates ability scores using dice.

Rolled-stat generation must support both:

* RealmWeaver system-generated dice
* Player-entered physical dice results

Point Buy is not required for V1.

---

## 5.3 Skills

RealmWeaver V1 uses the familiar skill structure.

### Strength

* Athletics

### Dexterity

* Acrobatics
* Sleight of Hand
* Stealth

### Intelligence

* Arcana
* History
* Investigation
* Nature
* Religion

### Wisdom

* Animal Handling
* Insight
* Medicine
* Perception
* Survival

### Charisma

* Deception
* Intimidation
* Performance
* Persuasion

Skills have default associated abilities.

However, RealmWeaver may permit alternative ability + skill combinations when the player's approach reasonably justifies them.

Example:

A player demonstrating physical strength to frighten someone may potentially make a:

**Strength (Intimidation)** check

rather than the default Charisma-based Intimidation check.

The AI may propose an alternative combination, but the rules system must validate the resulting check.

---

# 6. Proficiency

## Status: APPROVED

Skill checks use:

**d20 + Ability Modifier + Proficiency Bonus (when proficient)**

Non-proficient checks use:

**d20 + Ability Modifier**

Proficiency bonus progresses according to character level.

The rules/data model should be capable of supporting character levels 1–20.

V1 does not promise complete playable class content for all 20 levels.

---

## 6.1 Expertise

Expertise is supported in V1.

Expertise doubles the applicable proficiency bonus.

Conceptually:

**d20 + Ability Modifier + (2 × Proficiency Bonus)**

The rules engine performs this calculation.

---

# 7. Character Level Support

## Status: APPROVED

RealmWeaver uses three levels of support planning:

### V1 Minimum

Fully playable content for:

**Levels 1–5**

### V1 Stretch Goal

Following successful testing of levels 1–5:

**Levels 1–10**

may be implemented before V1 release if complexity, testing and schedule permit.

Levels 1–10 are a stretch target and must not delay the core V1 release unnecessarily.

### Architecture Target

The underlying data model and rules architecture should be designed to support:

**Levels 1–20**

Future versions may progressively expand playable content toward full levels 1–20 support.

---

# 8. Hit Points

## Status: APPROVED

Characters maintain:

* Maximum HP
* Current HP
* Temporary HP

Starting HP is primarily derived from class and Constitution.

The rules engine is authoritative over HP values.

The AI may narrate injuries and recovery but must not independently modify HP.

Detailed healing, unconsciousness and death mechanics are defined later in the Combat section.

---

# 9. Armour Class

## Status: APPROVED

RealmWeaver uses Armour Class (AC) as the primary defensive target for attack resolution.

Unarmoured characters generally derive AC from a base value plus relevant Dexterity modifiers.

Armour, shields, abilities and other supported effects may modify AC.

The rules engine calculates and stores AC.

The AI must not independently determine whether an attack hits or misses.

---

# 10. Speed

## Status: APPROVED

Character speed is stored as part of character state.

V1 does not use precise grid-based battlefield movement.

Speed will instead interact with RealmWeaver's abstract combat-positioning system.

---

# 11. Passive Skills

## Status: APPROVED

V1 supports passive mechanical awareness, at minimum:

**Passive Perception**

Passive checks may be used when revealing the existence of a roll would itself reveal hidden information.

Example uses include:

* Hidden creatures
* Traps
* Suspicious environmental details
* Secret activity

Passive checks may occur without notifying the player.

The player should not be informed that a hidden passive comparison occurred when doing so would reveal information their character does not possess.

Additional passive skills may be considered later.

---

# 12. Ability Checks

## Status: APPROVED

A dice roll should occur only when:

1. The outcome is meaningfully uncertain; and
2. Success and failure can produce meaningful consequences.

Actions should broadly resolve into one of three categories:

### Automatic Success

The action is reasonably achievable and uncertainty does not justify a roll.

### Mechanical Check

The outcome is uncertain and meaningful enough to require mechanical resolution.

### Impossible

The proposed action cannot reasonably succeed under the current circumstances.

Rolling a die does not make an impossible action possible.

---

# 13. AI Role in Ability Checks

## Status: APPROVED

The AI DM interprets the player's natural-language intent.

The AI may propose:

* Whether a check is appropriate
* Relevant ability
* Relevant skill
* Difficulty category
* Narrative reasoning

The AI does not calculate the final mechanical result.

The RealmWeaver rules engine remains authoritative over:

* Dice results
* Modifiers
* Proficiency
* Final totals
* DC comparison
* Success/failure
* Mechanical state changes

---

# 14. Difficulty Classes

## Status: APPROVED

RealmWeaver uses controlled difficulty bands rather than allowing unrestricted AI-generated DC values.

The approved baseline is:

| Difficulty        | Base DC |
| ----------------- | ------: |
| Very Easy         |       5 |
| Easy              |      10 |
| Moderate          |      15 |
| Hard              |      20 |
| Very Hard         |      25 |
| Nearly Impossible |      30 |

Most ordinary gameplay checks should generally fall between DC 10 and DC 20.

The AI should normally propose a difficulty category.

The deterministic system maps that category to the corresponding DC.

Environmental and narrative circumstances may influence which difficulty category is appropriate.

V1 should prefer selecting an appropriate difficulty band over maintaining numerous small mathematical environmental modifiers.

---

# 15. Saving Throws

## Status: APPROVED

Saving throws represent attempts to resist or avoid effects acting upon a character.

Saving throws use:

**d20 + Ability Modifier + Proficiency Bonus (if proficient in that saving throw)**

Characters may possess proficiency in specific saving throws.

The rules engine determines the final result.

The AI may determine when a narrative event reasonably requires a saving throw but cannot override the mechanical outcome.

---

# 16. Contested Checks

## Status: APPROVED

V1 supports contested checks.

Examples include:

* Stealth vs Perception
* Athletics vs Athletics
* Athletics vs Acrobatics

The rules engine resolves both mechanical values and determines the winner.

The AI narrates the resulting outcome.

---

# 17. Degrees of Success and Failure

## Status: APPROVED FOR V1

V1 uses **narrative degrees of success and failure only**.

The rules engine should calculate the margin between the final result and the target DC.

Example:

**Roll Total:** 24
**DC:** 15
**Result:** Success
**Margin:** +9

The AI may use the margin to vary the strength or style of its narration.

However, the margin does not automatically create additional mechanical rewards or penalties in V1.

### Future Expansion

Future RealmWeaver versions may introduce mechanical degrees of success/failure.

Possible future effects include:

* Additional information
* Faster completion
* Improved rewards
* Tool damage
* Complications
* Additional consequences

The V1 rules engine should retain success-margin information so this feature can be introduced later without redesigning the core check-resolution system.

---

# 18. Dice System

## Status: APPROVED

V1 supports:

* d4
* d6
* d8
* d10
* d12
* d20
* d100

The dice engine must support multiple-dice expressions such as:

* 2d6
* 3d8
* 4d6

Modifiers may be applied to dice expressions where required.

---

# 19. Dice Modes

## Status: APPROVED

RealmWeaver supports two player dice modes:

### Automatic Dice

RealmWeaver generates the dice result.

### Manual Dice

RealmWeaver instructs the player which dice to roll.

The player rolls physical dice and enters the result into the system.

Players may change their preferred dice mode during a campaign.

Manual dice results must be validated against the possible range of the requested dice.

RealmWeaver does not attempt to detect cheating in manual dice mode.

---

# 20. Randomness

## Status: APPROVED

System-generated dice should use genuine unbiased pseudo-random selection appropriate for gameplay.

RealmWeaver must not manipulate dice results merely to avoid repeated values.

Repeated identical results remain possible.

The previously considered rule preventing the same number from appearing more than three consecutive times is explicitly rejected because it would bias the dice system.

---

# 21. Advantage and Disadvantage

## Status: APPROVED

Advantage:

**Roll 2d20 and use the higher result.**

Disadvantage:

**Roll 2d20 and use the lower result.**

If advantage and disadvantage apply simultaneously, they cancel and the character makes a normal roll.

Multiple sources of advantage do not stack in V1.

Multiple sources of disadvantage do not stack in V1.

---

# 22. Natural 20 and Natural 1

## Status: PARTIALLY APPROVED

For ordinary ability checks:

* Natural 20 does not automatically make an impossible action successful.
* Natural 1 does not automatically cause every otherwise achievable check to fail.

Special Natural 20/Natural 1 behaviour for attack rolls will be defined in the Combat rules.

---

# 23. Hidden Rolls

## Status: APPROVED

RealmWeaver supports hidden DM/system rolls.

Possible uses include:

* NPC deception
* Enemy stealth
* Hidden perception-related resolution
* Random world events
* Secret campaign mechanics

Hidden rolls must not reveal information the player's character does not possess.

---

# 24. Dice History

## Status: APPROVED

RealmWeaver should maintain a player-accessible history of player-visible dice rolls.

Dice history may display information such as:

* Check type
* Raw roll
* Modifier
* Final total
* Mechanical outcome

Hidden DM/system rolls must not appear in player-visible dice history.

---

# 25. Inspiration

## Status: APPROVED

A character may hold a maximum of:

**1 Inspiration**

Inspiration may be awarded for appropriate gameplay behaviour such as:

* Strong roleplay
* Creative problem solving
* Meaningful character decisions
* Acting consistently with important character traits
* Significant narrative moments

The AI may recommend awarding Inspiration.

The deterministic system validates and applies the award.

The AI must not directly modify Inspiration state.

---

## 25.1 Using Inspiration

Inspiration may be spent to gain advantage on an eligible d20 roll.

Inspiration must be declared **before the roll occurs**.

It cannot be applied retroactively after seeing the result.

When Inspiration is validly spent:

**Inspiration: 1 → 0**

The rules engine performs this state change.

The player may not spend Inspiration they do not possess.

---

# 26. Combat

## Status: IN PROGRESS

Combat rules are currently being designed.

Sections 26.1–26.3 are approved.

Later combat sections remain pending.

---

# 26.1 Combat Start and Initiative

## Status: APPROVED

Structured combat begins when hostile interaction requires turn-based mechanical resolution.

The AI may identify that combat should begin and request combat initiation.

The deterministic game engine:

1. Creates the encounter.
2. Loads participants.
3. Rolls or requests initiative.
4. Establishes turn order.
5. Creates the combat round.
6. Tracks subsequent turns and rounds.

The AI does not determine turn order arbitrarily.

Initiative uses:

**d20 + Dexterity Modifier**

### Initiative Ties

For V1:

* Player vs enemy tie → player acts first.
* Enemy vs enemy tie → higher Dexterity acts first.
* If still tied → system randomly resolves the ordering.

The engine remains authoritative over initiative state.

---

# 26.2 Turn Structure and Action Economy

## Status: APPROVED

A normal combat turn may provide:

* Movement
* 1 Action
* 1 Bonus Action
* 1 Reaction per round
* Reasonable free interaction

The engine tracks whether relevant resources remain available.

A Bonus Action may only be used when an ability, spell, item or other supported mechanic explicitly permits a Bonus Action.

Players cannot freely convert the Bonus Action into an additional normal Action.

### Free Interaction

Minor interactions may generally occur without consuming the main Action when mechanically appropriate.

Examples may include:

* Drawing a weapon
* Dropping an item
* Brief speech
* Opening an uncomplicated unlocked door

More significant interactions may require an Action depending on circumstances.

---

# 26.3 Reactions

## Status: APPROVED — LIMITED V1 SUPPORT

The V1 combat-state model supports Reactions.

A creature normally has one Reaction available according to the supported reaction rules.

The Reaction system is intentionally extensible.

V1 does not need to implement every possible reaction-based mechanic.

Basic opportunity attacks are included in the intended V1 rules.

Additional reactions may be introduced in future versions.

---

# 26.4 Combat Positioning

## Status: APPROVED

RealmWeaver V1 does not use precise grid-based combat positioning or battle maps.

Instead, combat uses abstract distance bands.

Approved baseline:

| Distance Band | Approximate Meaning    |
| ------------- | ---------------------- |
| Engaged       | Approximately 0–5 ft   |
| Near          | Approximately 5–30 ft  |
| Far           | Approximately 30–60 ft |
| Distant       | Approximately 60+ ft   |

The combat engine tracks relevant positional relationships.

This prevents the AI from independently contradicting mechanical distance.

For example, a melee attack normally requires an appropriate close-distance relationship.

Character speed remains mechanically relevant when transitioning between distance bands.

---

## 26.5 Future Battle Maps

## Status: APPROVED FUTURE DIRECTION

Distance bands are a V1 simplification.

If a future RealmWeaver version introduces battle maps, the abstract distance-band system may be replaced or extended by precise spatial positioning.

The V1 architecture should therefore avoid unnecessary coupling that would make a future spatial system prohibitively difficult to introduce.

---

# 26.6 Core Movement Actions

## Status: APPROVED

V1 intends to support:

* Normal Movement
* Dash
* Disengage
* Dodge
* Help
* Hide

These actions must be mechanically validated by the rules engine.

---

# 26.7 Opportunity Attacks

## Status: APPROVED — BASIC IMPLEMENTATION

Basic opportunity attacks are supported in V1.

Leaving an Engaged relationship without an appropriate mechanic such as Disengage may allow an eligible opponent to use its Reaction for an opportunity attack.

The rules engine determines whether the opportunity attack is mechanically available.

---

# 26.8 Complex Natural-Language Combat Actions

## Status: APPROVED

Players may describe complex actions naturally.

Example:

> "I run past the goblin, jump over the table, grab the wizard's staff and attack him."

The AI must not automatically narrate all components as successful.

The intended resolution pipeline is:

**Player Intent → AI Interpretation → Structured Proposed Actions → Rules Validation → Mechanical Resolution → AI Narration**

A complex player statement may therefore be decomposed into:

* Movement
* Potential reaction triggers
* Ability checks
* Object interactions
* Actions
* Attacks

The rules engine determines which components are mechanically legal and their outcomes.

---

# 27. Combat Rules Still Pending

The following combat areas have not yet been approved:

### 4D — Attacks, Armour Class and Damage

Pending.

### 4E — Critical Hits

Pending.

### 4F — HP, Healing, Unconsciousness and Death

Pending.

### 4G — Enemy Combat AI

Pending.

### 4H — Combat End and Rewards

Pending.

---

# 28. Remaining M2.1 Rules Groups

The following M2.1 groups remain:

* Group 4 — Combat — In Progress
* Group 5 — Classes & Progression
* Group 6 — Equipment & Inventory
* Group 7 — Magic
* Group 8 — Conditions & Resting
* Group 9 — AI/Rules Boundary

---

# 29. Source-of-Truth Policy

This document is the authoritative game-rules specification for approved RealmWeaver V1 mechanics.

If future conversation, assumptions or proposed implementation contradict an approved rule in this document, this document takes precedence unless the rule is explicitly reviewed and changed.

Changes to previously approved rules should be deliberate and documented.

Major architectural or rules-strategy changes may additionally require an Architecture Decision Record (ADR).

Git history should preserve previous versions of this specification.

---

# 30. Next Design Task

Continue:

**M2.1B — Group 4D: Attacks, Armour Class & Damage**

Then proceed through the remaining Combat subsections before moving to Group 5.

---

# 31. Document Status

**M2.1:** In Progress

Approved:

* Rules Philosophy
* Character Core
* Ability Checks
* Difficulty Classes
* Saving Throws
* Contested Checks
* Narrative Degrees of Success
* Dice System
* Advantage / Disadvantage
* Hidden Rolls
* Inspiration
* Combat Start & Initiative
* Action Economy
* Abstract Combat Positioning
* Basic Movement Actions
* Basic Opportunity Attacks

In Progress:

* Combat

Not Yet Defined:

* Classes & Progression
* Equipment & Inventory
* Magic
* Conditions & Resting
* Final AI/Rules Boundary
