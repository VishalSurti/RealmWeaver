# RealmWeaver — AI / Rules Boundary

**Document Type:** Game Rules Specification  
**Milestone:** M2.1 — V1 Game Rules Specification & Rules-Engine Boundary  
**Group:** 9 — AI / Rules Boundary  
**Section Status:** 9A–9L APPROVED

**Rules Design Status:** COMPLETE

**Internal Review Status:** PASSED

**Internal Review Gate:** PASSED

**M2.1 Completion Gate:** PENDING
**Last Reviewed:** 31 August 2026

---

# 1. Purpose

This document defines the authoritative boundary between RealmWeaver's AI Dungeon Master and RealmWeaver's deterministic game systems.

RealmWeaver is designed around two foundational principles:

> **AI tells the story. Rules decide what happens.**

and:

> **AI proposes. RealmWeaver validates.**

The AI Dungeon Master provides the flexible reasoning, natural-language understanding, creativity and roleplay necessary for an open-ended tabletop-style campaign.

RealmWeaver's deterministic systems provide authoritative mechanics, persistent state, validation and resolution.

Neither system replaces the other.

The purpose of this boundary is to allow the AI to behave creatively like a Dungeon Master without allowing probabilistic model output to become the authoritative source of mechanical or persistent game state.

This document defines requirements for:

* AI authority
* RealmWeaver authority
* Player authority
* Mechanical proposals
* Player-intent interpretation
* Natural-language actions
* Structured actions
* Mechanical validation
* Mechanical commitment
* AI narration
* NPC decision-making
* Persistent world and NPC state
* AI knowledge boundaries
* Context and memory
* AI failure and retry behaviour
* Duplicate-resolution protection
* Latency and player experience

Detailed implementation decisions such as API contracts, tool/function calling, structured-output schemas, service boundaries, LLM providers, prompt architecture, context-retrieval implementation and database schemas are deferred to later M2 architecture work.

---

# 2. Core Principle

RealmWeaver separates creative intelligence from mechanical authority.

Conceptually:

```text
AI DM
→ Understands language
→ Interprets intent
→ Reasons creatively
→ Roleplays NPCs
→ Makes narrative decisions
→ Proposes mechanical interactions
→ Narrates validated outcomes

RealmWeaver
→ Stores authoritative state
→ Validates mechanics
→ Resolves deterministic rules
→ Resolves dice
→ Tracks resources
→ Tracks persistent state
→ Commits mechanical/world-state changes
```

The AI is not the rules engine.

The rules engine is not the storyteller.

RealmWeaver combines both systems to provide a flexible but mechanically consistent campaign.

---

# 3. 9A — Authority Model

## 3.1 Status

**APPROVED**

---

## 3.2 Three Categories of Authority

RealmWeaver divides campaign decisions into three broad authority categories:

1. AI Authority
2. RealmWeaver Authority
3. Shared / Proposal Authority

These categories define whether a decision may be made creatively, must be determined mechanically, or may originate from AI reasoning but requires RealmWeaver validation.

---

## 3.3 AI Authority

The AI controls creative decisions that do not independently alter authoritative mechanical or persistent state.

Examples include:

* Narrative description
* NPC dialogue
* NPC personality expression
* Tone
* Sensory description
* Scene presentation
* Narrative pacing
* Descriptive combat narration
* Interpretation of natural-language intent
* Creative suggestions
* Narrative flavour

Example:

> The old merchant nervously glances toward the locked cellar door.

This descriptive action does not inherently require deterministic mechanical resolution.

AI narrative freedom remains subject to existing authoritative world state.

The AI may not use narrative freedom to contradict or silently modify authoritative mechanics or persistent facts.

---

## 3.4 RealmWeaver Authority

RealmWeaver is authoritative over deterministic mechanics and persistent state.

This includes, where applicable:

* Ability scores
* Ability modifiers
* Proficiency
* Skills
* Saving Throws
* Armour Class
* Hit Points
* Temporary Hit Points
* Spell slots
* Spell preparation
* Spell legality
* Spell effects
* Concentration
* Conditions
* Exhaustion
* Weapon Mastery
* Equipment state
* Wield state
* Inventory
* Ammunition
* Currency
* Experience
* Level
* Class resources
* Rest state
* Dice results
* Attack resolution
* Damage
* Healing
* Movement
* World time
* Persistent NPC mechanical state
* Persistent world state
* Quest mechanical state
* Item ownership
* Item location
* Temporary effects
* Effect duration and expiry

If AI narration conflicts with authoritative RealmWeaver state, RealmWeaver state wins.

Example:

```text
Authoritative Player HP:
31

AI incorrectly implies:
4 HP
```

The authoritative value remains:

```text
31 HP
```

AI output cannot override it.

---

## 3.5 Shared / Proposal Authority

Some decisions require creative reasoning but ultimately interact with deterministic mechanics.

For these decisions, the AI may propose an interpretation or mechanical interaction.

RealmWeaver validates the proposal before it becomes authoritative.

Example:

```text
Player:
"I try to convince the suspicious guard to let me through."

AI proposes:

CHECK_REQUIRED
Skill: Persuasion
Difficulty Category: Hard
Reason: Guard is strongly suspicious
```

RealmWeaver then determines the authoritative mechanical resolution.

Conceptually:

```text
AI Proposal
        ↓
RealmWeaver Validation
        ↓
Authoritative DC / Mechanics
        ↓
Dice Resolution
        ↓
Mechanical Result
        ↓
State Commitment
        ↓
AI Narration
```

The AI may identify the likely mechanical interaction.

It does not independently determine the authoritative result.

---

## 3.6 Authority Matrix

RealmWeaver uses the following conceptual authority model:

| Decision | Authority |
| --- | --- |
| Story narration | AI |
| NPC dialogue | AI |
| NPC personality | AI |
| Scene description | AI |
| Narrative pacing | AI |
| Player intent interpretation | AI proposes |
| Whether a check may be appropriate | AI proposes |
| Check type | AI proposes; RealmWeaver validates |
| Difficulty / DC | RealmWeaver |
| Dice result | RealmWeaver |
| Attack legality | RealmWeaver |
| Attack result | RealmWeaver |
| Damage | RealmWeaver |
| AC / HP | RealmWeaver |
| Spell legality | RealmWeaver |
| Spell effects | RealmWeaver |
| Conditions | RealmWeaver |
| Weapon Mastery | RealmWeaver |
| Inventory / equipment / wield state | RealmWeaver |
| Currency | RealmWeaver |
| XP award | AI may propose; RealmWeaver validates |
| Loot/reward generation | AI proposes; RealmWeaver validates/materialises |
| Quest creation | AI proposes |
| Quest mechanical state | RealmWeaver |
| NPC tactical choice | AI chooses/proposes |
| NPC action legality | RealmWeaver |
| New NPC creation | AI proposes |
| Persistent NPC state | RealmWeaver |
| World/lore generation | AI proposes |
| Persistent world facts | RealmWeaver after materialisation |
| World clock | RealmWeaver |
| Hidden information access | RealmWeaver |
| Narrative consequences | AI within validated state |

This matrix defines behavioural authority.

Detailed implementation mechanisms are deferred to later architecture.

---

## 3.7 AI Cannot Directly Mutate Authoritative State

The AI must never directly modify authoritative game state.

For example, AI output such as:

```text
"The player loses 10 HP."
```

must not directly become:

```text
player.hp -= 10
```

Instead, a mechanically relevant proposal follows an authoritative resolution path.

Conceptually:

```text
AI proposes mechanical event
        ↓
RealmWeaver validates source
        ↓
RealmWeaver validates target
        ↓
RealmWeaver resolves mechanics
        ↓
RealmWeaver applies modifiers
        ↓
RealmWeaver commits state
        ↓
AI receives validated result
        ↓
AI narrates
```

Only RealmWeaver may commit authoritative mechanical state changes.

---

## 3.8 AI Cannot Override Deterministic Results

Once RealmWeaver resolves a deterministic result, AI narration must respect that result.

Example:

```text
Goblin Attack Roll: 14
Player AC: 16

Result:
MISS
```

The AI may narrate:

> The goblin's blade whistles past your shoulder.

or:

> You catch the strike against the rim of your shield.

The AI may not narrate the attack as causing mechanical damage.

Creative narration exists inside the authoritative mechanical result.

---

## 3.9 Narrative Flavour Does Not Require Universal Validation

RealmWeaver should not attempt to mechanically validate every descriptive statement generated by the AI.

Examples such as:

> Dust hangs in the abandoned hallway.

or:

> Rain taps softly against the window.

may remain narrative flavour where they do not materially establish or modify authoritative game state.

Validation is required when AI output:

* Changes authoritative state
* Depends upon authoritative state
* Creates a mechanically relevant entity or object
* Establishes a persistent world fact
* Produces a mechanical consequence
* Grants or removes a resource
* Changes a character, NPC, item, quest or world-state record

RealmWeaver should avoid unnecessary validation of purely descriptive flavour.

---

## 3.10 Authoritative State Overrides AI Memory

AI context, conversation history and model inference are not authoritative mechanical storage.

Conceptually:

```text
AUTHORITATIVE REALMWEAVER STATE
        >
AI CONTEXT
        >
AI MEMORY / INFERENCE
```

Example:

```text
Earlier:
NPC Gold = 200

NPC spends:
80

Current Authoritative State:
NPC Gold = 120
```

If the AI later incorrectly recalls the NPC as possessing 200 Gold, RealmWeaver's stored value of 120 remains authoritative.

The AI must use current authoritative state when mechanically relevant information is required.

---

## 3.11 Mechanically Relevant Uncertainty

The AI must not invent mechanically relevant information when authoritative state exists or can be queried.

Examples include:

* Current HP
* Armour Class
* Inventory
* Equipped items
* Wielded items
* Spell slots
* Prepared spells
* Conditions
* Weapon Mastery
* Currency
* Quest state
* NPC mechanical state
* Current location
* Current world time

If the AI is uncertain about such information, it should obtain or request the authoritative state rather than guess.

The implementation mechanism for retrieving this state is deferred to later architecture.

---

## 3.12 Player Authority

The player retains authority over meaningful decisions made by their player-controlled character.

The AI may interpret player intent.

It may not invent meaningful player decisions.

Example:

Player:

> I approach the bandit.

The AI may not silently transform this into:

> You draw your sword and attack him.

Likewise, if a mechanic provides several meaningful options, the AI cannot choose for the player unless the player has already expressed that choice.

Example:

```text
Cleave Available

Valid:
- Bandit B
- Bandit C
- Skip
```

The player chooses.

Trivial details that do not meaningfully affect the outcome do not require player confirmation.

---

## 3.13 NPC Authority

AI-controlled NPCs may make their own narrative and tactical decisions.

Example:

```text
NPC Turn
        ↓
AI chooses:
Attack Player
        ↓
RealmWeaver validates
        ↓
RealmWeaver resolves
        ↓
AI narrates
```

If the proposed action is mechanically invalid:

```text
AI proposes:
Cast Fireball

RealmWeaver:
REJECTED

Reason:
No valid spell slot
```

the invalid action is not committed.

The AI may then select another valid NPC action according to the applicable behaviour defined later in this specification.

The AI controls NPC decision-making.

RealmWeaver controls whether those decisions are mechanically legal and how they resolve.

---

## 3.14 Proposal, Validation and Commitment

AI-generated content is not automatically authoritative.

RealmWeaver distinguishes conceptually between:

```text
PROPOSED
        ↓
VALIDATED
        ↓
COMMITTED
        ↓
AUTHORITATIVE
```

Example:

```text
AI proposes:
"An enchanted dagger lies inside the chest."

        ↓

RealmWeaver validates/materialises item

        ↓

Persistent Item Instance created

        ↓

Item becomes authoritative world state
```

Before commitment, generated content remains a proposal.

After commitment, RealmWeaver's stored representation becomes authoritative.

Detailed materialisation, identity generation, database schemas and service ownership are deferred to later M2 architecture.

---

## 3.15 New NPC and Item Materialisation

When AI-generated content becomes mechanically or persistently relevant, RealmWeaver may materialise it into authoritative state.

Conceptually:

```text
AI proposes content
        ↓
RealmWeaver validates proposal
        ↓
Required mechanical data is resolved
        ↓
Persistent instance is created
        ↓
Unique identity and state are stored
        ↓
AI receives committed result
```

For an item, authoritative data may eventually include:

* Base item/weapon type
* Item properties
* Mechanical modifiers
* Weapon Mastery mapping where applicable
* Ownership
* Location
* Value
* Unique identity

For an NPC, authoritative data may eventually include:

* Identity
* Role
* Location
* Relationships
* Persistent narrative facts
* Mechanical profile where required
* Inventory/equipment where required
* Current state

Not every narrative NPC or object requires immediate full mechanical materialisation.

Detailed lightweight/persistent entity architecture is deferred to later M2 work.

---

## 3.16 Authority Invariants

RealmWeaver adopts the following hard authority invariants:

1. Authoritative mechanical state belongs to RealmWeaver.
2. AI cannot directly mutate authoritative state.
3. AI may propose mechanical actions/events; RealmWeaver validates them.
4. Deterministic results cannot be overridden by AI narration.
5. AI controls narrative expression within validated mechanical and world state.
6. Player-controlled meaningful choices remain with the player.
7. AI may make meaningful choices for AI-controlled NPCs.
8. Authoritative state overrides AI memory, inference or prior narration.
9. Mechanically relevant uncertainty must be resolved from authoritative state rather than guessed.
10. Pure narrative flavour should not require unnecessary deterministic validation.
11. AI-generated content is not authoritative until it has passed the applicable validation/materialisation process.
12. Committed RealmWeaver state becomes the source of truth for future AI reasoning and narration.

These invariants apply throughout the RealmWeaver AI/rules architecture.

---

# 4. 9B — Player Intent Interpretation

## 4.1 Status

**APPROVED**

---

## 4.2 Natural-Language Input

RealmWeaver allows players to express actions naturally rather than requiring direct mechanical commands.

Example:

> I charge across the room, leap onto the table and swing my sword at the cultist.

This may contain multiple mechanically relevant intents:

```text
MOVE
INTERACT_WITH_ENVIRONMENT
ATTACK
```

The player is not required to manually express these as structured commands.

The AI interpretation layer may translate natural-language input into structured action proposals for RealmWeaver validation.

---

## 4.3 Structured Intent Proposal

Natural-language interpretation produces a structured representation of player intent.

Conceptually:

```text
Player:

"I rush him and stab him with my rapier."

        ↓

Interpretation:

Primary Action:
WEAPON_ATTACK

Weapon:
RAPIER

Target:
BANDIT

Movement Intent:
APPROACH_TARGET
```

The exact structured-output schema is deferred to later architecture.

The important requirement is that natural-language interpretation produces proposals rather than directly changing game state.

---

## 4.4 Interpretation Does Not Determine Success

Understanding what the player intends to attempt is separate from determining whether the attempt succeeds.

Example:

> I jump over the pit.

Interpretation may determine:

```text
Action:
JUMP

Destination:
OPPOSITE_LEDGE
```

The interpretation layer does not determine:

```text
Result:
SUCCESS
```

RealmWeaver determines whether:

* The action is automatically possible
* A check is required
* The action is impossible under current conditions
* Applicable modifiers exist
* A roll is required
* The attempt succeeds or fails

---

## 4.5 Preserve Player Intent

RealmWeaver should preserve the player's intended action and goal wherever mechanically relevant.

Example:

> I put my sword on the table and try to intimidate the guard.

This should not automatically become:

```text
ATTACK_GUARD
```

A better interpretation is conceptually:

```text
Primary Intent:
SOCIAL_ACTION

Goal:
INTIMIDATE_GUARD

Supporting Action:
DISPLAY_WEAPON
```

Player intent should not be unnecessarily transformed into a different mechanical action.

---

## 4.6 Primary and Supporting Intent

A player message may contain a primary action, supporting actions and an intended goal.

Example:

> I quietly move behind the crates so I can get closer without the guards noticing.

Conceptually:

```text
Primary Action:
MOVE_TO_COVER

Goal:
AVOID_DETECTION

Style:
STEALTHY
```

RealmWeaver should preserve relevant intent information when validating the action.

---

## 4.7 Compound Actions

A single player message may contain several requested actions.

Example:

> I run over, draw my sword and attack.

The interpretation layer may produce:

```text
1. MOVE
2. DRAW_WEAPON
3. ATTACK
```

RealmWeaver then validates the sequence against:

* Movement
* Action economy
* Equipment state
* Wield state
* Interaction availability
* Current combat state
* Other applicable mechanics

Natural-language phrasing does not bypass normal rules.

---

## 4.8 Natural Language Cannot Bypass Action Economy

A player cannot gain additional mechanical actions merely by describing many actions in one message.

Example:

> I attack the goblin three times, drink a potion, run across the chamber and cast a spell.

RealmWeaver decomposes the requested sequence and validates each applicable component.

Only mechanically legal actions may be resolved.

---

## 4.9 Partial Validity

Compound actions may contain both valid and invalid components.

Example:

> I run to the orc and attack him with my greatsword.

Possible state:

```text
Movement:
VALID

Greatsword Attack:
INVALID
Reason:
Weapon not currently available in required equipment/wield state
```

RealmWeaver should avoid automatically committing an irreversible valid component when later invalid components could materially change whether the player would have chosen the action.

Where practical, compound actions should be preflight-validated before meaningful irreversible state changes are committed.

---

## 4.10 Preflight Validation

Compound player actions conceptually follow:

```text
Interpret requested sequence
        ↓
Preflight validation
        ↓
Determine valid/invalid components
        ↓
Resolve meaningful ambiguity if required
        ↓
Commit valid authorised action sequence
```

Detailed transaction and rollback implementation is deferred to later architecture.

The behavioural requirement is that RealmWeaver should avoid disadvantaging the player because only part of a compound request was understood or validated before commitment.

---

## 4.11 Clarification Threshold

RealmWeaver should not ask unnecessary clarification questions.

Clarification is normally required when ambiguity:

1. Materially changes the mechanical outcome.
2. Affects a meaningful player decision.
3. Cannot be resolved confidently from available context.

Example:

> I attack him.

If several creatures could reasonably be the intended target, clarification may be required.

If only one obvious target exists, RealmWeaver should proceed without unnecessary interruption.

---

## 4.12 Context May Resolve Trivial Ambiguity

RealmWeaver may use recent conversational and scene context to resolve ordinary references.

Example:

Previous action:

> I approach the bandit captain.

Next input:

> I attack him.

If the Bandit Captain is the clear contextual referent, the system may interpret:

```text
Target:
BANDIT_CAPTAIN
```

without requiring unnecessary clarification.

Contextual interpretation does not override authoritative mechanical state.

---

## 4.13 High-Stakes and Irreversible Choices

RealmWeaver should require stronger interpretation certainty for meaningful or irreversible player choices.

Examples include:

* Consuming a rare item
* Spending a limited resource
* Selecting between several spells
* Making a major agreement
* Attacking an important NPC
* Permanently relinquishing an item
* Making another mechanically or narratively significant commitment

If the player's intended choice is not sufficiently clear, RealmWeaver should ask for clarification rather than guess.

---

## 4.14 Explicit Mechanical Intent

Players familiar with the rules may state mechanical intent directly.

Examples:

> I make a Perception check.

> I attempt to Shove him Prone.

> I use Sneak Attack.

RealmWeaver should preserve explicit mechanical intent where possible.

Explicit player wording does not bypass validation.

For example:

> I make a DC 5 Persuasion check.

does not grant the player authority to choose the authoritative DC.

RealmWeaver validates the requested mechanic and determines the applicable rules.

---

## 4.15 Narrative Intent

Players are not required to understand or name underlying game mechanics.

Example:

> I look around for anything suspicious.

The interpretation layer may determine that the action corresponds to an applicable Perception, Investigation or other supported mechanic depending on the player's described approach and current situation.

RealmWeaver then validates the proposed mechanical interpretation.

This allows new players to interact naturally without requiring detailed tabletop rules knowledge.

---

## 4.16 Avoid Unnecessary Checks

RealmWeaver should not convert every player action into a dice roll.

Examples:

> I open the unlocked door.

> I walk downstairs.

> I ask the bartender for a drink.

These actions normally require no check unless current circumstances introduce meaningful uncertainty or consequence.

Checks should generally exist when there is:

* Meaningful uncertainty
* Meaningful consequence
* A relevant mechanical challenge

The AI should not manufacture checks solely to create activity.

---

## 4.17 Required Checks Must Not Be Bypassed

The opposite principle also applies.

AI narration cannot automatically grant success when a valid action requires mechanical resolution.

Example:

> I convince the king to surrender his kingdom to me.

The AI cannot narrate success merely because the player's argument sounds persuasive.

RealmWeaver determines whether:

* The action is possible
* The action is impossible
* A check is appropriate
* Another mechanic applies
* The situation requires additional interaction

---

## 4.18 Impossible Versus Difficult

RealmWeaver distinguishes impossible actions from difficult actions.

Example:

> I jump to the moon.

Under ordinary circumstances this is not:

```text
Athletics Check:
DC 30
```

It is mechanically impossible.

By contrast:

> I climb the rain-soaked castle wall.

may be possible but difficult.

Conceptually:

```text
Action Assessment:
IMPOSSIBLE
```

versus:

```text
Action Assessment:
POSSIBLE
CHECK_REQUIRED
```

The AI may propose this distinction.

RealmWeaver validates it.

A dice roll does not automatically make an otherwise impossible action possible.

---

## 4.19 Desired Consequences Are Not Guaranteed

Player intent may include a desired consequence.

Example:

> I shoot the rope holding the chandelier so it falls onto the guards.

The interpreted action may include:

```text
Action:
ATTACK_ROPE

Desired Consequence:
DROP_CHANDELIER_ON_GUARDS
```

The desired consequence is not automatically granted.

RealmWeaver determines applicable mechanics such as:

* Whether the rope can be targeted
* Object interaction
* Attack resolution
* Damage
* Whether the rope breaks
* Chandelier movement
* Valid targets
* Environmental consequences

The AI understands the creative intent.

RealmWeaver determines what actually happens mechanically.

---

## 4.20 Creative Actions

RealmWeaver must support creative actions that do not have dedicated UI buttons.

Examples:

> I throw sand in his eyes.

> I shove the bookshelf against the door.

> I freeze the puddle to make the floor slippery.

The AI may translate these into structured creative-action proposals.

RealmWeaver may then resolve them using applicable systems such as:

* Existing rules
* Controlled checks
* Saving Throws
* Controlled difficulty categories
* Environmental effects
* World objects
* Conditions
* Temporary effects
* Other validated mechanical proposals

RealmWeaver should not require every possible player action to be hardcoded in advance.

---

## 4.21 Interpretation Ambiguity

The intent-interpretation system must be capable of representing uncertainty or ambiguity.

Conceptually:

```text
Intent:
ATTACK

Target:
ORC_CAPTAIN

Interpretation Confidence:
HIGH
```

versus:

```text
Intent:
ATTACK

Possible Targets:
ORC_A
ORC_B

Interpretation:
AMBIGUOUS
```

RealmWeaver does not currently require a specific numerical confidence model.

The exact representation is deferred to architecture.

---

## 4.22 Structured UI Actions

Not every player action requires AI interpretation.

If the player uses an explicit structured interface such as:

```text
Cast Spell
→ Magic Missile
→ Goblin
```

or:

```text
Attack
→ Longsword
→ Orc Captain
```

the mechanical intent may already be sufficiently explicit.

Such actions may proceed directly to deterministic validation without an unnecessary LLM interpretation request.

Examples may include:

* Attack
* Cast Spell
* Stand Up
* Spend Hit Die
* Use Item
* End Turn
* Other explicit mechanical UI actions

Natural-language interaction and structured UI actions should coexist.

---

## 4.23 Interpretation Failure

If RealmWeaver cannot reliably determine the player's intended action, it should not guess and commit an irreversible action.

Instead, the player should receive a natural clarification request.

The interface should avoid exposing unnecessary parser, schema or model-error terminology.

The experience should remain consistent with interacting with a Dungeon Master.

---

## 4.24 Player Correction Before Commitment

A player may correct a misunderstood interpretation before mechanical resolution is committed.

Example:

```text
Interpreted Target:
Bandit Captain

Player:
"No, I meant the mage."
```

The proposed action may then be corrected before resolution.

RealmWeaver should not force the player to accept a materially incorrect interpretation when the action has not yet been committed.

---

## 4.25 No Silent Retcon After Commitment

Once mechanical resolution has been committed, later AI reinterpretation must not silently rewrite the resolved action.

Conceptually:

```text
ATTACK COMMITTED
        ↓
DICE ROLLED
        ↓
DAMAGE RESOLVED
        ↓
STATE UPDATED
```

A later reinterpretation does not silently change the original target or action.

Any future undo, rollback or save-restoration system must be an explicit RealmWeaver feature rather than an AI narrative retcon.

---

## 4.26 Intent Interpretation and Latency

Player-intent interpretation exists directly in the primary gameplay loop.

It should therefore avoid unnecessary latency.

Simple, mechanically explicit actions should not require unnecessarily expensive reasoning.

Structured UI actions may bypass AI interpretation entirely when intent is already known.

Detailed model routing, caching, structured parsing and performance architecture are deferred to later M2 work.

---

## 4.27 Player Intent Interpretation Invariants

RealmWeaver adopts the following requirements:

1. Natural-language player input may be interpreted into structured action proposals.
2. Intent interpretation does not determine mechanical success.
3. Player intent and goals should be preserved where mechanically relevant.
4. Inputs may contain primary and supporting intents.
5. Compound inputs may become ordered action sequences.
6. Natural-language phrasing cannot bypass action economy.
7. Compound actions should be preflight-validated before irreversible partial execution where practical.
8. Clarification is required only when ambiguity materially affects outcome or player choice.
9. Context may resolve trivial ambiguity.
10. High-stakes or irreversible choices require stronger interpretation certainty.
11. Explicit mechanical requests are preserved but remain subject to validation.
12. Narrative actions may map to mechanics without requiring the player to know formal rules terminology.
13. RealmWeaver avoids unnecessary checks.
14. Required mechanical uncertainty is not bypassed through AI narration.
15. Impossible actions are distinguished from difficult actions.
16. Desired consequences are proposals rather than guaranteed outcomes.
17. Creative natural-language actions may generate structured mechanical proposals.
18. Intent interpretation must be capable of representing meaningful ambiguity.
19. Structured UI actions may bypass AI interpretation.
20. Interpretation failure results in player-friendly clarification rather than irreversible guessing.
21. Player corrections are accepted before mechanical commitment.
22. Committed mechanical resolution is not silently retconned through later reinterpretation.
23. Intent interpretation should be designed to avoid unnecessary gameplay latency.

---

# 5. Deferred Architecture Questions

The following questions have intentionally not been answered by Sections 9A–9B:

* How the AI technically communicates with RealmWeaver
* Whether communication uses tool/function calling, structured outputs or another mechanism
* Exact proposal schemas
* Exact validation APIs
* Service boundaries
* Database schemas
* Item-instance creation implementation
* NPC-instance creation implementation
* Unique-ID generation
* Lightweight versus fully materialised entity implementation
* Transaction/rollback implementation
* LLM provider selection
* Model routing
* Prompt architecture
* Context retrieval implementation
* Caching
* Streaming
* Performance optimisation

