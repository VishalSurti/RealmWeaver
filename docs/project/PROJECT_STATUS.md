# RealmWeaver — Project Status

**Document Version:** 1.3
**Last Reviewed:** 22 August 2026
**Overall Status:** Active
**Current Milestone:** M2 — Technical Design & Architecture
**Milestone Status:** IN PROGRESS
**Current Activity:** M2.1 — V1 Game Rules Specification & Rules-Engine Boundary
**Next Activity:** Group 6 Amendment — Weapon Mastery, then M2.1 Group 9 — AI / Rules Boundary
**Current Progress:** Groups 1–8 APPROVED and documented


---

# 1. Current Project State

RealmWeaver has completed **Milestone 1 — Product Foundation** and has formally entered **Milestone 2 — Technical Design & Architecture**.

The product vision, V1 scope, requirements, user stories, acceptance criteria, product backlog, Definition of Done, risk register, competitive/reference analysis, and project-management documentation were established during M1.

M2 is now defining how RealmWeaver V1 will function technically before production implementation begins.

The first M2 activity, **M2.1 — V1 Game Rules Specification & Rules-Engine Boundary**, is currently in progress.

The following M2.1 rules groups have been approved:

* Group 1 — Character Core
* Group 2 — Checks, DCs & Saving Throws
* Group 3 — Dice & Inspiration
* Group 4 — Combat
* Group 5 — Classes & Progression
* Group 6 — Equipment & Inventory
* Group 7 - Magic
* Group 8 — Conditions & Resting

The next activity is:

> **Group 6 Amendment — Weapon Mastery**

After the Weapon Mastery amendment is documented, continue to:

> **M2.1 Group 9 — AI / Rules Boundary**

No production application code has been introduced yet.

The current focus remains technical specification and architecture rather than implementation.

---

# 2. Milestone 1 — Completion Summary

## Status

**COMPLETE — Gate Passed**

Milestone 1 established what RealmWeaver is, what V1 must accomplish, and what is explicitly outside V1 scope.

## Completed Work

* Product Vision
* Unique Value Proposition
* Reference & Competitive Analysis
* V1 Player Experience
* MoSCoW Scope
* Functional Requirements
* Non-Functional Requirements
* User Stories
* Acceptance Criteria
* Product Backlog
* Definition of Done
* Risk Register
* Project Status framework
* Git repository initialisation
* `.gitignore` configuration
* Initial documentation commit
* GitHub remote configuration
* Initial push to GitHub

## M1 Git Baseline

Commit:

`16fc61b — docs: complete M1 product foundation documentation`

This commit represents the initial approved RealmWeaver product-foundation baseline.

---

# 3. Milestone 2 — Current Progress

## Status

**IN PROGRESS**

The purpose of M2 is to determine how RealmWeaver V1 will actually be built before significant production coding begins.

Planned M2 areas include:

* M2.1 — V1 Game Rules Specification & Rules-Engine Boundary
* M2.2 — System Architecture
* M2.3 — Technology Stack Confirmation
* M2.4 — Domain Model
* M2.5 — Database Design
* M2.6 — AI Architecture
* M2.7 — Memory Architecture
* M2.8 — API Design
* M2.9 — Application / Repository Structure
* M2.10 — Testing Architecture
* M2.11 — Development Environment
* M2.12 — Architecture Review and Go/No-Go for implementation

These areas may be refined, combined or reordered where technically appropriate.

No major production implementation should begin until the relevant architectural foundations have been sufficiently defined and reviewed.

---

# 4. M2.1 — Game Rules Specification

## Status

**IN PROGRESS**

Authoritative specification:

`docs/game-rules/GAME_RULES.md`

The purpose of M2.1 is to determine:

* Which tabletop mechanics RealmWeaver V1 supports
* Which mechanics are intentionally simplified
* Which rules are explicitly excluded from V1
* Which decisions belong to deterministic code
* Which decisions belong to the AI Dungeon Master
* How player intent becomes structured mechanical actions
* How dice and rules outcomes are resolved
* How authoritative outcomes are returned to the AI for narration

