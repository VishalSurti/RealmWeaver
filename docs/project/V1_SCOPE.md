# RealmWeaver — V1 Scope

**Document Version:** 1.0
**Last Reviewed:** 7 August 2026
**Status:** Approved for V1 Planning

## 1. Purpose

This document defines the functional boundaries of RealmWeaver Version 1.0.

Its purpose is to prevent uncontrolled scope growth during development and establish which capabilities are required, desirable, optional, or explicitly postponed.

RealmWeaver uses MoSCoW prioritisation:

* **Must Have** — Required for RealmWeaver V1 to fulfil its core purpose.
* **Should Have** — Important to the intended experience but may be implemented after the initial playable foundation.
* **Could Have** — Desirable additions that should only be implemented if higher-priority work is complete.
* **Won't Have in V1** — Explicitly excluded from Version 1.0.

---

# 2. V1 Product Boundary

RealmWeaver V1 will be a:

> **Single-player, text-first, AI-powered fantasy tabletop RPG system with deterministic game mechanics and persistent campaign state.**

The objective of V1 is not to reproduce every feature of a complete tabletop platform.

V1 must instead prove RealmWeaver's central concept:

> **AI tells the story. Rules decide what happens.**

---

# 3. Must Have

The following capabilities are required before RealmWeaver V1.0 can be considered complete.

## AI Dungeon Master

RealmWeaver must provide an AI Dungeon Master capable of:

* Narrating the campaign.
* Role-playing NPCs.
* Responding to free-text player actions.
* Interpreting player intentions.
* Introducing encounters, locations, quests, and narrative developments.
* Narrating mechanical outcomes supplied by the game engine.
* Respecting authoritative campaign information.

The AI must not independently override authoritative game state.

## Persistent Campaign State

RealmWeaver must preserve important game information across sessions, including where applicable:

* Character state.
* Hit points.
* Inventory.
* Equipment.
* Currency.
* Progression.
* Quest state.
* Known locations.
* Important NPC state.
* Important campaign events.

## Campaign Saving and Loading

Players must be able to:

* Save campaign progress.
* Resume a saved campaign.
* Recover the latest valid campaign state.
* Receive an appropriate recap when returning to a campaign.

Autosave/checkpoint behaviour should protect against unnecessary progress loss.

## Character System

Players must be able to:

* Create a supported V1 character.
* Save the character.
* View a character sheet.
* Use character statistics during mechanical resolution.
* Equip supported equipment.
* Use supported abilities and items.

The exact V1 character rules will be defined in the Game Rules Specification.

## Dice System

RealmWeaver must support the standard RPG dice required by the V1 ruleset.

Players must be able to choose between:

* System-generated dice rolls.
* Manually entering results from physical dice.

Mechanical calculations must remain controlled by the game engine regardless of which rolling method is selected.

## Core Game Mechanics

RealmWeaver must implement the subset of tabletop RPG mechanics required for V1 gameplay.

This is expected to include concepts such as:

* Ability scores.
* Ability checks.
* Modifiers.
* Saving throws.
* Hit points.
* Armour Class or equivalent defence.
* Attack rolls.
* Damage and healing.
* Initiative.
* Advantage/disadvantage where applicable.
* Character progression.

The exact mechanics and rule interpretations will be formally defined during technical/game-rules design.

## Basic Combat

RealmWeaver must provide deterministic turn-based combat supporting:

* Combat initiation.
* Initiative/turn order.
* Player actions.
* Enemy actions.
* Attack resolution.
* Damage and healing.
* Character/enemy defeat.
* Appropriate AI narration of resolved outcomes.

## Inventory, Equipment and Rewards

The game must track:

* Inventory.
* Equipped items.
* Currency.
* Item acquisition/removal.
* Supported consumable items.
* Quest rewards.

Rewards may include:

* Currency.
* Items.
* Weapons.
* Armour.
* Supported abilities/spells.
* Character progression.

## Quest & Goal Tracking

Unlocked quests and objectives must be available through a player-accessible goal tracker.

The system must support appropriate states such as:

* Active.
* Completed.
* Failed.
* Abandoned.

Completed quests should remain accessible through campaign history.

The tracker must not reveal information the player's character has not discovered.

## Campaign Memory

RealmWeaver must maintain sufficient memory to preserve campaign continuity.

Memory must not depend exclusively on the AI model's conversational context.

The system should maintain a combination of:

* Recent context.
* Structured campaign state.
* Important campaign facts/events.
* Session summaries.

## Campaign Recap

When returning to an existing campaign, the player must be able to receive a summary of relevant previous events without exposing hidden Dungeon Master information.

## NPC Persistence

Important NPCs encountered during gameplay must be capable of maintaining persistent state where relevant.

This may include:

* Identity.
* Status.
* Known relationship with the player.
* Important previous interactions.
* Relevant known events.

The exact NPC representation will be defined during architecture design.

## Transparent Mechanics

Important mechanical outcomes should be visible and understandable to the player where appropriate.

For example:

**Roll + Modifier = Result vs Difficulty**

