# 07 — Magic

**Status:** In Progress
**Milestone:** M2.1 — Game Rules
**Rules Baseline:** SRD 5.1 / 2014-style mechanics
**RealmWeaver Principle:** AI tells the story. Rules decide what happens.
**Validation Principle:** AI proposes. RealmWeaver validates.

---

## 7. Magic

RealmWeaver's magic system follows SRD 5.1 / 2014-style spellcasting mechanics as its primary baseline, with explicitly documented RealmWeaver adaptations where required for the digital campaign experience.

Spell mechanics, eligibility, resources, effects, and persistent state are authoritative RealmWeaver data. The AI Dungeon Master may interpret player intent, recommend valid spells, and narrate magical effects, but it does not determine authoritative spell mechanics or directly modify persistent spellcasting state.

V1 guarantees character levels **1–5**, with levels **1–10 as a stretch target**. The magic architecture must support eventual character progression through **level 20** and spell levels **0–9** without requiring fundamental redesign.

---

# 7A — Spellcasting Core & Spell Data Model

**Status:** Approved

## 7A.1 Structured Spell Definitions

Every mechanically supported spell must exist as structured RealmWeaver content.

A spell definition should conceptually contain information such as:

* Unique spell ID
* Name
* Spell level
* School of magic
* Casting time
* Range
* Target type
* Components
* Duration
* Concentration requirement
* Ritual status
* Attack type
* Required saving throw
* Damage
* Healing
* Mechanical effects
* Scaling rules
* Class/access relationships
* Rules source
* Rules version
* RealmWeaver adaptations, where applicable

The final database schema will be designed during the architecture/data-model milestones.

The AI is not an authoritative spell database.

When a player attempts to cast a spell, RealmWeaver retrieves the spell's structured definition and uses deterministic systems to validate and resolve its mechanics.

---

## 7A.2 Spell Definitions and Character Spell State

Reusable spell definitions must be separate from character-specific spell state.

A `SpellDefinition` describes what a spell mechanically is.

Character spell state describes the character's relationship with that spell, such as:

* known
* acquired
* prepared
* class-granted
* subclass-granted
* species-granted
* feature-granted
* item-granted
* campaign-granted

The complete spell definition must not be duplicated into every character record.

This separation allows RealmWeaver to support future spell acquisition systems without duplicating canonical spell mechanics.

---

## 7A.3 Spell Levels

RealmWeaver supports the standard spell-level structure:

* Cantrips — level 0
* Levelled spells — levels 1–9

Character level and spell level are separate concepts.

The architecture must support spell levels 0–9 even when the currently supported character-level range cannot access the higher spell levels.

For the guaranteed V1 character levels 1–5, full spellcasters may reach 3rd-level spells.

If the level 1–10 stretch target is implemented, full spellcasters may reach 5th-level spells.

The architecture must support eventual level-20 characters and 9th-level spells.

---

## 7A.4 Schools of Magic

RealmWeaver supports the eight standard SRD schools of magic:

1. Abjuration
2. Conjuration
3. Divination
4. Enchantment
5. Evocation
6. Illusion
7. Necromancy
8. Transmutation

Spell school is structured spell metadata.

Even where a school has limited immediate mechanical impact, it must be retained for future mechanics, filtering, AI narration, worldbuilding, and character features.

---

## 7A.5 Spell Access Sources

Spell availability must not be represented through a single hardcoded class field.

A spell may potentially become available through:

* Class
* Subclass
* Species
* Feature
* Feat
* Magic item
* Special campaign reward
* Other validated game content

This allows RealmWeaver to support campaign-specific spell access without incorrectly changing a character's class.

For example, a campaign reward may grant a character access to a specific spell without making that character a Wizard or Cleric.

---

## 7A.6 Rules Source and Versioning

Every imported or implemented spell definition must be traceable to its rules source and rules version.

RealmWeaver uses **SRD 5.1 / 2014-style mechanics** as the primary rules baseline.

Spell definitions should conceptually retain:

* Rules source
* Rules version
* Whether a RealmWeaver adaptation exists
* Description/reference for the adaptation where applicable

RealmWeaver must not silently mix incompatible spell versions from different rulesets.

Intentional adaptations must be documented explicitly.

---

## 7A.7 AI-Generated Spell Mechanics

During normal gameplay, the AI may not invent authoritative spell mechanics.

If a player describes an improvised magical action, the AI may interpret the intent and propose an existing valid spell or ability.

RealmWeaver must then validate the proposed action.

If no valid spell or ability supports the requested action, the AI cannot create arbitrary damage, effects, ranges, saving throws, or other mechanics simply because the action would be narratively interesting.

Future architecture may support validated custom or campaign-specific spells.

Such spells must become structured RealmWeaver content before being treated as authoritative mechanical abilities.

---

## 7A.8 Spell Catalogue

RealmWeaver should not require manually authoring the complete supported spell catalogue.

Where legally and technically appropriate, reusable spell content should follow the project's content-import strategy:

**Permitted/licensed source → Importer → Validation/normalisation → RealmWeaver internal data**

RealmWeaver's internal spell database is the runtime mechanical authority.

External spell APIs must not be required runtime dependencies for normal gameplay.

---

## 7A Approved Decisions

1. Structured spell definitions are mechanically authoritative.
2. Character spell access/state is separate from reusable spell definitions.
3. Architecture supports spell levels 0–9 while V1 content follows supported character levels.
4. RealmWeaver supports all eight standard schools of magic.
5. Spell access supports multiple sources rather than being tied directly to one class.
6. Every spell records its rules source/version and any RealmWeaver adaptations.
7. AI cannot invent authoritative spell mechanics during normal gameplay.
8. Architecture leaves room for future validated custom/campaign spells.
9. Spell catalogue content should be imported from appropriately permitted/licensed sources where practical.
10. External spell APIs are not required runtime dependencies.

---

# 7B — Spellcasters, Known Spells & Prepared Spells

**Status:** Approved

## 7B.1 Spell Access States

RealmWeaver distinguishes between four related concepts:

### Available

Spells the character could theoretically gain or use because of a class, feature, item, or another valid source.

### Known / Acquired

Spells that the character has actually learned or acquired where the relevant spellcasting system requires this distinction.

### Prepared

Spells currently selected and available for ordinary casting where preparation is required.

### Castable

A spell that currently satisfies all relevant eligibility and resource requirements.

Conceptually:

**Available → Known/Acquired → Prepared → Castable**

Not every spellcasting class must use every layer.

---

## 7B.2 Wizard Spellcasting

Wizard spellcasting follows the SRD 5.1 / 2014-style model.

### Wizard Cantrips

Wizards know a fixed number of cantrips according to class progression.

Known Wizard cantrips:

* do not require preparation;
* are normally always available once known;
* do not consume spell slots;
* may scale according to applicable spell rules.

### Wizard Spellbook

A Wizard maintains a persistent spellbook.

At character creation, a level-1 Wizard begins with **six 1st-level Wizard spells** in the spellbook.

When gaining another Wizard level under normal progression, the character adds **two Wizard spells** to the spellbook that the character is capable of casting.

Additional Wizard spells may eventually be discovered during adventures and copied into the spellbook according to applicable rules.

The exact spell-copying mechanics, costs, and associated gameplay systems are specified separately.

### Wizard Preparation

The normal Wizard preparation limit is:

**Wizard level + Intelligence modifier**

Minimum: **1 prepared spell**

Prepared spells are selected from eligible spells contained in the Wizard's spellbook.

---

## 7B.3 Cleric Spellcasting

Clerics do not use a Wizard-style spellbook for ordinary Cleric spell access.

A Cleric has access to the appropriate Cleric spell list for spell levels the character is capable of casting.

The player prepares eligible spells from that list.

The normal Cleric preparation limit is:

**Cleric level + Wisdom modifier**

Minimum: **1 prepared spell**

This maintains an important mechanical distinction:

**Wizard:** builds a personal spell library and prepares from that library.

**Cleric:** accesses the eligible Cleric spell list and prepares directly from it.

---

## 7B.4 Automatically Prepared and Granted Spells

RealmWeaver must support spells that are automatically prepared or granted by sources such as:

* Class
* Subclass
* Species
* Feature
* Other validated game content

Automatically prepared/granted spells may exist outside the character's normal preparation limit where the applicable rule specifies this.

Character spell state should retain the source of such access.

---

## 7B.5 Changing Prepared Spells

Under normal Wizard and Cleric spellcasting rules, prepared spells may be changed following a valid **long rest**.

Changing prepared spells after a long rest is optional.

If the player does not choose to modify preparation, the existing prepared spell selection remains unchanged.

RealmWeaver must not allow the AI to silently change prepared spells because another spell would be more useful in the current situation.

---

## 7B.6 Spell Preparation UI

Following a valid long rest, RealmWeaver may provide a non-blocking option such as:

**Change Prepared Spells**

The player must not be forced through spell preparation after every long rest.

Existing selections remain unless explicitly changed by the player or another validated rule.

Detailed UI implementation is deferred to the UI/UX milestone.

---

## 7B.7 Cantrips

Cantrips are treated separately from prepared levelled spells.

Once legitimately known, a cantrip:

* normally remains available;
* does not require preparation;
* does not consume spell slots;
* follows any applicable character-level scaling rules.

---

## 7B.8 Spell Eligibility Validation

RealmWeaver's deterministic systems validate spell eligibility.

Validation may include:

1. Does the spell exist?
2. Does the character have legitimate access to it?
3. Is it known/acquired where required?
4. Is it prepared where required?
5. Can the character cast spells of that level?
6. Are the required resources available?
7. Are current casting conditions valid?
8. Are any additional spell-specific requirements satisfied?