## Approved Groups

### Group 1 — Character Core

**APPROVED**

Includes:

* Six core ability scores
* Standard Array and Rolled Stats
* Skill structure
* Alternative ability + skill combinations
* Proficiency
* Expertise
* HP
* AC
* Speed
* Passive skills
* Level-support strategy

### Group 2 — Checks, DCs & Saving Throws

**APPROVED**

Includes:

* Automatic Success / Mechanical Check / Impossible resolution
* Controlled DC bands
* Ability checks
* Saving throws
* Contested checks
* Passive checks
* Narrative degrees of success/failure
* AI interpretation with deterministic resolution

### Group 3 — Dice & Inspiration

**APPROVED**

Includes:

* Standard RPG dice
* Multiple-dice expressions
* Automatic dice
* Manual physical dice entry
* Advantage / Disadvantage
* Hidden rolls
* Dice history
* Inspiration
* Inspiration declared before the roll
* Unbiased system-generated dice

### Group 4 — Combat

**APPROVED**

Includes:

* Combat initiation
* Initiative
* Action economy
* Reactions
* Abstract distance-band positioning
* Movement actions
* Opportunity attacks
* Complex natural-language combat actions
* Attack rolls
* Hidden enemy AC
* Damage
* Damage types
* Resistances, vulnerabilities and immunities
* Critical hits
* Natural 1 attack misses
* Future critical-fumble / enhanced-critical expansion
* HP and healing
* Unconsciousness
* Death saves
* Massive damage
* Solo-play defeat outcomes
* Persistent character death
* Enemy tactical profiles
* Hybrid deterministic / AI-assisted enemy tactics
* Morale
* Combat finalisation
* Encounter objectives
* Contextual loot
* Quest updates
* Persistent NPC/world consequences
* Reward validation
* Encumbrance campaign setting
* Reusable content-pool direction

### Group 5 — Classes & Progression

**APPROVED**

Includes:

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

### Group 6 — Equipment & Inventory

**APPROVED**

Includes:

- Structured Item Definitions and persistent Item Instances
- Item ownership, location and transfer
- Currency: CP, SP, GP and PP
- Electrum excluded from V1
- Weapons and weapon properties
- Basic two-weapon fighting
- Ammunition tracking
- Armour and shields
- Non-proficient armour penalties
- Alternative AC architecture
- Equipment and hand state
- Containers and persistent storage
- Optional campaign Encumbrance
- Graduated Encumbrance thresholds
- Coin weight when Encumbrance is enabled
- Consumables
- Unidentified consumables
- Loot profiles and persistent loot
- Hidden containers and discoveries
- Merchant stock
- Buying and selling
- Haggling and basic bartering
- Theft / pickpocket integration
- Item-state validation
- Tools and Tool Proficiencies
- Flexible Ability + Tool checks
- Full crafting deferred from V1

### Group 7 — Magic

**APPROVED**

Includes:

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

### Group 8 — Conditions & Resting

**APPROVED**

Includes:

* Structured Condition Definitions and persistent Condition Instances
* Standard SRD 5.1 / 2014-style Conditions
* Condition sources, durations and removal rules
* Condition immunity
* Hidden condition / information visibility support
* Condition persistence across scenes and save/reload
* Distance-band adaptations for condition mechanics
* Six-level SRD 5.1 / 2014 Exhaustion system
* Environmental hazards and validated countermeasures
* Short Rest rules
* Hit Dice spending and recovery
* Long Rest rules
* Long Rest HP / spell-slot / resource recovery
* Exhaustion recovery
* Rest safety and preparation
* Watches
* Probabilistic, context-sensitive rest interruptions
* Persistent authoritative campaign-time requirements
* World-event scheduling requirements
* NPC rest / recovery persistence
* AI / rules authority for Conditions and Resting
* Mechanical event history and state-change provenance
* Fast deterministic validation without unnecessary LLM calls

## Remaining M2.1 Groups

* Group 6 Amendment — Weapon Mastery
* Group 9 — AI / Rules Boundary
* Final M2.1 cross-group consistency review

