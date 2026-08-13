# RealmWeaver — Combat

**Rule Group:** 4
**Status:** APPROVED
**Milestone:** M2 — Technical Design & Architecture
**Section:** M2.1 — V1 Game Rules Specification
**Last Reviewed:** 13 August 2026

---

# 1. Purpose

This document defines RealmWeaver V1 combat rules.

It covers:

* Combat initiation
* Initiative
* Turn structure
* Action economy
* Reactions
* Distance-band positioning
* Movement
* Opportunity Attacks
* Attack rolls
* Armour Class
* Damage
* Critical Hits
* Healing
* Unconsciousness
* Death
* Enemy combat behaviour
* Encounter resolution
* Rewards
* Loot
* Persistent combat consequences
* Encumbrance campaign setting
* Reusable content-pool direction

The governing principle is:

> **AI selects or interprets intent. The rules engine validates and resolves mechanics.**

The AI may interpret what the player wants to do and narrate the result.

The deterministic game systems remain authoritative over mechanical legality and state.

---

# 2. Combat Start & Initiative

## 2.1 Starting Combat

### Status: APPROVED

Structured combat begins when hostile interaction requires turn-based mechanical resolution.

The AI may identify that combat should begin.

The rules engine then:

1. Creates the encounter.
2. Loads combat participants.
3. Establishes participant state.
4. Rolls or requests Initiative.
5. Creates Initiative order.
6. Begins Round 1.
7. Tracks subsequent turns and rounds.

The AI cannot arbitrarily decide who acts first.

---

## 2.2 Initiative

### Status: APPROVED

Initiative uses:

**d20 + Dexterity Modifier**

Example:

```text
Player      18
Bandit A    14
Bandit B     7
```

Turn order remains consistent between rounds unless a supported mechanic explicitly changes it.

---

## 2.3 Initiative Ties

### Status: APPROVED

For a Player vs Enemy tie:

> **The player acts first.**

For Enemy vs Enemy ties:

1. Higher Dexterity acts first.
2. If Dexterity is also tied, the system resolves the order randomly.

The rules engine remains authoritative over Initiative order.

---

# 3. Turn Structure & Action Economy

## 3.1 Standard Turn

### Status: APPROVED

A normal combat turn may provide:

* Movement
* 1 Action
* 1 Bonus Action
* 1 Reaction per round
* Reasonable free interaction

The engine tracks what remains available.

---

## 3.2 Actions

### Status: APPROVED

Supported Actions may include:

* Attack
* Dash
* Disengage
* Dodge
* Help
* Hide
* Cast a spell
* Use an item
* Use a supported feature
* Other valid contextual Actions

The exact action may depend on the character's available features and current state.

---

## 3.3 Bonus Actions

### Status: APPROVED

A Bonus Action may only be used when a supported mechanic explicitly allows one.

Examples include:

* Class feature
* Spell
* Item
* Species feature
* Other supported ability

An unused Bonus Action does not automatically become another normal Action.

---

## 3.4 Free Interaction

### Status: APPROVED

Minor interactions may normally occur without consuming the character's main Action.

Examples:

* Drawing a weapon
* Dropping an item
* Brief speech
* Opening an uncomplicated unlocked door

More substantial interactions may require an Action depending on context.

The AI may propose whether an interaction is minor or substantial.

The rules system validates the result where required.

---

# 4. Reactions

## 4.1 Reaction Model

### Status: APPROVED — LIMITED V1 SUPPORT

V1 supports a Reaction system.

A creature normally has one Reaction available according to supported rules.

Conceptually:

```text
reaction_available = true
```

When used:

```text
reaction_available = false
```

The Reaction becomes available again according to the relevant combat timing rules.

---

## 4.2 V1 Reaction Scope

### Status: APPROVED

V1 does not need to implement every possible Reaction feature.

Minimum Reaction infrastructure must support mechanics such as:

* Opportunity Attacks
* Uncanny Dodge
* Future spell reactions
* Future class/species feature reactions

---

## 4.3 Reaction Authority

The rules engine determines:

* Whether a Reaction is available
* Whether the trigger occurred
* Whether the creature is eligible
* Whether the target is valid
* Whether another rule prevents the Reaction

The AI cannot create a Reaction opportunity that did not mechanically occur.

---

# 5. Combat Positioning

## 5.1 Abstract Distance Bands

### Status: APPROVED

RealmWeaver V1 does not use precise grid-based battlefield positioning.

It uses abstract distance bands:

| Distance Band | Approximate Meaning    |
| ------------- | ---------------------- |
| Engaged       | Approximately 0–5 ft   |
| Near          | Approximately 5–30 ft  |
| Far           | Approximately 30–60 ft |
| Distant       | Approximately 60+ ft   |

The combat engine stores relevant positional relationships.

---

## 5.2 Mechanical Position Authority

### Status: APPROVED

The AI cannot independently contradict authoritative combat positioning.

Example:

```text
Player → Near → Goblin
```

A normal melee attack requiring Engaged range cannot simply succeed because the AI narrated the player as already beside the Goblin.

The engine must validate the range first.

---

## 5.3 Speed

### Status: APPROVED

Character Speed remains mechanically relevant.

Speed helps determine whether a creature can transition between distance bands during a turn.

Species, class features, spells, conditions and other supported mechanics may modify Speed.

---

## 5.4 Future Battle Maps

### Status: APPROVED FUTURE DIRECTION

Distance bands are a V1 simplification.

Future versions may replace or extend this system with:

* Battle maps
* Exact coordinates
* Grid-based movement
* More detailed positioning

V1 architecture should avoid unnecessary coupling that would make a later transition prohibitively difficult.

---

# 6. Movement & Movement Actions

## 6.1 Normal Movement

### Status: APPROVED

Characters may move according to:

* Available Speed
* Current distance relationship
* Terrain/context
* Conditions
* Supported movement modifiers

The engine determines whether the requested movement is mechanically possible.

---

## 6.2 Dash

### Status: APPROVED

Dash uses an Action to increase available movement according to the supported movement rules.

The additional movement still obeys:

* Terrain
* Speed modifications
* Opportunity Attack rules
* Other restrictions

---

## 6.3 Disengage

### Status: APPROVED

Disengage allows a creature to leave an Engaged relationship without triggering a normal Opportunity Attack from eligible opponents.

---

## 6.4 Dodge

### Status: APPROVED

Dodge allows a creature to focus on defence according to the supported rule.

The exact resulting attack/save effects are resolved mechanically.

---

## 6.5 Help

### Status: APPROVED

Help allows a character to assist another eligible creature.

Its precise use depends on:

* Context
* Range
* Target
* Supported mechanic

Help may become more important once NPC allies or companions are expanded.

---

## 6.6 Hide

### Status: APPROVED

Hide may require a check such as:

**Stealth vs relevant Perception**

or another supported detection mechanic.

The rules engine resolves whether hiding succeeds.

The AI does not simply decide that a character is hidden.

---

# 7. Opportunity Attacks

## 7.1 Basic Rule

### Status: APPROVED

Basic Opportunity Attacks are supported in V1.

If a creature leaves an Engaged relationship without using a mechanic such as Disengage, an eligible opponent may use its Reaction for an Opportunity Attack.

---

## 7.2 Opportunity Attack Validation

The engine determines:

* Whether the creatures were Engaged
* Whether the movement qualifies
* Whether the attacker has a Reaction available
* Whether the attacker is capable of making the attack
* Whether another mechanic prevents the Opportunity Attack

The AI cannot grant an Opportunity Attack when these conditions are not met.

---

# 8. Complex Natural-Language Combat Actions

## 8.1 Free-Text Combat Intent

### Status: APPROVED

Players may describe complex actions naturally.

Example:

> I run past the goblin, jump over the table, grab the wizard's staff and attack him.

RealmWeaver should not require the player to manually break every complex action into UI buttons.

---

## 8.2 Resolution Pipeline

### Status: APPROVED

The intended pipeline is:

```text
Player Intent
    ↓
AI Interpretation
    ↓
Structured Proposed Actions
    ↓
Rules Validation
    ↓
Mechanical Resolution
    ↓
AI Narration
```

A complex statement may be decomposed into:

* Movement
* Reaction triggers
* Ability Checks
* Object interactions
* Actions
* Bonus Actions
* Attacks
* Resource use
* Other mechanical components

The rules system determines which components are legal and how they resolve.

---

## 8.3 Partial Success

A complex request does not have to resolve as:

* Completely succeeds
* Completely fails

Individual components may resolve independently.

Example:

The player may successfully:

* Move past one enemy
* Fail an Athletics check
* Trigger an Opportunity Attack
* Lose the remaining intended attack opportunity

The AI narrates the mechanically resolved sequence.

---

# 9. Attacks, Armour Class & Damage

## 9.1 Attack Roll

### Status: APPROVED

A standard attack uses:

**d20 + Relevant Ability Modifier + Proficiency Bonus when applicable**

Examples:

* Many melee weapons use Strength.
* Many ranged weapons use Dexterity.
* Finesse weapons may permit an eligible alternative.
* Spell attacks use the relevant Spellcasting Ability.

---

## 9.2 Hit Resolution

### Status: APPROVED

An attack hits when:

**Attack Total ≥ Target AC**

An attack misses when:

**Attack Total < Target AC**

The rules engine determines hit or miss.

---

## 9.3 Weapon Proficiency

### Status: APPROVED

Characters are not automatically proficient with every weapon.

If a character uses a weapon without proficiency:

* The attack may still be attempted where otherwise legal.
* Proficiency Bonus is not added.

Weapon/class proficiency data determines whether proficiency applies.

---

## 9.4 Hidden Enemy AC

### Status: APPROVED

Enemy Armour Class is normally hidden from the player.

The player may see:

* Raw d20 result
* Attack modifiers
* Final attack total
* Hit or miss

The exact enemy AC is not normally displayed.

Example:

```text
Attack Roll

d20: 14
Modifier: +5
Total: 19

Result: HIT
```

This preserves some uncertainty while keeping the player's own mechanics transparent.

---

## 9.5 Damage Resolution

### Status: APPROVED

A successful hit resolves damage separately from the attack roll.

Damage may include:

* One or more damage dice
* Applicable static modifier
* Additional eligible effects

Example:

**Longsword: 1d8 + STR Modifier**

The engine applies the resulting damage to authoritative HP state.

---

## 9.6 Damage Types

### Status: APPROVED

RealmWeaver stores damage type as structured data.

Supported types may include:

* Bludgeoning
* Piercing
* Slashing
* Acid
* Cold
* Fire
* Force
* Lightning
* Necrotic
* Poison
* Psychic
* Radiant
* Thunder

V1 only needs to implement the subset required by supported content.

---

## 9.7 Resistance

### Status: APPROVED

Resistance reduces eligible incoming damage according to the supported rule.

The rules engine determines whether Resistance applies.

---

## 9.8 Vulnerability

### Status: APPROVED

Vulnerability increases eligible incoming damage according to the supported rule.

---

## 9.9 Immunity

### Status: APPROVED

Immunity prevents eligible damage entirely.

---

## 9.10 Attack Categories

### Status: APPROVED

The system distinguishes attack categories such as:

* Melee
* Ranged
* Spell Attack
* Special Ability

Attack category may affect:

* Range
* Ability Modifier
* Proficiency
* Advantage/Disadvantage
* Feature eligibility

---

## 9.11 Ranged Attacks While Engaged

### Status: APPROVED

Making a ranged attack while Engaged with an eligible hostile creature may impose Disadvantage unless another supported mechanic overrides it.

---

## 9.12 Multiple Attacks

### Status: APPROVED

Multiple attacks during one Action are supported only when an explicit feature grants them.

Example:

```text
Attack Action
├── Attack 1
└── Attack 2
```

An unused Bonus Action does not automatically grant another Attack Action.

---

# 10. Critical Hits & Critical Misses

## 10.1 Natural 20 Attack

### Status: APPROVED