The AI must not be responsible for authoritative eligibility validation.

---

## 7B.9 AI Spell Assistance

The AI Dungeon Master may examine authoritative character spell state and explain or recommend legitimate options.

For example, if a player asks whether they possess a healing option, the AI may identify a currently prepared healing spell and explain its use.

The AI may not recommend an unavailable spell as though the character were capable of casting it.

Any AI-proposed spell action must still pass normal RealmWeaver validation.

---

## 7B Approved Decisions

1. RealmWeaver distinguishes available, known/acquired, prepared, and castable spells.
2. Wizards use persistent spellbooks.
3. A level-1 Wizard normally begins with six 1st-level Wizard spells.
4. Wizards normally add two eligible Wizard spells to their spellbook when gaining another Wizard level.
5. Wizard preparation limit = Wizard level + Intelligence modifier, minimum 1.
6. Clerics access the eligible Cleric spell list rather than maintaining Wizard-style learned spellbooks.
7. Cleric preparation limit = Cleric level + Wisdom modifier, minimum 1.
8. Architecture supports automatically prepared/granted spells that do not consume normal preparation capacity where applicable.
9. Wizards and Clerics normally change prepared spells following a valid long rest.
10. Changing prepared spells following a long rest is optional.
11. Cantrips are known and do not require preparation.
12. Deterministic RealmWeaver systems validate spell eligibility.
13. AI may recommend and explain legitimate spell options but cannot grant unavailable spells.
14. Architecture supports future spell access from subclasses, species, feats, items, features, and special campaign rewards.

---

# 7C — Spell Slots & Resource Consumption

**Status:** Approved

## 7C.1 Standard Spell Slot Progression

Wizard and Cleric use the SRD 5.1 / 2014-style full-caster spell-slot progression.

For the guaranteed V1 character levels:

| Class Level | 1st | 2nd | 3rd |
| ----------- | --: | --: | --: |
| 1           |   2 |   — |   — |
| 2           |   3 |   — |   — |
| 3           |   4 |   2 |   — |
| 4           |   4 |   3 |   — |
| 5           |   4 |   3 |   2 |

The architecture must support the complete full-caster progression through character level 20 even when higher levels are not initially enabled.

RealmWeaver tracks maximum and remaining spell slots separately.

---

## 7C.2 Spell Slots and Individual Spells

Spell slots belong to the character's spellcasting resource state.

They are not attached to individual prepared spells.

Preparing a spell does not provide a fixed number of casts of that spell.

An available spell slot may be spent on any currently valid spell for which that slot is eligible.

---

## 7C.3 Cantrips

Cantrips do not consume spell slots.

They remain subject to other casting requirements, including:

* action economy;
* range;
* targeting;
* components where enabled;
* conditions;
* other spell-specific restrictions.

---

## 7C.4 Minimum Slot Level

A levelled spell normally requires a spell slot equal to or greater than the spell's base level.

RealmWeaver deterministically validates slot eligibility.

A spell cannot be cast using a slot lower than its required spell level unless an explicit rule provides an exception.

---

## 7C.5 Higher-Level Casting and Upcasting

A lower-level spell may be cast using a higher-level spell slot where permitted by the normal spellcasting rules.

Using a higher-level slot does not automatically improve every spell.

Any additional effect from higher-level casting must come from structured spell scaling data.

RealmWeaver, not the AI, calculates the mechanical effect of upcasting.

---

## 7C.6 Spell Slot Selection

RealmWeaver should avoid unnecessary player prompts while preserving control over meaningful resource decisions.

### Single Valid Slot Choice

If only one valid slot level exists, RealmWeaver may automatically use that slot.

### Multiple Meaningfully Different Choices

If multiple slot levels are available and choosing a higher level changes the spell's mechanical effect, the player should be given the relevant choice.

### No Higher-Level Benefit

If casting the spell using a higher-level slot provides no mechanical benefit, RealmWeaver may automatically select the lowest valid available slot.

Future UI/preferences may allow players to choose between:

* always asking;
* automatically using the lowest valid slot;
* AI recommendation followed by player confirmation.

---

## 7C.7 Resource Mutation

A spell slot is consumed only after:

1. the proposed cast has been validated;
2. required player choices have been resolved;
3. the spell cast is committed as a valid game action.

Invalid or rejected casts consume no spell slot.

RealmWeaver follows the general rule:

**Validate first → mutate state second.**

---

## 7C.8 Insufficient Spell Slots

If a character lacks a valid spell slot for a requested levelled spell, RealmWeaver rejects the cast.

The rejection should provide a structured mechanical reason such as:

`NO_VALID_SPELL_SLOT`

The AI may explain the failure naturally and identify legitimate alternatives, such as available cantrips, but may not grant additional spell slots.

---

## 7C.9 Long Rest Restoration

Completing a mechanically valid long rest restores normal Wizard and Cleric spell slots to their maximum values according to applicable rules.

The AI cannot restore spell slots merely by narrating that the character rested.

RealmWeaver must first determine that the rest mechanically qualifies.

Complete resting rules are defined separately.

---

## 7C.10 Wizard Arcane Recovery

Wizard supports **Arcane Recovery** using the SRD 5.1 / 2014-style rule.

Once per applicable recovery period, following a qualifying short rest, a Wizard may recover expended spell slots.

The combined slot levels recovered may not exceed:

**ceil(Wizard level ÷ 2)**

Applicable restrictions on recoverable slot levels must be retained for eventual level-20 support.

For guaranteed V1 levels:

| Wizard Level | Combined Recoverable Slot Levels |
| ------------ | -------------------------------: |
| 1            |                                1 |
| 2            |                                1 |
| 3            |                                2 |
| 4            |                                2 |
| 5            |                                3 |

The player may choose an eligible combination of expended slots within the permitted total.

---

## 7C.11 Extensible Character Resources

RealmWeaver must not assume that spell slots are the only magical or class resource.

The architecture must eventually support resources such as:

* Spell slots
* Arcane Recovery
* Channel Divinity
* Class feature uses
* Item charges
* Future class-specific resources
* Other validated resource pools

The final resource data model will be determined during architecture.

---

## 7C.12 Spell Resource UI

Spell information should be accessible during a campaign without permanently occupying significant narrative space.

The campaign interface should provide an on-demand **Spells** control.

Conceptually, campaign controls may include:

`[Character] [Inventory] [Spells] [Goals] [Dice]`

Selecting **Spells** reveals the relevant spellcasting information.

The panel should be capable of displaying:

* known cantrips;
* prepared spells;
* available/remaining spell slots;
* spell levels;
* relevant spell actions;
* later concentration/effect information.

For example:

```text
SPELLS

Spell Slots
1st   ● ● ● ○    3 / 4
2nd   ● ○        1 / 2

CANTRIPS
Fire Bolt
Mage Hand
Light

PREPARED
Magic Missile
Shield
Mage Armor
Misty Step
```

The panel should be collapsible/closeable so that the campaign narrative remains the primary workspace.

The exact presentation mechanism — side drawer, modal, expandable panel, popover, or another responsive design — is deferred to the UI/UX milestone.

### Campaign UI Principle

**Narrative is the primary campaign workspace. Mechanical information should be quickly accessible on demand rather than permanently competing with the story for screen space.**

This principle should also guide other campaign interfaces such as inventory, goals, character information, and detailed mechanical results.

---

## 7C.13 AI Resource Authority

The AI may narrate the use or restoration of a resource only after RealmWeaver has validated the corresponding mechanical event.

The AI may not directly:

* consume spell slots;
* restore spell slots;
* create additional resource uses;
* modify maximum resources;
* grant temporary resources without a validated effect.

Narrative events such as shrines, blessings, magical locations, quests, or artifacts may propose resource changes, but persistent state changes require a valid RealmWeaver mechanical/campaign effect.

---

## 7C Approved Decisions

1. Wizard and Cleric use standard SRD 5.1 / 2014-style full-caster spell-slot progression.
2. Architecture supports full spell-slot progression through character level 20.
3. Spell slots are character resources separate from individual spells.
4. Cantrips consume no spell slots.
5. Levelled spells require a slot at least equal to the spell's level.
6. Higher-level slots may cast eligible lower-level spells.
7. Upcasting effects come from structured spell definitions.
8. If multiple meaningfully different slot choices exist, the player chooses.
9. If only one valid choice exists, RealmWeaver may automatically select it.
10. If higher slots provide no mechanical benefit, RealmWeaver may automatically use the lowest valid slot.
11. Slots are consumed only after successful validation and action commitment.
12. Invalid/rejected casts consume no resources.
13. Valid long rests restore normal Wizard/Cleric spell slots.
14. Wizard supports Arcane Recovery using SRD 5.1 / 2014-style mechanics.
15. RealmWeaver's resource architecture must support resources beyond spell slots.
16. Spell information and resources must be accessible through an on-demand campaign UI control.
17. The Spells interface must not permanently consume significant narrative space.
18. AI cannot directly alter authoritative resource state.
19. Special narrative resource restoration requires a validated mechanical/campaign effect.
20. Future UI/preferences may provide configurable automatic/manual spell-slot selection.

---

# 7D — Casting a Spell & Action Economy

**Status:** Approved

## 7D.1 Casting Time

Every supported spell must contain a structured casting time.

RealmWeaver must support casting-time categories including:

* Action
* Bonus Action
* Reaction
* Extended casting times measured in rounds, minutes, hours, or other applicable units

Casting time is authoritative spell data.

The AI may interpret that a player intends to cast a spell, but RealmWeaver determines the actual casting time and whether the required action resource is available.

