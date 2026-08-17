# 06 — Equipment & Inventory

> **Status:** Approved for RealmWeaver V1  
> **Rules Group:** 6 — Equipment & Inventory  
> **Scope:** V1 Game Rules Specification  
> **Authority:** This document is the canonical source for RealmWeaver equipment, inventory, currency, loot, trading, consumables, encumbrance, and tool rules.

---

## 1. Core Equipment Principles

### 1.1 System Authority

RealmWeaver maintains authoritative mechanical state for all mechanically significant items.

The AI DM may:

- Interpret player intent.
- Describe items.
- Narrate item interactions.
- Suggest contextually appropriate items or loot.
- Propose unusual uses of equipment.

The AI DM must not independently:

- Create authoritative items.
- Delete authoritative items.
- Duplicate items.
- Change quantities.
- Change currency.
- Equip or unequip items.
- Modify item ownership.
- Determine authoritative item effects.
- Override equipment restrictions.

Mechanically significant changes must be validated by RealmWeaver before becoming persistent campaign state.

### 1.2 General Resolution Flow

Mechanically significant equipment interactions should generally follow:

Player Intent  
→ AI Interpretation  
→ Structured Action  
→ Validation  
→ Mechanical Resolution  
→ State Change  
→ Persistence  
→ AI Narration

Validation should occur before final narration wherever possible.

### 1.3 Persistent Item State

Once an item becomes mechanically significant, RealmWeaver must persist its state.

AI narration must not override authoritative stored item state.

---

# 2. Item Model & Ownership — 6A

## 2.1 Item Definitions

Reusable equipment is represented by an **Item Definition**.

An Item Definition may contain:

- Name
- Item type
- Description
- Base value
- Weight
- Rarity
- Mechanical properties
- Usage rules
- Equipment requirements
- Stackability
- Tags
- Other definition-level properties

Example:

```text
Item Definition:
Longsword

Type: Weapon
Damage: 1d8
Damage Type: Slashing
Weight: 3 lb
Properties: Versatile
````

Definitions describe what an item **is**.

## 2.2 Item Instances

A specific persistent object in a campaign is represented by an **Item Instance** referencing an Item Definition.

An Item Instance may contain:

* Unique ID
* Definition reference
* Current owner
* Current location
* Container
* Quantity
* Charges
* Equipped state
* Condition
* Identified state
* Custom name
* Hidden properties
* Campaign-specific metadata

Instances describe a particular object that **exists in the world**.

## 2.3 Definition vs Instance

RealmWeaver must distinguish:

```text
Item Definition
    ↓
Item Instance
```

Example:

```text
Definition:
Longsword

Instances:
Longsword #001 — Player
Longsword #002 — Guard
Longsword #003 — Blacksmith
```

## 2.4 Ownership and Location

Items may exist in:

* Player inventory
* NPC inventory
* Merchant inventory
* Containers
* Persistent storage
* World locations
* Loot sources

An item must have one authoritative location/state at a time.

## 2.5 Owned vs Carried

Ownership does not imply that the item is currently carried.

Example:

```text
Player House:
Plate Armour
```

The player may own the armour without having access to it while exploring elsewhere.

## 2.6 Stackable Items

Appropriate items may use quantities.

Examples:

* Arrows
* Rations
* Common consumables
* Coins

Mechanically distinct items must not be incorrectly merged into the same stack.

## 2.7 Unique Items

Unique Item Instances must not be accidentally duplicated.

Truly unique content may additionally use:

```text
unique = true
```

to prevent multiple campaign instances from being generated where appropriate.

## 2.8 Item Condition

V1 does **not** use general numerical durability.

RealmWeaver does not simulate:

```text
Durability: 100 → 99 → 98
```

for ordinary equipment use.

Explicit states may still exist when mechanically or narratively relevant:

* Normal
* Damaged
* Broken
* Destroyed

---

# 3. Currency & Wealth — 6B

## 3.1 Supported Currency

V1 supports:

* Gold Pieces (GP)
* Silver Pieces (SP)
* Copper Pieces (CP)
* Platinum Pieces (PP)

Electrum is excluded from V1.

## 3.2 Currency Conversion

RealmWeaver uses the supported denomination relationships between GP, SP, CP and PP.

Currency must remain mechanically structured rather than existing only in AI narration.

## 3.3 Currency Ownership

Currency may belong to:

* Players
* NPCs
* Merchants
* Containers
* Treasure sources
* Other persistent world entities

## 3.4 Important NPC Wealth

Important/persistent NPCs may have explicit wealth and possessions.

Lightweight NPCs do not require fully materialised inventories until mechanically necessary.

When an NPC becomes mechanically important, RealmWeaver may materialise appropriate persistent possessions based on context and profile.

Once materialised, those possessions become authoritative.

## 3.5 Currency Transactions

Currency changes must be validated.

The AI cannot independently award, remove, or transfer authoritative currency.

## 3.6 Wealth Rewards

Currency may be obtained through:

* Loot
* Quests
* Trade
* Rewards
* Exploration
* Theft
* Other mechanically validated events

## 3.7 Coin Weight

Coin weight is only mechanically relevant when Encumbrance is enabled.

When enabled:

> **50 carried coins = 1 lb**

This applies regardless of denomination.

Currency is not automatically converted into higher denominations merely to reduce carried weight.

---

# 4. Weapons — 6C

## 4.1 Weapon Definitions

Weapons use structured Item Definitions.

A weapon may contain:

* Name
* Category
* Damage dice
* Damage type
* Weight
* Cost
* Properties
* Range
* Ammunition requirements
* Proficiency category
* Other mechanical properties

## 4.2 Weapon Categories

V1 supports appropriate simple and martial weapon categories required by supported classes/content.

## 4.3 Weapon Properties

RealmWeaver should structurally support relevant properties such as:

* Light
* Finesse
* Versatile
* Two-Handed
* Heavy
* Reach
* Thrown
* Ammunition
* Loading
* Other supported properties

## 4.4 Weapon Proficiency

Weapon proficiency is determined by class/features and stored in authoritative character state.

The AI does not infer proficiency from narrative context.

## 4.5 Attack Resolution

Weapons provide mechanical data to the Combat system.

The Combat system determines:

* Attack modifier
* Damage
* Applicable ability
* Proficiency
* Critical behaviour
* Range legality
* Other combat effects

The AI narrates the result.

## 4.6 Finesse Weapons

Finesse weapons allow the supported choice between relevant ability modifiers.

The rules engine determines the valid calculation.

## 4.7 Versatile Weapons

Versatile weapons support appropriate one-handed and two-handed damage configurations.

## 4.8 Two-Handed Weapons

Two-handed weapons require both hands while actively being used.

They cannot simultaneously be actively used with a shield or another held weapon.

## 4.9 Two-Weapon Fighting

Basic two-weapon fighting is included in V1.

RealmWeaver supports:

```text
Main Hand:
Eligible Light Weapon