A Natural 20 on an attack roll:

* Automatically hits.
* Becomes a Critical Hit.

The natural result refers to the raw d20 before modifiers.

---

## 10.2 Critical Damage

### Status: APPROVED

Eligible attack damage dice are doubled.

Static modifiers are added once.

Example:

Normal:

**1d8 + 3**

Critical:

**2d8 + 3**

The engine determines which dice are eligible to be doubled.

---

## 10.3 Natural 1 Attack

### Status: APPROVED

A Natural 1 on an attack roll automatically misses.

Modifiers cannot convert the attack into a hit.

---

## 10.4 No Critical Fumble Penalty in V1

### Status: APPROVED

A Natural 1 does not automatically cause additional penalties such as:

* Dropping a weapon
* Breaking equipment
* Falling
* Hitting an ally
* Losing additional actions

The AI may narrate the failure dramatically but cannot invent extra mechanical punishment.

---

## 10.5 Critical Narration

### Status: APPROVED

The AI may narrate Natural 20s and Natural 1s more dramatically.

Narrative flavour does not create unsupported mechanical effects.

---

## 10.6 Future Critical Expansion

### Status: APPROVED FUTURE DIRECTION

Future RealmWeaver versions may support configurable:

* Enhanced Natural 20 bonuses
* Natural 1 penalties
* Critical-fumble mechanics

These are outside initial V1.

---

# 11. Hit Points, Healing, Unconsciousness & Death

## 11.1 Temporary HP

### Status: APPROVED

Incoming damage is applied to Temporary HP before Current HP.

Example:

```text
Current HP: 24
Temporary HP: 5
Damage: 8

Temporary HP: 0
Current HP: 21
```

---

## 11.2 Healing

### Status: APPROVED

Healing restores Current HP.

Healing normally cannot raise Current HP above Maximum HP.

Possible healing sources include:

* Spells
* Potions
* Class Features
* Species Features
* Rest
* Other supported mechanics

The rules engine applies healing.

---

## 11.3 Reaching 0 HP

### Status: APPROVED

When a character reaches:

**0 HP**

they become Unconscious unless another supported mechanic produces a different result.

---

## 11.4 Death Saving Throws

### Status: APPROVED

An unconscious character at 0 HP makes Death Saving Throws.

Baseline:

* 10–20 → Success
* 1–9 → Failure

Three successes:

> **Stabilised**

Three failures:

> **Dead**

---

## 11.5 Natural 20 Death Save

### Status: APPROVED

A Natural 20 on a Death Saving Throw restores:

**1 HP**

and allows the character to regain consciousness according to supported rules.

---

## 11.6 Natural 1 Death Save

### Status: APPROVED

A Natural 1 on a Death Saving Throw counts as:

**Two Death Save Failures**

---

## 11.7 Damage While at 0 HP

### Status: APPROVED

Taking damage while at 0 HP may cause Death Save Failures according to supported rules.

The engine determines how many failures occur.

---

## 11.8 Massive Damage

### Status: APPROVED

RealmWeaver supports instant death from sufficiently severe excess damage.

Conceptually:

```text
Current HP = 5
Maximum HP = 20
Incoming Damage = 27

5 damage reduces Current HP to 0.
22 damage remains.

Remaining Damage >= Maximum HP

→ Instant Death
```

---

## 11.9 Stabilisation Is Not Automatic Rescue

### Status: APPROVED

Stabilisation does not automatically:

* Teleport the player to safety
* Fully heal the character
* Spawn a rescuer
* Remove environmental danger

The world state determines what happens next.

Possible outcomes include:

* Rescue
* Capture
* Arrest
* Robbery
* Remaining unconscious
* Continued danger
* Later natural recovery

---

## 11.10 Defeat Without Death

### Status: APPROVED

Not every enemy intends to kill the player.

Possible defeat outcomes include:

* Capture
* Robbery
* Arrest
* Interrogation
* Humiliation
* Abandonment
* Escape
* Other contextually appropriate outcomes

Enemy motivation and encounter objective influence what happens.

---