These are implementation and architecture concerns.

Sections 9A–9B define the behavioural contract that those future systems must satisfy.

---

# 6. 9C — AI Mechanical Proposals

## 6.1 Status

**APPROVED**

---

## 6.2 Purpose

The AI may identify or propose mechanically relevant interactions during natural-language play.

A proposal is not authoritative simply because the AI generated it.

Conceptually:

```text
AI:
"What mechanic appears appropriate here?"

        ↓

RealmWeaver:
"Is that mechanic valid, and what is the authoritative result?"
```

AI mechanical proposals allow RealmWeaver to preserve flexible tabletop-style interaction while ensuring that rules, legality and persistent state remain deterministic and authoritative.

---

## 6.3 Proposal Categories

The AI may propose mechanically relevant interactions including:

* Ability Checks
* Saving Throws
* Combat actions
* Spell use
* Conditions and temporary effects
* Movement and positioning
* Resource use
* NPC tactical actions
* Loot and rewards
* XP/reward significance
* Quest creation
* NPC creation
* Item creation
* Location creation
* Persistent world facts
* Time advancement
* Environmental hazards and effects
* Constrained creative/custom mechanical effects

All proposals remain subject to RealmWeaver validation.

---

## 6.4 Checks and Saving Throws

The AI may propose that a check or Saving Throw appears appropriate.

Conceptually:

```text
CHECK_PROPOSAL

Skill:
Perception

Suggested Difficulty:
Moderate

Reason:
Character attempts to detect concealed movement.
```

or:

```text
SAVE_PROPOSAL

Ability:
Dexterity

Reason:
Character is attempting to avoid falling debris.
```

RealmWeaver validates:

* Whether a roll is required
* Whether the proposed check/save type is appropriate
* Applicable ability or skill
* Authoritative difficulty/DC
* Modifiers
* Proficiency
* Advantage/disadvantage
* Other relevant effects
* Final roll
* Success/failure

The AI does not independently determine the authoritative roll result.

The AI does not own the final DC merely because it proposed a difficulty.

---

## 6.5 Combat Action Proposals

The AI may propose combat actions for AI-controlled creatures and may interpret player-described combat actions.

Example:

```text
ACTION_PROPOSAL

Actor:
Guard Captain

Action:
WEAPON_ATTACK

Weapon:
LONGSWORD

Target:
PLAYER
```

RealmWeaver validates all applicable mechanics including:

* Actor state
* Action availability
* Action economy
* Equipment
* Wield state
* Weapon properties
* Range
* Target legality
* Conditions
* Resources
* Movement
* Applicable features
* Weapon Mastery
* Other combat rules

The AI chooses or proposes the action.

RealmWeaver determines whether it is mechanically legal and how it resolves.

---

## 6.6 Spell Proposals

The AI may propose spell use.

Example:

```text
CAST_SPELL_PROPOSAL

Spell:
HOLD_PERSON

Target:
PLAYER
```

RealmWeaver validates applicable requirements including:

* Spell access
* Known/prepared state
* Available spell slots
* Casting time
* Action economy
* Components
* Range
* Valid targets
* Concentration
* Existing effects
* Class/resource restrictions
* Other spell-specific requirements

An invalid spell proposal does not consume resources or alter state.

---

## 6.7 Conditions and Temporary Effects

The AI may propose that a creative action could produce a condition or temporary mechanical effect.

Example:

Player:

> I throw sand into his eyes.

The AI may propose:

```text
CREATIVE_EFFECT_PROPOSAL

Desired Effect:
TEMPORARY_VISUAL_IMPAIRMENT

Source:
THROW_SAND
```

RealmWeaver determines whether this maps to:

* An existing condition
* Advantage/disadvantage
* A temporary modifier
* Another supported temporary effect
* No meaningful mechanical effect

The AI should not arbitrarily apply powerful conditions without a valid mechanical source.

For example:

```text
APPLY_BLINDED
duration = 10 rounds
```

is not valid merely because the AI generated it.

---

## 6.8 Resource-Use Proposals

The AI may propose actions that require resources.

Examples include:

```text
USE_ITEM
item = HEALING_POTION
```

```text
CAST_SPELL
slot_level = 2
```

```text
RANGED_ATTACK
ammunition = ARROW
```

RealmWeaver verifies that the required resource exists and that its use is legal.

Only RealmWeaver may commit consumption of:

* Items
* Spell slots
* Ammunition
* Charges
* Hit Dice
* Class resources
* Limited-use features
* Currency
* Other tracked resources

The AI does not independently decrement authoritative resources.

---

## 6.9 Time-Advancement Proposals

The AI may propose approximate campaign-time advancement for narrative activities whose duration is not already determined by explicit mechanics.

Example:

```text
TIME_ADVANCE_PROPOSAL

Activity:
Conversation with merchant

Suggested Duration:
15 minutes
```

RealmWeaver validates the proposed duration before advancing authoritative campaign time.

Rules-defined durations remain rules-owned.

Examples include:

* Combat rounds
* Short Rests
* Long Rests
* Spell casting times
* Explicit effect durations
* Other mechanically defined time periods

The AI cannot override these fixed durations.

---

## 6.10 Loot and Reward Proposals

The AI may propose loot or rewards generated through narrative events.

Example:

```text
REWARD_PROPOSAL

Gold:
40

Items:
- Silver Ring
```

RealmWeaver validates the proposal against relevant systems such as:

* Quest context
* Reward rules
* Economy constraints
* Item availability
* Item legality
* Rarity
* Character level/context
* Existing ownership
* Duplication
* Provenance
* Campaign restrictions

Only after successful validation/materialisation do currency or item instances become authoritative state.

---

## 6.11 XP Proposals

The AI may propose that a player achievement deserves progression credit.

Example:

```text
XP_REWARD_PROPOSAL

Reason:
Major diplomatic resolution

Significance:
MAJOR
```

RealmWeaver maps the proposal into the authoritative progression framework.

The AI should not independently assign arbitrary unsupported XP quantities.

Example:

```text
+5000 XP
```

is not automatically valid unless an authoritative rule, encounter or content definition already specifies that award.

---

## 6.12 Quest Proposals

The AI may propose new quests or objectives.

Example:

```text
QUEST_PROPOSAL

Title:
Missing Caravan

Quest Giver:
Merchant Elian

Goal:
Locate the missing caravan
```

Once RealmWeaver validates/materialises the quest, authoritative persistent state may include:

```text
AVAILABLE
ACTIVE
COMPLETED
FAILED
REWARDED
```

The AI may narrate developments within the quest.

RealmWeaver owns the persistent quest state.

---

## 6.13 NPC Creation Proposals

The AI may propose new NPCs.

Example:

```text
NPC_PROPOSAL

Name:
Seran

Role:
Guard Captain

Location:
East Gate

Disposition:
Suspicious
```

RealmWeaver may create a lightweight persistent representation where appropriate.

If the NPC later becomes mechanically important, further mechanical state may be materialised and persisted.

AI-generated NPC details remain proposals until they are accepted into authoritative RealmWeaver state.

---

## 6.14 Item Creation Proposals

The AI may propose new narrative items.

Example:

```text
ITEM_PROPOSAL

Description:
Ornate silver dagger

Location:
Locked chest
```

RealmWeaver determines applicable authoritative properties, potentially including:

* Base item type
* Mechanical properties
* Weapon properties
* Weapon Mastery mapping
* Rarity
* Value
* Ownership
* Location
* Magical properties
* Unique identity
* Provenance

The AI may not casually invent unsupported mechanical bonuses.

For example:

```text
+5 Longsword
```

does not automatically become valid authoritative content.

---

## 6.15 Location and World-Fact Proposals

The AI may propose new locations and world facts.

Example:

```text
LOCATION_PROPOSAL

Name:
Chapel of Saint Varyn

Parent Location:
Old District
```

or:

```text
WORLD_FACT_PROPOSAL

Fact:
Duke Harland secretly funds the smugglers.
```

Before materialisation, such information remains generated content.

After RealmWeaver accepts and persists it, the stored world state becomes authoritative for future narration and reasoning.

---

## 6.16 Environmental Proposals

The AI may propose environmental hazards or terrain effects.

Examples:

```text
ENVIRONMENTAL_HAZARD_PROPOSAL

Type:
EXTREME_COLD
```

```text
TERRAIN_EFFECT_PROPOSAL

Type:
SLIPPERY_SURFACE
```

RealmWeaver validates:

* Whether the environment supports the proposed effect
* Which existing rules apply
* Applicable checks or Saving Throws
* Damage/effect rules
* Countermeasures
* Duration
* Removal/mitigation conditions

Narrative description alone does not automatically create mechanical terrain or hazard effects.

---

## 6.17 Validation-Relevant Proposal Context

Mechanical proposals should include enough concise context for RealmWeaver to understand why the proposal exists.

Example:

```text
CHECK_PROPOSAL

Skill:
Intimidation

Reason:
Player openly threatens the guard while presenting evidence of the guard's corruption.
```

This is preferable to:

```text
CHECK_PROPOSAL

Skill:
Intimidation
```

when context materially affects validation.

Proposal context should be concise and machine-useful.

RealmWeaver does not require the AI to expose private chain-of-thought reasoning.

---

## 6.18 Unsupported Mechanics

The AI must not create arbitrary new game systems that are not part of the active RealmWeaver ruleset.

Examples of unsupported invention include:

```text
Fear Meter = 73
```

```text
Critical Chance +18%
```

```text
Mana = 42
```

unless such mechanics have been explicitly added to the active ruleset.

Creative actions should use existing mechanical primitives wherever possible.

---

## 6.19 Constrained Custom Mechanical Proposals

Some creative situations may not map cleanly to a predefined rule.

The AI may propose a constrained custom effect.

Example:

> The player pushes the bookshelf across the doorway.

Conceptually:

```text
CUSTOM_EFFECT_PROPOSAL

Type:
TERRAIN_OBSTRUCTION

Location:
DOORWAY

Desired Effect:
RESTRICT_MOVEMENT
```

RealmWeaver then determines the valid supported representation.

The AI does not generate arbitrary executable rules code.

---

## 6.20 Prefer Existing Mechanics

Where an existing RealmWeaver mechanic already represents the intended effect, the AI should prefer that mechanic instead of inventing a new one.

Example:

If a creature is knocked to the ground, prefer:

```text
PRONE
```

rather than:

```text
CUSTOM_DOWNED_STATE
```

This reduces rules fragmentation and keeps mechanical behaviour consistent.

---

## 6.21 Minimal Proposal Scope

Mechanical proposals should normally describe only the immediate action or required mechanic rather than predicting all downstream consequences.

Avoid:

```text
Attack
→ predicted damage
→ predicted death
→ predicted loot
→ predicted quest completion
→ predicted next scene
```

Prefer:

```text
ATTACK_PROPOSAL
```

RealmWeaver resolves the attack.

Subsequent events are determined only after the authoritative result is known.

---

## 6.22 Dependent Mechanical Proposals

Some actions contain dependent stages.

Example:

```text
Attack rope
        ↓
Did rope break?
        ↓
If yes:
Chandelier begins falling
        ↓
Determine affected targets
        ↓
Resolve damage/effects
```

Later consequences must not assume earlier stages succeeded.

RealmWeaver resolves dependent mechanical events in order.

---

## 6.23 Proposal Does Not Equal Acceptance

RealmWeaver explicitly distinguishes:

```text
PROPOSED
≠
VALID
≠
COMMITTED
```

An AI proposal may ultimately be:

```text
ACCEPTED
MODIFIED
REJECTED
NEEDS_CLARIFICATION
```

These validation outcomes are defined in Section 9D.

---

## 6.24 Proposal Provenance

Mechanically relevant proposals should retain provenance sufficient for debugging and consistency analysis.

Conceptually:

```text
source = AI_DM
```

or:

```text
source = NPC_AI
```

or:

```text
source = PLAYER_INTENT_INTERPRETATION
```

or:

```text
source = RULE_EFFECT
```

Additional applicable source identifiers may be stored where useful.

Detailed event/audit schema is deferred to architecture.

---

## 6.25 Player-Facing Presentation

Structured AI proposal machinery is an internal system concern.

The player should normally experience natural interaction.

Internal:

```text
CHECK_PROPOSAL
skill = Persuasion
```

Player-facing:

> Make a Persuasion check.

RealmWeaver should not expose unnecessary schemas, validation codes or internal AI-control terminology during ordinary gameplay.

---

## 6.26 AI Mechanical Proposal Invariants

RealmWeaver adopts the following requirements:

1. AI may propose checks and Saving Throws.
2. RealmWeaver validates whether the roll is required and determines authoritative mechanics.
3. AI may propose NPC combat actions.
4. AI may propose spell use.
5. AI may propose creative condition/effect outcomes, but arbitrary condition application requires valid mechanical support.
6. Resource use may be proposed but is committed only by RealmWeaver.
7. AI may propose narrative time advancement where duration is not mechanically fixed.
8. AI may propose loot/rewards; RealmWeaver validates and materialises them.
9. AI may propose progression significance but not arbitrary unsupported XP awards.
10. AI may propose quests; persistent quest state belongs to RealmWeaver.
11. AI may propose NPC creation.
12. AI may propose item creation.
13. AI may propose locations and persistent world facts.
14. AI may propose environmental hazards/effects.
15. Mechanical proposals should include concise validation-relevant context.
16. AI cannot invent unsupported mechanical systems.
17. Creative situations may use constrained custom-effect proposals.
18. Existing supported mechanics should be preferred over unnecessary custom mechanics.
19. Proposals should normally remain limited to the immediate required mechanic.
20. Dependent consequences must wait for earlier authoritative results.
21. A proposal does not imply acceptance or commitment.
22. Mechanically relevant proposal provenance should be retained.
23. Structured proposal machinery should remain invisible to the player during normal gameplay.

---

# 7. 9D — Validation & Rejection

## 7.1 Status

**APPROVED**

---

## 7.2 Purpose

RealmWeaver validates AI mechanical proposals before they may affect authoritative state.

The system must avoid both blindly accepting AI output and rejecting every imperfect proposal.

RealmWeaver therefore distinguishes between proposals that are:

```text
ACCEPTED
MODIFIED
REJECTED
NEEDS_CLARIFICATION
```

These are conceptual validation outcomes.

Exact implementation naming and schema are deferred to architecture.

---

## 7.3 Accepted Proposals

A proposal is accepted when it is mechanically valid as submitted.

Example:

```text
AI Proposal:

WEAPON_ATTACK
weapon = LONGSWORD
target = GOBLIN
```

Validation:

```text
Weapon available:
YES

Weapon wielded correctly:
YES

Target legal:
YES

Range valid:
YES

Action available:
YES
```

Result:

```text
ACCEPTED
```

RealmWeaver may then proceed to authoritative mechanical resolution.

---

## 7.4 Modified Proposals

A proposal may be mechanically appropriate while containing one or more incorrect rules-owned details.

Example:

```text
AI Proposal:

Persuasion Check
Suggested Difficulty:
HARD
```

RealmWeaver determines:

```text
Check:
VALID

Authoritative Difficulty:
MODERATE
```

Result:

```text
MODIFIED
```

The entire proposal does not need to be rejected when RealmWeaver can safely correct a purely mechanical detail.

---

## 7.5 Modification Must Preserve Intent

Automatic modification may correct rules-owned mechanical details.

It must not silently change the meaningful intent behind the action.

For example, RealmWeaver may correct:

* Damage die
* Modifier
* AC
* DC
* Weapon property
* Spell-slot bookkeeping
* Range calculation
* Rules-derived value

RealmWeaver should not silently transform:

```text
Attack Orc with Longsword
```

into:

```text
Attack Goblin with Shortbow
```

because that changes meaningful actor choice.

---

## 7.6 Rejected Proposals

A proposal is rejected when the requested mechanic is invalid and cannot be safely repaired without changing meaningful intent.

Example:

```text
AI-controlled Wizard proposes:

CAST_FIREBALL
```

Authoritative state:

```text
Fireball:
NOT AVAILABLE
```

Result:

```text
REJECTED

Reason:
SPELL_NOT_AVAILABLE
```

No spell slot is consumed.

No mechanical state is committed.

---

## 7.7 Impossible Actions

Truly impossible actions are rejected rather than converted into arbitrary high-difficulty checks.

Example:

```text
Actor proposes:

WALK_THROUGH_STONE_WALL
```

and no applicable spell, feature, item or environmental rule allows the action.

Result:

```text
REJECTED

Reason:
ACTION_IMPOSSIBLE
```

RealmWeaver does not create a high DC merely to allow every attempted action a chance of success.

---

## 7.8 Needs Clarification

A proposal may be mechanically valid in principle but lack a meaningful required choice.

Example:

```text
CAST_MAGIC_MISSILE

Targets:
UNSPECIFIED
```

when several legal target configurations exist.

Result:

```text
NEEDS_CLARIFICATION
```

RealmWeaver should request the missing decision rather than guess when the choice materially affects the outcome.

---

## 7.9 Player Versus NPC Clarification

Clarification is handled differently based on who owns the meaningful decision.

### Player-Controlled Character

If the missing information affects player choice, RealmWeaver asks the player.

### AI-Controlled NPC

If the missing information concerns an NPC's tactical decision, RealmWeaver returns the problem or legal alternatives to the AI.

The player should not be asked to make tactical decisions on behalf of hostile or otherwise AI-controlled NPCs.

---

## 7.10 Validation Order

RealmWeaver should perform validation in a predictable prerequisite-first order.

Conceptually:

```text
1. Is the proposal structurally valid?
2. Does the actor exist?
3. Is the actor currently capable of acting?
4. Does the requested mechanic exist?
5. Are required resources available?
6. Is equipment/state valid?
7. Does the target exist?
8. Is the target mechanically legal?
9. Are range/position requirements satisfied?
10. Is action economy valid?
11. Are rule-specific prerequisites satisfied?
12. Resolve the mechanic
```

The exact internal validation pipeline is deferred to architecture.

The behavioural requirement is that RealmWeaver should detect invalid prerequisites before performing irreversible state changes.

---

## 7.11 Validation Before Commitment

Validation should occur before irreversible mechanical state changes whenever practical.

Example:

```text
Validate spell
Validate spell slot
Validate target
Validate range
Validate concentration requirements
        ↓
Resolve complete spell effect and state change
        ↓
Commit / persist resource consumption and effects atomically
```

RealmWeaver should not consume resources during preliminary validation.

---

## 7.12 No Partial Resource Consumption on Rejection

If a proposal is rejected, it must not accidentally consume part of the requested resources.

Examples:

* Invalid potion use does not remove the potion.
* Invalid spell does not consume a spell slot.
* Rejected ranged attack does not consume ammunition.
* Invalid charge use does not reduce charges.
* Invalid Hit Die use does not consume the Hit Die.
* Invalid feature use does not consume the feature.

Resources are committed only once applicable legality requirements have been satisfied.

---

## 7.13 Coherent Mechanical Resolution

Mechanical actions may produce multiple state changes.

Example:

```text
Ranged Attack
        ↓
Ammunition consumed
        ↓
Damage applied
        ↓
Weapon Mastery effect applied
        ↓
Target HP updated
```

These related changes should commit coherently.

RealmWeaver must avoid leaving authoritative state partially updated because one later part of the same committed resolution failed.

Detailed database transactions and rollback implementation are deferred to architecture.

---

## 7.14 Rules-Owned Derived Values

RealmWeaver may automatically correct values that are derived from authoritative mechanics.

Example:

```text
AI proposes:

Longsword damage = 1d10
```

Current wield state:

```text
ONE_HANDED
```

RealmWeaver uses the applicable authoritative value instead.

Likewise:

```text
AI:
AC = 15

RealmWeaver State:
AC = 17
```

The authoritative value is 17.

No player clarification is required for correction of rules-owned derived data.

---

## 7.15 Meaningful Player Choices Must Not Be Replaced

RealmWeaver may not repair an invalid proposal by silently substituting a different meaningful player choice.

Example:

Player:

> I use my last Healing Potion on Mira.

If Mira is not a valid target, RealmWeaver must not silently use the potion on the player instead.

The action should be rejected or clarified.

---

## 7.16 Unsupported Mechanics

If AI output references a mechanic that RealmWeaver does not support, the system must not silently add that mechanic to the campaign.

Example:

```text
MORALE_SCORE = 73
```

Possible result:

```text
REJECTED

Reason:
UNSUPPORTED_MECHANIC
```

Alternatively, if the intended effect maps safely to an existing supported rule, RealmWeaver may return:

```text
MODIFIED
```

with the supported equivalent.

---

## 7.17 Prefer Supported Equivalent Mechanics

When an unsupported proposal can be mapped cleanly to an existing RealmWeaver rule without changing intent or balance, modification is preferred over unnecessary rejection.

Example:

AI proposes:

```text
Target becomes temporarily off-balance.
```

RealmWeaver may map this to an existing supported temporary modifier if one accurately represents the intended effect.

The mapping must preserve the intended scope and balance.

---

## 7.18 No Silent Balance Escalation

Automatic repair must not convert an invalid proposal into a substantially stronger mechanic.

Example:

AI proposes:

```text
Minor temporary movement penalty
```

RealmWeaver should not automatically replace it with:

```text
RESTRAINED
```

if that creates a significantly stronger effect.

If no safe supported equivalent exists, the proposal should be rejected or passed through the constrained custom-effect pathway.

---

## 7.19 Validation of AI-Generated Rewards

AI-generated rewards must be validated before materialisation.

Validation may consider:

* Item legality
* Character/campaign level
* Rarity
* Economy
* Quest significance
* Existing loot
* Duplication
* Campaign restrictions
* Balance
* Provenance

A reward proposal may be:

```text
ACCEPTED
MODIFIED
REJECTED
```

Example:

```text
AI Proposal:
+3 Longsword

Current Campaign Context:
Low-level characters
```

RealmWeaver may reject or modify the reward according to the authoritative reward policy.

Exact balancing rules are deferred to later content/system design.

---

## 7.20 Validation of NPC Proposals

Narrative NPC identity and NPC mechanical state may be validated independently.

Example:

```text
AI Proposal:

Name:
Captain Seran

Role:
Veteran Guard Captain

AC:
23

HP:
300
```

RealmWeaver may accept:

```text
Name:
Captain Seran

Role:
Veteran Guard Captain
```

while modifying the proposed mechanical profile to an appropriate valid archetype.

This allows AI creativity without granting unrestricted control over NPC mechanical power.

---

## 7.21 Validation of World Facts

AI proposals may not overwrite contradictory authoritative world state.

Example:

AI proposes:

> The king died last week.

Authoritative state:

```text
King:
ALIVE

Current Location:
ROYAL_PALACE
```

The persistent fact is rejected.

The AI may still narrate unverified information where narratively appropriate:

> Rumours have spread that the king died last week.

A rumour does not automatically modify the king's actual authoritative state.

---

## 7.22 Contradictory Proposals

When an AI proposal contradicts authoritative state, RealmWeaver state wins.

The proposal may be:

```text
MODIFIED
```

when the contradiction is incidental,

or:

```text
REJECTED
```

when the contradiction is central to the proposed action.

Example:

```text
AI proposes:
NPC draws Longsword

Authoritative State:
NPC no longer owns Longsword
```

Result:

```text
REJECTED
```

---

## 7.23 Hidden Information During Validation

Validation must respect character and NPC knowledge boundaries.

A proposal may be mechanically impossible from an actor's knowledge perspective even if the underlying hidden world state exists.

Example:

```text
Player Action Proposal:

Attack invisible assassin behind curtain
```

If the character has never detected or otherwise learned about the assassin, RealmWeaver must not allow hidden authoritative information to be used as though the character knows it.

The same anti-metagaming principle applies to AI-controlled NPCs.

Knowledge boundaries are expanded in Section 9I.

---

## 7.24 Safe Automatic Repair

RealmWeaver may automatically repair a proposal when all of the following are true:

1. Meaningful actor intent remains unchanged.
2. The correction is mechanical rather than strategic.
3. No meaningful new choice is introduced.
4. The correct value or interpretation is unambiguous.

Examples may include:

* Incorrect derived modifier
* Incorrect damage die
* Incorrect current AC
* Incorrect difficulty classification
* Incorrect slot bookkeeping
* Incorrect rules-derived range
* Other unambiguous mechanical corrections

---

## 7.25 Automatic Repair Restrictions

Automatic repair must not silently change meaningful choices such as:

* Target
* Spell
* Consumed item
* Chosen weapon
* Movement route
* Tactical approach
* Strategic decision
* Reward selection
* Irreversible player decision

These situations require rejection or clarification.

---

## 7.26 Validation Feedback to AI

When a proposal is rejected or modified, RealmWeaver should provide concise machine-useful feedback sufficient for recovery.

Example:

```text
REJECTED

Reason:
NO_SPELL_SLOT

Relevant State:
No level-3 spell slots remain.
```

or:

```text
MODIFIED

Field:
difficulty

Proposed:
HARD

Authoritative:
MODERATE
```

The exact response schema is deferred to architecture.

---

## 7.27 Limit Validation Context

Validation feedback should provide enough information for correction without unnecessarily sending large amounts of internal state.

Prefer:

```text
Action invalid:
No level-3 spell slot available.

Available:
Level-1 spells and cantrips
```

over exposing a large internal validation trace.

This supports:

* Lower latency
* Reduced token usage
* Reduced context pollution
* Lower hallucination risk
* Cleaner AI recovery

---

## 7.28 Player-Facing Error Translation

Internal validation results should be translated into natural player-facing explanations.

Internal:

```text
REJECTED

Reason:
TARGET_OUT_OF_RANGE
```

Player-facing:

> The cultist is too far away for that spell from your current position.

Players should not normally see internal error codes or validation schemas.

---

## 7.29 Helpful Rejection

Rejection should help the player continue where practical.

Instead of:

> INVALID ACTION.

RealmWeaver may provide:

> Your rapier is still sheathed, so you cannot make that attack with it yet. You can draw it first, or attack using the dagger already in your hand.

Useful legal alternatives may be surfaced when doing so does not make a meaningful choice on the player's behalf.

---

## 7.30 NPC Recovery From Rejection

When an AI-controlled NPC proposes an invalid action, the failure should normally be handled internally.

Example:

```text
NPC AI:
CAST_FIREBALL

RealmWeaver:
REJECTED
NO_SPELL_SLOT
```

RealmWeaver returns the relevant correction to the AI.

The AI may then choose another action.

The player should not normally be shown that the AI initially attempted an illegal action.

---

## 7.31 Repeated Invalid NPC Proposals

RealmWeaver must prevent indefinite validation/retry loops.

If an AI-controlled NPC repeatedly proposes invalid actions, later architecture must provide a bounded recovery path.

Possible future mechanisms include:

* Returning a constrained list of legal actions
* Reducing the AI's available choice space
* Falling back to a deterministic legal action
* Ending the attempted decision after a bounded number of retries

The exact fallback mechanism is deferred to architecture.

The requirement is that invalid AI behaviour must not produce an infinite gameplay loop.

---

## 7.32 Deterministic Validation

When legality depends on deterministic rules, the same authoritative state and same mechanical proposal should produce the same legality result.

AI randomness must not determine questions such as:

```text
Is this spell available?
Is this weapon equipped?
Is this target in range?
Does the actor own this item?
Is the actor currently Prone?
Is an action available?
```

These are RealmWeaver-owned determinations.

---

## 7.33 Validation Provenance

RealmWeaver should retain sufficient information to understand why a mechanically relevant proposal was:

```text
ACCEPTED
MODIFIED
REJECTED
NEEDS_CLARIFICATION
```

Example:

```text
Proposal:
CAST_HOLD_PERSON

Result:
REJECTED

Reason:
INVALID_TARGET_TYPE
```

or:

```text
Proposal:
Persuasion — Hard

Result:
MODIFIED

Authoritative Difficulty:
Moderate
```

This supports:

* Debugging
* Testing
* Mechanical audits
* Reproducibility
* AI-behaviour evaluation

The final audit/event schema is deferred to later architecture.

---

## 7.34 Validation Performance

Routine validation should primarily use RealmWeaver's deterministic systems and authoritative state rather than additional LLM calls.

Examples include:

* Inventory lookup
* Equipment validation
* Wield-state validation
* Spell-slot lookup
* Range calculation
* Action-economy validation
* Condition lookup
* Target legality
* Weapon Mastery validation
* Resource availability

An additional AI request should not be required merely to answer deterministic rules questions that RealmWeaver can resolve locally.

---

## 7.35 Validation and Rejection Invariants

RealmWeaver adopts the following requirements:

1. Validation conceptually produces `ACCEPTED`, `MODIFIED`, `REJECTED`, or `NEEDS_CLARIFICATION`.
2. Valid proposals proceed to mechanical resolution.
3. Partially incorrect proposals may be safely modified.
4. Modification must preserve meaningful intent.
5. Invalid proposals are rejected without authoritative state mutation.
6. Impossible actions are rejected rather than converted into arbitrary high-difficulty checks.
7. Missing meaningful choices result in clarification.
8. Player-controlled clarification goes to the player; NPC-controlled clarification returns to AI.
9. Validation should follow a predictable prerequisite-first order.
10. Validation occurs before irreversible commitment whenever practical.
11. Rejected proposals do not partially consume resources.
12. Multi-state mechanical resolutions should commit coherently.
13. RealmWeaver may correct rules-owned derived values automatically.
14. RealmWeaver may not silently replace meaningful player decisions.
15. Unsupported mechanics are rejected or safely mapped to supported equivalents.
16. Supported equivalents are preferred only when they preserve intent and balance.
17. Automatic repair must not escalate the strength of the proposed effect.
18. AI-generated rewards require validation before materialisation.
19. NPC narrative identity and mechanical profile may be validated separately.
20. Persistent world facts that contradict authoritative state are rejected.
21. Contradictory proposals defer to authoritative RealmWeaver state.
22. Validation must respect hidden-information and knowledge boundaries.
23. Safe automatic repair is allowed for unambiguous mechanical corrections.
24. Automatic repair is not allowed when it changes meaningful target, resource or strategic choices.
25. Validation feedback to AI should be concise and sufficient for recovery.
26. Validation should avoid unnecessary context/token bloat.
27. Internal rejection reasons should be translated into player-friendly explanations.
28. Rejection should surface useful legal alternatives where appropriate.
29. Invalid NPC proposals should normally be repaired internally without exposing AI failure to the player.
30. Repeated invalid NPC proposals require a bounded fallback mechanism.
31. Deterministic validation must produce deterministic legality results from the same state and proposal.
32. Validation decisions should retain appropriate provenance/audit information.
33. Routine deterministic validation should not require unnecessary LLM calls.

---

# 9. 9E — Mechanical Resolution Pipeline

## 9.1 Status

**APPROVED**

---

## 9.2 Purpose

RealmWeaver uses a consistent resolution pipeline for mechanically relevant actions.

The purpose of this pipeline is to ensure that:

* Player or NPC intent is interpreted correctly
* AI-generated mechanical proposals remain non-authoritative
* Rules are validated before resolution
* Dice and other deterministic mechanics remain RealmWeaver-owned
* State changes are committed coherently
* AI narration describes authoritative outcomes rather than creating them

The canonical conceptual flow is:

```text
INTENT
        ↓
PROPOSE
        ↓
VALIDATE
        ↓
RESOLVE
        ↓
COMMIT / PERSIST
        ↓
NARRATE
        ↓
CONTINUE WORLD
```

Not every interaction requires every stage.

`RESOLVE` calculates the complete state change without making it authoritative.

`COMMIT / PERSIST` applies that complete change through atomic durable persistence. State becomes authoritative only after this stage succeeds.

Narration describes only successfully committed outcomes. A persistence failure must not be narrated as a completed outcome.

Exact transaction technology remains deferred to M2.2 — System Architecture.

---

## 9.3 Canonical Mechanical Resolution Pipeline

Mechanically relevant actions should conceptually follow:

```text
1. RECEIVE INPUT
        ↓
2. DETERMINE INTENT
        ↓
3. BUILD MECHANICAL PROPOSAL
        ↓
4. PREFLIGHT VALIDATION
        ↓
5. REQUEST PLAYER DECISION / ROLL IF REQUIRED
        ↓
6. RESOLVE MECHANICS
        ↓
7. DETERMINE CONSEQUENCES
        ↓
8. COMMIT / PERSIST AUTHORITATIVE STATE
        ↓
9. PRODUCE MECHANICAL RESULT
        ↓
10. AI NARRATES VALIDATED RESULT
        ↓
11. CONTINUE GAMEPLAY
```

Individual mechanics may omit stages that are not applicable.

---

## 9.4 Narrative-Only Resolution Path

Purely narrative interactions should not be forced through unnecessary mechanical validation.

Example:

```text
Player:
"I ask Mira where she grew up."

        ↓

Intent:
Conversation

        ↓

No mechanical resolution required

        ↓

AI-controlled NPC responds
```

RealmWeaver therefore conceptually supports:

```text
NARRATIVE PATH

Input
↓
AI Narrative Interaction
↓
Response
```

and:

```text
MECHANICAL PATH

Input
↓
Proposal
↓
Validation
↓
Resolution
↓
Commit
↓
Narration
```

The AI/rules boundary determines when mechanical resolution is required.

---

## 9.5 Narrative-to-Mechanical Transition

A narrative interaction may become mechanically relevant.

Example:

Player:

> I ask Mira where she grew up.

This may remain narrative.

The player then says:

> I think she is lying. I watch her carefully while she answers.

RealmWeaver may now receive:

```text
CHECK_PROPOSAL

Skill:
INSIGHT
```

Narrative scenes may therefore transition naturally into checks, combat, spell use, exploration or other mechanical resolution.

---

## 9.6 Preflight Validation

Before mechanical resolution begins, RealmWeaver validates applicable prerequisites.

Examples include:

```text
Actor valid?
Action available?
Target valid?
Required resource available?
Range valid?
Equipment state valid?
Action economy valid?
Other prerequisites satisfied?
```

Dice should not be rolled and resources should not be consumed before relevant preflight validation succeeds.

---

## 9.7 Visible and Hidden Rolls

RealmWeaver supports both player-visible and system/hidden rolls where appropriate.

### Player-Visible Roll

Example:

```text
Stealth Check
Modifier: +7

[ROLL]
```

The player initiates or observes the roll.

### Hidden/System Roll

Some rolls should not expose hidden information merely by being requested.

For example, prompting:

> Make a Perception check.

may reveal that something concealed is present.

Where rules and information boundaries require it, RealmWeaver may resolve an authoritative hidden/system roll without exposing the reason or result directly to the player.

Knowledge and hidden-information rules are expanded in Section 9I.

---

## 9.8 AI Cannot Fabricate Dice Results

All authoritative dice results originate from RealmWeaver's dice system.

The AI may never independently establish a result such as:

```text
"You rolled an 18."
```

unless that value was supplied by RealmWeaver.

The AI may narrate the consequence of a valid roll after RealmWeaver resolves it.

---

## 9.9 Dice Results as Resolution Inputs

Dice outcomes become inputs to authoritative mechanical resolution.

Example:

```text
Check:
STEALTH

Modifier:
+7

RealmWeaver Roll:
11

Total:
18
```

RealmWeaver compares the total against the applicable authoritative difficulty or opposing mechanic.

Example result:

```text
Outcome:
SUCCESS

Relevant Consequence:
Guard fails to detect player.
```

The AI receives the resolved result rather than independently recalculating or deciding it.

---

## 9.10 Resolve Before Narration

Mechanical consequences must be determined before final outcome narration.

Example:

```text
Attack:
HIT

Damage:
14

Target HP:
11 → 0

Other Applicable Effects:
Resolved
```

Only after these outcomes are known should the AI narrate the result.

This prevents contradictions such as narrating a creature's death before RealmWeaver has determined that the creature actually reached 0 HP.

---

## 9.11 Commit Before Final Outcome Narration

The normal ordering should be:

```text
RESOLVE
↓
COMMIT / PERSIST
↓
NARRATE
```

rather than:

```text
RESOLVE
↓
NARRATE
↓
COMMIT / PERSIST
```

Narration should describe state that successfully became authoritative.

If commitment fails, RealmWeaver should not already have presented an uncommitted mechanical outcome as fact.

---

## 9.12 Resolution Result for Narration

After a successful commit, RealmWeaver should provide the AI with sufficient structured outcome information.

Conceptually:

```text
ACTION:
Rapier Attack

RESULT:
HIT

DAMAGE:
17 piercing

TARGET:
Bandit Captain

TARGET_HP:
31 → 14

WEAPON_MASTERY:
Vex applied

OTHER:
Player no longer hidden
```

The exact schema is deferred to later architecture.

---

## 9.13 Minimum Sufficient Narration Context

The AI should receive enough authoritative information to narrate the outcome naturally.

Relevant context may include:

* Actor
* Action
* Target
* Result
* Important mechanical consequences
* Relevant scene details
* Relevant NPC personality/emotional context
* Relevant environmental information

The AI should not receive large quantities of unrelated state.

RealmWeaver therefore follows the principle:

> **Provide minimum sufficient authoritative context for accurate narration.**

---

## 9.14 Multi-Stage Resolution

Some actions require multiple dependent mechanical stages.

Example:

> I shoot the rope so the chandelier falls on them.

Possible resolution:

```text
Player attacks rope
↓
Attack resolution
↓
Object damage resolution
↓
Did rope break?
        ↓ YES
Chandelier falls
↓
Determine affected creatures
↓
Resolve Saving Throws
↓
Resolve damage/effects
↓
Commit authoritative state
↓
Narrate full outcome
```

Each dependent stage must use the authoritative result of the preceding stage.

Later stages must not assume earlier stages succeeded.

---

## 9.15 Avoid Unnecessary AI Calls Between Stages

Deterministic mechanics should continue through the resolution pipeline without repeatedly returning to the AI.

RealmWeaver should avoid patterns such as:

```text
AI call
↓
Rules
↓
AI call
↓
Rules
↓
AI call
↓
Rules
```

for a single deterministic action.

A new AI or player decision should be requested only where creative reasoning or a meaningful choice is genuinely required.

---

## 9.16 Reaction and Interruption Windows

Mechanical resolution may need to pause when another actor receives a valid reaction or decision opportunity.

Example:

```text
Enemy moves
↓
Reaction opportunity created
↓
Resolution pauses
↓
Player decides whether to react
↓
Decision validated
↓
Resolution resumes
```

RealmWeaver therefore supports the conceptual state:

```text
PAUSED_FOR_DECISION
```

where mechanically required.

---

## 9.17 Player Decisions Pause Resolution

If a mechanic requires a meaningful player choice, RealmWeaver must not guess that choice.

Examples include:

* Using a reaction
* Activating an optional feature
* Spending an optional resource
* Choosing targets
* Choosing between effects
* Selecting a valid Weapon Mastery option where choice exists
* Other player-controlled optional mechanics

Conceptually:

```text
RESOLUTION
↓
PLAYER_CHOICE_REQUIRED
↓
PAUSE
↓
PLAYER DECISION
↓
VALIDATE
↓
RESUME
```

---

## 9.18 AI-Controlled NPC Decisions

If an AI-controlled NPC reaches a meaningful decision point, RealmWeaver may provide the AI with the relevant legal options and context.

Conceptually:

```text
NPC DECISION REQUIRED
↓
RealmWeaver provides legal/contextual options
↓
AI chooses
↓
RealmWeaver validates
↓
Resolution resumes
```

The AI owns the NPC's meaningful tactical decision.

RealmWeaver owns its legality and mechanical resolution.

---

## 9.19 Automatic Mechanical Triggers

Authoritative state changes may trigger additional deterministic mechanics automatically.

Example:

```text
Damage applied
↓
Concentration check triggered
↓
Concentration fails
↓
Concentrated spell ends
↓
Dependent effects expire
```

or:

```text
Target HP reaches 0
↓
Applicable unconsciousness/death rules trigger
```

RealmWeaver should not rely on the AI to remember to request deterministic consequences that follow directly from authoritative state changes.

---

## 9.20 Bounded Trigger Chains

Automatically triggered mechanics must be processed safely.

RealmWeaver must prevent infinite or cyclic chains such as:

```text
Effect A triggers B
↓
B triggers C
↓
C triggers A
↓
...
```

Detailed cycle-detection or processing architecture is deferred.

The behavioural requirement is:

> **Triggered mechanical resolution must be bounded and cycle-safe.**

---

## 9.21 Temporary Effects

Temporary effects use the same authoritative resolution and persistence principles as other mechanics.

Example:

```text
Vex triggers
↓
RealmWeaver validates effect
↓
Temporary effect instance created
↓
Target/source recorded
↓
Expiry condition recorded
↓
Future qualifying action consumes or expires effect
```

This principle applies to:

* Weapon Mastery effects
* Conditions
* Spell effects
* Environmental effects
* Temporary modifiers
* Other supported transient state

Temporary state may never exist solely in AI narration when it has mechanical significance.

---

## 9.22 World Time as Mechanical State

Actions that consume authoritative campaign time must update RealmWeaver's world clock as part of their resolution.

Examples include:

* Short Rests
* Long Rests
* Travel
* Extended spellcasting
* Downtime activities
* Other time-consuming actions

Example:

```text
Short Rest
↓
Validate
↓
Resolve resource recovery
↓
Advance world clock
↓
Process applicable world events
↓
Commit
```

Detailed scheduling architecture is deferred to later M2 design.

---

## 9.23 World Events During Time Advancement

RealmWeaver's world may progress independently of the player's immediate location or actions.

If authoritative time advancement crosses a scheduled or applicable world event, RealmWeaver must account for that event.

Example:

```text
Current Time:
14:00

Scheduled World Event:
Army attacks city at 18:00

Player begins:
8-hour Long Rest
```

The world clock crosses 18:00.

RealmWeaver must not simply advance to 22:00 while ignoring the authoritative event.

Applicable world events may:

* Occur
* Progress
* Change persistent world state
* Interrupt an activity where appropriate
* Create consequences the player later discovers

The exact world-event and scheduling engine is deferred to architecture.

This establishes that RealmWeaver supports a persistent living world whose state can progress independently of the player.

---

## 9.24 Mechanical Event History

Meaningful mechanical resolutions should retain sufficient event/history information for later inspection.

Example:

```text
Actor:
Player

Action:
Attack Bandit Captain

Attack Roll:
19

Outcome:
HIT

Damage:
17

Target HP:
31 → 14

Weapon Mastery:
Vex applied
```

Not every decorative narrative sentence requires persistence.

Mechanically significant state changes should be reconstructable where useful.

---

## 9.25 Idempotent Mechanical Resolution

Retries must not apply the same committed mechanical action more than once.

Example:

```text
ACTION_ID:
XYZ

Request is retried
```

RealmWeaver conceptually checks:

```text
Has action XYZ already committed?

YES:
Return existing committed result

NO:
Resolve and commit
```

Exact identifiers and implementation are deferred to architecture.

The invariant is:

> **The same committed mechanical action must not be applied twice because of retries, timeouts or duplicate requests.**

---

## 9.26 Failure Before Commitment

If an internal failure occurs before authoritative commitment, RealmWeaver should avoid leaving partial mechanical state.

Conceptually:

```text
Proposal accepted
↓
Resolution begins
↓
Internal failure
↓
No authoritative commit
```

Where practical, the action should remain safely retryable.

Players should not lose resources solely because an internal system operation failed before commitment.

---

## 9.27 Failure After Commitment

If mechanics successfully commit but AI narration subsequently fails, RealmWeaver must not rerun the mechanics.

Correct behaviour:

```text
Mechanics resolved
↓
State committed
↓
Narration request fails
↓
Retrieve committed result
↓
Retry/regenerate narration
```

The original mechanical result remains authoritative.

---

## 9.28 Narration Failure Does Not Undo Mechanics

AI narration is not part of the authoritative mechanical transaction.

If narration fails after commitment, RealmWeaver may:

* Retry narration
* Regenerate narration
* Present a temporary mechanical summary
* Continue from the committed result when narration becomes available

The committed mechanical action remains valid.

---

## 9.29 No State Mutation Through Narration Parsing

RealmWeaver must never depend on parsing final AI prose to determine what mechanically occurred.

Invalid architecture:

```text
AI:
"The goblin falls dead."

Backend:
Parse "falls dead"
↓
Set Goblin HP = 0
```

Correct architecture:

```text
RealmWeaver:
Goblin HP reaches 0

↓
Authoritative result supplied to AI

AI:
"The goblin falls dead."
```

Mechanical state creates narration.

Narration does not create mechanical state.

---

## 9.30 Narration as Presentation Layer

The canonical relationship is:

```text
PLAYER INTENT
       ↓
MECHANICAL PROPOSAL
       ↓
VALIDATION
       ↓
RESOLUTION
       ↓
AUTHORITATIVE COMMIT
       ↓
NARRATIVE PRESENTATION
```

Narration presents authoritative outcomes in natural language.

It does not establish those outcomes.

---

## 9.31 Narration May Create Future Proposals

AI narration may introduce temporary sensory or atmospheric flavour without materialisation.

Significant persistent content must be proposed, validated, materialised and committed/persisted before it is presented as established reality.

Example:

> A nervous young courier pushes through the tavern door.

If the courier remains incidental, no persistent state is required. If the courier becomes persistently relevant, RealmWeaver must complete:

```text
NPC Proposal
↓
Validation / Materialisation
↓
Commit / Persist
↓
Authoritative NPC
↓
Presentation as Established Reality
```

Lightweight content that gains persistent significance through player interaction must be materialised before gaining persistent authority.

Rumours, beliefs, allegations, predictions and uncertain information may be presented before their underlying truth is established only when clearly framed as claims. In that case, the authoritative fact is that the claim was made, not that its contents are objectively true.

Failed significant-content materialisation must not produce player-visible established canon or require a silent retcon.

---

## 9.32 Resolution Debugging and Replay

The separation between:

```text
Input
Proposal
Validation
Rolls
Resolution
State changes
Narration
```

should support future debugging and mechanical replay/inspection.

Example:

```text
Attack missed because:

Roll:
9

Attack Modifier:
+6

Total:
15

Target AC:
17
```

This supports:

* Automated testing
* Debugging
* Rules-engine verification
* Player-facing roll inspection
* AI/rules disagreement analysis
* Reproducibility

Exact event-log/replay architecture is deferred.

---

## 9.33 Mechanical Resolution Pipeline Invariants

RealmWeaver adopts the following requirements:

1. Mechanically relevant actions follow an interpretation → proposal → validation → resolution → commitment → narration pipeline.
2. Purely narrative interactions may use a shorter narrative path.
3. Narrative interactions may transition into mechanical resolution when required.
4. Mechanical proposals undergo preflight validation before resolution.
5. RealmWeaver supports both player-visible and hidden/system rolls.
6. AI never fabricates authoritative dice results.
7. Dice results become authoritative inputs to mechanical resolution.
8. Mechanical consequences are determined before final narration.
9. Authoritative state should normally be committed before outcome narration.
10. RealmWeaver provides AI with a structured resolution result for narration.
11. AI receives minimum sufficient authoritative context for accurate narration.
12. Multi-stage mechanics resolve dependent stages in authoritative order.
13. Additional AI calls between stages occur only when genuine reasoning or choice is required.
14. Mechanical resolution can pause for valid reaction or decision windows.
15. Meaningful player choices pause resolution rather than being guessed.
16. AI-controlled NPC decisions may return to AI during a paused pipeline.
17. State changes automatically trigger applicable deterministic mechanics.
18. Trigger chains must be bounded and cycle-safe.
19. Temporary effects use the authoritative resolution/state pipeline.
20. Authoritative world-time changes are part of mechanical resolution.
21. Time advancement processes applicable scheduled/world events crossed during that period.
22. RealmWeaver supports persistent world events that may progress independently of the player's immediate actions or location.
23. Significant mechanical resolutions retain sufficient event/history information.
24. Committed mechanical actions must be idempotent against retries and duplicate requests.
25. Failures before commitment should not leave partial authoritative state.
26. Failures after commitment must reuse the committed result rather than rerunning mechanics.
27. Narration failure does not undo committed mechanics.
28. RealmWeaver never derives authoritative outcomes by parsing final AI narration.
29. Narration is a presentation layer over authoritative resolution.
30. AI narration may create future content proposals but cannot silently mutate authoritative state.
31. The resolution pipeline should support future debugging and mechanical replay/inspection.

---

# 10. 9F — AI Narration Boundary

## 10.1 Status

**APPROVED**

---

## 10.2 Purpose

AI narration transforms authoritative RealmWeaver outcomes into immersive natural-language storytelling.

The AI has broad freedom over narrative expression but does not gain authority over mechanics or persistent state through narration.

The governing relationship is:

```text
RealmWeaver:
Determines what happened

AI:
Determines how it is described
```

---

## 10.3 AI Authority Over Narrative Expression

Once RealmWeaver provides an authoritative outcome, the AI may express that outcome creatively.

Example authoritative result:

```text
Attack:
HIT

Damage:
14 slashing

Target HP:
38 → 24
```

Possible AI narration:

> Your blade slips through the opening in his guard, carving across his side. He staggers with a curse before raising his sword again.

The narrative expression may vary while preserving the same mechanical truth.

---

## 10.4 Mechanical Truth Constrains Narration

AI narration must remain consistent with authoritative mechanical and persistent state.

If RealmWeaver reports:

```text
Attack:
MISS
```

the AI may not narrate the attack striking the target.

If RealmWeaver reports:

```text
Target HP:
24
```

the AI may not narrate the target as dead.

If the player is:

```text
PRONE
```

the AI may not describe the player as standing normally unless that state subsequently changes.

Authoritative RealmWeaver state always wins over generated prose.

---

## 10.5 Narrative Embellishment Without Mechanical Mutation

The AI may add descriptive flavour that does not create mechanical state.

Example:

> The impact drives him back a step as he grunts in pain.

This does not automatically imply authoritative forced movement.

Likewise:

> He looks dazed for a moment.

does not automatically create a mechanical condition.

RealmWeaver distinguishes between:

```text
NARRATIVE DESCRIPTION
```

and:

```text
MECHANICAL STATE CHANGE
```

Only the latter requires authoritative mechanical resolution.

---

## 10.6 Avoid Mechanically Misleading Embellishment

Narrative embellishment should not strongly imply a mechanical event that did not occur.

If authoritative position has not changed, narration should avoid statements such as:

> The blow launches him fifteen feet across the chamber.

Similarly, if no disarm occurred:

> His sword flies from his hand.

would be mechanically misleading.

The AI may embellish within the boundaries of the actual result, but should avoid implying nonexistent meaningful state changes.

---

## 10.7 Natural Narration Over Raw Mechanical Terminology

AI narration does not need to expose mechanical terminology in every sentence.

If RealmWeaver applies:

```text
FRIGHTENED
```

the AI may narrate:

> The colour drains from his face as he recoils from the creature.

The mechanical UI may separately display:

```text
Frightened
```

Narration should remain immersive where possible.

---

## 10.8 Mechanically Relevant Information Remains Understandable

Immersion must not obscure information that the player requires to make informed decisions.

If the player becomes:

```text
POISONED
```

the narrative may say:

> The venom burns through your veins, leaving your movements unsteady.

RealmWeaver may simultaneously expose the authoritative condition through UI or inspectable mechanical information.

Narration and mechanical clarity should complement one another.

---

## 10.9 Narration Cannot Grant Resources

AI prose cannot independently create player or NPC resources.

Example:

> The merchant gives you 100 gold.

requires authoritative RealmWeaver state such as:

```text
Currency Change:
+100 GP
```

Likewise:

> You find a Healing Potion.

must create/materialise an authoritative item if the potion becomes interactable or usable.

---

## 10.10 Narration Cannot Remove Resources

AI prose cannot independently destroy, consume, steal or remove authoritative resources.

Examples requiring RealmWeaver resolution include:

* Weapon destruction
* Item loss
* Currency theft
* Ammunition consumption
* Spell-slot use
* Charge use
* Consumable use

Narration may describe such events only after they become authoritative.

---

## 10.11 Narration Cannot Apply Mechanical Conditions

Narrative description alone cannot establish a mechanical condition.

Example:

> The explosion leaves your ears ringing.

may be flavour.

It does not automatically mean:

```text
DEAFENED
```

If Deafened applies mechanically, RealmWeaver must validate and commit the condition.

---

## 10.12 Narration Cannot Change Meaningful Position

AI prose cannot independently relocate actors.

If RealmWeaver says:

```text
Orc:
ENGAGED_WITH_PLAYER
```

the AI cannot mechanically move the Orc across the chamber through narration alone.

Minor cinematic motion is allowed where it does not materially alter authoritative positioning.

Meaningful positional changes require mechanical resolution.

---

## 10.13 Narration Cannot Change Authoritative Time

AI narration cannot independently advance RealmWeaver's world clock.

Narrative phrases such as:

> After talking for a while...

may describe pacing.

If meaningful campaign time passes, RealmWeaver must validate and commit the actual time advancement.

The AI cannot independently change:

```text
14:00
```

to:

```text
18:00
```

through prose.

---

## 10.14 Narration Cannot Complete Quests

AI narration cannot independently change authoritative quest state.

The AI may describe apparent success:

> With the bandits defeated, the road finally seems safe.

But RealmWeaver determines whether the quest's completion criteria were actually satisfied.

Authoritative states such as:

```text
ACTIVE
COMPLETED
FAILED
REWARDED
```

remain RealmWeaver-owned.

---

## 10.15 Narration Cannot Grant XP or Levels

The AI may not independently establish progression changes.

Statements such as:

```text
You gain 500 XP.
```

or:

```text
You have reached level 5.
```

require authoritative progression state.

AI narration may celebrate or describe the advancement only after RealmWeaver commits it.

---

## 10.16 Narrating Failed Checks

Failure narration should reflect the actual consequence of the failed mechanic rather than exaggerating it.

Example:

```text
Persuasion:
FAILURE
```

does not automatically mean:

> The guard now hates you.

A valid narration may simply be:

> The guard remains unconvinced.

Likewise:

```text
Stealth:
FAILURE
```

does not necessarily reveal the character's exact location to every nearby creature.

Narration should match the resolved mechanical consequence.

---

## 10.17 Competent Characters Should Remain Competent

A failed roll should not unnecessarily portray a skilled character as incompetent or ridiculous.

Example:

A highly skilled Rogue fails a Stealth check.