Off Hand:
Eligible Light Weapon
```

The Combat system determines the resulting Bonus Action attack and damage rules.

More advanced dual-wielding features may be added in later versions.

## 4.10 Ammunition

Ranged weapons requiring ammunition consume appropriate ammunition according to their weapon rules.

Ammunition is authoritative inventory state.

## 4.11 Improvised Weapon Use

Creative weapon use may be interpreted by the AI.

The rules engine determines the appropriate mechanical treatment.

The AI cannot invent arbitrary damage or bonuses.

---

# 5. Armour & Shields — 6D

## 5.1 Armour Definitions

Armour uses reusable structured Item Definitions.

Armour data may contain:

* Category
* Base AC
* Dexterity rules
* Strength requirement
* Stealth effect
* Weight
* Cost
* Proficiency category
* Don time
* Doff time
* Special properties

## 5.2 Armour Categories

V1 supports:

* Light Armour
* Medium Armour
* Heavy Armour
* Shields

## 5.3 Light Armour

Light Armour normally calculates:

> **Base AC + full Dexterity modifier**

## 5.4 Medium Armour

Medium Armour normally calculates:

> **Base AC + eligible Dexterity modifier, capped at +2**

unless a supported feature modifies this.

## 5.5 Heavy Armour

Heavy Armour generally provides fixed AC without adding the character's Dexterity modifier.

## 5.6 Shields

A properly equipped Shield normally provides:

> **+2 AC**

where compatible with the current equipment configuration.

## 5.7 Hand Occupancy

A Shield occupies a hand.

RealmWeaver must validate weapon/shield combinations.

Example:

```text
Longsword + Shield
✓

Greatsword + Shield
✗
```

## 5.8 Armour Proficiency

RealmWeaver uses **Option A** for non-proficient armour.

Characters may wear armour they are not proficient with where the supported rules permit it, but the appropriate non-proficiency penalties apply.

RealmWeaver does not simply prevent equipping it.

## 5.9 Shield Proficiency

Shield proficiency follows the same principle.

## 5.10 Strength Requirements

Some Heavy Armour may have Strength requirements.

Failure to meet the requirement applies the supported mechanical consequence rather than automatically preventing the armour from being worn.

## 5.11 Stealth

Appropriate armour may impose Disadvantage on Stealth.

This is a structured mechanical property.

## 5.12 Automatic AC Recalculation

AC must automatically recalculate when relevant state changes.

Examples include:

* Armour equipped
* Armour removed
* Shield equipped
* Shield removed
* Dexterity changes
* Class feature changes
* Magical effect changes

## 5.13 Active Armour

A character normally has only one active worn armour configuration.

Multiple armour sets cannot be stacked for AC.

## 5.14 Donning and Doffing

Armour retains meaningful don/doff times.

When no danger or meaningful time pressure exists, RealmWeaver may automatically fast-forward the required time.

When time matters, the actual duration must be respected.

## 5.15 Resting and Armour State

Equipped armour state persists through world events.

If a character removes armour before sleeping, it remains removed unless explicitly equipped again.

Detailed resting consequences are defined in Group 8.

## 5.16 Unarmoured AC

The normal unarmoured baseline is:

> **10 + Dexterity Modifier**

unless another valid AC formula applies.

## 5.17 Alternative AC Formulas

RealmWeaver must support alternative AC calculations from future features such as **Unarmored Defense**.

Possible calculations may include:

```text
Normal:
10 + DEX

Class Feature:
10 + DEX + another ability