---

# 5. Core Product Definition

RealmWeaver is a:

> **Single-player, text-first, AI-powered fantasy tabletop RPG system with deterministic game mechanics and persistent campaign state.**

Core engineering principle:

> **AI controls the narrative. The game engine controls the truth.**

Product positioning:

> **RealmWeaver is a transparent, rules-driven AI Dungeon Master where the AI controls the story but never the truth of the game.**

Product tagline:

> **AI tells the story. Rules decide what happens.**

A second implementation principle established during M2 is:

> **AI proposes. RealmWeaver validates.**

The AI may interpret intent, generate narrative content and propose actions.

The deterministic application remains authoritative over mechanics and persistent state.

---

# 6. V1 Product Priorities

RealmWeaver V1 currently prioritises:

1. Character System
2. Dice and Ability Resolution
3. Authoritative Game State
4. Campaign Management
5. AI Dungeon Master
6. Combat System
7. Inventory and Equipment
8. Quest and Goal Tracking
9. Character Progression
10. NPC Persistence
11. Exploration and World State
12. Campaign Memory
13. Campaign Interface
14. Testing and AI Evaluation

The project will prioritise reliable core gameplay over feature quantity.

---

# 7. V1 Ruleset Direction

RealmWeaver V1 uses a **D&D-inspired fantasy tabletop ruleset closely aligned with familiar 5e-style mechanics**, while deliberately simplifying systems where full tabletop fidelity would create disproportionate complexity for an AI-driven solo RPG.

RealmWeaver does not currently promise full 5e compatibility.

The approved level-support strategy is:

### V1 Minimum

Fully supported playable content:

**Levels 1–5**

### V1 Stretch Target

If testing, schedule and complexity permit:

**Levels 1–10**

The stretch target must not delay the core V1 unnecessarily.

### Architecture Target

The data model and rules architecture should support eventual expansion toward:

**Levels 1–20**

---

# 8. Current Documentation

## Product / Project Documentation

Located primarily under:

`docs/project/`

Current documents include:

* `PROJECT_VISION.md`
* `REFERENCE_ANALYSIS.md`
* `V1_SCOPE.md`
* `REQUIREMENTS.md`
* `USER_STORIES.md`
* `PRODUCT_BACKLOG.md`
* `DEFINITION_OF_DONE.md`
* `RISK_REGISTER.md`
* `PROJECT_STATUS.md`

## M2 Technical Documentation

Current:

* `docs/game-rules/GAME_RULES.md`

Planned technical documentation may include:

* `docs/architecture/SYSTEM_ARCHITECTURE.md`
* `docs/architecture/DATABASE_DESIGN.md`
* `docs/architecture/AI_ARCHITECTURE.md`
* `docs/architecture/MEMORY_ARCHITECTURE.md`
* `docs/architecture/API_DESIGN.md`

Architecture Decision Records may be stored under:

`docs/adr/`

These files should be created progressively as their related decisions are made rather than all at once.

---

# 9. Source-of-Truth Policy

RealmWeaver repository documentation is the authoritative source for approved project decisions.

If an older conversation, memory, assumption or suggestion conflicts with an approved and subsequently updated repository document:

> **The current approved repository documentation takes precedence.**

Conversation history and assistant memory may support development, but they are not substitutes for repository documentation.

Before making decisions materially dependent on previous RealmWeaver specifications, the relevant repository documents should be reviewed rather than relying solely on memory.

Major architectural or rules changes may additionally require an Architecture Decision Record.

Git history should preserve previous approved states.

---

# 10. Current Major Risks

Highest-priority risks currently identified include:

* Scope creep
* AI mechanical hallucination
* Long-term campaign-memory inconsistency
* Game-rule complexity
* Rule-interaction defects
* AI/game-engine integration complexity
* Solo-development workload

M2 should reduce these risks by clearly defining:

* Rules boundaries
* State ownership
* AI responsibilities
* Data architecture
* Validation boundaries
* Testing strategy

See `RISK_REGISTER.md` for the complete register and mitigation strategies.

