# RealmWeaver — V1 Game Rules

**Document Type:** Rules Index
**Status:** M2.1 In Progress
**Milestone:** M2 — Technical Design & Architecture
**Primary Rules Baseline:** SRD 5.1 / 2014-style mechanics with explicitly documented RealmWeaver adaptations
**Last Reviewed:** 13 August 2026

---

# 1. Purpose

This document is the entry point for RealmWeaver's V1 game-rules specification.

Detailed rules are divided into domain-specific documents so that the ruleset remains:

* Easy to navigate
* Easy to review
* Easy to maintain
* Easy to reference from architecture and implementation
* Less prone to accidental duplication or contradiction

This file is intentionally concise.

The detailed group specification is authoritative for its respective rules domain.

---

# 2. Core Rules Philosophy

RealmWeaver is a single-player, AI-powered fantasy tabletop RPG system built around deterministic game mechanics and persistent campaign state.

The core principle is:

> **AI tells the story. Rules decide what happens.**

A second governing principle is:

> **AI proposes. RealmWeaver validates.**

The AI Dungeon Master may:

* Interpret player intent
* Generate narration
* Control dialogue
* Suggest mechanically relevant actions
* Propose checks
* Propose encounters
* Propose rewards
* Assist with character-building decisions

The deterministic RealmWeaver systems remain authoritative over:

* Dice
* Modifiers
* Checks
* Saving Throws
* Combat legality
* Hit Points
* Armour Class
* Damage
* Inventory
* Currency
* Character features
* Progression
* Quest state
* Persistent world state
* Other objective game mechanics

---

# 3. Rules Baseline

## 3.1 Primary Baseline

RealmWeaver V1 primarily follows:

**SRD 5.1 / 2014-style mechanics**

for its core rules and progression model.

RealmWeaver is not intended to provide complete D&D 5e compatibility in V1.

It is a D&D-inspired ruleset adapted for reliable single-player AI-driven gameplay.

---

## 3.2 RealmWeaver Adaptations

Rules may be deliberately simplified or adapted for:

* Solo gameplay
* Text-first interaction
* Distance-band combat
* AI reliability
* Reduced V1 complexity
* Mechanical clarity

Examples include:

* Abstract distance bands instead of a full battle grid
* Controlled DC categories instead of arbitrary AI-generated DCs
* Simplified initial class/subclass scope
* Limited initial reaction support

Adaptations must be explicitly documented.

RealmWeaver should not silently change a rule and still present it as the unchanged baseline rule.

---

## 3.3 Later-SRD Exceptions

Specific mechanics from later appropriately licensed SRD material may be used when intentionally selected.

Such exceptions should document:

* Source rules version
* Reason for inclusion
* RealmWeaver adaptation
* Interaction with the primary baseline

Goliath is currently an example of a Species requiring an explicit later-SRD source decision.

---

# 4. Rules Specifications

## 4.1 Group 1 — Character Core

**Status:** APPROVED

Specification:

[01_CHARACTER_CORE.md](./01_CHARACTER_CORE.md)

Covers:

* Ability Scores
* Ability Score Generation
* Ability Modifiers
* Skills
* Alternative Ability + Skill combinations
* Proficiency
* Expertise
* Hit Points
* Armour Class
* Speed
* Character Level Support

---

## 4.2 Group 2 — Checks & Saving Throws

**Status:** APPROVED

Specification:

[02_CHECKS_AND_SAVES.md](./02_CHECKS_AND_SAVES.md)

Covers:

* Automatic Success
* Mechanical Checks
* Impossible Actions
* Ability Checks
* Difficulty Classes
* Saving Throws
* Contested Checks
* Passive Checks
* Natural 20 / Natural 1 Ability Check behaviour
* Narrative degrees of success and failure
* Free-text action resolution

---

## 4.3 Group 3 — Dice & Inspiration

**Status:** APPROVED

Specification:

[03_DICE_AND_INSPIRATION.md](./03_DICE_AND_INSPIRATION.md)

Covers:

* Supported Dice
* Dice Expressions
* Automatic Dice
* Manual Physical Dice
* Manual Input Validation
* Randomness
* Advantage
* Disadvantage
* Rerolls
* Hidden Rolls
* Dice History
* Inspiration
* Dice Authority