Armour:
Armour-specific calculation
```

Alternative AC formulas generally **do not stack** with each other.

RealmWeaver determines which formulas are valid and applies compatible additional modifiers.

This architecture must support future classes such as those with dedicated unarmoured defence mechanics.

## 5.18 Magical Armour

The architecture supports magical modifiers extending base armour definitions.

Detailed magic-item mechanics are defined later.

## 5.19 Armour Durability

V1 has no general armour durability system.

Explicit damaged/broken states may still exist.

## 5.20 Armour Loot

Armour worn by a defeated creature does not automatically enter player inventory.

It remains contextual loot until deliberately taken.

## 5.21 Armour Sizing

V1 does not implement detailed armour-sizing or alteration simulation.

Small/Medium humanoid equipment compatibility should generally remain permissive unless an item explicitly states otherwise.

## 5.22 Hidden Armour Properties

Architecture may support unidentified magical armour or hidden properties.

System knowledge and player knowledge may differ.

---

# 6. Inventory & Equipment — 6E

## 6.1 Inventory

Inventory represents items currently carried by the character.

The UI may organise items into categories such as:

* Weapons
* Armour
* Consumables
* Ammunition
* Adventuring Gear
* Quest Items
* Containers
* Miscellaneous

## 6.2 Item Availability States

RealmWeaver distinguishes:

```text
Owned
Stored
Carried
Accessible
Equipped
```

These are not equivalent.

## 6.3 Lightweight Equipment Slots

V1 uses a lightweight equipment-slot system.

Required states include:

* Armour
* Main Hand
* Off Hand
* Two-Handed weapon state where appropriate

Additional slots should only be introduced when mechanically necessary.

## 6.4 Hand State

RealmWeaver validates legal hand configurations.

Examples:

```text
Longsword + Shield
```

```text
Dagger + Dagger
```

```text
Greatsword occupying both hands
```

## 6.5 Drawing and Switching Weapons

One accessible ordinary weapon may normally be drawn using reasonable free interaction where supported.

Complex loadout changes may require additional interactions/actions.

RealmWeaver should avoid unnecessary hand-movement micromanagement while preserving meaningful action economy.

## 6.6 Dropping Items

Dropped items transfer from the character to the current world location.

They are not deleted.

## 6.7 Picking Up Items

Picking up an item depends on:

* Location
* Accessibility
* Current situation
* Action economy
* Other relevant constraints

## 6.8 Containers

Containers may hold other Item Instances.

Examples:

```text
Backpack
├── Rope
├── Rations
└── Torch ×5
```

V1 does not require detailed volume simulation.

## 6.9 Nested Containers

Nested containers may be supported structurally.

The UI should avoid encouraging excessive nesting complexity.

## 6.10 Quest Items

Quest items are persistent Item Instances with appropriate quest metadata.

Quest items are not universally:

* Undroppable
* Unsellable
* Indestructible

unless explicitly required.

The world/quest system must react to their loss where appropriate.

## 6.11 Consumable Accessibility

V1 does not require a videogame-style consumable hotbar.

A carried and reasonably accessible consumable may be used according to its item rules.

## 6.12 Inventory Sorting

The UI may support filtering/sorting by:

* Type
* Name
* Weight
* Value
* Equipped status
* Quest relevance

Sorting does not change authoritative state.

## 6.13 AI Inventory Context

The AI should not receive the entire inventory on every turn.

RealmWeaver should provide:

* Equipped items
* Relevant carried items
* Contextually necessary inventory information

Future AI tools/services may query authoritative inventory state when necessary.

## 6.14 Equipment Changes

Equipment changes may trigger recalculation of:

* AC
* Attack options
* Damage
* Speed
* Stealth effects
* Available features
* Other derived mechanics

## 6.15 Combat Restrictions

Inventory management remains subject to Combat action economy.

The player cannot freely reorganise their entire inventory during a combat turn.

## 6.16 Outside Combat

Ordinary inventory management may be streamlined outside combat when no meaningful time pressure exists.

## 6.17 Persistent Storage

Items may be stored in persistent locations such as:

* Player housing
* Camps
* Chests
* Rooms
* Other storage locations

Stored items are not automatically available when the character is elsewhere.

## 6.18 Inventory Slot Limits

V1 has no arbitrary videogame-style inventory-slot limit.

When Encumbrance is enabled, weight provides the primary carrying limitation.

When Encumbrance is disabled, general carrying-capacity penalties are ignored.

---

# 7. Carrying Capacity & Encumbrance — 6F

## 7.1 Campaign Setting

Before starting a campaign, the player chooses:

```text
Encumbrance:
Enabled
or
Disabled
```

This choice becomes locked when the campaign begins.

## 7.2 Disabled Encumbrance

When disabled:

* Item weight remains stored.
* Equipment restrictions still apply.
* Armour Strength requirements still apply.
* Item locations remain meaningful.
* Carrying-capacity penalties are ignored.
* Coin weight is ignored.

## 7.3 Enabled Encumbrance

When enabled, RealmWeaver calculates total carried weight from relevant physical possessions.

This includes:

* Weapons
* Armour
* Equipment
* Consumables
* Ammunition
* Containers
* Container contents
* Currency
* Other carried physical items

## 7.4 Carrying Capacity

Base Carrying Capacity:

> **Strength Score × 15 lb**

Strength **score**, not Strength modifier, is used.

## 7.5 Size Modifiers

The architecture supports Size and feature-based carrying-capacity modifiers.

## 7.6 Graduated Encumbrance

When Encumbrance is enabled, RealmWeaver uses graduated thresholds.

### Normal

> Weight ≤ STR × 5

No Encumbrance penalty.

### Encumbered

> Weight > STR × 5 and ≤ STR × 10

The supported movement penalty applies.

### Heavily Encumbered

> Weight > STR × 10 and ≤ STR × 15

The supported stronger penalties apply, including appropriate movement and physical check/save consequences.

### Beyond Ordinary Carrying Capacity

> Weight > STR × 15

The character cannot normally carry the entire load through ordinary movement.

## 7.7 Distance Bands

Encumbrance modifies authoritative Speed.

RealmWeaver's movement system translates the resulting Speed into valid Distance Band movement.

The AI does not manually determine movement penalties.

## 7.8 Container Weight

Containers and their contents both contribute to carried weight.

Items must not be double-counted.

## 7.9 Stored Items

Owned items stored elsewhere do not contribute to current carried weight.

## 7.10 Equipped Item Weight

Equipped weapons, armour, shields, and other items still contribute to carried weight.

## 7.11 Coin Weight

When Encumbrance is enabled:

> **50 coins = 1 lb**

## 7.12 Threshold Warnings

RealmWeaver should warn the player when taking/transferring items would cross an Encumbrance threshold.

The player may still proceed where mechanically possible.

## 7.13 Push, Drag, and Lift

Carrying capacity is not the universal limit for all physical interaction.

RealmWeaver distinguishes:

* Carrying
* Lifting
* Pushing
* Dragging

Heavy world objects do not need to become inventory items.

## 7.14 Temporary Lifting

Temporarily lifting an object does not add it to inventory.

Example:

> Holding a fallen beam while an NPC escapes.

may be resolved as a physical action rather than inventory acquisition.

## 7.15 Recalculation

Encumbrance recalculates after relevant state changes, including:

* Picking up items
* Dropping items
* Buying/selling
* Consuming items
* Currency changes
* Strength changes
* Size changes
* Relevant magical effects

## 7.16 Encumbrance UI

When enabled, RealmWeaver should clearly display current load and relevant thresholds.

When disabled, carrying calculations should not unnecessarily clutter the primary UI.

## 7.17 Future Modifiers

Architecture must support future modifiers such as:

```text
carrying_capacity_multiplier
```

and equivalent larger-size/carrying features.

---

# 8. Consumables & Item Use — 6G

## 8.1 Consumables

Consumables are structured Item Definitions/Instances.

Examples:

* Potions
* Rations
* Torches
* Antitoxins
* Poisons
* Spell Scrolls
* Other single/multi-use items

## 8.2 Structured Effects

Mechanically significant item effects should be structured rather than prose-only.

Example:

```text
Healing Potion

