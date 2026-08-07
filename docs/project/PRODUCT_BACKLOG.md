# RealmWeaver — Product Backlog

**Document Version:** 1.0
**Last Reviewed:** 7 August 2026
**Status:** Approved for V1 Planning

---

# 1. Purpose

This document contains the ordered Product Backlog for RealmWeaver Version 1.0.

The backlog represents the currently known engineering and product work required to build RealmWeaver V1.

Backlog items are expected to evolve during development through normal backlog refinement.

The backlog should be used alongside:

* `PROJECT_VISION.md`
* `V1_SCOPE.md`
* `REQUIREMENTS.md`
* `USER_STORIES.md`
* `DEFINITION_OF_DONE.md`
* `RISK_REGISTER.md`

---

# 2. Priority Model

RealmWeaver uses the following backlog priorities:

* **P0 — Critical/Core:** Required for the core V1 product.
* **P1 — Important:** Strongly desired for V1 and expected before release where practical.
* **P2 — Optional:** Useful but should not delay higher-priority work.

---

# 3. Relative Size Model

Initial estimates use relative sizing rather than hours.

* **S — Small**
* **M — Medium**
* **L — Large**
* **XL — Requires further decomposition**

These estimates are intentionally approximate.

More detailed estimates will be created during sprint planning and backlog refinement.

---

# 4. Product Backlog

## Epic A — Project Foundation

| ID     | Backlog Item                                           | Priority | Size | Key Dependency |
| ------ | ------------------------------------------------------ | -------: | :--: | -------------- |
| PB-001 | Establish repository and project structure             |       P0 |   S  | —              |
| PB-002 | Configure application environments and secret handling |       P0 |   S  | PB-001         |
| PB-003 | Establish logging and error-handling foundation        |       P0 |   M  | PB-001         |
| PB-004 | Establish code-quality conventions                     |       P0 |   S  | PB-001         |
| PB-005 | Establish initial development configuration            |       P0 |   S  | PB-001         |

---

## Epic B — Authentication

| ID     | Backlog Item                                       | Priority | Size | Key Dependency |
| ------ | -------------------------------------------------- | -------: | :--: | -------------- |
| PB-006 | Design user account model                          |       P1 |   S  | Persistence    |
| PB-007 | Implement account registration                     |       P1 |   M  | PB-006         |
| PB-008 | Implement login/logout and authentication sessions |       P1 |   M  | PB-006         |
| PB-009 | Implement secure password storage                  |       P1 |   M  | PB-006         |
| PB-010 | Build login and registration interface             |       P1 |   M  | PB-007–009     |

Authentication is important for professional-development experience but should not delay core gameplay if schedule pressure occurs.

---

## Epic C — Character System

| ID     | Backlog Item                                          | Priority | Size | Key Dependency           |
| ------ | ----------------------------------------------------- | -------: | :--: | ------------------------ |
| PB-011 | Design character domain model                         |       P0 |   M  | Game Rules Specification |
| PB-012 | Implement supported ability/stat calculations         |       P0 |   L  | PB-011                   |
| PB-013 | Implement character creation rules                    |       P0 |   L  | PB-011–012               |
| PB-014 | Implement character persistence                       |       P0 |   M  | PB-011                   |
| PB-015 | Implement character service/API                       |       P0 |   M  | PB-013–014               |
| PB-016 | Build character creation interface                    |       P0 |   L  | PB-015                   |
| PB-017 | Build character sheet interface                       |       P0 |   M  | PB-015                   |
| PB-018 | Implement supported character resources and abilities |       P0 |   L  | Game Rules Specification |
| PB-019 | Implement supported saving throws                     |       P0 |   M  | PB-011                   |
| PB-020 | Implement supported inspiration mechanics             |       P1 |   M  | Game Rules Specification |

---

## Epic D — Dice & Ability Resolution

