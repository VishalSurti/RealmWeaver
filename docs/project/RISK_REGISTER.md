# RealmWeaver — Risk Register

**Document Version:** 1.0
**Last Reviewed:** 8 August 2026
**Status:** Approved for V1 Planning

---

# 1. Purpose

This document records known risks and constraints that may affect RealmWeaver Version 1.0.

A **risk** is an uncertain event that may negatively affect the project.

A **constraint** is a limitation that is already known and must be considered during planning and implementation.

The purpose of this register is to identify problems early, reduce avoidable surprises, and support better engineering decisions throughout development.

---

# 2. Risk Rating

Risks are assessed using:

## Likelihood

* **Low** — Unlikely to occur
* **Medium** — Could reasonably occur
* **High** — Expected or likely to occur

## Impact

* **Low** — Minor inconvenience
* **Medium** — Noticeable project disruption
* **High** — Major feature, schedule, or quality impact
* **Critical** — Could seriously compromise the project or saved campaign data

---

# 3. Risk Register

| ID    | Risk                                                                                                  | Likelihood          | Impact     | Mitigation                                                                                         |
| ----- | ----------------------------------------------------------------------------------------------------- | ------------------- | ---------- | -------------------------------------------------------------------------------------------------- |
| R-001 | Scope creep from continuously adding new RPG features                                                 | High                | High       | Maintain strict V1 scope; place new ideas in future backlog unless formally approved               |
| R-002 | AI Dungeon Master invents incorrect mechanical state                                                  | High                | High       | Keep the game engine authoritative; validate structured AI requests                                |
| R-003 | Long-term campaign memory becomes inconsistent                                                        | High                | High       | Use structured state, event records, recent context, and session summaries                         |
| R-004 | AI produces repetitive scenarios, NPCs, or descriptions                                               | Medium              | Medium     | Use campaign context, prompt refinement, and dedicated AI evaluation                               |
| R-005 | AI response latency negatively affects gameplay                                                       | Medium              | High       | Keep prompts efficient, control context size, provide loading feedback, select appropriate models  |
| R-006 | AI API costs increase as campaign context grows                                                       | Medium              | Medium     | Use summaries, selective context, usage monitoring, and cost-efficient models                      |
| R-007 | V1 game mechanics become significantly more complex than expected                                     | High                | High       | Define a strict V1 Game Rules Specification before implementation                                  |
| R-008 | Interacting game rules produce incorrect outcomes                                                     | High                | High       | Build modular mechanics and comprehensive deterministic tests                                      |
| R-009 | Campaign state becomes corrupted or partially saved                                                   | Medium              | Critical   | Validate mutations, use atomic state updates, checkpoints, and persistence tests                   |
| R-010 | AI and game-engine integration becomes overly complicated                                             | High                | High       | Maintain a strict structured contract between the AI and deterministic systems                     |
| R-011 | Solo-development workload causes loss of momentum or burnout                                          | Medium              | High       | Use manageable sprints, realistic capacity, prioritisation, and deliberate scope control           |
| R-012 | Development slows because unfamiliar technologies require learning                                    | High                | Medium     | Treat learning as planned work; use prototypes and guided implementation before production use     |
| R-013 | Architecture becomes unnecessarily complex                                                            | Medium              | Medium     | Design for V1 requirements rather than hypothetical commercial scale                               |
| R-014 | AI behaviour receives insufficient testing                                                            | Medium              | High       | Create repeatable AI evaluation scenarios and structured playtesting                               |
| R-015 | New changes break previously working functionality                                                    | Medium              | High       | Use automated tests, regression testing, code review, and CI                                       |
| R-016 | Selected AI provider changes APIs, pricing, limits, or model availability                             | Medium              | Medium     | Isolate provider-specific integration behind an abstraction layer                                  |
| R-017 | API keys or credentials are accidentally committed to GitHub                                          | Medium              | High       | Use environment variables, `.gitignore`, repository reviews, and secret scanning where practical   |
| R-018 | Project context is lost during long breaks in development                                             | Medium              | High       | Maintain Project Bible, milestone documentation, sprint records, ADRs, and status documents        |
| R-019 | Development estimates are inaccurate                                                                  | High                | Medium     | Track estimated vs actual effort and refine future sprint planning                                 |
| R-020 | Rules or intellectual-property licensing becomes problematic if scope later becomes public/commercial | Low for personal V1 | High later | Use appropriately licensed material, record sources, and review licensing before commercialisation |

---

# 4. Highest-Priority Risks

The following risks require particular attention throughout RealmWeaver development.

## R-001 — Scope Creep

RealmWeaver contains many opportunities for attractive additional features.

Examples include:

* Multiplayer
* Voice
* Maps
* More classes
* More spells
* Advanced NPC simulation
* Crafting
* Large procedural worlds

These features may be valuable but can prevent completion of V1.

### Response

New ideas should normally enter the future backlog rather than active development.