## 11.11 Permanent Death

### Status: APPROVED

If death is mechanically established, the AI cannot simply narrate that the character survived.

Any resurrection, revival or reversal must come from a legitimate supported mechanic.

---

## 11.12 Campaign Continuation After Death

### Status: APPROVED DIRECTION

Character death does not necessarily delete the campaign world.

Architecture should allow future continuation in the same world with another character.

The previous character's:

* Actions
* Relationships
* Quest outcomes
* World changes
* History

may remain part of the campaign.

Full multi-character continuation does not need to be completed in the earliest V1.

---

# 12. Enemy Combat AI

## 12.1 Hybrid Tactical Model

### Status: APPROVED

V1 uses a hybrid enemy-control model:

* Simple enemies → primarily deterministic tactical behaviour
* Intelligent, important or complex enemies → AI-assisted tactical reasoning

This balances:

* Reliability
* Cost
* Latency
* Tactical variety

---

## 12.2 Future Direction

### Status: APPROVED FUTURE DIRECTION

Future versions may extend AI-assisted tactical reasoning to most or all enemies and NPCs.

The deterministic rules engine remains mechanically authoritative.

---

## 12.3 Tactical Profiles

### Status: APPROVED

Enemies may have lightweight tactical profiles such as:

* Aggressive
* Defensive
* Cowardly
* Ranged
* Ambusher
* Spellcaster
* Protector
* Leader

These profiles guide behaviour but do not override mechanics.

---

## 12.4 Enemy Objectives

### Status: APPROVED

Enemies may have objectives such as:

* Kill
* Capture
* Escape
* Defend
* Protect
* Delay
* Steal
* Hold a location
* Complete another encounter-specific objective

Enemy behaviour is not always:

> Reduce Player HP to zero.

---

## 12.5 Tactical Inputs

Enemy tactical reasoning may consider relevant information such as:

* Current HP
* Conditions
* Available Actions
* Bonus Actions
* Reactions
* Abilities
* Resources
* Distance bands
* Observable player state
* Allies
* Enemies
* Encounter objective
* Tactical profile

---

## 12.6 Mechanical Validation

### Status: APPROVED

Every enemy action must be validated.

Possible validation includes:

* Correct turn
* Action available
* Bonus Action available
* Reaction available
* Valid target
* Range
* Movement
* Resource availability
* Feature availability
* Condition restrictions
* Other relevant rules

Invalid AI-selected actions are rejected.

---

## 12.7 Deterministic Fallback

### Status: APPROVED

If AI tactical generation:

* Fails
* Times out
* Produces invalid output
* Produces unusable structured data

RealmWeaver uses deterministic fallback behaviour.

Possible fallback priority:

1. Make a legal attack if possible.
2. Move toward an appropriate target.
3. Use a defensive or safe Action if no valid attack exists.

AI failure must not freeze combat.

---

## 12.8 Enemy Knowledge

### Status: APPROVED

Enemies may only use information they reasonably possess.

They do not automatically know:

* Hidden player resources
* Exact unseen statistics
* Secret weaknesses
* Hidden inventory
* Unknown Species traits
* Other concealed information

unless that information was legitimately learned.

---

## 12.9 Morale

### Status: APPROVED

V1 supports basic morale.

Enemies may:

* Flee
* Surrender
* Retreat
* Change tactics
* Continue fighting

Potential factors include:

* Low HP
* Leader death
* Ally losses
* Failed objectives
* Intelligence
* Personality
* Fearlessness
* Tactical situation

Not every enemy behaves identically.

---

## 12.10 Difficulty and Tactical Quality

### Status: APPROVED

Campaign difficulty may influence tactical competence.

Conceptually:

### Easy

* Simpler tactics
* Less optimisation
* More forgiving enemy decisions

### Normal

* Sensible tactics
* Appropriate use of abilities

### Hard

* Better coordination
* Stronger tactical choices
* More efficient use of abilities

Difficulty does not permit enemies to:

* Manipulate dice
* Ignore mechanics
* Read hidden player state
* Gain impossible knowledge
* Arbitrarily increase statistics outside supported rules