| ID     | Backlog Item                                                      | Priority | Size | Key Dependency           |
| ------ | ----------------------------------------------------------------- | -------: | :--: | ------------------------ |
| PB-021 | Implement generic dice engine                                     |       P0 |   S  | —                        |
| PB-022 | Implement automatic dice mode                                     |       P0 |   S  | PB-021                   |
| PB-023 | Implement manual physical-dice entry mode                         |       P0 |   S  | PB-021                   |
| PB-024 | Implement manual roll validation                                  |       P0 |   S  | PB-023                   |
| PB-025 | Implement ability/check modifiers                                 |       P0 |   M  | Character System         |
| PB-026 | Implement difficulty-check resolution                             |       P0 |   M  | PB-021, PB-025           |
| PB-027 | Implement natural 1/natural 20 special behaviour where applicable |       P0 |   M  | Game Rules Specification |
| PB-028 | Implement advantage/disadvantage where applicable                 |       P0 |   M  | PB-021                   |
| PB-029 | Build dice interaction interface                                  |       P0 |   M  | PB-021–028               |
| PB-030 | Display transparent mechanical roll breakdown                     |       P0 |   S  | PB-026                   |

---

## Epic E — Authoritative Game State

| ID     | Backlog Item                                     | Priority | Size | Key Dependency   |
| ------ | ------------------------------------------------ | -------: | :--: | ---------------- |
| PB-031 | Design authoritative game-state model            |       P0 |   L  | Character System |
| PB-032 | Define allowed game-state mutations              |       P0 |   L  | PB-031           |
| PB-033 | Implement state-validation rules                 |       P0 |   M  | PB-031           |
| PB-034 | Implement state persistence                      |       P0 |   L  | Persistence      |
| PB-035 | Implement atomic/transactional state updates     |       P0 |   M  | PB-032–034       |
| PB-036 | Implement autosave/checkpoint behaviour          |       P0 |   M  | PB-034           |
| PB-037 | Implement controlled game-state correction tools |       P1 |   M  | PB-031–034       |
| PB-038 | Implement game-event/history log                 |       P0 |   M  | PB-031           |

---

## Epic F — Campaign Management

| ID     | Backlog Item                                 | Priority | Size | Key Dependency   |
| ------ | -------------------------------------------- | -------: | :--: | ---------------- |
| PB-039 | Design campaign domain model                 |       P0 |   M  | PB-031           |
| PB-040 | Implement campaign creation                  |       P0 |   M  | PB-039           |
| PB-041 | Implement campaign save/load                 |       P0 |   L  | PB-034, PB-039   |
| PB-042 | Implement multiple independent campaigns     |       P1 |   M  | PB-041           |
| PB-043 | Implement campaign deletion                  |       P1 |   S  | PB-041           |
| PB-044 | Add destructive-action confirmation          |       P1 |   S  | PB-043           |
| PB-045 | Implement campaign settings                  |       P1 |   M  | PB-039           |
| PB-046 | Implement supported starting-level selection |       P1 |   S  | Character System |
| PB-047 | Build campaign dashboard                     |       P0 |   L  | PB-040–043       |

---

## Epic G — AI Dungeon Master

| ID     | Backlog Item                                      | Priority | Size | Key Dependency |
| ------ | ------------------------------------------------- | -------: | :--: | -------------- |
| PB-048 | Define structured AI Dungeon Master contract      |       P0 |   L  | PB-031         |
| PB-049 | Implement AI provider abstraction                 |       P0 |   M  | PB-048         |
| PB-050 | Integrate initial LLM provider                    |       P0 |   M  | PB-049         |
| PB-051 | Design and implement DM system prompt             |       P0 |   L  | PB-048         |
| PB-052 | Build AI context assembly pipeline                |       P0 |  XL  | PB-048–051     |
| PB-053 | Implement free-text player-action handling        |       P0 |   M  | PB-052         |
| PB-054 | Implement player-intent interpretation            |       P0 |   L  | PB-052         |
| PB-055 | Implement AI-to-rules-engine action requests      |       P0 |   L  | Rules Engine   |
| PB-056 | Implement game-engine-result-to-AI narration flow |       P0 |   L  | PB-055         |
| PB-057 | Implement player-agency safeguards                |       P0 |   M  | PB-051–052     |
| PB-058 | Implement narrative consistency safeguards        |       P0 |   L  | Memory System  |
| PB-059 | Implement mechanical-authority safeguards         |       P0 |   M  | PB-048         |
| PB-060 | Implement graceful AI-request failure handling    |       P0 |   M  | PB-050         |

