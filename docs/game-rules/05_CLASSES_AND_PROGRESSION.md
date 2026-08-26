# RealmWeaver — Classes & Progression

**Rule Group:** 5
**Status:** APPROVED
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification
**Primary Rules Baseline:** SRD 5.1 / 2014-style rules
**Last Reviewed:** 26 August 2026

---

# 1. Purpose

This document defines RealmWeaver V1 rules for:

* Classes
* Class features
* Weapon Mastery access and progression
* Species
* Species traits
* Backgrounds
* Level progression
* XP progression
* Milestone progression
* Adventure Leads
* Subclasses
* Character-building choices
* Core class progression
* Progress visibility
* Progression validation
* Ruleset versioning

The deterministic RealmWeaver rules system is authoritative over progression and mechanical character features.

The AI may explain, recommend and narrate progression, but it may not independently change:

* Character level
* Class
* Subclass
* Species mechanics
* XP
* Milestones
* Spell resources
* Class resources
* Ability Scores
* Other authoritative progression state

The governing principles remain:

> **AI tells the story. Rules decide what happens.**

and:

> **AI proposes. RealmWeaver validates.**

---

# 2. Class Scope & Core Class Model

## 2.1 V1 Class Scope

### Status: APPROVED

RealmWeaver V1 minimum supports four classes:

* Fighter
* Rogue
* Cleric
* Wizard

These classes were selected because together they exercise most major V1 character systems.

### Fighter

Primary architectural coverage:

* Martial combat
* Armour
* Weapon proficiency
* Limited-use combat resources
* Multiple attacks

### Rogue

Primary architectural coverage:

* Skills
* Expertise
* Conditional damage
* Bonus Actions
* Reactions

### Cleric

Primary architectural coverage:

* Prepared spellcasting
* Healing
* Divine-style class resources
* Armour
* Support mechanics

### Wizard

Primary architectural coverage:

* Spellbook
* Prepared spellcasting
* Spell slots
* Broad spell selection
* Resource recovery

Other classes are deferred from the minimum V1 implementation.

The architecture must permit additional classes without redesigning the progression system.

Warlock is considered a strong candidate for an early post-V1 expansion because its unusual resource model can further test the flexibility of RealmWeaver's class architecture.

---

## 2.2 Class Data Model

### Status: APPROVED

Class definitions should be primarily data-driven.

A class may define:

* Name
* Hit Die
* Primary abilities
* Saving Throw proficiencies
* Armour proficiencies
* Weapon proficiencies
* Weapon Mastery access
* Weapon Mastery capacity progression
* Skill choices
* Starting equipment
* Level-based features
* Subclass rules
* Spellcasting configuration
* Resource definitions

Complex feature behaviour may still require deterministic rules-engine logic.

RealmWeaver should avoid implementing each class as one enormous isolated code object containing all behaviour.

Class definitions and reusable feature mechanics should be separated where practical.

---

## 2.3 Hit Dice

### Status: APPROVED

Initial V1 classes use:

| Class   | Hit Die |
| ------- | ------: |
| Fighter |     d10 |
| Rogue   |      d8 |
| Cleric  |      d8 |
| Wizard  |      d6 |

Hit Dice influence:

* Maximum HP progression
* Rest recovery
* Other supported class mechanics

Detailed resting behaviour is defined under Group 8.

---

## 2.4 Starting Hit Points

### Status: APPROVED

At level 1:

**Starting Maximum HP = Maximum Hit Die Value + Constitution Modifier**

Example:

Fighter:

* Hit Die: d10
* Constitution Modifier: +2

Starting HP:

**10 + 2 = 12**

The rules engine performs the calculation.

---

## 2.5 HP Increase After Level 1

### Status: APPROVED

When gaining a level after level 1, the player may choose either:

### Fixed HP

Use the class's supported fixed HP increase.

### Rolled HP

Roll the class Hit Die and add the Constitution Modifier.

The player may choose fixed or rolled HP independently at each level-up.

The campaign is not permanently locked to one method.

### Natural 1 Reroll

A natural 1 rolled on the class Hit Die for a level-up HP increase is not accepted.

The die must be rerolled.

This applies to:

* RealmWeaver-generated dice
* Player-entered physical dice

### Minimum HP Increase

After a valid Hit Die result and Constitution Modifier are applied, a level-up must increase Maximum HP by at least:

**+1 HP**

---

## 2.6 Proficiency Bonus

### Status: APPROVED

Proficiency Bonus is determined by character level.

| Character Level | Proficiency Bonus |
| --------------- | ----------------: |
| 1–4             |                +2 |
| 5–8             |                +3 |
| 9–12            |                +4 |
| 13–16           |                +5 |
| 17–20           |                +6 |

Proficiency does not increase every level.

The rules engine derives the correct Proficiency Bonus from character level.

When it changes, dependent statistics must be recalculated.

This may affect:

* Proficient skill checks
* Saving Throws
* Weapon attacks
* Spell attacks
* Expertise
* Other supported mechanics

---

## 2.7 Saving Throw Proficiencies

### Status: APPROVED

Classes define which Saving Throws receive proficiency.

These proficiencies are stored as class data.

The existing Saving Throw system uses them when calculating results.

The AI does not independently determine Saving Throw proficiency.

---

## 2.8 Skill Proficiencies

### Status: APPROVED

Classes provide:

* A defined set of eligible skills
* A defined number of skill choices

Players choose from the valid class list.

Selections must be validated by the rules system.

---

## 2.9 Equipment Proficiencies

### Status: APPROVED

Classes may grant:

* Armour proficiency
* Shield proficiency
* Weapon proficiency categories
* Other supported equipment proficiency

Detailed equipment behaviour is defined under Group 6.

---

## 2.10 Class Features

### Status: APPROVED

Class features unlock at defined levels.

Features may include:

* Passive effects
* Active abilities
* Limited-use resources
* Action modifications
* Bonus Action options
* Reactions
* Conditional effects
* Weapon Mastery
* Spellcasting
* Resource recovery
* Character choices

The rules engine determines:

* Whether the character possesses a feature
* Whether the feature is available
* Whether its requirements are met
* Whether a resource may be spent
* What mechanical effect occurs

The AI may explain or narrate the feature but cannot independently grant or execute unsupported mechanics.

Weapon Mastery is represented as a mechanically validated character feature rather than being inferred directly from class name.

A class may grant:

* Access to Weapon Mastery
* A defined Weapon Mastery capacity
* Level-based increases to that capacity

The existence of a Mastery property on a weapon does not itself grant the character access to that property.

Detailed Weapon Mastery combat behaviour is defined in `04_COMBAT.md`.

Detailed weapon-to-Mastery mapping and equipment interaction are defined in `06_EQUIPMENT_AND_INVENTORY.md`.

---

## 2.11 Feature State

### Status: APPROVED

Features may maintain structured state.

Example:

```text
Second Wind
uses_max = 1
uses_remaining = 1
recharge = appropriate_rest
```

Passive features may instead modify existing mechanics.

Example:

```text
Extra Attack
attacks_per_attack_action = 2
```

The architecture should support both stateful and passive feature types.

---

## 2.12 Multiclassing

### Status: DEFERRED FROM V1

V1 characters have one active class.

Multiclassing is not supported in initial V1 because it substantially increases complexity in:

* Class-level vs character-level progression
* Proficiencies
* Spell slots
* Feature combinations
* Requirements
* Balance
* Testing

The architecture should avoid making future multiclassing prohibitively difficult.

---

# 3. Species

## 3.1 Species Model

### Status: APPROVED

Species is mechanically separate from Class and Background.

Character identity conceptually includes:

* Species
* Background
* Class
* Ability Scores
* Skills
* Equipment
* Features

Species may define:

* Size
* Base speed
* Senses
* Languages
* Passive traits
* Active traits
* Resistances
* Proficiencies
* Innate magic
* Usage limits
* Recharge behaviour

Species abilities are authoritative mechanical features.

The AI may reference those abilities but may not invent unsupported species mechanics.

---

## 3.2 V1 Species Scope

### Status: APPROVED

Initial V1 species:

* Human
* Elf
* Dwarf
* Halfling
* Goliath
* Tiefling

These provide a useful spread of:

* Generalist traits
* Sensory abilities
* Resistances
* Movement differences
* Reroll mechanics
* Innate abilities
* Innate magic

Additional species may be added later.

---

## 3.3 Subspecies and Lineages

### Status: DEFERRED FROM V1

Initial V1 does not require subspecies or lineage trees.

Examples such as:

* High Elf
* Wood Elf
* Drow
* Hill Dwarf
* Mountain Dwarf

are outside minimum V1 scope.

Future architecture may support:

```text
Species
└── Lineage / Subspecies
```

without requiring initial V1 content to use it.

---

## 3.4 Species and Ability Scores

### Status: APPROVED

RealmWeaver does not rigidly tie character-creation Ability Score increases to Species.

Species primarily represents:

* Physical characteristics
* Movement
* Senses
* Resistances
* Innate abilities
* Special traits

Ability Score increases during character creation use a flexible Background or character-creation mechanism.

This permits viable combinations such as:

* Goliath Wizard
* Elf Fighter
* Tiefling Cleric

without species-specific Ability Score optimisation.

---

## 3.5 Mechanical vs Narrative Species Identity

### Status: APPROVED

RealmWeaver distinguishes:

### Mechanical Species Identity

Controls authoritative mechanics such as:

* Speed
* Darkvision
* Resistance
* Innate abilities
* Rerolls
* Languages

### Narrative Identity

May include:

* Appearance
* Culture
* Homeland
* Traditions
* Personal history

Narrative identity does not automatically create mechanical effects.

Example:

```text
Mechanical Species:
Human

Narrative Culture:
Ashvale Highlander
```

---

## 3.6 Species Knowledge and NPC Behaviour

### Status: APPROVED

NPCs and enemies should not automatically know every mechanical trait associated with a Species.

Example:

An enemy should not automatically know that a Tiefling has Fire Resistance unless:

* The creature reasonably knows this in-world
* The character has demonstrated the trait
* Another supported information source reveals it

This follows the existing enemy-knowledge restrictions.

---

## 3.7 Species Feature Progression

### Status: APPROVED

Species features may unlock or improve at particular character levels.

The feature system must therefore support progression from multiple sources:

```text
Character Progression
├── Class Features
├── Subclass Features
├── Species Features
└── Background Features
```

Species must not be treated as permanently static data if a supported trait progresses by level.

---

## 3.8 Human Traits

### Status: APPROVED V1 DIRECTION

Human is the mechanically flexible/generalist V1 Species.

Recommended V1 traits:

* Size: Medium
* Base Speed: 30 ft
* Common language
* One additional supported language choice
* One additional supported skill proficiency choice
* Flexible generalist character-creation benefit

Human does not receive blanket Species-based Ability Score increases because RealmWeaver handles Ability Score allocation separately.

Any flexible Human benefit must come from a controlled list of validated options rather than arbitrary AI-generated bonuses.

---

## 3.9 Elf Traits

### Status: APPROVED V1 DIRECTION

Baseline V1 Elf traits:

* Size: Medium
* Base Speed: 30 ft
* Darkvision
* Keen Senses
* Fey Ancestry
* Trance

### Darkvision

Darkvision is stored as a mechanical sensory feature with a supported range.

It may affect:

* Visibility
* Hidden-creature detection
* Environmental perception
* Relevant exploration checks

### Keen Senses

Elf receives Perception proficiency unless an equivalent supported rule modifies this.

Duplicate-proficiency handling follows normal character-creation validation.

### Fey Ancestry

Fey Ancestry provides the relevant supported resistance/advantage interactions against applicable magical effects.

The engine evaluates those conditions.

### Trance

Trance is represented as a Species feature relevant to rest and sleep mechanics.

Detailed rest interaction is defined under Group 8.

No Elf subspecies are required in V1.

---

## 3.10 Dwarf Traits

### Status: APPROVED V1 DIRECTION

Baseline V1 Dwarf traits:

* Size: Medium
* Base Speed: 25 ft
* Darkvision
* Dwarven Resilience
* Stonecunning-style knowledge feature

### Darkvision

Implemented through the shared sensory-feature system.

### Dwarven Resilience

Provides supported poison-related resistance or advantage where applicable.

The engine validates qualifying damage/effects.

### Stonecunning

Stonecunning represents exceptional familiarity with worked stone, ancient construction and related structures.

It may influence relevant:

* History checks
* Investigation checks
* Perception
* Automatic contextual knowledge
* Difficulty assessment

The AI may identify a stone-related situation, but the rules system determines the mechanical benefit.

---

## 3.11 Halfling Traits

### Status: APPROVED V1 DIRECTION

Baseline V1 Halfling traits:

* Size: Small
* Base Speed: 25 ft
* Lucky
* Brave
* Halfling Nimbleness

### Lucky

When an eligible d20 roll produces a natural 1, Halfling Lucky allows the roll to be rerolled according to the supported rule.

Conceptually:

```text
d20 = 1
↓
Lucky detected
↓
Reroll
```

The engine detects the Species feature automatically.

The AI does not need to remember to invoke it.

### Brave

Brave affects eligible fear-related mechanics.

The rules engine validates when the feature applies.

### Halfling Nimbleness

Traditional exact-position movement rules must be adapted to RealmWeaver's abstract distance-band system.

The adaptation should preserve the intended ability to move through spaces occupied by sufficiently larger creatures where mechanically appropriate.

---

## 3.12 Goliath Traits

### Status: APPROVED V1 DIRECTION

Goliath is retained as a V1 Species.