---

# 13. Combat End & Encounter Resolution

## 13.1 Ending Combat

### Status: APPROVED

Combat ends when structured turn-by-turn resolution is no longer required.

Combat does not require killing every enemy.

---

## 13.2 Valid Combat End Conditions

### Status: APPROVED

Combat may end because:

* All active hostile enemies are defeated.
* Opponents surrender.
* Player surrenders.
* Opponents flee successfully.
* Player escapes successfully.
* Encounter objective is completed.
* Hostility otherwise ends.
* Structured turn resolution is no longer necessary.

---

## 13.3 Pursuit

### Status: APPROVED

A fleeing opponent does not automatically end the encounter if the player chooses and is mechanically able to pursue.

Pursuit may continue in:

* Combat
* Chase-style resolution
* Narrative resolution

depending on context and future supported systems.

---

## 13.4 Encounter Objectives

### Status: APPROVED

Encounters may have objectives beyond defeating enemies.

Examples:

* Survive for a number of rounds.
* Protect an NPC.
* Capture someone alive.
* Escape.
* Hold a location.
* Retrieve an item.
* Delay opponents.
* Defeat one important target.
* Prevent an enemy action.

---

# 14. Combat Finalisation

## 14.1 Authoritative Finalisation

### Status: APPROVED

When combat ends, RealmWeaver should resolve and persist the mechanical aftermath before normal narrative play resumes.

The system should:

1. Finalise participant HP.
2. Finalise conditions/status.
3. Record deaths.
4. Record unconscious/stabilised characters.
5. Record surrender.
6. Record capture.
7. Record escape.
8. Resolve encounter objective.
9. Determine eligible rewards.
10. Update quests.
11. Update progression.
12. Apply NPC/world consequences.
13. Save authoritative state.
14. Return to normal narrative play.

The AI narrates the aftermath after state finalisation.

---

## 14.2 Persistent NPC State

### Status: APPROVED

Combat outcomes persist.

Example:

```text
NPC: Captain Daren
status: Alive
relationship: Hostile
last_event: Escaped combat with player
```

or:

```text
NPC: Captain Daren
status: Dead
```

Future AI narration must respect the stored state.

---

# 15. Progression Rewards

## 15.1 XP Mode

### Status: APPROVED

XP campaigns may award progression for overcoming:

* Encounters
* Encounter objectives
* Alternative resolutions

XP is not awarded solely for killing creatures.

Detailed XP rules are defined in:

`05_CLASSES_AND_PROGRESSION.md`

---

## 15.2 Milestone Mode

### Status: APPROVED

Milestone campaigns do not automatically receive combat XP.

Encounter outcomes may instead contribute toward:

* Milestone conditions
* Quest progression
* Story progression
* Other persisted accomplishments

---

# 16. Loot

## 16.1 Contextual Loot

### Status: APPROVED

Loot must be plausible for the creature, NPC and context.

Relevant factors may include:

* Creature/NPC archetype
* Equipped items
* Persistent inventory
* Wealth
* Encounter difficulty
* Location
* Campaign progression
* Story context
* Occupation
* Faction
* Current circumstances

Level or encounter difficulty alone must not determine loot.

---

## 16.2 Persistent NPC Possessions

### Status: APPROVED

Important NPC possessions should come from persistent state where practical.

If an NPC has already been established as carrying:

* A sword
* A letter
* 12 silver pieces

RealmWeaver should not generate a completely different inventory after that NPC dies.

---

## 16.3 Crime and Robbery Cases

### Status: APPROVED

If the player:

* Robs
* Attacks
* Kills
* Searches

an ordinary NPC, available possessions should derive from:

* Actual inventory
* Equipped items
* Wealth
* Stored possessions
* Contextually validated generated possessions

The AI cannot invent arbitrary valuable treasure simply because the NPC was defeated.

---

## 16.4 Available Loot vs Player Inventory

### Status: APPROVED

Encounter loot and player inventory are separate.

An item is not automatically transferred merely because it is available.

Example:

```text
Available Loot:
- Shortsword
- Leather Armour
- 12 Silver

Player Takes:
- Shortsword
- 12 Silver
```

Only selected items enter the player inventory.

---

## 16.5 Hidden Loot

### Status: APPROVED

Some loot may require discovery.

Examples:

* Hidden key
* Secret document
* Concealed pouch
* Hidden compartment

Relevant checks may include:

* Investigation
* Perception
* Search action
* Other supported mechanics

The engine resolves whether the hidden item is discovered.

---

# 17. Quest & World Consequences

## 17.1 Quest Updates

### Status: APPROVED

Combat outcomes may affect quests.

Possible changes include:

* Progress
* Completion
* Failure
* Objective replacement
* New objective
* Removed objective

Example:

```text
Quest:
Capture the Bandit Leader Alive

Bandit Leader:
Dead

→ Quest Failed
```

The AI cannot keep an impossible objective active purely for narrative convenience.

---

## 17.2 World Consequences

### Status: APPROVED DIRECTION

Combat may affect:

* NPC relationships
* Faction hostility
* Reputation
* Story progression
* Location access
* Information availability
* Political state
* Persistent world state

Detailed world-state architecture will be defined later in M2.

---

# 18. Reward Types

## 18.1 Supported Reward Categories

### Status: APPROVED

Rewards may include:

* XP
* Currency
* Equipment
* Consumables
* Information
* Quest progression
* Relationships
* Reputation
* Location access
* Story progress
* Other valid campaign state changes

Not every reward is physical loot.

---

## 18.2 Reward Authority

### Status: APPROVED

The AI may propose narratively appropriate rewards.

Mechanically significant rewards must be validated by the appropriate system.

The AI cannot arbitrarily create:

* Excessive treasure
* Unsupported magic items
* Unjustified XP
* Free level-ups
* Impossible quest rewards

The governing rule remains:

> **AI proposes. RealmWeaver validates.**

---

# 19. Encumbrance Campaign Setting

## 19.1 Campaign Choice

### Status: APPROVED

During campaign creation, the player chooses:

* Encumbrance Enabled
* Encumbrance Disabled

---

## 19.2 Immutable Setting

### Status: APPROVED

Once campaign creation is confirmed and gameplay begins, this setting cannot normally be changed for that campaign.

---

## 19.3 Encumbrance Disabled

### Status: APPROVED

When Encumbrance is disabled:

* Inventory is still tracked.
* Equipment is still tracked.
* Item ownership is still tracked.
* Currency is still tracked.
* Item state remains authoritative.
* Carrying-capacity restrictions are ignored.

---

## 19.4 Encumbrance Enabled

### Status: APPROVED DIRECTION

When enabled, supported carrying-capacity and Encumbrance rules apply.

Detailed mechanics are defined in:

`06_EQUIPMENT_AND_INVENTORY.md`

---

# 20. Reusable Content Pools

## 20.1 Approved Direction

### Status: APPROVED DIRECTION

RealmWeaver should support structured reusable content pools to reduce repetitive AI-generated world content.

Potential pools include:

* Character names
* Settlement names
* Tavern names
* Dungeon names
* Wilderness-location names
* Faction names
* Item names
* Cultural/regional naming groups
* Other reusable terminology

---

## 20.2 Content Source Restrictions

### Status: APPROVED DIRECTION

Imported content should come from:

* Appropriately licensed sources
* Public-domain sources
* Other permitted material

RealmWeaver should not blindly scrape copyrighted setting-specific content for reuse.

---

## 20.3 Used-Name Tracking

### Status: APPROVED DIRECTION

Campaign state should track previously used names/identities where appropriate.

This helps reduce unintended repetition.

Possible future mechanisms include:

* Direct pool selection
* AI selection from candidate names
* Procedural generation
* Cultural/regional pools
* Duplicate detection
* Used-name registry

Detailed implementation is deferred to later M2 architecture.

---

# 21. Encounter Persistence

## 21.1 Save Before Narrative Continuation

### Status: APPROVED

Authoritative encounter results should be persisted before normal narrative play resumes.

