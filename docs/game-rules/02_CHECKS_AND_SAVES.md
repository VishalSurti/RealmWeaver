# RealmWeaver — Checks & Saving Throws

**Rule Group:** 2
**Status:** APPROVED
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification
**Last Reviewed:** 13 August 2026

---

# 1. Purpose

This document defines how RealmWeaver determines whether uncertain character actions succeed or fail.

It covers:

* Action Resolution
* Ability Checks
* Difficulty Classes
* Saving Throws
* Contested Checks
* Passive Checks
* AI Responsibility
* Narrative Degrees of Success and Failure

The central principle is:

> **A roll should occur only when the outcome is meaningfully uncertain and success or failure matters.**

The AI may interpret player intent and propose an appropriate check.

The deterministic RealmWeaver rules engine remains authoritative over the actual mechanical resolution.

---

# 2. Action Resolution

## 2.1 Resolution Categories

### Status: APPROVED

Player actions broadly resolve into three categories:

1. Automatic Success
2. Mechanical Check
3. Impossible

---

## 2.2 Automatic Success

### Status: APPROVED

An action automatically succeeds when:

* It is reasonably achievable.
* There is no meaningful uncertainty.
* Failure would not create a meaningful consequence.
* A roll would not add useful gameplay.

Example:

Opening an ordinary unlocked door normally requires no check.

The AI should not request unnecessary dice rolls merely because a player performed an action.

---

## 2.3 Mechanical Check

### Status: APPROVED

A Mechanical Check occurs when:

* The result is uncertain.
* Success is reasonably possible.
* Failure is reasonably possible.
* The outcome has meaningful consequences.

Example:

Attempting to climb a wet collapsing wall may require an Athletics check.

The AI may identify that a check is appropriate.

The rules engine resolves the check.

---

## 2.4 Impossible Actions

### Status: APPROVED

An action is Impossible when it cannot reasonably succeed under the current circumstances.

Examples may include:

* Attempting to lift an entire castle.
* Attempting to jump an impossible distance without a relevant supernatural ability.
* Attempting to persuade an NPC to perform something fundamentally impossible for them.

Rolling a die does not make an impossible action possible.

A Natural 20 on an Ability Check does not override this rule.

---

# 3. Ability Checks

## 3.1 Basic Ability Check Formula

### Status: APPROVED

An Ability Check generally uses:

**d20 + Ability Modifier + Proficiency Bonus when applicable**

If Expertise applies:

**d20 + Ability Modifier + doubled Proficiency Bonus**

The rules engine calculates the final value.

---

## 3.2 Skill Proficiency

A relevant Skill may determine whether Proficiency Bonus applies.

Examples:

* Strength (Athletics)
* Dexterity (Stealth)
* Wisdom (Perception)
* Charisma (Persuasion)

Skill definitions are maintained in:

`01_CHARACTER_CORE.md`

---

## 3.3 Player Approach

### Status: APPROVED

The player's described approach may influence:

* Which Ability is relevant.
* Which Skill is relevant.
* Whether proficiency applies.
* Whether a check is required.
* The difficulty category.

The system should avoid forcing the player into a fixed menu of actions when free-text intent can reasonably be interpreted.

---

## 3.4 Alternative Ability + Skill Combinations

### Status: APPROVED

RealmWeaver supports justified alternative Ability + Skill combinations.

Example:

A character attempting to intimidate someone through physical strength may use:

**Strength (Intimidation)**

instead of the default:

**Charisma (Intimidation)**

The AI may propose an alternative combination.

The rules system validates the resulting check.

---

# 4. AI Role in Checks

## 4.1 AI Responsibilities

### Status: APPROVED

The AI Dungeon Master may interpret:

* Natural-language player intent.
* Narrative context.
* Environmental circumstances.
* Relevant Skill.
* Relevant Ability.
* Whether a check is appropriate.
* Difficulty category.
* Narrative reasoning.

Example:

```text
Player:
"I try to quietly climb onto the roof without the guards noticing."

AI interpretation:
- Mechanical check required
- Dexterity (Stealth)
- Moderate difficulty
```

---

## 4.2 Rules-Engine Responsibilities

### Status: APPROVED

The deterministic rules engine remains authoritative over:

* Dice results
* Ability Modifiers
* Proficiency
* Expertise
* Final totals
* Difficulty Class
* Success/failure
* Margin of success/failure
* Mechanical state changes

The AI cannot override these results.

