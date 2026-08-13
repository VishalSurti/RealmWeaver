# RealmWeaver — V1 Game Rules Specification

**Document Version:** 0.2
**Status:** Draft — M2.1 In Progress
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification & Rules-Engine Boundary
**Last Reviewed:** 13 August 2026

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

# RealmWeaver — GAME_RULES.md Combat Update

## 26.9 Attacks, Armour Class and Damage

### Status: APPROVED

Attack rolls are resolved deterministically by the RealmWeaver rules engine.

A standard attack roll uses:

**d20 + Relevant Ability Modifier + Proficiency Bonus (when applicable)**

Examples:

* Melee weapon attacks commonly use Strength.
* Ranged weapon attacks commonly use Dexterity.
* Finesse weapons may allow an eligible alternative ability.
* Spell attacks use the ability defined by the relevant spellcasting feature.

An attack hits when:

**Attack Total ≥ Target Armour Class**

An attack misses when:

**Attack Total < Target Armour Class**

The AI does not determine whether an attack hits or misses.

---

### 26.9.1 Weapon Proficiency

Characters are not automatically proficient with every weapon.

Weapon and character/class data determine whether proficiency applies to the attack roll.

If a character uses a weapon without proficiency, the attack may still be attempted where otherwise legal, but the proficiency bonus is not added.

---

### 26.9.2 Hidden Enemy Armour Class

Enemy Armour Class is normally hidden from the player.

The player may see:

* Raw attack roll
* Applicable modifiers
* Final attack total
* Hit or miss result

The exact enemy AC is not normally revealed.

This preserves uncertainty while keeping the player's own mechanics transparent.

Future abilities or systems may explicitly reveal defensive information where appropriate.

---

### 26.9.3 Damage Resolution

A successful attack resolves damage separately from the attack roll.

Damage may include:

* One or more damage dice
* Applicable static modifiers
* Additional eligible damage components

Example:

**1d8 + Strength Modifier**

The rules engine calculates damage and applies the resulting state change.

The AI may narrate the injury but must not independently alter HP.

---

### 26.9.4 Damage Types

RealmWeaver stores damage types as structured mechanical data.

Supported types may include:

* Bludgeoning
* Piercing
* Slashing
* Acid
* Cold
* Fire
* Force
* Lightning
* Necrotic
* Poison
* Psychic
* Radiant
* Thunder

V1 may use only the subset required by supported content.

The architecture should permit additional supported types without redesigning core damage resolution.

---

### 26.9.5 Resistance, Vulnerability and Immunity

RealmWeaver supports:

* Damage Resistance
* Damage Vulnerability
* Damage Immunity

Conceptually:

* Resistance reduces eligible incoming damage.
* Vulnerability increases eligible incoming damage.
* Immunity prevents eligible damage entirely.

Exact rounding and interaction rules must be resolved deterministically by the rules engine.

---

### 26.9.6 Attack Categories and Range

The combat engine distinguishes attack categories such as:

* Melee attacks
* Ranged attacks
* Spell attacks
* Special attacks or abilities

Distance-band state constrains which attacks are mechanically valid.

Melee attacks normally require an appropriate Engaged relationship.

Ranged and spell attacks must respect the supported range rules of the associated weapon, spell or ability.

---

### 26.9.7 Ranged Attacks While Engaged

Making a ranged attack while directly Engaged with a hostile creature may impose Disadvantage unless an ability, feature or other supported rule overrides this restriction.

The rules engine determines whether the penalty applies.

---

### 26.9.8 Multiple Attacks

The combat system supports multiple attacks within a single Action only when a supported feature explicitly grants them.

A character does not gain additional attacks merely because an unused Bonus Action exists.

The rules engine tracks each granted attack and validates its use.

---

## 26.10 Critical Hits and Critical Misses

### Status: APPROVED

A Natural 20 or Natural 1 refers to the raw d20 result before modifiers.

---

### 26.10.1 Natural 20 — Attack Rolls

A Natural 20 on an attack roll:

* Automatically hits.
* Counts as a Critical Hit.

The attack succeeds even if the final modified attack total would otherwise fail to meet the target's AC.

---

### 26.10.2 Critical Damage