PB-052 should be decomposed further during later backlog refinement.

---

## Epic H — Combat System

| ID     | Backlog Item                                 | Priority | Size | Key Dependency           |
| ------ | -------------------------------------------- | -------: | :--: | ------------------------ |
| PB-061 | Design V1 combat-state model                 |       P0 |   M  | Game Rules Specification |
| PB-062 | Implement combat initiation                  |       P0 |   M  | PB-061                   |
| PB-063 | Implement initiative and turn order          |       P0 |   M  | PB-021, PB-061           |
| PB-064 | Implement turn management                    |       P0 |   M  | PB-063                   |
| PB-065 | Implement attack resolution                  |       P0 |   L  | PB-025–028               |
| PB-066 | Implement damage calculations                |       P0 |   M  | PB-065                   |
| PB-067 | Implement healing                            |       P0 |   M  | PB-066                   |
| PB-068 | Implement combatant defeat/death-state rules |       P0 |   M  | PB-066                   |
| PB-069 | Implement basic enemy combat actions         |       P0 |   L  | PB-063–068               |
| PB-070 | Connect combat outcomes to AI narration      |       P0 |   L  | PB-056                   |
| PB-071 | Implement combat persistence/recovery        |       P0 |   M  | PB-034, PB-061           |

---

## Epic I — Inventory, Equipment & Items

| ID     | Backlog Item                                | Priority | Size | Key Dependency           |
| ------ | ------------------------------------------- | -------: | :--: | ------------------------ |
| PB-072 | Design item and inventory model             |       P0 |   M  | Game Rules Specification |
| PB-073 | Implement inventory operations              |       P0 |   L  | PB-072                   |
| PB-074 | Implement equipment slots and equip/unequip |       P0 |   M  | PB-072                   |
| PB-075 | Apply equipment effects to character state  |       P0 |   M  | PB-074                   |
| PB-076 | Implement consumable-item usage             |       P0 |   M  | PB-073                   |
| PB-077 | Implement supported tool usage              |       P1 |   M  | PB-072                   |
| PB-078 | Build inventory/equipment interface         |       P0 |   M  | PB-073–076               |

---

## Epic J — Currency & Rewards

| ID     | Backlog Item                                 | Priority | Size | Key Dependency         |
| ------ | -------------------------------------------- | -------: | :--: | ---------------------- |
| PB-079 | Implement copper/silver/gold currency system |       P0 |   M  | Game State             |
| PB-080 | Implement reward model                       |       P0 |   M  | Inventory, Progression |
| PB-081 | Implement quest reward application           |       P0 |   M  | Quest System           |
| PB-082 | Prevent duplicate one-time rewards           |       P0 |   S  | PB-081                 |
| PB-083 | Implement reward notifications               |       P1 |   S  | PB-081                 |

---

## Epic K — Quest & Goal System

| ID     | Backlog Item                                       | Priority | Size | Key Dependency |
| ------ | -------------------------------------------------- | -------: | :--: | -------------- |
| PB-084 | Design quest and objective model                   |       P0 |   M  | Game State     |
| PB-085 | Implement quest discovery/unlocking                |       P0 |   M  | PB-084         |
| PB-086 | Implement quest-state transitions                  |       P0 |   M  | PB-084         |
| PB-087 | Implement quest-progress tracking                  |       P0 |   L  | PB-086         |
| PB-088 | Implement main/side quest classification           |       P0 |   S  | PB-084         |
| PB-089 | Implement quest history                            |       P1 |   S  | PB-086         |
| PB-090 | Implement hidden-information protection for quests |       P0 |   M  | PB-084–089     |
| PB-091 | Build quest/goal tracker interface                 |       P0 |   M  | PB-084–090     |

