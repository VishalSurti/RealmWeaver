# RealmWeaver — AI / Rules Boundary

**Document Type:** Game Rules Specification  
**Milestone:** M2.1 — V1 Game Rules Specification & Rules-Engine Boundary  
**Group:** 9 — AI / Rules Boundary  
**Status:** IN PROGRESS  
**Last Reviewed:** 27 August 2026

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

# 6. Current Group 9 Progress

Approved:

* 9A — Authority Model
* 9B — Player Intent Interpretation

Next:

* 9C — AI Mechanical Proposals
* 9D — Validation & Rejection

After 9C–9D are approved, they should be added to this specification before proceeding to the next documentation batch.