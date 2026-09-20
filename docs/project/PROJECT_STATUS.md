# RealmWeaver — Project Status

**Document Version:** 1.3
**Last Reviewed:** 31 August 2026
**Overall Status:** Active
**Current Milestone:** M2 — Technical Design & Architecture
**Milestone Status:** IN PROGRESS
**Current Activity:** M2.1 — V1 Game Rules Specification & Rules-Engine Boundary
**Next Activity:** Independent verification of Review Batch 3

**Current Progress:** Sections 9A–9L APPROVED; Group 9 rules design COMPLETE; internal consistency review PASSED; Foundation Review COMPLETE, PASSED AND COMMITTED; Review Batches 1–2 COMPLETE AND PASSED; Review Batch 3 IN PROGRESS — APPROVED CORRECTIONS IMPLEMENTED, INDEPENDENT VERIFICATION PENDING

**Group 9 Internal Review Gate:** PASSED

**M2.1 Completion Gate:** PENDING

**Production Coding:** NOT AUTHORIZED

---

# 1. Current Project State

RealmWeaver has completed Milestone 1 — Product Foundation and has formally entered Milestone 2 — Technical Design & Architecture.

The product vision, V1 scope, requirements, user stories, acceptance criteria, product backlog, Definition of Done, risk register, competitive/reference analysis, and project-management documentation were established during M1.

M2 is defining how RealmWeaver V1 will function technically before production implementation begins.

The first M2 activity, M2.1 — V1 Game Rules Specification & Rules-Engine Boundary, has completed its primary rules-design stage.

The following M2.1 rules groups are APPROVED and documented:

* Group 1 — Character Core
* Group 2 — Checks & Saving Throws
* Group 3 — Dice & Inspiration
* Group 4 — Combat
* Group 5 — Classes & Progression
* Group 6 — Equipment & Inventory
* Group 7 — Magic
* Group 8 — Conditions & Resting
* Group 9 — AI / Rules Boundary

The detailed specifications are stored under:

`docs/game-rules/`

The primary rules-design work is complete, but M2.1 has **not yet passed its completion gate**.

Before M2.1 is formally closed, RealmWeaver must complete:

1. Full Groups 1–9 cross-group consistency review.
2. Terminology and cross-reference review.
3. V1 scope/deferred-feature review.
4. SRD/IP/content-provenance audit.
5. Required documentation corrections.
6. Final M2.1 gate review.
7. Documentation checkpoint commit and push.

No production application code has been introduced yet.

The project remains in technical specification and architecture preparation rather than implementation.

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

**RULES DESIGN COMPLETE — GROUP 9 INTERNAL REVIEW PASSED — CROSS-GROUP REVIEW IN PROGRESS — REVIEW BATCHES 1–2 PASSED — REVIEW BATCH 3 VERIFICATION PENDING — M2.1 GATE PENDING**

Authoritative rules index:

`docs/game-rules/GAME_RULES.md`

Detailed specifications:

* `docs/game-rules/01_CHARACTER_CORE.md`
* `docs/game-rules/02_CHECKS_AND_SAVES.md`
* `docs/game-rules/03_DICE_AND_INSPIRATION.md`
* `docs/game-rules/04_COMBAT.md`
* `docs/game-rules/05_CLASSES_AND_PROGRESSION.md`
* `docs/game-rules/06_EQUIPMENT_AND_INVENTORY.md`
* `docs/game-rules/07_MAGIC.md`
* `docs/game-rules/08_CONDITIONS_AND_RESTING.md`
* `docs/game-rules/09_AI_RULES_BOUNDARY.md`

The purpose of M2.1 is to determine:

* Which tabletop mechanics RealmWeaver V1 supports
* Which mechanics are intentionally simplified or adapted
* Which mechanics are explicitly excluded or deferred
* Which decisions belong to deterministic RealmWeaver systems
* Which decisions belong to the AI Dungeon Master
* How natural-language player intent becomes structured mechanical proposals
* How AI-generated proposals are validated
* How dice and deterministic mechanics are resolved
* How authoritative state is committed and persisted
* How hybrid AI/deterministic NPC-controller decisions remain bounded by rules and knowledge
* How world content becomes authoritative
* How player/NPC/world knowledge remains separated
* How long-running campaign context and memory are assembled
* How retries and failures preserve authoritative state
* How committed outcomes are returned to AI for narration
* How narrative-first UX coexists with inspectable deterministic mechanics

## Approved Groups

### Group 1 — Character Core

**APPROVED**