This reduces risks such as:

* Dead NPCs reappearing
* Forgotten loot
* Incorrect quest state
* Lost HP changes
* Duplicated enemies
* Forgotten captures
* Forgotten escapes
* Incorrect reward state

---

## 21.2 State Before Narration

The intended order is:

```text
Combat Ends
    ↓
Mechanical Finalisation
    ↓
Persist State
    ↓
Build Updated Context
    ↓
AI Narrates Aftermath
```

The AI should narrate from the saved authoritative result rather than narrating first and hoping state later matches the story.

---

# 22. AI Combat Authority Boundary

## 22.1 AI May

The AI may:

* Interpret combat intent
* Decompose complex natural-language actions
* Choose enemy tactical intent where appropriate
* Propose enemy actions
* Describe visible environmental circumstances
* Narrate attacks
* Narrate misses
* Narrate damage
* Narrate defeat
* Narrate surrender
* Narrate aftermath

---

## 22.2 Rules Engine Controls

The deterministic rules systems determine:

* Combat start state
* Initiative
* Turn order
* Round state
* Action availability
* Bonus Action availability
* Reaction availability
* Movement legality
* Distance relationships
* Attack validity
* Attack modifiers
* Hit/miss
* Damage
* Damage type
* Resistance
* Vulnerability
* Immunity
* Critical results
* HP
* Temporary HP
* Healing
* Unconsciousness
* Death Saves
* Death
* Loot legality
* Quest state
* Mechanical rewards
* Progression events
* Persistent encounter state

---

## 22.3 Invalid AI Combat Output

### Status: APPROVED

If the AI proposes something mechanically invalid, RealmWeaver should reject or correct it before narration.

Example:

```text
AI proposes:
Enemy makes three attacks.

Enemy feature data:
1 attack only.

Rules engine:
Rejects extra attacks.
```

The final narration must reflect the validated result rather than the invalid proposal.

---

# 23. Standard Combat Resolution Flow

## 23.1 Player Turn Flow

```text
Player describes action
        ↓
AI interprets intent
        ↓
Structured action proposal
        ↓
Rules engine validates:
- Action?
- Movement?
- Bonus Action?
- Reaction trigger?
- Target?
- Range?
- Resources?
        ↓
Resolve required checks / attacks / damage
        ↓
Update authoritative state
        ↓
AI narrates result
```

---

## 23.2 Enemy Turn Flow

```text
Enemy turn begins
        ↓
Determine tactical profile/objective
        ↓
Deterministic logic or AI proposes action
        ↓
Rules engine validates
        ↓
Invalid?
    ├── Yes → fallback / retry
    └── No
        ↓
Mechanical resolution
        ↓
Update state
        ↓
AI narrates visible result
```

---

# 24. Group Status

**Group 4 — Combat: APPROVED**

Approved areas:

1. Combat Start & Initiative
2. Turn Structure & Action Economy
3. Reactions
4. Distance-Band Positioning
5. Movement
6. Opportunity Attacks
7. Complex Natural-Language Combat Actions
8. Attacks, Armour Class & Damage
9. Critical Hits & Critical Misses
10. HP, Healing, Unconsciousness & Death
11. Enemy Combat AI
12. Combat End & Encounter Resolution
13. Combat Finalisation
14. Progression Rewards
15. Contextual Loot
16. Quest & World Consequences
17. Reward Types
18. Encumbrance Campaign Setting
19. Reusable Content-Pool Direction
20. Encounter Persistence
21. AI Combat Authority Boundary

Related specifications:

* Character statistics → `01_CHARACTER_CORE.md`
* Checks and Saving Throws → `02_CHECKS_AND_SAVES.md`
* Dice and Inspiration → `03_DICE_AND_INSPIRATION.md`
* Classes and Progression → `05_CLASSES_AND_PROGRESSION.md`
* Equipment and Encumbrance → `06_EQUIPMENT_AND_INVENTORY.md`
* Magic → `07_MAGIC.md`
* Conditions and Resting → `08_CONDITIONS_AND_RESTING.md`