---

# 11. Current Constraints

RealmWeaver V1 is:

* Developed by one developer
* Primarily intended for personal use
* A professional software-engineering learning project
* Single-player
* Text-first
* Low-budget
* Designed around API-based AI rather than heavy local inference
* Intentionally limited in game-rules coverage

The development environment includes a MacBook Air with an Apple M2 processor and 8 GB RAM, making heavyweight local-model inference unsuitable as a primary V1 architecture.

---

# 12. Current Technical State

No complete production architecture has yet been approved.

Current preliminary technical direction includes:

* Python backend
* FastAPI
* Web frontend
* Persistent database
* API-based LLM
* Deterministic rules engine
* Structured communication between AI and game engine
* Persistent campaign/world state
* Controlled AI access to game data and rules services

These remain architectural candidates until formally reviewed during M2.

### Rules / AI Boundary Already Established

The following principle is approved:

**AI responsibilities may include:**

* Interpret player intent
* Generate narration
* Generate NPC dialogue
* Suggest checks
* Suggest encounter behaviour
* Propose mechanically relevant actions

**Deterministic system responsibilities include:**

* Dice
* Modifiers
* Checks
* HP
* AC
* Combat outcomes
* Inventory state
* Currency
* Quest state
* Progression
* Persistent NPC/world state
* Validation of AI-proposed mechanics
* Conditions and ongoing effects
* Exhaustion
* Hit Dice
* Rest eligibility and recovery
* Campaign-time advancement
* Rest-event probability
* Environmental-hazard validation
* Spell resources and spell-state validation
* Mechanical event history
* State-change provenance

The core authority model is established. Group 9 will consolidate the remaining cross-system AI / Rules Boundary, and M2.6 will define its technical AI orchestration and implementation.

---

# 13. Current Content and Data Direction

RealmWeaver should avoid manually hard-coding large catalogues of game content where appropriately licensed structured content sources are available.

Potential reusable content includes:

* Monsters
* Equipment
* Weapons
* Armour
* Spells
* Conditions
* Character names
* Location names
* Settlement names
* Tavern names
* Faction names
* Item names
* Other world-building vocabulary

The intended long-term pattern is approximately:

**Licensed / permitted source → Importer → Validation / Normalisation → RealmWeaver internal data**

External content APIs should not become mandatory runtime dependencies for core gameplay.

The final content-import and storage architecture will be defined later in M2.

---

# 14. Repository State

RealmWeaver is under Git version control and connected to the public GitHub repository.

The `main` branch contains:

* M1 product-foundation documentation
* M2 game-rules documentation
* Current project-status documentation

No production application source code has yet been introduced.

Recent documentation work includes approved M2.1 rules specifications through Group 8 — Conditions & Resting.

Current detailed rules specifications cover:

* Character Core
* Checks & Saving Throws
* Dice & Inspiration
* Combat
* Classes & Progression
* Equipment & Inventory
* Magic
* Conditions & Resting

Future development should continue to use:

1. Make controlled changes.
2. Review `git status`.
3. Stage intended files.
4. Review staged changes with `git diff --staged`.
5. Commit with a descriptive message.
6. Push the checkpoint to GitHub.

---

# 15. Development Capacity & Planning Assumption

Current planning assumes approximately:

**20 development hours per week minimum**

Initial high-level estimates remain:

* Technical prototype: approximately 2 months
* Playable Alpha: approximately 4 months
* Strong V1 target: approximately 6 months
* Conservative V1 window: approximately 9 months

These estimates are preliminary.

Once sprint development begins, actual project velocity should replace broad calendar estimates.

---

# 16. Professional Development Workflow

RealmWeaver should continue following a simplified professional software-development lifecycle:

**Planning → Sprint Planning → Development → Review → Testing → Sprint Review → Retrospective → Release**

Professional practices to introduce progressively include:

* Backlog management
* Acceptance criteria
* Definition of Done
* Code review
* Pull requests where useful
* Automated testing
* Architecture Decision Records
* Technical debt tracking
* Risk tracking
* Release notes / changelog
* Milestone gates