Defines core character statistics, ability generation, skills, proficiency, Expertise, HP, AC, movement and level-support foundations.

### Group 2 — Checks & Saving Throws

**APPROVED**

Defines automatic success, checks, impossible actions, DCs, saving throws, contested checks, passive checks and narrative interpretation around deterministic outcomes.

### Group 3 — Dice & Inspiration

**APPROVED**

Defines supported dice, automatic and manual rolling, advantage/disadvantage, rerolls, hidden rolls, dice history, Inspiration and dice authority.

### Group 4 — Combat

**APPROVED**

Defines initiative, action economy, reactions, distance-band positioning, attacks, damage, healing, unconsciousness, death, combat AI, encounter consequences and related combat rules.

Includes approved Weapon Mastery cross-group amendments.

### Group 5 — Classes & Progression

**APPROVED**

Defines the V1 Fighter, Rogue, Wizard and Cleric scope, species/background foundations, class progression, XP/milestone progression, level-up behaviour and progression validation.

### Group 6 — Equipment & Inventory

**APPROVED**

Defines structured item definitions/instances, ownership, currency, weapons, armour, shields, hand/equipment state, ammunition, consumables, loot, merchants, tools and optional Encumbrance.

Includes the authoritative Weapon Mastery weapon-state integration.

### Group 7 — Magic

**APPROVED**

Defines structured spell data, spell access, preparation, spellbooks, spell slots, casting time, range, targeting, attack/save resolution, components, concentration, rituals, upcasting, innate magic, damage/healing, persistent effects and magic authority boundaries.

### Group 8 — Conditions & Resting

**APPROVED**

Defines structured conditions, environmental hazards, Exhaustion, short/long rests, Hit Dice recovery, death/stabilisation interactions, persistent recovery, rest interruption and authoritative world-time progression requirements.

### Group 9 — AI / Rules Boundary

**Sections 9A–9L:** APPROVED

**Rules Design:** COMPLETE

**Internal Consistency Review:** PASSED

**Internal Review Gate:** PASSED

Defines:

* Authority Model
* Player Intent Interpretation
* AI Mechanical Proposals
* Validation & Rejection
* Mechanical Resolution Pipeline
* AI Narration Boundary
* Hybrid NPC Controller Authority
* World & Content Proposals
* Knowledge & Information Boundaries
* Context & Memory Boundary
* Failure, Retry & Consistency
* Latency, UX & Final Contract

Group 9 establishes the central contract:

> **Player chooses. Assigned NPC controllers propose actor intent; AI interprets and narrates. RealmWeaver validates, resolves, commits and remembers.**

The canonical lifecycle is:

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

Commit/persist includes atomic durable persistence. Significant persistent content must complete validation, materialisation and commit/persistence before presentation as established reality. Claims and rumours remain distinct from objective world truth.

Randomness generated for a validated action remains bound to that action across technical and persistence retries. Deterministic controllers may routinely select simple NPC behaviour; AI selects intent for actors assigned to AI. Every controller-selected action remains a proposal until RealmWeaver validates, resolves and commits/persists it. For AI-assigned actors, deterministic fallback is limited to bounded failure recovery after bounded AI failure.

It also establishes that:

> **The interface should make the player want to enter the world.**

Visual quality, a coherent RealmWeaver visual identity, accessibility, responsiveness and explicit visual/UX review are mandatory for the core V1 player experience. Elaborate animations and other purely decorative enhancements remain optional or deferred.

## Current Review State

All nine detailed rules groups have approved rules-design sections.

Group 9 rules design is complete. Its internal consistency review and internal-review gate passed on 31 August 2026.

The Groups 1–9 cross-group consistency review uses this approved bounded structure:

* Foundation Review — Character Mathematics, Checks, Saves, Dice, and Inspiration: COMPLETE, PASSED AND COMMITTED
* Review Batch 1 — Combat and State Effects: COMPLETE AND PASSED
* Review Batch 2 — Progression, Recovery, Persistence, and AI Lifecycle: COMPLETE AND PASSED
* Review Batch 3 — Scope, Authority, Terminology, and Final Consolidation: IN PROGRESS — APPROVED CORRECTIONS IMPLEMENTED; INDEPENDENT VERIFICATION PENDING

The Foundation Review resolved `CG-FND-001` through `CG-FND-006`. Review Batches 1–2 are complete and passed. The earlier baseline-preparation findings are superseded and resolved by the applicable Review Batch 3 corrections. These passed components do not complete or pass the overall Groups 1–9 cross-group review.

