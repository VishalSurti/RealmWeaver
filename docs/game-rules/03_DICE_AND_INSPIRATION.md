# RealmWeaver — Dice & Inspiration

**Rule Group:** 3
**Status:** APPROVED
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification
**Last Reviewed:** 13 August 2026

---

# 1. Purpose

This document defines RealmWeaver V1's dice and Inspiration mechanics.

It covers:

* Supported dice
* Dice expressions
* Automatic dice
* Manual physical dice
* Input validation
* Randomness
* Advantage
* Disadvantage
* Natural d20 behaviour
* Hidden rolls
* Dice history
* Inspiration
* Rules authority

The dice system is responsible for producing or validating authoritative roll results.

The AI may request a roll or narrate its outcome, but it may not choose, alter or override the result.

---

# 2. Supported Dice

## 2.1 Dice Types

### Status: APPROVED

RealmWeaver V1 supports:

* d4
* d6
* d8
* d10
* d12
* d20
* d100

These dice may be used by:

* Ability Checks
* Saving Throws
* Attack Rolls
* Damage
* Healing
* Class Features
* Species Features
* Spell Effects
* Random Tables
* Other supported mechanics

---

## 2.2 Dice Expressions

### Status: APPROVED

The dice engine must support multiple-dice expressions.

Examples:

```text
1d20
2d6
3d8
4d6
2d6+3
1d8-1
```

A dice expression may contain:

* Number of dice
* Die size
* Positive static modifier
* Negative static modifier

The engine should preserve both the individual rolls and the final calculated result where useful.

Example:

```text
Expression: 2d6 + 3

Rolls:
4
6

Dice Total: 10
Modifier: +3
Final Total: 13
```

---

# 3. Dice Modes

## 3.1 Automatic Dice

### Status: APPROVED

In Automatic Dice mode, RealmWeaver generates the required dice result.

Example:

```text
Required Roll:
1d20

RealmWeaver Result:
17
```

The generated result becomes the authoritative mechanical result.

---

## 3.2 Manual Physical Dice

### Status: APPROVED

In Manual Dice mode, RealmWeaver tells the player which die or dice must be rolled.

The player rolls physical dice and enters the result.

Example:

```text
RealmWeaver:
Roll 1d20.

Player enters:
17
```

The entered result becomes authoritative once validated.

---

## 3.3 Changing Dice Mode

### Status: APPROVED

The player may change between:

* Automatic Dice
* Manual Dice

during an active campaign.

Dice Mode is not permanently locked during campaign creation.

---

# 4. Manual Dice Validation

## 4.1 Valid Range

### Status: APPROVED

Manual dice results must be within the possible range of the requested roll.

Examples:

### 1d20

Valid:

```text
1–20
```

### 1d8

Valid:

```text
1–8
```

### 2d6 Total Entry

Valid:

```text
2–12
```

Invalid values must be rejected.

---

## 4.2 Individual Dice

Where a mechanic requires the individual dice values rather than only their sum, RealmWeaver may request each die separately.

This is especially important for mechanics such as:

* Advantage
* Disadvantage
* Critical damage
* Drop-lowest stat generation
* Reroll features
* Other dice-manipulation mechanics

Example:

```text
Roll 2d20.

Die 1: 8
Die 2: 17
```

rather than only:

```text
Total: 25
```

because the system must know which d20 result applies.

---

## 4.3 Manual Dice and Honesty

### Status: APPROVED

RealmWeaver does not implement an anti-cheat system for manually entered physical dice.

The project is primarily a personal single-player experience.

A player who deliberately enters an incorrect result only changes their own game experience.

---

# 5. Randomness

## 5.1 Unbiased System Rolls

### Status: APPROVED

System-generated dice should use unbiased pseudo-random generation appropriate for gameplay.

RealmWeaver must not deliberately alter results merely to:

* Make the story more dramatic
* Protect the player
* Harm the player
* Avoid repeated values
* Create predetermined outcomes

---

## 5.2 Repeated Results

### Status: APPROVED

Repeated identical dice results are valid.

Example:

```text
17
17
17
17
```

is possible.

RealmWeaver must not prevent a number from appearing repeatedly.

---

## 5.3 Rejected Anti-Repetition Rule

The previously considered rule:

> The same number cannot occur more than three consecutive times.

is explicitly rejected.

Such a rule would make the dice system less random by modifying the natural distribution of results.

---

# 6. Advantage

## 6.1 Advantage Rule

### Status: APPROVED

When a d20 roll has Advantage:

> **Roll 2d20 and use the higher result.**

Example:

```text
Rolls:
8
17

Result Used:
17
```

Relevant modifiers are then applied according to the underlying mechanic.

---

# 7. Disadvantage

## 7.1 Disadvantage Rule

### Status: APPROVED

When a d20 roll has Disadvantage:

> **Roll 2d20 and use the lower result.**

Example:

```text
Rolls:
16
5

Result Used:
5
```

---

# 8. Advantage and Disadvantage Interaction

## 8.1 Cancellation

### Status: APPROVED

If Advantage and Disadvantage both apply to the same roll:

> **They cancel.**

The character makes a normal d20 roll.

Example:

```text
1 source of Advantage
+
1 source of Disadvantage

→ Normal 1d20
```

---

## 8.2 Multiple Sources

### Status: APPROVED

Multiple sources of Advantage do not stack in V1.

Example:

```text
Advantage source 1
Advantage source 2

→ Still roll 2d20 and take the higher.
```

Multiple sources of Disadvantage also do not stack.

---

# 9. Natural d20 Results

## 9.1 Ability Checks

### Status: APPROVED

For ordinary Ability Checks:

* Natural 20 is not automatic success.
* Natural 1 is not automatic failure.

The final modified total and action possibility determine the result.

Detailed rules are defined in:

`02_CHECKS_AND_SAVES.md`

---

## 9.2 Attack Rolls

Attack-roll Natural 20 and Natural 1 behaviour is defined in:

`04_COMBAT.md`

For attacks:

* Natural 20 normally automatically hits and critically hits.
* Natural 1 normally automatically misses.

Supported class or other features may modify these rules.

---

## 9.3 Feature-Based Dice Overrides

### Status: APPROVED

Character features may modify normal dice behaviour.

Examples include:

* Champion Improved Critical
* Halfling Lucky
* Future reroll mechanics
* Other supported class/species abilities

The rules engine detects and resolves such features.

The AI does not need to remember to apply them.

---

# 10. Rerolls

## 10.1 General Reroll Principle

### Status: APPROVED

When a supported mechanic grants a reroll, the rules engine determines:

* Whether the reroll is eligible
* Which die is rerolled
* Whether the original result is discarded
* Whether the new result must be used
* Whether additional rerolls are permitted

The AI cannot arbitrarily grant rerolls.

---

## 10.2 Example — Halfling Lucky

An eligible Natural 1 may trigger Halfling Lucky according to the supported Species rule.

Conceptually:

```text
d20 = 1
↓
Halfling Lucky detected
↓
Reroll eligible
↓
New d20 result
```

The exact Species rule is defined in:

`05_CLASSES_AND_PROGRESSION.md`

---

## 10.3 Example — Level-Up HP Natural 1

When using rolled HP during level-up, a Natural 1 on the class Hit Die is rejected and rerolled according to the approved progression rules.

This rule is defined in:

`05_CLASSES_AND_PROGRESSION.md`

It is not a universal rule for all d20 or damage rolls.

---

# 11. Hidden Rolls

## 11.1 Hidden DM/System Rolls

### Status: APPROVED

RealmWeaver supports hidden dice rolls.

Potential uses include:

* NPC deception
* Enemy stealth
* Secret Perception comparisons
* Hidden random events
* Secret campaign mechanics
* Other checks where revealing the roll would reveal hidden information

---

## 11.2 Player Visibility

The player may not be informed that a hidden roll occurred when doing so would reveal information their character does not possess.

Example:

An enemy attempting to sneak toward the player may make a hidden Stealth-related roll.

RealmWeaver should not display:

```text
Enemy Stealth Roll: 18
```

to the player.

---

## 11.3 Hidden Roll Authority

Hidden rolls are still resolved by the deterministic dice/rules systems.

The AI cannot invent their outcomes.

---

# 12. Dice History

## 12.1 Player-Visible Dice History

### Status: APPROVED

RealmWeaver maintains a player-accessible history of visible rolls.

An entry may include:

* Roll type
* Dice expression
* Individual dice
* Selected die where Advantage/Disadvantage applies
* Modifier
* Final total
* Mechanical result

Example:

```text
Persuasion Check

d20: 14
Charisma Modifier: +3
Proficiency: +2

Final Total: 19
Result: Success
```

---

## 12.2 Advantage Example

A visible Advantage roll may show:

```text
Attack Roll — Advantage

d20 #1: 8
d20 #2: 17

Selected: 17
Modifier: +5
Final Total: 22

Result: Hit
```

---

## 12.3 Hidden Rolls and History

### Status: APPROVED

Hidden DM/system rolls must not appear in player-visible dice history.

They may still be recorded internally for:

* Debugging
* Testing
* Auditability

if later architecture determines this is appropriate.

Internal storage must not leak hidden information to the player interface.

---

# 13. Inspiration

## 13.1 Inspiration Capacity

### Status: APPROVED

A character may hold a maximum of:

**1 Inspiration**

Conceptually:

```text
inspiration = 0 | 1
```

A character cannot accumulate multiple Inspiration points in V1.

---

## 13.2 Inspiration Award Criteria

### Status: APPROVED

Inspiration may be awarded for appropriate moments such as:

* Strong roleplay
* Creative problem solving
* Meaningful character decisions
* Acting consistently with important character traits
* Significant narrative moments

---

## 13.3 AI Role in Inspiration Awards

### Status: APPROVED

The AI may recommend or request an Inspiration award.

Example conceptually:

```text
award_inspiration
reason = meaningful_character_decision
```

The deterministic game system validates and applies the award.

The AI cannot directly modify Inspiration state.

---

## 13.4 Preventing Over-Awarding

### Status: APPROVED DIRECTION

Inspiration should not be awarded casually for every positive interaction.

The exact frequency and AI-award controls will be defined later in:

* Group 9 — AI / Rules Boundary
* M2.6 — AI Architecture

The maximum-one rule provides an additional natural limit.

---

# 14. Spending Inspiration

## 14.1 Inspiration Effect

### Status: APPROVED

The player may spend Inspiration to gain Advantage on an eligible d20 roll.

---

## 14.2 Inspiration Timing

### Status: APPROVED

Inspiration must be declared:

> **Before the roll occurs.**

The player cannot wait to see the result and then retroactively spend Inspiration.

---

## 14.3 Example

```text
RealmWeaver:
Make a Persuasion Check.

Inspiration Available: 1

Player:
Use Inspiration.

↓
Roll with Advantage.

Rolls:
8
17

Selected:
17
```

---

## 14.4 Hidden Difficulty

The player does not necessarily know the target DC before deciding whether to spend Inspiration.

Where the DC should remain hidden, RealmWeaver may tell the player:

* What check is required
* That Inspiration is available

without revealing the target number.

---

## 14.5 Inspiration State Change

When Inspiration is validly spent:

```text
Inspiration:
1 → 0
```

The rules engine performs this change.

A character cannot spend Inspiration they do not possess.

---

# 15. Inspiration and Advantage

## 15.1 Existing Advantage

If the character already has Advantage from another source, spending Inspiration does not create additional stacking Advantage in V1.

The normal non-stacking Advantage rules apply.

---

## 15.2 Advantage and Disadvantage

If Inspiration grants Advantage while another mechanic grants Disadvantage:

> Advantage and Disadvantage cancel according to the standard rule.

The player should be able to understand this before confirming the Inspiration spend where practical.

---

# 16. Dice Resolution Authority

## 16.1 Rules-System Responsibilities

The deterministic dice/rules systems control:

* Which dice are required
* Number of dice
* Dice size
* Valid manual ranges
* Advantage
* Disadvantage
* Rerolls
* Static modifiers
* Selected die
* Final authoritative result
* Feature-based dice modifications

---

## 16.2 AI Responsibilities

The AI may:

* Request an appropriate roll
* Explain why a roll is happening
* Narrate the final result
* Recommend Inspiration where appropriate
* Describe visible dice outcomes

The AI may not:

* Choose a die result
* Modify a completed die result
* Secretly fudge automatic rolls
* Ignore valid reroll features
* Invent Advantage
* Invent Disadvantage
* Grant Inspiration directly
* Spend Inspiration directly
* Rewrite an authoritative roll because it prefers another narrative outcome

---

# 17. Standard Dice Resolution Flow

## 17.1 Automatic Dice

```text
Mechanical Roll Required
        ↓
Determine Dice Expression
        ↓
Apply Advantage / Disadvantage
        ↓
Check Eligible Features
        ↓
Generate Dice
        ↓
Resolve Rerolls if Required
        ↓
Apply Modifiers
        ↓
Produce Authoritative Result
        ↓
Record Visible Roll if Applicable
        ↓
Return Result to AI
        ↓
AI Narrates
```

---

## 17.2 Manual Dice

```text
Mechanical Roll Required
        ↓
Determine Dice Expression
        ↓
Tell Player What to Roll
        ↓
Player Enters Result(s)
        ↓
Validate Range
        ↓
Resolve Eligible Rerolls
        ↓
Apply Modifiers
        ↓
Produce Authoritative Result
        ↓
Record Visible Roll if Applicable
        ↓
Return Result to AI
        ↓
AI Narrates
```

---

# 18. Group Status

**Group 3 — Dice & Inspiration: APPROVED**

Approved areas:

1. Supported Dice
2. Dice Expressions
3. Automatic Dice
4. Manual Physical Dice
5. Manual Input Validation
6. Unbiased Randomness
7. Advantage
8. Disadvantage
9. Advantage / Disadvantage Cancellation
10. Natural d20 Behaviour
11. Feature-Based Rerolls
12. Hidden Rolls
13. Dice History
14. Inspiration Awards
15. Inspiration Spending
16. Inspiration Before-Roll Requirement
17. Dice Resolution Authority

Related specifications:

* Ability Checks → `02_CHECKS_AND_SAVES.md`
* Combat Attack/Critical Rules → `04_COMBAT.md`
* Class and Species Dice Features → `05_CLASSES_AND_PROGRESSION.md`