On a Critical Hit, eligible attack damage dice are rolled twice.

Static modifiers are normally added only once.

Example:

Normal:

**1d8 + 3**

Critical:

**2d8 + 3**

Additional damage dice may also be doubled when the relevant supported rule permits them.

The rules engine determines which damage components qualify for critical doubling.

---

### 26.10.3 Natural 1 — Attack Rolls

A Natural 1 on an attack roll automatically misses.

Modifiers cannot convert a Natural 1 attack into a successful hit.

---

### 26.10.4 No V1 Critical Fumble Penalty

A Natural 1 does not create an additional mechanical penalty in V1.

Examples of unsupported automatic penalties include:

* Dropping or breaking a weapon
* Falling prone
* Hitting an ally
* Damaging equipment

The AI may narrate the failure dramatically but may not invent an additional mechanical consequence unless another valid mechanic independently causes it.

---

### 26.10.5 Critical Narration

The AI may use stronger narrative language for Natural 20s and Natural 1s.

Narrative flavour must not create additional mechanical effects that were not produced by the rules engine.

---

### 26.10.6 Future Critical Expansion

Future RealmWeaver versions may introduce configurable additional mechanical bonuses for Natural 20 attack rolls and penalties or critical-fumble effects for Natural 1 attack rolls.

These additional mechanics are explicitly outside V1.

---

## 26.11 Hit Points, Healing, Unconsciousness and Death

### Status: APPROVED

RealmWeaver tracks:

* Maximum HP
* Current HP
* Temporary HP

The rules engine remains authoritative over all HP state.

---

### 26.11.1 Damage and Temporary HP

Incoming damage is applied to Temporary HP before Current HP.

Temporary HP does not increase Maximum HP.

When Temporary HP reaches zero, remaining incoming damage is applied to Current HP.

---

### 26.11.2 Healing

Healing restores Current HP.

Healing cannot normally increase Current HP above Maximum HP.

Healing may originate from:

* Spells
* Abilities
* Potions
* Resting
* Other supported mechanics

The rules engine calculates and applies healing.

The AI may narrate healing but may not independently modify HP.

---

### 26.11.3 Reaching 0 HP

When a character reaches 0 HP, they become Unconscious unless another supported rule explicitly causes a different result.

The engine records relevant unconscious/death-save state.

---

### 26.11.4 Death Saving Throws

An Unconscious character at 0 HP makes Death Saving Throws according to the supported turn rules.

A standard Death Saving Throw uses a d20.

Baseline result:

* 10 or higher → Success
* 1–9 → Failure

Three Death Save Successes stabilise the character.

Three Death Save Failures cause death.

---

### 26.11.5 Natural 20 and Natural 1 on Death Saves

A Natural 20 on a Death Saving Throw restores 1 HP and allows the character to regain consciousness according to the supported rules.

A Natural 1 counts as two Death Save Failures.

---

### 26.11.6 Damage While Unconscious

Taking damage while at 0 HP may cause Death Save Failures according to the supported rules.

The game engine resolves the mechanical consequence.

The AI cannot ignore or override the result.

---

### 26.11.7 Massive Damage

RealmWeaver supports instant death from sufficiently severe excess damage.

If damage reduces a character to 0 HP and the remaining damage meets the required massive-damage threshold, the character may die immediately.

The rules engine determines whether the threshold is reached.

---

### 26.11.8 Stabilisation in Solo Play

Stabilisation does not automatically teleport, rescue or fully recover the player.

The current world state determines what happens after stabilisation.

Possible outcomes include:

* Rescue by allies
* Capture
* Robbery
* Arrest
* Remaining unconscious until later recovery
* Continued danger
* Other contextually valid consequences

The AI may determine narrative consequences only within the constraints of the authoritative game state.

---

### 26.11.9 Defeat Without Death

Defeat does not always imply death.

Enemies may have objectives such as:

* Capture
* Theft
* Arrest
* Interrogation
* Humiliation
* Escape
* Other non-lethal outcomes

Enemy intention, encounter context and mechanics determine whether a defeated player is killed or subjected to another valid consequence.

The AI may not retroactively undo a mechanically established death merely to continue the story.

---

### 26.11.10 Character Death and Campaign Continuity