---

## 7D.2 Action Spells

A spell with a casting time of one Action requires the caster to have an available Action.

Once that Action has been consumed, another Action spell cannot normally be cast during the same turn unless another validated feature grants an additional Action or otherwise permits it.

Action availability is determined by the RealmWeaver combat/action-economy system.

---

## 7D.3 Bonus-Action Spells

A spell with a casting time of one Bonus Action requires an available Bonus Action.

RealmWeaver follows the SRD 5.1 / 2014-style bonus-action spell restriction.

When a character casts a spell using a Bonus Action, the character cannot cast another spell during that turn except for a **cantrip with a casting time of one Action**, unless another explicit rule provides an exception.

RealmWeaver must implement the actual bonus-action casting restriction rather than simplifying it to a generic "one levelled spell per turn" rule.

---

## 7D.4 Reaction Spells

Reaction spells require:

1. an available Reaction;
2. the appropriate triggering event;
3. all normal spellcasting requirements to be satisfied.

RealmWeaver determines whether the trigger occurred and whether the Reaction remains available.

The AI may narrate the triggering event but does not determine mechanical reaction eligibility.

---

## 7D.5 Player-Controlled Reaction Prompts

When a valid player-controlled spell reaction becomes available, RealmWeaver should prompt the player before consuming the Reaction or associated resources.

For example:

```text
INCOMING ATTACK

The enemy's attack would hit you.

Shield is available.

[Cast Shield]
[Don't Cast]
```

If the player chooses to cast the spell:

**Validate → consume Reaction/resource → apply spell → recalculate affected mechanics → update state → AI narrates**

RealmWeaver must not automatically spend a player's Reaction or spell slot without player permission.

---

## 7D.6 Reaction Prompt Behaviour

For V1, RealmWeaver should default to prompting whenever a valid player-controlled reaction is available.

Future versions may support preferences such as:

* Always ask
* Smart prompts
* Do not prompt

These preferences are deferred until later UI/UX design and playtesting.

---

## 7D.7 Casting Outside Combat

Casting a spell outside combat does not automatically create initiative or turn-based combat.

RealmWeaver still validates:

* spell eligibility;
* resources;
* components where applicable;
* targets;
* range;
* other spell requirements.

Action/Bonus Action terminology does not need to produce unnecessary turn bookkeeping outside situations where action timing matters.

---

## 7D.8 Extended Casting and Campaign Time

Spells with extended casting times must use structured time values.

For example:

```text
casting_time:
    value = 10
    unit = MINUTES
```

Mechanically defined casting times are determined by RealmWeaver rather than the AI.

Extended casting advances authoritative campaign time.

The persistent campaign-time system is specified separately and implemented during the appropriate later architecture milestone.

---

## 7D.9 Interrupted Casting

RealmWeaver must support casting activities that remain in progress for a period of time.

An extended cast may be interrupted by mechanically relevant events.

Conceptually, an in-progress cast may retain:

* spell;
* caster;
* start time;
* required completion time;
* current status;
* interruption state.

RealmWeaver, not AI memory, determines whether an extended cast successfully completes.

---

## 7D.10 Casting Through Natural Language and UI

RealmWeaver supports two primary spellcasting interaction paths.

### Natural-Language Casting

The player may describe the intended action naturally.

Example:

> "I fire Magic Missile at the goblin."

The AI or intent-processing layer interprets the request into a structured proposed action.

### Structured UI Casting

The player may open the Spells interface, select a spell, choose **Cast**, and provide any required targets or options.

Structured UI casting does not require AI intent interpretation where the player's mechanical intention is already explicit.

Both interaction paths must enter the **same deterministic casting pipeline**.

Neither path bypasses RealmWeaver validation.

---

## 7D.11 AI Spell Suggestions

The AI may suggest mechanically legitimate spells based on authoritative character state.

Suggested spells must still pass normal validation before casting.

AI recommendations do not provide additional spell access or bypass resource, action, targeting, component, or eligibility requirements.

---

## 7D.12 Required Player Choices

A spell cannot be committed until all mechanically required player decisions have been resolved.

These may include:

* target;
* targets;
* target distribution;
* spell-slot level;
* area placement;
* optional spell behaviour;
* other spell-specific choices.

The AI may interpret or propose choices from natural language but must not arbitrarily make significant resource or targeting decisions for the player.

---

## 7D.13 Authoritative Casting Pipeline

Spellcasting should conceptually follow:

**Player Intent**
↓
**AI/Intent Interpretation, where required**
↓
**Proposed Cast**
↓
**Spell Existence Validation**
↓
**Character Eligibility Validation**
↓
**Known/Prepared Validation**
↓
**Action-Economy Validation**
↓
**Casting Restriction Validation**
↓
**Target/Range Validation**
↓
**Resource Validation**
↓
**Resolve Required Player Choices**
↓
**Cast Validated**
↓
**Commit Action**
↓
**Consume Action/Reaction**
↓
**Consume Required Resources**
↓
**Resolve Mechanics**
↓
**Update Persistent Game State**
↓
**AI Narrates Resolved Result**

Validation must occur before authoritative state mutation.

---

## 7D.14 Recoverable Validation Failures

Failed spellcasting validation should return structured reasons rather than generic errors.

Examples may include:

* `SPELL_NOT_KNOWN`
* `SPELL_NOT_PREPARED`
* `INSUFFICIENT_ACTION`
* `REACTION_UNAVAILABLE`
* `NO_VALID_SPELL_SLOT`
* `TARGET_OUT_OF_RANGE`
* `INVALID_TARGET`
* `REQUIRED_COMPONENT_MISSING`

The UI may display these directly where appropriate.

The AI may transform structured failures into natural explanations and suggest legitimate alternatives.

This validation pattern should eventually be reused for non-spell RealmWeaver actions.

---

## 7D Approved Decisions

1. Casting times are structured authoritative spell data.
2. Combat casting consumes the appropriate action resource.
3. RealmWeaver uses the actual SRD 5.1 / 2014-style Bonus Action spell restriction rather than a simplified one-spell-per-turn rule.
4. Reaction spells require their proper trigger.
5. Player-controlled spell reactions generate a reaction prompt.
6. RealmWeaver cannot automatically spend a player's Reaction or associated resources without permission.
7. V1 defaults to prompting whenever a valid player-controlled reaction is available.
8. Future reaction-prompt preferences are supported architecturally.
9. Outside combat, normal spellcasting does not unnecessarily invoke turn-based combat.
10. Extended casting advances authoritative campaign time.
11. RealmWeaver supports interrupted/in-progress extended casting.
12. Players may cast through natural language or the structured Spells UI.
13. Both casting paths use the same deterministic rules pipeline.
14. AI may suggest legitimate spells but cannot bypass validation.
15. Required player choices must be resolved before committing a cast.
16. Resources and action state mutate only after successful validation.
17. AI narration occurs after mechanical resolution.
18. Failed validation produces structured, recoverable reasons.
19. The validation pattern should eventually underpin non-spell RealmWeaver actions.

---

# 7E — Spell Attacks, Saving Throws & Spell Save DC

**Status:** Approved

## 7E.1 Spellcasting Ability

Spellcasting ability is determined by the applicable class, feature, or other validated spellcasting source.

For the V1 spellcasting classes:

* **Wizard — Intelligence**
* **Cleric — Wisdom**

Spellcasting ability should be represented as configurable class/feature data rather than implemented as a globally hardcoded class check.

This allows future classes and features to introduce other spellcasting abilities without redesigning the spell-resolution system.

---

## 7E.2 Spell Attack Modifier

The standard spell attack modifier is:

**Proficiency Bonus + Spellcasting Ability Modifier**

For example:

```text
Level 4 Wizard
Intelligence 18 → +4
Proficiency Bonus → +2

Spell Attack Modifier = +6
```

RealmWeaver calculates this deterministically from authoritative character state.

---

## 7E.3 Shared Attack Resolution

Spell attacks should reuse RealmWeaver's existing attack-resolution infrastructure.

The shared attack pipeline handles mechanics such as:

* d20 rolls;
* attack modifiers;
* Armor Class;
* advantage/disadvantage;
* natural 20s;
* natural 1s;
* critical hits;
* other applicable attack modifiers.

Weapon attacks and spell attacks may derive their modifiers differently but should not require independent attack engines.

---

## 7E.4 Spell Save DC

The standard Spell Save DC is:

**8 + Proficiency Bonus + Spellcasting Ability Modifier**

For example:

```text
Level 5 Cleric
Wisdom 18 → +4
Proficiency Bonus → +3

Spell Save DC = 15
```

RealmWeaver calculates Spell Save DC from authoritative character state.

---

## 7E.5 Required Saving Throw

The saving throw required by a spell must come from its structured Spell Definition.

The AI must not determine which ability saving throw "feels appropriate."

Spell metadata may specify saving throws such as:

* Strength
* Dexterity
* Constitution
* Intelligence
* Wisdom
* Charisma

RealmWeaver retrieves and resolves the specified save.

---

## 7E.6 Save Success and Failure Outcomes

RealmWeaver must not assume that every successful spell saving throw produces half damage.

Each Spell Definition must describe the applicable outcome for:

* successful save;
* failed save.

Possible behaviours include:

* full damage / half damage;
* full effect / no effect;
* condition / no condition;
* different effect strengths;
* other spell-specific results.

The structured spell definition is authoritative.

---

## 7E.7 Natural 20s, Natural 1s and Spell Attacks

Spell attack rolls follow the existing RealmWeaver attack rules for natural 20s, natural 1s, and critical hits.