Because RealmWeaver's primary baseline is SRD 5.1 while Goliath appears in later SRD material, Goliath is treated as an explicitly documented RealmWeaver rules-source exception.

Baseline V1 Goliath identity should include:

* Size: Medium
* Base Speed: 30 ft
* Powerful-build-style carrying capability
* Cold-related resilience where supported
* Limited-use durability or giant-ancestry-style ability

The exact Goliath trait implementation should be selected from appropriately licensed SRD material and simplified where necessary for V1 consistency.

The implementation must not silently combine incompatible versions of the Species.

---

## 3.13 Tiefling Traits

### Status: APPROVED V1 DIRECTION

Baseline V1 Tiefling traits:

* Size: Medium
* Base Speed: 30 ft
* Darkvision
* Fire Resistance
* Innate infernal-style magic
* Relevant language/context traits

### Darkvision

Uses the shared sensory-feature system.

### Fire Resistance

Eligible Fire damage is reduced according to the supported Resistance rules.

### Innate Magic

Tiefling may receive Species-based innate spells or magical abilities.

These are stored as authoritative Species features.

Species spellcasting may unlock at defined character levels.

The system distinguishes:

* Innate Species spell
* Class spell
* Prepared spell
* Spell-slot resource

Species-granted magic must not be treated as ordinary class spellcasting unless the relevant rule explicitly says so.

---

## 3.14 Species Trait Authority

### Status: APPROVED

Species traits are part of authoritative character state.

The AI may not:

* Forget a mechanically active Species trait
* Invent a new Species ability
* Disable a Species trait without a valid mechanic
* Grant a Species trait from another Species
* Apply a Species trait when eligibility requirements are not met

Species-related mechanics must be resolved through the rules system.

---

# 4. Backgrounds

## 4.1 Background Model

### Status: APPROVED

Background is mechanically separate from Species and Class.

Background represents the character's profession, history or life experience before the current adventure.

Background may provide:

* Skill proficiencies
* Tool or language choices
* Starting equipment
* Ability Score allocation
* Lightweight features
* Narrative context
* Character hooks

---

## 4.2 V1 Background Scope

### Status: APPROVED

Initial V1 backgrounds:

* Soldier
* Criminal
* Scholar
* Acolyte
* Artisan
* Noble
* Outlander
* Custom

Additional backgrounds may be added later.

---

## 4.3 Duplicate Proficiencies

### Status: APPROVED

If a Background grants a proficiency the character already possesses, the proficiency should not be wasted.

The player is offered an eligible replacement according to supported character-creation rules.

All replacements must be validated.

---

## 4.4 Background Ability Score Allocation

### Status: APPROVED DIRECTION

Character creation should support flexible Ability Score increases.

The intended model may support:

* +2 to one ability and +1 to another
* +1 to three different abilities

The exact implementation remains subject to final character-creation validation rules.

Normal Ability Score limits apply.

---

## 4.5 Background Features

### Status: APPROVED

Backgrounds may provide lightweight mechanical or narrative features.

Examples may involve:

* Social access
* Knowledge
* Contacts
* Trade familiarity
* Survival experience

V1 Background features should avoid requiring disproportionately complex simulation systems.

---

## 4.6 Custom Backgrounds

### Status: APPROVED

V1 supports Custom Backgrounds.

The player may describe their history naturally.

The AI may help translate that description into legal mechanical options.

Example:

> I was a travelling healer who worked in remote villages.

AI may propose:

* Medicine
* Insight
* Relevant tool or language option
* Narrative description

The rules system validates all mechanical selections.

---

## 4.7 Background Narrative Context

### Status: APPROVED

Background information may be exposed to the AI for narrative reasoning.

Examples:

* Soldier may understand military organisations.
* Criminal may recognise underworld behaviour.
* Scholar may recognise historical references.
* Acolyte may understand religious traditions.

Background does not automatically guarantee success.

Depending on context it may influence:

* Automatic knowledge
* Proficiency
* Advantage
* Difficulty
* Narrative interpretation

Mechanical resolution remains authoritative.

---

## 4.8 Background Permanence

### Status: APPROVED

Background normally becomes immutable once campaign creation is confirmed.

A future explicit game mechanic may alter identity/history state, but the player cannot simply change Background through settings during normal V1 gameplay.

---

# 5. Level-Up Structure

## 5.1 Level-Up Eligibility

### Status: APPROVED

Reaching the relevant XP threshold or completing a valid Milestone grants:

> **Level Up Available**

It does not immediately modify the character.

---

## 5.2 Level-Up Timing

### Status: APPROVED

Level-up should occur at a mechanically appropriate narrative point outside active structured combat.

Examples:

* After combat
* During rest
* Between major scenes
* Session end
* Session start

RealmWeaver should not unlock new mechanics halfway through an unresolved combat turn.

---

## 5.3 Player Control

### Status: APPROVED

The player chooses when to complete an available level-up.

The player may postpone an available level indefinitely.

RealmWeaver continues showing:

> **Level Up Available**

until completed.

The AI cannot silently or forcibly level the character.

---

## 5.4 Atomic Level-Up Transaction

### Status: APPROVED

A level-up is applied as one authoritative transaction.

Potential changes include:

* Character level
* Proficiency Bonus
* Maximum HP
* Current HP adjustment
* Hit Dice
* Class features
* Weapon Mastery capacity
* Feature upgrades
* Species progression features
* Spellcasting progression
* Spell slots
* Spell preparation limits
* Ability Score Improvements
* Subclass features
* Resource maxima

Either the full valid level-up succeeds or the previous valid character state remains unchanged.

---

## 5.5 Pending Level-Up State

### Status: APPROVED

Incomplete selections may be stored separately as pending state.

Example:

```text
Pending Level Up
target_level = 5

completed:
- HP choice
- ASI

remaining:
- spell selections
```

Pending selections do not partially mutate the authoritative character.

---

## 5.6 Level-Up Summary

### Status: APPROVED

Before committing, RealmWeaver displays:

### Automatic Changes

Examples:

* Proficiency Bonus change
* Automatically unlocked features
* Resource-capacity changes
* Spell-slot progression
* Species feature progression
* Weapon Mastery access
* Weapon Mastery capacity progression

### Player Choices Required

Examples:

* HP method
* Subclass
* ASI
* Spells
* Fighting Style
* Expertise
* New Weapon Mastery selection where capacity increases

After completion, RealmWeaver displays exactly what changed.

---

## 5.7 Current HP During Level-Up

### Status: APPROVED

When Maximum HP increases, Current HP increases by the same amount.

Example:

Before:

```text
22 / 30 HP
```

Level-up HP increase:

```text
+8
```

After:

```text
30 / 38 HP
```

The character is not otherwise fully healed.

---

## 5.8 Resource Restoration

### Status: APPROVED

Level-up does not automatically function as a Long Rest.