---

## 4.4 Group 4 — Combat

**Status:** APPROVED

Specification:

[04_COMBAT.md](./04_COMBAT.md)

Covers:

* Combat Start
* Initiative
* Turn Structure
* Action Economy
* Reactions
* Distance Bands
* Movement
* Opportunity Attacks
* Natural-Language Combat Actions
* Attack Resolution
* Armour Class
* Damage
* Damage Types
* Resistance
* Vulnerability
* Immunity
* Critical Hits
* Critical Misses
* Healing
* Temporary HP
* Unconsciousness
* Death Saving Throws
* Permanent Death
* Enemy Combat AI
* Morale
* Combat End Conditions
* Encounter Finalisation
* Progression Rewards
* Contextual Loot
* Quest Consequences
* World Consequences
* Encumbrance Campaign Setting
* Reusable Content-Pool Direction
* Encounter Persistence
* AI Combat Authority Boundary

---

## 4.5 Group 5 — Classes & Progression

**Status:** APPROVED

Specification:

[05_CLASSES_AND_PROGRESSION.md](./05_CLASSES_AND_PROGRESSION.md)

Covers:

* V1 Class Scope
* Class Data Model
* Hit Dice
* Class Features
* Proficiency Progression
* Species
* Species Traits
* Backgrounds
* Level-Up Structure
* XP Progression
* Milestone Progression
* Adventure Leads
* Subclasses
* Character Choices
* Fighter Progression
* Rogue Progression
* Cleric Progression
* Wizard Progression
* Progress Visibility
* Progression Validation
* Ruleset Versioning

---
## 4.6 Group 6 — Equipment & Inventory

**Status:** APPROVED

Specification:

[06_EQUIPMENT_AND_INVENTORY.md](./06_EQUIPMENT_AND_INVENTORY.md)

Covers:

- Item Definitions & Item Instances
- Item Ownership & Location
- Currency & Wealth
- Weapons
- Two-Weapon Fighting
- Armour & Shields
- Inventory & Equipment State
- Containers & Persistent Storage
- Carrying Capacity
- Optional Encumbrance
- Consumables & Item Use
- Unidentified Items
- Loot & Treasure
- Hidden Containers & Discovery
- Merchant Stock
- Buying & Selling
- Haggling
- Bartering
- Item State & Validation
- Tools & Tool Proficiencies
- Flexible Ability + Tool Checks
- Future Crafting Architecture

---

## 4.7 Group 7 — Magic

**Status:** NOT STARTED

Planned specification:

`07_MAGIC.md`

Expected topics include:

* Spellcasting
* Spell Slots
* Prepared Spells
* Known Spells
* Spellbook Rules
* Cantrips
* Spell Components
* Concentration
* Spell Range
* Spell Targeting
* Spell Attacks
* Saving Throw Spells
* Spell Damage
* Healing Spells
* Ritual Casting
* Upcasting
* Innate Species Magic

---

## 4.8 Group 8 — Conditions & Resting

**Status:** NOT STARTED

Planned specification:

`08_CONDITIONS_AND_RESTING.md`

Expected topics include:

* Conditions
* Condition Duration
* Short Rest
* Long Rest
* Hit Dice Recovery
* Class Resource Recovery
* Spell-Slot Recovery
* Species Rest Traits
* Stabilisation Recovery
* Exhaustion
* Rest Restrictions
* Interrupted Rest

---

## 4.9 Group 9 — AI / Rules Boundary

**Status:** NOT STARTED

Planned specification:

`09_AI_RULES_BOUNDARY.md`

Expected topics include:

* AI Authority
* Rules-Engine Authority
* Player Intent Interpretation
* Structured Action Proposals
* Mechanical Validation
* Tool / Service Boundaries
* Progression Events
* Quest Events
* Discovery Events
* World-State Changes
* Invalid AI Output
* Deterministic Fallback
* Error Recovery
* AI Failure Safety
* State Integrity

---

# 5. V1 Level-Support Strategy

RealmWeaver uses three levels of progression support.

## 5.1 Minimum V1 Support

Fully playable and tested:

**Levels 1–5**

---

## 5.2 V1 Stretch Target

If schedule, complexity and testing permit:

**Levels 1–10**

Additional levels must not delay the reliable core V1 release unnecessarily.

---

## 5.3 Architecture Target

The underlying architecture should support eventual expansion toward:

**Levels 1–20**

Architecture support does not mean all levels are initially playable.

---

# 6. Rules Content Strategy

RealmWeaver should avoid hard-coding large catalogues of static content when appropriately licensed structured sources are available.

Potential reusable rules content includes:

* Classes
* Species
* Subclasses
* Monsters
* Weapons
* Armour
* Equipment
* Spells
* Conditions
* Other static game data

The intended high-level pipeline is:

```text id="kx01pr"
Licensed / Permitted Source
        ↓
Importer
        ↓
Validation
        ↓
Normalisation
        ↓
RealmWeaver Internal Format
        ↓
Local / Database Seed Data
```

External rules APIs should not become mandatory runtime dependencies for ordinary gameplay.

Rules logic remains under RealmWeaver's deterministic systems.

---

# 7. World-Building Content Strategy

RealmWeaver may also maintain reusable content pools for:

* Character names
* Settlement names
* Taverns
* Dungeons
* Wilderness locations
* Factions
* Item names
* Cultural/regional naming groups

Such content should come from:

* Appropriately licensed sources
* Public-domain sources
* Other permitted material

Campaign state should track used identities where useful to reduce unwanted repetition.

Detailed architecture is deferred to later M2 work.

---

# 8. Source-of-Truth Policy

RealmWeaver documentation uses the following authority order:

1. **Current detailed rules-group specification**
2. **Applicable Architecture Decision Record**
3. **`GAME_RULES.md`**
4. **`PROJECT_STATUS.md`**
5. **Conversation history / assistant memory**

If a summary, previous conversation or status document conflicts with a current approved detailed rules specification, the detailed specification takes precedence unless the rule is intentionally reviewed and changed.

---

# 9. Rule Change Policy

Approved rules may still change if a later design review identifies:

* A contradiction
* An implementation problem
* A balance problem
* An unclear rule
* A licensing issue
* A better architecture-compatible design

However, changes must be intentional.

A significant rule change should:

1. Identify the existing rule.
2. Explain why it is changing.
3. Define the replacement.
4. Check cross-group consequences.
5. Update all affected specifications.
6. Record an ADR where the decision is architecturally significant.
7. Preserve history through Git.

---

# 10. Cross-Group Consistency

Rules groups must not be treated as isolated systems.

Examples:

* Species traits may modify dice rules.
* Class features may modify combat.
* Equipment may modify Armour Class.
* Magic may apply Conditions.
* Rest may restore class resources.
* Combat may trigger XP or Milestone progress.
* AI proposals must respect every authoritative rules domain.

Before M2.1 is considered complete, all nine rules groups require a consistency review.

---

# 11. Documentation Policy

Rules documentation should be updated incrementally throughout M2.1.

When a rules group is completed:

1. Update or create its detailed specification.
2. Update this index.
3. Review cross-group references.
4. Record deferred mechanics.
5. Check terminology consistency.
6. Commit an appropriate documentation checkpoint.

The project should not rely on conversation history as the only record of an approved rule.

---

# 12. Current M2.1 Status

## Approved

* Group 1 — Character Core
* Group 2 — Checks & Saving Throws
* Group 3 — Dice & Inspiration
* Group 4 — Combat
* Group 5 — Classes & Progression
* Group 6 — Equipment & Inventory

## Not Started

* Group 7 — Magic
* Group 8 — Conditions & Resting
* Group 9 — AI / Rules Boundary

---

# 13. M2.1 Completion Gate

M2.1 — V1 Game Rules Specification is complete only when:

* Groups 1–9 have approved specifications.
* Cross-group contradictions have been reviewed.
* Major V1 simplifications are documented.
* AI authority boundaries are explicitly defined.
* Core rules are implementable without relying on AI memory.
* Unsupported/deferred mechanics are clearly identified.
* Rules-source/licensing decisions are documented.
* The rules specification is sufficiently stable to support system architecture and domain modelling.

---

# 14. Next Design Activity

Continue with:

> **Group 7 — Magic**

Planned file:

`07_MAGIC.md`
