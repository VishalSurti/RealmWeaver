# RealmWeaver — Character Core

**Rule Group:** 1
**Status:** APPROVED
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification
**Last Reviewed:** 13 August 2026

---

# 1. Purpose

This document defines RealmWeaver V1's core character mechanics.

It covers:

- Ability Scores
- Ability Score Generation
- Ability Modifiers
- Skills
- Proficiency
- Expertise
- Hit Points
- Armour Class
- Speed
- Level-Support Strategy

More detailed class, species, background and progression rules are defined in:

`05_CLASSES_AND_PROGRESSION.md`

The deterministic rules engine is authoritative over calculated character mechanics.

The AI may interpret and narrate character abilities but must not independently modify authoritative character statistics.

---

# 2. Ability Scores

## 2.1 Core Abilities

### Status: APPROVED

RealmWeaver V1 uses six primary Ability Scores:

- Strength (STR)
- Dexterity (DEX)
- Constitution (CON)
- Intelligence (INT)
- Wisdom (WIS)
- Charisma (CHA)

---

## 2.2 Ability Modifiers

### Status: APPROVED

Ability Modifiers are calculated as:

**floor((Ability Score - 10) / 2)**

Examples:

| Score | Modifier |
|---:|---:|
| 8 | -1 |
| 10 | +0 |
| 12 | +1 |
| 14 | +2 |
| 16 | +3 |
| 18 | +4 |
| 20 | +5 |

The rules engine performs the calculation.

The AI must not independently calculate or override Ability Modifiers.

---

# 3. Ability Score Generation

## 3.1 Supported V1 Methods

### Status: APPROVED

RealmWeaver V1 supports:

- Standard Array
- Rolled Stats

Point Buy is not required for V1.

---

## 3.2 Standard Array

The player assigns predefined balanced scores among the six Ability Scores.

Exact character-creation presentation will be defined during implementation.

---

## 3.3 Rolled Stats

Rolled Stats use the familiar method:

**4d6, drop the lowest die**

for each generated Ability Score.

Rolled stat generation supports:

- RealmWeaver system-generated dice
- Player-entered physical dice

Dice behaviour follows `03_DICE_AND_INSPIRATION.md`.

---

# 4. Skills

## 4.1 Skill List

### Status: APPROVED

RealmWeaver V1 uses the following skill structure.

### Strength

- Athletics

### Dexterity

- Acrobatics
- Sleight of Hand
- Stealth

### Intelligence

- Arcana
- History
- Investigation
- Nature
- Religion

### Wisdom

- Animal Handling
- Insight
- Medicine
- Perception
- Survival

### Charisma

- Deception
- Intimidation
- Performance
- Persuasion

---

## 4.2 Default Ability Associations

Skills have default associated Ability Scores.

These defaults determine the normal modifier used for the check.

---

## 4.3 Alternative Ability + Skill Combinations

### Status: APPROVED

RealmWeaver may permit an alternative Ability Score to be combined with a skill when the player's described approach reasonably justifies it.

Example:

A character intimidating someone by demonstrating physical strength may make:

**Strength (Intimidation)**

instead of the usual:

**Charisma (Intimidation)**

The AI may propose an alternative combination.

The rules system must validate the resulting check.

---

# 5. Proficiency

## 5.1 Basic Proficiency

### Status: APPROVED

When a character is proficient:

**d20 + Ability Modifier + Proficiency Bonus**

When not proficient:

**d20 + Ability Modifier**

The rules engine determines whether proficiency applies.

---

## 5.2 Proficiency Bonus Progression

### Status: APPROVED

| Character Level | Proficiency Bonus |
|---|---:|
| 1–4 | +2 |
| 5–8 | +3 |
| 9–12 | +4 |
| 13–16 | +5 |
| 17–20 | +6 |

Proficiency Bonus does not increase every level.

It is derived from character level.

Detailed progression behaviour is defined in `05_CLASSES_AND_PROGRESSION.md`.

---

# 6. Expertise

## 6.1 Expertise Rule

### Status: APPROVED

Expertise doubles the applicable Proficiency Bonus.

Conceptually:

**d20 + Ability Modifier + (2 × Proficiency Bonus)**

Expertise does not double the Ability Modifier.

The rules engine performs the calculation.

---

# 7. Hit Points

## 7.1 Character HP State

### Status: APPROVED

Characters maintain:

- Maximum HP
- Current HP
- Temporary HP

Starting HP is primarily derived from:

- Class
- Constitution Modifier

Detailed class-specific HP calculations are defined in `05_CLASSES_AND_PROGRESSION.md`.

Detailed damage, healing, unconsciousness and death rules are defined in `04_COMBAT.md`.

---

## 7.2 HP Authority

The rules engine is authoritative over HP.

The AI may narrate:

- Injuries
- Healing
- Exhaustion
- Recovery

but cannot independently change HP values.

---

# 8. Armour Class

## 8.1 Armour Class Concept

### Status: APPROVED

Armour Class (AC) is the primary defensive target for attack resolution.

An unarmoured character generally derives AC from:

**10 + Dexterity Modifier**

unless another supported mechanic changes the calculation.

Armour, shields, class features, species traits, spells and other effects may modify AC.

---

## 8.2 AC Authority

The rules engine calculates authoritative AC.

The AI cannot independently decide whether an attack hits or misses.

Detailed attack rules are defined in `04_COMBAT.md`.

---

# 9. Speed

## 9.1 Character Speed

### Status: APPROVED

Speed is stored as part of character state.

Species and supported features may modify Speed.

---

## 9.2 V1 Movement Model

V1 does not use precise grid-based battlefield positioning.

Speed interacts with the abstract distance-band combat system defined in:

`04_COMBAT.md`

---

# 10. Character Level Support

## 10.1 V1 Minimum

### Status: APPROVED

Fully supported playable range:

**Levels 1–5**

---

## 10.2 V1 Stretch Target

If implementation, testing and schedule permit:

**Levels 1–10**

may be supported before final V1 release.

This stretch target must not delay the reliable core V1.

---

## 10.3 Architecture Target

The underlying character and rules architecture should be capable of eventual expansion toward:

**Levels 1–20**

Architecture support does not mean all levels are initially playable.

---

# 11. Character Core Authority

The deterministic game system is authoritative over:

- Ability Scores
- Ability Modifiers
- Skill proficiency
- Expertise
- HP
- AC
- Speed
- Character level
- Derived statistics

The AI may use these values for narrative reasoning but cannot override them.

---

# 12. Group Status

**Group 1 — Character Core: APPROVED**

Detailed related rules:

- Checks & Saves → `02_CHECKS_AND_SAVES.md`
- Dice → `03_DICE_AND_INSPIRATION.md`
- Combat → `04_COMBAT.md`
- Classes & Progression → `05_CLASSES_AND_PROGRESSION.md`