Saving throws do not automatically gain critical success or critical failure mechanics merely because the saving throw produced a natural 20 or natural 1 unless an explicit applicable rule provides such behaviour.

---

## 7E.8 NPC and Enemy Saving Throws

RealmWeaver rolls NPC and enemy saving throws through the deterministic dice system.

The AI does not determine whether an NPC succeeds or fails based on narrative convenience.

The process is:

**Required Save → Target Modifier → System Dice Roll → Compare With DC → Resolve Spell Effect**

---

## 7E.9 Player Saving Throws Against Magic

When a player character makes a saving throw against a spell, RealmWeaver uses the existing dice-control preference.

### Player-Controlled Dice

RealmWeaver prompts the player to roll.

### System-Controlled Dice

RealmWeaver performs the roll automatically.

Spell saving throws therefore behave consistently with other RealmWeaver saving throws.

---

## 7E.10 Saving Throw DC Visibility

For V1, when the player is actively making a saving throw, RealmWeaver should normally display the relevant DC.

For example:

```text
WISDOM SAVING THROW

DC: 15
Modifier: +4

[Roll]
```

Mechanical transparency helps reinforce that outcomes are controlled by deterministic rules rather than secretly changed by the AI.

Future immersive modes may optionally alter the amount of mechanical information shown.

---

## 7E.11 Inspectable NPC Mechanical Resolution

NPC/enemy spell saving throws and similar mechanical outcomes should be inspectable by the player.

Detailed mechanical numbers do not need to clutter the AI narrative.

RealmWeaver may instead provide a compact result such as:

```text
Hold Person
Bandit — WIS Save: 10 vs DC 15 → Failed

[Details]
```

The narrative layer can then describe the result naturally.

This preserves both:

* narrative immersion;
* mechanical transparency.

---

## 7E.12 Mechanics and Narrative Separation

Detailed mechanical resolution should be available through compact or expandable UI elements.

For example:

```text
Guiding Bolt — Hit — 15 Radiant
[Details]
```

Expanded details may show:

* attack roll;
* modifiers;
* target AC;
* damage dice;
* damage type;
* other relevant calculations.

The AI narrative should not be required to repeatedly state every mechanical number.

---

## 7E.13 Advantage and Disadvantage

Spell attack rolls use RealmWeaver's shared advantage/disadvantage system.

Active conditions, features, effects, and environmental circumstances determine whether the roll is:

* normal;
* advantage;
* disadvantage.

The AI may explain the circumstances but does not determine the authoritative roll state independently.

---

## 7E.14 Saving Throw Modifiers

Bonuses, penalties, conditions, features, and magical effects that modify saving throws use the shared RealmWeaver modifier/effect system.

Magic must not maintain a separate saving-throw modifier system.

---

## 7E.15 Multiple Targets

When a spell requires multiple affected creatures to make saving throws, RealmWeaver resolves a separate saving throw for each affected creature unless the spell explicitly specifies otherwise.

Each target's result and resulting effect are resolved independently.

---

## 7E.16 Player-Controlled Dice and NPC Rolls

For V1, selecting player-controlled dice primarily affects rolls made by the player's character.

Examples include:

* player spell attack;
* player ability check;
* player saving throw;
* player concentration check.

NPC and enemy rolls remain system-controlled.

This prevents area spells and multi-creature encounters from requiring excessive manual rolling and preserves campaign pacing.

A future highly manual tabletop mode may expand player control over NPC dice.

---

## 7E.17 Structured Spell Resolution

Spell resolution should eventually produce a structured mechanical result.

Conceptually:

```text
SpellResolution

caster = Player
spell = Sacred Flame
target = Cultist

save:
    ability = DEX
    dc = 14
    roll = 12
    modifier = +1
    total = 13
    result = FAILURE

damage:
    dice = 1d8
    result = 6
    type = RADIANT

state_change:
    target_hp_before = 17
    target_hp_after = 11
```

The AI Dungeon Master receives the resolved mechanical result and narrates it.

The AI must not be asked to independently determine whether the spell mechanically succeeds.

---

## 7E Approved Decisions

1. Wizard spellcasting ability is Intelligence.
2. Cleric spellcasting ability is Wisdom.
3. Spellcasting ability is configurable by class/feature rather than globally hardcoded.
4. Spell Attack Modifier = proficiency bonus + spellcasting ability modifier.
5. Spell Save DC = 8 + proficiency bonus + spellcasting ability modifier.
6. Spell attack rolls reuse RealmWeaver's existing attack-resolution engine.
7. Required saving throw ability comes from structured Spell Definitions.
8. Save success/failure effects are defined per spell rather than through a universal half-damage rule.
9. Spell attack rolls follow existing natural-20/natural-1 and critical-hit rules.
10. Saving throws do not automatically gain critical-success/failure mechanics.
11. RealmWeaver rolls NPC/enemy spell saves deterministically.
12. Player saving throws against spells use the existing dice-control preference.
13. V1 normally shows the DC when the player makes a saving throw.
14. Mechanical spell resolution remains inspectable.
15. Detailed mechanical information should be available through expandable UI rather than cluttering story narration.
16. Advantage/disadvantage uses RealmWeaver's shared deterministic system.
17. Buffs, debuffs, conditions, and other effects modify attacks/saves through the shared modifier system.
18. Multi-target saving throws resolve separately for each affected creature.
19. NPC/enemy dice remain system-controlled in V1 to preserve pacing.
20. Spell resolution produces structured mechanical results for AI narration.

---

# 7F — Range, Targets, Areas & Distance Bands

**Status:** Approved
**RealmWeaver Adaptation:** Exact SRD ranges and geometry are retained as canonical data while V1 presents simplified distance bands and resolves positioning without requiring a battle map.

## 7F.1 Canonical Spell Range

RealmWeaver must preserve the exact SRD range in structured Spell Definitions.

Examples include:

* 30 feet
* 60 feet
* 120 feet
* Touch
* Self

The player-facing combat system may simplify these values through RealmWeaver distance bands, but canonical mechanical information must not be discarded.

Preserving exact range supports:

* consistent mechanical validation;
* future positioning improvements;
* optional tactical combat;
* future grid/battle-map support;
* rules transparency.

---

## 7F.2 RealmWeaver Distance Bands

V1 uses simplified player-facing distance concepts including:

* **Near**
* **Short**
* **Long**
* **Beyond**

These provide a readable abstraction over exact tabletop measurements.

The precise shared mapping and positional implementation must remain consistent with RealmWeaver's Combat and Movement systems.

---

## 7F.3 Player-Facing Bands and Internal Positioning

The UI may display simplified information such as:

```text
Cultist — Near
Goblin — Short
Archer — Long
```

RealmWeaver may maintain more precise underlying positional information where required for deterministic mechanical resolution.

The internal representation does not need to be a full two-dimensional battle-map grid in V1.

Possible representations may include:

* relative distances;
* zones;
* clusters;
* positional relationships;
* combinations of these concepts.

The final implementation is deferred to architecture.

### Principle

**Distance bands are the player-facing abstraction; RealmWeaver may preserve more precise authoritative positional information underneath.**

---

## 7F.4 Touch Spells

A spell with a range of **Touch** requires the target to be within appropriate physical/Near range.

If the target is too far away, the cast is invalid until the caster moves sufficiently close.

RealmWeaver validates the range rather than allowing AI narration to override it.

---

## 7F.5 Self and Area-From-Self Spells

RealmWeaver must distinguish between targeting categories such as:

* Self
* Touch
* Ranged
* Area originating from Self

A spell defined as `Self` may still create an area originating from the caster, such as a cone.

The structured spell definition must retain this distinction.

---

## 7F.6 Ranged Spell Validation

For a ranged spell, RealmWeaver compares authoritative caster/target positioning against the spell's permitted range.

If the target is outside the valid range, the cast is rejected with a structured reason such as:

`TARGET_OUT_OF_RANGE`

The AI may explain the failure but cannot change the mechanical range.

---

## 7F.7 Authoritative Positioning

AI narration cannot independently change authoritative combat positioning.

If narration describes a creature moving, an actual validated movement/state change must correspond to that description where the movement is mechanically relevant.

Conceptually:

```text
NPC_MOVE
from = SHORT
to = LONG
```

must be committed mechanically before the new position becomes authoritative.

### Principle

**Narration describes position; RealmWeaver determines position.**

---

## 7F.8 Single-Target Spells

Single-target spells must validate applicable requirements including:

* target count;
* target type;
* range;
* visibility;
* spell-specific eligibility;
* other applicable restrictions.

The AI may propose a target from natural-language intent, but RealmWeaver determines whether that target is mechanically valid.

---

## 7F.9 Multi-Target Spells

Spell definitions must support structured multi-target constraints.

Possible metadata includes:

* maximum target count;
* target-selection rules;
* whether duplicate targeting is permitted;
* range requirements;
* distribution rules.

The player controls significant target-distribution decisions.

RealmWeaver validates the final target selection before committing the spell.

---

## 7F.10 Area-of-Effect Geometry

RealmWeaver must preserve standard structured area geometry where applicable.

Supported metadata should accommodate shapes such as:

* sphere/radius;
* cone;
* line;
* cube;
* cylinder;
* other applicable shapes.

For example:

```text
area:
    shape = SPHERE
    radius_ft = 20
```

Simplified V1 combat must not replace canonical geometry with vague AI concepts such as "hits nearby enemies."

---

## 7F.11 V1 Area Resolution

Without requiring a battle map, V1 should resolve area effects through deterministic positioning concepts such as:

* zones;
* clusters;
* relative positions;
* authoritative distance relationships.

