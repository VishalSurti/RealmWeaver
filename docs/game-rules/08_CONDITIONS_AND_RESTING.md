# 08 — Conditions & Resting

**Status:** Complete - Approved  
**Rules Baseline:** SRD 5.1 / 2014-style mechanics  
**Scope:** RealmWeaver V1, with architecture designed for future expansion

---

## Purpose

This document defines RealmWeaver's rules for conditions, condition duration and removal, resting, exhaustion, recovery, and the interaction between these mechanics and the AI Dungeon Master.

Conditions and resting are authoritative mechanical systems.

The AI may describe, interpret, propose, and narrate these mechanics, but it does not directly determine or modify authoritative mechanical state.

RealmWeaver follows the project's core principles:

> **AI tells the story. Rules decide what happens.**

> **AI proposes. RealmWeaver validates.**

The deterministic rules engine and persistent campaign state are authoritative over condition application, duration, removal, rest completion, recovery, and related mechanical effects.

V1 uses SRD 5.1 / 2014-style mechanics as its primary baseline. Any RealmWeaver-specific adaptations are explicitly documented.

---

# 8A — Core Condition Model

## 8A.1 Conditions Are Structured Mechanical State

Conditions are not represented only through AI narration.

A mechanically affected creature stores authoritative condition state that can be evaluated by the rules engine.

Conceptually:

```text
ConditionInstance
- condition_type
- target
- source
- applied_at
- duration
- expiration
- save_end_rule
- concentration_dependency
- visibility
- metadata
```

The exact implementation schema is deferred to architecture design.

---

## 8A.2 Conditions Are Applied Through Validated Effects

The AI cannot directly assign a condition.

Instead, a valid action, spell, feature, environmental effect, or other mechanical source produces a condition proposal or effect.

Example:

```text
Spell successfully resolves
        ↓
Effect requests CONDITION_APPLIED
        ↓
RealmWeaver validates target/rules
        ↓
Condition stored
        ↓
AI narrates outcome
```

This prevents narrative wording from accidentally modifying game mechanics.

---

## 8A.3 Narrative Language Does Not Automatically Create Conditions

AI narration and mechanical terminology are distinct.

For example:

> "The revelation leaves the guard stunned."

does not automatically apply the mechanical `STUNNED` condition.

Likewise:

> "The creature looks frightened."

does not necessarily mean that the creature has the mechanical `FRIGHTENED` condition.

A mechanical condition exists only when a validated rule or effect applies it.

---

## 8A.4 Conditions Have Sources

Condition instances should retain their source where relevant.

Examples:

```text
POISONED
source = Giant Spider Venom
```

```text
FRIGHTENED
source = Dragon Fear
```

```text
RESTRAINED
source = Web Spell
```

Source tracking allows RealmWeaver to correctly resolve:

* duration;
* removal;
* concentration dependencies;
* saving throws;
* source-specific immunities;
* overlapping effects;
* mechanical history.

---

## 8A.5 Multiple Sources May Apply the Same Condition

A creature may be affected by the same condition from multiple independent sources.

RealmWeaver must not assume:

```text
condition = true / false
```

is sufficient for all cases.

Example:

```text
POISONED
Source A — expires in 3 rounds
Source B — expires in 1 hour
```

Removing or expiring Source A does not remove the condition if Source B still applies.

The rules engine therefore tracks condition instances/sources rather than relying only on a single Boolean flag.

---

## 8A.6 Condition Effects Are Derived From Rules

Conditions reference authoritative mechanical definitions.

Example:

```text
POISONED
→ disadvantage on attack rolls
→ disadvantage on ability checks
```

The AI is not responsible for remembering or enforcing those effects.

When a relevant roll occurs, RealmWeaver checks active conditions and applies the appropriate modifiers.

---

## 8A.7 Conditions Integrate With the Shared Modifier System

Condition effects should feed into existing mechanics such as:

* advantage/disadvantage;
* movement;
* attack resolution;
* saving throws;
* ability checks;
* action availability;
* targeting;
* perception;
* critical-hit handling where applicable.

Condition logic should not create an entirely separate resolution system.

---

## 8A.8 Conditions Are Persistent

Active conditions are part of authoritative campaign state.

Saving and reloading a campaign preserves applicable condition state.

Example:

```text
Character:
POISONED
remaining duration = 34 minutes
```

Reloading the campaign does not require the AI to reconstruct the condition from conversation history.

---

## 8A.9 Conditions Generate Mechanical Events

Important condition changes should generate structured events.

Examples:

```text
CONDITION_APPLIED
CONDITION_REMOVED
CONDITION_EXPIRED
CONDITION_SAVE_SUCCEEDED
CONDITION_SAVE_FAILED
```

These events support:

* debugging;
* campaign persistence;
* AI narration;
* UI updates;
* mechanical history.

---

## 8A.10 AI Narrates Validated Condition Results

The preferred authority flow is:

```text
PLAYER / NPC ACTION
        ↓
AI interprets intent if necessary
        ↓
REALMWEAVER validates mechanics
        ↓
RULES ENGINE resolves result
        ↓
CONDITION STATE changes
        ↓
AI narrates validated outcome
```

The AI describes what happened after RealmWeaver establishes the mechanical result.

---

# 8B — Standard Conditions

## 8B.1 V1 Condition Baseline

RealmWeaver V1 supports the standard SRD 5.1 / 2014-style conditions required by the supported rules content.

The core condition set is:

* Blinded
* Charmed
* Deafened
* Frightened
* Grappled
* Incapacitated
* Invisible
* Paralyzed
* Petrified
* Poisoned
* Prone
* Restrained
* Stunned
* Unconscious

Exhaustion is handled separately in Section 8D because it is a level-based state rather than an ordinary binary condition.

---

## 8B.2 Blinded

A blinded creature:

* cannot see;
* automatically fails ability checks requiring sight;
* has disadvantage on its attack rolls;
* grants advantage to attack rolls made against it.

RealmWeaver applies these effects automatically where mechanically relevant.

---

## 8B.3 Charmed

A charmed creature:

* cannot attack the charmer;
* cannot target the charmer with harmful abilities or magical effects;
* gives the charmer advantage on applicable social interaction checks against it.

Charmed does not mean the target automatically obeys every command.

AI narration must respect this distinction.

---

## 8B.4 Deafened

A deafened creature:

* cannot hear;
* automatically fails ability checks requiring hearing.

The AI may use this state when determining what information is narratively available to the creature.

RealmWeaver remains authoritative over mechanical consequences.

---

## 8B.5 Frightened

While the source of its fear is within the applicable perception/visibility requirement, a frightened creature:

* has disadvantage on ability checks;
* has disadvantage on attack rolls;
* cannot willingly move closer to the source of its fear.

RealmWeaver validates movement attempts against the condition.

The AI may narrate fear but does not decide whether the mechanical restriction applies.

---

## 8B.6 Grappled

A grappled creature has its movement speed reduced to zero.

The condition ends if:

* the grappler becomes incapacitated; or
* an effect removes the grappled creature from the grappler's effective reach.

Because RealmWeaver uses distance-band combat, grapple reach and forced separation must be interpreted through the distance-band movement model.

---

## 8B.7 Incapacitated

An incapacitated creature:

* cannot take actions;
* cannot take reactions.

Other mechanics that reference Incapacitated use this authoritative condition state.

---

## 8B.8 Invisible

An invisible creature:

* cannot be seen without appropriate magic or special senses;
* is treated as heavily obscured for purposes of hiding;
* gains advantage on applicable attack rolls;
* grants disadvantage to applicable attack rolls against it.

The creature's location is not necessarily automatically unknown merely because it is Invisible.

Detection, hiding, sound, perception, and special senses remain separate mechanics.

---

## 8B.9 Paralyzed

A paralyzed creature:

* is Incapacitated;
* cannot move;
* cannot speak;
* automatically fails Strength saving throws;
* automatically fails Dexterity saving throws;
* grants advantage to attack rolls against it;
* suffers critical hits from applicable attacks made within the baseline close-range requirement.

RealmWeaver's distance-band system must translate the close-range critical-hit requirement consistently.

---

## 8B.10 Petrified

A petrified creature is transformed, along with applicable nonmagical carried/worn equipment, into an inert solid substance.

The creature:

* is Incapacitated;
* cannot move;
* cannot speak;
* is unaware of its surroundings;
* grants advantage to attacks against it;
* automatically fails Strength saving throws;
* automatically fails Dexterity saving throws;
* gains resistance to applicable damage;
* is immune to poison and disease under the baseline rules, with existing poison/disease suspended rather than automatically removed where applicable.

The detailed transformation and restoration effects follow the adopted baseline.

---

## 8B.11 Poisoned

A poisoned creature has disadvantage on:

* attack rolls;
* ability checks.

RealmWeaver automatically integrates this with the shared advantage/disadvantage resolver.

---

## 8B.12 Prone

A prone creature:

* has restricted movement;
* has disadvantage on its own attack rolls;
* grants advantage or disadvantage to attackers depending on the applicable attack distance/rule.

Standing up consumes the appropriate portion of movement capacity.

Because RealmWeaver uses distance bands rather than exact grid movement, standing and attack-distance effects are translated through the movement/distance-band system.

---

## 8B.13 Restrained

A restrained creature:

* has movement speed reduced to zero;
* grants advantage to attack rolls against it;
* has disadvantage on its own attack rolls;
* has disadvantage on Dexterity saving throws.

Forced movement or specific effects may still reposition a restrained creature where the originating rule permits it.

---

## 8B.14 Stunned

A stunned creature:

* is Incapacitated;
* cannot move;
* can speak only falteringly;
* automatically fails Strength saving throws;
* automatically fails Dexterity saving throws;
* grants advantage to attack rolls against it.

---

## 8B.15 Unconscious

An unconscious creature:

* is Incapacitated;
* cannot move;
* cannot speak;
* is unaware of its surroundings;
* drops held items;
* falls Prone where applicable;
* automatically fails Strength saving throws;
* automatically fails Dexterity saving throws;
* grants advantage to attack rolls against it;
* suffers critical hits from applicable close-range attacks under the baseline rules.

Unconscious interacts with the separate death/dying system where applicable.

---

## 8B.16 Condition Dependencies

Some conditions incorporate the effects of other conditions.

Examples:

```text
PARALYZED
→ includes INCAPACITATED
```

```text
STUNNED
→ includes INCAPACITATED
```

```text
UNCONSCIOUS
→ includes INCAPACITATED
```

RealmWeaver should represent these relationships through reusable rule effects rather than duplicating unrelated logic throughout the codebase.

---

## 8B.17 Hidden Conditions

Not every condition must automatically be revealed to the player.

Example:

```text
NPC:
CHARMED

Player visibility:
HIDDEN
```

RealmWeaver may know the authoritative state while presenting only information the player's character can reasonably perceive.

This supports mystery, deception, hidden magical effects, and NPC secrets.

---

## 8B.18 Condition UI

Player-visible active conditions should be accessible without permanently occupying large amounts of campaign space.

A compact interface may show:

```text
CONDITIONS
Poisoned
Restrained
```

Selecting a condition can reveal:

* mechanical effects;
* known source;
* known duration;
* removal requirements;
* saving throw information where appropriate.

Hidden mechanical information must not be exposed through the UI.

---