Avoid:

> You trip over your own feet, crash into a barrel and shout in surprise.

unless the scene genuinely supports that outcome.

Prefer:

> You move quietly, but the guard happens to glance toward the corridor at exactly the wrong moment.

Mechanical failure does not necessarily imply character incompetence.

---

## 10.18 High Rolls Do Not Create Superpowers

High rolls do not grant outcomes beyond what the mechanic can reasonably accomplish.

Example:

```text
Persuasion:
Natural 20
```

does not automatically allow:

> The king abdicates and gives you the throne.

Narration remains constrained by:

* What the attempted action could reasonably accomplish
* Applicable rules
* World state
* NPC motivations
* Narrative context

This follows RealmWeaver's distinction between difficult and impossible outcomes.

---

## 10.19 Degree and Margin of Outcome

Where RealmWeaver exposes meaningful degree-of-success information, AI narration may reflect that difference.

Example:

```text
DC:
15

Result:
15
```

may be narrated as:

> After a long pause, the guard reluctantly steps aside.

Whereas:

```text
DC:
15

Result:
27
```

may be narrated as:

> The guard's suspicion visibly fades. He steps aside and apologises for delaying you.

A larger margin does not automatically create additional mechanical benefits unless the applicable rules explicitly support them.

---

## 10.20 Critical Results

Critical hits or other explicitly defined special results may receive more dramatic narration.

Example:

```text
CRITICAL_HIT
```

may justify heightened descriptive emphasis.

It does not automatically establish additional effects such as:

```text
DISMEMBERMENT
INSTANT_DEATH
STUN
```

unless RealmWeaver's mechanics produced those outcomes.

---

## 10.21 Player Agency

AI narration must not invent meaningful decisions for the player character.

Player:

> I enter the tavern.

AI should not automatically narrate:

> You enter, walk directly to the bartender, order an ale and tell him you are searching for the missing prince.

The player chose only:

```text
ENTER_TAVERN
```

Meaningful subsequent actions remain player-controlled.

---

## 10.22 Minor Cinematic Player Motion

The AI may add small physical details consistent with the player's expressed action.

Player:

> I threaten him.

AI may narrate:

> You lean closer and lower your voice.

This does not create a new meaningful decision.

However, the AI should not add:

> You draw your dagger and press it to his throat.

unless that action was already expressed or mechanically established.

---

## 10.23 Player Thoughts and Emotions

AI narration should generally avoid dictating the player character's internal thoughts, beliefs or emotions.

Avoid statements such as:

> You are terrified.

> You feel guilty.

> You immediately trust her.

unless an authoritative magical/mechanical effect explicitly imposes an applicable response.

The AI may instead describe external circumstances, sensations or cues and leave the player's interpretation to the player.

This preserves roleplay agency.

---

## 10.24 Mechanical Emotional Effects

Authoritative conditions such as:

```text
CHARMED
```

or:

```text
FRIGHTENED
```

may influence narration within their actual mechanical scope.

The AI should not exaggerate them beyond their rules-defined effects.

For example, Frightened does not automatically imply:

> You collapse screaming uncontrollably.

unless another applicable mechanic establishes that result.

---

## 10.25 AI Freedom Over NPC Narration

AI has significantly greater narrative freedom over AI-controlled NPCs.

The AI may determine:

* Dialogue
* Body language
* Emotional reactions
* Deception
* Social responses
* Personality expression
* Tactical reasoning
* Immediate nonmechanical behaviour

This freedom remains constrained by authoritative:

* Mechanical state
* Conditions
* Knowledge
* Relationships
* Goals
* Established world facts
* Location/state
* Applicable rules

---

## 10.26 Persistent NPC Changes

Significant NPC changes intended to influence future behaviour should become authoritative state rather than remaining only in prose.

Examples include:

```text
Hostile → Neutral
Neutral → Friendly
Trust increases significantly
Loyalty changes
Major promise made
Debt established
Important secret revealed
Relationship materially changes
```

The exact relationship/reputation model is deferred to later architecture.

The principle is that persistent narratively important NPC changes must be materialised and committed/persisted before they are presented as established changes.

---

## 10.27 Non-Conflicting World Detail

AI may freely enrich scenes with non-conflicting incidental detail.

Example authoritative state:

```text
Location:
Greyhaven

Type:
Port City

Government:
Merchant Council
```

AI may narrate:

> Gulls circle above slate rooftops while dock bells echo across the harbour.

Such descriptive detail enriches the world without requiring every visual detail to become authoritative state.

---

## 10.28 Incidental Detail Versus Persistent Fact

RealmWeaver distinguishes between incidental flavour and significant persistent facts.

Example incidental detail:

> A cracked blue vase sits beside the window.

This may remain purely narrative.

Example significant world fact:

> The king's hidden son lives in this village.

This may affect future continuity and therefore must enter the proposal, validation, materialisation and commit/persistence process before it is presented as established reality.

RealmWeaver should not persist every decorative detail, while important continuity-relevant facts should not exist solely in AI memory.

---

## 10.29 Promotion of Incidental Details

An incidental narrative detail may later become relevant through player interaction.

Example:

AI narrates:

> A cracked blue vase sits beside the window.

Player:

> I inspect the blue vase.

RealmWeaver may then promote/materialise the object:

```text
INCIDENTAL DETAIL
↓
Player interacts
↓
Materialisation required
↓
Authoritative object/state
```

This allows open-ended interaction without requiring every scene object to be pre-created.

---

## 10.30 Significant New Lore

Major worldbuilding information that should persist across the campaign requires proposal, validation, materialisation and commit/persistence before presentation as established reality.

Examples include:

* Rulers
* Major factions
* Settlements
* Wars
* Gods
* Major historical events
* Important political changes
* Major NPC relationships
* Other continuity-critical lore

AI may propose these facts.

They do not become authoritative merely because they appear in narration.

---

## 10.31 Hidden Information and Foreshadowing

AI may hint at hidden facts without exposing authoritative secrets directly.

Example authoritative state:

```text
Bartender:
SECRET_CULT_MEMBER
```

AI may narrate:

> The bartender's smile falters for the briefest moment when you mention the symbol.

It should avoid revealing:

> The cultist bartender looks nervous.

unless the player character has legitimately discovered that fact.

This supports mystery, foreshadowing and deception without leaking hidden state.

---

## 10.32 Suspicion Is Not Mechanical Knowledge

AI may create ambiguity or suspicion.

Example:

> You get the impression he may be holding something back.

This does not automatically mean:

```text
NPC_IS_LYING = TRUE
```

unless RealmWeaver has established that knowledge mechanically or through authoritative narrative state.

Narrative suspicion and authoritative knowledge are distinct.

---

## 10.33 Combat Narration Context

Combat narration should use relevant mechanical context where available.

Useful context may include:

* Weapon
* Damage type
* Target
* Hit/miss
* Critical result
* Damage amount
* Target condition
* Applicable Weapon Mastery
* Environment
* Relevant movement
* Important triggered effects

This allows narration to remain specific without giving AI authority over combat resolution.

---

## 10.34 Combat Narration Variety

AI should vary combat descriptions while preserving identical mechanics.

Repeated attacks should not always produce identical prose.

Narrative variation is encouraged when it does not change:

* Damage
* Position
* Conditions
* Equipment
* Resources
* Target state
* Other authoritative mechanics

---

## 10.35 Adaptive Narration Length

Narration length should scale with the significance of the event.

Conceptually:

```text
Routine attack:
Brief

Ordinary failed check:
Brief

Critical hit:
Moderate emphasis

Boss defeat:
Significant narration

Major story revelation:
Expanded narration
```

This supports pacing, readability, lower latency and reduced token usage.

---

## 10.36 Narrative and Mechanical Feedback

Narrative presentation and explicit mechanical information may coexist.

Example narration:

> Your arrow catches the cultist high in the shoulder. He stumbles but quickly steadies himself.

Mechanical UI may show:

```text
Hit
17 piercing damage
Cultist: 24 / 41 HP
Vex applied
```

RealmWeaver may present narrative as the primary experience while retaining clear mechanical transparency.

---

## 10.37 Inspectable Mechanical Resolution

Players should be able to inspect relevant mechanical resolution details where useful.

Conceptually:

```text
Narrative:
Your arrow strikes...

[View Roll Details]
```

Expanded:

```text
Attack Roll:
14 + 7 = 21

Target AC:
17

Result:
Hit

Damage:
1d8 + 4 + Sneak Attack

Total:
17
```

Exact frontend interaction is deferred.

The requirement is that authoritative mechanics remain inspectable even when the primary presentation is narrative.

---

## 10.38 Narration Contradiction Handling

If generated narration contradicts authoritative state, RealmWeaver state wins.

Example:

AI narrates:

> The bandit collapses dead.

Authoritative state:

```text
Bandit HP:
12
```

RealmWeaver must not modify the database to match the narration.

Instead, the narration should where practical be:

* Prevented
* Regenerated
* Corrected
* Replaced with an accurate presentation

Generated prose never overrides authoritative state.

---

## 10.39 Lightweight Narration Consistency Safeguards

RealmWeaver should not mechanically parse and validate every adjective or sentence produced by the AI.

That approach would create unnecessary:

* Latency
* Complexity
* Cost
* False positives

Instead, narration consistency should primarily rely on:

1. Strong structured authoritative context
2. Clear AI narration constraints
3. Prompt/tool contracts
4. Detection of obvious/high-risk contradictions where practical
5. Regeneration or correction when necessary

Detailed narration-validation architecture is deferred.

---

## 10.40 No Backdoor State Mutation

AI narration can never become a hidden path for authoritative state mutation.

Mechanically or persistently significant changes must use the appropriate lifecycle:

```text
INTENT
↓
PROPOSE
↓
VALIDATE
↓
RESOLVE
↓
COMMIT / PERSIST
↓
NARRATE
↓
CONTINUE WORLD
```

where applicable.

Displaying prose to the player does not automatically make the prose mechanically authoritative.

---

## 10.41 AI Narration Boundary Invariants

RealmWeaver adopts the following requirements:

1. AI owns narrative expression of committed outcomes.
2. Narration must respect authoritative mechanical and world state.
3. AI may embellish outcomes without creating mechanics.
4. Embellishment should avoid strongly implying nonexistent mechanical changes.
5. Mechanical terminology does not need to dominate narrative prose.
6. Important mechanical effects remain clearly accessible to the player.
7. Narration cannot independently grant resources.
8. Narration cannot independently remove resources.
9. Narration cannot independently apply mechanical conditions.
10. Narration cannot independently change meaningful position.
11. Narration cannot independently change authoritative world time.
12. Narration cannot independently complete quests.
13. Narration cannot independently grant XP or levels.
14. Failed checks are narrated according to their actual consequences rather than exaggerated failure.
15. Failed rolls should not unnecessarily portray competent characters as incompetent.
16. High rolls do not grant outcomes beyond what the mechanic permits.
17. Narration may reflect degree or margin where supported without inventing extra mechanical benefits.
18. Critical results may receive dramatic narration without unsupported additional effects.
19. AI does not invent meaningful player decisions.
20. Minor cinematic player motion consistent with expressed intent is allowed.
21. AI generally does not dictate the player character's internal thoughts or emotions.
22. Mechanical emotional effects may be narrated within their actual rules-defined scope.
23. AI has greater narrative freedom over AI-controlled NPCs.
24. Significant persistent NPC changes should become authoritative state.
25. AI may freely add non-conflicting incidental world detail.
26. Incidental detail and persistent world facts are distinct.
27. Incidental details may be materialised if they later become relevant.
28. Significant new lore requires the appropriate proposal/materialisation process.
29. AI may foreshadow hidden information without directly leaking it.
30. Narrative suspicion does not automatically establish mechanical truth.
31. Combat narration should use relevant authoritative mechanical context.
32. Combat narration should vary without changing mechanics.
33. Narration length should scale with event importance.
34. Narrative presentation and explicit mechanical feedback may coexist.
35. Relevant mechanical resolution should be inspectable by the player.
36. If narration contradicts authoritative state, authoritative state wins.
37. Narration consistency safeguards should remain lightweight rather than mechanically parsing every sentence.
38. Narration can never act as a backdoor authoritative state mutation.

---

# 11. 9G — NPC AI Authority

## 11.1 Status

**APPROVED**

---

## 11.2 Purpose

RealmWeaver treats relevant NPCs as autonomous AI-controlled actors while preserving deterministic mechanical authority.

The governing relationship is:

```text
AI:
Decides what an NPC wants to do and submits that intended action as a proposal.

RealmWeaver:
Determines whether the NPC can legally do it
and validates, resolves and commits/persists what mechanically happens.
```

NPC autonomy must remain constrained by:

* Authoritative NPC state
* Mechanical capabilities
* Knowledge
* Personality
* Goals
* Relationships
* Current circumstances
* World state
* Applicable rules

AI control of an NPC does not grant the AI authority over mechanical resolution.

---

## 11.3 NPCs as AI-Controlled Actors

Relevant NPCs may make autonomous decisions based on information such as:

* Goals
* Personality
* Relationships
* Knowledge
* Current situation
* Mechanical capabilities
* Faction or allegiance
* Fears
* Obligations
* Emotional state
* Tactical circumstances

The AI may determine the NPC's immediate intentions and behaviour within these constraints.

NPCs should behave as characters with motivations rather than as passive narrative objects.

---

## 11.4 NPC Decisions Remain Mechanical Proposals

AI-selected NPC actions are not mechanically authoritative until RealmWeaver validates them.

Example:

```text
AI chooses:
Guard Captain attacks player with Longsword

        ↓

RealmWeaver validates:
Actor may act?
Weapon available?
Target legal?
Range valid?
Action available?
Conditions permit action?
Required resources available?

        ↓

RealmWeaver resolves action
```

Therefore:

```text
NPC DECISION
≠
MECHANICAL SUCCESS
```

The AI chooses intent.

RealmWeaver validates and resolves execution.

---

## 11.5 Intent Versus Legality

The AI may choose actions such as:

> The goblin retreats.

> Mira attempts to persuade the magistrate.

> The cultist casts Hold Person.

RealmWeaver remains authoritative over:

* Movement legality
* Action availability
* Action economy
* Spell access
* Spell preparation
* Spell slots
* Resources
* Targets
* Range
* Equipment state
* Conditions
* Dice
* DCs
* Damage
* Healing
* Mechanical effects
* Resulting state

Personality and AI intent cannot bypass mechanical rules.

---

## 11.6 Persistent NPC Identity

NPCs that become continuity-relevant should retain sufficient authoritative state to remain consistent across future interactions.

Conceptually, persistent NPC state may eventually include:

```text
Identity
Role
Location
Faction
Goals
Personality
Relationships
Knowledge
Inventory
Currency
Mechanical profile
Conditions
Current plans
Relevant history
```

The exact domain model is deferred to later architecture.

The requirement is:

> **Once an NPC becomes continuity-relevant, RealmWeaver should retain sufficient authoritative state for future AI behaviour to remain consistent.**

---

## 11.7 Lightweight NPCs

Not every NPC requires a complete persistent character profile.

An incidental NPC may initially contain lightweight information such as:

```text
Name
Role
Location
Disposition
Relevant traits
```

Example:

```text
Random fisherman
↓
Player interacts
↓
NPC becomes scene-relevant
↓
Repeated or important interaction occurs
↓
NPC promoted/materialised
↓
Persistent NPC state created
```

RealmWeaver should support gradual promotion rather than requiring every background NPC to be fully simulated.

---

## 11.8 Existing NPC Mechanical Capabilities

Once an NPC has an authoritative mechanical profile, the AI cannot invent additional mechanical capabilities for convenience.

Example authoritative NPC:

```text
Profile:
Veteran Guard

Capabilities:
Longsword
Crossbow
Second Wind
```

The AI may not suddenly have that NPC:

* Cast Fireball
* Gain an unsupported feat
* Use a nonexistent magical item
* Gain additional movement
* Gain resistance or immunity
* Use unsupported legendary actions
* Use Weapon Mastery without an authoritative source

Mechanically relevant capabilities must originate from authoritative state.

---

## 11.9 Capabilities During NPC Creation

Before an NPC is fully materialised, the AI may propose an intended role or capability profile.

Example:

```text
AI Proposal:
Elite royal battlemage
```

RealmWeaver may then resolve that proposal into a supported mechanical profile.

Conceptually:

```text
Before Materialisation:
AI may propose capabilities

After Materialisation:
Authoritative NPC profile constrains capabilities
```

AI proposals remain subject to validation against supported game content.

---

## 11.10 Character-Appropriate Tactical Behaviour

NPC tactical behaviour should reflect the NPC rather than universally optimal play.

Relevant factors may include:

```text
Intelligence
Training
Experience
Fear
Goals
Personality
Knowledge
Risk tolerance
Current injuries
Available resources
Orders
Allies
Environment
```

A disciplined commander, frightened farmer, starving wolf, reckless warrior and experienced wizard should not make identical tactical decisions.

---

## 11.11 NPCs Need Not Choose the Mathematically Optimal Action

RealmWeaver should allow character-consistent decisions even when they are not mathematically optimal.

Examples:

* A proud knight refuses to retreat.
* A frightened bandit flees.
* A fanatic fights despite poor odds.
* A mercenary surrenders when continued fighting is no longer worthwhile.
* A loyal guard prioritizes protecting another NPC over dealing maximum damage.

RealmWeaver validates whether the chosen action is legal.

It should not force every NPC to behave like an optimal tactical solver.

---

## 11.12 Mechanical Restrictions Override Personality

Personality influences choices among legal possibilities.

It does not override game rules.

Example:

```text
Trait:
Reckless
```

does not allow an NPC to gain additional actions beyond the authoritative action economy.

The principle is:

> **Character behaviour influences legal choices; it does not create new mechanics.**

---

## NPC Knowledge

## 11.13 NPC Decisions Must Respect NPC Knowledge

NPCs should make decisions using information they reasonably possess.

An NPC's decision context may include:

* Personally observed facts
* Information previously learned
* Reports received from others
* Memories
* Known world facts
* Reasonable inference
* Suspicion

NPCs must not act on information they have no valid reason to possess.

---

## 11.14 AI/System Knowledge Is Not NPC Knowledge

The AI model may receive information needed to function as the DM that a particular NPC must not know.

Example system context:

```text
Secret:
Mira is secretly working for the Crown.
```

An unrelated guard must not act as if they know this simply because the AI model received the secret.

RealmWeaver therefore distinguishes:

```text
AI / DM CONTEXT
≠
NPC KNOWLEDGE
```

This is a hard information-boundary requirement.

Knowledge and information boundaries are expanded in Section 9I.

---

## 11.15 NPC Inference and Suspicion

NPCs may make reasonable deductions from information they genuinely possess.

Example:

A guard observes:

```text
Blood on player's sword
Broken nearby window
Player fleeing scene
```

The guard may reasonably suspect the player is involved.

This is not metagaming because the inference originates from observed information.

NPC reasoning may therefore distinguish:

```text
KNOWN FACT
OBSERVATION
REPORTED INFORMATION
MEMORY
INFERENCE
SUSPICION
```

---

## 11.16 Suspicion Is Not Certainty

NPC suspicion must not silently become authoritative certainty.

Example:

```text
NPC suspects:
Player is a thief
```

does not become:

```text
NPC knows:
Player is a thief
```

without sufficient evidence or discovery.

This distinction is important for:

* Investigation
* Social encounters
* Deception
* Mysteries
* Reputation
* Political intrigue

---

## 11.17 Knowledge Propagation

Information may propagate between NPCs through valid world events.

Example:

```text
Guard A witnesses crime
↓
Guard A reports incident
↓
Captain receives report
↓
Captain learns player's description
↓
Information may spread further
```

Where continuity requires it, knowledge propagation should become authoritative NPC/world state rather than existing only in generated prose.

Detailed communication, rumours and information-propagation architecture are deferred.

---

## NPC Social Behaviour

## 11.18 NPC Dialogue Authority

AI has broad authority over NPC social expression.

This may include:

* Dialogue
* Tone
* Mannerisms
* Bargaining
* Lies
* Evasiveness
* Humour
* Threats
* Negotiation
* Refusal
* Social reactions

These behaviours remain constrained by the NPC's:

* Knowledge
* Personality
* Relationships
* Goals
* Cultural context
* Current state
* Established history

---

## 11.19 NPC Deception

AI-controlled NPCs may:

* Lie
* Bluff
* Mislead
* Omit information
* Exaggerate
* Refuse to answer
* Provide partial truths

NPC dialogue is not required to represent objective world truth.

This allows deception, mystery and unreliable characters.

---

## 11.20 Deception Must Respect NPC Knowledge

NPC deception remains constrained by what the NPC knows or can plausibly fabricate.

An NPC may falsely claim:

> I never saw the duke.

even if they did.

However, an NPC with no knowledge of an undiscovered secret city should not produce detailed accurate information about that city unless there is a valid reason.

NPC fabrication must remain consistent with character context.

---

## 11.21 Persistent Relationships Influence NPC Behaviour

Persistent relationship state should influence AI decision-making.

A high-trust relationship may increase the likelihood that an NPC:

* Shares information
* Accepts personal risk
* Offers assistance
* Defends the player socially
* Accepts requests they would normally reject

Hostile relationships may influence behaviour in the opposite direction.

The exact relationship model and representation are deferred.

---

## 11.22 Relationships Do Not Remove NPC Agency

Positive relationships do not guarantee automatic compliance.

Even highly trusted or allied NPCs retain:

* Goals
* Ethics
* Fears
* Responsibilities
* Personal boundaries
* Risk tolerance
* Conflicting loyalties

NPCs should remain believable autonomous characters.

---

## NPC Combat Authority

## 11.23 AI Selects NPC Combat Intent

During combat, AI may select an NPC's intended action based on:

* Tactical situation
* Personality
* Intelligence
* Objectives
* Available resources
* Allies
* Injuries
* Battlefield state
* Knowledge
* Orders

RealmWeaver then validates and resolves the selected action.

---

## 11.24 Legal-Action Context

RealmWeaver should eventually provide AI with concise information about relevant legal or unavailable options where useful.

Conceptually:

```text
Available:
Longsword Attack
Heavy Crossbow
Shove
Dash
Disengage
Dodge

Unavailable:
Second Wind — already used
```

Providing constrained action context can reduce:

* Invalid AI proposals
* Hallucinated abilities
* Retry frequency
* Token usage
* Latency

Exact tool and API structures are deferred to later architecture.

---

## 11.25 Retreat, Surrender and Negotiation

NPC combat behaviour should not default to fighting until death.

Depending on context, NPCs may:

* Retreat
* Flee
* Surrender
* Negotiate
* Hide
* Call for help
* Defend another character
* Hold a position
* Escape with an objective
* Attempt a strategic withdrawal

AI determines the NPC's intent.

RealmWeaver validates the mechanical action.

---

## 11.26 No Required Universal Morale Stat

RealmWeaver V1 does not require a universal numerical morale system.

A value such as:

```text
Morale:
72
```

should not be invented merely to control NPC decisions unless a future rules system explicitly introduces it.

Initial retreat/surrender decisions may derive from authoritative/contextual information such as:

* Personality
* Goals
* Current HP
* Ally losses
* Leadership
* Fear conditions
* Scenario instructions
* Tactical circumstances

A formal morale subsystem may be considered separately in the future.

---

## 11.27 Objectives Beyond Killing

NPCs may pursue combat objectives other than defeating the player.

Examples:

* A thief escapes with an artifact.
* An assassin targets a specific individual.
* A guard protects an entrance.
* A cultist attempts to complete a ritual.
* A creature protects its offspring.
* Soldiers hold a bridge.
* A messenger attempts to escape and warn allies.

NPC combat intent should reflect actual goals rather than defaulting to attacking the nearest opponent.

---

## 11.28 Mechanical Combat Constraints

NPC tactical creativity remains constrained by authoritative mechanics.

Applicable restrictions include:

```text
Movement
Actions
Bonus Actions
Reactions
Range
Positioning
Equipment
Wield State
Spellcasting
Resources
Conditions
Targeting
Weapon Mastery
```

AI proposes what the NPC attempts.

RealmWeaver validates and resolves how that attempt occurs mechanically.

---

## NPC Resources and Persistence

## 11.29 Persistent NPC Resources

Mechanically significant resources belonging to continuity-relevant NPCs should persist.

Examples include:

```text
HP
Spell Slots
Inventory
Currency
Consumables
Charges
Conditions
Equipment
Limited-Use Abilities
```

Resources cannot regenerate merely because a new AI response is generated.

---

## 11.30 Persistent NPC Injuries and Recovery

Important NPC injuries should follow applicable healing, rest and world-time rules.

Example:

```text
NPC HP:
12 / 70
```

An NPC should not automatically return to full health shortly afterward unless:

* Healing occurred
* A valid rest occurred
* An applicable ability restored HP
* Another authoritative event changed the state

This follows Group 8 recovery rules.

---

## 11.31 Materialising Lightweight NPC Resources

RealmWeaver does not need to pre-generate every possession owned by every background NPC.

Resources may be materialised when interaction makes them relevant.

Example:

```text
Player attempts to pickpocket random merchant
↓
Merchant possessions become mechanically relevant
↓
RealmWeaver materialises plausible carried resources
↓
Validated resources become authoritative
```

These details must:

* Fit existing context
* Respect supported content
* Be validated where mechanically relevant
* Persist after materialisation

---

## 11.32 Materialised State Cannot Be Rerolled for Convenience

Once an NPC detail has become authoritative, it should not be regenerated simply because another outcome would be convenient.

Example:

```text
Merchant Currency:
23 GP
```

The same merchant should not later possess:

```text
90 GP
```

without an authoritative event explaining the change.

The governing principle is:

> **Once materialised, NPC state persists until changed through an authoritative event.**

---

## NPC Goals and Off-Screen Progression

## 11.33 Persistent NPC Goals

Important NPCs may have persistent goals.

Examples:

```text
Gain political influence
Find missing daughter
Hide involvement in conspiracy
Become guildmaster
Protect village
Defeat rival
Recover stolen artifact
```

AI behaviour should consider these goals during relevant decisions.

---

## 11.34 Off-Screen Goal Progression

Important NPC goals may continue progressing while the player is elsewhere.

Conceptually:

```text
Player leaves town
↓
World time advances
↓
Relevant NPC continues pursuing goal
↓
World state may change
```

Example:

A political rival may secure an alliance while the player spends several days travelling.

This follows RealmWeaver's living-world requirement established in Section 9E.

---

## 11.35 Selective Off-Screen Simulation

RealmWeaver should not continuously simulate every NPC in the world.

Meaningful off-screen progression should focus on currently relevant world elements, such as:

* Important NPCs
* Active quests
* Scheduled events
* Important NPC goals
* Active factions
* Player-related consequences
* Major conflicts

Background NPCs should not require continuous AI processing merely to simulate ordinary life.

This preserves scalability, cost and latency.

---

## 11.36 AI Proposals for Off-Screen Actions

AI/world systems may propose relevant off-screen NPC actions.

Example:

```text
NPC Goal:
Expose the duke

World Time:
Three days pass

Proposed Action:
NPC contacts an important witness
```

RealmWeaver validates/materialises any persistent consequence.

AI proposals do not directly rewrite world state.

---

## NPC Death and Removal

## 11.37 NPC Death Requires Authoritative State