RealmWeaver determines which creatures occupy the affected area.

The AI does not decide which creatures are "probably close enough."

This is an explicit RealmWeaver adaptation of exact tabletop spatial geometry.

---

## 7F.12 Friendly Fire

Area spells affect allies where the normal spell rules and positioning would include them.

RealmWeaver must not automatically reposition allies or alter an area to protect friendly creatures.

When the selected area would affect an ally, RealmWeaver should warn the player before committing the cast.

For example:

```text
WARNING

This spell area also includes Kael.

[Cast Anyway]
[Choose Different Area]
```

The final decision belongs to the player.

---

## 7F.13 Area Placement

For spells requiring area placement, the player controls the final mechanically significant placement.

The AI may interpret natural-language intentions such as:

> "I place the Fireball behind the goblins so Kael isn't caught in it."

RealmWeaver then determines whether that requested placement is mechanically possible.

When multiple valid interpretations exist, the player should be allowed to resolve the ambiguity rather than having the AI arbitrarily choose.

---

## 7F.14 Lines, Cones and Directional Effects

Directional areas such as lines and cones must use deterministic positioning information.

For V1, RealmWeaver may identify valid directional target groups using zones, clusters, or relative-position information.

The player chooses the relevant direction where meaningful.

This simplified directional resolution is an explicit RealmWeaver adaptation for V1 combat without a tactical battle map.

Canonical spell geometry remains stored for future systems.

---

## 7F.15 Line of Sight, Line of Effect and Cover

Spell range alone does not automatically make a target valid.

Where applicable, spell targeting must interact with RealmWeaver's shared systems for:

* line of sight;
* visibility;
* obstruction;
* cover;
* line of effect.

Magic should reuse shared Combat/Positioning systems rather than implementing independent visibility or cover logic.

---

## 7F.16 Target Visibility Requirements

Spell Definitions must be capable of representing requirements such as:

* visible creature;
* visible point;
* creature in range;
* point in range;
* other spell-specific target restrictions.

The AI must not infer these requirements from memory when structured spell data provides the authoritative rule.

---

## 7F.17 Hidden and Unseen Targets

Spell targeting must respect what the player character can legitimately perceive or target.

The AI may possess broader campaign context than the player character, but that information cannot be used to bypass mechanical visibility or targeting restrictions.

### Principle

**The AI may know more than the player character, but mechanics may only use information mechanically available to the character.**

This principle should later be incorporated into RealmWeaver's AI-context architecture.

---

## 7F.18 Shared Positioning and Range System

RealmWeaver should eventually use one shared positioning/range system for:

* movement;
* melee reach;
* weapon range;
* spell range;
* monster abilities;
* area effects;
* other distance-dependent mechanics.

Spell range must not use a different interpretation of distance from weapons or movement.

### Principle

**The same mechanical distance must mean the same thing throughout RealmWeaver.**

---

## 7F.19 Future Battle-Map Compatibility

RealmWeaver should preserve exact range and area geometry now so future tactical/battle-map systems can use the existing spell catalogue.

V1 may use:

**Distance bands + zones/clusters + relative positioning**

A future implementation may introduce:

* exact coordinates;
* grids;
* maps;
* tokens;
* precise area overlays.

The spell system should not require fundamental redesign to support this evolution.

---

## 7F.20 Range and Area UI

The primary Spells interface should favor RealmWeaver's simplified presentation.

For example:

```text
Fireball

Range: Long
Area: 20-ft radius
Target: Area
Save: Dexterity

[Cast]
```

Expandable details may expose canonical rules information such as:

```text
Exact Range: 150 ft
Area: 20-ft-radius sphere
```

This allows newer players to use the simplified RealmWeaver abstraction while experienced players can inspect precise rules.

---

## 7F Approved Decisions

1. Preserve exact SRD range values in structured Spell Definitions.
2. Use Near/Short/Long as the primary player-facing distance abstraction, with Beyond representing targets outside normal current reach where appropriate.
3. RealmWeaver may maintain more precise underlying positional state than the UI exposes.
4. Touch spells require appropriate Near/contact range.
5. Self, Touch, Ranged, and Self-originating area spells are distinct targeting concepts.
6. Range validation is deterministic.
7. AI narration cannot independently alter authoritative positioning.
8. Single-target spells validate target count, type, range, and other requirements.
9. Multi-target spells use structured target constraints.
10. Preserve standard area geometry metadata such as sphere/radius, cone, line, cube, and cylinder.
11. V1 resolves areas through deterministic zones/clusters/relative positioning rather than AI judgment.
12. Friendly fire remains possible where normal spell rules and positioning allow it.
13. RealmWeaver warns before committing an area spell that includes allies.
14. The player controls ambiguous area placement; AI may only interpret/propose.
15. Cones, lines, and similar directional effects use deterministic directional grouping in V1 as an explicit RealmWeaver adaptation.
16. Spell targeting reuses shared line-of-sight, cover, visibility, and positioning systems.
17. Spell metadata explicitly represents targeting and visibility requirements.
18. AI knowledge unavailable to the player character cannot be used to bypass targeting rules.
19. One shared positioning/range system should govern spells, weapons, movement, monster abilities, and other distance-dependent mechanics.
20. Precise canonical ranges and geometry are retained for future battle-map compatibility.
21. The UI presents simplified distance information while allowing exact SRD details to be inspected.

---
# 7G — Components, Spellcasting Focuses & Material Components

**Status:** Approved

## 7G.1 Verbal, Somatic and Material Components

RealmWeaver supports the standard spell-component categories:

* **V — Verbal**
* **S — Somatic**
* **M — Material**

Each Spell Definition must explicitly describe which components are required.

RealmWeaver validates component requirements mechanically.

The AI may explain component-related restrictions but does not determine whether a spell is mechanically castable.

---

## 7G.2 Campaign Builder Component Mode

Spell-component enforcement is configurable during campaign creation.

The player chooses one of the following locked campaign rules:

### Full Components

RealmWeaver enforces:

* Verbal components
* Somatic components
* Ordinary material components
* Costed material components
* Consumed material components
* Applicable focus/component-pouch requirements

### Simplified Components

RealmWeaver ignores:

* Verbal restrictions
* Somatic restrictions
* Routine non-costed material-component bookkeeping

However, **mechanically significant priced or consumed material components remain required**.

Examples include:

* valuable gems;
* costly ritual materials;
* explicitly consumed components;
* other components whose economic or gameplay significance is part of the spell's balance.

The selected component mode is **locked once the campaign begins**, following the same principle as the campaign encumbrance setting.

This prevents changing component enforcement halfway through a campaign in ways that could invalidate existing inventory, economy, or spellcasting decisions.

---

## 7G.3 Verbal Components

When **Full Components** mode is enabled, a spell requiring a Verbal component can only be cast if the character is mechanically capable of producing the required speech.

Relevant restrictions may include:

* magical silence;
* inability to speak;
* explicit effects preventing verbal spellcasting;
* other mechanically defined restrictions.

For example:

```text
Spell requires: V
Caster state: Silenced

Result:
CAST_INVALID
reason = VERBAL_COMPONENT_BLOCKED
```

RealmWeaver determines the failure.

The AI may explain it naturally.

---

## 7G.4 Somatic Components

When **Full Components** mode is enabled, a spell requiring Somatic components requires the caster to be capable of performing the required gestures.

This may interact with:

* occupied hands;
* shields;
* weapons;
* restraints;
* spellcasting focuses;
* class features;
* other equipment or conditions.

RealmWeaver should preserve mechanical legality while avoiding unnecessary player micromanagement.

Where a legal hand/equipment adjustment can reasonably occur under existing action rules, the player should not be forced to manually describe every minor hand movement.

The exact equipment/action implementation is deferred to the relevant architecture/UI milestones.

---

## 7G.5 Ordinary Material Components

Routine, non-costed material ingredients should not be individually tracked.

RealmWeaver should not require inventory bookkeeping for minor spell ingredients where an appropriate focus or component pouch satisfies the requirement.

This avoids low-value inventory clutter while preserving mechanically meaningful component restrictions.

---

## 7G.6 Component Pouch

A valid component pouch satisfies ordinary non-costed Material component requirements where permitted by the underlying rules.

Conceptually:

```text
Component Pouch
status = AVAILABLE
```

is sufficient for routine eligible components.

RealmWeaver should not require inventory entries for every minor ingredient contained within the pouch.

---

## 7G.7 Spellcasting Focuses

Spellcasting-focus eligibility is determined by class, feature, or other validated spellcasting source.

For the V1 spellcasting classes:

* **Wizard — appropriate Arcane Focus**
* **Cleric — appropriate Holy Symbol**

Focus eligibility should not be hardcoded individually into every spell.

Focus items use the shared inventory/equipment system.

---

## 7G.8 Focuses and Material Substitution

A valid spellcasting focus or component pouch may replace ordinary Material components only where permitted by the applicable spellcasting rules.

A focus cannot automatically replace an explicitly priced or otherwise non-substitutable component.

For example:

```text
Required:
Diamond worth 300 GP

Owned:
Arcane Focus
Gold: 2,000 GP

Result:
Requirement NOT satisfied
```

The actual required component must exist.

---

## 7G.9 Costed and Special Components

Mechanically significant spell components must exist as authoritative inventory items or resources.

Relevant information may include:

* item/material type;
* required value;
* quantity;
* whether the component is consumed;
* whether a specific form or quality is required.

RealmWeaver validates actual possession before casting.

---

## 7G.10 Currency Does Not Automatically Purchase Components

Possessing enough money does **not** automatically satisfy a material-component requirement.

For example:

```text
Required:
Diamond worth at least 300 GP

Character:
Gold = 5,000 GP
Diamond = none
```

The spell remains unavailable.

The player must actually obtain the required component through a legitimate campaign action, such as:

* purchasing it from a merchant;
* finding it as loot;
* receiving it as a quest reward;
* crafting or creating it where allowed;
* trading for it;
* stealing it;
* another validated campaign event.

This ensures that spell components meaningfully interact with:

* merchants;
* shops;
* inventory;
* world economy;
* loot;
* exploration;
* quests;
* scarcity.

RealmWeaver must never silently convert currency into a required spell component at casting time.

---

## 7G.11 Consumed Components

Spell Definitions must distinguish between components that are required but retained and components that are consumed by the spell.

Conceptually:

```text
material:
    required = true
    minimum_value_gp = 300
    consumed = true
```

Consumed material components are removed from authoritative inventory only after:

1. the spell has passed validation;
2. all required player choices are complete;
3. the spell cast has been committed.

RealmWeaver follows:

**Validate first → mutate inventory second.**

Invalid or cancelled spell casts consume nothing.

---

## 7G.12 Components as Campaign Gameplay

Valuable components may become meaningful gameplay objectives.

For example, acquiring a rare component may involve:

* locating a specialist merchant;
* negotiating with a temple;
* exploring a dungeon;
* accepting a quest;
* purchasing limited stock;
* recovering a stolen object;
* receiving a reward.

The AI Dungeon Master may create narrative opportunities for component acquisition.

However, the AI cannot simply claim that the character possesses a required component when authoritative inventory says otherwise.

---

## 7G.13 Holy Symbols and Equipment

RealmWeaver should support appropriate holy-symbol usage modes where required by the rules.

Conceptually, an item may include metadata such as:

```text
spellcasting_focus = HOLY_SYMBOL
focus_usage = HELD | WORN | SHIELD
```

This allows Cleric focus validation to interact correctly with equipment state.

The exact item/equipment data model is deferred to architecture.

---

## 7G.14 Losing or Transferring a Focus

Spellcasting focuses are authoritative inventory objects.

If a focus is:

* stolen;
* dropped;
* destroyed;
* transferred;
* otherwise made unavailable,

spellcasting availability must reflect that change.

The AI cannot continue treating the focus as available after RealmWeaver's persistent inventory state says it is gone.

---

## 7G.15 Automatic Component Resolution

Players should not normally need to manually choose a valid focus or component pouch every time they cast a spell.

RealmWeaver should automatically resolve routine component satisfaction where possible.

Conceptually:

**Material component required**
↓
**Valid focus available?**
→ Yes: requirement satisfied
→ No
↓
**Valid component pouch available?**
→ Yes: requirement satisfied
→ No
↓
**Exact required material possessed?**
→ Yes: requirement satisfied
→ No: cast rejected

This automatic resolution keeps spellcasting fast while preserving deterministic validation.

---

## 7G.16 Component Information in the Spells UI

The primary Spells interface should remain compact.

Routine component information may appear within expandable spell details.

For example:

```text
Components: V, S, M
Material: routine component
Satisfied by: Arcane Focus ✓
```

Mechanically significant missing components should be surfaced more prominently.

For example:

```text
Required Material:
Diamond worth at least 300 GP

Inventory:
Not available
```

The exact presentation is deferred to UI/UX design.

---

## 7G.17 AI Behaviour Around Components

When a cast fails due to a component requirement, RealmWeaver returns a structured reason.

For example:

```text
CAST_INVALID
reason = REQUIRED_MATERIAL_MISSING
required = DIAMOND_300_GP
```

The AI may:

* explain why casting failed;
* suggest where such an item might be obtained;
* suggest alternative valid spells.

The AI may not:

* invent the component;
* convert currency into it automatically;
* bypass the requirement;
* directly mutate inventory.

---

## 7G Approved Decisions

1. RealmWeaver supports Verbal, Somatic, and Material spell components.
2. Component requirements come from structured Spell Definitions.
3. Campaign creation includes a locked **Full Components / Simplified Components** setting.
4. Full Components mode enforces normal V/S/M mechanics.
5. Simplified Components mode ignores V/S/routine Material bookkeeping.
6. Significant priced or consumed material components remain required even in Simplified Components mode.
7. Verbal restrictions are mechanically enforced in Full Components mode.
8. Somatic restrictions are mechanically enforced in Full Components mode.
9. RealmWeaver minimizes unnecessary hand/equipment micromanagement where legal automatic handling is possible.
10. Routine non-costed material ingredients are not individually tracked.
11. Component pouches satisfy eligible routine material requirements.
12. Wizard supports appropriate Arcane Focus use.
13. Cleric supports appropriate Holy Symbol use.
14. Focus eligibility comes from class/features rather than individual spell hardcoding.
15. Focuses cannot replace explicitly priced/non-substitutable components.
16. Costed and special components exist as authoritative inventory resources.
17. Having enough currency does not automatically acquire or satisfy a required component.
18. Required material components must actually be obtained and present in inventory.
19. Spell Definitions distinguish consumed and non-consumed components.
20. Consumed materials are removed only after a validated cast commits.
21. Valuable spell components may naturally drive merchant, economy, loot, quest, and exploration gameplay.
22. Holy Symbol equipment modes such as held, worn, or shield-mounted are supported where applicable.
23. Losing or transferring a focus affects actual spellcasting availability.
24. Routine component validation should normally happen automatically.
25. Component information should remain compact in the main Spells UI, with important missing materials clearly surfaced.
26. AI may explain component problems or suggest legitimate solutions but cannot bypass authoritative component requirements.

---

# 7H — Concentration, Duration & Ongoing Magical Effects

**Status:** Approved

## 7H.1 Spell Duration

Every supported spell must contain structured duration information.

RealmWeaver must support duration types including:

* Instantaneous
* Round-based
* Timed
* Concentration
* Until dispelled
* Until a trigger occurs
* Permanent
* Special

Examples of timed duration may include:

* 1 minute
* 10 minutes
* 1 hour
* 8 hours

Duration is authoritative spell data.

The AI may narrate the passage or ending of an effect but does not determine when it mechanically expires.

---

## 7H.2 Instantaneous Effects

Instantaneous spells resolve immediately.

Examples may include:

* damage;
* healing;
* immediate movement or teleportation;
* another direct mechanical state change.

Once the mechanical effect is resolved, no ongoing spell instance remains unless the Spell Definition explicitly creates a persistent effect.

---

## 7H.3 Timed Effects and Campaign Time

Timed spell effects must use RealmWeaver's persistent authoritative campaign time.

For example:

```text
Mage Armor

Started:
Day 12 — 09:00

Duration:
8 hours

Expires:
Day 12 — 17:00
```

RealmWeaver stores the effect and expiry information as persistent campaign state.

Closing and reopening the campaign does not erase or recalculate the duration from real-world elapsed time.

The effect remains active until authoritative campaign time reaches its expiry condition.

---

## 7H.4 Round-Based Duration

Effects measured in combat rounds must be attached to precise combat state rather than approximate AI memory.

RealmWeaver must retain enough information to resolve effects ending at times such as:

* start of a turn;
* end of a turn;
* start of the caster's next turn;
* end of the target's next turn;
* after a specified number of rounds;
* another explicit timing rule.

The exact timing comes from structured effect/spell data.

---

## 7H.5 Concentration

RealmWeaver follows the standard SRD 5.1 / 2014-style concentration system.

A creature may normally maintain **only one concentration effect at a time**.

Concentration continues automatically unless a normal rule or effect threatens or ends it.

RealmWeaver does **not** require a concentration-maintenance roll every combat round by default.

This follows standard tabletop behaviour and avoids making concentration spells significantly weaker than their baseline rules.

---

## 7H.6 Casting Another Concentration Spell

If a character is already concentrating and attempts to cast another concentration spell, RealmWeaver must make the consequence clear before the new spell is committed.

For example:

```text
WARNING

You are currently concentrating on Bless.

Casting Hold Person will end Bless.

[Cast Hold Person]
[Cancel]
```

If the player confirms:

1. the previous concentration ends;
2. its dependent effects end;
3. the new spell is committed;
4. the new concentration state begins.

The AI cannot decide which concentration spell the player values more.

---

## 7H.7 Concentration Checks From Damage

When a concentrating character takes damage, they make a Constitution saving throw according to the standard concentration rule.

The DC is:

**10 or half the damage taken, whichever is higher**

Examples:

```text
Damage taken: 14
Half = 7
Concentration DC = 10
```

```text
Damage taken: 30
Half = 15
Concentration DC = 15
```

RealmWeaver calculates this deterministically.

---

## 7H.8 Separate Damage Events

Each separate source of damage may trigger its own concentration check.

For example:

```text
Damage event 1: 8
Damage event 2: 6
```

normally produces two separate DC 10 concentration checks rather than one check based on 14 combined damage.

RealmWeaver must therefore preserve discrete damage events where required by concentration and other mechanics.

---

## 7H.9 Concentration Dice Control

Concentration checks are Constitution saving throws.

Player-character concentration checks follow the campaign's existing dice-control preference.

### Player-Controlled Dice

RealmWeaver prompts the player.

### System-Controlled Dice

RealmWeaver rolls automatically.

NPC and enemy concentration checks remain system-controlled in V1.

---

## 7H.10 Concentration Ending Conditions

Concentration may end from mechanically valid causes including:

* failing a concentration saving throw;
* becoming incapacitated where applicable;
* death;
* voluntarily ending concentration;
* beginning concentration on another spell;
* another explicit effect or rule that ends concentration.