# 8C — Condition Duration, Removal & Interaction

## 8C.1 Conditions Require Explicit Duration Rules

RealmWeaver should not depend on the AI remembering when a condition ends.

A condition may use duration models such as:

```text
UNTIL_END_OF_SOURCE_NEXT_TURN
UNTIL_START_OF_TARGET_NEXT_TURN
ROUNDS
MINUTES
HOURS
UNTIL_SAVE_SUCCEEDS
WHILE_CONCENTRATION
UNTIL_SOURCE_ENDS
PERMANENT_UNTIL_REMOVED
```

The exact internal representation is deferred to architecture.

---

## 8C.2 Round-Based Durations

Combat conditions may expire at defined combat timing points.

Examples:

```text
until start of caster's next turn
until end of target's next turn
for 1 round
```

RealmWeaver's combat/turn engine determines when these durations expire.

The AI does not manually count turns.

---

## 8C.3 Time-Based Durations

Conditions lasting minutes or hours use the authoritative campaign clock.

Example:

```text
POISONED
duration = 1 hour
applied_at = 14:20
expires_at = 15:20
```

If a Short Rest advances campaign time from 14:30 to 15:30, the condition expires during that interval at 15:20.

RealmWeaver must process the expiration rather than waiting for the AI to notice it.

---

## 8C.4 Saving-Throw-Based Removal

Some effects allow repeated saving throws.

Example:

```text
At end of each turn:
Wisdom save

Success:
condition ends

Failure:
condition continues
```

RealmWeaver determines:

* when the save occurs;
* applicable DC;
* modifiers;
* advantage/disadvantage;
* result;
* condition removal.

The dice-control preference determines whether the player or system performs applicable rolls.

---

## 8C.5 Concentration-Dependent Conditions

If a condition depends on a concentration spell:

```text
Spell
→ applies condition
→ requires concentration
```

then ending concentration removes the applicable condition/effect when the spell rules require it.

RealmWeaver tracks the dependency explicitly.

Example:

```text
condition.source = spell_instance
condition.requires_source_active = true
```

The AI does not manually decide whether the condition remains.

---

## 8C.6 Source-Dependent Conditions

Some conditions exist only while their source remains mechanically relevant.

Example:

```text
condition:
FRIGHTENED

source:
specific creature/effect
```

RealmWeaver retains enough source information to evaluate rules that depend on:

* distance;
* visibility;
* source existence;
* source state;
* effect duration.

---

## 8C.7 Condition Removal Requires Valid Mechanical Cause

A condition may end because:

* its duration expires;
* a required save succeeds;
* its source ends;
* concentration ends;
* a spell or feature removes it;
* an environmental/mechanical requirement is satisfied;
* another explicit rule ends it.

AI narration alone cannot remove the condition.

Example:

> "After calming down, you no longer feel afraid."

does not remove `FRIGHTENED` unless the applicable mechanical rule has actually ended it.

---

## 8C.8 Rest Does Not Automatically Remove Conditions

Neither Short Rest nor Long Rest should globally execute:

```text
conditions = []
```

Each condition/effect defines its own ending rules.

A particular effect may explicitly specify:

```text
ends_on = SHORT_REST
```

or:

```text
ends_on = LONG_REST
```

but this is effect-specific.

---

## 8C.9 Overlapping Identical Conditions

If multiple independent sources impose the same condition, ending one source does not necessarily end the condition.

Example:

```text
POISONED
Source A — active
Source B — active
```

Source A expires:

```text
POISONED
Source B — still active
```

The target therefore remains Poisoned.

---

## 8C.10 Overlapping Conditions Do Not Normally Stack Their Identical Effect

If two sources both impose disadvantage on the same roll through the same condition, they do not create "double disadvantage."

RealmWeaver sends all relevant advantage/disadvantage sources into the shared resolver.

Example:

```text
Poisoned → Disadvantage
Other effect → Disadvantage
One source → Advantage

Final:
Normal Roll
```

following the adopted advantage/disadvantage rules.

---

## 8C.11 Immunity Is Validated Before Application

If a creature is immune to a condition:

```text
CONDITION_APPLICATION_REQUEST
        ↓
check immunity
        ↓
IMMUNE
        ↓
condition not applied
```

The AI may narrate the failed attempt appropriately after RealmWeaver resolves it.

---

## 8C.12 Condition Removal Does Not Rewrite History

When a condition ends, RealmWeaver removes the active mechanical effect but retains relevant event history.

Example:

```text
14:20 — POISONED applied
14:48 — saving throw succeeded
14:48 — POISONED removed
```

This supports debugging, campaign summaries, and AI narrative continuity.

---

## 8C.13 Conditions Persist Across Scene Changes

Leaving combat, travelling to another location, starting dialogue, or changing UI screens does not automatically remove a condition.

Conditions persist until their actual removal rule is satisfied.

Example:

```text
Combat ends
↓
POISONED still has 42 minutes remaining
↓
Exploration begins
↓
POISONED remains active
```

---

## 8C.14 Campaign Time Controls Persistent Durations

Conditions with real in-world durations use campaign time rather than:

* real-world time;
* number of AI messages;
* number of scenes;
* application uptime.

Closing RealmWeaver for two real-world days does not consume two campaign days.

---

## 8C.15 Time Advancement Must Process Expirations

If RealmWeaver advances time across multiple scheduled expirations, it must process them in chronological order.

Example:

```text
Current Time:
14:00

Advance to:
16:00

14:30 — Condition A expires
15:00 — Spell B expires
15:20 — Condition C save/event
16:00 — Destination reached
```

The clock cannot simply jump to 16:00 and ignore intermediate mechanical events.

Detailed World Clock/event scheduling architecture is deferred to the future World & Campaign State design.

---

## 8C.16 Condition Changes Are Authoritative Events

Condition lifecycle changes should generate structured events such as:

```text
CONDITION_APPLIED
CONDITION_SAVE_ATTEMPTED
CONDITION_REMOVED
CONDITION_EXPIRED
```

These events can be consumed by:

* UI;
* rules systems;
* persistence;
* AI narration;
* campaign history;
* debugging tools.

---

## 8C.17 AI Failure Does Not Undo Condition Resolution

If RealmWeaver successfully resolves and stores:

```text
CONDITION_APPLIED:
PRONE
```

but the subsequent AI narration fails or times out, the mechanical result remains valid.

Retrying narration must not reroll or reapply the condition.

---

## 8C.18 Condition Authority Flow

The intended flow is:

```text
SOURCE EFFECT
      ↓
CONDITION PROPOSED
      ↓
REALMWEAVER VALIDATES
      ↓
IMMUNITY / RULE CHECKS
      ↓
CONDITION INSTANCE CREATED
      ↓
RULE EFFECTS ACTIVE
      ↓
DURATION / SAVE / SOURCE TRACKED
      ↓
REMOVAL TRIGGER OCCURS
      ↓
CONDITION REMOVED
      ↓
AI NARRATES VALIDATED STATE
```

This keeps condition handling deterministic while allowing the AI DM to provide flexible narrative presentation.

---

# 8D — Exhaustion

## 8D.1 Exhaustion Is a Level-Based State

Exhaustion is separate from RealmWeaver's ordinary binary condition model.

It represents severe physical or mental depletion caused by mechanics such as:

- prolonged lack of rest;
- forced travel;
- starvation or dehydration;
- environmental hazards;
- specific abilities;
- magical effects;
- other explicitly defined mechanics.

RealmWeaver tracks Exhaustion as an authoritative level:

```text
exhaustion_level = 0..6
````

The effects of Exhaustion are cumulative.

---

## 8D.2 Exhaustion Levels

RealmWeaver V1 uses the SRD 5.1 / 2014 six-level Exhaustion system.

| Level | Effect                                         |
| ----- | ---------------------------------------------- |
| 1     | Disadvantage on ability checks                 |
| 2     | Speed halved                                   |
| 3     | Disadvantage on attack rolls and saving throws |
| 4     | Hit Point maximum halved                       |
| 5     | Speed reduced to 0                             |
| 6     | Death                                          |

A creature at Exhaustion Level 3 therefore suffers the effects of Levels 1, 2, and 3.

---

## 8D.3 Level 1 — Ability Checks

At Exhaustion Level 1:

```text
Ability Checks
→ Disadvantage
```

This feeds into RealmWeaver's shared advantage/disadvantage resolver.

If another valid effect grants advantage, the normal advantage/disadvantage cancellation rules apply.

---

## 8D.4 Level 2 — Movement

At Exhaustion Level 2:

```text
Movement Capacity
→ 50%
```

Because RealmWeaver uses distance bands rather than exact grid movement, this is represented through the movement-budget system.

RealmWeaver determines which distance-band transitions remain possible after the reduction.

---

## 8D.5 Level 3 — Attacks and Saving Throws

At Exhaustion Level 3:

```text
Attack Rolls
→ Disadvantage

Saving Throws
→ Disadvantage
```

This applies to applicable:

* weapon attacks;
* spell attacks;
* Strength saves;
* Dexterity saves;
* Constitution saves;
* Intelligence saves;
* Wisdom saves;
* Charisma saves.

---

## 8D.6 Level 4 — Maximum HP

At Exhaustion Level 4:

```text
Effective Maximum HP
→ 50%
```

RealmWeaver retains the character's underlying maximum HP separately.

Example:

```text
base_max_hp = 60
effective_max_hp = 30
```

If current HP exceeds the new effective maximum:

```text
Current HP:
48 → 30
```

The character's underlying HP progression is not permanently overwritten.

---

## 8D.7 Recovering From Level 4

When Exhaustion decreases below Level 4, the character's effective maximum HP returns to its normal value.

Current HP does not automatically increase.

Example:

```text
Before Recovery:
Current HP = 22
Effective Max HP = 30

Exhaustion 4 → 3

After Recovery:
Current HP = 22
Effective Max HP = 60
```

Normal healing is required to restore the missing HP.

---

## 8D.8 Level 5 — Speed 0

At Exhaustion Level 5:

```text
movement_speed = 0
```

This prevents ordinary voluntary movement.

It does not inherently prevent valid:

* forced movement;
* teleportation;
* carrying;
* dragging;
* movement caused by another creature or effect.

---

## 8D.9 Level 6 — Death

Reaching Exhaustion Level 6 causes death.

```text
Exhaustion = 6
→ DEATH
```

This does not trigger ordinary death saving throws.

The consequence is resolved mechanically by RealmWeaver rather than by AI discretion.

---

## 8D.10 Exhaustion Must Have a Valid Mechanical Source

The AI cannot arbitrarily assign Exhaustion because a character appears tired.

Instead:

```text
VALIDATED MECHANICAL TRIGGER
        ↓
applicable save/check if required
        ↓
RULES RESOLUTION
        ↓
GAIN_EXHAUSTION
```

Examples include:

```text
FORCED_MARCH
```

or:

```text
ENVIRONMENTAL_HAZARD
```

where the corresponding rules justify Exhaustion.

---

## 8D.11 AI May Create Exhaustion Risks

The AI may create or describe situations that could result in Exhaustion.

Example:

> The mountain pass is bitterly cold. Continuing through the night without adequate protection could be dangerous.

If the player continues, RealmWeaver evaluates the applicable environmental mechanics.

The AI creates the narrative situation.

RealmWeaver determines the mechanical consequence.

---

## 8D.12 Environmental Hazard Countermeasures

Environmental hazards may define structured:

* preventative measures;
* protections;
* mitigations;
* remedies.

Example:

```text
EXTREME_COLD