effect_type = healing
amount = 2d4 + 2
```

## 8.3 Item Use Flow

Using a consumable generally follows:

Player Intent
→ Identify Item
→ Validate Ownership/Accessibility
→ Validate Target
→ Validate Requirements
→ Validate Action/Time Cost
→ Resolve Effect
→ Consume/Modify Item
→ Persist
→ AI Narration

## 8.4 Consumption Validation

Consumables must not be removed before valid use is established.

Invalid attempts generally leave the item unchanged.

## 8.5 Quantities

Stackable consumables reduce their authoritative quantity when used.

Quantity-zero stacks leave active inventory.

## 8.6 Healing

Healing consumables resolve mechanically.

Healing cannot normally raise Current HP above Maximum HP.

## 8.7 Potions in Combat

Drinking or administering an ordinary potion during combat normally requires an:

> **Action**

V1 does not use the common Bonus-Action self-potion homebrew rule.

This may become an optional rule in later versions.

## 8.8 Administering Consumables

Eligible consumables may be administered to another valid and accessible creature.

## 8.9 Food and Water

Food/rations and water may be represented and consumed.

Detailed hunger, hydration, survival, and resting consequences are deferred to later relevant rules.

V1 does not require a detailed survival simulation by default.

## 8.10 Consumption Models

RealmWeaver supports multiple models:

### Immediate

Example: Potion.

### Duration-Based

Example: Torch.

### Charge-Based

Example: Multi-use item.

## 8.11 Charges

Items may have:

```text
charges_current
charges_max
```

Recharge rules may later include:

* Rest
* Dawn
* Trigger
* Never
* Other supported conditions

Magic-specific recharge behaviour is defined later.

## 8.12 Poisons

Poison items are structurally supported.

Detailed poison effects interact with:

* Saving Throws
* Damage
* Conditions
* Duration
* Immunity

and belong to the appropriate rules systems.

## 8.13 Consumable Targets

Consumables may target:

* Self
* Creature
* Item
* Object/location
* Other supported target types

## 8.14 Spell Scrolls

Spell Scrolls are represented as consumable Item Instances.

Their spellcasting mechanics are defined in Group 7 — Magic.

## 8.15 Unidentified Consumables

V1 supports unidentified potions and similar items.

Example player-visible state:

> A cloudy violet potion.

RealmWeaver may internally know its true definition while the player does not.

## 8.16 System Knowledge vs Player Knowledge

RealmWeaver distinguishes:

```text
SYSTEM KNOWLEDGE:
Actual item identity/properties