RealmWeaver determines and records the state change.

The AI does not decide when concentration is lost for narrative convenience.

---

## 7H.11 Voluntarily Ending Concentration

The player may voluntarily end concentration where allowed by the normal rules.

This may be performed through:

* natural-language intent;
* an active-effects control;
* the Spells interface.

For example:

```text
ACTIVE EFFECTS

Bless
Concentration
Remaining: 7 rounds

[End Concentration]
```

RealmWeaver validates and ends the effect.

---

## 7H.12 Concentration UI

Active concentration should be clearly visible without permanently consuming large amounts of campaign space.

The main campaign interface may show a compact indicator on the Spells control.

Opening the relevant panel should display:

* current concentration spell;
* remaining duration where applicable;
* affected targets;
* an option to end concentration;
* additional details where useful.

The exact visual design is deferred to the UI/UX milestone.

---

## 7H.13 Generic Ongoing Effect System

RealmWeaver should use a reusable ongoing-effect concept rather than hardcoding every spell individually.

An effect may conceptually retain:

```text
Effect

source_spell
source_cast
source_character
target
start_time
expiry
concentration_link
modifiers
conditions
triggers
status
```

This allows spells, features, items, and future systems to reuse common effect infrastructure.

The final schema is deferred to architecture.

---

## 7H.14 Multi-Target Ongoing Effects

One spell cast may create effects on multiple targets.

Those target effects should retain a relationship with the same source cast instance.

For example:

```text
Bless Cast Instance
├── Fighter Effect
├── Rogue Effect
└── Cleric Effect
```

If concentration on the source cast ends, all dependent effects must end accordingly.

---

## 7H.15 Effect Expiration

Timed effects expire automatically when authoritative campaign/combat time reaches the effect's expiry condition.

For example:

```text
Mage Armor expires at 17:00

Campaign time reaches 17:00

→ Mage Armor status = EXPIRED
```

The AI does not need to remember or manually trigger the expiration.

---

## 7H.16 Effect Processing and LLM Performance

Routine effect processing must occur through deterministic backend/rules logic rather than separate LLM calls.

Examples include:

* decrementing round duration;
* checking campaign-time expiry;
* ending concentration;
* removing expired modifiers;
* updating condition state.

Meaningful changes may be included in the next normal AI Dungeon Master response where narration is useful.

This supports RealmWeaver's broader responsiveness principle of minimizing unnecessary LLM round trips.

---

## 7H.17 Indefinite and Special Effects

RealmWeaver's effect system must support duration/status concepts such as:

* `TIMED`
* `CONCENTRATION`
* `UNTIL_DISPELLED`
* `UNTIL_TRIGGER`
* `PERMANENT`
* `SPECIAL`

This allows future spell content to be implemented without redesigning the effect system.

---

## 7H.18 Effect End Reason

Ongoing effects should retain how they ended where useful.

Possible statuses/reasons may include:

* Active
* Expired
* Dispelled
* Concentration broken
* Replaced by another concentration effect
* Removed by another effect
* Voluntarily ended
* Source destroyed
* Other rule-defined reason

This supports both debugging and accurate campaign history.

---

## 7H.19 AI Narration of Effect Changes

RealmWeaver determines the authoritative state transition first.

The AI may then narrate meaningful changes.

For example:

```text
SYSTEM EVENT

Mage Armor expired.
Campaign Time: Day 12 — 17:00
```

may produce narrative such as:

> The faint arcane shimmer surrounding you finally fades.

Minor or mechanically obvious expirations do not always require dedicated narration.

The UI may simply update silently where appropriate.

---

## 7H.20 Persistence Across Saves

Concentration and ongoing magical effects must survive campaign save/reload.

Persistent effect state may include:

* source spell/cast;
* caster;
* targets;
* duration;
* expiry;
* concentration relationship;
* active modifiers;
* conditions;
* trigger state;
* current status.

The AI must not reconstruct this information from previous narrative text.

---

## 7H Approved Decisions

1. Spell durations are structured authoritative data.
2. RealmWeaver supports instantaneous, round-based, timed, indefinite, concentration, and special duration models.
3. Timed effects use persistent authoritative campaign time.
4. Round-based effects use exact combat turn/round timing.
5. RealmWeaver uses standard SRD 5.1 / 2014-style concentration rules.
6. Concentration does not require a default maintenance roll every round.
7. A character may normally maintain only one concentration spell at a time.
8. Casting a second concentration spell warns the player before replacing the current concentration.
9. Concentration checks use DC 10 or half the damage taken, whichever is higher.
10. Separate damage events may trigger separate concentration checks.
11. Player concentration checks follow the existing dice-control preference.
12. NPC concentration checks remain automated.
13. Concentration ends only from mechanically valid causes.
14. Players may voluntarily end concentration.
15. Active concentration should be clearly but compactly visible in the campaign UI.
16. RealmWeaver should use a generic reusable ongoing-effect system.
17. Multi-target effects retain a shared source/cast relationship.
18. Ending a source concentration effect ends dependent effects.
19. Effect expiration is driven by authoritative campaign/combat state rather than AI memory.
20. Routine effect processing occurs through backend/rules logic without unnecessary LLM calls.
21. Architecture supports timed, concentration, until-dispelled, until-trigger, permanent, and special effect durations.
22. Effect state may retain why/how the effect ended.
23. AI may narrate meaningful effect changes only after RealmWeaver determines them.
24. Active spell, concentration, and effect state persists across campaign saves and reloads.

---

# 7I — Spell Damage, Healing & Conditions

**Status:** Approved

## 7I.1 Shared Damage System

Spell damage uses RealmWeaver's existing shared damage-resolution system.

Conceptually:

**Determine damage expression**
↓
**Roll damage**
↓
**Apply modifiers**
↓
**Determine damage type(s)**
↓
**Check resistance/immunity/vulnerability**
↓
**Determine final damage**
↓
**Update HP and related state**

Magic must not maintain a separate independent damage engine.

The Spell Definition supplies the spell-specific damage rules.

---

## 7I.2 Damage Types

RealmWeaver supports the standard damage types:

* Acid
* Bludgeoning
* Cold
* Fire
* Force
* Lightning
* Necrotic
* Piercing
* Poison
* Psychic
* Radiant
* Slashing
* Thunder

Damage type is structured mechanical data shared by spells, weapons, monster abilities, environmental hazards, and other game systems.

---

## 7I.3 Resistance, Immunity and Vulnerability

RealmWeaver deterministically resolves interactions with:

* Damage resistance
* Damage immunity
* Damage vulnerability
* Normal damage

For example:

```text
Raw Fire Damage: 28
Target: Fire Resistant
Final Damage: 14
```

The AI receives the resolved mechanical result and may narrate the target's resistance.

The AI does not determine the final damage amount.

---

## 7I.4 Saving Throw Damage Outcomes

Damage or effects following a saving throw are determined by the individual Spell Definition.

RealmWeaver must not assume a universal rule that successful saving throws always halve damage.

Possible spell-defined results include:

* full damage / half damage;
* full damage / no damage;
* full condition / no condition;
* reduced effect;
* another explicit spell-specific outcome.

---

## 7I.5 Multiple Damage Types

A single spell or effect may produce multiple distinct damage components.

RealmWeaver must preserve them separately where mechanically relevant.

For example:

```text
Damage:
10 FIRE
6 RADIANT
```

A target may resist one damage type while taking the other normally.

The system must not collapse multiple types into a single undifferentiated damage number before resistances/immunities are resolved.

---

## 7I.6 Cantrip Damage Scaling

Damage-dealing cantrips follow structured scaling rules.

Where the applicable 2014-style rule scales a cantrip using total character level, RealmWeaver must use character level rather than only spellcasting-class level.

Typical scaling thresholds include:

* Levels 1–4
* Levels 5–10
* Levels 11–16
* Levels 17–20

Because V1 guarantees character levels 1–5, RealmWeaver must support the first scaling breakpoint at character level 5.

Scaling rules belong to structured spell data rather than duplicated spell definitions.

---

## 7I.7 Upcast Damage and Effects

Higher-level casting may increase:

* damage;
* healing;
* number of targets;
* duration;
* another spell-specific effect.

RealmWeaver calculates this using structured upcasting/scaling metadata.

The AI does not infer higher-level effects from memory.

---

## 7I.8 Healing

Spell healing uses authoritative RealmWeaver HP state.

A healing effect:

1. determines the healing expression;
2. rolls/calculates healing;
3. applies applicable modifiers;
4. updates HP;
5. does not normally increase HP above the target's maximum.

For example:

```text
HP before: 7 / 24
Healing: 9
HP after: 16 / 24
```

---

## 7I.9 Healing a Full-Health Target

A valid healing spell should not necessarily be mechanically blocked simply because the target is already at maximum HP.

RealmWeaver should warn the player where the healing would be wasted.

For example:

```text
Kael is already at maximum HP.

Cure Wounds would restore 0 HP.

[Cast Anyway]
[Cancel]
```

The player retains the decision where the underlying spell remains otherwise valid.

---

## 7I.10 Healing From 0 HP

When a creature at 0 HP receives valid healing and returns to positive HP, RealmWeaver deterministically applies the appropriate state transition.

For example:

```text
HP:
0 → 6

State:
UNCONSCIOUS → removed where applicable
```

This must integrate with RealmWeaver's existing/future death, dying, and unconsciousness rules.

---

## 7I.11 Temporary Hit Points

Temporary Hit Points are stored separately from ordinary HP.

For example:

```text
HP: 18 / 24
Temporary HP: 7
```

They must not be represented as HP above maximum.