Potential Valid Protections:
- appropriate cold-weather clothing
- suitable shelter
- relevant magical protection
- cold resistance
- other validated protection
```

The AI may suggest contextually appropriate solutions.

Example:

> Heavy winter clothing would make crossing the pass considerably safer.

However, only RealmWeaver determines whether a proposed protection mechanically satisfies the hazard rules.

---

## 8D.13 Environmental Preparation as Gameplay

Environmental hazards should function as problems the player can prepare for rather than merely as unavoidable saving throws.

Possible player responses may include:

* purchasing suitable equipment;
* obtaining shelter;
* using magic;
* changing route;
* waiting for safer conditions;
* completing a quest for protective equipment;
* accepting the risk.

V1 should support a limited core environmental-hazard/countermeasure system rather than attempting a comprehensive survival simulation.

Detailed environmental systems are deferred to later World & Campaign State design.

---

## 8D.14 Player Warning of Known Exhaustion Risks

When a character can reasonably understand that an action carries an Exhaustion risk, RealmWeaver should communicate that risk.

Example:

> Continuing the forced march may require a Constitution saving throw and could cause Exhaustion.

Exact hidden information such as the DC does not have to be revealed if the character should not know it.

The objective is to allow meaningful player decisions without revealing inappropriate system information.

---

## 8D.15 Multiple Exhaustion Levels

Architecture should support effects capable of applying more than one level where a future validated rule requires it.

Example:

```text
GAIN_EXHAUSTION(2)
```

RealmWeaver immediately processes all newly reached thresholds.

Exhaustion remains bounded between:

```text
0..6
```

---

## 8D.16 Exhaustion Recovery

A Short Rest does not normally reduce Exhaustion.

```text
Exhaustion 3
→ Short Rest
→ Exhaustion 3
```

A qualifying Long Rest normally removes one level of Exhaustion:

```text
Exhaustion 3
→ Qualifying Long Rest
→ Exhaustion 2
```

A Long Rest does not normally reset Exhaustion to zero.

Other validated spells, abilities, or effects may remove Exhaustion where their rules explicitly permit it.

---

## 8D.17 AI Cannot Narratively Remove Exhaustion

AI narration such as:

> You relax at the tavern and feel completely refreshed.

does not mechanically reduce Exhaustion.

Only a validated recovery rule can change the authoritative Exhaustion level.

---

## 8D.18 NPC Exhaustion

Mechanically relevant NPCs may use the same Exhaustion system.

This includes:

* player characters;
* companions;
* mechanically materialised NPCs;
* important persistent NPCs where relevant.

RealmWeaver does not continuously simulate Exhaustion for every background NPC in the world.

---

## 8D.19 AI Awareness of Exhaustion

The AI may receive relevant Exhaustion state for narrative and tactical reasoning.

Example:

```text
NPC:
Exhaustion = 3
Movement reduced
Attack rolls disadvantaged
Saving throws disadvantaged
```

The AI may therefore decide that an exhausted NPC attempts to retreat.

RealmWeaver remains responsible for applying the actual penalties.

---

## 8D.20 Exhaustion UI

Exhaustion should be displayed separately from ordinary conditions.

Example:

```text
CONDITIONS
Poisoned

EXHAUSTION
Level 2 / 6
```

Expanded:

```text
EXHAUSTION — LEVEL 2

Active Effects:
• Disadvantage on ability checks
• Speed halved

Next Level:
• Disadvantage on attack rolls
• Disadvantage on saving throws
```

Showing the next level's consequence helps players make informed strategic decisions.

---

## 8D.21 Critical Exhaustion Warning

At Exhaustion Level 5, the UI should clearly communicate that another level causes death.

Example:

```text
EXHAUSTION — LEVEL 5

NEXT LEVEL:
DEATH
```

If the player knowingly attempts an action that risks another Exhaustion level, RealmWeaver should provide a strong warning before proceeding.

---

## 8D.22 Exhaustion Events and Persistence

Changes in Exhaustion generate structured mechanical events.

Example:

```text
EXHAUSTION_GAINED

previous = 2
new = 3
source = FORCED_MARCH
```

Recovery:

```text
EXHAUSTION_REMOVED

previous = 3
new = 2
source = LONG_REST
```

Exhaustion is authoritative persistent state and survives save/reload independently of AI conversation history.

---

## 8D.23 V1 Exhaustion Ruleset

V1 uses the standard SRD 5.1 / 2014-style six-level Exhaustion system.

RealmWeaver does not initially provide multiple selectable Exhaustion rulesets.

Alternative systems may be considered after playtesting if the standard system proves unsuitable for RealmWeaver's solo campaign experience.

---

# 8E — Short Rests

## 8E.1 Short Rest Definition

A Short Rest is a period of downtime lasting at least:

```text
1 hour
```

during which the character performs no activity more strenuous than appropriate restful activities such as:

* eating;
* drinking;
* reading;
* talking;
* tending wounds;
* other compatible light activity.

A Short Rest is a mechanical activity rather than simply narrative downtime.

---

## 8E.2 Short Rests Advance Campaign Time

Short Rests advance the authoritative campaign clock.

Example:

```text
Rest Begins:
14:20

Successful Completion:
15:20
```

The AI may narrate the downtime, but RealmWeaver controls the actual time advancement.

Detailed World Clock architecture is deferred to later World & Campaign State design.

---

## 8E.3 Declaring a Short Rest

Players may request a Short Rest through:

* natural-language input;
* a dedicated Rest UI.

Example:

```text
[Rest]
   ↓
[Short Rest]
[Long Rest]
```

Both ultimately produce a structured Short Rest request.

---

## 8E.4 Natural-Language Rest Recognition

Natural-language requests such as:

> Let's stop here, eat something and rest for an hour.

may be interpreted as a Short Rest proposal.

Where appropriate, RealmWeaver should show lightweight confirmation:

```text
Take a Short Rest?
Duration: 1 hour

[Confirm]
[Cancel]
```

This prevents accidental mechanical recovery or campaign-time advancement.

---

## 8E.5 AI Narrative Downtime Is Not Automatically a Short Rest

AI narration such as:

> You spend an hour beside the fire discussing the ruins.

does not automatically trigger Short Rest mechanics.

A mechanical Short Rest must be explicitly declared or validated.

---

## 8E.6 Rest Eligibility and Safety

RealmWeaver distinguishes between:

```text
Can attempt rest?
```

and:

```text
Can safely complete rest?
```

A dangerous location such as a dungeon does not automatically prohibit a Short Rest.

The player may attempt to rest, but world circumstances may create risk.

---

## 8E.7 Short Rest Interruption

A Short Rest must successfully complete its qualifying period to provide completed-rest benefits.

Example:

```text
Rest begins:
14:20

Interrupted:
14:47

Elapsed:
27 minutes
```

The Short Rest has not completed.

Completed-rest recovery is therefore not granted.

---

## 8E.8 Interrupted Rest Still Advances Time

An interrupted Short Rest does not rewind campaign time.

Example:

```text
Start:
14:20

Interrupted:
14:47

Current Campaign Time:
14:47
```

The elapsed 27 minutes remain part of campaign history.

---

## 8E.9 Restarting an Interrupted Short Rest

A materially interrupted Short Rest must normally restart its qualifying uninterrupted one-hour period.

Example:

```text
27 minutes rest
↓
combat
↓
combat ends
↓
new Short Rest requires 1 hour
```

---

## 8E.10 Rest-Breaking Activities

Activities that may materially interrupt a Short Rest include:

* combat;
* significant travel;
* strenuous physical activity;
* incompatible spellcasting or actions where applicable;
* other explicitly strenuous activities.

Compatible light activities may include:

* conversation;
* eating;
* drinking;
* reading;
* tending wounds;
* light equipment maintenance.

RealmWeaver should classify the activity rather than rely solely on AI judgement.

---

## 8E.11 Spending Hit Dice

During successful Short Rest completion, a character may spend available Hit Dice to recover HP.

For each Hit Die:

```text
Hit Die Roll
+
Current Constitution Modifier
=
HP Recovered
```

Hit Dice are authoritative persistent resources.

---

## 8E.12 Player Controls Hit Dice Spending

RealmWeaver does not automatically spend all available Hit Dice.

The player chooses how many to spend.

Example:

```text
HP:
18 / 42

Available Hit Dice:
4d10

[Spend Hit Die]
[Finish Rest]
```

---

## 8E.13 Sequential Hit Dice Spending

Hit Dice may be spent one at a time.

Example:

```text
Spend d10
↓
Roll 8 + 3 CON
↓
Recover 11 HP
↓
Choose whether to spend another
```

This allows the player to manage resources without unnecessary waste.

---

## 8E.14 Hit Dice and Dice-Control Preference

Hit Dice follow the campaign's existing dice-control preference.

If player-controlled:

```text
Player rolls Hit Die
```

If system-controlled:

```text
RealmWeaver rolls Hit Die
```

---

## 8E.15 Healing Limits

Hit Die healing cannot increase normal HP beyond the character's effective maximum HP.

Any excess healing is lost.

The spent Hit Die remains consumed.

---

## 8E.16 Short-Rest Resource Recovery

Class features and other resources should declare their recharge trigger rather than being hardcoded into the Short Rest system.

Conceptually:

```text
recharge = SHORT_REST
```

or:

```text
recharge = SHORT_OR_LONG_REST
```

When:

```text
SHORT_REST_COMPLETED
```

occurs, RealmWeaver restores eligible resources.

---

## 8E.17 Recovery Requires Successful Completion

An interrupted Short Rest does not trigger completed-rest resource recovery.

RealmWeaver should treat recovery transactionally.

---

## 8E.18 Hit Dice Are Spent During Completion

Hit Dice spending should occur during the successful rest-completion phase rather than before the one-hour period is secured.

Flow:

```text
1 hour successfully completed
        ↓
Short Rest completion phase
        ↓
Player chooses Hit Dice
        ↓
HP updated
        ↓
Short-rest resources restored
        ↓
SHORT_REST_COMPLETED
```

This prevents spending Hit Dice during a rest that later fails.

---

## 8E.19 Short Rest Preview

Before resting, RealmWeaver may show:

```text
SHORT REST

Duration:
1 hour

Potential Benefits:
• Spend Hit Dice
• Recover eligible Short-Rest resources

Current Location:
Abandoned Crypt

Safety:
Uncertain

[Begin Rest]
[Cancel]
```

Only information the character can reasonably know should be exposed.

Exact hidden encounter probabilities remain hidden.

---

## 8E.20 Short Rest Completion UI

After successful completion:

```text
SHORT REST COMPLETE

Time Passed:
1 hour

HP:
18 / 42

Hit Dice:
4 / 4

[Spend Hit Die]

Resources Restored:
[...]