The AI must narrate the result determined by the rules engine rather than secretly replacing it.

## Game-State Integrity

Mechanical and persistent state changes must occur through controlled game-engine operations.

AI narration alone must not be capable of modifying authoritative game state.

---

# 4. Should Have

The following features are strongly desired for V1 but should not prevent development of the core playable system.

## Multiple Campaigns

Players should be able to maintain and resume multiple independent campaigns.

## Saved Characters

Players should be able to maintain reusable saved characters.

## Character Progression Options

Campaigns should support:

* Experience-point progression.
* Milestone progression.

The selected method should be tracked reliably throughout the campaign.

## Progression Viewer

Players should be able to view their current progression during gameplay.

XP campaigns should display appropriate XP progress.

Milestone campaigns should communicate progression without revealing hidden campaign information.

## Campaign Difficulty

V1 should support a small number of difficulty settings, initially expected to be:

* Easy.
* Normal.
* Hard.

Players should be able to modify difficulty during an existing campaign without resetting campaign progress.

The exact mechanical effects of difficulty will be defined during game-rules design.

## Player-Selected Game Style

Players should be able to express a preferred campaign style:

* Roleplay-focused.
* Balanced.
* Combat-focused.

This preference should influence the AI Dungeon Master's campaign direction without completely preventing other encounter types.

Players should be able to modify this preference during a campaign.

## Progressive Exploration

Players should be able to explore the world through natural-language actions.

The AI may progressively introduce new locations, NPCs, encounters, clues, and potential discoveries.

Important generated content should become part of persistent campaign state when appropriate.

## Visible Campaign Memory

Players should be able to inspect selected information RealmWeaver currently remembers about their campaign.

This may include:

* Known NPCs.
* Known locations.
* Major events.
* Quest history.
* Discovered information.

Hidden Dungeon Master information must remain inaccessible.

## Session Summaries

Ending a session should generate and preserve a useful summary of important events.

## User-Friendly Campaign Interface

The primary campaign screen should focus on:

* AI narration.
* Player input.

Secondary information such as:

* Character sheet.
* Inventory.
* Quests.
* Progression.
* Campaign memory.

should be accessible through on-demand panels rather than permanently cluttering the gameplay screen.

---

# 5. Could Have

These features may be implemented if V1 core and Should-Have functionality are stable.

## Dark Mode

An alternative dark interface theme.

## Dice Animations

Visual dice-roll animations while preserving deterministic dice results.

## Enhanced Goal Classification

Goals may optionally be classified using information such as:

* Current.
* Future.
* Short-term.
* Long-term.
* Estimated difficulty.

## Basic Relationship Indicators

The interface may expose simple player-known NPC relationship information.

## Additional Campaign Customisation

Additional lightweight campaign preferences may be introduced if they do not significantly increase V1 complexity.

---

# 6. Won't Have in V1

The following capabilities are explicitly outside RealmWeaver V1 scope.

## Multiplayer

V1 is exclusively single-player.

## Voice Dungeon Master

Voice input, speech recognition, and AI-generated voice narration are postponed.

## Battle Maps

V1 will not provide tactical graphical battle maps.

Combat will primarily be text-driven.

## Fully Simulated Open World

RealmWeaver will not attempt to simulate an entire persistent fantasy world containing complex economies, populations, political simulations, or every location simultaneously.

World content will instead be generated and persisted progressively where required.

## Elaborate Campaign Builder

V1 will not provide a full campaign-authoring suite.

Players may provide basic campaign ideas and preferences, but advanced world-building tools are postponed.

## AI Companions as Independent Agents

NPC party members may exist narratively, but V1 will not require complex autonomous multi-agent AI companions.

## Advanced RAG Memory

Vector databases, embeddings, and advanced semantic retrieval are not mandatory for V1.

They may be introduced later if simpler memory architecture proves insufficient.

## Complete D&D Rules Coverage

V1 will not attempt to implement every:

* Class.
* Subclass.
* Species/race.
* Feat.
* Spell.
* Monster.
* Magic item.
* Optional rule.

A formally defined subset will be selected for V1.

## Mobile Application

RealmWeaver V1 will be developed as a web application.

## Commercial Infrastructure

V1 does not require:

* Subscription billing.
* Payment processing.
* Commercial analytics.
* Enterprise-scale infrastructure.
* Marketplace functionality.

---

# 7. Scope Change Policy

New ideas discovered during development should not automatically enter V1.

A proposed feature should be evaluated against the following questions:

1. Does it directly improve persistent storytelling?
2. Does it improve meaningful RPG mechanics?
3. Does it improve player agency or transparency?
4. Is it necessary for the core V1 player experience?
5. What additional development and testing complexity does it introduce?

Features that provide value but are unnecessary for V1 should be recorded in the future backlog.

Changes to Must-Have scope should require explicit review before entering active development.

---

# 8. V1 Scope Principle

When uncertain whether a feature belongs in V1, RealmWeaver will prefer:

> **A smaller system that works consistently over a larger system that works unreliably.**

V1 should demonstrate RealmWeaver's core architecture and player experience before expanding into more sophisticated features.