### Consolidated Cross-Group Findings Ledger

| Finding | Origin | Status | Summary |
| --- | --- | --- | --- |
| `CG-BASE-001` | Baseline preparation | SUPERSEDED — RESOLVED | Domain-specific document ownership replaces the universal hierarchy. |
| `CG-BASE-002` | Baseline preparation | SUPERSEDED — RESOLVED | `GAME_RULES.md` is the authoritative rules index rather than universal technical authority. |
| `CG-BASE-003` | Baseline preparation | SUPERSEDED — RESOLVED | Core/P1 requiredness is aligned across scope, requirements and backlog. |
| `CG-FND-001` | Foundation Review | RESOLVED | Contested-check ties preserve the pre-contest status quo. |
| `CG-FND-002` | Foundation Review | RESOLVED | Passive checks use all normal modifiers with non-stacking `+5` Advantage and `−5` Disadvantage adjustments. |
| `CG-FND-003` | Foundation Review | RESOLVED | Ordinary Saving Throws resolve natural 1 and natural 20 through the final modified total. |
| `CG-FND-004` | Foundation Review | RESOLVED | Halfling Lucky and level-up Hit Point rerolls have explicit timing, replacement, repetition, and retry rules. |
| `CG-FND-005` | Foundation Review | RESOLVED | Validated manual results and their complete resolution records remain bound across technical retries. |
| `CG-FND-006` | Foundation Review | RESOLVED | Inspiration is mandatory Core V1 functionality. |
| `CG-CMB-001` | Review Batch 1 | RESOLVED | Spell and magic-item flows resolve completely before atomic durable commit/persistence and narration. |
| `CG-CMB-002` | Review Batch 1 | RESOLVED | Mechanically significant item and spell changes commit/persist atomically rather than optionally or eventually. |
| `CG-CMB-003` | Review Batch 1 | RESOLVED | General combat flows include the durable commit/persistence boundary before narration. |
| `CG-CMB-004` | Review Batch 1 | RESOLVED | Combat, Magic and Conditions use the shared Engaged/Near/Far/Distant distance model. |
| `CG-CMB-005` | Review Batch 1 | RESOLVED | A creature regains its Reaction at the start of its turn. |
| `CG-CMB-006` | Review Batch 1 | RESOLVED | Opportunity Attacks require willing movement out of effective reach; forced movement and teleportation do not trigger. |
| `CG-CMB-007` | Review Batch 1 | RESOLVED | Two-weapon fighting, its damage rule, its once-per-turn limit and Nick integration are explicit. |
| `CG-CMB-008` | Review Batch 1 | RESOLVED | Reach, Ammunition, Loading, range and ammunition-recovery behaviour are explicit. |
| `CG-CMB-009` | Review Batch 1 | RESOLVED | Armour/shield non-proficiency and Heavy Armour Strength consequences are explicit. |
| `CG-CMB-010` | Review Batch 1 | RESOLVED | All eight Weapon Mastery properties and the supported Core V1 weapon mapping are self-contained. |
| `CG-CMB-011` | Review Batch 1 | RESOLVED | Full Components hand-state legality and permitted equipment transitions are explicit. |
| `CG-CMB-012` | Review Batch 1 | RESOLVED | The shared damage pipeline defines modifier, immunity, Resistance, Vulnerability and rounding order. |
| `CG-CMB-013` | Review Batch 1 | RESOLVED | Damage-at-zero, Death Saving Throw, stabilisation and reset transitions are explicit. |
| `CG-CMB-014` | Review Batch 1 | RESOLVED | Temporary HP retain-or-replace behaviour is explicit and never stacks by default. |
| `CG-PRP-001` | Review Batch 2 | RESOLVED — VERIFIED | Trance provides an eligible four-hour Long Rest duration without changing other Long Rest rules. |
| `CG-PRP-002` | Review Batch 2 | RESOLVED — VERIFIED | Stable-at-0-HP natural recovery uses one bound, durable and retry-safe `1d4`-hour result. |
| `CG-PRP-003` | Review Batch 2 | RESOLVED — VERIFIED | Long Rest benefits require at least 1 HP at start; Exhaustion reduction requires applicable food and drink. |
| `CG-PRP-004` | Review Batch 2 | RESOLVED — VERIFIED | One accumulated hour of qualifying strenuous interruption fails a Long Rest. |
| `CG-PRP-005` | Review Batch 2 | RESOLVED — VERIFIED | Second Wind, Action Surge, Channel Divinity and Arcane Recovery have explicit recharge rules. |
| `CG-PRP-006` | Review Batch 2 | RESOLVED — VERIFIED | Newly granted count-based level-up capacity becomes available without refilling spent capacity. |
| `CG-PRP-007` | Review Batch 2 | RESOLVED — VERIFIED | Level-up HP changes recalculate Effective Maximum HP and cap Current HP accordingly. |
| `CG-PRP-008` | Review Batch 2 | RESOLVED — VERIFIED | Level-up cannot commit during an `ACTIVE` or `PAUSED` Rest. |
| `CG-PRP-009` | Review Batch 2 | RESOLVED — VERIFIED | Dead and 0-HP living characters retain progression but cannot commit level-up. |
| `CG-PRP-010` | Review Batch 2 | RESOLVED — VERIFIED | Milestone rewards create persistent ordered entitlements with stable identities and one-at-a-time consumption. |
| `CG-PRP-011` | Review Batch 2 | RESOLVED — VERIFIED | XP/milestone rewards use stable identities, atomic durable ledgers and retry-safe pending results. |
| `CG-PRP-012` | Review Batch 2 | RESOLVED — VERIFIED | Paused activities and meaningful choices persist and resume without replay or retroactive reopening. |
| `CG-PRP-013` | Review Batch 2 | RESOLVED — VERIFIED | Rest time, events, qualification, recovery, commit/persistence and narration use the approved order. |
| `CG-PRP-014` | Review Batch 2 | RESOLVED — VERIFIED | Group 8 conversational wrapper and unmatched Markdown fences were removed. |
| `CG-FIN-001` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Domain-specific document ownership and rules-index authority are explicit. |
| `CG-FIN-002` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Core progression mechanics, durable state and minimum display are Core V1/P0. |
| `CG-FIN-003` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Authentication, difficulty, game style, standalone summaries and enhanced progression presentation remain Should Have/P1. |
| `CG-FIN-004` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Save/load, authoritative memory/state, continuity and minimum resume context remain Core V1/P0. |
| `CG-FIN-005` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Review status records Batches 1–2 passed and Batch 3 verification pending. |
| `CG-FIN-006` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Hybrid deterministic/AI NPC-controller terminology and proposal authority are aligned. |
| `CG-FIN-007` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | The accidental governing-principle Markdown fence was removed. |
| `CG-FIN-008` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Authoritative completed outcomes require atomic durable commit/persistence before narration. |
| `CG-FIN-009` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Inspiration is P0 and mandatory approved rule domains have traceable backlog coverage. |
| `CG-FIN-010` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Background Ability Score allocation and the Human flexible benefit are exact and bounded. |
| `CG-FIN-011` | Review Batch 3 | CORRECTED — VERIFICATION PENDING | Six Species/eight Background scope is approved while source-dependent Species mechanics remain provenance-pending. |