[Finish]
```

The interface should make mechanical consequences clear without permanently occupying campaign space.

---

## 8E.21 Light Activities During Rest

Characters may perform compatible light activities during a Short Rest.

Example:

> While resting, I examine the strange ring.

Conceptually:

```text
SHORT_REST
+
LIGHT_ACTIVITY(EXAMINE_ITEM)
```

RealmWeaver validates whether the activity remains compatible with resting.

---

## 8E.22 Incompatible Player Activity

If the player attempts an activity that would interrupt the rest, RealmWeaver may warn:

> Leaving now will interrupt your Short Rest. Continue?

The system should avoid silently destroying rest progress where a meaningful player confirmation is appropriate.

---

## 8E.23 World Events During Short Rests

The world does not necessarily freeze during a Short Rest.

Potential events may include:

* patrol arrival;
* weather changes;
* NPC arrival;
* enemy movement;
* scheduled world events;
* environmental-hazard progression.

These events must ultimately come from validated world/event logic rather than arbitrary AI mechanical decisions.

Detailed event simulation is deferred to later World & Campaign State design.

---

## 8E.24 AI Narration During Short Rest

A normal uninterrupted Short Rest should generally be represented as one narrative interval rather than requiring repeated AI calls.

The AI may use the period for:

* party conversation;
* companion interaction;
* reflection;
* environmental description;
* light roleplay.

This helps preserve responsive campaign pacing.

---

## 8E.25 NPC Short Rests

Mechanically relevant companions and NPCs may participate in Short Rests.

Each applicable creature resolves its own:

* Hit Dice;
* HP;
* Short-Rest resources;
* conditions/effects.

Background NPCs do not require detailed Short Rest simulation.

---

## 8E.26 AI-Controlled Companion Hit Dice

AI-controlled companions may propose their own Hit Dice spending.

Example:

```text
AI Proposal:
Companion spends 2 Hit Dice
```

RealmWeaver validates:

* available Hit Dice;
* applicable rolls;
* healing;
* resulting state.

The AI does not directly modify the companion's resources.

---

## 8E.27 Conditions and Exhaustion

A Short Rest does not automatically remove conditions.

Each condition/effect determines whether resting affects it.

A Short Rest also does not normally reduce Exhaustion.

```text
Exhaustion 3
→ Short Rest
→ Exhaustion 3
```

---

## 8E.28 Short Rest Frequency

V1 does not impose an arbitrary universal limit such as:

```text
Maximum Short Rests Per Day = 2
```

Characters may attempt Short Rests when circumstances allow.

Repeated resting is naturally constrained through:

* campaign-time progression;
* world events;
* quest deadlines;
* enemy reactions;
* environmental conditions;
* resource pressure.

---

## 8E.29 Repeated Resting and World Consequences

Repeated Short Rests consume real campaign time.

Example:

```text
Three Short Rests
→ approximately 3 campaign hours
```

During that time:

* guards may discover bodies;
* reinforcements may arrive;
* prisoners may be moved;
* NPCs may change location;
* villains may progress plans;
* daylight may change.

Detailed world progression rules are deferred to the World & Campaign State system.

---

## 8E.30 Short Rest Events

Successful completion generates a structured event.

Example:

```text
SHORT_REST_COMPLETED

start_time = 14:20
end_time = 15:20

hit_dice_spent = 2
hp_recovered = 17
resources_restored = [...]
```

An interruption may generate:

```text
SHORT_REST_INTERRUPTED

start_time = 14:20
interrupted_at = 14:47
reason = COMBAT_STARTED
```

---

## 8E.31 Save/Reload and Real-World Time

Rest progress belongs to campaign time.

Closing RealmWeaver does not cause campaign time to advance.

Example:

```text
Campaign Rest Progress:
15 minutes

Application closed for:
24 real-world hours

Campaign Rest Progress after reload:
15 minutes
```

Real-world elapsed time and campaign time remain separate.

---

# 8F — Long Rests

## 8F.1 Long Rest Definition

A Long Rest is an extended period of downtime lasting at least:

```text
8 hours
```

A normal Long Rest supports:

* at least 6 hours of sleep;
* up to 2 hours of compatible light activity.

Light activity may include:

* reading;
* talking;
* eating;
* keeping watch;
* other compatible activity.

---

## 8F.2 Long Rests Advance Campaign Time

Long Rests advance the authoritative campaign clock.

Example:

```text
Rest Begins:
22:00

Successful Completion:
06:00 next day
```

RealmWeaver processes applicable events and expirations occurring during that interval.

---

## 8F.3 Declaring a Long Rest

Players may request a Long Rest through:

* natural language;
* the Rest UI.

Because a Long Rest commits significant campaign time, RealmWeaver should normally provide lightweight confirmation.

Example:

```text
LONG REST

Duration:
8 hours

Current Time:
22:00

Expected Completion:
06:00

[Begin Rest]
[Cancel]
```

---

## 8F.4 Narrative Sleep Is Not Automatically a Long Rest

AI narration such as:

> You sleep peacefully beneath the stars.

does not automatically grant Long Rest benefits.

Likewise, simply advancing eight campaign hours does not automatically imply that a qualifying Long Rest occurred.

Long Rest completion is a validated mechanical event.

---

## 8F.5 HP Recovery

A successfully completed Long Rest restores all lost normal HP.

Example:

```text
Before:
17 / 46 HP

Long Rest completed

After:
46 / 46 HP
```

Recovery remains subject to applicable maximum-HP effects.

---

## 8F.6 Hit Dice Recovery

A successful Long Rest restores spent Hit Dice equal to half of the character's total Hit Dice, rounded down, with a minimum recovery of one Hit Die.

Example:

```text
Level 5 Fighter

Maximum Hit Dice:
5d10

Available Before Rest:
0d10

Recovered:
2d10

Available After Rest:
2d10
```

Hit Dice cannot be recovered beyond the character's maximum.

A Long Rest does **not** normally restore every spent Hit Die.

This preserves longer-term resource attrition across consecutive adventuring days.

---

## 8F.7 Spell Slot Recovery

Applicable Wizard and Cleric spell slots restore on successful Long Rest according to their rules.

Example:

```text
Before:
1st Level: 0/4
2nd Level: 1/3
3rd Level: 0/2

After:
1st Level: 4/4
2nd Level: 3/3
3rd Level: 2/2
```

---

## 8F.8 Class and Feature Recovery

Features/resources define their own recharge trigger.

Examples:

```text
recharge = LONG_REST
```

or:

```text
recharge = SHORT_OR_LONG_REST
```

Successful completion emits:

```text
LONG_REST_COMPLETED
```

and RealmWeaver restores eligible resources.

The rest engine does not hardcode recovery separately for every class.

---

## 8F.9 Exhaustion Recovery

A qualifying Long Rest normally removes:

```text
1 Exhaustion Level
```

Example:

```text
Before:
Exhaustion 3

After:
Exhaustion 2
```

A Long Rest does not normally reset Exhaustion to zero.

Applicable survival requirements such as food/water may affect qualification where required by the adopted rules.

---

## 8F.10 Conditions

A Long Rest does not automatically remove every active condition.

Each condition/effect defines its own duration or removal rule.

Applicable effects may explicitly define:

```text
ends_on = LONG_REST
```

---

## 8F.11 Prepared Spells

Completing a valid Long Rest provides the applicable opportunity for Wizard and Cleric prepared-spell changes.

Changing prepared spells remains optional.

Example:

```text
LONG REST COMPLETE

[Change Prepared Spells]
[Continue]
```

If the player does not modify preparation, the existing valid preparation remains.

V1 abstracts unnecessary minute-by-minute preparation bookkeeping.

---

## 8F.12 Long Rest Safety

Dangerous locations do not automatically prohibit Long Rest attempts.

RealmWeaver distinguishes:

```text
Can attempt Long Rest?
YES

Can safely complete Long Rest?
Context dependent
```

A player may therefore attempt to rest inside hostile territory.

The world may react.

---

## 8F.13 Rest Preparation

Players may improve rest safety through actions such as:

* barricading entrances;
* finding shelter;
* moving to a safer location;
* establishing watches;
* using defensive magic;
* concealing camp;
* managing fires/light;
* obtaining environmental protection.

These actions create validated world state rather than merely narrative flavour.

---

## 8F.14 AI Rest Recommendations

The AI may suggest preparations.

Example:

> The ruined chapel is defensible, but the shattered northern doorway leaves the camp exposed. Barricading it would make the position safer.

The AI cannot automatically make the preparation mechanically true.

The player must choose or authorize the action, and RealmWeaver validates the resulting state.

---

## 8F.15 Watches

V1 supports basic watch scheduling.

Characters may perform appropriate watch duty within the allowed light-activity portion of a Long Rest.

For parties with companions, watch schedules may distribute responsibility while preserving qualifying rest requirements.

---

## 8F.16 Solo Characters and Watches

A solo character is not required to have another creature keeping watch in order to attempt a Long Rest.

The player may instead:

* sleep without a watch;
* secure shelter;
* use defensive magic;
* use an Alarm-style effect;
* rely on other validated protections.

These choices may influence rest risk and detection.

---

## 8F.17 AI Companion Watches

AI-controlled companions may propose watch schedules.

Example:

```text
Mira → First Watch
Kael → Second Watch
```

RealmWeaver validates whether the schedule remains compatible with each participant's rest requirements.

---

## 8F.18 Long Rest Interruption Model

A Long Rest does **not** automatically generate an interruption.

Rest interruption is:

* possible;
* probabilistic;
* dependent on world context;
* affected by player preparation.

RealmWeaver must avoid making interruptions routine because repeated interruption would damage campaign pacing and become frustrating.

---

## 8F.19 No Universal Interruption Percentage

RealmWeaver does not use one universal rule such as:

```text
Every Long Rest:
25% encounter chance
```

Instead, the eventual risk model may consider factors such as:

```text
location danger
nearby enemies
active pursuit
recent player actions
environmental hazards
shelter
barricades
watch schedule
defensive magic
other protections
```

The exact probability model is deferred to the World & Campaign State / architecture design.

---

## 8F.20 Player-Facing Rest Risk

Exact hidden interruption probabilities should not normally be shown.

Instead, the UI may communicate qualitative information such as:

```text
Rest Safety:
Safe
Low Risk
Uncertain
Dangerous
Extreme Danger
```

This information should be based on what the character can reasonably know.

The system's actual hidden risk may differ.

---

## 8F.21 Watches and Interruption

A watch does not make interruption impossible.

Instead, watches may improve:

* detection;
* warning time;
* surprise prevention;
* response opportunities.

Example:

An approaching enemy may still reach the camp, but a successful watch may allow the party to detect the threat before being surprised.

---

## 8F.22 Interruption Events Are Not Always Combat

Possible Long Rest events may include:

* hostile encounters;
* NPC arrival;
* environmental events;
* severe weather;
* suspicious noises;
* creatures passing nearby;
* quest/world events;
* companion events.

Not every event necessarily invalidates the Long Rest.

---

## 8F.23 2014-Style Rest Interruption Threshold

RealmWeaver follows the adopted 2014-style Long Rest interruption model rather than treating any combat as automatic rest failure.

Brief interruption may occur without invalidating the entire Long Rest.

RealmWeaver tracks applicable strenuous interruption time.

Conceptually:

```text
Combat = short duration
Search after combat = 12 minutes
Relocation = 25 minutes

Accumulated strenuous activity:
tracked by RealmWeaver
```

If the applicable rest-breaking threshold is reached, the Long Rest fails.

---

## 8F.24 Light Activity During Long Rest

Compatible light activity does not automatically invalidate a Long Rest.

Examples may include:

* conversation;
* eating;
* reading;
* reasonable watch duty;
* light equipment maintenance.

RealmWeaver classifies activities according to applicable rest rules.

---

## 8F.25 Failed Long Rest and Campaign Time

A failed Long Rest does not rewind campaign time.

Example:

```text
Rest begins:
22:00