Applicable damage normally interacts with Temporary HP before reducing ordinary HP according to the shared HP/damage rules.

---

## 7I.12 Temporary HP Replacement

Temporary HP does not normally stack by simple addition.

If a character already has Temporary HP and gains another source, RealmWeaver applies the appropriate replacement/choice behaviour under the applicable rules.

For example:

```text
Existing Temp HP: 8
New Temp HP: 5

Result:
Do not automatically become 13
```

The exact handling follows the standard rule or any explicit feature exception.

---

## 7I.13 Spell-Caused Conditions

Conditions applied by spells use RealmWeaver's shared condition system.

Magic must not create a separate spell-only condition engine.

A spell effect may conceptually specify:

```text
condition = PARALYZED
duration = ...
save_recurrence = ...
concentration_dependency = ...
```

Shared conditions can therefore interact consistently with:

* combat;
* attacks;
* saves;
* movement;
* spellcasting;
* monster abilities;
* environmental effects.

---

## 7I.14 Condition Source Tracking

Conditions and other ongoing spell effects should retain their source.

Conceptually:

```text
ConditionEffect

condition = PARALYZED
source_spell = Hold Person
source_cast = ...
source_caster = ...
target = ...
start_time = ...
expiry = ...
concentration_link = ...
```

This allows RealmWeaver to correctly remove dependent conditions if their source ends.

---

## 7I.15 Repeated Saving Throws

Some ongoing spell effects permit repeated saving throws.

The Spell Definition/effect data must describe:

* required saving throw;
* timing;
* DC source;
* success outcome;
* failure outcome;
* whether the effect ends on success.

Examples of timing may include:

* start of target turn;
* end of target turn;
* when damage is taken;
* another defined trigger.

RealmWeaver detects and resolves these triggers deterministically.

The AI must not be responsible for remembering when another save is due.

---

## 7I.16 Player and NPC Repeated Saves

Repeated saving throws made by player characters follow the campaign's normal dice-control preference.

When player-controlled dice are enabled, RealmWeaver prompts the player.

NPC and enemy repeated saving throws remain system-controlled in V1.

---

## 7I.17 Damage-Over-Time Effects

RealmWeaver's ongoing-effect system must support triggered recurring damage.

Conceptually:

```text
trigger = START_OF_TURN
damage = 2d6 FIRE
duration = ...
source = ...
```

When the trigger occurs, RealmWeaver resolves the damage mechanically.

No additional AI call is required to determine whether the damage occurs.

---

## 7I.18 Healing-Over-Time and Generic Triggered Effects

The same generic effect infrastructure should support recurring healing and other repeated mechanical operations.

RealmWeaver should prefer a reusable triggered-effect model over building completely separate systems for:

* damage over time;
* healing over time;
* repeated saves;
* recurring bonuses;
* other timed operations.

The exact architecture is deferred to later milestones.

---

## 7I.19 Persistent Area Effects

Some spells may create areas that persist after initial casting.

The ongoing-effect and positioning systems must support effects triggered when a creature:

* enters an affected area;
* starts a turn there;
* ends a turn there;
* remains there for a defined interval;
* satisfies another spell-defined trigger.

V1 may resolve such areas through the approved deterministic zone/cluster/relative-position model.

Future battle-map support should be able to use the same canonical spell geometry.

---

## 7I.20 Rolled Amount and Actual State Change

RealmWeaver should distinguish between the raw rolled/calculated result and the actual state change.

For healing:

```text
HP before: 21 / 24
Healing rolled: 9
Actual healing applied: 3
HP after: 24 / 24
```

For damage:

```text
Target HP before: 4
Damage dealt: 17
Target HP after: 0
```

Retaining both values may matter for:

* death/dying mechanics;
* triggers;
* achievements;
* combat logs;
* debugging;
* future features.

---

## 7I.21 Compact Mechanical Result UI

Spell damage, healing, and conditions should be mechanically inspectable without cluttering the narrative.

For example:

```text
Sacred Flame
Failed DEX Save
8 Radiant Damage
HP: 17 → 9

[Details]
```

or:

```text
Cure Wounds
11 Healing
HP: 4 → 15

[Details]
```

Expandable details may show:

* dice;
* modifiers;
* save/DC;
* resistance;
* damage type;
* target state changes.

AI narration remains separate and story-focused.

---

## 7I.22 AI Receives Resolved Outcomes

The AI Dungeon Master should receive structured mechanical results.

For example:

```text
Spell: Hold Person
Target: Bandit Captain

Saving Throw:
Failure

Condition:
Paralyzed

Duration:
Concentration, up to 1 minute

State:
Condition active
```

The AI then narrates the consequence.

The AI must not be asked to determine independently whether the spell succeeds or what mechanical state should result.

---

## 7I.23 Atomic Spell State Changes

A resolved spell may cause multiple connected state changes.

For example, one area spell may:

* damage several creatures;
* reduce HP;
* defeat one creature;
* trigger concentration checks;
* break another concentration effect;
* apply or remove conditions;
* create world/combat events.

RealmWeaver should eventually process the committed spell resolution as a coherent mechanical transaction so that partial state updates do not leave campaign state inconsistent.

The exact database/transaction implementation is deferred to architecture.

---

## 7I Approved Decisions

1. Spell damage uses RealmWeaver's existing shared damage engine.
2. RealmWeaver supports the standard damage types.
3. Resistance, immunity, and vulnerability are resolved deterministically.
4. Saving-throw outcomes come from individual Spell Definitions rather than a universal half-damage rule.
5. A spell may contain multiple separately resolved damage types.
6. Damage-dealing cantrips scale using structured rules, including the V1-required level-5 breakpoint.
7. Upcast damage and other higher-level effects use structured scaling rules.
8. Healing uses authoritative HP state and does not normally exceed maximum HP.
9. RealmWeaver warns rather than automatically blocking otherwise valid healing on a full-health target.
10. Healing from 0 HP applies the appropriate deterministic state transition.
11. Temporary HP is separate from ordinary HP.
12. Temporary HP follows normal non-stacking/replacement rules unless an explicit exception exists.
13. Spell-caused conditions reuse RealmWeaver's shared condition system.
14. Conditions retain their source spell/cast/effect relationship.
15. Repeated saving throws are represented as deterministic effect triggers.
16. Player repeated saves follow the existing dice-control preference.
17. NPC/enemy repeated saves remain automated in V1.
18. Ongoing damage and healing use generic triggered-effect infrastructure.
19. Persistent area effects integrate with RealmWeaver's deterministic positioning/zones.
20. RealmWeaver retains rolled/calculated amounts separately from actual HP state changes where useful.
21. Mechanical spell outcomes should be compact and expandable in the campaign UI.
22. AI narrates already-resolved mechanical outcomes.
23. Multi-target and multi-effect spell resolutions should eventually be processed atomically by the backend.

---

# Cross-System Architecture Notes Identified During Group 7

The following requirements were identified while designing Magic. They are recorded here so they are not lost, but their complete implementation design belongs to later RealmWeaver milestones.

## Persistent Authoritative Campaign Time

RealmWeaver should maintain an authoritative in-world campaign clock as persistent campaign state.

Mechanically defined durations are determined by RealmWeaver.

For narrative activities without mechanically defined durations, the AI may propose a reasonable duration, but RealmWeaver records and advances authoritative campaign time.

Real-world time passing while the campaign is closed does not automatically advance in-world campaign time.

Persistent campaign time will eventually support systems including:

* spell durations;
* rests;
* travel;
* timed quests;
* NPC schedules;
* world events;
* effect expiration;
* environmental events;
* campaign history.

The complete time-progression and event architecture is deferred to the appropriate later milestone.

---

## AI Responsiveness and LLM Round Trips

Deterministic mechanical validation should execute through RealmWeaver backend/rules logic wherever possible rather than requiring LLM reasoning.

Examples include:

* spell eligibility;
* spell slots;
* HP;
* AC;
* dice calculations;
* range;
* conditions;
* inventory;
* effect expiration;
* resource consumption.

RealmWeaver should minimize LLM round trips per player action.

Where practical, the target architecture should aim for approximately **one main AI Dungeon Master generation call per completed player action**, with additional interpretation/model calls used only where genuinely necessary.

Structured UI actions may bypass AI intent interpretation entirely and enter the deterministic rules pipeline directly.

The complete AI orchestration, context-management, latency, streaming, and model-selection strategy is deferred to the architecture/AI milestones.

---

## Group 7 Progress

| Section | Topic                                                  | Status                               |
| ------- | ------------------------------------------------------ | ------------------------------------ |
| 7A      | Spellcasting Core & Spell Data Model                   | **Approved**                         |
| 7B      | Spellcasters, Known Spells & Prepared Spells           | **Approved**                         |
| 7C      | Spell Slots & Resource Consumption                     | **Approved**                         |
| 7D      | Casting a Spell & Action Economy                       | **Approved** |
| 7E      | Spell Attacks, Saving Throws & Spell Save DC           | **Approved** |
| 7F      | Range, Targets, Areas & Distance Bands                 | **Approved** |
| 7G      | Components, Spellcasting Focuses & Material Components | **Approved** |
| 7H      | Concentration, Duration & Ongoing Effects              | **Approved** |
| 7I      | Spell Damage, Healing & Conditions                     | **Approved** |
| 7J      | Cantrips, Ritual Casting & Special Casting             | Not started                          |
| 7K      | Scrolls, Magical Items & Identification                | Not started                          |
| 7L      | AI Spellcasting, Validation & State Integrity          | Not started                          |

**Next design subsection:** 7J — Cantrips, Ritual Casting & Special Casting