Character death does not necessarily delete the campaign.

The RealmWeaver architecture should permit a future continuation model in which the player may create another character within the same persistent campaign world.

Previous character actions and world consequences may remain part of campaign history.

Full multi-character campaign continuation is not required to be completely implemented in the earliest V1 release, but the architecture must avoid assuming that one campaign can only ever contain one playable character permanently.

---

## 26.12 Enemy Combat AI

### Status: APPROVED

RealmWeaver uses a hybrid enemy-combat decision model.

The core principle is:

> **AI selects tactical intent where appropriate. The rules engine validates and resolves mechanics.**

---

### 26.12.1 Tactical Profiles

Enemies may have lightweight tactical profiles such as:

* Aggressive
* Defensive
* Cowardly
* Ambusher
* Ranged
* Spellcaster
* Protector
* Leader

Enemy behaviour should reflect creature intelligence, personality, combat role and current objective where possible.

---

### 26.12.2 Enemy Objectives

Enemies may have objectives beyond reducing the player to 0 HP.

Examples include:

* Kill
* Capture
* Escape
* Defend
* Delay
* Protect another creature
* Steal an item
* Hold a location
* Complete another encounter-specific goal

Objectives influence tactical decisions and defeat outcomes.

---

### 26.12.3 Combat Decision Inputs

Enemy decision logic may consider relevant information such as:

* Current HP
* Status effects
* Available actions
* Available abilities
* Distance bands
* Known player position/state
* Allies
* Enemies
* Objective
* Tactical profile
* Observable battlefield state

Enemy decision logic must not use hidden information the creature could not reasonably know.

---

### 26.12.4 Mechanical Validation

Every enemy action is validated by the deterministic rules engine.

Validation may include:

* Correct turn
* Action availability
* Bonus Action availability
* Reaction availability
* Target validity
* Range
* Movement
* Resource availability
* Ability availability
* Status restrictions

An AI-proposed action that is mechanically invalid must not be executed as if valid.

---

### 26.12.5 Deterministic Fallback Behaviour

RealmWeaver must provide deterministic fallback behaviour when AI-assisted tactical generation fails, times out or returns invalid output.

Fallback logic may prioritise:

1. Use a legal attack against an appropriate target.
2. Move toward a valid target where required.
3. Use a defensive or safe action when no valid attack is available.

AI failure must not freeze or corrupt combat.

---

### 26.12.6 Enemy Knowledge Restrictions

Enemies may only act using information they reasonably possess.

They must not automatically know:

* Hidden player resources
* Exact unseen statistics
* Unrevealed weaknesses
* Secret inventory information
* Other hidden state

unless a supported rule or narrative event has made that information available.

---

### 26.12.7 Morale

V1 supports basic morale behaviour.

Enemies may:

* Flee
* Surrender
* Change tactics
* Retreat
* Continue fighting

depending on circumstances.

Relevant factors may include:

* Low HP
* Loss of a leader
* Loss of allies
* Failed objectives
* Creature personality
* Creature type
* Intelligence
* Fearlessness or similar traits

Not all creatures use identical morale logic.

---

### 26.12.8 Difficulty and Tactical Quality

Campaign difficulty may influence the quality of enemy tactical decision-making.

Example direction:

* Easy → simpler tactics
* Normal → competent tactics
* Hard → stronger coordination and better use of supported abilities

Difficulty must not permit enemies to:

* Manipulate dice
* Read hidden player state
* Ignore rules
* Gain impossible knowledge

---

### 26.12.9 V1 Hybrid Tactical Model

For V1:

* Simple or ordinary enemies may primarily use deterministic tactical logic.
* Important, intelligent or mechanically complex enemies may use AI-assisted tactical reasoning.

This approach reduces latency, API cost and unpredictable behaviour while preserving dynamic encounters where it matters most.

---

### 26.12.10 Future Tactical Expansion

Future versions may extend AI-assisted tactical reasoning to most or all enemies and NPCs.

The deterministic rules engine will remain authoritative over mechanical validity and resolution.

---

## 26.13 Combat End and Encounter Resolution

### Status: APPROVED

Combat ends when structured turn-by-turn combat resolution is no longer required.