Rest ultimately fails:
04:30

Current Campaign Time:
04:30
```

The elapsed six and a half hours remain part of campaign history.

---

## 8F.26 Restarting a Failed Long Rest

Once a Long Rest is mechanically invalidated, the party must begin a new qualifying Long Rest period.

Previously accumulated rest time does not automatically carry into the new rest.

---

## 8F.27 Combat During Long Rest

Combat during a Long Rest does not automatically determine whether the rest succeeds or fails.

RealmWeaver should:

1. pause/interact with the active rest;
2. resolve combat normally;
3. advance campaign time;
4. record applicable strenuous activity;
5. determine whether the Long Rest remains valid;
6. resume or fail the rest accordingly.

---

## 8F.28 Recovery Occurs Only at Completion

Long Rest benefits are not granted when the rest begins.

RealmWeaver does not restore:

* HP;
* spell slots;
* Hit Dice;
* Long-Rest resources;
* Exhaustion;

until the Long Rest successfully completes.

This prevents interrupted-rest exploits and inconsistent state.

---

## 8F.29 Atomic Long Rest Recovery

Successful Long Rest recovery should be processed as a coherent mechanical transaction.

Conceptually:

```text
LONG_REST_COMPLETED
        ↓
HP restored
        ↓
Hit Dice recovered
        ↓
spell slots restored
        ↓
eligible resources restored
        ↓
Exhaustion reduced if eligible
        ↓
applicable conditions/effects processed
        ↓
state committed
```

The exact transaction architecture is deferred to implementation design.

---

## 8F.30 One Long Rest Benefit Per 24 Hours

RealmWeaver follows the standard restriction that a character normally cannot benefit from more than one Long Rest within a 24-hour period.

RealmWeaver tracks the last qualifying successful Long Rest.

Example:

```text
Last Long Rest:
Day 12 — 06:00

Attempt:
Day 12 — 13:00
```

The character may still sleep or pass time, but another Long Rest's mechanical recovery is not yet available.

---

## 8F.31 Sleeping Is Not Always a Mechanical Long Rest

Characters may sleep or spend extended downtime without receiving Long Rest benefits.

Example:

> I sleep for six hours.

RealmWeaver may advance campaign time and narrate the sleep without producing:

```text
LONG_REST_COMPLETED
```

This allows narrative freedom without bypassing recovery rules.

---

## 8F.32 Excessive Long Resting

RealmWeaver should not rely primarily on arbitrary videogame-style anti-rest restrictions.

Long Rests naturally cost:

```text
8 campaign hours
```

plus any applicable:

* travel;
* preparation;
* environmental exposure;
* world events.

During this time:

* enemies may react;
* defenses may improve;
* prisoners may move;
* quests may progress;
* villains may advance plans;
* NPC schedules may change;
* weather may change.

This creates natural consequences for excessive resting.

---

## 8F.33 Environmental Hazards and Long Rest

Environmental hazards may affect the ability to safely or successfully rest.

Example:

```text
Environment:
EXTREME_COLD

Protection:
Cold-weather clothing ✓
Shelter ✓
```

may mitigate the hazard.

Without adequate protection, RealmWeaver applies the appropriate validated environmental rules.

---

## 8F.34 Limited V1 Survival Layer

V1 may support practical survival factors such as:

* food/water where mechanically relevant;
* basic camp equipment;
* shelter;
* climate protection;
* environmental hazards.

RealmWeaver should not initially simulate unnecessary detail such as:

```text
Tent Fabric Condition = 73%
Blanket Warmth = +2
Sleep Quality = 81%
```

The objective is meaningful preparation without turning the campaign into a survival spreadsheet.

---

## 8F.35 Sleeping in Armor

RealmWeaver V1 does not introduce a general penalty for sleeping in armor beyond the adopted SRD 5.1 / 2014 baseline.

Optional rules from other sources should not be silently imported into V1.

---

## 8F.36 Dawn, Daily and Long-Rest Recharge Are Different

RealmWeaver must distinguish between effects that recharge:

```text
AT_DAWN
```

```text
ON_LONG_REST
```

```text
DAILY
```

These are not automatically equivalent.

For example, an item that recharges at dawn may recharge when dawn occurs regardless of whether the player completed a Long Rest.

---

## 8F.37 NPC Long Rests

Mechanically relevant companions and persistent NPCs may use Long Rest mechanics where appropriate.

RealmWeaver does not simulate detailed nightly rest mechanics for every background NPC.

---

## 8F.38 Persistent Enemy Recovery

Important recurring enemies should not arbitrarily return at full HP/resources because the AI finds it narratively convenient.

If an injured enemy escapes, future recovery should depend on applicable:

* campaign time;
* rest;
* healing;
* world events;
* NPC state.

Detailed off-screen NPC simulation is deferred to later World & Campaign State design.

---

## 8F.39 Long Rest Preview UI

Before resting:

```text
LONG REST

Duration:
8 hours

Current Time:
22:00

Current State:
HP: 18/42
Exhaustion: 1
Hit Dice: 2/5

Expected Recovery:
• HP → Full
• Spell Slots → Restore
• Long-Rest abilities → Restore
• Hit Dice → +2
• Exhaustion → -1 if eligible

Location:
Ruined Chapel

Rest Safety:
Uncertain

[Begin Long Rest]
[Cancel]
```

The interface should expose known consequences without revealing inappropriate hidden information.

---

## 8F.40 Long Rest Completion UI

After successful completion:

```text
LONG REST COMPLETE

Time:
06:00

HP:
42/42

Hit Dice:
4/5

Spell Slots:
Restored

Exhaustion:
1 → 0

[Change Prepared Spells]
[Continue]
```

Note that `1 → 0` occurs here only because the character entered the rest at Exhaustion Level 1.

A character at Exhaustion Level 3 would normally become Level 2, not Level 0.

---

## 8F.41 AI Narration

Normal uninterrupted Long Rests should generally use one main narrative interval rather than repeated hourly LLM calls.

If nothing significant occurs, the AI may narrate the transition to morning.

If a validated event occurs, normal player interaction resumes around that event.

---

## 8F.42 Save/Reload and Real-World Time

In-progress Long Rest state is part of campaign state.

Closing RealmWeaver does not cause real-world elapsed time to advance the rest.

Example:

```text
Long Rest Progress:
3h 20m

Application closed

Application reopened next day

Long Rest Progress:
3h 20m
```

Campaign time remains authoritative.

---

## 8F.43 Long Rest Mechanical Events

Successful completion generates an event such as:

```text
LONG_REST_COMPLETED

start:
Day 12 22:00

end:
Day 13 06:00

hp:
18 → 42

hit_dice:
2 → 4

exhaustion:
1 → 0

resources_restored:
[...]
```

A failed rest may generate:

```text
LONG_REST_FAILED

start:
22:00

failure_time:
04:30

reason:
STRENUOUS_INTERRUPTION_THRESHOLD
```

These events support persistence, AI narration, debugging, and campaign history.

---

## 8F.44 Long Rest Authority Flow

The intended Long Rest flow is:

```text
PLAYER
requests Long Rest
      ↓
REALMWEAVER
validates eligibility
      ↓
REST BEGINS
      ↓
CAMPAIGN TIME ADVANCES
      ↓
WORLD / REST RISK SYSTEM
evaluates possible events
      ↓
possible validated interruption
      ↓
REST STILL VALID?
   ↙             ↘
 NO              YES
 ↓                ↓
FAIL         8h completed
                  ↓
          RECOVERY TRANSACTION
                  ↓
             STATE COMMIT
                  ↓
                 AI
          narrates outcome
```
---

Absolutely. Below is the remaining approved content for **8G — Hit Dice & Recovery Rules, 8H — Rest Restrictions, Interruptions & Campaign Time, and 8I — AI, NPCs, Persistence & Validation**.

````markdown
# 8G — Hit Dice & Recovery Rules

## 8G.1 Class Hit Dice

RealmWeaver V1 uses the standard class Hit Dice for the supported classes:

| Class | Hit Die |
|---|---|
| Fighter | d10 |
| Rogue | d8 |
| Wizard | d6 |
| Cleric | d8 |

For a single-class character, the maximum number of Hit Dice normally equals the character's class level.

Example:

```text
Level 5 Fighter

Maximum Hit Dice:
5d10
````

---

## 8G.2 Hit Dice Are Persistent Resources

RealmWeaver tracks both the maximum and currently available Hit Dice.

Conceptually:

```text
HitDicePool

die_type = d10
maximum = 5
available = 3
```

Available Hit Dice are authoritative persistent state.

Saving and reloading a campaign does not automatically restore them.

---

## 8G.3 Spending Hit Dice

Characters normally spend Hit Dice during successful Short Rest completion.

For each Hit Die spent:

```text
Hit Die Roll
+
Current Constitution Modifier
=
HP Recovered
```

Example:

```text
Rogue Hit Die:
d8

CON Modifier:
+2

Roll:
5

Healing:
7 HP
```

---

## 8G.4 Constitution Modifier

Hit Die healing uses the character's current applicable Constitution modifier when the Hit Die is spent.

RealmWeaver should not permanently store a Constitution bonus inside each Hit Die.

If Constitution changes during the campaign, future Hit Die healing uses the updated modifier.

---

## 8G.5 Minimum Healing

Hit Die spending must never reduce a character's HP.

If a negative Constitution modifier would otherwise produce a negative healing result, RealmWeaver safeguards the resolution so that Hit Die spending does not damage the character.

The exact minimum treatment should follow the adopted baseline where explicitly defined.

---

## 8G.6 Sequential Hit Dice Spending

Players spend Hit Dice sequentially.

Example:

```text
HP:
14 / 40

Hit Dice:
4d10 available

Spend d10
↓
Recover 9 HP

HP:
23 / 40

[Spend Another]
[Finish]
```

This allows the player to evaluate recovery after each die before committing another resource.

---

## 8G.7 Hit Dice at Full HP

RealmWeaver should prevent pointless Hit Die spending when the character is already at full effective HP.

Example:

```text
HP:
40 / 40

Hit Dice:
3 / 5
```

The Spend Hit Die action should be unavailable unless another valid rule creates a reason for it.

---

## 8G.8 Overhealing

Hit Die healing cannot exceed the character's current effective maximum HP.

Example:

```text
HP:
37 / 40

Hit Die Healing:
9

Actual Healing Applied:
3

Final HP:
40 / 40
```

The Hit Die remains spent.

---

## 8G.9 Long Rest Hit Dice Recovery

A successful qualifying Long Rest restores spent Hit Dice equal to half the character's total Hit Dice, rounded down, with a minimum recovery of one.

Examples:

```text
Level 1:
recover 1
```

```text
Level 3:
recover 1
```

```text
Level 5:
recover 2
```

Recovered Hit Dice cannot exceed the character's maximum.

A Long Rest does not normally restore the entire spent Hit Dice pool.

---

## 8G.10 Long Rest and HP

Characters do not normally spend Hit Dice during a Long Rest to recover ordinary HP.

