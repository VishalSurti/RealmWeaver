# RealmWeaver — Software Requirements Specification

**Document Version:** 1.0
**Last Reviewed:** 7 August 2026
**Status:** Approved for V1 Planning

---

# 1. Purpose

This document defines the functional and non-functional requirements for RealmWeaver Version 1.0.

RealmWeaver is a single-player, AI-powered tabletop fantasy RPG system that combines an AI Dungeon Master with an authoritative game engine.

The fundamental engineering principle is:

> **AI controls the narrative. The game engine controls the truth.**

Functional requirements describe what RealmWeaver must do.

Non-functional requirements describe the quality, reliability, security, usability, and engineering characteristics expected from the system.

Detailed game-rule behaviour will be defined separately in the RealmWeaver V1 Game Rules Specification.

---

# 2. Requirement Terminology

Requirements use the following identifiers:

* **FR-XXX** — Functional Requirement
* **NFR-XXX** — Non-Functional Requirement

The term **shall** indicates a required V1 behaviour.

The term **should** indicates a desired behaviour where strict guarantees may not always be technically possible, particularly for probabilistic AI behaviour.

---

# 3. Functional Requirements

## 3.1 User & Authentication

**FR-001 — Account Creation**
The system shall allow a user to create an account.

**FR-002 — Authentication**
The system shall allow a registered user to log in using valid credentials.

**FR-003 — Authentication Session**
The system shall maintain an authenticated user session until the user logs out or the session expires.

**FR-004 — Logout**
The system shall allow an authenticated user to securely log out.

---

# 3.2 Campaign Management

**FR-005 — Campaign Creation**
The system shall allow the player to create a new campaign.

**FR-006 — Campaign Listing**
The system shall allow the player to view their saved campaigns.

**FR-007 — Campaign Resume**
The system shall allow the player to resume a previously saved campaign.

**FR-008 — Campaign Deletion**
The system shall allow the player to permanently delete a campaign after confirmation.

**FR-009 — Campaign Autosave**
The system shall automatically preserve campaign progress at appropriate checkpoints.

**FR-010 — Multiple Campaigns**
The system should allow the player to maintain multiple independent campaigns.

**FR-011 — Campaign Settings**
The system shall maintain settings associated with an individual campaign.

---

# 3.3 Campaign Creation & Preferences

**FR-012 — Campaign Idea**
The system shall allow the player to provide a basic campaign idea or request an AI-generated campaign premise.

**FR-013 — Campaign Play Style**
The system shall allow the player to select a preferred campaign style from supported options such as Roleplay-Focused, Balanced, or Combat-Focused.

**FR-014 — Play-Style Modification**
The system shall allow the player to modify the preferred campaign style during an active campaign.

**FR-015 — Campaign Difficulty**
The system shall support defined campaign difficulty levels.

**FR-016 — Mid-Campaign Difficulty Change**
The system shall allow the player to modify campaign difficulty without restarting or resetting the campaign.

**FR-017 — Starting Character**
The system shall require a supported character to be associated with a campaign before gameplay begins.

---

# 3.4 Character Management

**FR-018 — Character Creation**
The system shall allow the player to create a character using the options supported by the V1 ruleset.

**FR-019 — Saved Characters**
The system should allow the player to save characters for later use.

**FR-020 — Existing Character Selection**
The system should allow the player to select a previously created compatible character when starting a campaign.

**FR-021 — Character Sheet**
The system shall provide a character sheet containing relevant statistics, hit points, abilities, equipment, inventory, progression, and other supported character information.

**FR-022 — Character-State Updates**
The system shall update the character sheet when authoritative character state changes.

**FR-023 — Character Mechanics**
The game engine shall use the character's authoritative statistics when resolving mechanical actions.

---

# 3.5 AI Dungeon Master

**FR-024 — Opening Scenario**
The AI Dungeon Master shall generate an appropriate opening scenario for a new campaign.

**FR-025 — Free-Text Actions**
The system shall allow the player to submit free-text actions during gameplay.

**FR-026 — Narrative Response**
The AI Dungeon Master shall generate narrative responses based on the player's action and relevant campaign state.

**FR-027 — Player Intent Interpretation**
The AI Dungeon Master shall interpret reasonable player intentions expressed through natural language.

**FR-028 — Mechanical Authority Boundary**
The AI Dungeon Master shall not independently modify authoritative mechanical state such as hit points, inventory, currency, progression, dice results, or quest completion.

**FR-029 — Mechanical Resolution Request**
When a player action requires deterministic game mechanics, the AI layer shall request appropriate resolution from the game engine.

**FR-030 — Outcome Narration**
The AI Dungeon Master shall narrate mechanical outcomes supplied by the game engine.