The level of ceremony should remain appropriate for a solo developer while still teaching professional engineering practices.

---

# 17. Immediate Next Action

Before beginning the final main M2.1 rules group, complete:

> **Group 6 Amendment — Weapon Mastery**

The amendment will evaluate the selective adoption of revised Weapon Mastery mechanics as an explicitly documented RealmWeaver exception to the primary SRD 5.1 / 2014 baseline.

Expected areas include:

* Vex
* Sap
* Nick
* Cleave
* Graze
* Push
* Slow
* Topple
* Weapon-to-Mastery mapping
* Class access to Weapon Mastery
* Distance-band adaptations
* Condition/effect interactions
* NPC Weapon Mastery use
* AI / rules authority
* V1 scope and balance implications

Once the Weapon Mastery amendment is approved and documented:

1. Update `06_EQUIPMENT_AND_INVENTORY.md` and any affected Combat/Class documentation.
2. Update `GAME_RULES.md`.
3. Continue to **Group 9 — AI / Rules Boundary**.
4. After Group 9, perform the full M2.1 cross-group consistency review.

---

# 18. M2.1 Completion Gate

M2.1 is complete only when all approved rules areas have been specified and reviewed.

Required groups:

* Character Core
* Checks, DCs & Saving Throws
* Dice & Inspiration
* Combat
* Classes & Progression
* Equipment & Inventory
* Magic
* Conditions & Resting
* AI/Rules Boundary

Before the M2.1 completion gate, the approved **Weapon Mastery amendment** must also be incorporated into all affected Group 4, Group 5, and Group 6 rules documentation.

At M2.1 completion:

1. Perform a full `GAME_RULES.md` review.
2. Resolve contradictions or missing decisions.
3. Review V1 scope alignment.
4. Record any deferred mechanics.
5. Add required ADRs.
6. Update `PROJECT_STATUS.md`.
7. Commit and push the completed M2.1 baseline.
8. Proceed to the next M2 architecture activity.

---

# 19. Project Resume Instructions

When returning to RealmWeaver after a break:

1. Read `PROJECT_STATUS.md`.
2. Identify the current milestone and immediate next action.
3. Review the relevant authoritative specification.
4. Review recent Git history where necessary.
5. Review applicable Architecture Decision Records.
6. Check known blockers, technical debt and risks.
7. Continue from the documented next action rather than reconstructing project state from conversation history.

For the current M2.1 activity, the primary specification to review is:

`docs/game-rules/GAME_RULES.md`

---

# 20. Documentation Principle

RealmWeaver documentation and Git history form the project's long-term source of truth.

Conversation history may support development, but important decisions should be captured in the repository.

Documentation should be updated incrementally throughout M2 because technical design decisions are too numerous and interconnected to safely defer all documentation until milestone completion.

At every major milestone:

1. Review the authoritative documents.
2. Update project status.
3. Resolve inconsistencies.
4. Record significant architectural decisions.
5. Commit the milestone baseline.
6. Pass the relevant milestone gate before moving forward.

---

# 21. Next Session

## Starting Point

> **Group 6 Amendment — Weapon Mastery**

## Current Stopping Point

> **M1 complete. M2 active. M2.1 Groups 1–8 approved and documented. Group 8 — Conditions & Resting is complete. Weapon Mastery has been identified as a deliberate rules amendment to resolve before Group 9.**

## Current Authoritative M2 Specification

`docs/game-rules/GAME_RULES.md`

---

# 22. Status Summary

**M1 — Product Foundation:** COMPLETE  
**M2 — Technical Design & Architecture:** IN PROGRESS  
**M2.1 — Game Rules Specification:** IN PROGRESS  
**M2.1 Groups 1–8:** APPROVED  
**Immediate Next Work:** Group 6 Amendment — Weapon Mastery  
**Next Main Group:** Group 9 — AI / Rules Boundary  
**Production Coding:** NOT STARTED  
**Current Technical Source of Truth:** `docs/game-rules/GAME_RULES.md`