AI narration cannot independently establish a significant NPC's death.

Example:

> Later that night, the captain dies.

If the death is meant to affect persistent world state, it must occur through an applicable:

* Mechanical resolution
* World event
* Validated content proposal
* Other authoritative state transition

---

## 11.38 NPC Death Persists

Once an NPC is authoritatively dead, generated narration must not casually reintroduce that NPC as alive.

An authoritative state change is required, such as:

* Valid resurrection
* Relevant transformation
* Undead mechanics
* Previously established mistaken identity
* Another explicitly supported world event

AI memory or improvisation cannot override authoritative life/death state.

---

## Allied NPCs and Controlled Creatures

## 11.39 Allied NPCs Retain Agency

Companions, allies, retainers and friendly NPCs normally remain autonomous unless rules explicitly state otherwise.

Player:

> Mira, stay here while I scout ahead.

Mira may:

* Agree
* Refuse
* Ask for clarification
* Suggest another approach
* React according to her goals and personality

Positive relationship state does not automatically transfer decision authority to the player.

---

## 11.40 Explicit Player Control

Some rules may explicitly grant the player direct control over another creature's actions.

Where authoritative mechanics grant such control:

```text
Rules grant player decision authority
```

that authority takes precedence over normal AI-controlled NPC autonomy.

RealmWeaver determines the correct controller from authoritative mechanics.

---

## NPC AI Failure Handling

## 11.41 Invalid NPC Proposals

Invalid NPC AI actions should normally be recovered internally.

Conceptually:

```text
AI NPC Proposal
↓
RealmWeaver validation
↓
INVALID
↓
Proposal rejected
↓
AI receives concise recovery context
↓
AI selects another action
```

The player normally does not need to see internal failed proposals.

---

## 11.42 Bounded NPC Decision Retries

RealmWeaver must prevent endless invalid NPC decision loops.

After a bounded number of unsuccessful proposals, later architecture should provide a safe recovery mechanism.

Possible conceptual fallback:

```text
Legal Actions:
Longsword Attack
Dash
Disengage
Dodge
```

The AI may be constrained to legal choices. If bounded AI recovery fails, RealmWeaver may use an appropriate safe deterministic fallback as an explicit failure-recovery exception to normal AI-owned NPC intent.

A deterministic fallback must be legal, conservative and limited to existing supported mechanics. It does not transfer ordinary NPC decision authority from AI to RealmWeaver.

Exact retry counts and fallback algorithms are deferred to Section 9K and later architecture.

---

## 11.43 NPC AI Authority Invariants

RealmWeaver adopts the following requirements:

1. Relevant NPCs are AI-controlled actors capable of autonomous decisions.
2. NPC decisions remain mechanical proposals until RealmWeaver validates them.
3. AI controls NPC intent; RealmWeaver controls mechanical legality and outcome.
4. Continuity-relevant NPCs should have persistent authoritative identity and state.
5. Lightweight NPCs are allowed and may later be promoted/materialised.
6. AI cannot invent new mechanical capabilities for already-materialised NPCs.
7. AI may propose capabilities during initial NPC creation, subject to validation.
8. NPC tactics should reflect intelligence, training, personality, goals, fear and context.
9. NPCs do not need to choose mathematically optimal actions.
10. Personality influences legal choices but never overrides mechanics.
11. NPC decisions must use information the NPC actually knows or can reasonably infer.
12. AI/system knowledge does not automatically become NPC knowledge.
13. NPCs may make reasonable inferences from known or observed information.
14. NPC suspicion remains distinct from certainty.
15. Knowledge may propagate through authoritative communication and world events.
16. AI has broad authority over NPC dialogue and social expression.
17. NPCs may lie, mislead, omit information and bluff.
18. NPC deception remains bounded by the NPC's knowledge and context.
19. Persistent relationships influence NPC behaviour.
20. Positive relationships do not remove NPC agency.
21. AI chooses NPC combat intent while RealmWeaver validates and resolves mechanics.
22. RealmWeaver should provide relevant legal-action context where useful to reduce invalid AI actions.
23. NPCs may retreat, surrender, negotiate, hide, call for help or pursue other appropriate responses.
24. RealmWeaver V1 does not require a universal numeric morale subsystem.
25. NPCs may prioritize objectives other than killing opponents.
26. NPC actions remain bound by action economy, positioning, resources and other mechanics.
27. Important NPC resources persist.
28. Important NPC injuries and recovery persist according to authoritative rules and time.
29. Lightweight NPC possessions/resources may be materialised when relevant.
30. Once materialised, NPC state cannot be rerolled or regenerated for convenience.
31. Important NPCs may have persistent goals.
32. Important NPC goals may progress off-screen as world time advances.
33. Off-screen simulation should be selective rather than continuously simulate every NPC.
34. AI may propose off-screen NPC actions; RealmWeaver validates/materialises consequences.
35. AI cannot create significant NPC death through narration alone.
36. Authoritative NPC death persists unless explicitly changed by valid mechanics or world events.
37. Allied NPCs normally retain agency unless rules explicitly grant the player control.
38. Directly player-controlled creatures follow authoritative control rules rather than default NPC autonomy.
39. Invalid NPC AI actions should normally be recovered internally.
40. NPC AI retries require bounded fallback behaviour.

---

# 12. 9H — World & Content Proposals

## 12.1 Status

**APPROVED**

---

## 12.2 Purpose

RealmWeaver allows AI to improvise and expand campaign content while preserving a coherent, persistent and authoritative world.

The AI may propose:

* Locations
* NPCs
* Factions
* Organisations
* Encounters
* Quests
* Items
* Treasure
* Lore
* Historical events
* Rumours
* Environmental details
* World-state changes

Generated content does not automatically become authoritative merely because it appeared in AI narration.

The governing principle is:

```text
AI CAN CREATE POSSIBILITIES.

REALMWEAVER DECIDES
WHAT BECOMES PERSISTENT REALITY.
```

---

## 12.3 World Content Proposal Lifecycle

Significant AI-generated world content should conceptually follow:

```text
AI GENERATES / PROPOSES CONTENT
        ↓
CLASSIFY SIGNIFICANCE
        ↓
VALIDATE AGAINST EXISTING WORLD
        ↓
MATERIALISE IF REQUIRED
        ↓
COMMIT / PERSIST
        ↓
AUTHORITATIVE WORLD STATE
        ↓
PRESENT AS ESTABLISHED REALITY
```

Generated prose alone does not automatically create canon.

Significant persistent NPCs, quests, promises, relationships, locations, rewards, injuries, deaths, statuses and consequential world facts must complete this lifecycle before they are presented as established reality.

Rumours, beliefs, allegations, predictions and uncertain information may be presented earlier only when clearly framed as claims. The committed fact is that the claim exists or was communicated, not that its contents are objectively true.

Temporary sensory and atmospheric flavour may remain non-materialised. If lightweight content gains persistent significance through player interaction, it must be materialised before gaining persistent authority.

---

## 12.4 Content Significance Levels

RealmWeaver conceptually distinguishes three levels of generated world content.

### Level 1 — Incidental Detail

Examples:

> Rainwater drips from a copper gutter.

> A faded red rug covers the floor.

> An empty mug rests beside the fireplace.

These details usually require no persistence.

### Level 2 — Scene-Relevant Content

Examples:

> A locked chest sits beneath the bed.

> Two guards stand outside the door.

> A horse is tied beside the inn.

These details may affect the current scene or mechanics.

They may require temporary or scene-level state.

### Level 3 — Persistent World Content

Examples:

> The city is ruled by Queen Elara.

> The Crimson Hand controls the eastern docks.

> A ruined fortress stands north of the capital.

> Captain Renn secretly works for the rebellion.

These facts may affect future continuity and should become authoritative persistent state.

---

## 12.5 Promotion Between Significance Levels

Content may become more significant through player interaction or story development.

Conceptually:

```text
INCIDENTAL
↓
Player interaction
↓
SCENE-RELEVANT
↓
Becomes continuity-relevant
↓
PERSISTENT
```

Example:

```text
Travelling musician appears in tavern
↓
Player starts conversation
↓
Musician becomes scene-relevant
↓
Repeated interactions occur
↓
Musician becomes player's informant
↓
Persistent NPC materialised
```

RealmWeaver should not require every generated detail to be persisted immediately.

---

## Authoritative Canon

## 12.6 RealmWeaver Owns Campaign Canon

Once a world fact becomes authoritative, RealmWeaver's stored state is the source of truth.

Example:

```text
Capital of Kingdom:
Arvendale
```

The AI may not later establish:

```text
Capital:
Greyspire
```

unless an authoritative world-state change or valid reinterpretation explains the difference.

---

## 12.7 Existing Canon Constrains New Generation

New generated content must respect relevant existing world state.

Example:

```text
Kingdom:
Eldoria

Current Ruler:
Queen Seraphine

Capital:
Valmere
```

AI should not casually establish:

> King Aldric rules Eldoria from Blackhaven.

unless this is deliberately represented as:

* Misinformation
* A rumour
* Historical information
* A claimant's assertion
* Another subjective belief
* A proposed world-state change

Existing authoritative canon constrains subsequent generation.

---

## 12.8 Contradictions Do Not Silently Rewrite Canon

If an AI proposal contradicts authoritative state, RealmWeaver must not silently overwrite existing facts.

Example:

```text
AI:
"The bridge was destroyed years ago."

Authoritative State:
Bridge is intact.
```

Possible responses include:

* Rejecting the proposal
* Correcting generation
* Reinterpreting it as a rumour
* Representing it as false NPC information where appropriate

Example reinterpretation:

> A traveller claims the bridge was destroyed years ago.

The claim may exist without changing the objective state of the bridge.

---

## Locations

## 12.9 Location Proposals

AI may propose locations such as:

* Settlements
* Cities
* Villages
* Ruins
* Dungeons
* Taverns
* Roads
* Forests
* Landmarks
* Buildings
* Districts
* Rooms
* Other explorable spaces

Location generation must respect relevant world constraints.

---

## 12.10 Persistent Locations

Locations that become important to exploration, travel, quests or continuity should be materialised.

Conceptual persistent location information may eventually include:

```text
Location ID
Name
Type
Region
Parent Location
Connections
Known Facts
Current State
Relevant Inhabitants
Discovery State
```

Exact location schemas are deferred.

---

## 12.11 Geographic Relationships

Geographic relationships that materially affect travel or continuity should become authoritative.

Example:

> Ashford lies two days east of Greyhaven.

Once established as meaningful geography, future AI generation should preserve that relationship unless the world itself changes or previous information is explicitly unreliable.

---

## Factions and Organisations

## 12.12 Faction Proposals

AI may propose organisations such as:

* Guilds
* Criminal organisations
* Noble houses
* Religious orders
* Armies
* Merchant groups
* Secret societies
* Political movements
* Other organised groups

---

## 12.13 Persistent Factions

Important factions should receive persistent authoritative state.

Conceptual faction information may include:

```text
Name
Type
Leadership
Goals
Allies
Enemies
Territory
Resources
Player Relationship
Current Plans
Known Information
```

Exact domain modelling is deferred.

---

## 12.14 Faction Relationships

Meaningful relationships between factions are authoritative world state.

Example:

```text
Faction A:
At war with Faction B
```

Such relationships may affect:

* NPC behaviour
* Encounters
* Quest generation
* Travel
* Politics
* Economy
* World events

They should not exist solely as temporary AI prose when they influence future gameplay.

---

## Quests

## 12.15 Quest Proposals

AI may recognize or generate quest opportunities.

Example:

> The merchant asks you to find his missing caravan.

This may create a quest proposal.

The AI does not directly establish authoritative quest state.

---

## 12.16 Structured Quest State

Once materialised, quests should use structured authoritative state.

Conceptual quest information may include:

```text
Quest
Status
Objectives
Relevant NPCs
Relevant Locations
Dependencies
Rewards
Completion Conditions
Failure Conditions
```

Exact schemas are deferred.

---

## 12.17 Quest Completion and Failure

AI narration cannot independently complete or fail quests.

Example:

```text
AI:
"You have completed the quest."
```

does not establish completion unless RealmWeaver determines that authoritative completion criteria were satisfied.

Likewise, AI cannot declare a quest failed because of an event that did not authoritatively occur.

---

## 12.18 Dynamic Quest Evolution

Quest objectives may evolve when authoritative world circumstances change.

Example:

Original objective:

```text
Rescue Captain Varek
```

Later authoritative state:

```text
Captain Varek escaped independently
```

The AI may propose an updated objective:

```text
Find Captain Varek
```

RealmWeaver validates and commits the structured quest-state change.

This allows quests to respond dynamically to open-world events.

---

## Encounters

## 12.19 Encounter Proposals

AI may propose encounters involving:

* Combat
* Social interaction
* Environmental hazards
* Exploration
* Discoveries
* Ambushes
* Puzzles
* Travel events
* Other campaign situations

Mechanically relevant encounters remain subject to the validation and resolution pipeline established earlier in Group 9.

---

## 12.20 Encounter Context

Encounter generation should respect relevant world context.

Factors may include:

* Biome
* Settlement type
* Local threats
* Faction control
* Character level
* Current story state
* Time
* Recent events
* Region
* World-generation profile
* Established inhabitants

Encounter generation should not introduce contextually nonsensical content without narrative justification.

---

## 12.21 No Universal Automatic Level Scaling

Established world threats should not automatically scale to the player's level.

If an ancient dragon exists in a region, the dragon does not become weak because a low-level character enters that region.

RealmWeaver may provide warnings or contextual clues where danger would reasonably be perceivable.

The world should retain meaningful differences in danger.

---

## 12.22 Appropriate Newly Generated Encounters

When generating new encounters, AI may favour content appropriate to:

* Current pacing
* Region
* Player capabilities
* Narrative circumstances
* Campaign goals

This does not permit AI to rewrite already-established creatures or locations simply to match the player's level.

---

## Items and Rewards

## 12.23 Item Proposals

AI may propose:

* Mundane equipment
* Treasure
* Weapons
* Armour
* Consumables
* Magic items
* Quest objects
* Other possessions

Mechanically usable items must resolve to supported structured RealmWeaver content before becoming authoritative.

---

## 12.24 Unsupported Item Mechanics

AI cannot independently create authoritative mechanics that are unsupported by RealmWeaver.

Example:

> Ring of Infinite Fireballs

does not automatically become mechanically usable.

A custom proposed item must be:

1. Mapped to existing supported content
2. Represented through supported structured effects
3. Rejected
4. Kept purely narrative/nonmechanical where appropriate

AI-generated prose cannot execute arbitrary rules.

---

## 12.25 Reward Validation

AI-proposed rewards must be validated before authoritative materialisation.

Validation may consider:

* Narrative context
* Quest significance
* Rarity
* Game balance
* World economy
* Duplication
* Existing ownership
* Supported game content
* Rules provenance

AI may propose reward intent.

RealmWeaver owns authoritative reward state.

---

## 12.26 Unique Items and Ownership

Unique items must remain unique once established.

Example:

```text
Item:
Crown of Ashes

Classification:
Unique Artifact

Current Holder:
Lady Selene
```

AI cannot independently generate another identical Crown of Ashes as unrelated loot.

Uniqueness, ownership and location are authoritative state.

---

## Lore and History

## 12.27 Historical and Cultural Lore

AI may propose:

* Historical battles
* Ancient rulers
* Myths
* Legends
* Dynasties
* Ruins
* Cultural practices
* Local traditions
* Historical conflicts
* Other worldbuilding information

This allows RealmWeaver's world to expand naturally.

---

## 12.28 Persistent Lore

Significant lore established during play should become persistent where continuity requires it.

Example:

> The Empire of Serath fell 430 years ago during the Ash War.

If this history may influence future NPC dialogue, exploration or quests, it should not depend solely on AI conversational memory.

---

## 12.29 Fact, Rumour, Myth, Belief and Propaganda

RealmWeaver should conceptually distinguish different information truth statuses.

Examples include:

```text
FACT
RUMOUR
MYTH
BELIEF
PROPAGANDA
UNKNOWN
```

Example:

```text
FACT:
The tomb exists.

MYTH:
The king buried there was immortal.

RUMOUR:
The tomb opened last week.
```

A statement existing within the world does not necessarily make the statement objectively true.

---

## 12.30 Conflicting Subjective Beliefs

Different NPCs, factions or cultures may hold conflicting interpretations.

Example:

```text
Empire:
"The rebellion was treason."

Rebels:
"The rebellion was liberation."
```

RealmWeaver should preserve subjective viewpoints rather than incorrectly forcing all perspectives into a single objective truth.

This is important for:

* Politics
* Culture
* Religion
* History
* Propaganda
* Mysteries

---

## Hidden World Content

## 12.31 Undiscovered Authoritative Facts

RealmWeaver may contain authoritative facts the player has not discovered.

Example:

```text
Hidden Temple:
Exists

Player Knowledge:
UNKNOWN
```

World truth and player knowledge are distinct.

This supports genuine discovery and hidden-world simulation.

---

## 12.32 Generated Secrets

AI may propose hidden facts such as:

> The magistrate secretly funds the smugglers.

If RealmWeaver validates and materialises the secret, the fact becomes authoritative world state.

It does not automatically become player knowledge.

Hidden-content access remains subject to the knowledge boundaries defined in Section 9I.

---

## Dynamic World State

## 12.33 World Entities May Change Over Time

Persistent world content is not immutable.

Examples:

```text
City:
Prosperous
↓
Besieged
↓
Occupied
↓
Rebuilt
```

```text
Faction:
Powerful
↓
Weakened
↓
Destroyed
```

```text
Road:
Open
↓
Flooded
↓
Repaired
```

Changes must occur through authoritative world events or validated state transitions.

---

## 12.34 World-State Change Proposals

AI or world-simulation systems may propose significant world-state changes.

Example:

> The rebellion captures Blackgate.

RealmWeaver may validate the proposal against:

* Existing world state
* Faction capabilities
* Current world time
* Active events
* Dependencies
* Previous outcomes
* Current conflicts

Only validated results become authoritative.

---

## 12.35 Causal Grounding

Significant world changes should normally have understandable causes.

Potential causes include:

* Active faction goals
* Scheduled events
* Player choices
* NPC actions
* Previous unresolved conflicts
* Environmental events
* Campaign-generation logic
* Rule effects

Major world changes should not occur purely because the AI requires novelty.

---

## Player Influence on Canon

## 12.36 Player-Created Canon

Player actions may create new authoritative content where appropriate.

Examples:

Player:

> I name this sword Dawnbringer.

RealmWeaver may make:

```text
Item Name:
Dawnbringer
```

authoritative.

Likewise, if the player founds:

> The Silver Company

RealmWeaver may materialise that organisation as persistent world content.

Player agency may therefore create canon through validated actions.

---

## 12.37 Player Claims Versus Objective Truth

A player's statement does not automatically rewrite objective world state.

Player:

> I tell everyone I am the king.

This may establish:

```text
Player Claim:
"I am the king."
```

It does not automatically establish:

```text
Objective World Fact:
Player is king
```

NPCs and factions may react to the claim according to context.

---

## Consistent Generation

## 12.38 Relevant World Context

New content generation should use relevant established world context.

When generating a new settlement, useful context may include:

```text
Region
Climate
Culture
Nearby Settlements
Political Control
Religion
Local Conflicts
Technology Level
World-Generation Profile
Relevant History
```

Context-aware generation should produce more coherent content than isolated generation.

---

## 12.39 Duplicate Identity Avoidance

RealmWeaver should avoid accidentally generating confusing duplicate identities where practical.

Example existing NPC:

```text
Lord Aric Venn
```

AI should not casually create another major NPC with exactly the same identity unless intentional.

Validation may eventually consider:

* Existing names
* Existing locations
* Existing factions
* Existing unique objects
* Relevant aliases

Exact duplicate-detection implementation is deferred.

---

## 12.40 Reuse Existing Content Before Unnecessary Generation

AI should prefer reusing suitable existing world content where appropriate.

Instead of continuously creating:

* New NPCs
* New factions
* New taverns
* New plot organisations
* New unresolved conflicts

the AI should consider whether existing campaign elements can fulfil the required role.

Examples:

* An existing faction becomes relevant again.
* A previously met NPC returns.
* An unresolved quest thread develops.
* A known location gains new significance.
* A prior political conflict produces consequences.

This improves continuity and makes the campaign world feel interconnected.

---

## 12.41 Appropriate New Content Generation

Reuse should not prevent genuine world expansion.

New content is appropriate when:

* The player explores a genuinely new region.
* Existing content cannot reasonably fulfil the required role.
* World logic requires new participants or locations.
* A world event creates new entities.
* Campaign generation intentionally expands the world.
* New content materially improves the campaign.

RealmWeaver should balance continuity with expansion.

---

## World-Generation Profiles

## 12.42 World-Generation Profile Constraint

Future campaign creation may support world-generation profiles inspired by:

```text
Greek-inspired settings
Indian mythology-inspired settings
Victorian / 1800s-inspired settings
Classic high fantasy
Dark fantasy
Custom profiles
```

Once selected, a world-generation profile should constrain or influence future content generation.

Relevant dimensions may include:

* Naming
* Architecture
* Government
* Clothing
* Mythology
* Social structures
* Geography
* Technology level
* Cultural flavour
* Settlement design
* Historical inspiration

Detailed world-generation architecture is deferred.

---

## 12.43 Cultural Inspiration Versus Direct Duplication

Historical and cultural profiles should normally act as inspiration/configuration rather than automatically becoming direct copies of real historical cultures.

Example:

```text
Greek-inspired
```

does not necessarily mean:

```text
Literal Ancient Greece
```

unless the campaign explicitly requests historical simulation.

This allows coherent fantasy cultures while retaining creative flexibility.

---

## Provenance

## 12.44 Content Creation Provenance

Significant persistent content should retain its origin where useful.

Possible conceptual provenance values include:

```text
CAMPAIGN_GENERATION
AI_DM
PLAYER_ACTION
QUEST_GENERATION
WORLD_EVENT
RULE_EFFECT
PREBUILT_CONTENT
```

Provenance can support:

* Debugging
* Consistency
* Content editing
* Auditing
* Understanding why state exists
* Future migration/versioning

Exact representation is deferred.

---

## 12.45 Mechanical Content Provenance

Mechanically defined content should retain sufficient provenance to identify its rules/content source.

Conceptual examples include:

```text
SRD_CC
REALMWEAVER_ORIGINAL
THIRD_PARTY_LICENSED
CAMPAIGN_CUSTOM
```

This applies especially to content such as:

* Spells
* Monsters
* Equipment
* Magic items
* Conditions
* Mechanical features
* Other rules-defined entities

Exact provenance classifications may be refined during the later licensing/content audit.

RealmWeaver should preserve provenance rather than losing source information during import or generation.

---

## Retcons and Revelation

## 12.46 No Silent Canon Retcon

Once a persistent fact becomes authoritative, AI should not silently contradict or replace it.

Example:

```text
Authoritative Fact:
Queen Elara has one son.
```

AI should not later casually narrate:

> Her three sons enter the chamber.

If authoritative information changes or earlier information was incomplete, the distinction must be represented deliberately.

---

## 12.47 Legitimate Recontextualization

Later revelations may reinterpret earlier information without constituting an invalid contradiction.

Example public belief:

```text
Prince Aldric is the king's biological son.
```

Hidden authoritative truth:

```text
Prince Aldric was secretly adopted.
```

A later revelation may expose the hidden truth.

This is valid because RealmWeaver distinguishes between:

* Objective fact
* Public belief
* NPC knowledge
* Rumour
* Hidden truth

Revelations may change what characters know without retroactively rewriting objective history.

---

## 12.48 World & Content Proposal Invariants

RealmWeaver adopts the following requirements:

1. AI may propose new world and campaign content.
2. AI-generated content is not automatically authoritative canon.
3. Content is conceptually classified as incidental, scene-relevant or persistent.
4. Content may be promoted between significance levels as it becomes relevant.
5. RealmWeaver owns authoritative campaign canon.
6. Existing canon constrains future AI generation.
7. Contradictory proposals cannot silently rewrite authoritative facts.
8. AI may propose new locations.
9. Significant locations should become persistent world entities.
10. Material geographic relationships should become authoritative.
11. AI may propose factions and organisations.
12. Important factions should have persistent authoritative state.
13. Meaningful faction relationships are world state.
14. AI may propose quests.
15. Materialised quests should use structured authoritative state.
16. AI cannot independently complete or fail quests.
17. Quest objectives may evolve through validated state changes.
18. AI may propose combat, social, exploration, environmental and other encounters.
19. Encounter generation should respect world and campaign context.
20. The world should not automatically scale all established threats to the player's level.
21. AI may favour appropriately challenging newly generated encounters without rewriting existing threat strength.
22. AI may propose items and treasure.
23. AI cannot invent unsupported authoritative item mechanics.
24. Rewards require validation for context, balance, rarity, economy and supported rules.
25. Unique items and established ownership remain authoritative.
26. AI may propose historical and cultural lore.
27. Significant established lore should persist.
28. RealmWeaver should distinguish objective fact, rumour, myth, belief, propaganda and unknown information.
29. Conflicting subjective beliefs may coexist without becoming contradictory objective facts.
30. The world may contain authoritative facts the player has not yet discovered.
31. Generated secrets remain subject to knowledge boundaries.
32. Persistent world entities may change over campaign time.
33. AI may propose world-state changes, but RealmWeaver validates and commits them.
34. Significant world changes should normally have causal grounding.
35. Player actions may create new authoritative world content.
36. Player claims do not automatically rewrite objective world state.
37. New content generation should use relevant existing world context.
38. Generated names and entities should avoid accidental duplication where practical.
39. AI should prefer reusing relevant existing world content over endlessly generating replacements.
40. New content should still be created when exploration or world logic genuinely requires it.
41. Future world-generation profiles should constrain coherent content generation.
42. Historical and cultural inspiration profiles should guide rather than necessarily duplicate real-world cultures.
43. Significant persistent content should retain creation provenance where useful.
44. Mechanically defined content should retain rules/content provenance.
45. Authoritative canon cannot be silently retconned.
46. Legitimate revelations may recontextualize earlier information when fact, belief, rumour and hidden truth are properly distinguished.

---

# 13. 9I — Knowledge & Information Boundaries

## 13.1 Status

**APPROVED**

---

## 13.2 Purpose

RealmWeaver distinguishes between objective world truth, actor-specific knowledge, AI/DM context and information visible to the player.

The governing principle is:

```text
WORLD TRUTH
≠
PLAYER CHARACTER KNOWLEDGE
≠
NPC KNOWLEDGE
≠
AI / DM CONTEXT
```

RealmWeaver may know a fact without every character knowing that fact.

The AI may receive hidden information when required to perform DM responsibilities, but receiving information does not grant permission to reveal it.

This boundary is essential for:

* Mysteries
* Secrets
* Deception
* Investigation
* Hidden creatures
* Traps
* Political intrigue
* Rumours
* Propaganda
* NPC motives
* Living-world information changes

---

## 13.3 World Truth Versus Actor Knowledge

RealmWeaver must distinguish:

```text
WHAT IS TRUE
```

from:

```text
WHO KNOWS OR BELIEVES WHAT
```

Example:

```text
WORLD TRUTH:
Lord Varen murdered the duke.

PLAYER CHARACTER:
Unknown.

Guard Captain:
Believes bandits killed the duke.

Mira:
Suspects Lord Varen.

Lord Varen:
Knows he committed the murder.
```

These states are not interchangeable.

Authoritative world truth does not automatically become actor knowledge.

---

## 13.4 Authoritative Objective Truth

Objective campaign facts remain RealmWeaver-owned.

Example:

```text
Secret Passage:
Exists behind library wall.
```

The passage exists regardless of whether:

* The player discovered it
* An NPC knows about it
* The AI mentioned it
* Anyone currently believes it exists

Character belief cannot overwrite objective world state.

---

## 13.5 Actor-Specific Knowledge

Important information may have different knowledge states for different actors.

Conceptually:

```text
FACT:
Smuggler entrance exists beneath warehouse.

Player:
UNKNOWN

Dockmaster:
KNOWN

City Guard:
SUSPECTED

Smugglers:
KNOWN
```

Exact knowledge schemas are deferred to later architecture.

RealmWeaver must support the principle that knowledge can be actor-specific.

---

## Information Truth and Claim Status

## 13.6 Information Classification

RealmWeaver should conceptually distinguish information categories such as:

```text
FACT
RUMOUR
MYTH
BELIEF
PROPAGANDA
UNKNOWN
```

Examples:

```text
FACT:
The tomb contains a sealed chamber.

RUMOUR:
A dragon sleeps inside.

MYTH:
The king buried there was immortal.

PROPAGANDA:
The rebels destroyed the temple.

BELIEF:
The priest believes the valley is cursed.
```

These classifications describe the nature or status of information.

They are separate from whether an actor has heard or believes the information.

---

## 13.7 Knowing a Claim Does Not Make It True

An actor may possess information that is objectively false.

Example:

```text
NPC BELIEF:
The bridge collapsed.
```

while:

```text
WORLD TRUTH:
Bridge remains intact.
```

RealmWeaver therefore distinguishes:

```text
ACTOR KNOWLEDGE / BELIEF
```

from:

```text
OBJECTIVE WORLD TRUTH
```

---

## 13.8 False, Partial and Unverified Information

Characters may possess information that is:

* False
* Partially true
* Outdated
* Misunderstood
* Unverified
* Deliberately misleading

Examples:

> The king died last night.

> The crypt contains treasure.

> The merchant is a spy.

RealmWeaver should not automatically correct an actor merely because the backend knows the objective truth.

---

## Player Knowledge

## 13.9 Player Character Knowledge

RealmWeaver primarily models the knowledge of the **player character** rather than attempting to control what the human player personally remembers.

Example:

```text
WORLD SECRET:
Mira is a spy.

PLAYER CHARACTER KNOWLEDGE:
UNKNOWN
```

The AI should not narrate:

> You approach Mira, knowing she is a spy.

unless the player character has legitimately discovered that information.

---

## 13.10 Prevent Hidden-Information Exposure

RealmWeaver should avoid exposing secret information through player-facing UI or narration and then asking the human player to ignore it.

Invalid design:

```text
NPC SECRET:
Member of Assassin Guild
```

displayed openly to the player.

The preferred design is:

> **Do not expose hidden information until the player character is entitled to receive it.**

---

## 13.11 Persistent Player Discoveries

Important information legitimately discovered by the player character should persist.

Example:

```text
Secret:
Mayor is a cult member.

Discovery:
Player finds signed cult correspondence.

Player Knowledge:
KNOWN
```

Future AI operations may then appropriately assume that the character knows the fact.

---

## 13.12 Knowledge Source and Time

Important knowledge may retain information about:

```text
Claim
Source
Time learned
Method learned
Verification state
```

Example:

```text
Claim:
Captain Renn left for Blackgate.

Source:
Innkeeper

Learned:
Day 12, 18:20

Status:
Unverified
```

Exact representation is deferred.

---

## Stale and Outdated Knowledge

## 13.13 Knowledge May Become Outdated

Living-world changes may make previously accurate information stale.

Example:

Day 3:

```text
Player learns:
Eastern bridge is open.
```

Day 7:

```text
World Event:
Flood destroys eastern bridge.
```

If the player has received no new information:

```text
WORLD TRUTH:
Bridge destroyed.

PLAYER BELIEF:
Bridge believed open.
```

This is valid and not a contradiction.

---

## 13.14 Narration Uses Available Knowledge

Player-facing AI narration should reflect information legitimately available to the character.

Instead of:

> The eastern bridge was destroyed yesterday.

when the character has no way to know that, AI may say:

> The last you heard, the eastern bridge was still open.

Current omniscient world state must not automatically leak into character-facing narration.

---

## Observation and Discovery

## 13.15 Direct Observation

Obvious direct observation may create actor knowledge automatically.

Example:

```text
Player opens vault
↓
Vault visibly empty
↓
Player learns:
Vault is empty
```

AI does not need to invent a separate discovery mechanic for information that is plainly observable.

---

## 13.16 Mechanically Hidden Information

Some facts may require applicable mechanics such as:

* Perception
* Investigation
* Insight
* Arcana
* History
* Religion
* Other supported checks

Example:

```text
Scene Truth:
Small bloodstain beneath desk.

Player:
Searches room.

RealmWeaver:
Investigation resolution required.
```

Successful resolution may reveal the relevant information.

---

## 13.17 Failed Checks Must Not Leak Secrets

Failed checks must not reveal the thing that was missed.

Avoid:

> You fail to notice the secret compartment.

The phrase itself reveals the secret.

Prefer:

> You search the desk carefully but find nothing else of interest.

The system may know what was missed.

The player character does not.

---

## 13.18 Hidden/System Rolls

RealmWeaver may use hidden/system rolls where asking the player to roll would itself expose information.

Example:

```text
Player passes beneath hidden assassin
↓
RealmWeaver secretly resolves Perception
```

Failure:

> The alley seems quiet.

Success:

> A faint scrape sounds from the rooftop above.

This follows the hidden-roll principles established in Section 9E.

---

## Partial Knowledge

## 13.19 Partial Revelation

Knowledge need not be binary.

Example world truth:

```text
The Black Concord ordered the assassination
through Captain Veyr,
who hired three local killers.
```

The player may discover information incrementally:

```text
Stage 1:
Someone paid the killers.

Stage 2:
Captain Veyr arranged payment.

Stage 3:
The Black Concord issued the order.
```

RealmWeaver should conceptually support progressive revelation.

---

## 13.20 Unknown Information Must Remain Unknown

AI must not fill undiscovered gaps with unsupported facts.

Example player knowledge:

```text
The assassin met someone at the harbour.
```

If the identity is still unknown, AI cannot later state:

> You remember that the assassin met Captain Veyr.

unless that information was legitimately discovered.

---

## NPC Knowledge

## 13.21 NPC-Specific Context

When AI acts for an NPC, decision context should be scoped to information that NPC legitimately possesses.

Relevant inputs may include:

* NPC knowledge
* NPC memories
* NPC observations
* NPC goals
* Relevant public world information
* Reasonable inferences

The acting NPC should not automatically receive every campaign secret.

---

## 13.22 Information Transfer Between Actors

NPCs may transfer information to other actors.

Example:

Mira tells the player:

> The eastern gate closes at midnight.

RealmWeaver may establish:

```text
Player learned:
Eastern gate closes at midnight.

Source:
Mira.
```

The claim's objective truth status remains separate from the fact that Mira communicated it.

---

## 13.23 Intentional False Information

NPCs may deliberately provide false information.

Example:

```text
WORLD TRUTH:
Treasure beneath old chapel.

NPC KNOWLEDGE:
Knows true location.
```

NPC lies:

> It is buried in the graveyard.

Player may then hold:

```text
Claim:
Treasure is in graveyard.

Source:
NPC.

Verification:
Unverified.

Objective Truth:
False.
```

The backend may know the statement is false without revealing that fact to the player.

---

## Insight and Deception

## 13.24 Insight Is Not Mind Reading

Successful Insight does not automatically reveal arbitrary hidden facts.

If an NPC lies, a successful check may reveal:

* Suspicious behaviour
* Emotional cues
* Inconsistency
* Possible deception
* Unusual hesitation

It should not automatically reveal the NPC's complete secret history or true motive unless the mechanic and context justify that information.

---

## 13.25 Failed Insight Does Not Confirm Truth

A failed Insight check normally means the character gained no useful additional understanding.

It does not automatically establish:

```text
NPC statement = TRUE
```

Failure to detect deception is not proof that deception is absent.

---

## 13.26 Deception Changes Belief, Not Truth

Successful deception affects what an actor may believe.

It does not rewrite objective world state.

Conceptually:

```text
SUCCESSFUL DECEPTION
↓
Actor belief may change
```

not:

```text
SUCCESSFUL DECEPTION
↓
Objective reality changes
```

---

## Secrets

## 13.27 Secret Access Boundaries

Important secrets may have scoped access.

Example:

```text
SECRET:
Queen is terminally ill.

KNOWN BY:
Queen
Royal Physician
Royal Adviser

UNKNOWN TO:
Public
Player
Most Palace Staff
```

This knowledge distribution may later change through authoritative events.

---

## 13.28 Secret Discovery

Secret information may become known through:

* Investigation
* Confession
* Direct observation
* Stolen documents
* Magical effects
* Rumours
* Intercepted communication
* World events
* Other supported mechanics

RealmWeaver updates knowledge when discovery legitimately occurs.

---

## 13.29 Knowledge Does Not Automatically Become Public

If the player learns:

```text
King is dying.
```

that does not automatically mean:

```text
Entire city knows king is dying.
```

Knowledge remains scoped to actors or audiences.

The player may later choose to share or conceal the information.

---

## Information-Bearing Objects

## 13.30 Persistent Information Sources

Physical objects may contain information independently of whether anyone has read them.

Examples:

* Letters
* Journals
* Books
* Maps
* Posters
* Inscriptions
* Contracts
* Notes

Conceptually, a document may eventually include:

```text
Identity
Author
Recipient
Content / summary
Creation date
Authenticity state
Information claims
Current holder
```

Exact schema is deferred.

---

## 13.31 Possession Versus Reading

Possessing an object does not automatically grant knowledge of its contents.

Example:

```text
Player finds sealed letter.

Player knows:
Letter exists.
```

Until opened/read:

```text
Player does not necessarily know:
Letter contents.
```

Reading or otherwise accessing the content may create new knowledge.

---

## Location Knowledge

## 13.32 Existence and Position Are Separate

A character may know:

```text
Temple of Vaelor exists.
```

without knowing exactly where it is.

Location knowledge may conceptually progress:

```text
Unknown
↓
Existence known
↓
Approximate region known
↓
Exact location discovered
```

---

## 13.33 Navigation Respects Discovery

An undiscovered location should not automatically appear as a player-facing travel destination merely because RealmWeaver stores it.

Navigation and map interfaces should respect applicable discovery/knowledge state.

---

## Mechanical Information

## 13.34 Hidden Mechanical State

Not all authoritative mechanical information needs to be shown directly to the player.

RealmWeaver may know:

```text
Enemy HP:
36 / 72
```

while presenting:

> The ogre is badly wounded.

Likewise, exact enemy:

* AC
* Spell slots
* Remaining resources
* Hidden abilities
* Resistances
* Other private mechanics

may remain undisclosed until appropriate.

---

## 13.35 Player-Owned Mechanical Transparency

The player should generally have access to their own clearly knowable mechanical state, including:

* HP
* AC
* Spell slots
* Conditions
* Equipment
* Carried resources
* Currency
* Active effects
* XP/progression where applicable

Specific supported effects may create exceptions.

---

## 13.36 Discoverable Enemy Mechanics

Enemy properties may become player-character knowledge through play.

Example:

Repeated fire attacks produce reduced effects.

The character may learn:

> This creature appears resistant to fire.

That discovered information may persist for future encounters.

---

## DM-Level AI Context

## 13.37 DM Access to Hidden Information

The AI acting as DM may need hidden information to portray scenes correctly.

Example:

```text
OBJECTIVE SECRET:
Bartender is a cultist.
```

The AI may need this information to produce subtle behaviour.

The information should remain classified:

```text
DM-AVAILABLE
PLAYER-HIDDEN
```

---

## 13.38 No Hidden-Context Leakage

Hidden DM information must not automatically leak through narration.

This includes:

* Traps
* Enemy plans
* Hidden NPC identities
* Undiscovered locations
* Concealed creatures
* Secrets
* Private NPC knowledge
* Scheduled events
* Future plans

Reveal requires legitimate discovery or intentional authorized disclosure.

---

## 13.39 Explicit Knowledge Labels

AI context should conceptually distinguish information scopes.

Instead of ambiguous input:

```text
Mira works for the Crown.
```

prefer conceptually:

```text
OBJECTIVE_SECRET:
Mira works for the Crown.

PLAYER_CHARACTER:
UNKNOWN.

MIRA:
KNOWN.

OTHER NPCs:
Scoped individually.
```

Exact prompt and data structures are deferred.

---

## Future Information

## 13.40 Scheduled Events Are Not Character Knowledge

A future event existing in RealmWeaver's scheduler does not automatically make that event known.

Example:

```text
Scheduled Event:
Assassins attack at midnight.
```

Possible state:

```text
AI DM:
May know.

Assassins:
Know.

Player:
Unknown.
```

---

## 13.41 Prophecy and Prediction

Predictions and prophecies should normally remain claims rather than guaranteed future truth.

Example:

> The city will fall before winter.

may be classified as:

```text
PROPHECY / PREDICTION / CLAIM
```

If a campaign deliberately defines a prophecy as an authoritative fixed event, that must be represented separately.

---

## Belief Correction

## 13.42 Beliefs May Be Corrected

Actor knowledge may evolve as evidence appears.

Example:

Initial belief:

```text
Captain Renn killed the merchant.
```

Later evidence proves:

```text
Captain Renn was elsewhere.
```

Knowledge may change conceptually:

```text
BELIEVED
↓
DISPROVEN
```

or an equivalent future state.

---

## 13.43 Knowledge Change Is Not World Retcon

Changing what an actor believes does not automatically change what objectively happened.

This distinction supports:

* Mysteries
* False accusations
* Propaganda
* Unreliable narrators
* Misunderstandings
* Evolving investigations

without repeatedly rewriting campaign history.

---

## 13.44 Knowledge & Information Boundary Invariants

RealmWeaver adopts the following requirements:

1. Objective world truth and actor knowledge are separate.
2. RealmWeaver owns authoritative objective truth.
3. Knowledge may be actor-specific.
4. Information may be classified as fact, rumour, myth, belief, propaganda or unknown.
5. Knowing a claim does not establish that the claim is objectively true.
6. Characters may hold false, partial, outdated or unverified information.
7. RealmWeaver primarily models player-character knowledge rather than attempting to control the human player's memory.
8. Hidden information should be prevented from reaching player-facing surfaces rather than relying on the player to ignore it.
9. Important player-character discoveries persist.
10. Important knowledge may retain source and acquisition time.
11. Previously accurate knowledge may become stale as the living world changes.
12. AI narration should respect what the character currently knows rather than omniscient current truth.
13. Direct observation may create knowledge.
14. Hidden information may require applicable checks or mechanics.
15. Failed checks must not leak what was hidden.
16. Hidden/system rolls may protect information boundaries.
17. Information may be partially revealed rather than binary known/unknown.
18. AI cannot fill undiscovered gaps with authoritative facts.
19. NPC AI context should be scoped to the NPC's legitimate knowledge.
20. NPCs may transfer claims and information to other actors.
21. NPCs may intentionally provide false information.
22. Insight does not function as unrestricted mind reading.
23. Failed Insight does not establish that a statement is true.
24. Successful deception changes actor belief, not objective world truth.
25. Important secrets may have explicit knowledge/access boundaries.
26. Secret access may change through valid discovery.
27. Revealing a secret to one actor does not automatically make it public.
28. Physical objects may persistently contain information.
29. Possessing an information-bearing object does not automatically mean its contents were read or understood.
30. Knowing a location exists and knowing its position are separate states.
31. Undiscovered locations should not automatically appear in player-facing navigation.
32. RealmWeaver may retain hidden mechanical state not directly exposed to the player.
33. Player-owned mechanical state should generally remain transparent.
34. Enemy mechanical properties may become discovered knowledge through play.
35. AI DM may receive hidden facts required to portray the world correctly.
36. Hidden DM context must not automatically leak through narration.
37. AI context should explicitly distinguish objective truth, secrets and actor-specific knowledge.
38. Scheduled future events are not automatically character knowledge.
39. Prophecy and prediction are not guaranteed future truth unless the campaign explicitly defines them as such.
40. Actor beliefs may be corrected or disproven as new evidence appears.
41. Changing knowledge or belief does not itself retcon objective world truth.

---

# 14. 9J — Context & Memory Boundary

## 14.1 Status

**APPROVED**

---

## 14.2 Purpose

RealmWeaver must support campaigns that remain coherent across:

* Long play sessions
* Multiple sessions
* Application restarts
* Large campaign histories
* Model context-window limits
* AI model upgrades
* AI provider changes

The AI must therefore not function as RealmWeaver's database or authoritative long-term memory.

The governing principle is:

```text
THE AI SHOULD NOT BE THE DATABASE.

REALMWEAVER STORES THE CAMPAIGN.

THE AI RECEIVES ONLY THE CONTEXT
REQUIRED FOR THE CURRENT TASK.
```

---

## 14.3 Authoritative State Is Not AI Memory

RealmWeaver must not rely on the AI remembering authoritative state from earlier conversation.

Examples that belong in RealmWeaver persistence include:

* Character HP
* Inventory
* Currency
* Spell slots
* Conditions
* NPC state
* Relationships
* Quests
* Locations
* World time
* World facts
* Player discoveries
* Important world events

The AI receives relevant state when required.

---

## 14.4 Conversation History Is Not Authoritative State

Raw conversation history is not the source of truth for persistent campaign information.

Example:

If an earlier narration established an important persistent fact:

> Mira lost her left eye during the siege.

and that fact matters to continuity, it should be materialised.

Future correctness should not depend on the AI rereading an old conversation transcript.

Therefore:

```text
CHAT HISTORY
≠
AUTHORITATIVE CAMPAIGN STATE
```

---

## 14.5 Context as a Temporary View

AI context should be treated as a temporary assembled view of relevant RealmWeaver information.

Conceptually:

```text
AUTHORITATIVE CAMPAIGN STATE
        +
RELEVANT MEMORY
        +
CURRENT SCENE
        +
PLAYER INPUT
        +
RELEVANT RULE / TOOL CONTEXT
        ↓
AI CONTEXT PACKAGE
```

The AI operates on that temporary package.

It does not need direct access to every campaign record.

---

## Context Layers

## 14.6 Layered Context

AI context should conceptually distinguish information such as:

```text
SYSTEM / GAME CONTRACT
CAMPAIGN CONFIGURATION
CURRENT SCENE
PLAYER CHARACTER STATE
RELEVANT NPC STATE
RELEVANT WORLD STATE
RELEVANT KNOWLEDGE
RELEVANT HISTORY / MEMORY
PLAYER INPUT
MECHANICAL RESULT
```

Different AI operations may receive different combinations.

---

## 14.7 Stable Versus Dynamic Context

Stable instructions differ from changing campaign state.

Stable context may include:

* Authority boundaries
* Narration constraints
* Knowledge rules
* Output contracts
* Campaign-level configuration

Dynamic context may include:

* Current HP
* Current scene participants
* Current world time
* Conditions
* Current quest state
* Current inventory
* Recent dialogue

RealmWeaver should conceptually distinguish these categories.

---

## 14.8 Task-Specific AI Context

Different AI operations should receive context appropriate to their task.

### Intent Interpretation

May require:

* Player input
* Immediate scene
* Nearby entities
* Relevant mechanical options

### NPC Decision

May require:

* NPC goals
* NPC knowledge
* Tactical state
* Available resources
* Legal/relevant actions

### Narration

May require:

* Committed mechanical result
* Scene context
* Involved characters
* Relevant narrative history
* Tone information

### World Content Generation

May require:

* Region
* Existing canon
* World-generation profile
* Relevant factions
* Existing content that may be reused

Task-specific context reduces leakage, cost and confusion.

---

## Minimum Sufficient Context

## 14.9 Minimum Sufficient Context Principle

Every AI operation should receive:

> **The minimum sufficient context required to perform its task accurately.**

RealmWeaver should avoid both extremes:

```text
Entire campaign sent every turn.
```

and:

```text
Insufficient information that forces AI guessing.
```

---

## 14.10 More Context Is Not Automatically Better

Excessive context may increase:

* Latency
* Token usage
* Cost
* Distraction
* Contradictory information
* Hallucination risk
* Hidden-information leakage

RealmWeaver should prioritize relevance over volume.

---

## 14.11 Correctness Overrides Token Optimisation

Required authoritative information must not be omitted merely to reduce tokens.

Example:

```text
NPC Condition:
BLINDED
```

If omitted from NPC combat context, AI may choose actions that are inconsistent with state.

Likewise, important discovered knowledge relevant to a conversation must not disappear purely for prompt-size optimisation.

Correctness takes priority over token savings.

---

## Memory Categories

## 14.12 Memory Types

RealmWeaver should conceptually distinguish different kinds of persistent information.

### Mechanical State

Exact authoritative values.

```text
HP = 31
Gold = 187 GP
```

### World Facts

```text
Queen Elara rules Greyhaven.
```

### Knowledge State

```text
Player knows the captain lied.
```

### Relationship / Social State

```text
Mira strongly trusts player.
```

### Event History

```text
Player saved Mira at Blackgate.
```

### Narrative Memory

Concise summaries of continuity-relevant prior interactions.

Exact storage architecture is deferred.

---

## 14.13 Mechanical State Remains Structured

Critical mechanics must not exist only in prose summaries.

Invalid authoritative representation:

> The player was badly injured after the battle.

Required structured state:

```text
HP:
13 / 47
```

Narrative summaries may exist alongside structured mechanical state.

They may not replace it.

---

## 14.14 Structured Persistent Facts

Important facts should be represented structurally where practical when they materially affect future behaviour.

Example:

Instead of relying solely on:

> Mira likes the player.

RealmWeaver may eventually retain:

* Structured relationship state
* Relevant historical events
* Narrative explanation

Exact models are deferred.

---

## Event Memory

## 14.15 Not Every Event Requires Long-Term Memory

A campaign may contain thousands of actions.

Low-value events such as:

* Opening an ordinary door
* Drinking an ordinary ale
* A routine enemy miss
* Walking between nearby rooms

do not automatically require high-priority persistent memory.

Long-term retention should be driven by future continuity value.

---

## 14.16 Significant Events Should Remain Recoverable

Examples of potentially significant events include:

* Important NPC death
* Betrayal
* Quest completion
* Promise
* Faction alliance
* Major secret discovery
* Important player decision
* Major battle
* Major world-state change
* Acquisition or loss of a unique item

Such events should remain recoverable for future context.

---

## 14.17 Memory Importance May Change

An initially minor event may later become meaningful.

Example:

Session 2:

> Player gives a beggar 5 GP.

Initially low significance.

Session 20:

That same character becomes an important revolutionary leader.

The earlier event may now be relevant.

RealmWeaver should allow historical information to gain relevance later.

---

## Summarisation

## 14.18 Long History Should Be Summarized

RealmWeaver should not repeatedly send entire historical conversation transcripts.

It may maintain summaries such as:

```text
RECENT SCENE SUMMARY
SESSION SUMMARY
NPC RELATIONSHIP SUMMARY
QUEST HISTORY SUMMARY
MAJOR CAMPAIGN SUMMARY
```

Exact summary hierarchy is deferred.

---

## 14.19 Summaries Cannot Override Authoritative State

If a summary states:

> Mira is injured.

but current authoritative state says:

```text
Mira HP:
Maximum
```

the current structured state wins.

Summaries are contextual aids, not authority.

---

## 14.20 Summaries Should Preserve Causality

Useful summaries should preserve why important state exists.

Weak:

> Duke dislikes the player.

Better:

> Duke distrusts the player after the player publicly accused him of financing smugglers during the council meeting.

Causal information improves future AI behaviour.

---

## 14.21 Summaries Must Not Invent Facts

Summarisation compresses existing information.

It does not create new canon.

Example history:

> Mira hesitated before answering.

Invalid summary:

> Mira secretly hates the player.

unless supported by authoritative state or validated events.

---

## Context Retrieval

## 14.22 Retrieve Historical Information When Relevant

RealmWeaver should retrieve relevant historical context rather than loading everything every turn.

Conceptually:

```text
Current interaction
↓
Identify relevant entities/topics
↓
Retrieve relevant history/state
↓
Assemble task context
```

Example:

When speaking with Mira, retrieve relevant:

* Mira state
* Relationship
* Knowledge
* Goals
* Promises
* Recent interactions
* Relevant quest involvement

Do not automatically retrieve unrelated history for every other campaign NPC.

---

## 14.23 Retrieval Signals

Later memory architecture may consider:

* Entity identity
* Location
* Active quest
* Current topic
* Recency
* Importance
* Causal relevance
* Semantic similarity
* Current scene

No single retrieval mechanism is locked during Group 9.

---

## 14.24 Entity-Linked Retrieval

Semantic similarity alone should not be the only retrieval mechanism for persistent campaign entities.

If the current NPC is:

```text
NPC_ID:
MIRA_001
```

RealmWeaver should be able to reliably retrieve state/history linked directly to Mira.

Later memory architecture should therefore support deterministic/entity-linked retrieval alongside semantic retrieval where useful.

---

## 14.25 Recent Context

Long-term memory retrieval does not replace immediate conversational context.

Example:

Player asks:

> Why did you do that?

The AI may require recent dialogue to identify what "that" refers to.

Context assembly should therefore combine:

```text
RECENT INTERACTION
+
RELEVANT LONG-TERM MEMORY
+
AUTHORITATIVE STATE
```

---

## Memory Versus Authority

## 14.26 Narrative Memory Is Supporting Context

Retrieved narrative memory is evidence/context rather than authoritative state.

Example memory:

> Mira once said her brother died at Blackgate.

Authoritative state:

```text
Mira's Brother:
Alive
```

Possible explanations include:

* Mira lied
* Memory refers to a rumour
* Memory is stale
* Later correction occurred

RealmWeaver must not overwrite authoritative state solely because an old narrative memory says otherwise.

---

## 14.27 Conflict Priority

Context conflicts should resolve toward authoritative RealmWeaver state.

Conceptually:

```text
CURRENT / VERSIONED AUTHORITATIVE STATE AND FACTS
        >
VALIDATED AUTHORITATIVE EVENT HISTORY
        >
MEMORY SUMMARIES
        >
RAW OLD AI NARRATION
```

Structured current state and structured persistent facts are both authoritative RealmWeaver state. Validated event history remains authoritative history, but it does not overwrite a later valid state transition concerning the same fact.

This expresses authority direction and temporal precedence rather than locking a specific implementation.

---

## Session and Provider Independence

## 14.28 Session Persistence

Campaign continuity must survive application and conversation boundaries.

The player should be able to:

```text
Play campaign
↓
Close RealmWeaver
↓
Return later
↓
Resume campaign
```

without continuity depending on an old LLM session remaining open.

---

## 14.29 AI Provider Independence

Campaign memory belongs to RealmWeaver.

