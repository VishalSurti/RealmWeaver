# RealmWeaver — V1 Game Rules

**Document Type:** Rules Index
**Status:** M2.1 Rules Design Complete — Group 9 Internal Review Passed — Completion Gate Pending
**Milestone:** M2 — Technical Design & Architecture
**Primary Rules Baseline:** SRD 5.1 / 2014-style mechanics with explicitly documented RealmWeaver adaptations
**Last Reviewed:** 31 August 2026

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

```markdown
A third governing principle established by the completed AI/rules boundary is:

> **AI may remember context. RealmWeaver preserves truth.**

The player owns meaningful player-character intent and decisions.

AI may interpret intent, choose behaviour for AI-controlled actors, propose mechanics/content and narrate outcomes.

RealmWeaver remains authoritative over rules, controlled randomness, mechanical resolution, persistent campaign state, world canon, knowledge boundaries and long-term campaign continuity.
```


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
* Selective adoption of revised-rules Weapon Mastery while retaining SRD 5.1 / 2014-style mechanics as the primary baseline
* Configurable Full / Simplified spell-component handling
* Persistent campaign-time requirements
* Distance-band adaptations for spell areas and Conditions
* Context-sensitive probabilistic rest interruptions
* Limited environmental-hazard / countermeasure system

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

Weapon Mastery is an approved later-rules exception for RealmWeaver V1.

RealmWeaver selectively adopts Weapon Mastery to increase tactical weapon differentiation and martial character depth while retaining SRD 5.1 / 2014-style mechanics as the primary rules baseline.

Weapon Mastery is always enabled and is not a campaign-builder toggle.

The approved Weapon Mastery system includes:

* Cleave
* Graze
* Nick
* Push
* Sap
* Slow
* Topple
* Vex

Weapon Mastery class access and progression are defined in `05_CLASSES_AND_PROGRESSION.md`.

Weapon definitions, Mastery mappings and equipment-state interaction are defined in `06_EQUIPMENT_AND_INVENTORY.md`.

Weapon Mastery combat resolution is defined in `04_COMBAT.md`.

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
* Weapon Mastery Combat Resolution
* Weapon Mastery Temporary Effects
* Weapon Mastery Forced Movement
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
* Weapon Mastery Class Access
* Weapon Mastery Capacity & Progression
* Weapon Mastery Selection & Reselection
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
- Weapon Mastery Definitions & Mapping
- Weapon Mastery / Equipment Interaction
- Two-Weapon Fighting
- Armour & Shields
- Inventory, Equipped & Wielded State
- Hand & Wield State
- Weapon Drawing & Switching
- Dropped & Thrown Weapon State
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

Status: APPROVED

Specification:

`07_MAGIC.md`

Covers:

* Structured Spell Definitions
* Spell Access Sources
* Known / Acquired / Prepared / Castable State
* Wizard Spellbooks
* Cleric Preparation
* Cantrips
* Spell Slots
* Upcasting
* Arcane Recovery
* Casting Time
* Actions / Bonus Actions / Reactions
* Extended Casting
* Range
* Targeting
* Areas of Effect
* Spell Attack Rolls
* Saving Throw Spells
* Spell Save DC
* Damage
* Healing
* Verbal / Somatic / Material Components
* Campaign-Level Component Setting
* Concentration
* Ritual Casting
* Innate Species Magic
* Persistent Spell Effects
* Spell Provenance
* AI / Rules Authority for Spellcasting
* Deterministic Spell Validation and Resolution

---
## 4.8 Group 8 — Conditions & Resting

Status: APPROVED

Specification:

`08_CONDITIONS_AND_RESTING.md`

Covers:

* Structured Condition Instances
* Standard Conditions
* Multiple Condition Sources
* Condition Duration
* Environmental Hazards
* Environmental Countermeasures
* Exhaustion
* Death / Stabilisation Interactions
* Short Rest
* Long Rest
* Hit Dice Recovery
* Class Resource Recovery
* Spell-Slot Recovery
* Species Rest Traits
* Rest Restrictions
* Interrupted Rest
* Persistent NPC Recovery
* Authoritative World-Time Advancement
* Rest Event History
* Condition / Recovery Provenance
* Idempotent Recovery Behaviour

---

## 4.9 Group 9 — AI / Rules Boundary

Section Status: APPROVED

Rules Design Status: COMPLETE

Internal Review Status: PASSED

Internal Review Gate: PASSED

M2.1 Completion Gate: PENDING

Specification:

[09_AI_RULES_BOUNDARY.md](./09_AI_RULES_BOUNDARY.md)

Covers:

### Authority & Intent

* Authority Model
* Player Intent Interpretation
* AI Mechanical Proposals
* Validation & Rejection

### Resolution & Narration

* Mechanical Resolution Pipeline
* Commit-Before-Narration Contract
* AI Narration Boundary
* Structured Mechanical Results
* Player Agency
* Deterministic Consequence Handling

### NPC & World Authority

* NPC AI Authority
* NPC Knowledge Boundaries
* Persistent NPC State
* Selective Off-Screen NPC Progression
* World & Content Proposals
* Incidental / Scene-Relevant / Persistent Content
* Locations
* Factions
* Quests
* Items
* Lore
* Dynamic World State
* World-Generation Profile Constraints

### Knowledge & Memory

* Objective World Truth
* Player-Character Knowledge
* NPC Knowledge
* AI / DM Context
* Fact / Rumour / Myth / Belief / Propaganda Distinctions
* Hidden Information
* Partial Discovery
* Context Assembly
* Memory Categories
* Summarisation
* Entity-Linked Retrieval
* Provider-Independent Campaign Memory
* Session-Independent Campaign Continuity

### Reliability

* Failure Categories
* Coherent State Commitment
* Idempotency
* Duplicate Request Protection
* Bounded AI Retries
* Retry-Safe Randomness
* No Free Rerolls from Technical Failure
* Randomness Bound to Validated Action Identity
* Safe Fallback Behaviour
* Stale-State Revalidation
* Narration Regeneration
* Save Integrity
* World-Event Failure Recovery

### Latency & UX

* Narrative-First Gameplay
* Inspectable Mechanics
* Local Deterministic Resolution
* Minimum Necessary AI Calls
* Structured UI Bypassing Intent Interpretation Where Appropriate
* Hidden and Visible Dice UX
* Direct Structured State Inspection
* Campaign Resume and Recap
* Player Agency
* Correctness Before Latency
* Task-Proportional Context and Processing

### Visual Quality

RealmWeaver treats visual quality as a first-class product requirement.

Visual quality is mandatory for the core V1 player experience.

The UI should:

* Present a distinctive RealmWeaver fantasy identity
* Use a coherent reusable design system
* Avoid appearing as a generic AI chatbot or unmodified UI-library application
* Preserve readability and interaction clarity
* Use purposeful animation
* Elevate important gameplay/story moments visually
* Remain responsive, accessible and performant
* Permit suitable UI/component/animation libraries and development accelerators
* Require explicit visual/UX review for important player-facing features

Elaborate animations, custom artwork for every entity, 3D environments, fully animated maps, generated voice/video, multiple complete visual themes, Dark Mode and purely decorative effects remain optional or deferred.

The governing visual principle is:

> **The interface should make the player want to enter the world.**

### Final Group 9 Contract

> **Player chooses. AI interprets, proposes, decides for AI-controlled actors and narrates. RealmWeaver validates, resolves, commits and remembers.**

The complete lifecycle is:

```text
INTENT
        ↓
PROPOSE
        ↓
VALIDATE
        ↓
RESOLVE
        ↓
COMMIT / PERSIST
        ↓
NARRATE
        ↓
CONTINUE WORLD
```