PLAYER KNOWLEDGE:
Discovered identity/properties
```

The AI must not reveal hidden properties without legitimate discovery.

## 8.17 Usage Requirements

Items may have requirements involving:

* Class
* Level
* Proficiency
* Target state
* Attunement in future
* Context
* Other supported mechanics

## 8.18 Creative Item Use

Unsupported standard item use does not mean unsupported player action.

Example:

> "I throw the potion at the skeleton."

The AI may interpret this as another structured action.

The rules engine then determines the mechanical result.

## 8.19 Outside Combat

Ordinary item use may be streamlined outside combat when time has no meaningful consequence.

## 8.20 AI-Created Consumables

AI may propose narratively appropriate consumables.

Mechanically significant effects must be mapped to supported content or validated before becoming authoritative.

## 8.21 Atomic Item Use

Mechanically significant item use should update the item and affected game state atomically where practical.

---

# 9. Loot, Containers & Discovery — 6H

## 9.1 Loot Sources

Loot may originate from:

* Defeated creatures
* NPCs
* Containers
* Quest rewards
* Exploration
* Hidden locations
* Theft
* Environmental discoveries
* Story events

## 9.2 Contextual Loot

Loot generation should consider:

* Source
* Creature/NPC type
* Location
* Character/campaign level
* Encounter difficulty
* Economy
* Narrative context
* Rarity restrictions

## 9.3 Loot Profiles

RealmWeaver may use structured Loot Profiles.

Example:

```text
BANDIT_LOW_LEVEL