Spent resources do not automatically refill.

New capacity may be added where the level grants it.

---

## 5.9 Ability Score Improvement

### Status: APPROVED

When progression grants an ASI, the player may choose:

* +2 to one eligible ability
* +1 to two eligible abilities

Normal Ability Score maximum:

**20**

unless a supported feature explicitly permits a higher value.

---

## 5.10 Feats

### Status: DEFERRED FROM INITIAL V1

A general feat catalogue is not required for initial V1.

Architecture should allow feats to be represented as character features in future versions.
A future supported feat may grant Weapon Mastery access, additional Mastery Capacity or another explicitly defined Mastery-related feature.

Weapon Mastery must therefore remain feature-driven rather than being structurally restricted to particular class names.

---

## 5.11 Level History

### Status: APPROVED

RealmWeaver maintains progression history.

Example:

```text
Level 2
- HP Roll: 7
- Gained Action Surge

Level 3
- Selected subclass
- HP Method: Fixed

Level 4
- STR +2
```

This supports:

* Player understanding
* Debugging
* Auditability
* Future migrations
* Potential future respec

---

## 5.12 Supported Level Cap

### Status: APPROVED

Initial guaranteed V1 support:

**Levels 1–5**

Stretch target:

**Levels 1–10**

Architecture target:

**Levels 1–20**

The engine must not allow progression into an unsupported level.

---

# 6. XP Progression

## 6.1 XP Mode

### Status: APPROVED

XP is stored cumulatively.

It does not normally reset after level-up.

---

## 6.2 XP Thresholds

### Status: APPROVED

For guaranteed V1 levels:

| Level | Total XP Required |
| ----- | ----------------: |
| 1     |                 0 |
| 2     |               300 |
| 3     |               900 |
| 4     |             2,700 |
| 5     |             6,500 |

The broader supported progression table should exist as rules data.

The AI does not determine thresholds.

---

## 6.3 XP Sources

### Status: APPROVED

Potential XP sources include:

* Combat encounters
* Alternative encounter resolution
* Quest objectives
* Quest completion
* Exploration
* Significant discovery
* Problem solving
* Major character achievements
* Story achievements

XP is not restricted to kills.

---

## 6.4 Encounter XP

### Status: APPROVED

XP primarily represents overcoming encounters or objectives.

Valid resolutions may include:

* Defeating opponents
* Surrender
* Capture
* Avoidance
* Negotiation
* Alternative objective completion

---

## 6.5 Quest XP

### Status: APPROVED

Individual objectives may grant XP independently.

Overall quest failure does not automatically erase XP earned for meaningful completed objectives.

---

## 6.6 Exploration XP

### Status: APPROVED

Exploration XP may be granted for meaningful discoveries.

Routine movement or entering ordinary rooms does not generate XP.

Discovery XP should generally be tied to meaningful first-time state transitions.

---

## 6.7 Roleplay XP

### Status: APPROVED

Ordinary strong roleplay is generally rewarded through:

**Inspiration**

Major character or story achievements may qualify for XP.

---

## 6.8 Event-Driven XP

### Status: APPROVED

XP progression must not depend on the AI remembering to award XP.

Conceptually:

```text
Player / World Event
        ↓
Structured Event
        ↓
Progression Service
        ↓
Eligibility / Significance
        ↓
XP Calculation
        ↓
Duplicate Prevention
        ↓
Award
```

The AI may help classify ambiguous narrative events.

---

## 6.9 XP Reward Categories

### Status: APPROVED

Reusable categories may include:

* Encounter
* Quest Objective
* Quest Completion
* Discovery
* Character Milestone
* Story Achievement

Possible significance bands:

* Minor
* Moderate
* Major

The exact numerical balancing system is deferred to implementation/balancing.

---

## 6.10 XP Farming Protection

### Status: APPROVED

Repeated trivial activity should not provide unlimited XP.

Eligibility may consider:

* Challenge
* Significance
* Repetition
* First-time completion
* Character level
* Objective state

---

## 6.11 XP State Transitions

### Status: APPROVED

Where practical, XP should be associated with persisted state transitions.

Example:

```text
discovered = false
```

becomes:

```text
discovered = true
```

and may trigger discovery XP once.

---

## 6.12 XP Loss

### Status: APPROVED

Normal V1 gameplay does not subtract earned XP.

---

## 6.13 Multiple Level Thresholds

### Status: APPROVED

All XP is retained when multiple thresholds are crossed.

However, level-ups are completed one level at a time.

---

## 6.14 XP Beyond Supported Level Cap

### Status: APPROVED

XP may continue accumulating even when the current playable level cap has been reached.

If a later compatible rules release raises the cap, accumulated XP may immediately make the next level available.

---

## 6.15 XP Visibility

### Status: APPROVED

The player may inspect:

* Current XP
* Next threshold
* XP remaining
* Level-up availability

Awarded XP is shown after resolution.

---

## 6.16 XP Ledger

### Status: APPROVED

RealmWeaver maintains an auditable XP event history containing information such as:

* Amount
* Source type
* Source identifier
* Reason
* Timestamp

This helps prevent duplicate rewards.

---

# 7. Milestone Progression & Adventure Leads

## 7.1 Milestone Mode

### Status: APPROVED

Milestone progression is an alternative to XP progression.

A Milestone campaign does not secretly accumulate XP.

---

## 7.2 Milestone Objects

### Status: APPROVED

Milestones are stored as structured campaign state.

Conceptually:

```text
Milestone
├── id
├── title
├── description
├── status
├── significance
├── source
├── completion_conditions
└── level_reward
```

---

## 7.3 Milestone Completion

### Status: APPROVED

Milestone completion occurs through authoritative state transition.

Example:

```text
INCOMPLETE → COMPLETE
```

This may produce:

> **Level Up Available**

The player remains free to postpone the level-up.

---

## 7.4 Explicit and Dynamic Milestones

### Status: APPROVED

RealmWeaver supports:

### Explicit Milestones

Defined as part of known campaign progression.

### Dynamic Milestones

Created when the campaign evolves unexpectedly.

Dynamic milestones must be:

1. Proposed
2. Validated
3. Persisted
4. Significant enough to qualify

---

## 7.5 Milestone Significance Scaling

### Status: APPROVED

Milestone significance should increase appropriately with:

* Current character level
* Campaign stage
* Narrative impact
* Danger
* Scope
* Story-arc importance

A milestone suitable for level 2 may not be sufficient for level 9.

Higher-level advancement generally requires greater accomplishments.

---

## 7.6 Milestone Reward Limit

### Status: APPROVED

A single milestone grants at most one level-up in V1.

---

## 7.7 Duplicate Milestone Prevention

### Status: APPROVED

A completed milestone cannot grant progression twice.

---

## 7.8 Secret Milestones

### Status: APPROVED

Some milestone conditions may remain hidden when revealing them would expose information the character does not possess.

