# RealmWeaver — User Stories & Acceptance Criteria

**Document Version:** 1.0
**Last Reviewed:** 7 August 2026
**Status:** Approved for V1 Planning

---

# 1. Purpose

This document describes RealmWeaver V1 requirements from the player's perspective.

Each user story explains:

* Who needs the feature.
* What they want to accomplish.
* Why it matters.

Acceptance criteria define the minimum observable behaviour required before the story can be considered complete.

These stories should be used alongside `REQUIREMENTS.md` and `PRODUCT_BACKLOG.md`.

---

# 2. US-001 — Authentication

## User Story

As a player, I want a personal account so that my characters and campaigns remain associated with me.

## Acceptance Criteria

* **AC-001.1:** Given valid registration information, when the player creates an account, then the account shall be created successfully.
* **AC-001.2:** Given valid credentials, when the player logs in, then they shall be taken to the campaign dashboard.
* **AC-001.3:** Given invalid credentials, the player shall be denied access and shown an appropriate error.
* **AC-001.4:** A logged-in player shall be able to securely log out.

---

# 3. US-002 — Campaign Creation

## User Story

As a player, I want to create and configure a campaign so that I can begin an adventure suited to my preferred style of play.

## Acceptance Criteria

* **AC-002.1:** The player shall be able to create a new campaign from the dashboard.
* **AC-002.2:** The player shall be able to provide a basic campaign idea or request an AI-generated premise.
* **AC-002.3:** The player shall be able to configure supported campaign settings.
* **AC-002.4:** The player shall select an existing compatible character or create a new character before gameplay begins.
* **AC-002.5:** Once setup is complete, the AI Dungeon Master shall generate an opening scenario.
* **AC-002.6:** The newly created campaign shall be saved and appear on the campaign dashboard.
* **AC-002.7:** The player shall be able to select Roleplay-Focused, Balanced, or Combat-Focused play style.
* **AC-002.8:** The selected play style shall influence the AI Dungeon Master's emphasis during the campaign.
* **AC-002.9:** Selecting a play style shall not completely prevent other encounter types.
* **AC-002.10:** The player shall be able to modify the preferred play style during an active campaign.

---

# 4. US-003 — Resume Campaign

## User Story

As a player, I want to continue previous campaigns without losing progress so that long-running adventures remain meaningful.

## Acceptance Criteria

* **AC-003.1:** Saved campaigns shall appear on the player's dashboard.
* **AC-003.2:** Selecting a saved campaign shall restore its latest valid state.
* **AC-003.3:** Character HP, inventory, equipment, currency, progression, location, and quests shall be restored.
* **AC-003.4:** Relevant world, NPC, and campaign state shall be restored.
* **AC-003.5:** The player shall receive an appropriate recap before continuing.
* **AC-003.6:** Continuing shall resume the existing campaign rather than generating a new starting scenario.

---

# 5. US-004 — Character Management

## User Story

As a player, I want to create and manage a character whose statistics and equipment meaningfully influence gameplay.

## Acceptance Criteria

* **AC-004.1:** The player shall be able to create a character using the options supported by the V1 Game Rules Specification.
* **AC-004.2:** Character statistics shall be calculated according to the implemented rules.
* **AC-004.3:** A completed character shall be saveable.
* **AC-004.4:** The player shall be able to view the character sheet during gameplay.
* **AC-004.5:** HP, equipment, inventory, currency, progression, and other supported state changes shall update on the character sheet.
* **AC-004.6:** The game engine shall use the character's authoritative statistics when resolving mechanics.
* **AC-004.7:** Supported equipment shall be equipable and unequipable.
* **AC-004.8:** Applicable equipment effects shall influence game mechanics correctly.

---

# 6. US-005 — AI Dungeon Master Interaction

## User Story

As a player, I want to interact freely with an AI Dungeon Master so that gameplay feels like tabletop role-playing rather than a fixed-choice game.

## Acceptance Criteria

* **AC-005.1:** The player shall be able to submit free-text actions.
* **AC-005.2:** The AI Dungeon Master shall respond using the current scene and relevant campaign state.
* **AC-005.3:** The AI shall not independently alter authoritative HP, inventory, currency, progression, quest state, or dice results.
* **AC-005.4:** The AI shall not make significant decisions or actions on behalf of the player's character unless requested.
* **AC-005.5:** Important established campaign facts supplied to the AI shall be respected.
* **AC-005.6:** Reasonable unexpected player actions shall be interpreted rather than rejected merely because they were not predefined.
* **AC-005.7:** Actions requiring mechanics shall be resolved by the game engine before the final outcome is narrated.