Currency: Low
Equipment: Common
Valuables: Low Chance
Consumables: Low Chance
Rare/Magic: Very Low
```

Loot profiles provide controlled randomness.

## 9.4 Persistent NPC Inventory

If an NPC already has persistent possessions, those possessions are authoritative when the NPC is searched, robbed, or defeated.

RealmWeaver must not reroll their inventory.

## 9.5 Loot Materialisation

Lightweight NPCs/loot sources may initially contain profiles rather than exact contents.

When exact possessions become mechanically relevant, RealmWeaver may materialise them.

After materialisation, they become persistent.

## 9.6 Generate Once, Persist Afterwards

> **Randomness occurs once. Persistence occurs afterward.**

Opening/searching the same container again does not reroll its contents.

## 9.7 Container State

Containers may track relevant state such as:

* Open/closed
* Locked
* Hidden
* Trapped
* Broken
* Sealed
* Owner
* Location
* Contents materialised

## 9.8 Locked Containers

Locked containers may support:

* Keys
* Lockpicking
* Breaking
* Magic
* Other creative solutions

## 9.9 Breaking Containers

Players may attempt to break containers.

Possible consequences include:

* Noise
* Time
* Damaged contents
* Triggered traps
* NPC reactions

## 9.10 Hidden Containers and Objects

Hidden objects remain unknown until legitimately discovered.

Discovery may involve:

* Appropriate searching
* Passive skills
* Ability checks
* Clues
* Spells/features
* Specific interactions

## 9.11 Search Scope

Searching must consider what and where the character is actually searching.

A successful roll does not automatically reveal unrelated secrets elsewhere.

## 9.12 Specific Searching

Specific player descriptions may affect discovery resolution.

Example:

> "I check underneath the desk and examine the drawers for false bottoms."

may be more effective than:

> "I look around."

where contextually appropriate.

## 9.13 Discovery State

Discoverable objects may conceptually progress through:

```text
UNDISCOVERED
→ DISCOVERED
→ INTERACTED
→ RESOLVED / LOOTED
```

## 9.14 Exploration Rewards

Discovery may reward:

* Items
* Currency
* XP
* Quest progress
* New goals
* Locations
* Lore
* Clues
* Shortcuts
* NPC/faction information

Physical loot is not required.

## 9.15 Treasure Categories

Treasure may include:

* Currency
* Gems
* Jewellery
* Art objects
* Trade goods
* Valuable materials
* Relics
* Story valuables

## 9.16 Buyers

Specialised treasure may require an appropriate buyer.

Not every merchant purchases every item category.

## 9.17 Rare Loot

Rare/powerful loot uses stricter generation restrictions.

Powerful items should require an appropriate source and context.

## 9.18 Quest Loot

Required quest items override random loot generation.

Random treasure must not replace required quest content.

## 9.19 Creature Loot

Not every defeated creature produces valuable treasure.

Loot eligibility depends strongly on creature/context.

## 9.20 Harvestable Resources

Architecture may support resources such as:

* Venom
* Hides
* Scales
* Herbs
* Alchemical materials

A full harvesting/crafting economy is outside initial V1 scope.

## 9.21 Loot Discovery

Defeating an enemy does not automatically reveal all possessions.

Obvious visible equipment may be apparent.

Other possessions may require searching.

## 9.22 Discovery vs Acquisition

RealmWeaver distinguishes:

```text
Discover
→ Inspect
→ Take
→ Inventory
```

Loot does not automatically enter player inventory.

## 9.23 Take All

V1 supports a convenience **Take All** operation.

RealmWeaver must check resulting Encumbrance before performing relevant batch transfers.

## 9.24 Loot Persistence

Uncollected loot persists unless legitimate world-state changes affect it.

Items must not disappear because the AI forgot about them.

## 9.25 AI Loot Proposals

AI may propose contextually interesting treasure.

Mechanically significant proposals must be validated before becoming persistent items.

## 9.26 Loot Randomness

Controlled loot randomness belongs to RealmWeaver's randomisation/content systems rather than being delegated entirely to the LLM.

AI provides contextual narrative flavour.

---

# 10. Item State, Trading & Validation — 6I

## 10.1 Authoritative State

Every persistent Item Instance has one authoritative current state.

AI narration cannot override stored state.

## 10.2 Structured Item Operations

Supported operations may include:

```text
TAKE
DROP
TRANSFER
EQUIP
UNEQUIP
USE
CONSUME
BUY
SELL
GIVE
STEAL
STORE
RETRIEVE
DESTROY
THROW
```

The AI requests/interprets actions rather than directly modifying database fields.

## 10.3 Transaction Validation

Transactions validate all relevant requirements before execution.

## 10.4 Atomic Transactions

Mechanically significant transactions should be atomic.

Example:

```text
Validate
↓
Transfer Item
+
Transfer Currency
↓
Commit
```

If the transaction fails, partial state changes must not remain.

## 10.5 Buying

Buying validates:

* Merchant
* Availability
* Stock
* Price
* Player funds
* Purchase restrictions

## 10.6 Merchant Stock

Merchant stock persists.

Purchased items do not immediately regenerate without a legitimate restock mechanism.

## 10.7 Restocking

Ordinary stock may replenish through controlled Restock Profiles after appropriate campaign time.

Unique/rare items do not automatically respawn.

## 10.8 Merchant Specialisation

Merchants buy/sell appropriate categories.

Examples:

### Blacksmith

* Weapons
* Armour
* Metal equipment

### Alchemist

* Potions
* Ingredients
* Alchemical supplies

### Jeweller

* Gems
* Jewellery
* Precious metals

## 10.9 Pricing

Transaction prices may consider:

* Base value
* Merchant type
* Location/economy
* Item state
* Relationship/reputation
* Haggling
* Context

V1 does not require a complex dynamic economic simulation.

## 10.10 Buy vs Sell Prices

Merchant purchase and sale prices may differ.

The exact economic spread can be tuned during implementation/playtesting.

## 10.11 Haggling

Haggling is supported.

Relevant checks may produce **bounded** price adjustments.

Extreme rolls do not produce absurd results such as free high-value merchandise without appropriate circumstances.

## 10.12 Bartering

**Basic bartering is included in V1.**

Players may propose transactions using combinations of:

* Currency
* Items
* Valuables

Example:

> "I'll give you 40 GP and this emerald for the sword."

The merchant may accept/reject based on value, desirability, context, and relationship.

V1 does not require a complex supply-and-demand economy.

## 10.13 Giving Items

Items may be transferred to NPCs when accepted.

Persistent NPC inventory should update accordingly.

## 10.14 Recipient Refusal

NPCs may refuse voluntary transfers.

A player's attempt to give an item does not automatically force the item into the NPC's inventory.

## 10.15 Theft

Theft requires appropriate action resolution before possession changes.

Failure may produce:

* Detection
* NPC reactions
* Guards
* Reputation consequences
* Combat
* Other world consequences

## 10.16 Pickpocketing

Pickpocketing considers:

* Target
* Accessible possession
* Relevant check
* Detection
* Success/failure

Successful resolution occurs before authoritative transfer.

## 10.17 Dropped and Abandoned Items

Dropped items remain persistent world objects.

World events may later legitimately move, destroy, or transfer them.

## 10.18 Item Destruction

Items may be destroyed where context and mechanics permit.

Important quest items are not universally protected.

## 10.19 Quest Consequences

If an important item is:

* Sold
* Lost
* Destroyed
* Stolen

the Quest system must react appropriately.

Possible consequences include:

* Objective failure
* Alternative solution
* Recovery objective
* New quest branch

## 10.20 Item Reservation

RealmWeaver may internally reserve/lock an item during an active transaction to prevent conflicting simultaneous operations.

## 10.21 Stack Merging

Mechanically equivalent stackable items may merge.

Mechanically different items must not incorrectly merge.

## 10.22 Stack Splitting

Stacks may be split for:

* Partial transfer
* Giving
* Selling
* Storage
* Other relevant operations

## 10.23 Current-State Validation

Every item action must validate against current authoritative state.

The AI cannot rely on stale narrative assumptions.

## 10.24 AI Contradiction

If AI narration conflicts with authoritative state:

> **RealmWeaver state wins.**

The narrative should be corrected/regenerated where necessary.

## 10.25 Transaction Events

Meaningful item interactions may create structured events such as:

```text
ITEM_TRANSFERRED
ITEM_DESTROYED
ITEM_PURCHASED
ITEM_STOLEN
```

These may support:

* Campaign history
* Quest reactions
* AI context
* Debugging

Mundane operations do not need to clutter major campaign history.

---

# 11. Tools & Tool Proficiencies — 6J

## 11.1 Tool Items

Tools use the existing Item Definition/Instance architecture.

They participate normally in:

* Inventory
* Ownership
* Weight
* Storage
* Trading
* Loot
* Theft
* Item state

## 11.2 Tool Categories

RealmWeaver structurally supports categories such as:

### Artisan's Tools

* Alchemist's Supplies
* Smith's Tools
* Carpenter's Tools
* Brewer's Supplies
* Other appropriate artisan tools

### Specialist Kits

* Thieves' Tools
* Disguise Kit
* Forgery Kit
* Herbalism Kit
* Poisoner's Kit

### Other Tools

* Gaming sets
* Musical instruments
* Healer's Kit
* Other supported equipment

V1 does not require exhaustive implementation of every possible tool.

## 11.3 Possession vs Proficiency

RealmWeaver distinguishes:

```text
HAS TOOL
≠
HAS TOOL PROFICIENCY
```

A character may possess a tool without being proficient.

A character may be proficient without currently possessing the tool.

## 11.4 Tool Proficiency Sources

Tool Proficiency may come from:

* Background
* Class
* Species where supported
* Future feats
* Training
* Other supported features

Tool proficiencies are stored in authoritative character state.

## 11.5 Flexible Ability + Tool Checks

RealmWeaver does **not** permanently bind each tool to one Ability.

A tool check may use:

> **d20 + appropriate Ability Modifier + Proficiency Bonus if applicable**

Example:

```text
Picking a lock:
DEX + Thieves' Tools