**FR-031 — Player Agency**
The AI Dungeon Master shall not make significant decisions or actions on behalf of the player's character unless explicitly requested.

---

# 3.6 Dice & Ability Checks

**FR-032 — Supported Dice**
The system shall support the dice required by the V1 ruleset, expected to include d4, d6, d8, d10, d12, d20, and d100.

**FR-033 — Automatic Dice Mode**
The system shall allow the game engine to generate dice results.

**FR-034 — Manual Dice Mode**
The system shall allow the player to enter results rolled using physical dice.

**FR-035 — Manual Roll Validation**
The system shall reject manually entered dice values outside the valid range of the required die.

**FR-036 — Dice Mode Selection**
The player shall be able to select whether supported rolls are performed manually or automatically.

**FR-037 — Mechanical Modifiers**
The system shall apply appropriate character modifiers to supported checks.

**FR-038 — Check Resolution**
The game engine shall determine success or failure using the applicable roll, modifier, difficulty, and game rule.

**FR-039 — Special Dice Rules**
The system shall apply appropriate V1 rules to special results such as natural 1 and natural 20 where applicable.

**FR-040 — Transparent Roll Results**
The system shall display sufficient information for the player to understand important mechanical rolls, including the natural roll, modifier, final result, and applicable difficulty where appropriate.

**FR-041 — Alternative Approaches**
The AI Dungeon Master may suggest reasonable mechanical approaches to an obstacle while still allowing the player to attempt free-text alternatives.

---

# 3.7 Core Game Mechanics

**FR-042 — Rules Engine**
The system shall provide a deterministic rules engine responsible for supported V1 game mechanics.

**FR-043 — Hit Points**
The game engine shall track and modify character and combatant hit points according to the V1 ruleset.

**FR-044 — Saving Throws**
The system shall support saving throws required by the V1 ruleset.

**FR-045 — Defensive Statistics**
The system shall support Armour Class or equivalent defensive mechanics required by V1.

**FR-046 — Advantage and Disadvantage**
The system shall support advantage and disadvantage where included by the V1 Game Rules Specification.

**FR-047 — Inspiration**
The system should support inspiration or an equivalent player resource if included in the V1 Game Rules Specification.

Detailed behaviour for these systems shall be defined in the Game Rules Specification.

---

# 3.8 Combat

**FR-048 — Combat Initiation**
The system shall recognise and initialise supported combat encounters.

**FR-049 — Initiative**
The game engine shall establish combat turn order according to the V1 rules.

**FR-050 — Turn Management**
The system shall maintain the current combat turn and active participants.

**FR-051 — Attack Resolution**
The game engine shall resolve attacks using the applicable character statistics, dice, and target defence.

**FR-052 — Damage**
The game engine shall calculate and apply damage.

**FR-053 — Healing**
The game engine shall calculate and apply supported healing effects.

**FR-054 — Combatant Defeat**
The system shall recognise when a combatant has been defeated or otherwise removed from active combat.

**FR-055 — Enemy Actions**
The system shall support basic enemy actions during combat.

**FR-056 — Combat Narration**
The AI Dungeon Master shall narrate combat outcomes determined by the game engine.

**FR-057 — Combat Persistence**
The system shall preserve sufficient combat state to recover an active encounter when required.

---

# 3.9 Character Progression

**FR-058 — Progression Method**
The system shall support experience-point and milestone-based character progression.

**FR-059 — XP Tracking**
In XP mode, the system shall track experience earned by the character.

**FR-060 — Milestone Tracking**
In milestone mode, the system shall track relevant progression milestones.

**FR-061 — Progression Evaluation**
The system shall evaluate progression after qualifying encounters, quests, achievements, or milestones.

**FR-062 — Progression Reason**
The system shall record or communicate the reason for significant XP awards or milestone progression.

**FR-063 — Level Eligibility**
The system shall recognise when the character qualifies for a level increase.

**FR-064 — Level Processing**
The system shall update applicable character statistics and abilities when a supported level increase occurs.

**FR-065 — Progression Persistence**
Character progression shall persist between sessions.

**FR-066 — Progression Viewer**
The player shall be able to view current progression during gameplay.

**FR-067 — XP Progress Display**
For XP campaigns, the progression view shall display current XP and the requirement for the next supported level.

**FR-068 — Milestone Progress Display**
For milestone campaigns, the system shall communicate appropriate progression information without revealing hidden campaign information.

---

# 3.10 Quest & Goal Tracking

**FR-069 — Quest Creation**
The system shall support quests and objectives associated with a campaign.

**FR-070 — Quest Discovery**
The system shall add a quest or objective to the player's goal tracker when it becomes known or available to the character.

