# RealmWeaver — Project Status

**Document Version:** 1.1
**Last Reviewed:** 8 August 2026
**Overall Status:** Active
**Current Milestone:** M1 — Product Foundation
**Milestone Status:** COMPLETE
**Next Milestone:** M2 — Technical Design & Architecture
**M2 Status:** Not Started

---

# 1. Current Project State

RealmWeaver has completed Milestone 1 — Product Foundation.

The product vision, V1 scope, requirements, user stories, acceptance criteria, product backlog, Definition of Done, risk register, competitive/reference analysis, and project documentation have been defined.

Milestone 1 documentation has been committed to Git and pushed to the remote GitHub repository.

No production application code has been written.

The project is currently positioned between M1 and M2. The next development session will formally open Milestone 2 — Technical Design & Architecture.

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

**Commit:**

`16fc61b — docs: complete M1 product foundation documentation`

This commit represents the initial approved RealmWeaver product-foundation baseline.

---

# 3. Core Product Definition

RealmWeaver is a:

> **Single-player, text-first, AI-powered fantasy tabletop RPG system with deterministic game mechanics and persistent campaign state.**

Core engineering principle:

> **AI controls the narrative. The game engine controls the truth.**

Product positioning:

> **RealmWeaver is a transparent, rules-driven AI Dungeon Master where the AI controls the story but never the truth of the game.**

Product tagline:

> **AI tells the story. Rules decide what happens.**

---

# 4. V1 Product Priorities

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

# 5. Current Documentation

Milestone 1 documentation:

* `PROJECT_VISION.md`
* `REFERENCE_ANALYSIS.md`
* `V1_SCOPE.md`
* `REQUIREMENTS.md`
* `USER_STORIES.md`
* `PRODUCT_BACKLOG.md`
* `DEFINITION_OF_DONE.md`
* `RISK_REGISTER.md`
* `PROJECT_STATUS.md`

These documents represent the current authoritative product context.

If an older discussion conflicts with an approved and subsequently updated project document, the project documentation should normally be treated as the source of truth.

---

# 6. Current Major Risks

Highest-priority risks currently identified include:

* Scope creep
* AI mechanical hallucination
* Long-term campaign-memory inconsistency
* Game-rule complexity
* Rule-interaction defects
* AI/game-engine integration complexity
* Solo-development workload

See `RISK_REGISTER.md` for the complete register and mitigation strategies.

---

# 7. Current Constraints

RealmWeaver V1 is:

* Developed by one developer
* Primarily intended for personal use
* A professional software-engineering learning project
* Single-player
* Text-first
* Low-budget
* Designed around API-based AI rather than heavy local inference
* Intentionally limited in game-rules coverage

---

# 8. Current Technical State

No production architecture has been approved.

Current preliminary technical direction includes:

* Python backend
* FastAPI
* Web frontend
* Persistent database
* API-based LLM
* Deterministic rules engine
* Structured communication between AI and game engine

These are architectural candidates rather than final decisions.

Milestone 2 will formally evaluate and approve the technical architecture.

---

# 9. Repository State

RealmWeaver is under Git version control and connected to a GitHub remote repository.

The `main` branch currently contains the Milestone 1 documentation baseline.

No production application source code has been introduced.

Future development should follow the Git and review workflow established during M1.

---

# 10. Next Milestone

## M2 — Technical Design & Architecture

**Status:** Not Started

The purpose of M2 is to determine how RealmWeaver V1 will actually be built.

Expected M2 areas include:

* V1 Game Rules Specification
* Rules-Engine Boundary
* System Architecture
* Technology Stack Confirmation
* Domain Model
* Database Design
* Authoritative Game-State Design
* AI Architecture
* AI/Game-Engine Contract
* Campaign Memory Architecture
* API Boundaries
* Repository/Application Structure
* Development Environment
* Testing Architecture
* Architecture Decision Records

No major production implementation should begin until the relevant architectural foundations are sufficiently defined.

---

# 11. Immediate Next Action

At the next RealmWeaver development session:

> **Open M2 — Technical Design & Architecture.**

The first planned activity is:

### M2.1 — V1 Game Rules Specification & Rules-Engine Boundary

This activity will determine:

* Which tabletop mechanics RealmWeaver V1 actually supports.
* Which rules are explicitly excluded.
* Which decisions belong to deterministic code.
* Which decisions belong to the AI Dungeon Master.
* How uncertain player actions become mechanical checks.
* How mechanical outcomes are returned to the AI for narration.

---

# 12. Development Capacity & Planning Assumption

Current planning assumes approximately:

**20 development hours per week minimum**

Initial high-level estimates suggest:

* Technical prototype: approximately 2 months
* Playable Alpha: approximately 4 months
* Strong V1 target: approximately 6 months
* Conservative V1 window: approximately 9 months

These estimates are preliminary.

Once sprint development begins, actual project velocity should replace broad calendar estimates.

---

# 13. Project Resume Instructions

When returning to RealmWeaver after a break:

1. Read `PROJECT_STATUS.md`.
2. Identify the current milestone and immediate next action.
3. Review relevant milestone or sprint documentation.
4. Review recent Git history where necessary.
5. Review relevant architecture decisions.
6. Check known blockers, technical debt, and risks.
7. Continue from the documented next action rather than reconstructing project state from conversation history.

---

# 14. Documentation Principle

RealmWeaver documentation and Git history form the project's long-term source of truth.

Conversation history may support development, but important decisions should be captured in the repository.

After every major milestone, project documentation should be reviewed and updated before the milestone is formally closed.

---

# 15. Next Session

**Starting Point:**

> M2.1 — V1 Game Rules Specification & Rules-Engine Boundary

**Current stopping point:**

> M1 Product Foundation complete, committed, and pushed. M2 has not yet started.