---

## 7.9 Opportunity Knowledge States

### Status: APPROVED

RealmWeaver distinguishes:

1. Hidden Opportunity
2. Known Lead
3. Discovered Quest / Goal

### Hidden Opportunity

Exists in the world but is unknown to the character.

### Known Lead

The character knows something may be worth investigating.

### Discovered Quest / Goal

The objective is sufficiently known to become formal tracked content.

---

## 7.10 Adventure Leads

### Status: APPROVED

Adventure Leads help prevent campaigns from stalling after all known quests are completed.

Possible sources include:

* NPC rumours
* Tavern conversations
* Letters
* Books
* Notice boards
* Visible landmarks
* Map discoveries
* Faction activity
* Quest consequences
* Character background
* NPC relationships

---

## 7.11 Lead Secrecy

### Status: APPROVED

RealmWeaver should not expose hidden quest implementation details simply to guide the player.

Instead of:

> Talk to Blacksmith Daren to start Quest Q-108.

it may surface:

> Townsfolk mention that the blacksmith has seemed unusually worried.

---

## 7.12 Known Leads Interface

### Status: APPROVED

Known Leads should be visible separately from Active Quests.

Conceptually:

```text
Journal
├── Active Quests
├── Known Leads
└── Completed Quests
```

---

## 7.13 Campaign Momentum

### Status: APPROVED

If:

* No active quests remain
* No meaningful Leads remain
* No suitable existing opportunity can be surfaced

a future Campaign/Story Director may propose new adventure content.

New content must be:

1. Proposed
2. Validated
3. Persisted
4. Introduced naturally

---

## 7.14 Progression and Leads

### Status: APPROVED

The system should help prevent progression from stalling solely because all known objectives have been exhausted.

However, not every Lead must produce:

* XP
* Milestone progress
* Level progression

Some Leads may instead provide:

* Lore
* Equipment
* Currency
* Relationships
* Secrets
* Narrative development

---

# 8. Subclasses & Character Choices

## 8.1 Subclass Model

### Status: APPROVED

Subclasses are mechanically separate from base classes.

Base-class features are not duplicated in subclass definitions.

Conceptually:

```text
Class
├── Base Features
└── Subclass
    └── Additional Features
```

---

## 8.2 Subclass Selection Level

### Status: APPROVED

Subclass-selection level is defined by class progression.

RealmWeaver does not impose one universal subclass-selection level.

---

## 8.3 V1 Subclass Scope

### Status: APPROVED

Initial V1 supports one mechanically straightforward subclass per supported class:

* Fighter → Champion
* Rogue → Thief
* Cleric → Life Domain
* Wizard → School of Evocation

Additional subclasses are a V1 stretch feature.

---

## 8.4 Simple-Subclass Strategy

### Status: APPROVED

Initial V1 prioritises mechanically straightforward subclasses to reduce:

* Implementation complexity
* Balance risk
* Testing burden

Later subclasses may deliberately exercise more advanced systems.

---

## 8.5 Subclass Features

### Status: APPROVED

Subclass features use the same structured feature system as class features.

They may be:

* Passive
* Active
* Limited-use
* Reaction-based
* Resource-consuming
* Conditional
* Spell-related

---

## 8.6 Subclass Permanence

### Status: APPROVED

Subclass choice normally becomes permanent once confirmed.

Initial V1 does not include a general respec or subclass-switching system.

Architecture should support future controlled respec.

---

## 8.7 Character Choice System

### Status: APPROVED

RealmWeaver should use a generic Character Choice system for progression decisions.

Possible choice types include:

* Subclass
* Skill
* Fighting Style
* Spell
* Expertise
* Ability Score Improvement
* Feature option
* Future feat

Conceptually:

```text
CharacterChoice
├── source
├── choice_type
├── available_options
├── number_required
├── prerequisites
└── selected_options
```

---

## 8.8 Choice Validation

### Status: APPROVED

Choices may have prerequisites and validation rules.

Invalid choices cannot be committed.

---

## 8.9 AI Recommendations

### Status: APPROVED

The AI may recommend character-building choices.

Permanent decisions remain under player control.

The AI may not silently choose:

* Subclass
* ASI
* Spell selection
* Fighting Style
* Expertise
* Other permanent build decisions

unless the player explicitly delegates the choice through a supported interface.

---

## 8.10 Custom Mechanical Subclasses

### Status: DEFERRED FROM V1

The AI cannot invent arbitrary mechanical subclasses in V1.

Narrative flavour may be applied to supported mechanical subclasses.

Example:

```text
Mechanical Subclass:
Champion

Narrative Identity:
Knight of the Ashen Order
```

---

# 9. Core Class Progression

## 9.1 Rules Baseline

### Status: APPROVED

RealmWeaver V1 primarily uses SRD 5.1 / 2014-style mechanics as its class-progression baseline.

Specific later SRD mechanics may be incorporated as explicitly documented exceptions when appropriate.

RealmWeaver may adapt mechanics for solo AI gameplay.

Such adaptations must be documented.

---

## 9.1A Weapon Mastery Class Framework

### Status: APPROVED

RealmWeaver selectively adopts Weapon Mastery as an explicit RealmWeaver adaptation while retaining SRD 5.1 / 2014-style mechanics as the primary rules baseline.

Weapon Mastery is always enabled.

Weapon Mastery access is granted through authoritative character features.

V1 class access is:

| Class   | Weapon Mastery |
| ------- | -------------- |
| Fighter | Yes            |
| Rogue   | Yes            |
| Cleric  | No             |
| Wizard  | No             |

Cleric and Wizard do not receive Weapon Mastery through their normal V1 class progression.

They may still use weapons according to their normal proficiencies and other applicable rules.

Future supported mechanics may grant Weapon Mastery through sources such as:

* Feats
* Subclasses
* Special training
* Campaign rewards
* Other validated character features

The architecture must therefore not hardcode Weapon Mastery exclusively to Fighter and Rogue class names.

---

### 9.1A.1 Mastery Capacity

Characters with Weapon Mastery have a defined Mastery Capacity.

Mastery Capacity represents the maximum number of weapon types the character may currently have selected for Weapon Mastery.

Conceptually:

```text
Weapon Mastery Feature: ACTIVE
Mastery Capacity: 3

Selected:
- Longsword
- Longbow
- Greatsword
```
he number of selected weapon types may be lower than the character's current capacity.

Unused Mastery Capacity does not provide any mechanical benefit until a valid weapon type is selected.

## 9.1A.2 Weapon-Specific Selection

Weapon Mastery selections apply to specific weapon types.

Example:

Mastered:
Longsword

does not mean:

Mastered:
Sap

The character masters the weapon type.

The weapon definition determines which Mastery property that weapon provides.

This preserves weapon identity and allows the same character feature system to support future weapon content.

## 9.1A.3 Weapon Mastery and Proficiency