Successful Long Rest completion restores HP according to the Long Rest rules and separately restores a portion of spent Hit Dice.

---

## 8G.11 Hit Dice at Level Up

When a committed level-up grants another class level, the character gains one additional Hit Die of the appropriate class type.

Example:

```text
Fighter Level 4

Maximum:
4d10

Available:
2d10

Level Up → Fighter Level 5

Maximum:
5d10

Available:
3d10
```

The newly gained Hit Die becomes available immediately.

Previously spent Hit Dice are not automatically restored.

---

## 8G.12 Postponed Level Up

If a player has earned a level-up but chooses to postpone completing it, Hit Dice do not change until the level-up is actually committed.

```text
Level Up Available
but postponed

Hit Dice:
unchanged
```

Once the player completes the level-up:

```text
Maximum Hit Dice +1
Available Hit Dice +1
```

---

## 8G.13 Multiclass-Ready Architecture

Multiclassing is not required for initial V1 scope.

However, Hit Dice architecture should support multiple Hit Dice pools.

Future example:

```text
Fighter 3 / Wizard 2

d10:
3 maximum

d6:
2 maximum
```

The player may then choose which available Hit Die type to spend during recovery.

---

## 8G.14 NPC Hit Dice

Recoverable Hit Dice should be tracked where mechanically useful for:

* player characters;
* companions;
* important persistent NPCs;
* mechanically materialised NPCs whose recovery matters.

Disposable encounter enemies do not require detailed recoverable-Hit-Dice bookkeeping unless a specific mechanic requires it.

---

## 8G.15 HP-Generation Hit Dice vs Recovery Hit Dice

Creature stat blocks may use Hit Dice to describe or generate maximum HP.

This does not necessarily mean every creature needs a player-style recoverable Hit Dice resource.

RealmWeaver should conceptually distinguish:

```text
HP-generation Hit Dice
```

from:

```text
recoverable Hit Dice resource
```

even where similar tabletop terminology is used.

---

## 8G.16 Effective Maximum HP

Hit Die healing respects the character's current effective maximum HP.

Example:

```text
Base Max HP:
60

Exhaustion Level 4

Effective Max HP:
30
```

Hit Die recovery cannot increase HP above 30 while that reduction remains active.

If the maximum later returns to 60, current HP does not automatically increase.

---

## 8G.17 Temporary Hit Points

Hit Dice restore normal HP.

They do not restore or increase Temporary HP unless an explicit feature says otherwise.

Example:

```text
HP:
20 / 40

Temporary HP:
5

Hit Die Healing:
8

Result:
HP = 28 / 40
Temporary HP = 5
```

---

## 8G.18 Modifiers to Hit Dice Healing

RealmWeaver architecture should support future features that modify Hit Die recovery.

Possible effects may include:

* additional healing;
* rerolls;
* minimum roll values;
* bonus dice;
* other validated feature effects.

These should use RealmWeaver's shared effect/modifier infrastructure rather than requiring special Short Rest code.

---

## 8G.19 AI and Hit Dice

The AI may explain or recommend Hit Dice use.

Example:

> You're badly injured and still have three Hit Dice available. You could spend some during this Short Rest.

The AI must not automatically spend player Hit Dice unless a future explicit player preference permits it.

---

## 8G.20 Automated Hit Dice Spending

Automated Hit Dice spending preferences are deferred from initial V1.

A future version may support options such as:

```text
Short Rest Healing

Ask Me
Heal to ~50%
Heal to ~75%
Heal as Much as Possible
```

Initial V1 should preserve direct sequential player control.

---

## 8G.21 Hit Dice UI

Character information may display:

```text
HIT DICE

3 / 5 d10
```

During a Short Rest:

```text
HP:
19 / 47

Hit Dice:
3 / 5 d10

[Spend d10]

Last Roll:
7 + 3 CON = 10 HP

HP:
29 / 47

[Spend Another]
[Finish]
```

The exact presentation is deferred to UI/UX design.

---

## 8G.22 Hit Dice Mechanical Events

Hit Dice spending should generate structured events.

Example:

```text
HIT_DIE_SPENT

character = Fighter
die = d10
roll = 7
constitution_modifier = +3
healing = 10

available:
3 → 2

HP:
19 → 29
```

Recovery:

```text
HIT_DICE_RECOVERED

source = LONG_REST
amount = 2

available:
2 → 4
```

---

## 8G.23 AI Failure Does Not Reroll Hit Dice

Once a Hit Die roll and healing result have been committed, subsequent AI failure does not cause the roll to occur again.

Example:

```text
Hit Die rolled
↓
Healing committed
↓
AI narration fails
```

RealmWeaver retains the original mechanical result.

Only narration may be retried.

---

## 8G.24 Hit Dice Do Not Require LLM Processing

The complete Hit Dice resolution:

```text
roll die
+
CON modifier
↓
calculate healing
↓
cap at effective max HP
↓
consume Hit Die
↓
update HP
```

is handled through deterministic rules and dice systems.

No LLM call is required.

---

# 8H — Rest Restrictions, Interruptions & Campaign Time

## 8H.1 Rest as a Persistent Mechanical Activity

Short and Long Rests should exist as authoritative mechanical activities rather than instantaneous recovery commands.

Conceptually, an active rest may contain:

```text
RestActivity

type
start_time
required_duration
participants
status
interruptions
light_activities
risk_context
```

Possible states may include:

```text
PROPOSED
ACTIVE
PAUSED
COMPLETED
FAILED
CANCELLED
```

The final schema is deferred to architecture.

---

## 8H.2 Rest Eligibility and Rest Safety

RealmWeaver distinguishes between:

```text
Can the character attempt to rest?
```

and:

```text
How safe is attempting to rest here?
```

A dangerous area does not normally make resting mechanically impossible.

Example:

```text
Ancient Crypt

Rest Attempt:
Allowed

Safety:
Dangerous
```

---

## 8H.3 Hard Rest Restrictions

Certain circumstances may prevent a rest from beginning.

Examples include:

* active combat;
* continuous strenuous activity;
* explicit effects preventing rest;
* another mechanically incompatible state.

RealmWeaver validates hard restrictions deterministically.

---

## 8H.4 Soft Rest Risks

Other circumstances create risk rather than prohibition.

Examples:

* hostile territory;
* exposed wilderness;
* nearby enemies;
* active pursuit;
* dangerous weather;
* inadequate shelter;
* unsecured dungeon areas.

These factors contribute to the rest-risk system.

---

## 8H.5 Probabilistic Rest Interruption

Rest interruption is possible but not automatic.

RealmWeaver evaluates context and determines whether a meaningful rest event occurs.

Conceptually:

```text
REST
↓
evaluate world/location state
↓
determine interruption risk
↓
RNG / event evaluation
↓
event or no event
```

Many rests should complete without incident.

---

## 8H.6 No Universal Rest-Interruption Percentage

RealmWeaver should not use one universal fixed chance for all rests.

The eventual model may consider:

* location threat;
* nearby creatures;
* active pursuit;
* recent player actions;
* environmental conditions;
* length of exposure;
* shelter;
* barricades;
* watches;
* defensive magic;
* concealment;
* other validated protections.

The exact probability formula is deferred to World & Campaign State / architecture design.

---

## 8H.7 Player Preparation Modifies Risk

Preparations should create actual mechanical/world-state changes.

Examples:

```text
Door = BARRICADED
```

```text
Alarm Effect = ACTIVE
```

```text
Camp = CONCEALED
```

```text
Watch = ASSIGNED
```

These may influence interruption risk, detection, or response.

---

## 8H.8 Player-Facing Risk Categories

Exact internal interruption probabilities should normally remain hidden.

RealmWeaver may instead communicate qualitative assessments such as:

```text
SAFE
LOW RISK
UNCERTAIN
DANGEROUS
EXTREME DANGER
```

These assessments reflect what the character can reasonably determine.

---

## 8H.9 System Risk vs Character Knowledge

The system's actual risk may differ from what the character knows.

Example:

```text
Character Assessment:
Appears Safe

Hidden World State:
Assassin tracking player
```

RealmWeaver may therefore display:

```text
Rest Safety:
Appears Safe
```

without revealing hidden threats.

---

## 8H.10 Investigating Rest Safety

Players may attempt to gather more information before resting.

Examples:

> I search the campsite.

> I check for tracks.

> I inspect the room for another entrance.

Relevant checks such as Perception, Investigation, or Survival may reveal additional information.

Successful investigation may improve the player's **knowledge of the risk** without necessarily reducing the underlying risk.

---

## 8H.11 Preparation vs Information

Some actions reveal risk.

Example:

```text
Search for tracks
→ better knowledge
```

Other actions actually change risk.

Example:

```text
Barricade door
→ world state changed
→ certain threats become harder to reach camp
```

RealmWeaver should distinguish these outcomes.

---

## 8H.12 Watches

Keeping watch does not make rest interruption impossible.

A watch may instead improve:

* detection;
* warning time;
* resistance to surprise;
* response opportunity.

The threat may still occur.

---

## 8H.13 Rest Events Are Not Always Combat

Possible rest events may include:

* hostile patrols;
* travellers;
* NPC arrivals;
* environmental changes;
* weather;
* suspicious noises;
* nearby creatures;
* scheduled quest/world events;
* companion interactions.

Some events may interrupt the rest.

Others may be observed without invalidating it.

---

## 8H.14 AI and Rest Events

The AI does not independently decide:

> This rest has been too peaceful, so enemies attack.

RealmWeaver's validated world/rest system determines whether an event occurs.

Once an event is selected, the AI may narrate it creatively.

---

## 8H.15 AI-Proposed Events

Future systems may allow AI to propose contextually appropriate rest events.

Example:

```text
EVENT REQUIRED

Location:
Haunted Forest

Constraints:
No known humanoid patrols nearby
Storm approaching
```

AI proposals might include:

* distant spectral lights;
* wounded animal;
* worsening storm;
* supernatural whispering.

RealmWeaver must validate the event before it receives mechanical authority.

---

## 8H.16 Short Rest Exposure

Short Rest interruption probability should account for the shorter one-hour duration.

A one-hour rest should not necessarily have the same cumulative exposure as an eight-hour Long Rest in the same location.

The exact model is deferred.

---

## 8H.17 Long Rest Event Evaluation Windows

Long Rests may internally evaluate risk over multiple portions of the eight-hour period.

Conceptually:

```text
22:00–00:00
00:00–02:00
02:00–04:00
04:00–06:00
```

These evaluations should remain internal.

RealmWeaver should not expose repetitive rolls or make repeated unnecessary AI calls.

Exact interval design is deferred to world simulation.

---

## 8H.18 Preventing Encounter Farming

Repeated resting should not create unlimited enemies or loot.

Rest events must respect persistent world state.

Example:

```text
Goblin Outpost Population:
8
```

A rest encounter defeats three goblins:

```text
Remaining:
5
```

RealmWeaver should not repeatedly generate infinite replacement patrols unless the world actually has reinforcements or another valid source.

---

## 8H.19 Persistent World State

Rest events should interact with persistent:

* NPC populations;
* enemy locations;
* patrols;
* settlements;
* quest state;
* environmental state;
* other relevant world data.

The detailed world-simulation system is deferred to later design.

---

## 8H.20 Authoritative Campaign Clock

RealmWeaver maintains an authoritative in-world campaign clock.