**FR-071 — Goal Tracker**
The player shall be able to access the current goal tracker during gameplay.

**FR-072 — Quest Status**
The system shall support appropriate quest states including Active, Completed, Failed, and Abandoned.

**FR-073 — Quest Progress**
The system shall update quest progress when relevant authoritative game events occur.

**FR-074 — Quest Categories**
The system shall support main and optional/side quests where applicable.

**FR-075 — Quest History**
Completed quests shall remain accessible through campaign history.

**FR-076 — Quest Information Security**
The player-facing goal tracker shall not reveal undiscovered quests, hidden objectives, or information unknown to the player's character.

---

# 3.11 Inventory, Equipment & Currency

**FR-077 — Inventory Tracking**
The system shall maintain the character's authoritative inventory.

**FR-078 — Item Acquisition**
The system shall add valid acquired items to the character inventory.

**FR-079 — Item Removal**
The system shall correctly remove consumed, sold, dropped, transferred, or otherwise lost items.

**FR-080 — Equipment**
The system shall allow supported equipment to be equipped and unequipped.

**FR-081 — Equipment Effects**
Supported equipment effects shall be applied to authoritative character state where applicable.

**FR-082 — Consumable Items**
The system shall support use of defined consumable items.

**FR-083 — Currency**
The system shall track supported currency denominations, expected to include copper, silver, and gold.

---

# 3.12 Rewards

**FR-084 — Quest Rewards**
Supported quests shall be capable of providing rewards after successful completion.

**FR-085 — Supported Reward Types**
Rewards may include currency, items, weapons, armour, supported spells/abilities, or character progression.

**FR-086 — Reward Application**
The system shall apply rewards to the appropriate authoritative character or campaign state.

**FR-087 — Duplicate Reward Prevention**
One-time rewards shall not be claimable more than once.

**FR-088 — Reward Notification**
The player shall be informed when rewards are received.

---

# 3.13 Exploration & World State

**FR-089 — Free Exploration**
The player shall be able to explore locations using free-text actions.

**FR-090 — Discoverable Content**
Locations may contain discoverable items, tools, currency, clues, NPCs, or encounters.

**FR-091 — Mechanical Discovery Resolution**
Discoveries requiring mechanical checks shall be resolved through the game engine.

**FR-092 — Authoritative Discoveries**
AI narration alone shall not add discovered mechanical items or resources to authoritative player state.

**FR-093 — Location Persistence**
Important discovered locations shall persist as part of campaign state.

**FR-094 — Progressive Location Generation**
The system should allow new locations and relevant content to be introduced progressively as the campaign develops.

---

# 3.14 NPC State

**FR-095 — Important NPC Registration**
The system shall maintain structured state for important NPCs encountered during a campaign.

**FR-096 — NPC Status**
The system shall track relevant persistent NPC status information.

**FR-097 — NPC Relationships**
The system should maintain relevant player-known relationship information for important NPCs.

**FR-098 — NPC Event Memory**
Important interactions between the player and persistent NPCs should be capable of influencing future interactions.

---

# 3.15 Campaign Memory & Session Management

**FR-099 — Persistent Campaign Memory**
The system shall maintain campaign information required for continuity across sessions.

**FR-100 — Recent Context**
The system shall maintain sufficient recent interaction context for coherent AI responses.

**FR-101 — Structured Campaign Facts**
Important factual campaign information shall be stored independently of conversational AI memory.

**FR-102 — Campaign Events**
The system shall record important campaign events where required for future continuity.

**FR-103 — End Session**
The player shall be able to deliberately end an active gameplay session.

**FR-104 — Session Summary**
The system shall generate a summary of important session events.

**FR-105 — Campaign Recap**
The system shall provide an appropriate recap when an existing campaign is resumed.

**FR-106 — Memory Information Security**
Player-facing summaries and memory views shall not expose information unknown to the player's character.

**FR-107 — Visible Campaign Memory**
The player should be able to inspect selected known campaign information maintained by RealmWeaver.

This may include known NPCs, locations, important events, discovered information, and quest history.

---

# 3.16 Campaign Interface

**FR-108 — Primary Gameplay View**
The system shall provide a primary gameplay interface focused on AI narration and player interaction.

**FR-109 — Player Input**
The campaign interface shall provide an accessible mechanism for submitting player actions.

**FR-110 — Secondary Information Panels**
Character, inventory, quests, progression, and other secondary information shall be accessible without permanently occupying the primary gameplay area.

**FR-111 — Dice Controls**
The system shall provide appropriate dice controls when player interaction with a roll is required.

**FR-112 — Processing Feedback**
The interface shall indicate when an AI Dungeon Master response is being processed.

**FR-113 — Important Event Feedback**
Important events such as damage, rewards, quest completion, or level progression shall be communicated clearly to the player.