Weapon proficiency and Weapon Mastery are separate mechanics.

A character may be proficient with a weapon without having selected that weapon for Mastery.

Conceptually:

Longsword Proficiency: YES
Longsword Mastery: NO

The character may use the Longsword normally but does not receive its Mastery property.

When selecting Weapon Mastery through normal class progression, RealmWeaver restricts selection to weapon types the character is mechanically eligible to master under the supported rules.

## 9.1A.4 Ownership Is Not Required

A character does not need to currently own a weapon to select that weapon type for Mastery.

Weapon Mastery represents training rather than ownership.

Example:

Mastered:
Greatsword

Inventory:
No Greatsword

is valid.

If the character later obtains a Greatsword, the existing Mastery selection becomes usable immediately when all normal equipment and wield requirements are satisfied.

During character creation, RealmWeaver should warn the player when a selected Mastery weapon is not available in their current starting equipment.

The warning does not invalidate the selection.

## 9.1A.5 Character-Creation Selection

A level-1 character whose class grants Weapon Mastery selects their initial mastered weapon types during character creation.

RealmWeaver presents only mechanically valid choices.

The interface should expose the Mastery property associated with each available weapon type.

Example:

Choose Weapon Masteries

Longsword — Sap
Greatsword — Graze
Longbow — applicable Mastery
Battleaxe — Topple

The player makes the selection.

The AI may explain or recommend options but may not select Weapon Masteries on the player's behalf.

## 9.1A.6 Changing Weapon Mastery

After completing a successful Long Rest, a character with Weapon Mastery may optionally change their current mastered weapon selections.

Changing Weapon Mastery is never automatic.

The player may retain all existing selections or reselect any number of eligible weapon types, provided the final selection does not exceed current Mastery Capacity.

Conceptually:

Before Long Rest:

Longsword
Longbow
Greatsword

After optional reselection:

Battleaxe
Longbow
Maul

The AI may recommend a change but cannot perform the change for a player-controlled character.

Weapon Mastery reselection and equipment changes are separate mechanics.

Selecting a new Weapon Mastery does not:

Grant the weapon
Equip the weapon
Wield the weapon
Remove another physical weapon from inventory

Likewise, equipping or acquiring a weapon does not automatically alter Weapon Mastery selections.

Detailed Long Rest resolution is defined in 08_CONDITIONS_AND_RESTING.md.

## 9.1A.7 Capacity Increase

When class progression increases Weapon Mastery Capacity, existing Mastery selections remain unchanged.

Example:

Before:
Capacity = 3
Selected = 3

After Level Up:
Capacity = 4
Selected = 3

RealmWeaver then allows the player to select one additional eligible weapon type.

A capacity increase does not force the player to reselect existing Masteries.

9.1A.8 Postponing a New Mastery Selection

A player may postpone filling newly available Weapon Mastery Capacity.

Example:

Mastery Capacity = 4
Selected Masteries = 3

is valid.

The unused selection provides no mechanical benefit until the player chooses an eligible weapon type.

RealmWeaver should continue to indicate that an unused Mastery selection is available.

## 9.1A.9 Persistent Mastery State

Weapon Mastery access, capacity and selected weapon types are authoritative persistent character state.

They must survive:

Scene transitions
Rest
Save/reload
AI context reconstruction

The AI does not reconstruct Weapon Mastery selections from conversation history.

If the feature granting Weapon Mastery becomes temporarily inactive through a future supported mechanic, the selections remain stored but provide no Mastery benefit until the feature becomes active again.

## 9.1A.10 Higher-Level Progression

RealmWeaver V1 guarantees class progression through levels 1–5.

Architecture must support Weapon Mastery progression through eventual levels 1–20.

Higher-level:

Mastery Capacity increases
Fighter-specific Mastery enhancements
Flexible Mastery features
Other revised-rules Mastery interactions

are not automatically imported into V1.

Any such mechanics require explicit future RealmWeaver approval and documentation.

---

## 9.2 Fighter

### 9.2.1 Fighter Progression — Levels 1–5

| Level | Proficiency | Weapon Mastery Capacity | Features                                    |
| ----- | ----------: | ----------------------: | ------------------------------------------- |
| 1     |          +2 |                       3 | Weapon Mastery, Fighting Style, Second Wind |
| 2     |          +2 |                       3 | Action Surge                                |
| 3     |          +2 |                       3 | Champion                                    |
| 4     |          +2 |                       3 | Ability Score Improvement                   |
| 5     |          +3 |                       4 | Extra Attack                                |

### 9.2.2 Weapon Mastery

Fighter gains Weapon Mastery at level 1.

Fighter Weapon Mastery Capacity for guaranteed V1 levels is:

| Fighter Level | Mastery Capacity |
| ------------: | ---------------: |
| 1             |                3 |
| 2             |                3 |
| 3             |                3 |
| 4             |                3 |
| 5             |                4 |

The Fighter therefore begins with broader Weapon Mastery access than the Rogue.

At level 5:

```text
Mastery Capacity
3 → 4
```
Existing selections remain unchanged.

The player may select one additional eligible weapon type immediately or postpone the selection.

Fighter Weapon Mastery follows the shared rules defined under 9.1A Weapon Mastery Class Framework.

Advanced higher-level Fighter Weapon Mastery features are deferred until the corresponding level range is formally designed.

### 9.2.3 Fighting Style

At level 1, Fighter selects one supported Fighting Style.

Initial supported options may include:

* Archery
* Defense
* Dueling
* Great Weapon Fighting
* Protection
* Two-Weapon Fighting

The choice is mechanically validated.

### 9.2.4 Second Wind

Second Wind is a Bonus Action.

Healing:

**1d10 + Fighter Level**

The rules engine tracks:

* Uses
* Availability
* Healing result
* Recharge

### 9.2.5 Action Surge

At Fighter level 2, Action Surge grants an additional Action according to the supported rule.

The AI does not independently grant extra Actions.

### 9.2.6 Champion

Champion is the initial Fighter subclass.

Its V1 Improved Critical feature allows eligible attack rolls to critically hit on:

* Natural 19
* Natural 20

### 9.2.7 Ability Score Improvement

At Fighter level 4, the standard RealmWeaver ASI rules apply.

### 9.2.8 Extra Attack

At Fighter level 5:

```text
Attack Action
├── Attack 1
└── Attack 2
```

This does not grant a second unrestricted Action.

---

## 9.3 Rogue

### 9.3.1 Rogue Progression — Levels 1–5

| Level | Proficiency | Sneak Attack | Weapon Mastery Capacity | Features                                               |
| ----- | ----------: | -----------: | ----------------------: | ------------------------------------------------------ |
| 1     |          +2 |          1d6 |                       2 | Weapon Mastery, Expertise, Sneak Attack, Thieves' Cant |
| 2     |          +2 |          1d6 |                       2 | Cunning Action                                         |
| 3     |          +2 |          2d6 |                       2 | Thief                                                  |
| 4     |          +2 |          2d6 |                       2 | Ability Score Improvement                              |
| 5     |          +3 |          3d6 |                       2 | Uncanny Dodge                                          |