Studying an unusual lock mechanism:
INT + Thieves' Tools
```

The contextual Ability may differ when justified.

## 11.6 AI and Tool Checks

The AI may propose:

* Relevant tool
* Relevant Ability
* Intended action

RealmWeaver validates:

* Tool possession
* Accessibility
* Proficiency
* Relevant Ability
* DC
* Other mechanical requirements

The rules engine calculates the final modifier.

## 11.7 Required vs Beneficial Tools

Actions may classify a tool as:

### Required

The action normally requires suitable equipment.

### Beneficial

The action is possible without the tool, but relevant tool use may improve resolution.

## 11.8 Improvised Tools

Players may attempt creative substitutes when appropriate.

Example:

> Using bent wire instead of Thieves' Tools.

RealmWeaver may determine:

* Attempt allowed
* Disadvantage
* Different DC
* Other consequence
* Attempt impossible

depending on context.

Improvised equipment does not automatically become a proper Tool Item.

## 11.9 Missing Tool

Proficiency alone does not create required physical equipment.

A proficient character without their tools may need to:

* Improvise
* Borrow
* Find
* Purchase

appropriate equipment.

## 11.10 Missing Proficiency

Possessing a tool does not automatically grant Proficiency Bonus.

Some actions may still be attempted without proficiency.

Some specialised actions may explicitly require proficiency.

## 11.11 Expertise

Tool Expertise is structurally supported.

Conceptually:

```text
NONE
PROFICIENT
EXPERTISE
```

Expertise applies the appropriate enhanced Proficiency Bonus where supported.

## 11.12 Duplicate Proficiency

Duplicate Tool Proficiency does not numerically stack.

Applicable character-creation replacement/selection rules should handle duplicate grants where necessary.

## 11.13 Thieves' Tools

Thieves' Tools support appropriate interactions including:

* Lockpicking
* Trap disarming
* Mechanical device manipulation

Trap discovery and trap disarming are separate operations.

## 11.14 Alchemist's Supplies

Alchemist's Supplies may support:

* Substance identification
* Chemical analysis
* Alchemical knowledge
* Relevant quest interactions

Full potion crafting is outside initial V1 scope.

## 11.15 Herbalism Kit

The Herbalism Kit may support:

* Herb identification
* Medicinal plant recognition
* Relevant botanical interactions

A full herbalism/crafting system is outside initial V1 scope.

## 11.16 Healer's Kit

Healer's Kits may use the Consumables/Charges architecture.

Their specific mechanical effects follow supported rules.

## 11.17 Disguise Kit

Disguise Kits may support:

* Creating disguises
* Impersonation preparation
* Persistent disguise state
* Relevant checks

Resolution may consider:

* Materials
* Time
* Proficiency
* Target appearance
* Context

## 11.18 Forgery Kit

Forgery Kits may create persistent forged objects.

Example:

```text
Forged Travel Permit