Combat does not require every opponent to be killed.

---

### 26.13.1 Valid Combat-End Conditions

Combat may end because:

* All active hostile opponents are defeated.
* Opponents surrender.
* The player surrenders.
* Opponents successfully flee.
* The player successfully escapes.
* The encounter objective is completed.
* Hostility otherwise ends.
* Continued turn-based resolution is no longer necessary.

---

### 26.13.2 Fleeing and Pursuit

A fleeing opponent does not automatically end the encounter if the player chooses and is mechanically able to pursue.

If pursuit is abandoned or no longer appropriate, the encounter may end.

---

### 26.13.3 Encounter Objectives

Encounters may have explicit objectives beyond defeating all opponents.

Examples include:

* Survive for a number of rounds.
* Protect an NPC.
* Capture a target.
* Escape an area.
* Hold a position.
* Retrieve an item.
* Delay enemies.
* Defeat a particular target.

Encounter completion is determined by the relevant objective state.

---

### 26.13.4 Combat Finalisation

When combat ends, RealmWeaver performs an authoritative encounter-finalisation process.

This may include:

1. Finalise participant HP and status.
2. Record deaths.
3. Record unconscious or stabilised characters.
4. Record surrendered opponents.
5. Record captured opponents.
6. Record escaped opponents.
7. Resolve the encounter objective.
8. Determine eligible rewards.
9. Update quests.
10. Update progression state.
11. Apply persistent world/NPC consequences.
12. Save authoritative state.
13. Return control to normal narrative play.

The AI narrates the aftermath only after authoritative state has been resolved.

---

### 26.13.5 Persistent Character and NPC State

Combat outcomes persist beyond the encounter.

Examples:

* Dead NPCs remain dead unless a legitimate supported mechanic later changes that state.
* Escaped NPCs may return later.
* Captured NPCs remain captured until the world state changes.
* Hostility and relationships may change.
* Important injuries or other supported states may persist.

The AI must respect persisted state.

---

### 26.13.6 Progression Rewards

RealmWeaver supports both:

* XP-based progression
* Milestone-based progression

XP campaigns may award progression for successfully overcoming encounters or objectives.

XP should not depend exclusively on killing opponents.

Alternative successful resolutions may also qualify, including:

* Surrender
* Capture
* Negotiation
* Bypassing an encounter
* Completing the encounter objective through another valid method

Milestone campaigns do not automatically award combat XP.

Exact XP values and level-progression formulas will be defined under Classes & Progression.

---

### 26.13.7 Contextual Loot

Loot must be consistent with the defeated, surrendered, robbed, searched or otherwise interacted-with creature or NPC.

Loot generation may consider:

* Creature/NPC archetype
* Equipped items
* Persistent inventory
* Wealth
* Encounter difficulty
* Location
* Campaign progression
* Story context

Level or encounter difficulty alone must not determine available loot.

Important NPC possessions should preferably originate from persistent world state rather than being generated only when the NPC is looted.

The AI may suggest loot but cannot arbitrarily create mechanically significant items or currency.

---

### 26.13.8 Available Loot vs Player Inventory

Available encounter loot is distinct from the player's inventory.

Items are not automatically transferred merely because an opponent possesses them.

The player may decide which eligible items to take.

Only items actually acquired are added to the player's inventory.

This separation supports later rules such as:

* Carrying capacity
* Encumbrance
* Item restrictions
* Inventory management

---

### 26.13.9 Hidden Loot and Discovery

Some loot or information may not be immediately visible.

Appropriate discovery mechanics may be required.

Examples include:

* Investigation
* Perception
* Search actions
* Opening containers
* Examining clothing or equipment

The rules engine resolves relevant checks.

---

### 26.13.10 Quest Updates

Encounter outcomes may update quest state.

Possible results include:

* Progress
* Completion
* Failure
* Objective replacement
* New objectives
* Other supported state changes

Quest state must reflect authoritative encounter outcomes.

The AI may not keep an impossible objective active merely for narrative convenience.

---

### 26.13.11 World Consequences

Combat may produce persistent world consequences.

Examples include:

* NPC relationship changes
* Reputation changes
* Faction hostility
* Access to locations
* Discovery of information
* Story progression
* Death of significant characters
* Escape of recurring antagonists
* Capture or rescue outcomes

The exact world systems will be defined later in M2.

---

### 26.13.12 Reward Types

Rewards may include:

* XP
* Currency
* Equipment
* Consumable items
* Information
* Quest progression
* NPC relationships
* Reputation
* Access to locations
* Story progression
* Other validated campaign-state changes

Not all rewards are physical objects.

---

### 26.13.13 Reward Authority

The AI may propose narratively appropriate rewards.

Mechanically significant rewards must be validated by RealmWeaver systems.

The AI must not arbitrarily generate unreasonable rewards that violate campaign progression, content rules or authoritative world state.

The governing principle remains:

> **AI proposes. RealmWeaver validates.**

---

### 26.13.14 Encounter Persistence

Authoritative encounter results must be persisted before normal campaign narration proceeds.

This reduces the risk of narrative contradictions, duplicated enemies, forgotten deaths, lost loot or incorrect quest state.

---

## 26.14 Encumbrance Campaign Setting

### Status: APPROVED

RealmWeaver campaigns support a player-selectable Encumbrance setting during campaign creation.

Available modes:

* Enabled
* Disabled

Once campaign creation is confirmed and gameplay begins, this setting cannot be changed for that campaign.

When Encumbrance is disabled:

* Inventory ownership is still tracked.
* Equipment is still tracked.
* Item state is still tracked.
* Normal carrying-capacity restrictions are ignored.

When Encumbrance is enabled, the player is subject to the supported carrying-capacity and encumbrance mechanics.

Detailed encumbrance mechanics will be defined under Equipment & Inventory.

---

## 26.15 Reusable Content Pools

### Status: APPROVED DIRECTION — Architecture Pending

RealmWeaver should support reusable structured content libraries to reduce repetitive AI-generated names, locations and world-building terminology.

Potential content categories include:

* Character names
* Settlement names
* Tavern names
* Wilderness locations
* Dungeon names
* Faction names
* Item names
* Titles
* Other reusable world-building vocabulary

Imported content must originate from appropriately licensed, public-domain or otherwise permitted sources.

The system should avoid relying on copyrighted setting-specific names or content that RealmWeaver does not have permission to use.

Campaign state should track previously selected or generated identities where useful to reduce accidental repetition within the same campaign.

The content system may eventually support:

* Direct selection from approved name pools
* AI selection from generated candidates
* Procedural generation informed by approved datasets
* Cultural or regional naming groups
* Duplicate detection
* Campaign-specific used-name registries

The exact import, storage, retrieval and generation architecture will be defined later in M2.

---

# 27. Remaining M2.1 Rules Groups

The following M2.1 groups remain:

* Group 5 — Classes & Progression
* Group 6 — Equipment & Inventory
* Group 7 — Magic
* Group 8 — Conditions & Resting
* Group 9 — AI/Rules Boundary

---

# 28. Source-of-Truth Policy

This document is the authoritative game-rules specification for approved RealmWeaver V1 mechanics.

If future conversation, assumptions or proposed implementation contradict an approved rule in this document, this document takes precedence unless the rule is explicitly reviewed and changed.

Changes to previously approved rules should be deliberate and documented.

Major architectural or rules-strategy changes may additionally require an Architecture Decision Record (ADR).

Git history should preserve previous versions of this specification.

---

# 29. Next Design Task

Continue:

**M2.1B — Group 4D: Attacks, Armour Class & Damage**

Then proceed through the remaining Combat subsections before moving to Group 5.

---

# 30. Document Status

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
* Abstract Combat Positioning
* Basic Movement Actions
* Basic Opportunity Attacks
* Turn Structure & Action Economy
* Reactions
* Abstract Distance-Band Positioning
* Movement Actions
* Opportunity Attacks
* Complex Natural-Language Combat Actions
* Attacks, AC & Damage
* Critical Hits
* HP, Healing, Unconsciousness & Death
* Enemy Combat AI
* Combat End & Rewards

In Progress:

* Classes & Progression

Not Yet Defined:

* Equipment & Inventory
* Magic
* Conditions & Resting
* Final AI/Rules Boundary