---

## 4.3 AI Cannot Predetermine Success

The AI must not narrate an unresolved attempted action as successful before mechanical resolution.

Incorrect:

> You leap across the gap and land safely.

before the required check is resolved.

Correct flow:

```text
Player Intent
    ↓
AI Interpretation
    ↓
Mechanical Check
    ↓
Rules Resolution
    ↓
AI Narration
```

---

# 5. Difficulty Classes

## 5.1 Controlled Difficulty Bands

### Status: APPROVED

RealmWeaver uses controlled Difficulty Class categories.

| Difficulty        | Base DC |
| ----------------- | ------: |
| Very Easy         |       5 |
| Easy              |      10 |
| Moderate          |      15 |
| Hard              |      20 |
| Very Hard         |      25 |
| Nearly Impossible |      30 |

Most ordinary gameplay checks should generally fall between:

**DC 10 and DC 20**

---

## 5.2 AI Difficulty Selection

### Status: APPROVED

The AI should normally propose a difficulty category rather than an unrestricted numerical DC.

Example:

```text
difficulty = HARD
```

The deterministic system converts this into:

```text
HARD → DC 20
```

This prevents the AI from arbitrarily inventing unusual DC values.

---

## 5.3 Contextual Difficulty

### Status: APPROVED

Environmental and narrative circumstances may alter the appropriate difficulty category.

Example:

Climbing a stable wall:

**Moderate**

Climbing the same wall while:

* Wet
* Crumbling
* Under pressure

may instead be:

**Hard**

---

## 5.4 V1 Difficulty Adjustment Philosophy

### Status: APPROVED

V1 generally prefers changing the overall difficulty category instead of stacking numerous small environmental modifiers.

For example:

Instead of:

```text
Base DC 15
+2 rain
+1 darkness
+1 damaged wall
```

RealmWeaver may simply classify the overall situation as:

**Hard → DC 20**

This reduces unnecessary mechanical complexity.

---

# 6. Saving Throws

## 6.1 Saving Throw Purpose

### Status: APPROVED

Saving Throws represent attempts to resist, avoid or withstand something happening to a character.

Examples include:

* Poison
* Magical effects
* Falling hazards
* Fear
* Environmental effects
* Traps
* Certain spells
* Creature abilities

---

## 6.2 Saving Throw Formula

### Status: APPROVED

Saving Throws use:

**d20 + Ability Modifier + Proficiency Bonus when proficient**

Classes determine which Saving Throws receive proficiency.

Class Saving Throw proficiencies are defined in:

`05_CLASSES_AND_PROGRESSION.md`

---

## 6.3 Saving Throw Authority

The AI may identify that a narrative event reasonably requires a Saving Throw.

The rules engine determines:

* Relevant Ability
* Modifier
* Proficiency
* Dice result
* Difficulty Class
* Final outcome

The AI cannot override the mechanical result.

---

# 7. Contested Checks

## 7.1 Contested Check Support

### Status: APPROVED

RealmWeaver V1 supports Contested Checks.

Examples may include:

* Stealth vs Perception
* Athletics vs Athletics
* Athletics vs Acrobatics
* Other directly opposed actions

---

## 7.2 Resolution

Each participant makes the appropriate mechanical check.

The rules engine compares the results and determines the winner.

The AI narrates the outcome after resolution.

---

# 8. Passive Checks

## 8.1 Passive Perception

### Status: APPROVED

RealmWeaver V1 supports at minimum:

**Passive Perception**

Baseline calculation:

**10 + applicable modifiers**

Applicable modifiers may include:

* Wisdom Modifier
* Proficiency
* Expertise
* Other supported effects

---

## 8.2 Passive Investigation and Insight

### Status: FUTURE / OPTIONAL V1 SUPPORT

The architecture may also support:

* Passive Investigation
* Passive Insight

These do not require the same minimum implementation priority as Passive Perception.

---

## 8.3 Hidden Passive Resolution

### Status: APPROVED

Passive checks may be used when asking the player to roll would reveal information the character should not know.

Examples:

* Hidden enemies
* Traps
* Secret doors
* NPC deception
* Suspicious environmental details
* Concealed activity

The player should not be notified that a hidden passive comparison occurred if doing so would reveal secret information.

---

# 9. Natural 20 and Natural 1 on Ability Checks

## 9.1 Natural 20

### Status: APPROVED

A Natural 20 on an ordinary Ability Check does not automatically succeed.