V1 scope may only change following deliberate review.

---

## R-002 — AI Mechanical Hallucination

The AI Dungeon Master may incorrectly claim that:

* HP changed
* An item was gained
* A spell slot was restored
* An NPC died
* A quest completed
* A dice result occurred

### Response

The AI is not authoritative.

Mechanical changes must be validated and applied through deterministic application logic.

---

## R-003 — Memory Inconsistency

Campaigns may continue across many sessions, increasing the chance of contradictory information.

### Response

RealmWeaver shall not rely solely on conversational AI memory.

Campaign continuity should combine:

* Structured state
* Important events
* NPC information
* Session summaries
* Relevant recent conversation

---

## R-007 — Game Rules Complexity

Attempting to implement the complete tabletop ruleset would significantly expand V1.

### Response

A formal V1 Game Rules Specification shall define exactly which mechanics are supported before the game engine is implemented.

Unsupported rules should remain outside V1.

---

## R-008 — Rule Interaction Defects

Individual mechanics may work correctly while combinations of mechanics produce incorrect results.

Examples may include:

* Advantage interacting with special rolls
* Equipment changing defence
* Damage interacting with conditions
* Level progression changing modifiers

### Response

Game mechanics should remain modular and receive dedicated unit and integration testing.

---

## R-010 — AI/Game-Engine Integration Complexity

Natural-language player actions may produce ambiguous or unusual requests.

Allowing the AI to interact freely with internal systems could make the application unreliable.

### Response

AI interactions should use structured, validated requests.

The flow should remain conceptually:

Player Action
→ AI Interpretation
→ Validated Game Action
→ Rules Engine
→ Authoritative State Update
→ AI Narration

---

# 5. Known Project Constraints

| ID    | Constraint                                                                 | Effect on Project                                                                 |
| ----- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| C-001 | RealmWeaver is developed by one developer                                  | Scope and sprint capacity must remain realistic for solo development              |
| C-002 | RealmWeaver is also a learning project                                     | Time spent learning professional workflows and technologies is valid project work |
| C-003 | V1 is primarily for personal use                                           | Commercial-scale architecture is unnecessary                                      |
| C-004 | Development hardware is limited for local LLM inference                    | AI models should primarily be accessed through external APIs                      |
| C-005 | Operating budget should remain low                                         | Prefer free or inexpensive development and hosting services                       |
| C-006 | Gameplay depends on an external AI provider                                | Internet availability and provider reliability may affect AI interactions         |
| C-007 | V1 is text-first                                                           | Voice, graphical battle maps, and complex visual simulation are excluded          |
| C-008 | V1 is single-player                                                        | Multiplayer synchronisation and concurrency are unnecessary for V1                |
| C-009 | V1 uses a deliberately limited game ruleset                                | Complete tabletop rules coverage is not a V1 objective                            |
| C-010 | Development is not full-time                                               | Sprint planning must reflect actual available capacity                            |
| C-011 | The repository is intended to demonstrate professional engineering quality | Documentation, testing, structure, and Git practices should remain presentable    |

---

# 6. Risk Response Strategy

RealmWeaver will generally use four approaches to risk.

## Avoid

Change the plan so the risk no longer exists.

Example:

Avoid multiplayer complexity by excluding multiplayer from V1.

## Reduce

Lower the likelihood or impact of the risk.

Example:

Reduce state corruption risk through transactions and validation.

## Accept

Recognise a risk without taking immediate action when mitigation cost exceeds its current value.

Example:

Accept that AI narration may occasionally be imperfect during early prototypes.

## Monitor

Continue observing the risk and act if indicators worsen.

Example:

Monitor API costs as campaign context increases.

---

# 7. Risk Review Process

The Risk Register should be reviewed:

* At major milestone reviews
* When significant architecture changes occur
* When a serious bug exposes a previously unidentified risk
* When V1 scope changes
* Before V1 release

New risks may be added at any time.

Existing risks may have their:

* Likelihood changed
* Impact changed
* Mitigation updated
* Status changed

---

# 8. Risk Escalation

A risk should receive immediate review when:

* It threatens authoritative campaign data.
* It creates a significant security concern.
* It threatens completion of a Must-Have V1 feature.
* It substantially expands expected project effort.
* It invalidates an important architecture decision.

If necessary, affected development work should pause until an appropriate response is agreed.

---

# 9. Constraints as Design Inputs

Constraints should not automatically be treated as problems.

For example:

Single-player V1 allows RealmWeaver to avoid unnecessary real-time synchronisation.

Personal-use scope allows infrastructure to remain simple.

A limited ruleset makes high-quality deterministic implementation more achievable.

The project should use its constraints to simplify development wherever possible.

---

# 10. Risk Management Principle

> **Identify uncertainty early, design around high-impact risks, and avoid solving problems that V1 does not actually have.**

Risk management for RealmWeaver should support development rather than create unnecessary bureaucracy.
