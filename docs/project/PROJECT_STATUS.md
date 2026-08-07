# RealmWeaver — Project Status

**Document Version:** 1.0
**Last Reviewed:** 8 August 2026
**Overall Status:** Active
**Current Milestone:** M1 — Product Foundation
**Milestone Status:** Complete, documentation finalisation in progress

---

# 1. Current Project State

RealmWeaver is currently in the pre-development phase.

The product foundation has been defined and approved. No production application code has been written yet.

The project is now finalising Milestone 1 documentation before moving into technical design and architecture.

---

# 2. Completed Work

## Milestone 1 — Product Foundation

The following activities are complete:

* Product Vision
* V1 Player Experience
* V1 Scope
* Functional Requirements
* Non-Functional Requirements
* User Stories
* Acceptance Criteria
* Product Backlog
* Definition of Done
* Risk Register
* Initial competitive/reference analysis
* Unique Value Proposition
* Formal Milestone 1 review
* Go decision for technical design

---

# 3. Core Product Definition

RealmWeaver is a:

> **Single-player, text-first, AI-powered fantasy tabletop RPG system with deterministic game mechanics and persistent campaign state.**

The core engineering principle is:

> **AI controls the narrative. The game engine controls the truth.**

The product positioning is:

> **RealmWeaver is a transparent, rules-driven AI Dungeon Master where the AI controls the story but never the truth of the game.**

Product tagline:

> **AI tells the story. Rules decide what happens.**

---

# 4. Current V1 Priorities

The major V1 systems currently planned are:

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

---

# 5. Current Documentation

The following documents exist or are being finalised:

* `PROJECT_VISION.md`
* `REFERENCE_ANALYSIS.md`
* `V1_SCOPE.md`
* `REQUIREMENTS.md`
* `USER_STORIES.md`
* `PRODUCT_BACKLOG.md`
* `DEFINITION_OF_DONE.md`
* `RISK_REGISTER.md`
* `PROJECT_STATUS.md`

Future documentation will include:

* Game Rules Specification
* Software Architecture
* Database Design
* API Specification
* Architecture Decision Records
* Sprint records
* Testing strategy
* Release documentation

---

# 6. Current Major Risks

Highest-priority risks currently identified:

* Scope creep
* AI mechanical hallucination
* Long-term memory inconsistency
* Game-rule complexity
* Rule-interaction defects
* AI/game-engine integration complexity
* Solo-development workload

Refer to `RISK_REGISTER.md` for the full register.

---

# 7. Current Constraints

RealmWeaver V1 is:

* Developed by one developer
* Primarily for personal use
* A professional software-engineering learning project
* Single-player
* Text-first
* Low-budget
* Dependent on API-based AI rather than heavy local model inference
* Intentionally limited in rules coverage

---

# 8. Current Technical Decisions

No final production architecture has yet been approved.

Preliminary direction includes:

* Python backend
* FastAPI
* Web frontend
* Persistent database
* API-based LLM
* Deterministic rules engine
* Structured AI/game-engine communication

These remain subject to Milestone 2 architecture review.

---

# 9. Current Repository State

The RealmWeaver repository currently contains project documentation only.

Application folders, runtime dependencies, virtual environments, and production source code should not be introduced until the technical architecture has been sufficiently defined.

---

# 10. Next Milestone

## Milestone 2 — Technical Design & Architecture

The next milestone will define how RealmWeaver will be built.

Expected areas include:

* V1 Game Rules Specification
* System Architecture
* Technology Stack Confirmation
* Domain Model
* Database Design
* AI Architecture
* Memory Architecture
* Rules Engine Design
* API Boundaries
* Repository Structure
* Development Environment
* Testing Architecture
* Initial Architecture Decision Records

---

# 11. Immediate Next Action

Complete the Milestone 1 documentation gate.

After documentation review:

1. Commit Milestone 1 documentation to Git.
2. Push the repository to GitHub.
3. Formally close Milestone 1.
4. Open Milestone 2.

---

# 12. Project Resume Instructions

When returning to RealmWeaver after a break:

1. Read `PROJECT_STATUS.md`.
2. Review the current milestone.
3. Review the latest sprint documentation if development has started.
4. Check recent Git history.
5. Review open technical debt, bugs, and risks where relevant.

This document should always provide the fastest summary of the project's current position.

---

# 13. Status Update Rule

Update this document when:

* A milestone begins or ends.
* A sprint begins or ends.
* Project scope materially changes.
* A major architecture decision changes project direction.
* Development is intentionally paused.
* A major release is completed.

Minor development tasks do not require a status-document update.