Conceptually:

```text
Day 17
14:35
```

The clock is part of persistent campaign state.

It is not based on real-world elapsed time.

---

## 8H.21 Time Advancement

Game activities advance campaign time.

Examples include:

```text
Short Rest → approximately +1 hour
Long Rest → approximately +8 hours
Travel → calculated duration
Combat → round-based duration
Spellcasting → applicable casting time
Extended activity → applicable duration
```

The authoritative time change is committed by RealmWeaver.

---

## 8H.22 AI Does Not Own Time

The AI may propose narrative duration where no exact mechanical duration exists.

Example:

```text
TIME_ADVANCE_PROPOSAL

reason = NPC_CONVERSATION
duration = 12 minutes
```

RealmWeaver determines whether and how that time advancement is committed.

Mechanically defined durations remain fully deterministic.

---

## 8H.23 Different Time Granularity

Different systems may operate at different levels of time precision.

Examples:

```text
Combat:
rounds / seconds
```

```text
Exploration:
minutes
```

```text
Travel:
minutes / hours
```

```text
Downtime:
hours / days
```

All systems ultimately reference the same authoritative campaign timeline.

---

## 8H.24 Narrative Conversation Time

Real-world conversation length does not determine campaign time.

Five player/AI messages might represent:

* thirty seconds;
* ten minutes;
* an hour.

AI may propose a reasonable narrative duration where required.

RealmWeaver validates and commits the resulting advancement.

Detailed time-estimation rules are deferred to later design.

---

## 8H.25 Time Advancement Must Process Intermediate Events

RealmWeaver must not simply change the clock from one value to another while ignoring events in between.

Example:

```text
Current:
14:00

Advance To:
16:00
```

Relevant events may include:

```text
14:30 — condition expires
15:00 — merchant closes
15:20 — NPC leaves
15:45 — spell expires
16:00 — quest deadline
```

RealmWeaver processes due events in the appropriate chronological order.

---

## 8H.26 Future World Event Scheduler

Future architecture requires some mechanism for scheduled events.

Conceptually:

```text
WORLD EVENT QUEUE

14:30 → effect expiration
15:00 → NPC schedule event
16:00 → quest event
SUNSET → environment transition
```

This is an architectural requirement, not a finalized implementation design.

---

## 8H.27 Combat and Campaign Time

Combat should eventually advance the same authoritative campaign timeline.

Example:

```text
Combat Begins:
14:32:00

5 Rounds:
approximately 30 seconds

Combat Ends:
14:32:30
```

The UI does not need to display second-level precision constantly.

---

## 8H.28 Day and Night

Campaign time should eventually support derived periods such as:

* dawn;
* day;
* dusk;
* night.

These may affect:

* visibility;
* NPC schedules;
* shop availability;
* travel;
* environmental effects;
* encounters;
* abilities and magic items.

Detailed implementation is deferred.

---

## 8H.29 Calendar Design

The world calendar is intentionally not finalized during Group 8.

Different campaign/world-generation profiles may eventually use different calendars.

Examples may include:

* familiar historical calendars;
* generated fantasy calendars;
* era-specific calendars;
* culturally inspired calendars.

Calendar design belongs to the future World & Campaign State / world-generation sections.

---

## 8H.30 World Profiles and Time Systems

Future historical, cultural, mythological, and era-based world-generation profiles may influence:

* calendars;
* seasons;
* daylight;
* festivals;
* NPC schedules;
* climate;
* environmental hazards;
* travel patterns.

AI/world generation may configure the setting.

RealmWeaver remains mechanically authoritative over time progression.

---

## 8H.31 Real-World Offline Time

Closing RealmWeaver does not advance campaign time.

Example:

```text
Campaign closed:
Friday

Campaign reopened:
Monday
```

The in-world clock remains where gameplay left it unless an explicit campaign mechanic advances it.

RealmWeaver is not an always-running MMO simulation.

---

## 8H.32 Rest Probability Logging

Probabilistic rest-event checks should produce enough internal mechanical data for debugging and balancing.

Conceptually:

```text
REST_EVENT_CHECK

context = [...]
result = NO_EVENT
```

The player does not need to see this internal information.

---

## 8H.33 Testable Rest Probability

Rest interruption probability must be implemented through deterministic code/RNG logic that can be tested independently from the LLM.

Developers should eventually be able to simulate large numbers of identical rest scenarios and inspect whether event frequency matches intended balancing.

This helps diagnose problems such as:

> Players are being interrupted almost every night.

---

## 8H.34 Difficulty and Rest Risk

Campaign difficulty may eventually modify rest-risk pressure.

Potentially:

```text
Easy
→ slightly reduced pressure

Normal
→ baseline

Hard
→ increased threat pressure
```

Difficulty modifiers remain subordinate to actual world context.

A secure inn should not routinely become dangerous merely because the campaign difficulty is higher.

Exact implementation is deferred to difficulty/world design.

---

## 8H.35 Player Experience Principle

The complexity of the time/risk simulation should remain mostly invisible.

Typical player experience:

```text
LONG REST

Location:
Forest Clearing

Safety:
Uncertain

Preparations:
✓ Camp established
✓ Companion keeping watch
✗ No proper shelter

[Begin Rest]
```

The player should experience a responsive RPG rather than feel like they are operating a simulation dashboard.

---

# 8I — AI, NPCs, Persistence & Validation

## 8I.1 Responsibility Layers

RealmWeaver separates natural-language interpretation, deterministic mechanics, and narrative presentation.

Conceptually:

```text
PLAYER
↓
AI DM / INTENT INTERPRETATION
↓
REALMWEAVER RULES + STATE
```

Resolved information flows back:

```text
REALMWEAVER
resolves mechanics
↓
AI DM
narrates result
↓
PLAYER
```

The AI is not mechanically authoritative over RealmWeaver's rules engine.

---

## 8I.2 AI Mechanical Proposals

The AI may interpret natural-language actions into structured mechanical proposals.

Example:

Player:

> I kick the bandit's legs out from under him.

AI interpretation:

```text
ACTION_PROPOSAL

type = SHOVE_PRONE
actor = Player
target = Bandit
```

RealmWeaver then determines:

* legality;
* required check;
* modifiers;
* immunities;
* dice;
* result.

If successful:

```text
CONDITION_APPLIED
PRONE
```

The AI then narrates the validated result.

---

## 8I.3 AI Cannot Directly Mutate State

The AI cannot directly modify authoritative values such as:

* HP;
* conditions;
* Exhaustion;
* Hit Dice;
* resources;
* inventory;
* position;
* rest state;
* campaign time;
* NPC state.

Mechanical changes occur through validated RealmWeaver actions/effects.

---

## 8I.4 Narrative Language Is Not Mechanical State

Narration alone does not create mechanics.

Examples:

> The revelation leaves Elara stunned.

does not automatically mean:

```text
STUNNED
```

> You feel exhausted after the journey.

does not automatically mean:

```text
EXHAUSTION +1
```

> You rest against the wall for an hour.

does not automatically mean:

```text
SHORT_REST_COMPLETED
```

A structured validated event is required.

---

## 8I.5 Structured Mechanical Results

RealmWeaver returns structured results to the AI for narration.

Example:

```text
ACTION_RESULT

actor = Player
action = SHOVE
target = Bandit

result = SUCCESS

effects:
- PRONE applied
```

The AI narrates this outcome but cannot contradict it.

---

## 8I.6 Relevant AI Context

AI context should contain relevant state rather than the entire campaign database.

Example:

```text
PLAYER
HP 27/38
Condition: Poisoned

BANDIT CAPTAIN
HP 16/31
Condition: Prone

BANDIT 2
HP 8/11

ENVIRONMENT
Burning table
Door barricaded
```

This improves:

* latency;
* token efficiency;
* focus;
* consistency;
* hallucination resistance.

---

## 8I.7 Mechanically Relevant NPC State

Once an NPC becomes mechanically relevant, RealmWeaver maintains authoritative mechanical state.

Examples may include:

```text
AC
HP
ability scores
attacks
spells
spell slots
resources
conditions
Exhaustion
inventory
position
```

The AI must not rely on prose memory for these values.

---

## 8I.8 NPC Materialisation

Not every NPC requires a full mechanical profile immediately.

A lightweight NPC may initially contain:

```text
name
role
location
relationship
basic narrative traits
```

If that NPC becomes mechanically relevant:

```text
Narrative NPC
↓
mechanical relevance detected
↓
materialisation required
↓
AI/system proposes appropriate archetype
↓
RealmWeaver validates
↓
persistent mechanical profile created
```

Detailed NPC materialisation rules are deferred to the dedicated World/NPC design.

---

## 8I.9 AI-Proposed NPC Mechanics

For dynamically generated NPCs, AI may propose an appropriate archetype or mechanical profile.

Example:

```text
Narrative Role:
Veteran City Guard

Proposed Archetype:
VETERAN_GUARD
```

RealmWeaver validates the proposal against:

* available rules content;
* expected power range;
* campaign difficulty;
* region/context;
* valid equipment;
* valid spells/features.

Accepted mechanics then become authoritative persistent state.

---

## 8I.10 Important NPC Persistence

Once an NPC becomes persistent or important, RealmWeaver should retain relevant state.

Potential persistent information may include:

* identity;
* location;
* relationships;
* HP;
* conditions;
* Exhaustion;
* inventory;
* abilities;
* spells;
* resources;
* goals;
* knowledge;
* important history.

The exact schema is deferred.

---

## 8I.11 NPC Conditions Persist Off-Screen

Leaving the scene does not automatically remove NPC conditions.

Example:

```text
Captain Varek

POISONED
duration = 1 hour
```

If the player leaves for 20 campaign minutes and returns:

```text
remaining duration:
approximately 40 minutes
```

unless another validated event changed the state.

---

## 8I.12 Off-Screen NPC Recovery

Important NPC recovery should eventually use authoritative world/time logic.

An injured enemy does not simply return fully healed because the AI wants another encounter.

Possible off-screen state changes may depend on:

* reaching safety;
* healing;
* resting;
* travel;
* environmental conditions;
* other NPCs;
* world events.

Detailed off-screen simulation is deferred to World & Campaign State design.

---

## 8I.13 Simulation Levels

RealmWeaver should not fully simulate every NPC in the world continuously.

Future architecture should support different simulation/detail levels.

Conceptually:

```text
ACTIVE
MATERIALISED
ABSTRACTED
BACKGROUND
```

Nearby companions may require detailed rules processing.

Distant/background NPCs may use abstract state progression.

The exact system is deferred.

---

## 8I.14 NPC Resting

Mechanically relevant companions and NPCs use Short/Long Rest rules where appropriate.

Background NPCs do not require detailed nightly rest simulation.

---

## 8I.15 Hidden NPC Conditions

NPCs may possess conditions or effects that are not known to the player.

Example:

```text
NPC:
CHARMED

Visibility:
HIDDEN_FROM_PLAYER
```

RealmWeaver retains the authoritative condition while respecting information boundaries.

---

## 8I.16 Hidden Information and AI

AI context must distinguish hidden system information from information that may be revealed to the player.

The AI must not accidentally narrate:

> The magically charmed guard approaches you...

if the player's character has not discovered that fact.

---

## 8I.17 System Truth, Character Knowledge and Player Information

RealmWeaver should conceptually distinguish:

### System Truth

```text
Guard is Charmed by Mage.
```

### Character Knowledge

```text
Guard has been behaving strangely.
```

### Player-Facing Narrative

> The guard seems unusually protective of the stranger.

These layers should not automatically collapse into one another.

This principle applies beyond conditions to:

* NPC motives;
* traps;
* secrets;
* hidden quests;
* locations;
* magic items;
* other campaign information.

---

## 8I.18 Rest Validation

AI may suggest:

> This looks like a good place to rest.

RealmWeaver determines:

```text
REST_ELIGIBLE?
REST_RISK?
LONG_REST_BENEFIT_AVAILABLE?
ENVIRONMENTAL_PROTECTION?
```

The AI's assessment does not override mechanics.

---

## 8I.19 Rest Interruption Probability

Rest interruption checks belong to RealmWeaver's world/rest systems.

Conceptually:

```text
World / Rest State
↓
risk calculation
↓
RNG
↓
event or no event
```

AI does not independently decide whether an interruption occurs.

This allows rest probability to be tested and balanced independently of the LLM.

---

## 8I.20 AI-Proposed Rest Events

Future orchestration may allow the AI to propose a contextual event after RealmWeaver determines that an event should occur.

Example:

```text
EVENT_REQUIRED

type = WILDERNESS_DISTURBANCE
constraints = [...]
```

AI may propose a specific narrative event.

RealmWeaver validates it before mechanical consequences are applied.

---

## 8I.21 Environmental Hazards

Environmental hazards are structured mechanical systems.

Example:

```text
EXTREME_COLD
```

RealmWeaver determines:

* exposure;
* protection;
* required saves;
* Exhaustion risk;
* other mechanical consequences.

AI narrates the environment but does not decide whether the rules apply.

---

## 8I.22 AI Environmental Suggestions

AI should be allowed to suggest contextually appropriate countermeasures.

Example:

> Better winter clothing, proper shelter, or magical protection would make the mountain crossing safer.

The player may then choose among actions such as:

* purchasing equipment;
* finding materials;
* finding shelter;
* using magic;
* changing route;
* accepting the risk.

---

## 8I.23 Structured Countermeasure Validation

Mechanically recognized protections should come from structured rules/content.

Conceptually:

```text
EXTREME_COLD

recognized_mitigations:
- COLD_WEATHER_CLOTHING
- COLD_RESISTANCE
- PROTECTED_SHELTER
- applicable magical protection
```

The AI may propose alternatives, but RealmWeaver determines whether the proposal actually satisfies the protection requirement.

---

## 8I.24 Player Creativity

Structured mechanics should not force players to use exact database terminology.

Example:

Player:

> I line my armor with wolf pelts, wrap myself in heavy blankets, and cover every exposed part of my body.

AI may interpret:

```text
PROPOSED_ENVIRONMENTAL_PROTECTION
type = COLD_PROTECTION
```

RealmWeaver then evaluates whether the proposal meets the relevant mechanical threshold.

### Principle

> **Structured rules should validate creativity, not restrict the player to UI buttons.**

---

## 8I.25 AI Failure and Committed State

If RealmWeaver successfully resolves a mechanical event and the subsequent AI narration fails, the mechanical state remains committed.

Example:

```text
Long Rest completed
HP restored
Hit Dice recovered
Exhaustion 2 → 1
↓
AI request times out
```

RealmWeaver does not rerun the Long Rest.

Narration may be retried from the existing mechanical result.

---

## 8I.26 Duplicate Request Protection

Retried or duplicate requests must not duplicate mechanical consequences.

Example:

A Long Rest request cannot accidentally produce:

```text
Exhaustion 2 → 1
```

and then on network retry:

```text
Exhaustion 1 → 0
```

for the same rest.

Likewise Hit Dice must not be spent twice because of frontend/network retries.

Architecture requires action identities/idempotency or an equivalent mechanism.

---

## 8I.27 Fast Validation

Most Group 8 mechanics should run through fast deterministic backend logic.

Examples:

```text
condition immunity
advantage/disadvantage
condition duration
rest eligibility
rest recovery
Hit Dice arithmetic
Exhaustion effects
environmental protection checks
```

These operations should not require LLM calls.

---

## 8I.28 Avoid AI Ping-Pong

RealmWeaver should avoid an architecture where ordinary actions repeatedly alternate between AI and rules systems.

Avoid:

```text
AI
↓
rules
↓
AI
↓
conditions
↓
AI
↓
inventory
↓
AI
```

Prefer:

```text
Player Intent
↓
Interpretation
↓
RealmWeaver gathers relevant state
↓
Rules resolve mechanics
↓
Structured result
↓
One primary AI narrative call
```

where practical.

---

## 8I.29 Deterministic UI Actions

Many explicit UI actions should not require AI interpretation.

Examples:

```text
[Stand Up]
[Spend Hit Die]
[Begin Short Rest]
[Begin Long Rest]
[End Concentration]
[Change Prepared Spells]
```

RealmWeaver can validate these actions directly.

AI narration may be skipped, deferred, or bundled into a later response where appropriate.

---

## 8I.30 Mechanical Audit Trail

RealmWeaver should be able to determine why a mechanical outcome occurred.

Example:

```text
Why was this attack rolled with disadvantage?

1. Attacker = POISONED
2. POISONED grants attack disadvantage
3. No valid advantage source
4. Final roll state = DISADVANTAGE
```

Rest example:

```text
Why did Long Rest fail?

1. Rest began 22:00
2. Strenuous interruption threshold exceeded
3. LONG_REST_FAILED
```

This supports debugging, testing, transparency, and future UI explanations.

---

## 8I.31 Player-Facing "Why?" Explanations

Where practical, RealmWeaver may expose explanations through UI.

Example:

```text
Attack Roll — Disadvantage
[Why?]
```

Expanded:

```text
Poisoned
→ Disadvantage on attack rolls
```

The underlying rules engine should support explanation/provenance even if the full UI feature is deferred.

---

## 8I.32 Structured AI Context

AI context should be assembled from structured relevant state.

Potential context may include:

* active actors;
* relevant HP/resources;
* conditions;
* Exhaustion;
* location/environment;
* legal constraints;
* recent mechanical events;
* relevant narrative history;
* known/hidden information boundaries.

Detailed context architecture is deferred.

---

## 8I.33 Rules Do Not Depend on Prompt Memory

RealmWeaver must never depend on the LLM remembering rules such as:

> Poisoned gives disadvantage on attack rolls.

That rule belongs to deterministic content/rules data.

The AI only needs enough information to narrate or make appropriate decisions.

---

## 8I.34 Group 8 Authority Flow

Condition example:

```text
PLAYER
"I try to knock the cultist down."
        ↓
AI / INTENT LAYER
proposes SHOVE
        ↓
REALMWEAVER
validates action
        ↓
DICE SYSTEM
resolves check
        ↓
RULES ENGINE
applies PRONE
        ↓
PERSISTENT STATE
stores condition
        ↓
MECHANICAL HISTORY
records event
        ↓
AI DM
narrates validated result
```

Rest example:

```text
PLAYER
"We camp here for the night."
        ↓
REST PROPOSAL
        ↓
REALMWEAVER
validates Long Rest
        ↓
WORLD / TIME SYSTEM
advances time and evaluates risk
        ↓
possible validated events
        ↓
REST COMPLETES
        ↓
RECOVERY ENGINE
restores applicable state
        ↓
STATE COMMIT
        ↓
AI DM
narrates night / morning
```

---

# Group 8 — Conditions & Resting Completion

**Status:** COMPLETE — APPROVED

All M2.1 Group 8 subsections have been reviewed and approved:

| Section | Topic                                            | Status       |
| ------- | ------------------------------------------------ | ------------ |
| 8A      | Core Condition Model                             | **Approved** |
| 8B      | Standard Conditions                              | **Approved** |
| 8C      | Condition Duration, Removal & Interaction        | **Approved** |
| 8D      | Exhaustion                                       | **Approved** |
| 8E      | Short Rests                                      | **Approved** |
| 8F      | Long Rests                                       | **Approved** |
| 8G      | Hit Dice & Recovery Rules                        | **Approved** |
| 8H      | Rest Restrictions, Interruptions & Campaign Time | **Approved** |
| 8I      | AI, NPCs, Persistence & Validation               | **Approved** |

---

## Group 8 RealmWeaver Adaptations

The following RealmWeaver-specific adaptations or implementation requirements were established during Group 8:

### Distance-Band Condition Interactions

Exact close-range requirements from the 2014 baseline are translated through RealmWeaver's approved distance-band combat system.

Examples include:

* qualifying close-range attacks against Paralyzed targets use **Near**;
* qualifying close-range attacks against Unconscious targets use **Near**;
* Prone attacker-distance effects use **Near** versus farther distance bands;
* Grappled uses an appropriate Near/engaged relationship;
* Frightened movement restrictions prohibit reducing distance toward the fear source.

Exact spatial implementation remains shared with Combat/Positioning architecture.

### Environmental Countermeasures

Environmental hazards may define structured preventative measures, protections, mitigations, and remedies.

AI may suggest creative solutions.

RealmWeaver determines whether they mechanically satisfy the hazard.

V1 supports a limited practical environmental/survival layer rather than comprehensive simulation.

### Probabilistic Rest Interruptions

Rest interruptions are:

* possible;
* context-sensitive;
* probabilistic;
* influenced by world state and player preparation.

They are not automatically generated whenever a character rests.

Exact probability and event-generation architecture is deferred to World & Campaign State design.

### Persistent Campaign Time

Conditions, spells, rests, travel, environmental systems, NPC schedules, and future world events require an authoritative persistent campaign timeline.

Group 8 establishes the requirement but intentionally does not finalize:

* calendar design;
* event-queue implementation;
* NPC schedules;
* off-screen world simulation;
* time-estimation algorithms.

These belong to later World & Campaign State design.

---

## Group 8 Architectural Requirements Identified

Detailed implementation is deferred, but Group 8 establishes requirements for:

* reusable Condition Definitions;
* persistent Condition Instances;
* structured condition sources;
* condition/effect dependencies;
* generic modifier/effect infrastructure;
* condition immunity;
* condition visibility/discovery state;
* Exhaustion state;
* persistent Hit Dice resources;
* Short/Long Rest state machines;
* rest-risk calculation;
* testable probabilistic event checks;
* campaign-time integration;
* scheduled world-event processing;
* NPC mechanical materialisation;
* off-screen NPC/world simulation levels;
* structured player/system knowledge boundaries;
* mechanical event history;
* state-change provenance;
* idempotent mechanical transactions;
* AI failure recovery;
* relevant structured AI context;
* deterministic explanation/audit support.

---

## Core Group 8 Principle

Conditions, Exhaustion, resting, recovery, environmental hazards, and related NPC state follow RealmWeaver's central authority model:

> **AI tells the story. Rules decide what happens.**

> **AI proposes. RealmWeaver validates.**

AI controls narrative interpretation, storytelling, roleplay, suggestions, and appropriate tactical intent.

RealmWeaver remains authoritative over:

* condition mechanics;
* duration;
* immunities;
* saves;
* Exhaustion;
* Hit Dice;
* rest qualification;
* recovery;
* environmental protection;
* time advancement;
* persistent state;
* mechanical outcomes.

```