### 9.3.2 Weapon Mastery

Rogue gains Weapon Mastery at level 1.

Rogue Weapon Mastery Capacity for guaranteed V1 levels is:

| Rogue Level | Mastery Capacity |
| ----------: | ---------------: |
| 1           |                2 |
| 2           |                2 |
| 3           |                2 |
| 4           |                2 |
| 5           |                2 |

Rogue uses the shared Weapon Mastery framework defined under `9.1A Weapon Mastery Class Framework`.

The Rogue intentionally has narrower Weapon Mastery breadth than the Fighter.

This preserves the distinction:

```text
Fighter
→ broader weapon expertise

Rogue
→ narrower weapon specialization
```
Rogue does not gain an additional Mastery selection within guaranteed V1 levels 1–5.

Future higher-level Rogue progression may expand or modify Weapon Mastery only after explicit RealmWeaver design approval.

### 9.3.3 Sneak Attack

Sneak Attack is deterministic conditional damage.

Scaling:

| Rogue Level | Sneak Attack |
| ----------- | -----------: |
| 1           |          1d6 |
| 2           |          1d6 |
| 3           |          2d6 |
| 4           |          2d6 |
| 5           |          3d6 |

The engine checks:

* Eligible weapon
* Advantage
* Alternate nearby-enemy condition where supported
* Disadvantage
* Whether Sneak Attack has already been used during the relevant turn window

The AI does not need to remember to add Sneak Attack.

### 9.3.4 Expertise

Rogue receives Expertise according to class progression.

Selected eligible proficiencies receive doubled proficiency.

### 9.3.5 Thieves' Cant

Stored as an authoritative character feature.

It may affect:

* Criminal communication
* Signs
* Relevant narrative understanding

### 9.3.6 Cunning Action

At Rogue level 2, supported actions may be used as a Bonus Action:

* Dash
* Disengage
* Hide

### 9.3.7 Thief

Thief is the initial Rogue subclass.

Exact-position features must be adapted to RealmWeaver's distance-band system where necessary.

### 9.3.8 Ability Score Improvement

At Rogue level 4, the standard ASI rules apply.

### 9.3.9 Uncanny Dodge

At Rogue level 5, Uncanny Dodge provides an eligible Reaction to reduce incoming damage according to the supported rule.

---

## 9.4 Cleric

### 9.4.1 Cleric Progression — Levels 1–5

| Level | Proficiency | Cantrips | 1st-Level Slots | 2nd-Level Slots | 3rd-Level Slots | Main Features                    |
| ----- | ----------: | -------: | --------------: | --------------: | --------------: | -------------------------------- |
| 1     |          +2 |        3 |               2 |               — |               — | Spellcasting, Life Domain        |
| 2     |          +2 |        3 |               3 |               — |               — | Channel Divinity                 |
| 3     |          +2 |        3 |               4 |               2 |               — | 2nd-level spells                 |
| 4     |          +2 |        4 |               4 |               3 |               — | Ability Score Improvement        |
| 5     |          +3 |        4 |               4 |               3 |               2 | 3rd-level spells, Destroy Undead |

### Weapon Mastery

Cleric does not receive Weapon Mastery through normal V1 class progression.

Cleric weapon proficiency continues to function normally.

Future supported features such as a subclass, feat, special training or campaign reward may grant Weapon Mastery independently of the Cleric class.

### 9.4.2 Spellcasting Ability

Cleric uses Wisdom.

The rules engine calculates:

* Spell attack modifier
* Spell save DC
* Relevant preparation limits

### 9.4.3 Prepared Spells

Cleric uses prepared spellcasting.

Prepared spells and spell slots are separate concepts.

### 9.4.4 Spell Slots

Spell slots are deterministic resources.

The system tracks:

* Maximum slots
* Remaining slots
* Spell level

The AI cannot independently consume or restore them.

### 9.4.5 Life Domain

Life Domain is the initial Cleric subclass.

It provides healing/support-oriented mechanics and relevant Domain features.

### 9.4.6 Channel Divinity

Channel Divinity is a limited-use class resource.

The engine tracks:

* Uses
* Eligible effects
* Recharge
* Availability

### 9.4.7 Ability Score Improvement

At Cleric level 4, the standard ASI rules apply.

### 9.4.8 Destroy Undead

At Cleric level 5, Turn Undead gains the supported enhanced effect against qualifying undead.

---

## 9.5 Wizard

### 9.5.1 Wizard Progression — Levels 1–5

| Level | Proficiency | Cantrips | 1st-Level Slots | 2nd-Level Slots | 3rd-Level Slots | Main Features                 |
| ----- | ----------: | -------: | --------------: | --------------: | --------------: | ----------------------------- |
| 1     |          +2 |        3 |               2 |               — |               — | Spellcasting, Arcane Recovery |
| 2     |          +2 |        3 |               3 |               — |               — | School of Evocation           |
| 3     |          +2 |        3 |               4 |               2 |               — | 2nd-level spells              |
| 4     |          +2 |        4 |               4 |               3 |               — | Ability Score Improvement     |
| 5     |          +3 |        4 |               4 |               3 |               2 | 3rd-level spells              |

### Weapon Mastery

Wizard does not receive Weapon Mastery through normal V1 class progression.

Wizard weapon proficiency continues to function normally.

Future supported features such as a subclass, feat, special training or campaign reward may grant Weapon Mastery independently of the Wizard class.

### 9.5.2 Spellbook

Wizard uses a persistent Spellbook.

RealmWeaver distinguishes:

* Spellbook spells
* Prepared spells
* Cantrips
* Spell slots

These are separate concepts.

### 9.5.3 Prepared Spells

Wizard prepares eligible spells from the character's Spellbook.

Preparation limits depend on supported Wizard rules and Intelligence/level values.

### 9.5.4 Spell Slots

Maximum and remaining slots are tracked deterministically by spell level.

### 9.5.5 Arcane Recovery

Arcane Recovery restores limited expended spell-slot capacity according to the supported class rule.

The engine tracks:

* Eligibility
* Usage
* Recovery limit
* Recharge conditions

### 9.5.6 School of Evocation

School of Evocation is the initial Wizard subclass.

Its mechanics integrate with the generic spell and feature systems.

### 9.5.7 Ability Score Improvement

At Wizard level 4, the standard ASI rules apply.

---

## 9.6 Feature-System Requirements Identified by the Classes

### Status: ARCHITECTURAL REQUIREMENT

The future RealmWeaver feature engine must support at minimum:

### Passive Modifier

Example:

* Improved Critical

### Limited Resource

Examples:

* Second Wind
* Action Surge
* Channel Divinity