---

# 3.17 State Integrity & Recovery

**FR-114 — Controlled State Mutation**
Authoritative game state shall only be modified through approved game-engine operations.

**FR-115 — State Validation**
Proposed state changes shall be validated before being persisted.

**FR-116 — Atomic State Changes**
Where required, related state changes shall either complete successfully together or fail without leaving partially applied authoritative state.

**FR-117 — State Correction**
The system should provide a controlled method for correcting critical campaign-state errors during V1 development and personal use.

---

# 4. Non-Functional Requirements

## Performance

**NFR-001 — AI Response Time**
Under normal operating conditions, the system should aim to begin presenting an AI Dungeon Master response within approximately five seconds where technically practical.

**NFR-002 — Interaction Responsiveness**
Non-AI user-interface interactions should provide prompt feedback and should not unnecessarily block gameplay.

---

## AI Quality

**NFR-003 — Narrative Variety**
The AI Dungeon Master should avoid unnecessary repetition of scenarios, NPC concepts, descriptions, and encounters.

**NFR-004 — Narrative Continuity**
The AI Dungeon Master should remain consistent with important established campaign facts supplied by the memory system.

**NFR-005 — Player Agency**
The AI Dungeon Master should preserve player control over significant character decisions and actions.

---

## Reliability & Data Integrity

**NFR-006 — AI Failure Isolation**
Failure of an AI request shall not corrupt authoritative campaign state.

**NFR-007 — State Consistency**
Authoritative character, inventory, currency, progression, quest, NPC, and campaign state shall remain internally consistent.

**NFR-008 — Recoverability**
Reasonable failures should not unnecessarily destroy valid previously saved campaign progress.

---

## Usability

**NFR-009 — Gameplay Focus**
The primary campaign interface shall prioritise narration and player interaction over simultaneous display of all available information.

**NFR-010 — On-Demand Information**
Secondary gameplay information should be accessible through clear on-demand interface controls.

**NFR-011 — Processing Feedback**
When operations take noticeable time, the interface should indicate that processing is occurring.

**NFR-012 — Understandable Errors**
User-facing failures should provide understandable feedback rather than exposing raw internal exceptions.

**NFR-013 — Responsive Layout**
V1 should remain usable on common desktop and tablet display sizes.

Mobile optimisation is not a V1 requirement.

---

## Security

**NFR-014 — Password Security**
Passwords shall never be stored in plaintext.

**NFR-015 — Secret Management**
API keys, credentials, and other secrets shall not be hard-coded into source code or committed to the public repository.

**NFR-016 — Input Validation**
External/user-controlled inputs shall be appropriately validated before being trusted by application logic.

---

## Maintainability

**NFR-017 — Separation of Concerns**
Game rules, AI integration, persistence, and user-interface logic should remain sufficiently separated to allow components to evolve independently.

**NFR-018 — AI Provider Isolation**
Provider-specific AI integration should be isolated so that changing the LLM provider does not require unnecessary modification of the game engine.

**NFR-019 — Readability**
Production code should use clear naming, appropriate structure, and documentation where necessary to remain understandable and maintainable.

---

## Testability

**NFR-020 — Deterministic Testing**
Deterministic mechanics including dice boundaries, ability calculations, combat, progression, quests, rewards, and state transitions shall be testable independently of the AI provider.

**NFR-021 — Automated Testing**
Critical deterministic game behaviour should be covered by automated tests.

**NFR-022 — AI Evaluation**
Probabilistic AI behaviour should be evaluated using defined scenarios for continuity, repetition, player agency, and rules adherence.

---

# 5. Requirements Traceability

Requirements should eventually be traceable to:

* User stories.
* Product backlog items.
* Architecture components.
* Implementation.
* Automated/manual tests.

A detailed requirements traceability matrix is not required during initial V1 planning but may be introduced as development progresses.

---

# 6. Requirement Change Management

This document is expected to evolve.

A requirement may be modified when:

* Architecture reveals a technical constraint.
* Testing reveals ambiguity.
* Player experience demonstrates a design problem.
* V1 scope is formally changed.
* A requirement proves unnecessarily complex.

Significant changes to V1 requirements should be reviewed before implementation.

New feature ideas should not automatically become V1 requirements.

Where appropriate, they should instead be recorded in the future product backlog.

---

# 7. Related Documents

This specification should be read alongside:

* `PROJECT_VISION.md`
* `V1_SCOPE.md`
* `USER_STORIES.md`
* `PRODUCT_BACKLOG.md`
* `DEFINITION_OF_DONE.md`
* `RISK_REGISTER.md`
* Future `GAME_RULES.md`
* Future architecture documentation