Persistent consequences cannot bypass RealmWeaver's authoritative state model.

Resolve calculates the complete state change without making it authoritative. Commit/persist applies atomic durable persistence. Narration describes only successfully committed outcomes.

Significant persistent content must be proposed, validated, materialised and committed/persisted before presentation as established reality. Rumours and other uncertain claims remain distinct from objective world truth.

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
* Weapon Mastery definitions and mappings
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

Cross-group amendments such as Weapon Mastery must be reflected in every affected detailed specification before the amendment is considered fully incorporated into the authoritative ruleset.

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
* Weapon Mastery access originates from character features and progression.
* Weapon Mastery availability depends on weapon and equipment state.
* Weapon Mastery effects may modify combat, movement, action economy and temporary effect state.
* Temporary effects modify effective state without overwriting authoritative base statistics.

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

## Rules Design

All required M2.1 rules-design sections are APPROVED:

* Group 1 — Character Core
* Group 2 — Checks & Saving Throws
* Group 3 — Dice & Inspiration
* Group 4 — Combat
* Group 5 — Classes & Progression
* Group 6 — Equipment & Inventory
* Group 7 — Magic
* Group 8 — Conditions & Resting
* Group 9 — AI / Rules Boundary

The primary M2.1 rules-design stage is complete. Group 9's internal consistency review and internal-review gate passed on 31 August 2026.

## Current Phase

**GROUP 9 INTERNAL REVIEW PASSED — CROSS-GROUP REVIEW NEXT — M2.1 GATE PENDING**

Remaining M2.1 work:

1. Full Groups 1–9 cross-group consistency review.
2. Terminology and cross-reference review.
3. Weapon Mastery cross-group verification.
4. V1 scope and deferred-mechanics review.
5. SRD/IP/content-provenance audit.
6. Required specification corrections.
7. Final M2.1 completion gate.

M2.1 must not be marked COMPLETE until these review activities pass.

---
# 13. M2.1 Completion Gate

M2.1 — V1 Game Rules Specification is complete only when:

* Groups 1–9 have approved specifications.
* Group 9 has passed internal consistency review.
* All nine groups have passed cross-group consistency review.
* Cross-group contradictions and ambiguous interactions have been resolved.
* Major V1 simplifications and adaptations are documented.
* Weapon Mastery amendments are consistent across affected files.
* AI authority boundaries are explicitly and consistently defined.
* Core rules are implementable without relying on AI memory.
* Player agency is consistently protected.
* NPC decisions respect mechanical and information boundaries.
* Persistent world facts cannot be created through narration alone.
* Knowledge boundaries distinguish world truth, actor knowledge and AI/DM context.
* Retry/idempotency rules prevent duplicate effects and free rerolls.
* Unsupported and deferred mechanics are clearly identified.
* Rules-source/licensing decisions have been reviewed.
* Mechanically defined content has appropriate provenance where required.
* Unknown or unsupported content sources required for implementation are resolved.
* The specification is sufficiently stable to support system architecture and domain modelling.
* `PROJECT_STATUS.md` reflects the final M2.1 state.
* The completed M2.1 documentation baseline is committed and pushed.

Current gate status:

> **NOT YET PASSED — RULES DESIGN COMPLETE, GROUP 9 INTERNAL REVIEW PASSED, CROSS-GROUP REVIEW PENDING**

---
# 14. Next Design Activity

Do not proceed directly to M2.2 yet.

Continue with:

> **M2.1 — Groups 1–9 Cross-Group Consistency Review**

Then:

```text
Groups 1–9 Cross-Group Review
        ↓
V1 / Deferred Mechanics Review
        ↓
SRD / IP / Content-Provenance Audit
        ↓
Documentation Corrections
        ↓
M2.1 Final Gate
        ↓
M2.2 — System Architecture
```

The next major architecture activity after the M2.1 gate passes will be:

> **M2.2 — System Architecture**