---

# 7. US-006 — Dice & Ability Checks

## User Story

As a player, I want dice rolls to determine uncertain outcomes so that my character's actions have unpredictable, rule-based consequences.

## Acceptance Criteria

* **AC-006.1:** System-generated dice results shall fall randomly within the valid range of the selected die.
* **AC-006.2:** Natural 1 and natural 20 results shall receive their appropriate special treatment where required by the implemented rules.
* **AC-006.3:** When multiple reasonable approaches exist, the AI Dungeon Master may suggest appropriate checks while still allowing free-text alternatives.
* **AC-006.4:** In manual dice mode, the player shall be able to enter a valid physical dice result.
* **AC-006.5:** The system shall calculate and display the natural roll, applicable modifier, final result, and difficulty where appropriate.

---

# 8. US-007 — Combat

## User Story

As a player, I want combat to follow consistent rules so that victories and defeats depend on character abilities, decisions, and dice rather than arbitrary AI decisions.

## Acceptance Criteria

* **AC-007.1:** The system shall recognise when supported combat begins.
* **AC-007.2:** Combat participants shall receive a defined turn order.
* **AC-007.3:** The player shall perform actions according to the implemented combat rules.
* **AC-007.4:** Attack rolls shall be resolved against the appropriate target defence.
* **AC-007.5:** Successful attacks shall calculate damage through the game engine.
* **AC-007.6:** Damage and healing shall correctly modify hit points.
* **AC-007.7:** The system shall recognise when a combatant is defeated or otherwise removed from active combat.
* **AC-007.8:** The AI Dungeon Master shall narrate mechanical combat outcomes rather than inventing them.
* **AC-007.9:** Required combat state shall remain recoverable if the campaign is saved during combat.

---

# 9. US-008 — Character Progression

## User Story

As a player, I want my character to progress through the campaign so that achievements produce meaningful character development.

## Acceptance Criteria

* **AC-008.1:** A campaign shall support XP or milestone progression.
* **AC-008.2:** XP mode shall track experience earned from qualifying events.
* **AC-008.3:** Milestone mode shall track relevant progression milestones.
* **AC-008.4:** Progression shall be evaluated when qualifying quests, encounters, achievements, or milestones are completed.
* **AC-008.5:** The player shall be informed when XP or meaningful milestone progress is awarded.
* **AC-008.6:** The system shall recognise when the character qualifies for a level increase.
* **AC-008.7:** Supported level progression shall correctly update applicable character statistics and abilities.
* **AC-008.8:** Progression shall persist between sessions.
* **AC-008.9:** The player shall be able to access current progression from the campaign interface.
* **AC-008.10:** XP campaigns shall display current XP and the requirement for the next level.
* **AC-008.11:** Milestone campaigns shall communicate progression without revealing hidden campaign information.

---

# 10. US-009 — Exploration

## User Story

As a player, I want to freely explore the campaign world and discover meaningful content.

## Acceptance Criteria

* **AC-009.1:** The player shall be able to explore using free-text actions.
* **AC-009.2:** Locations may contain discoverable items, clues, characters, currency, or encounters.
* **AC-009.3:** Discoveries requiring checks shall be resolved through the game engine.
* **AC-009.4:** Items shall not become part of the player's inventory solely because the AI narrates their existence.
* **AC-009.5:** Successfully collected items and discoveries shall be recorded in authoritative campaign state.
* **AC-009.6:** Important previously discovered locations shall remain known to the campaign.
* **AC-009.7:** New locations may be introduced progressively as the player explores.

---

# 11. US-010 — Quest & Goal Tracking

## User Story

As a player, I want discovered quests and objectives tracked so that I understand my current goals without having to remember everything myself.

## Acceptance Criteria