---

## Epic L — Character Progression

| ID     | Backlog Item                                     | Priority | Size | Key Dependency           |
| ------ | ------------------------------------------------ | -------: | :--: | ------------------------ |
| PB-092 | Implement XP progression                         |       P1 |   M  | Character System         |
| PB-093 | Implement milestone progression                  |       P1 |   M  | Quest System             |
| PB-094 | Implement progression-event evaluation           |       P1 |   M  | PB-092–093               |
| PB-095 | Implement level eligibility detection            |       P1 |   M  | PB-092–094               |
| PB-096 | Implement supported level-up processing          |       P1 |   L  | Game Rules Specification |
| PB-097 | Persist progression state                        |       P1 |   S  | PB-092–096               |
| PB-098 | Build progression viewer                         |       P1 |   M  | PB-092–097               |
| PB-099 | Communicate XP and milestone progression reasons |       P1 |   S  | PB-094                   |

---

## Epic M — NPC System

| ID     | Backlog Item                             | Priority | Size | Key Dependency     |
| ------ | ---------------------------------------- | -------: | :--: | ------------------ |
| PB-100 | Design persistent NPC model              |       P0 |   M  | Game State         |
| PB-101 | Implement important-NPC registration     |       P0 |   M  | PB-100             |
| PB-102 | Implement NPC status tracking            |       P0 |   M  | PB-100             |
| PB-103 | Implement basic NPC relationship state   |       P0 |   M  | PB-100             |
| PB-104 | Record important player/NPC interactions |       P0 |   M  | Game Event Log     |
| PB-105 | Provide relevant NPC state to AI context |       P0 |   M  | PB-052, PB-100–104 |

---

## Epic N — Exploration & World State

| ID     | Backlog Item                                                         | Priority | Size | Key Dependency    |
| ------ | -------------------------------------------------------------------- | -------: | :--: | ----------------- |
| PB-106 | Design V1 location/world-state model                                 |       P1 |   L  | Game State        |
| PB-107 | Implement location discovery                                         |       P1 |   M  | PB-106            |
| PB-108 | Implement discoverable items, clues, and resources                   |       P1 |   L  | Inventory, PB-106 |
| PB-109 | Implement exploration checks through rules engine                    |       P1 |   M  | Dice/Rules        |
| PB-110 | Implement dynamic location generation                                |       P1 |   L  | AI DM, PB-106     |
| PB-111 | Persist important dynamically generated locations                    |       P1 |   M  | PB-106, PB-110    |
| PB-112 | Prevent narrative-only discoveries from mutating authoritative state |       P0 |   M  | PB-031            |

RealmWeaver V1 will use progressively generated and persisted locations rather than attempting to simulate a complete open world.

---

## Epic O — Campaign Memory

| ID     | Backlog Item                                 | Priority | Size | Key Dependency |
| ------ | -------------------------------------------- | -------: | :--: | -------------- |
| PB-113 | Design V1 campaign-memory architecture       |       P0 |   L  | Game State     |
| PB-114 | Implement recent conversation context        |       P0 |   M  | PB-113         |
| PB-115 | Implement structured campaign facts          |       P0 |   L  | PB-113         |
| PB-116 | Integrate game-event history with memory     |       P0 |   M  | PB-038, PB-113 |
| PB-117 | Implement session summarisation              |       P0 |   L  | AI DM          |
| PB-118 | Implement end-session workflow               |       P0 |   M  | PB-117         |
| PB-119 | Implement campaign recap                     |       P0 |   M  | PB-117         |
| PB-120 | Prevent player-facing memory spoilers        |       P0 |   M  | PB-115–119     |
| PB-121 | Build visible Campaign Chronicle/memory view |       P1 |   L  | PB-115–120     |