authentic = false
quality = ...
```

Future NPC interactions may detect the forgery.

## 11.19 Poisoner's Kit

Poisoner's Kits may support:

* Poison identification
* Safe handling
* Application
* Extraction where supported

Detailed poison mechanics belong to relevant item/condition rules.

## 11.20 Smith's Tools

Smith's Tools may support:

* Metalwork analysis
* Workmanship identification
* Contextual repairs
* Story interactions

V1 does not use routine numerical weapon durability.

## 11.21 Musical Instruments

Musical instruments may function as:

* Items
* Tool Proficiencies
* Roleplay objects

They may support activities such as:

* Performance
* Earning money
* Attracting attention
* NPC interaction

## 11.22 Skill + Tool Synergy

Multiple relevant proficiencies must not simply stack Proficiency Bonus multiple times.

Where both a Skill and Tool Proficiency are relevant, RealmWeaver may provide controlled benefits such as:

* Additional information
* Advantage where appropriate
* Different discoveries
* Contextual benefit

according to supported rules.

## 11.23 Tool Activity Time

Tool interactions may require meaningful time.

Examples:

* Picking a lock — short
* Creating a disguise — longer
* Forging a document — potentially hours

When time has no meaningful consequence, RealmWeaver may fast-forward appropriately.

## 11.24 Tool State

Tools follow normal Item State rules.

They may be:

* Carried
* Stored
* Dropped
* Sold
* Stolen
* Destroyed

General numerical Tool Durability is not required.

## 11.25 Creative Tool Use

AI may propose tool applications not explicitly hardcoded.

Example:

> Using Carpenter's Tools to analyse a hidden wooden mechanism.

The AI may propose:

```text
Tool: Carpenter's Tools
Ability: Intelligence
Action: Analyse Mechanism
```

RealmWeaver validates the proposed interaction.

## 11.26 AI Restrictions

The AI cannot invent:

* Tool ownership
* Tool Proficiency
* Expertise
* Mechanical bonuses

Authoritative character and inventory state determines these.

## 11.27 AI Context

The AI should receive/query relevant tool capabilities rather than receiving every tool rule every turn.

Example:

```text
Relevant Capabilities:

Thieves' Tools — Expertise
Disguise Kit — Proficient

Currently Carried:
Thieves' Tools
Disguise Kit
```

## 11.28 Future Crafting Architecture

The Tool system should support future expansion into:

```text
TOOLS
+
MATERIALS
+
RECIPES
+
CHECKS
+
TIME
↓
CRAFTING
```

Possible future systems include:

* Potion brewing
* Smithing
* Poison creation
* Scroll creation
* Cooking
* Enchanting

**Full crafting is outside initial V1 scope.**

---

# 12. V1 Scope Summary

## 12.1 Included in V1

V1 includes:

* Structured Item Definitions
* Persistent Item Instances
* Item ownership/location
* GP/SP/CP currency
* Weapons
* Basic two-weapon fighting
* Armour and Shields
* Armour proficiency penalties
* Alternative AC architecture
* Inventory/equipment state
* Containers
* Optional Encumbrance
* Graduated Encumbrance rules
* Coin weight when Encumbrance is enabled
* Consumables
* Item charges
* Unidentified consumables
* Loot generation profiles
* Persistent loot
* Hidden containers/discoveries
* Treasure/valuables
* Merchant inventories
* Buying and selling
* Haggling
* Basic bartering
* Theft/pickpocketing integration
* Item destruction/loss
* Tool items
* Tool Proficiencies
* Flexible Ability + Tool checks
* Improvised tool attempts
* Tool Expertise architecture

## 12.2 Deferred / Future Features

The following are intentionally outside initial V1 or deferred for later specification:

* General equipment durability simulation
* Detailed armour fitting
* Electrum currency
* Complex dynamic economic simulation
* Detailed container-volume simulation
* Full crafting system
* Potion-brewing system
* Full harvesting economy
* Detailed herbalism system
* Complex poison crafting
* Extensive magical item rules
* Attunement rules
* Advanced dual-wielding features
* Exhaustive tool catalogue
* Complex survival/hydration simulation
* Detailed magic-item recharge behaviour

Where possible, V1 architecture should avoid preventing these features from being added later.

---

# 13. Cross-System Dependencies

Group 6 interacts with:

### Character Core

* Ability Scores
* Size
* Proficiencies
* Character state

### Checks & Saves

* Tool checks
* Lockpicking
* Searching
* Theft
* Haggling
* Tool interactions

### Dice & Inspiration

* Tool/check resolution
* Random loot generation where appropriate

### Combat

* Weapon attacks
* Armour Class
* Shields
* Two-weapon fighting
* Item use
* Action economy
* Loot after combat
* Movement penalties

### Classes & Progression

* Weapon proficiency
* Armour proficiency
* Tool proficiency
* Expertise
* Future class equipment features

### Magic

* Spell Scrolls
* Magical weapons
* Magical armour
* Magical consumables
* Item charges
* Identification
* Future attunement

### Conditions & Resting

* Poison
* Item-based conditions
* Food/survival
* Rest-related recharge
* Armour/rest interaction

### Quest & World Systems

* Quest items
* Persistent loot
* Merchants
* Theft
* Item loss
* Hidden discoveries
* NPC possessions
* World storage

### AI Context System

* Relevant inventory retrieval
* Item knowledge
* Tool capability retrieval
* Loot proposals
* Structured item actions
* Player/system knowledge separation

---

# 14. Group 6 Completion Status

**Group 6 — Equipment & Inventory: COMPLETE**

Approved subsections:

* 6A — Item Model & Ownership
* 6B — Currency & Wealth
* 6C — Weapons
* 6D — Armour & Shields
* 6E — Inventory & Equipment
* 6F — Carrying Capacity & Encumbrance
* 6G — Consumables & Item Use
* 6H — Loot, Containers & Discovery
* 6I — Item State, Trading & Validation
* 6J — Tools & Tool Proficiencies

Next Rules Group:

> **Group 7 — Magic**