The Foundation Review and Review Batches 1–2 are complete and passed. The A–D labels used during the combat work were decision sub-batches within Review Batch 1, not separate review milestones.

Applying the approved Review Batch 3 corrections does not pass Review Batch 3. Independent verification remains required before its status may change, and the full review and M2.1 gate remain incomplete.

Remaining work before the M2.1 gate:

* Full Groups 1–9 cross-group consistency review
* Rules terminology review
* Deferred-feature review
* V1 scope alignment review
* SRD/IP/content-provenance review
* Final documentation corrections
* M2.1 gate decision

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

SRD 5.1 / 2014-style mechanics are the primary mechanical baseline.

RealmWeaver may selectively adopt later mechanics where explicitly approved and documented.

Weapon Mastery is one such approved adaptation. It is always enabled in RealmWeaver and is not a per-campaign rules toggle.

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

## M2 Game-Rules Documentation

Located under:

`docs/game-rules/`

Current rules documentation includes:

* `GAME_RULES.md`
* `01_CHARACTER_CORE.md`
* `02_CHECKS_AND_SAVES.md`
* `03_DICE_AND_INSPIRATION.md`
* `04_COMBAT.md`
* `05_CLASSES_AND_PROGRESSION.md`
* `06_EQUIPMENT_AND_INVENTORY.md`
* `07_MAGIC.md`
* `08_CONDITIONS_AND_RESTING.md`
* `09_AI_RULES_BOUNDARY.md`

## Planned Architecture Documentation

Later M2 work is expected to introduce documentation such as:

* `docs/architecture/SYSTEM_ARCHITECTURE.md`
* `docs/architecture/DATABASE_DESIGN.md`
* `docs/architecture/AI_ARCHITECTURE.md`
* `docs/architecture/MEMORY_ARCHITECTURE.md`
* `docs/architecture/API_DESIGN.md`

Architecture Decision Records may be stored under:

`docs/adr/`

These files should be created progressively as their related architectural decisions are made.

---

# 9. Domain-Specific Document Authority

RealmWeaver repository documentation is authoritative by domain:

* `AGENTS.md` owns working process and agent permissions.
* `PROJECT_STATUS.md` owns current milestone, activity and gate status.
* `V1_SCOPE.md` owns V1, stretch, optional, deferred and excluded boundaries.
* `REQUIREMENTS.md` owns binding product behaviour.
* The relevant numbered Group 1–9 specification owns detailed mechanics for its domain.
* `GAME_RULES.md` is the authoritative rules index and summary; it must agree with detailed specifications but does not silently override them.
* `DEFINITION_OF_DONE.md` owns completion standards.
* A future approved Architecture Decision Record owns only its architectural decision scope and must not silently override approved rules, scope or requirements.

When documents own different aspects of the same issue, reconcile them by domain. If two documents claim authority over the same aspect and disagree, report a source-of-truth conflict for explicit review. Current approved repository documentation remains authoritative over older conversation, memory, assumption or suggestion.

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
* FastAPI as a backend candidate
* Web frontend
* Persistent database
* API-based LLM integration
* Deterministic rules engine
* Structured AI ↔ RealmWeaver communication
* Persistent campaign/world state
* Controlled AI access to game state
* Task-specific context assembly
* Long-term campaign memory outside the LLM
* Structured UI reading authoritative RealmWeaver state directly
* Strong visual design system and polished fantasy UI as a first-class product requirement

These remain architectural candidates until formally reviewed during the appropriate M2 activities.

## Rules / AI Boundary Established

The detailed AI/rules contract is now defined in:

`docs/game-rules/09_AI_RULES_BOUNDARY.md`

The core authority relationship is:

### Player responsibilities

The player owns:

* Player-character intent
* Meaningful player decisions
* Dialogue/actions chosen by the player
* Optional player-controlled choices

### AI responsibilities

AI may:

* Interpret natural-language intent
* Generate narration
* Control NPC dialogue/personality expression
* Select intentions for AI-controlled NPCs
* Propose checks and mechanical actions
* Propose world/content additions
* Propose continuity-relevant memory updates

### RealmWeaver responsibilities

RealmWeaver owns:

* Rules
* Dice/randomness
* Mechanical legality
* Mechanical resolution
* Persistent state
* Inventory/equipment
* Currency/resources
* Conditions
* Progression
* Quest state
* World clock
* Materialised world canon
* Knowledge boundaries
* Commit/retry consistency
* Long-term authoritative campaign truth

The governing principles are:

> **AI tells the story. Rules decide what happens.**

> **AI proposes. RealmWeaver validates.**

> **AI may remember context. RealmWeaver preserves truth.**

---

# 13. Current Content and Data Direction

RealmWeaver should avoid manually hard-coding large catalogues of game content where appropriately licensed structured content sources are available.

Potential reusable content includes:

* Monsters
* Equipment
* Weapons
* Weapon Mastery definitions and mappings
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

The `main` branch is intended to contain:

* M1 product-foundation documentation
* M2 game-rules documentation
* Current project-status documentation
* Approved technical documentation as M2 progresses

The M2.1 rules-design documentation now covers Groups 1–9.

No production application source code has yet been introduced, and production coding is not authorized.

The next repository checkpoint should capture the completed Groups 1–9 rules-design baseline before or together with the M2.1 review corrections.

Development should continue to use:

1. Make controlled changes.
2. Review `git status`.
3. Stage only intended files.
4. Review changes with `git diff`.
5. Review staged changes with `git diff --staged`.
6. Commit with a descriptive message.
7. Push the checkpoint to GitHub.

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

The primary rules-design stage of M2.1 is complete. The Group 9 internal consistency review and internal-review gate passed on 31 August 2026.

The immediate next activity is:

> **Independent verification of Review Batch 3**

Review Batches 1–2 are complete and passed. Review Batch 3 remains in progress within the still-pending M2.1 Groups 1–9 cross-group consistency review. Its approved corrections are implemented, but independent verification is pending; Review Batch 3 and the overall review have not been marked passed or complete.

Review Groups 1–9 for cross-group contradictions, ambiguous interactions, terminology consistency, source-of-truth alignment, and affected cross-file rules including Weapon Mastery.