---

## Epic P — Campaign Difficulty & Style

| ID     | Backlog Item                                             | Priority | Size | Key Dependency           |
| ------ | -------------------------------------------------------- | -------: | :--: | ------------------------ |
| PB-122 | Define V1 Easy/Normal/Hard difficulty behaviour          |       P1 |   M  | Game Rules Specification |
| PB-123 | Implement campaign difficulty                            |       P1 |   M  | PB-122                   |
| PB-124 | Implement mid-campaign difficulty changes                |       P1 |   M  | PB-123                   |
| PB-125 | Implement Roleplay/Balanced/Combat play-style preference |       P1 |   M  | AI DM                    |
| PB-126 | Implement mid-campaign play-style changes                |       P1 |   S  | PB-125                   |

---

## Epic Q — Campaign Interface

| ID     | Backlog Item                                       | Priority | Size | Key Dependency              |
| ------ | -------------------------------------------------- | -------: | :--: | --------------------------- |
| PB-127 | Design primary campaign-screen UX                  |       P0 |   L  | Product requirements        |
| PB-128 | Implement AI narration/chat view                   |       P0 |   L  | AI DM                       |
| PB-129 | Implement player-action input                      |       P0 |   M  | PB-053                      |
| PB-130 | Implement collapsible/on-demand information panels |       P0 |   L  | Character, Quest, Inventory |
| PB-131 | Implement DM processing/loading feedback           |       P1 |   S  | AI DM                       |
| PB-132 | Implement important game-event notifications       |       P1 |   M  | Game State                  |
| PB-133 | Implement campaign-settings access                 |       P1 |   M  | Settings                    |
| PB-134 | Implement desktop/tablet responsive layout         |       P1 |   M  | PB-127–133                  |

---

## Epic R — Testing & Quality Assurance

| ID     | Backlog Item                             | Priority | Size | Key Dependency  |
| ------ | ---------------------------------------- | -------: | :--: | --------------- |
| PB-135 | Establish automated testing framework    |       P0 |   M  | Foundation      |
| PB-136 | Unit-test dice engine                    |       P0 |   M  | Dice            |
| PB-137 | Unit-test ability/check calculations     |       P0 |   M  | Dice/Character  |
| PB-138 | Unit-test character state                |       P0 |   M  | Character       |
| PB-139 | Unit-test game-state mutations           |       P0 |   L  | Game State      |
| PB-140 | Unit-test combat                         |       P0 |   L  | Combat          |
| PB-141 | Unit-test progression                    |       P0 |   M  | Progression     |
| PB-142 | Unit-test quest and reward behaviour     |       P0 |   L  | Quest/Rewards   |
| PB-143 | Unit-test inventory/equipment logic      |       P0 |   M  | Inventory       |
| PB-144 | Integration-test persistence             |       P0 |   L  | Persistence     |
| PB-145 | Integration-test AI/game-engine boundary |       P0 |   L  | AI DM           |
| PB-146 | Integration-test save/resume flow        |       P0 |   L  | Campaign/Memory |
| PB-147 | Test state recovery and failure cases    |       P0 |   L  | Game State      |

---

## Epic S — AI Evaluation

| ID     | Backlog Item                                            | Priority | Size | Key Dependency |
| ------ | ------------------------------------------------------- | -------: | :--: | -------------- |
| PB-148 | Create repeatable AI DM evaluation scenarios            |       P1 |   L  | AI DM          |
| PB-149 | Evaluate narrative continuity                           |       P1 |   L  | Memory         |
| PB-150 | Evaluate player agency                                  |       P1 |   M  | AI DM          |
| PB-151 | Evaluate narrative repetition                           |       P1 |   M  | AI DM          |
| PB-152 | Evaluate adherence to authoritative mechanical outcomes |       P0 |   M  | AI/Game Engine |
| PB-153 | Record AI quality issues discovered during playtesting  |       P1 |   S  | Evaluation     |