Changing:

```text
Provider A
↓
Provider B
```

must not destroy campaign continuity.

Likewise, upgrading or replacing an AI model should not remove persistent campaign state.

---

## 14.30 Campaign Length Is Independent of Model Context Window

A model's context limit must not define the maximum length of a RealmWeaver campaign.

Campaign history should be externalized through:

* Structured persistence
* Event history
* Summaries
* Retrieval
* Context assembly

Therefore:

```text
CAMPAIGN LENGTH
≠
LLM CONTEXT WINDOW
```

---

## Player Corrections

## 14.31 Player Memory Corrections

If the player states:

> Mira already told me about the ring several sessions ago.

RealmWeaver should be able to inspect relevant authoritative/history state and correct assembled context where appropriate.

The AI should not treat summaries as infallible.

---

## 14.32 Player Claims Cannot Directly Rewrite State

Natural-language claims cannot directly modify authoritative state.

Example:

> Actually, I had 5,000 gold.

does not automatically overwrite:

```text
Gold:
300 GP
```

Changes to authoritative state require appropriate validation, verification or an explicit supported correction/debug workflow.

---

## AI Memory Proposals

## 14.33 AI May Propose Memory Updates

AI may identify continuity-relevant information.

Example:

```text
MEMORY PROPOSAL:
Mira promised to protect the player's sister.
```

RealmWeaver determines whether and how the information should be persisted.

AI does not directly own long-term memory state.

---

## 14.34 Deterministic State Does Not Depend on Memory Proposals

If RealmWeaver already knows:

```text
Quest:
COMPLETED
```

that result should automatically update relevant structured state/event history.

The system should not require the AI to separately say:

> Remember that the quest was completed.

---

## 14.35 AI Omission Cannot Delete State

A fact remains authoritative even when it is omitted from a particular AI prompt.

Likewise, information must not disappear merely because it was absent from a generated summary.

Context omission and state deletion are different operations.

---

## Compression and Retention

## 14.36 Low-Value History May Be Compressed

Old low-value narrative history may be summarized or compressed to improve scalability.

Compression must preserve important:

* Consequences
* Persistent facts
* Relationships
* Promises
* Active quests
* World changes
* Discoveries
* Major decisions

---

## 14.37 Compression Cannot Destroy Authoritative State

Converting:

```text
100 dialogue messages
```

into:

```text
Conversation summary
```

must not remove structured facts established by those interactions.

Authoritative state remains independently persisted.

---

## 14.38 Raw History May Be Retained Separately

Where practical, RealmWeaver may retain raw:

* Event logs
* AI narration
* Dialogue
* Mechanical resolution history

for:

* Debugging
* Player history
* Advanced retrieval
* Auditing
* Replay

The AI does not need raw history on every request.

Detailed retention policies are deferred.

---

## Context Safety

## 14.39 Knowledge Filtering During Context Assembly

Retrieval does not automatically grant permission to reveal information.

Conceptually:

```text
Relevant history retrieved
↓
Contains player-hidden secret
↓
Target operation:
Player-facing narration
↓
Apply knowledge boundary
↓
Secret remains hidden
```

Context assembly must enforce Section 9I knowledge rules.

---

## 14.40 AI Role/Task Scoping

The same underlying AI model may perform different logical roles:

```text
Intent Interpreter
NPC Decision Maker
DM Narrator
World Content Generator
```

Each operation should receive context appropriate to its role.

For example, an NPC decision operation should not automatically receive private knowledge belonging only to unrelated NPCs.

---

## 14.41 Controlled AI Operations

Even if a single LLM provider performs several functions, RealmWeaver should treat those functions as separate controlled operations.

Conceptually:

```text
TASK
↓
ASSEMBLE APPROPRIATE CONTEXT
↓
AI OPERATION
↓
VALIDATE OUTPUT
```

This reduces cross-role leakage and improves observability.

---

## Memory Update Timing

## 14.42 Mechanical Memory Follows Commitment

Memory representing mechanically authoritative events should be created from committed results.

Examples include:

* Combat results
* Item acquisition
* Quest resolution
* NPC death
* World events
* Conditions
* Resource changes

Uncommitted AI narration must not become authoritative history.

---

## 14.43 Narrative Interaction Memory

Purely narrative interactions may still create persistent continuity information.

Example:

> Mira promises never to reveal the player's secret.

If that promise matters in future play, RealmWeaver may create an appropriate:

* Event
* Relationship update
* Knowledge update
* Narrative memory

AI may propose such persistence where appropriate.

---

## Missing Information

## 14.44 No Confident Fabrication From Missing Context

If required context cannot be retrieved or is genuinely absent, AI should not invent a confident answer merely to maintain narrative flow.

Appropriate responses may include:

* Using known information only
* Remaining intentionally vague
* Retrieving additional context where available
* Treating the information as unknown
* Creating a new content proposal where appropriate

---

## 14.45 UNKNOWN Is a Valid State

RealmWeaver must allow:

```text
UNKNOWN
```

as a legitimate state.

Example:

Player asks:

> Who built this ruined tower?

If no canonical answer exists, RealmWeaver need not fabricate a pre-existing truth.

The answer may remain unknown or trigger a valid world-content proposal under Section 9H.

---

## Latency and Cost

## 14.46 Efficient Context Assembly

Context assembly should be selective and task-specific.

Loading large amounts of irrelevant campaign history before every AI request would increase:

* Latency
* Token usage
* Cost
* Noise
* Information-leak risk

Efficiency must be balanced against correctness.

---

## 14.47 Stable Context Optimisation

Stable instructions and campaign configuration may later be:

* Cached
* Reused
* Provider-optimized
* Stored separately from dynamic context

Exact optimisation techniques are deferred to AI architecture.

These optimisations must not alter RealmWeaver's authority or knowledge boundaries.

---

## 14.48 Context & Memory Boundary Invariants

RealmWeaver adopts the following requirements:

1. Authoritative RealmWeaver state is distinct from AI memory.
2. Conversation history is not the authoritative source of campaign truth.
3. AI context is a temporary assembled view of relevant RealmWeaver state.
4. AI context should be conceptually layered.
5. Stable system/rules context should be distinguishable from dynamic campaign context.
6. Different AI tasks receive context appropriate to their purpose.
7. AI operations follow a minimum-sufficient-context principle.
8. More context is not automatically better.
9. Required authoritative state cannot be omitted merely to save tokens.
10. RealmWeaver should distinguish mechanical, world, knowledge, social, event and narrative memory categories.
11. Critical mechanical state remains structured.
12. Important persistent facts should be structured where practical.
13. Not every gameplay event requires long-term memory.
14. Significant continuity-relevant events should remain recoverable.
15. Memory importance may change as campaign context evolves.
16. Long histories should be summarized rather than always resent verbatim.
17. Memory summaries cannot override current authoritative state.
18. Summaries should preserve useful causality.
19. Summaries must not invent new facts.
20. Historical information should be retrieved when relevant rather than always loaded.
21. Retrieval may use entities, location, quests, topic, recency, importance, causality and semantic relevance.
22. Entity-linked/deterministic retrieval should complement semantic retrieval.
23. Recent interaction context remains important alongside long-term retrieval.
24. Narrative memory is supporting context rather than authoritative state.
25. Context conflicts resolve in favour of authoritative RealmWeaver state.
26. Campaign continuity must survive session and conversation boundaries.
27. AI provider or model changes must not destroy campaign memory.
28. LLM context-window limits must not define maximum campaign length.
29. Player corrections may trigger review of relevant memory/history.
30. Player claims cannot directly overwrite authoritative state.
31. AI may propose continuity-relevant memory updates.
32. Deterministic state changes must not depend on AI remembering to store them.
33. AI omission cannot delete existing authoritative information.
34. Low-value historical information may be compressed.
35. Structured authoritative state cannot be lost through summarization or compression.
36. Raw event/narrative history may be retained separately where practical.
37. Context retrieval and assembly must enforce hidden-information boundaries.
38. Context must be scoped to the AI role/task being performed.
39. Separate controlled AI operations should reduce cross-role information leakage.
40. Memory for mechanical outcomes should follow committed state.
41. Important narrative interactions may create validated memory/state updates.
42. Missing required context must not be replaced by confident fabrication.
43. UNKNOWN is a legitimate state and may lead to later content proposals.
44. Context assembly should remain selective for latency and cost.
45. Stable/reusable context may be optimized later without changing authority or information rules.

---

# 15. 9K — Failure, Retry & Consistency

## 15.1 Status

**APPROVED**

---

## 15.2 Purpose

RealmWeaver must remain mechanically and narratively consistent when AI operations, network requests, persistence operations, context retrieval or other internal systems fail.

The governing principles are:

```text
FAILURE MUST NOT CORRUPT CAMPAIGN STATE.

RETRY MUST NOT APPLY THE SAME ACTION TWICE.

TECHNICAL FAILURE MUST NOT CREATE FREE REROLLS.

WHEN SYSTEMS DISAGREE,
AUTHORITATIVE REALMWEAVER STATE WINS.
```

Failures are expected operational conditions that must be handled safely rather than allowed to damage persistent campaigns.

---

## Failure Categories

## 15.3 Conceptual Failure Types

RealmWeaver should conceptually distinguish failure categories such as:

```text
AI_FAILURE
VALIDATION_FAILURE
RESOLUTION_FAILURE
COMMIT_FAILURE
CONTEXT_FAILURE
NARRATION_FAILURE
CLIENT_TRANSPORT_FAILURE
INTERNAL_SYSTEM_FAILURE
```

Exact error codes and exception structures are deferred to later architecture.

Different failure classes may require different recovery behaviour.

---

## 15.4 Gameplay Rejection Is Not a Technical Failure

An invalid gameplay action is a normal rules outcome rather than a system error.

Example:

Player:

> I cast Fireball.

Authoritative character state:

```text
Fireball:
Not known / prepared / available.
```

Result:

```text
REJECTED
```

Likewise, attempting an impossible action may produce a normal validation rejection.

RealmWeaver must distinguish:

```text
INVALID GAME ACTION
```

from:

```text
SYSTEM FAILURE
```

---

## Coherent Mechanical State

## 15.5 Coherent Multi-State Commitment

A mechanical resolution may change several pieces of state.

Example:

```text
Ammo:
-1

Enemy HP:
-17

Vex:
Applied

Player Hidden State:
Removed
```

These changes belong to one logical mechanical resolution.

RealmWeaver should not leave the campaign in an impossible state where only some required consequences were committed.

Conceptually:

```text
RESOLVE COMPLETE STATE DELTA
        ↓
VALIDATE DELTA
        ↓
COMMIT COHERENTLY
```

Exact database transaction mechanisms are deferred.

---

## 15.6 No Partial Authoritative State

If a required mechanical commit fails, RealmWeaver should preserve the last valid authoritative state.

A failed resolution must not leave partial effects such as:

```text
Resource consumed
but
action result not committed
```

where those changes were intended to occur as one atomic logical operation.

Rollback/transaction implementation is deferred to database architecture.

---

## 15.7 Explicit Commit Boundaries

Multi-stage mechanical effects require clearly defined commit boundaries.

Example:

```text
Attack rope
↓
Rope breaks
↓
Chandelier falls
↓
Saving Throws
↓
Damage
```

RealmWeaver must know which consequences form one coherent resolution and when a later consequence becomes a separate event.

The behavioural requirement is:

> **Commit boundaries must be explicit and must not leave the campaign in mechanically impossible intermediate states.**

---

## Action Identity and Idempotency

## 15.8 Significant Action Identity

Mechanically significant operations should eventually have an identity concept such as:

```text
ACTION_ID
RESOLUTION_ID
EVENT_ID
```

Exact naming and generation are deferred.

The purpose is to distinguish:

```text
RETRY OF EXISTING ACTION
```

from:

```text
GENUINELY NEW GAMEPLAY ACTION
```

---

## 15.9 Duplicate Requests Cannot Duplicate Effects

Example:

Player attacks once.

RealmWeaver commits:

```text
Damage:
17
```

A network timeout occurs before the client receives the response.

The client retries.

RealmWeaver must not apply another 17 damage.

Correct behaviour:

```text
Existing action detected
↓
Return existing committed result
```

The invariant is:

> **Duplicate requests cannot duplicate committed mechanical consequences.**

---

## 15.10 Narration Retry Reuses Mechanical Result

If mechanical state has already committed:

```text
Mechanics committed
↓
Narration fails
```

a narration retry must use the existing committed resolution.

It must not:

* Revalidate as a new gameplay action
* Generate another attack roll
* Reapply damage
* Consume resources again
* Change the mechanical outcome

---

## AI Output Failure

## 15.11 Malformed AI Output

When an AI operation is expected to produce structured output but returns malformed or unusable information:

```text
Expected:
Structured proposal

Received:
Malformed / unparseable output
```

RealmWeaver should:

1. Reject the malformed output.
2. Leave authoritative state unchanged.
3. Retry or invoke an appropriate fallback where useful.

Malformed AI output must never directly mutate campaign state.

---

## 15.12 Structured Output Still Requires Rules Validation

Syntactically correct structured output is not automatically mechanically valid.

Example:

```text
{
  "spell": "Fireball",
  "slot": 3
}
```

may be valid structured data while still being illegal for the acting character.

RealmWeaver therefore requires conceptually:

```text
STRUCTURE VALIDATION
        ↓
GAME-RULE VALIDATION
```

AI-generated structure never bypasses deterministic rules validation.

---

## 15.13 Bounded AI Retries

Automatic AI retries must be bounded.

RealmWeaver must not enter:

```text
Retry
↓
Retry
↓
Retry
↓
Retry
↓
...
```

indefinitely.

Exact retry limits are deferred.

After bounded attempts, RealmWeaver should use an appropriate fallback or player-facing recovery path.

---

## 15.14 Concise Retry Context

When retrying an AI operation, RealmWeaver should provide concise information needed to correct the failure.

Example:

```text
Rejected:
Fireball unavailable.

Relevant legal options:
Longsword
Dash
Disengage
Dodge
```

Recovery context should avoid unnecessary full-context repetition where practical.

---

## Fallback Behaviour

## 15.15 Task-Specific Fallbacks

Different AI tasks may require different failure behaviour.

### NPC Action Failure

Possible fallback:

```text
Constrain NPC to known legal actions.
```

### Narration Failure

Possible fallback:

```text
Display committed mechanical result.
```

### World-Generation Failure

Possible fallback:

```text
Do not materialise new content.
```

### Intent-Interpretation Failure

Possible fallback:

```text
Request player clarification.
```

A universal fallback is not required.

---

## 15.16 Fallback Cannot Bypass Authority

Fallback logic remains subject to all RealmWeaver authority rules.

Invalid:

```text
AI failed repeatedly
↓
Accept invalid action anyway
```

Fallback behaviour must remain mechanically valid and state-safe.

---

## 15.17 Conservative Mechanical Fallbacks

For AI-controlled actors, safe deterministic fallback actions are a bounded failure-recovery exception used only after AI recovery fails. They should use existing supported mechanics.

Depending on circumstances, fallback behaviour may include actions such as:

* Dodge
* Basic legal attack
* Disengage
* Legal movement toward an objective

Fallback logic must not invent new abilities or resources.

Fallback logic must remain legal and conservative and must not replace normal AI-owned NPC intent outside failure recovery.

---

## Player-Facing Failure Behaviour

## 15.18 Player-Friendly Error Presentation

Internal errors should not normally expose implementation details during ordinary gameplay.

Avoid:

```text
NullReferenceException...
ValidationPipelineError...
LLM_TOOL_PARSE_FAILURE...
```

Prefer game-facing information such as:

> The action could not be completed. Your campaign state has not changed.

Detailed diagnostics may remain available to developer/debug systems.

---

## 15.19 Commit Status Must Be Clear

When an error occurs, RealmWeaver should know whether:

```text
ACTION DID NOT COMMIT
```

or:

```text
ACTION COMMITTED
BUT PRESENTATION FAILED
```

Where relevant, player-facing UX should make this distinction understandable.

Example:

> Your attack was resolved successfully, but its narration could not be generated.

---

## 15.20 Do Not Repeat Committed Actions

If an action already committed, RealmWeaver must not tell the player to repeat that gameplay action merely because narration or presentation failed.

Recovery should reuse the committed result.

---

## 15.21 Uncommitted Actions May Be Retried

If an action genuinely failed before authoritative commitment, a retry may be allowed where appropriate.

RealmWeaver must distinguish this from resubmitting an already-completed action.

---

## Stale State and Paused Resolution

## 15.22 Resolution Uses Authoritative State

Mechanical resolution must operate against known authoritative state.

If an AI decision was produced using stale information, relevant assumptions must be revalidated before commitment.

---

## 15.23 Revalidation After State Changes

A resolution may pause because of:

* Reaction
* Player decision
* AI-controlled NPC choice
* Other interrupt

If authoritative state changes while resolution is paused, affected assumptions should be revalidated.

Example:

```text
Player attacks target
↓
Reaction interrupts
↓
Target teleports
↓
Original targeting assumptions invalid
```

---

## 15.24 State Awareness for Paused Pipelines

Conceptual resolution states such as:

```text
PLAYER_CHOICE_REQUIRED
PAUSED_FOR_REACTION
```

must not blindly resume against an obsolete snapshot.

Exact state versioning, locking or concurrency mechanisms are deferred to later architecture.

---

## Randomness and Retry

## 15.25 Randomness Is Bound to the Validated Action

Once RealmWeaver generates randomness for a validated action, the result is bound to that action's unique identity:

```text
Attack Roll:
17
```

All technical resubmissions and persistence retries of that same action must continue to use that result.

A retry cannot reroll bound randomness, whether or not the action has already committed.

Failed validation produces no unnecessary random result.

---

## 15.26 Technical Failure Should Not Grant a Free Reroll

If RealmWeaver generates an authoritative random result for a specific action and a later technical commit step fails:

```text
ACTION XYZ
Roll:
17

Commit:
Failed
```

then retrying the same action must reuse the already-generated resolution data:

```text
Retry ACTION XYZ
↓
Reuse Roll 17
```

Technical failure should not alter fate or create free rerolls.

If the bound result cannot be recovered, RealmWeaver must fail safely and reconcile authoritative state rather than silently reroll.

Detailed random/event persistence is deferred.

---

## 15.27 New Gameplay Attempts Are Different

A new random result is permitted only when the earlier action was conclusively cancelled while uncommitted and the player or AI then submits a materially different validated action.

Cosmetic rewording or technical resubmission does not create a new action.

If the rules permit another attempt:

```text
NEW ACTION
```

then new randomness may be generated normally only when it is a genuinely new validated action under this rule.

---

## Narration Consistency Recovery

## 15.28 Narration Regeneration Preserves Facts

If generated narration contradicts a committed result, regeneration must use the same authoritative facts.

Example:

```text
Committed:
Enemy survives at 8 HP.
```

Incorrect narration:

> The enemy falls dead.

Regenerated narration must preserve:

* HP
* Damage
* Rolls
* Conditions
* Position
* Resource state
* Other committed consequences

---

## 15.29 Fix Narration, Not Game State

When AI prose conflicts with authoritative state:

```text
FIX THE NARRATION
```

not:

```text
CHANGE REALMWEAVER STATE
TO MATCH THE PROSE
```

Narration failure is a presentation problem.

It is not permission to mutate campaign reality.

---

## Context and Memory Failure

## 15.30 Missing Memory Cannot Be Fabricated

If relevant historical context cannot be retrieved, AI should not confidently invent a previous event.

Avoid unsupported statements such as:

> As I promised you yesterday...

unless the promise exists in relevant authoritative/event/memory context.

---

## 15.31 Stale Summaries Cannot Override Current State

If an old summary conflicts with current structured authoritative state, current authoritative state wins.

This follows Section 9J.

---

## 15.32 Critical Missing Context Fails Safely

If an AI task requires essential information and that information cannot be resolved, RealmWeaver should not ask AI to confidently guess.

Conceptually:

```text
CONTEXT_INCOMPLETE
```

may trigger:

* Additional retrieval
* Clarification
* Internal recovery
* Safe fallback

depending on the operation.

---

## Content and World Failure

## 15.33 No Partial Persistent Content

If materialisation of important generated content fails, RealmWeaver should not leave an invalid partially-created entity where required information is missing.

Example:

```text
Faction name created
but
required identity/state invalid
```

should not become authoritative persistent canon.

---

## 15.34 Unmaterialised Content Remains Non-Authoritative

If generated content fails validation or persistence, its appearance in AI output alone does not make it reliable campaign canon.

Materialisation/commit determines authoritative persistence.

---

## 15.35 Scheduled Events Must Not Silently Disappear

If an authoritative scheduled event reaches its trigger time but processing fails:

```text
Assassination Attempt:
18:00
```

RealmWeaver must not simply forget the event and continue as if nothing occurred.

The event should remain recoverable, pending, failed or otherwise explicitly represented until safely resolved.

Exact scheduler state architecture is deferred.

---

## 15.36 Critical Event Failure and Time Progression

If future world state depends upon an unresolved critical event, RealmWeaver may need to prevent or safely defer dependent progression rather than creating contradictory future state.

Exact event scheduling behaviour will be designed later.

---

## Recovery and Auditability

## 15.37 Failure Traceability

Important failures should be traceable to relevant context such as:

```text
Campaign
Action
Resolution
AI Operation
Event
Actor
```

Exact logs, telemetry and observability architecture are deferred.

---

## 15.38 Recovery Provenance

Recovery should preserve enough provenance to distinguish cases such as:

```text
AI Proposal
↓
Rejected
↓
Modified / retried
↓
Resolved
```

RealmWeaver should not falsely represent a repaired proposal as though the original AI output was mechanically valid.

---

## 15.39 UI Reconciliation

After errors or retries, player-facing UI state must reconcile to authoritative backend state.

Example:

Temporary client state:

```text
Potion ×3
```

Authoritative committed state:

```text
Potion ×2
```

The client must ultimately display the authoritative value.

Frontend state management details are deferred.

---

## Cross-System Consistency

## 15.40 Authoritative Priority

When systems disagree:

```text
AUTHORITATIVE REALMWEAVER STATE
        >
AI ASSUMPTIONS
        >
AI NARRATION
```

AI does not maintain an independent competing campaign reality.

---

## 15.41 Derived Mechanical State

Derived mechanical values should be calculated from authoritative structured sources where practical.

Examples include:

* Attack modifiers
* Spell save DC
* Current AC
* Other derived statistics

AI should not independently maintain or guess authoritative derived values.

---

## 15.42 No Silent Player-Visible Retcons

If a system failure caused inaccurate player-visible information, meaningful correction should not happen silently.

Example:

AI told the player:

> You received 500 GP.

but authoritative commitment failed.

RealmWeaver should clearly correct the discrepancy rather than quietly changing displayed state later.

The preferred architecture should prevent such cases by committing before narration wherever applicable.

---

## Campaign Save Integrity

## 15.43 Valid Save State

Persistent campaign saves should satisfy critical RealmWeaver state invariants.

Examples of invalid state may include:

```text
Negative spell slots

Equipped item not validly owned/carried

Dead NPC active without valid state explanation

Required quest reference points to nonexistent entity
```

Exact integrity validation depth is deferred.

---

## 15.44 Load-Time Integrity Checks

When loading a campaign, RealmWeaver should verify enough critical state to avoid continuing from obvious corruption.

This does not require replaying or revalidating the entire campaign history on every load.

---

## 15.45 Migration Safety

Future RealmWeaver versions may require persistence/schema migrations.

A failed migration must not simply destroy or overwrite the user's existing campaign.

Detailed backup, migration and recovery architecture is deferred.

---

## Degraded Operation

## 15.46 Limited AI-Outage Behaviour

If an AI provider becomes temporarily unavailable while deterministic campaign state remains healthy, limited degraded operation is a desirable design goal.

Potentially accessible functionality may include:

* Character sheet
* Inventory
* Quest state
* Campaign history
* Mechanical information
* Existing persisted world information

Full new gameplay without AI is not required as a V1 guarantee.

---

## 15.47 Persisted State Remains Accessible

Where technically possible, an AI failure should not prevent access to persisted non-AI campaign information.

The AI provider must not become the sole gateway to RealmWeaver state.

---

## Recovery Tools

## 15.48 Explicit Recovery Operations

RealmWeaver may later expose controlled operations such as:

```text
Regenerate Narration
Report Inconsistency
Reload Authoritative State
View Resolution Details
```

These tools should correct presentation or context problems without allowing arbitrary mechanical state mutation.

---

## 15.49 Regenerate Narration Is Not Reroll

This is a hard RealmWeaver contract.

Selecting:

```text
REGENERATE NARRATION
```

may change:

```text
HOW THE RESULT IS DESCRIBED
```

It may not change:

* Dice rolls
* Success or failure
* Damage
* Healing
* Loot
* NPC mechanical choices
* Resource consumption
* Conditions
* Other committed state

---

## 15.50 Failure, Retry & Consistency Invariants

RealmWeaver adopts the following requirements:

1. Technical failures are expected recoverable conditions and must not corrupt persistent campaign state.
2. Failure types should be conceptually distinguishable.
3. Invalid gameplay actions are different from technical system failures.
4. Multi-state mechanical actions should commit coherently.
5. Failed mechanical resolutions must not leave invalid partial authoritative state.
6. Multi-stage mechanics require explicit safe commit boundaries.
7. Significant actions should have identities that permit retry recognition.
8. Duplicate requests cannot duplicate committed effects.
9. Narration retries reuse existing committed mechanical results.
10. Malformed AI output is rejected without authoritative mutation.
11. Well-formed AI output still requires game-rules validation.
12. Automatic AI retries must be bounded.
13. Retry operations should receive concise useful recovery information.
14. Different AI operations may use different fallback behaviours.
15. Fallback behaviour cannot bypass RealmWeaver authority rules.
16. Mechanical fallbacks should remain conservative and supported.
17. Internal technical details should normally remain outside player-facing gameplay errors.
18. RealmWeaver should distinguish whether an action committed when presenting recovery information.
19. Players should not repeat an already-committed action merely because presentation failed.
20. Truly uncommitted actions may be retried where appropriate.
21. Mechanical resolution must operate against known authoritative state.
22. Relevant assumptions should be revalidated if state may have changed before commitment.
23. Paused resolution pipelines must account for state changes before resuming.
24. Once generated for a validated action, randomness is bound to that action's unique identity.
25. Technical or persistence retries of the same action must reuse its bound randomness.
26. A new roll is allowed only after a conclusively cancelled, uncommitted action is followed by a materially different validated action.
27. Narration regeneration preserves all committed mechanical facts.
28. Mechanical state never changes merely to match incorrect AI prose.
29. Memory retrieval failure must not result in fabricated previous events.
30. Stale summaries cannot override current authoritative state.
31. Missing critical context should fail safely rather than force AI guessing.
32. Failed persistent world/content creation must not leave invalid partial canon.
33. Failed significant-content materialisation must not produce player-visible established canon or silent retcons.
34. Scheduled world events must not silently disappear because their processing failed.
35. Critical unresolved events may constrain dependent time progression until safely handled.
36. Significant failures should be traceable for development and debugging.
37. Recovery should retain relevant provenance.
38. Player-facing UI state must ultimately reconcile with authoritative backend state.
39. RealmWeaver authoritative state wins over AI assumptions and narration.
40. Derived mechanical values should originate from authoritative structured sources where practical.
41. Recovery should not create silent player-visible retcons where discrepancies matter.
42. Campaign saves should satisfy critical authoritative-state invariants.
43. Campaign loading should perform appropriate critical integrity checks.
44. Future migration failures must not destroy existing campaigns.
45. Limited degraded operation during AI outages is a desirable design goal rather than a required V1 gameplay guarantee.
46. Persisted campaign information should remain accessible where possible during AI failure.
47. Explicit recovery tools may later repair presentation/context problems without bypassing authoritative state.
48. Regenerating narration never rerolls or changes the underlying committed outcome.