During and after the Groups 1–9 cross-group consistency review:

1. Perform the full Groups 1–9 cross-group consistency review.
2. Resolve contradictions and ambiguous interactions.
3. Complete independent verification of Review Batch 3.
4. Review any remaining V1/deferred boundaries required by the consolidated review.
5. Perform the SRD/IP/content-provenance audit.
6. Update affected documentation.
7. Update `PROJECT_STATUS.md` if required.
8. Perform the M2.1 completion gate.
9. Commit and push the completed M2.1 baseline.
10. Proceed to M2.2 — System Architecture only after the gate passes.

---
# 18. M2.1 Completion Gate

## Current Gate Status

**NOT YET PASSED**

All required rules groups have approved rules-design sections, and the Group 9 internal-review gate has passed. The M2.1 completion gate has not passed.

Required rules groups:

* Character Core — APPROVED
* Checks & Saving Throws — APPROVED
* Dice & Inspiration — APPROVED
* Combat — APPROVED
* Classes & Progression — APPROVED
* Equipment & Inventory — APPROVED
* Magic — APPROVED
* Conditions & Resting — APPROVED
* AI / Rules Boundary — APPROVED

Before M2.1 can be marked COMPLETE:

1. Perform the complete Groups 1–9 cross-group review.
2. Resolve contradictory or ambiguous rules.
3. Verify V1 scope alignment.
4. Record explicitly deferred mechanics.
5. Verify rules terminology and cross-file references.
6. Review Weapon Mastery cross-group amendments.
7. Perform the SRD/IP/content-provenance audit.
8. Classify mechanically defined content provenance where applicable.
9. Resolve any `UNKNOWN / REQUIRES_REVIEW` licensing/content-source issues required before implementation.
10. Add any required ADRs.
11. Update `GAME_RULES.md`.
12. Update `PROJECT_STATUS.md`.
13. Commit and push the completed M2.1 baseline.
14. Record the M2.1 gate as PASSED.
15. Proceed to M2.2 — System Architecture.

Until those checks are complete, M2.1 remains:

> **RULES DESIGN COMPLETE — GROUP 9 INTERNAL REVIEW PASSED — CROSS-GROUP REVIEW IN PROGRESS — M2.1 GATE PENDING**

# 19. Project Resume Instructions

When returning to RealmWeaver after a break:

1. Read `PROJECT_STATUS.md`.
2. Identify the current milestone and immediate next action.
3. Review the relevant authoritative specification.
4. Review recent Git history where necessary.
5. Review applicable Architecture Decision Records.
6. Check known blockers, technical debt and risks.
7. Continue from the documented next action rather than reconstructing project state from conversation history.

For the current M2.1 activity, the authoritative rules index to review is:

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

> **Independent verification of Review Batch 3**

## Current Stopping Point

> **M1 complete. M2 active. Sections 9A–9L approved. Group 9 rules design complete. Group 9 internal consistency review and internal-review gate passed on 31 August 2026. Foundation Review complete, passed and committed; Review Batches 1–2 complete and passed; Review Batch 3 corrections implemented with independent verification pending. Full cross-group review in progress. SRD/IP/content-provenance audit outstanding. M2.1 gate pending. Production coding not authorized. M2.2 blocked.**

## Current Authoritative Rules Index

`docs/game-rules/GAME_RULES.md`

Before beginning the Groups 1–9 cross-group consistency review, review:

* `docs/game-rules/GAME_RULES.md`
* Relevant detailed M2.1 rule specifications where cross-system behaviour is being defined
* `docs/project/PROJECT_STATUS.md`

Repository documentation remains authoritative over conversation history or assistant memory.
---

# 22. Status Summary

**M1 — Product Foundation:** COMPLETE

**M2 — Technical Design & Architecture:** IN PROGRESS

**M2.1 — Game Rules Specification:** IN PROGRESS

**M2.1 Groups 1–9 Rules Design:** APPROVED

**Weapon Mastery Cross-Group Rules:** APPROVED — REVIEW BATCH 1 PASSED

**Group 9 Rules Design:** COMPLETE

**Group 9 Internal Consistency Review:** PASSED

**Group 9 Internal Review Gate:** PASSED

**M2.1 Completion Gate:** PENDING

**Production Coding:** NOT AUTHORIZED

**M2.2 — System Architecture:** BLOCKED UNTIL THE COMPLETE M2.1 GATE PASSES

**Current Authoritative Rules Index:** `docs/game-rules/GAME_RULES.md`