---

## Epic T — Security

| ID     | Backlog Item                                            | Priority | Size | Key Dependency |
| ------ | ------------------------------------------------------- | -------: | :--: | -------------- |
| PB-154 | Protect API keys and application secrets                |       P0 |   S  | Foundation     |
| PB-155 | Validate external and user-controlled input             |       P0 |   M  | API/UI         |
| PB-156 | Review authentication security                          |       P1 |   M  | Authentication |
| PB-157 | Review public repository for accidental secret exposure |       P0 |   S  | Git            |

---

## Epic U — DevOps & Repository Quality

| ID     | Backlog Item                                  | Priority | Size | Key Dependency          |
| ------ | --------------------------------------------- | -------: | :--: | ----------------------- |
| PB-158 | Establish professional Git branch/PR workflow |       P1 |   S  | Repository              |
| PB-159 | Configure GitHub CI for automated tests       |       P1 |   M  | Testing                 |
| PB-160 | Establish dependency-management process       |       P0 |   S  | Development environment |
| PB-161 | Prepare deployment configuration              |       P1 |   M  | Application             |
| PB-162 | Document local-development setup              |       P0 |   M  | Application             |
| PB-163 | Maintain project changelog                    |       P1 |   S  | Releases                |

---

## Epic V — V1 Release

| ID     | Backlog Item                                  | Priority | Size | Key Dependency |
| ------ | --------------------------------------------- | -------: | :--: | -------------- |
| PB-164 | Conduct complete V1 acceptance testing        |       P0 |  XL  | All V1 systems |
| PB-165 | Conduct regression testing                    |       P0 |   L  | PB-164         |
| PB-166 | Resolve release-blocking defects              |       P0 |  XL  | PB-164–165     |
| PB-167 | Review unresolved technical debt              |       P0 |   M  | All            |
| PB-168 | Review V1 requirements against implementation |       P0 |   M  | All            |
| PB-169 | Prepare V1 release documentation              |       P0 |   M  | V1 complete    |
| PB-170 | Perform formal V1 Go/No-Go review             |       P0 |   M  | PB-164–169     |

---

# 5. Future Backlog — Explicitly Outside V1

The following items are recognised but shall not enter V1 implementation unless the project scope is formally changed:

* Multiplayer campaigns.
* Real-time multiplayer synchronisation.
* Voice input.
* AI-generated Dungeon Master voice.
* Graphical battle maps.
* Advanced map editing.
* AI companion agents.
* Advanced vector/RAG memory.
* Full procedural-world simulation.
* Expanded large-scale rules catalogue.
* Mobile application.
* Marketplace functionality.
* Community campaign sharing.
* Subscription/payment infrastructure.
* Human Dungeon Master co-pilot mode.
* Advanced animated dice systems.

---

# 6. Backlog Refinement Policy

The Product Backlog is a living document.

Backlog refinement may:

* Break large items into smaller tasks.
* Change relative estimates.
* Reorder priorities.
* Add newly discovered technical work.
* Remove unnecessary work.
* Clarify dependencies.

Any change that materially expands V1 product scope should be reviewed against `V1_SCOPE.md`.

Large items marked **XL** must be decomposed before entering an active sprint.

---

# 7. Sprint Entry Criteria

A backlog item should generally not enter a sprint until:

* Its purpose is understood.
* Relevant acceptance criteria are identified.
* Important dependencies are known.
* The item is sufficiently small to estimate.
* Major architectural questions affecting it have been resolved.

---

# 8. Product Backlog Principle

RealmWeaver should prioritise:

> **Core gameplay reliability before feature quantity.**

The backlog should favour work that improves:

* Persistent storytelling.
* Deterministic game mechanics.
* Player agency.
* Transparency.
* Campaign consistency.
* Maintainability.

Features that do not meaningfully support these objectives should remain outside V1 unless required as technical infrastructure.