The fundamental reliability contract is:

```text
NO PARTIAL STATE.

NO DOUBLE APPLICATION.

NO FREE REROLLS FROM RETRIES.

FIX PRESENTATION ERRORS
WITHOUT CHANGING AUTHORITATIVE RESULTS.
```

---

# 16. 9L — Latency, UX & Final Contract

## 16.1 Status

**APPROVED**

This section includes the approved visual-quality/UI amendment establishing polished interface design as a first-class RealmWeaver requirement.

---

## 16.2 Purpose

RealmWeaver's internal AI, rules, memory and persistence architecture may be complex.

The normal player experience should not feel mechanically or technically complicated.

The governing UX principle is:

```text
THE SYSTEM MAY BE COMPLEX INTERNALLY.

THE PLAYER EXPERIENCE SHOULD NOT FEEL COMPLEX.
```

The final RealmWeaver product contract is:

```text
AI TELLS THE STORY.

RULES DECIDE WHAT HAPPENS.

REALMWEAVER PRESERVES THE WORLD.
```

---

## Narrative-First Experience

## 16.3 Narrative as Primary Interaction

RealmWeaver should primarily feel like interacting with an immersive fantasy RPG rather than operating a rules database.

Normal interaction may resemble:

```text
Player:
"I kick the door open and rush the guard."

        ↓

RealmWeaver interprets and resolves

        ↓

Immersive narrative response
```

Internal concepts such as:

```text
INTENT_CLASSIFICATION
ACTION_PROPOSAL
VALIDATION_STAGE
STATE_DELTA
COMMIT_STATUS
```

should normally remain invisible during ordinary gameplay.

---

## 16.4 Inspectable Mechanics

Narrative-first presentation does not mean hiding relevant rules information.

Players should be able to inspect applicable details such as:

* Dice results
* Modifiers
* DCs where appropriate
* Damage calculations
* Healing
* Conditions
* Resource use
* Spell-slot use
* Weapon Mastery effects
* Other relevant mechanical outcomes

Conceptually:

```text
IMMERSIVE NARRATIVE

[Roll Details]
[Mechanical Result]
```

RealmWeaver should combine immersion with mechanical transparency.

---

## Latency and AI Usage

## 16.5 Local Deterministic Resolution

Routine deterministic mechanics should resolve without unnecessary LLM round trips.

Examples include:

* Attack calculations
* Damage
* Saving throw resolution
* Resource consumption
* Spell-slot validation
* Conditions
* Movement validation
* Weapon Mastery
* Rest recovery
* Inventory state
* Derived mechanical calculations

The deterministic rules engine handles deterministic rules.

---

## 16.6 AI Calls Must Add Value

RealmWeaver should not ask AI to perform deterministic work such as:

> Is 14 + 7 greater than AC 18?

or:

> Does the player have a remaining spell slot?

AI usage should focus on functions where generative/reasoning capability adds value, including:

* Natural-language interpretation
* NPC decision-making
* Narration
* World/content proposals
* Selected contextual reasoning

---

## 16.7 Minimize AI Call Chains

For ordinary player actions, RealmWeaver should minimize unnecessary sequences of AI operations.

A desirable conceptual path is:

```text
PLAYER INPUT
↓
AI intent/proposal where required
↓
LOCAL deterministic validation/resolution
↓
COMMIT / PERSIST
↓
PRIMARY narration generation
```

Some situations may require additional AI calls, including:

* NPC decisions
* Clarification
* Multi-stage choices
* World generation

The requirement is to avoid unnecessary LLM round trips.

---

## 16.8 Structured UI May Bypass Intent Interpretation

When a player selects a fully structured action such as:

```text
Cast
↓
Magic Missile
↓
Target Goblin
```

RealmWeaver already knows the intended mechanic.

The action may proceed directly to rules validation and resolution without requiring AI intent interpretation.

AI may still narrate the committed result.

Conceptually:

```text
NATURAL LANGUAGE
→ AI interpretation where required

STRUCTURED UI ACTION
→ Direct deterministic validation
```

---

## 16.9 Free-Form Natural Language Remains Core

Structured UI should accelerate common gameplay interactions.

It must not replace free-form play.

The player should remain able to write:

> I throw my cloak over the brazier and use the smoke to distract the guards.

Creative actions should continue through RealmWeaver's interpretation/proposal pipeline.

---

## Responsiveness

## 16.10 Immediate Action Acknowledgement

The UI should quickly indicate that player input has been received.

Exact presentation is deferred, but may include:

```text
Resolving action...
```

or relevant visible mechanical progression.

The player should not be left uncertain whether the application registered the action.

---

## 16.11 Committed Results May Precede Narration

If mechanical resolution completes before narration generation, RealmWeaver may display the committed result.

Example:

```text
Attack:
Hit

Damage:
17
```

while narration is produced.

Only committed outcomes may be displayed as authoritative facts.

```text
COMMITTED RESULT
→ May be presented
```

```text
UNCOMMITTED PREDICTION
→ Must not be presented as fact
```

---

## 16.12 Streaming Narration

RealmWeaver may support streamed AI narration where provider capabilities and UX justify it.

Streaming is an optimisation.

It does not alter any authority boundaries or permit streamed prose to become authoritative state.

---

## Dice Experience

## 16.13 Visible Dice Feedback

Player-visible rolls should retain the feel and transparency of tabletop dice where appropriate.

Example:

```text
d20:
14

Modifier:
+7

Total:
21
```

This supports player trust and tabletop identity.

---

## 16.14 Hidden Rolls

Information-boundary requirements remain authoritative.

Not every RealmWeaver roll must be exposed to the player.

Hidden/system rolls remain hidden where revealing the roll would leak information.

---

## 16.15 Dice Presentation Is Not Dice Authority

Visual dice, animations and presentation effects do not independently determine random results.

The authoritative value originates from RealmWeaver's dice system.

The UI presents the result.

It does not create a competing roll.

---

## Interruptions and Meaningful Choices

## 16.16 Clear Reaction Windows

When resolution pauses for a meaningful reaction or decision, the UI should make the available choice understandable.

Example:

```text
Use your reaction?

[Opportunity Attack]
[Decline]
```

Where relevant, the player should understand:

* Why resolution paused
* What options exist
* Applicable resource implications

---

## 16.17 Avoid Trivial Interruptions

RealmWeaver should avoid unnecessary confirmation prompts.

Avoid interaction patterns such as:

```text
Open unlocked door?

Are you sure?

Step through doorway?
```

Clarification and decision prompts should be reserved for mechanically or narratively meaningful uncertainty.

---

## 16.18 Optional Resources Are Not Silently Consumed

Meaningful optional mechanics should generally require the controlling player's decision unless a rule explicitly permits automated behaviour.

Examples include:

* Reactions
* Limited resources
* Optional rerolls
* Optional features
* Optional spell effects

Automation must not remove meaningful player agency.

---

## Error Experience

## 16.19 Helpful Rule Rejections

Rule rejection should provide useful information where possible.

Prefer:

> You cannot cast Fireball because it is not currently prepared.

Potentially followed by:

> Available prepared spells include Scorching Ray and Magic Missile.

rather than:

> Invalid action.

---

## 16.20 Hide Internal Architecture Language

Normal gameplay should avoid exposing terms such as:

```text
Proposal validation failed.
LLM tool call invalid.
NPC retry attempt #2.
```

Player-facing language should remain game-oriented.

Developer/debug tooling may expose internal diagnostics separately.

---

## 16.21 Graceful Narration Failure

If narration generation fails after mechanical commitment, RealmWeaver should continue to expose the committed outcome.

Example:

```text
Attack successful.
17 piercing damage.
```

Narration may then be retried/regenerated without rerunning mechanics.

---

## Direct Structured State Access

## 16.22 Inspectable Character and Campaign State

Players should have direct access to relevant structured information such as:

* Character sheet
* HP and AC
* Inventory
* Equipment
* Spells
* Conditions
* Resources
* Quest state
* Active effects

This information should not require the player to ask the AI.

---

## 16.23 Basic UI State Does Not Require LLM Calls

Opening the inventory should not require:

```text
LLM:
"What items does the player own?"
```

The UI should read authoritative state directly.

The same applies to:

* HP
* Currency
* Equipment
* Spell slots
* Conditions
* Quest status
* Other structured player data

---

## 16.24 Conversational State Queries Remain Supported

Players may still ask natural-language questions such as:

> How badly hurt am I?

AI may answer from authoritative provided context.

Conversational access complements structured UI.

It does not replace it.

---

## Player Agency and Automation

## 16.25 Automate Rules Administration

RealmWeaver should automate mechanical administration such as:

* Modifier calculations
* Resource tracking
* Effect expiry
* Trigger handling
* Valid target checking
* World-clock changes
* Deterministic consequences

---

## 16.26 Preserve Meaningful Roleplay Decisions

RealmWeaver should not automatically decide meaningful player-character choices such as:

* Whom the player trusts
* Whether the player accepts a deal
* What the player says
* Whether the player spends a significant optional resource
* Major moral decisions
* Major strategic/story decisions

The player owns player-character intent.

---

## 16.27 AI Assistance Is Not Autoplay

AI may interpret and support player intent.

It must not begin playing the player's character automatically.

Player agency remains a core product requirement.

---

## UI and Backend Separation

## 16.28 UI Reads Authoritative State

Structured UI components should derive their state from RealmWeaver's authoritative backend.

Conceptually:

```text
REALMWEAVER STATE
        ↓
UI
```

not:

```text
AI NARRATION
        ↓
Parse prose
        ↓
Infer gameplay state
```

---

## 16.29 Narrative and UI Detail Levels

Narration and structured UI may represent the same event at different levels of detail.

Example narration:

> The blade cuts deeply across your shoulder.

UI:

```text
HP:
32 → 21

Damage:
11 slashing
```

Both must describe the same authoritative event.

---

## Long-Running Campaign UX

## 16.30 Automatic Campaign Resume Context

When a player returns to a campaign, RealmWeaver should reconstruct relevant context automatically.

This may include:

* Player-character state
* Current location
* World time
* Current quests
* Relevant NPC state
* Recent scene state
* Relevant campaign memory/history

The player should not need to manually paste a campaign summary.

---

## 16.31 Optional Resume Recap

RealmWeaver may provide a concise campaign recap when resuming.

Example:

> Last time, you escaped Blackgate with Mira after discovering the duke's correspondence. Captain Renn remains missing, and the city guard believes you were involved in the prison break.

The recap must be grounded in appropriate persisted memory/state.

---

## 16.32 Recap Knowledge Boundaries

Player-facing recaps remain subject to Section 9I.

They must not expose:

* NPC secrets
* Off-screen plans
* Hidden world events
* Undiscovered locations
* Other player-hidden information

merely because RealmWeaver stores those facts.

---

## Generated Content UX

## 16.33 Generated Content Should Feel Native

Valid AI-generated/materialised content should integrate naturally into RealmWeaver.

The player should not need to care whether an NPC, quest or location originated from:

```text
AI_DM
CAMPAIGN_GENERATION
PREBUILT_CONTENT
```

unless provenance is intentionally exposed for another reason.

---

## 16.34 Unsupported Mechanics Must Not Be Offered

The UI and AI should not present unsupported systems as available gameplay options.

A mechanic that is not implemented/supported in the current ruleset should not be casually suggested as though the player can use it.

---

## Performance Priorities

## 16.35 Correctness Before Speed

RealmWeaver should prioritize correct authoritative outcomes over faster but incorrect results.

```text
FAST WRONG RESULT
```

is worse than:

```text
SLIGHTLY SLOWER CORRECT RESULT
```

Performance optimisation must preserve mechanical and information correctness.

---

## 16.36 Avoid Unnecessary Correctness Overhead

Correctness does not justify needless processing.

RealmWeaver should avoid patterns such as:

* AI validation for basic arithmetic
* Retrieving hundreds of unrelated memories
* Running full-world simulation for routine combat
* Mechanically analysing decorative adjectives
* Repeated AI calls where deterministic rules suffice

The architecture should remain proportionate to the task.

---

## 16.37 Processing Should Scale With Importance

Routine events should generally use:

```text
Small relevant context
Local rules
Brief narration
```

while major events may justify:

```text
Richer context
More complex reasoning
Persistent world updates
More detailed narration
```

Processing effort should be proportional to gameplay significance.

---

## Visual Quality and RealmWeaver Identity

## 16.38 Visual Quality Is a First-Class Requirement

RealmWeaver's visual presentation is a first-class product requirement rather than an optional final styling phase.

Visual quality is mandatory for the core V1 player experience.

A player-facing feature is not considered production-quality solely because it functions correctly.

RealmWeaver should present a distinctive, polished and cohesive fantasy RPG experience.

The visual objective is:

> **The interface should make the player want to enter the world.**

Mandatory visual/UX review areas include:

* Landing and campaign entry
* Character creation
* Main campaign/narration interface
* Character sheet and resources
* Dice and mechanical-result presentation
* Combat status and actions
* Inventory and equipment
* Spells and conditions
* Quest/objective tracking
* Save, loading, error and recovery states
* Responsive behaviour on supported screen sizes

Mandatory quality expectations include:

* A coherent fantasy identity
* Consistent colours, typography, spacing, components and iconography
* Clear distinction between narration, dialogue, player choices and mechanics
* Clear presentation of authoritative state changes
* Readable contrast and accessible interaction states
* Designed loading, empty, disabled, success and error states
* No obviously unfinished placeholder UI in the V1 release

---

## 16.39 RealmWeaver Design System

RealmWeaver should use a coherent design system rather than styling individual screens independently.

The design system should eventually define areas such as:

```text
Typography
Colour palette
Surface hierarchy
Spacing
Borders / radius
Shadows
Icons
Buttons
Forms
Cards
Panels
Navigation
Dialogs
Tooltips
Narrative components
Character components
Combat components
Dice components
Quest components
Inventory components
Animation language
Responsive behaviour
Accessibility
```

Exact design tokens and implementation are deferred to frontend/UI design.

---

## 16.40 Distinct RealmWeaver Visual Identity

RealmWeaver should not appear to be an unmodified generic component library or generic AI chat interface.

External UI libraries may provide engineering foundations, but components should be adapted into a consistent RealmWeaver identity.

The interface should visually communicate:

* Fantasy
* Adventure
* Storytelling
* Exploration
* Tabletop RPG mechanics

without sacrificing usability.

---

## 16.41 Fantasy Styling Must Preserve Readability

Immersive visual styling must not produce:

* Difficult-to-read text
* Excessive ornamentation
* Confusing hierarchy
* Poor contrast
* Cluttered screens
* Unnecessary visual noise

Fantasy presentation should support the gameplay experience rather than obstruct it.

---

## 16.42 Purposeful Animation

Animations and transitions may enhance important interactions such as:

* Dice rolls
* Mechanical result presentation
* Panel changes
* Combat events
* Conditions
* Discoveries
* Quest completion
* Level-ups
* Important item acquisition
* Major narrative events

Animations should remain purposeful and should not unnecessarily slow routine gameplay.

---

## 16.43 Important Moments May Receive Elevated Presentation

Major events may use stronger visual presentation than routine actions.

Examples include:

```text
Critical Hit
Level Up
Major Location Discovery
Legendary Item Acquisition
Boss Defeat
Major Quest Completion
Major Story Revelation
```

RealmWeaver may visually emphasize such moments while maintaining the authoritative mechanical result.

---

## 16.44 Responsive and Accessible Design

Visual richness must not override:

* Accessibility
* Responsiveness
* Performance
* Clear navigation
* Keyboard usability where relevant
* Readability
* Appropriate contrast

The interface should adapt appropriately across supported screen sizes.

Exact supported device requirements are deferred.

---

## 16.45 UI Libraries and Development Accelerators

RealmWeaver may use:

* UI component libraries
* Design-system libraries
* Animation libraries
* Icon libraries
* AI-assisted component tooling
* Component registries
* Other suitable frontend accelerators

to improve quality and development speed.

The exact frontend technology stack is deferred to later M2 technology and architecture decisions.

Third-party components must be adapted into RealmWeaver's design language rather than mixed together inconsistently.

---

## 16.46 Visual UX Review as Part of Completion

Important player-facing features should undergo explicit visual/UX review before being considered complete.

Review should consider:

* Visual hierarchy
* Consistency
* Responsiveness
* Accessibility
* Interaction clarity
* Animation
* Narrative immersion
* Mechanical transparency
* Overall polish

This requirement should later be reflected in RealmWeaver's implementation Definition of Done.

The mandatory quality standard does not require elaborate animations or cinematic transitions, custom artwork for every entity, 3D environments, fully animated maps, generated video or voice presentation, multiple complete visual themes, Dark Mode, or purely decorative effects without usability value. These remain optional or deferred.

---

## Final Authority Contract

## 16.47 Player Contract

The player controls:

```text
Player-character intent
Meaningful player choices
Player-selected dialogue/actions
Optional player-controlled decisions
```

RealmWeaver and AI must not silently replace meaningful player decisions.

---

## 16.48 AI Contract

AI controls or proposes:

```text
Narration
NPC dialogue
NPC personality expression
NPC intentions
Player-intent interpretation
Mechanical proposals
World/content proposals
Narrative presentation
```

within the context and authority provided by RealmWeaver.

Generated AI output is not mechanically authoritative merely because it was generated.

---

## 16.49 RealmWeaver Contract

RealmWeaver owns:

```text
Rules
Dice
Mechanical legality
Mechanical resolution
Persistent state
World clock
Knowledge boundaries
Inventory
Equipment
Currency
Resources
Conditions
Progression
Quest state
Materialised world canon
Commit consistency
Retry consistency
```

RealmWeaver is the authoritative state owner.

---

## 16.50 Final Source-of-Truth Rule

When:

```text
AI says X
```

but:

```text
RealmWeaver authoritative state says Y
```

then:

```text
Y IS AUTHORITATIVE
```

unless a legitimate subsequent action changes the state.

---

## 16.51 Final Player Action Loop

The standard player action loop is:

```text
PLAYER
expresses intent
        ↓
AI
interprets / proposes where required
        ↓
REALMWEAVER
validates
        ↓
REALMWEAVER
resolves rules and controlled randomness
        ↓
REALMWEAVER
commits/persists authoritative state
        ↓
AI
narrates committed outcome
        ↓
PLAYER
receives immersive narrative
and inspectable mechanics
        ↓
WORLD
continues from committed state
```

---

## 16.52 Final NPC Action Loop

For AI-controlled NPCs:

```text
REALMWEAVER
supplies scoped NPC context
        ↓
AI
selects NPC intent
        ↓
REALMWEAVER
validates and resolves
        ↓
REALMWEAVER
commits/persists
        ↓
AI
narrates
```

---

## 16.53 Structured UI Action Loop

For fully structured player inputs:

```text
PLAYER UI ACTION
        ↓
REALMWEAVER VALIDATION
        ↓
MECHANICAL RESOLUTION
        ↓
COMMIT / PERSIST
        ↓
AI NARRATION
where useful
```

AI intent interpretation is not required when the structured action already communicates unambiguous intent.

---

## 16.54 Latency, UX & Final Contract Invariants

RealmWeaver adopts the following requirements:

1. Narrative remains the primary player-facing gameplay experience.
2. Relevant mechanical details remain inspectable.
3. Routine deterministic mechanics resolve without unnecessary AI calls.
4. AI is used primarily where generative or contextual reasoning adds value.
5. Ordinary player actions should minimize unnecessary chains of AI calls.
6. Structured UI actions may bypass AI intent interpretation.
7. Natural-language free-form interaction remains fully supported.
8. UI should acknowledge player actions promptly.
9. Committed mechanical results may be displayed before complete narration where useful.
10. Streaming narration may be supported without changing authority boundaries.
11. Player-visible rolls should expose meaningful dice/modifier information where appropriate.
12. Hidden rolls remain hidden where information boundaries require it.
13. Dice presentation cannot independently determine authoritative results.
14. Reaction and decision windows should clearly communicate meaningful choices.
15. RealmWeaver should not interrupt the player for trivial decisions.
16. Meaningful optional resources/mechanics should not be silently consumed.
17. Rule rejection should provide useful player-facing explanations where practical.
18. Internal AI/system terminology should normally remain outside standard gameplay UX.
19. AI narration failure should degrade gracefully using committed mechanical information.
20. Players should be able to inspect important structured campaign/character state directly.
21. Basic structured state inspection should not require an LLM call.
22. Conversational questions about authoritative state remain supported.
23. RealmWeaver automates rules administration rather than meaningful roleplay decisions.
24. AI assistance must not become player-character autoplay.
25. Structured UI derives from authoritative RealmWeaver state rather than parsing AI prose.
26. Narrative and structured UI may present the same event at different detail levels.
27. Campaign resume should reconstruct relevant persisted context automatically.
28. Optional campaign recaps may be generated from authoritative/relevant memory.
29. Resume recaps must respect hidden-information boundaries.
30. Materialised generated content should integrate naturally into the player experience.
31. Unsupported mechanics should not be presented as though they are available.
32. Correctness takes priority over latency.
33. Correctness should not justify unnecessary processing or AI calls.
34. Processing/context/narration effort should scale with event importance.
35. Visual quality is a first-class RealmWeaver product requirement.
36. RealmWeaver should use a coherent reusable design system.
37. The product should have a distinctive RealmWeaver visual identity rather than an unmodified generic UI-library appearance.
38. Fantasy styling must preserve readability and interaction clarity.
39. Animation should be purposeful and should not unnecessarily slow routine gameplay.
40. Important gameplay/story moments may receive elevated visual treatment.
41. Visual richness must preserve responsiveness, accessibility and performance.
42. External UI/component/animation tools may accelerate development but should conform to RealmWeaver's design language.
43. Important player-facing features should receive explicit visual/UX review before completion.
44. The player owns meaningful player-character intent and decisions.
45. AI owns narrative expression and bounded proposals/AI-controlled decisions.
46. RealmWeaver owns rules, mechanical resolution and authoritative persistent state.
47. Authoritative RealmWeaver state wins whenever AI output conflicts with it.
48. All interaction paths ultimately converge on validated and committed RealmWeaver state before persistent consequences become real.

The final Group 9 contract is:

```text
PLAYER CHOOSES.

AI INTERPRETS,
DECIDES FOR NPCs,
PROPOSES,
AND NARRATES.

REALMWEAVER VALIDATES,
RESOLVES,
COMMITS,
AND REMEMBERS.
```

And the final UX principle is:

```text
THE INTERFACE SHOULD MAKE
THE PLAYER WANT TO ENTER THE WORLD.
```

---

# 17. Group 9 — Review Status

## 17.1 Status

**SECTIONS 9A–9L APPROVED**

**RULES DESIGN COMPLETE**

**INTERNAL CONSISTENCY REVIEW PASSED**

**INTERNAL REVIEW GATE PASSED**

**M2.1 COMPLETION GATE PENDING**

Completed:

* 9A — Authority Model
* 9B — Player Intent Interpretation
* 9C — AI Mechanical Proposals
* 9D — Validation & Rejection
* 9E — Mechanical Resolution Pipeline
* 9F — AI Narration Boundary
* 9G — NPC AI Authority
* 9H — World & Content Proposals
* 9I — Knowledge & Information Boundaries
* 9J — Context & Memory Boundary
* 9K — Failure, Retry & Consistency
* 9L — Latency, UX & Final Contract

Group 9 establishes the complete behavioural boundary between:

```text
PLAYER
AI
RULES ENGINE
PERSISTENCE
WORLD STATE
MEMORY
NPC AUTONOMY
KNOWLEDGE
UI
```

for RealmWeaver V1.

---

## 17.2 Group 9 Core Architecture Principle

The complete authority relationship is:

```text
PLAYER
controls meaningful player-character intent.

AI
interprets natural language,
chooses behaviour for AI-controlled actors,
proposes mechanics/content,
and narrates outcomes.

REALMWEAVER
validates rules,
generates authoritative randomness,
resolves mechanics,
controls knowledge boundaries,
commits persistent state,
and preserves campaign continuity.
```

---

## 17.3 Complete End-to-End Principle

```text
INTENT
        ↓
PROPOSE
        ↓
VALIDATE
        ↓
RESOLVE
        ↓
COMMIT / PERSIST
        ↓
NARRATE
        ↓
CONTINUE WORLD
```

Persistent mechanical or world consequences cannot bypass this authority model.

Resolve calculates the complete state change without making it authoritative. Commit/persist applies that change through atomic durable persistence; only then does the state become authoritative and available for narration.

---

## 17.4 Next M2.1 Steps

With Group 9 rules design complete and its internal consistency review passed, the remaining work is:

1. Perform the **full M2.1 cross-group consistency review** across Groups 1–9.
2. Verify cross-file terminology and authority boundaries.
3. Review Weapon Mastery amendments against affected groups.
4. Review spell, condition, rest, equipment, NPC and world-state interactions.
5. Update the main game-rules/index documentation as required.
6. Update `PROJECT_STATUS.md`.
7. Perform the planned **SRD/IP/content-provenance audit** before implementation begins.
8. Complete the M2.1 gate.
9. Proceed into the remaining M2 technical architecture milestones.

---

## 17.5 Licensing / Provenance Gate Reminder

Before implementation begins, RealmWeaver must perform the planned rules/content provenance review.

The review should classify applicable content such as:

* Classes
* Spells
* Equipment
* Conditions
* Mechanical features
* Monsters
* Magic items
* Other rules-defined content

against provenance classifications such as:

```text
SRD_CC
REALMWEAVER_ORIGINAL
THIRD_PARTY_LICENSED
CAMPAIGN_CUSTOM
UNKNOWN / REQUIRES_REVIEW
```

Unknown or unsupported source material should be resolved before production implementation/content seeding.

---

## 17.6 Group 9 Internal Review Gate

The Group 9 internal consistency review evaluated whether:

* All 9A–9L sections exist in `09_AI_RULES_BOUNDARY.md`.
* All sections are marked APPROVED.
* No known unresolved design question remains inside Group 9.
* Authority consistently follows:

```text
AI proposes.
RealmWeaver validates.

AI narrates.
RealmWeaver commits.

AI may remember context.
RealmWeaver preserves truth.
```

* Player agency remains protected.
* NPC autonomy remains bounded by rules and knowledge.
* Persistent world canon cannot be created through prose alone.
* Hidden information cannot leak merely because the AI knows it.
* Technical retries cannot duplicate mechanical outcomes.
* UI state derives from authoritative RealmWeaver state.
* Visual quality is recognized as a first-class player-facing requirement.

These checks passed on 31 August 2026. The Group 9 internal-review gate is PASSED. The M2.1 completion gate remains PENDING, and the next approved activity is the Groups 1–9 cross-group consistency review.
