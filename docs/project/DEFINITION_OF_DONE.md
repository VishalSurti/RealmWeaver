# RealmWeaver — Definition of Done

**Document Version:** 1.0
**Last Reviewed:** 7 August 2026
**Status:** Approved Completion Standard for Future Authorized V1 Development

---

# 1. Purpose

This document defines the minimum quality standard required before RealmWeaver work is considered complete.

A feature or backlog item is not considered done simply because it appears to work.

The Definition of Done ensures that completed work is:

* Correct
* Testable
* Reviewed
* Maintainable
* Documented
* Safe to integrate into the project

---

# 2. Standard Definition of Done

A RealmWeaver backlog item may be marked **Done** only when all applicable conditions below are satisfied.

## 2.1 Requirement Satisfied

The implementation satisfies its linked:

* Functional requirements
* User story
* Acceptance criteria

Any intentional deviation must be documented and reviewed.

---

## 2.2 Implementation Works

The expected behaviour works under normal supported conditions.

The feature must not contain known defects that prevent its intended use.

---

## 2.3 Code Review Completed

The implementation has been reviewed for:

* Correctness
* Readability
* Naming
* Structure
* Maintainability
* Error handling
* Separation of concerns
* Unnecessary duplication
* Security concerns where applicable

Required review changes must be addressed before completion.

---

## 2.4 Appropriate Tests Exist

Applicable tests have been written and pass successfully.

Depending on the feature, this may include:

* Unit tests
* Integration tests
* API tests
* Manual acceptance tests
* AI evaluation scenarios

Not every feature requires every test type.

---

## 2.5 Existing Behaviour Remains Stable

Relevant existing functionality has been checked to ensure the new change has not introduced unintended regressions.

Automated regression tests should be used where available.

---

## 2.6 Error Cases Are Handled

Expected failure conditions and invalid inputs are handled appropriately.

The application should not:

* Crash unnecessarily
* Corrupt game state
* Expose raw internal errors to the player
* Leave partially applied state where atomic behaviour is required

---

## 2.7 Documentation Is Updated

Relevant documentation has been updated when required.

This may include:

* Requirements
* Architecture documentation
* Game Rules Specification
* API documentation
* Architecture Decision Records
* Sprint documentation
* Technical debt records
* Changelog
* README

Documentation should only be updated where the implementation meaningfully affects it.

---

## 2.8 Git Hygiene Is Complete

Before completion:

* No secrets or credentials are committed.
* Debug files and temporary artifacts are removed.
* Commits use meaningful messages.
* Changes are committed to the appropriate branch.
* Merge conflicts are resolved.
* Repository status is clean where expected.

---

## 2.9 Acceptance Criteria Verified

Each applicable acceptance criterion has been explicitly checked.

A feature cannot be considered complete while required acceptance criteria remain unverified.

---

## 2.10 Technical Debt Is Recorded

If a known shortcut, limitation, or future improvement is intentionally accepted, it must be recorded as technical debt rather than silently ignored.

Recorded technical debt should explain:

* What the limitation is
* Why it was accepted
* Potential future impact

---

## 2.11 Visual and UX Quality Is Verified

Important player-facing features have completed applicable visual/UX review.

Review must verify all applicable criteria:

* Coherent RealmWeaver fantasy identity.
* Consistent colours, typography, spacing, components, and iconography.
* Clear distinction between narration, dialogue, player choices, and mechanics.
* Clear communication of authoritative state changes.
* Readable contrast and accessible interaction states.
* Designed loading, empty, disabled, success, error, and recovery states.
* Responsive behaviour on supported screen sizes.
* No obviously unfinished placeholder UI in the V1 release.

Applicable review coverage includes landing and campaign entry, character creation, the main campaign interface, character state/resources, dice and mechanical results, combat, inventory/equipment, spells/conditions, quests/objectives, and save/load/error/recovery states.

Elaborate animations, custom artwork for every entity, 3D environments, fully animated maps, generated voice/video, multiple complete visual themes, Dark Mode, and purely decorative effects are not universally required for completion.

---

# 3. Additional Definition for Major Features

For significant features, epics, or sprint deliverables, the work must also be:

## Demonstrable End-to-End

The feature must be usable through its intended workflow rather than existing only as isolated internal code.

For example, Character Creation is not considered complete merely because character objects can be created in Python.

A completed Character Creation feature should allow the supported workflow to function through the application and correctly persist the resulting character state.

---

# 4. Definition of Done vs Acceptance Criteria

Acceptance criteria describe:

> **What must the feature do?**

The Definition of Done describes:

> **What quality standard must the implementation meet before we accept it?**

Both must be satisfied.

---

# 5. Definition of Done vs Technical Debt

The project may occasionally accept technical debt while still marking a feature complete.

This is only acceptable when:

* The feature remains functionally correct.
* The debt does not create an unacceptable security or data-integrity risk.
* The limitation is explicitly documented.
* A future backlog item can address it if necessary.

Critical defects must not be reclassified as technical debt simply to mark work complete.

---

# 6. AI-Specific Completion Criteria

Features involving the AI Dungeon Master should additionally verify, where applicable:

* The AI respects authoritative game state.
* Mechanical outcomes are not silently overwritten.
* Player agency is preserved.
* Relevant campaign information reaches the model.
* AI failures do not corrupt authoritative state.
* Structured AI responses are validated before use.

Probabilistic AI quality does not need to be perfect, but known severe behavioural failures must be addressed or documented before release.

---

# 7. Completion Authority

During RealmWeaver development, completion will be determined through the project's professional-development workflow.

The developer implements the work.

The review process then checks:

1. Requirements
2. Code
3. Tests
4. Acceptance criteria
5. Documentation
6. Known technical debt

Only after these checks should the backlog item move to **Done**.

---

# 8. Definition of Done Principle

> **Working code is necessary, but working code alone is not Done.**

RealmWeaver considers quality, testing, maintainability, documentation, and integration part of completing a feature.