### Conditional Damage

Example:

* Sneak Attack

### Action Modification

Examples:

* Cunning Action
* Extra Attack

### Reaction

Example:

* Uncanny Dodge

### Spell Resource

Example:

* Spell Slots

### Resource Recovery

Example:

* Arcane Recovery

### Persistent Collection

Example:

* Wizard Spellbook

### Character Choice

Examples:

* Fighting Style
* Expertise
* ASI
* Subclass
* Spell selection

### Species Feature

Examples:

* Darkvision
* Lucky
* Fire Resistance
* Innate Magic

The exact software implementation is deferred to later architecture.

---

## 9.7 Spatial Rules Adaptation

### Status: APPROVED

Class or Species mechanics written around precise spatial positioning may be adapted to RealmWeaver's distance-band system.

Examples:

* Exact 5-foot relationships may map to Engaged.
* Grid-dependent movement may receive an equivalent abstract-distance interpretation.

Adaptations must preserve intended mechanical purpose where practical and must be documented.

---

# 10. Progress Visibility, Validation & Versioning

## 10.1 Progression Overview

### Status: APPROVED

The player should always be able to inspect:

* Current Level
* Current Class
* Current Subclass
* Species
* Proficiency Bonus
* Progression Mode
* XP or Milestone status
* Unlocked Features
* Level-Up Availability
* Pending Character Choices

---

## 10.2 Future-Level Preview

### Status: APPROVED

Players may inspect supported future class progression.

Character progression mechanics are not secret.

The UI should distinguish:

* Implemented content
* Planned or unsupported future content

---

## 10.3 Persistent Level-Up Notification

### Status: APPROVED

A postponed level-up remains available across:

* Page refresh
* Logout/login
* Session end
* Campaign reload

This comes from persisted state rather than AI memory.

---

## 10.4 Required-Choice Tracking

### Status: APPROVED

RealmWeaver explicitly tracks unresolved level-up choices.

Example:

```text
Level-Up Completion

✓ HP
✓ ASI
○ Spell Choice 1
○ Spell Choice 2
```

A level-up cannot commit until mandatory selections are valid.

---

## 10.5 Pre-Commit Validation

### Status: APPROVED

Before progression is applied, RealmWeaver validates:

* Current level
* Target level
* Progression eligibility
* Supported level cap
* Class
* Subclass
* Species progression
* Feature choices
* Ability Score limits
* Spell eligibility
* Required choice count
* Duplicate restrictions
* HP-roll validity

Frontend validation may improve usability.

Backend/domain validation remains authoritative.

---

## 10.6 Post-Commit Validation

### Status: APPROVED

After progression commits, the resulting character should be checked for internal consistency.

---

## 10.7 Character-State Invariants

### Status: APPROVED

Examples include:

```text
Current HP <= Maximum HP
```

```text
Level 5
→ Proficiency Bonus = +3
```

```text
Unsupported Feature
→ must not exist on character
```

These invariants should later be used in automated testing and runtime integrity checks.

---

## 10.8 Progression History

### Status: APPROVED

RealmWeaver maintains player-readable progression history.

Example:

```text
Level 3
- Reached 900 XP
- Sneak Attack increased to 2d6
- Selected Thief
```

Milestone campaigns record the relevant milestone instead of an XP threshold.

---

## 10.9 AI Progression Context

### Status: APPROVED

The AI receives relevant current progression state.

Example:

```text
Level: 4
Class: Rogue
Subclass: Thief
Species: Halfling

Features:
- Sneak Attack 2d6
- Cunning Action
- Lucky
```

The full progression table does not need to appear in every AI prompt.

---

## 10.10 Unsupported Feature Prevention

### Status: APPROVED

If the AI proposes an unavailable feature, RealmWeaver rejects it.

Examples:

* Level-4 Rogue cannot use Uncanny Dodge.
* Wizard cannot cast an unavailable spell.
* Character cannot spend a resource they do not possess.
* Non-Halfling cannot use Halfling Lucky.

---

## 10.11 Progression Failure Recovery

### Status: APPROVED

If a level-up transaction fails, the previous valid character state remains authoritative.

Pending progression state remains recoverable where possible.

---

## 10.12 Progression Integrity Validation

### Status: APPROVED

RealmWeaver should provide reusable validation such as:

```text
validate_character_progression(character)
```

Potential checks include:

* Correct level
* Correct Proficiency Bonus
* Valid class
* Valid subclass
* Valid Species traits
* Correct unlocked features
* Correct resource maxima
* Valid Ability Scores
* Correct spell-slot progression
* Supported level

It may run:

* After character creation
* After level-up
* After campaign load
* During migration
* During automated tests
* During recovery operations

---

## 10.13 Ruleset Versioning

### Status: APPROVED DIRECTION

Campaigns should eventually record:

```text
ruleset = RealmWeaver-SRD5.1
rules_version = 1.0
```

Specific later-SRD exceptions should also be traceable where needed.

Weapon Mastery is one such explicit RealmWeaver rules-source exception.

RealmWeaver selectively adopts Weapon Mastery from the revised rules while retaining SRD 5.1 / 2014-style mechanics as the primary baseline.

Weapon Mastery is always enabled within the RealmWeaver V1 ruleset and is not a per-campaign rules toggle.

Existing campaigns should not silently change mechanics when rules are updated.

Possible future migration strategies include:

* Remain on old rules
* Explicit migration
* Safe automatic migration

---

## 10.14 Content-Level Versioning

### Status: APPROVED DIRECTION

Supported content limits should come from rules/content configuration.

Example:

```text
supported_level_cap = 5
```

A later release may increase this without redesigning progression.

---

# 11. Rules Source Policy for Group 5

## 11.1 Primary Baseline

RealmWeaver V1 primarily follows:

**SRD 5.1 / 2014-style mechanics**

for its class and progression baseline.

---

## 11.2 Explicit Later-SRD Exceptions

Later CC BY SRD material may be used where intentionally selected and documented.

Goliath is currently the clearest example.

RealmWeaver must not silently mix mechanics from different rules versions.

Any exception should identify:

* Source rules version
* Reason for inclusion
* RealmWeaver adaptation
* Interaction with the primary rules baseline

---

## 11.3 RealmWeaver Adaptation

Rules may be simplified where required for:

* Solo play
* Distance-band combat
* AI reliability
* V1 scope
* Mechanical clarity

Adaptations must remain explicit.

---

# 12. Group 5 Status

**Group 5 — Classes & Progression: APPROVED**

Major approved topics:

1. Class Scope & Core Class Model
2. Species
3. Backgrounds
4. Level-Up Structure
5. XP Progression
6. Milestone Progression & Adventure Leads
7. Subclasses & Character Choices
8. Core Class Progression
9. Progress Visibility, Validation & Versioning

Next Rules Group:

> **Group 6 — Equipment & Inventory**