If the action is Impossible, it remains Impossible.

If the action is mechanically possible, the final total is compared to the DC normally.

---

## 9.2 Natural 1

### Status: APPROVED

A Natural 1 on an ordinary Ability Check does not automatically fail.

If the final modified total still meets the DC, the check may succeed.

---

## 9.3 Combat Exception

Attack-roll Natural 20 and Natural 1 behaviour is different.

Combat-specific rules are defined in:

`04_COMBAT.md`

---

# 10. Degrees of Success and Failure

## 10.1 V1 Narrative Degrees

### Status: APPROVED

V1 uses **narrative degrees of success and failure**.

The rules engine calculates:

**Margin = Final Result - Target DC**

Example:

```text
Roll Total: 24
DC: 15

Margin: +9
Result: Success
```

The AI may use the margin to influence how strongly the result is narrated.

---

## 10.2 Example Narrative Interpretation

A narrow success may be narrated differently from a strong success.

Example:

```text
DC: 15
Result: 15
Margin: 0
```

may represent a barely successful outcome.

Whereas:

```text
DC: 15
Result: 25
Margin: +10
```

may be narrated as a particularly confident or impressive success.

---

## 10.3 No Automatic Mechanical Margin Effects in V1

### Status: APPROVED

The margin does not automatically create additional mechanical effects in V1.

It does not automatically grant:

* Extra damage
* Extra XP
* Bonus loot
* Additional information
* Automatic complications
* Extra penalties

unless another explicit rule independently creates such an effect.

---

## 10.4 Future Mechanical Degrees of Success

### Status: APPROVED FUTURE DIRECTION

Future RealmWeaver versions may introduce mechanical degrees of success and failure.

Possible future effects include:

* Additional information
* Improved rewards
* Faster completion
* Better-quality results
* Tool damage
* Complications
* Additional consequences

The V1 engine should retain the calculated margin so this feature can later be introduced without redesigning basic check resolution.

---

# 11. Free-Text Action Choice

## 11.1 No Fixed Action Requirement

### Status: APPROVED

RealmWeaver may suggest mechanically reasonable approaches such as:

* Persuasion
* Deception
* Intimidation
* Stealth
* Athletics
* Investigation

However, the player must not be restricted to only those suggested choices.

Free-text actions remain available.

---

## 11.2 AI Interpretation

The AI interprets the player's chosen approach and maps it to an appropriate mechanical request where necessary.

Example:

```text
Player:
"I pretend to be one of the merchant's guards and confidently walk through the gate."
```

The AI may interpret this as:

```text
Charisma (Deception)
```

rather than forcing the player to manually select Deception beforehand.

---

# 12. Check Resolution Flow

## 12.1 Standard Flow

### Status: APPROVED

A typical check follows:

```text
Player describes action
        ↓
AI interprets intent
        ↓
Automatic / Check / Impossible?
        ↓
If Check:
Ability + Skill + Difficulty proposed
        ↓
Rules engine validates
        ↓
Dice resolved
        ↓
Modifiers applied
        ↓
DC comparison
        ↓
Success / Failure + Margin
        ↓
AI narrates result
```

This preserves natural-language gameplay while maintaining deterministic mechanical authority.

---

# 13. Rules Authority

The AI may:

* Interpret player intent
* Suggest checks
* Suggest Skills
* Suggest Abilities
* Suggest difficulty categories
* Narrate outcomes

The deterministic rules engine controls:

* Whether the proposed check is mechanically valid
* Modifiers
* Proficiency
* Expertise
* DC mapping
* Dice result
* Final total
* Success/failure
* Margin
* State changes

The AI cannot replace the authoritative result with a narratively preferred outcome.

---

# 14. Group Status

**Group 2 — Checks & Saving Throws: APPROVED**

Approved areas:

1. Action Resolution
2. Ability Checks
3. AI Check Interpretation
4. Difficulty Classes
5. Saving Throws
6. Contested Checks
7. Passive Checks
8. Natural 20 / Natural 1 Ability Check Behaviour
9. Narrative Degrees of Success and Failure
10. Free-Text Action Resolution

Related specifications:

* Character statistics → `01_CHARACTER_CORE.md`
* Dice mechanics → `03_DICE_AND_INSPIRATION.md`
* Combat checks and attacks → `04_COMBAT.md`
* Class Saving Throw proficiencies → `05_CLASSES_AND_PROGRESSION.md`