* **AC-010.1:** A newly unlocked quest or objective shall automatically appear in the goal tracker.
* **AC-010.2:** The tracker shall distinguish main and optional/side quests where applicable.
* **AC-010.3:** Goals shall support states including Active, Completed, Failed, or Abandoned.
* **AC-010.4:** Quest progress shall update when relevant authoritative events occur.
* **AC-010.5:** The player shall be able to access the goal tracker during gameplay.
* **AC-010.6:** Completed quests shall remain available through quest history.
* **AC-010.7:** The tracker shall not reveal undiscovered quests, hidden objectives, or information unknown to the player's character.

---

# 12. US-011 — Inventory, Currency & Rewards

## User Story

As a player, I want resources and rewards to be tracked so that equipment, discoveries, and quest rewards have persistent gameplay value.

## Acceptance Criteria

* **AC-011.1:** The system shall track items currently possessed by the character.
* **AC-011.2:** The system shall track supported currency denominations.
* **AC-011.3:** Eligible quests may award currency, items, equipment, supported spells/abilities, or character progression.
* **AC-011.4:** Rewards shall only be granted after the relevant completion requirements are satisfied.
* **AC-011.5:** A one-time reward shall not be claimable repeatedly.
* **AC-011.6:** The player shall be informed about received rewards.
* **AC-011.7:** Gained, consumed, sold, dropped, transferred, or otherwise removed items shall update inventory correctly.
* **AC-011.8:** Inventory and currency shall persist between sessions.
* **AC-011.9:** Supported consumable items shall produce their defined game effect when used.

---

# 13. US-012 — End Session & Campaign Memory

## User Story

As a player, I want RealmWeaver to remember important events between sessions so that my campaign remains coherent over long-term play.

## Acceptance Criteria

* **AC-012.1:** The player shall be able to deliberately end a gameplay session.
* **AC-012.2:** Ending a session shall preserve the latest valid campaign state.
* **AC-012.3:** The system shall generate a summary of important session events.
* **AC-012.4:** Important decisions, discoveries, NPC interactions, quest changes, and campaign events shall be preserved where relevant.
* **AC-012.5:** Future AI interactions shall receive relevant established campaign information.
* **AC-012.6:** Factual campaign continuity shall not rely exclusively on the AI model's conversational memory.
* **AC-012.7:** Resuming a campaign shall provide an appropriate recap.
* **AC-012.8:** Player-facing recaps shall not expose information unknown to the player's character.
* **AC-012.9:** The player should be able to inspect selected known information currently retained by the campaign-memory system.

---

# 14. US-013 — Campaign Interface

## User Story

As a player, I want a clean campaign interface so that I can focus on the adventure while still accessing game information when necessary.

## Acceptance Criteria

* **AC-013.1:** AI narration and player input shall remain the primary focus of the campaign screen.
* **AC-013.2:** Character sheet, inventory, quest tracker, progression, and other secondary information shall not permanently occupy the main gameplay area.
* **AC-013.3:** Supported secondary panels shall be openable and closable by the player.
* **AC-013.4:** Dice controls shall be accessible when a roll requires player interaction.
* **AC-013.5:** The interface shall indicate when the AI Dungeon Master is processing a response.
* **AC-013.6:** Important events such as damage, rewards, progression, and quest completion shall be clearly communicated without unnecessarily interrupting gameplay.

---

# 15. US-014 — Campaign Difficulty

## User Story

As a player, I want to change campaign difficulty during an active campaign so that I can adjust the challenge without restarting my adventure.

## Acceptance Criteria

* **AC-014.1:** Difficulty shall be changeable through campaign settings.
* **AC-014.2:** Changing difficulty shall not reset or corrupt campaign progress.
* **AC-014.3:** The selected difficulty shall affect subsequent relevant encounters or mechanics according to the V1 Game Rules Specification.
* **AC-014.4:** Existing character statistics, inventory, XP, milestones, and completed quests shall remain intact when difficulty changes.

---

# 16. Story Completion

A user story is not considered complete merely because its primary interface appears to work.

Completion requires:

* Acceptance criteria satisfied.
* Applicable functional requirements satisfied.
* Applicable automated or manual tests completed.
* Code review completed.
* Relevant documentation updated.
* No unresolved release-blocking defects.

The project's full completion standard is defined in `DEFINITION_OF_DONE.md`.

---

# 17. Related Documents

* `PROJECT_VISION.md`
* `V1_SCOPE.md`
* `REQUIREMENTS.md`
* `PRODUCT_BACKLOG.md`
* `DEFINITION_OF_DONE.md`
* Future `GAME_RULES.md`
