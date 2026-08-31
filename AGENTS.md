# AGENTS.md

## Repository Authority

RealmWeaver repository documentation is the authoritative source for project status, scope, requirements, rules, and approved decisions.

Repository documentation takes precedence over:

- AI memory
- Previous conversations
- Assumptions
- General conventions
- Earlier superseded decisions

Read the relevant documents before acting. Do not reconstruct project decisions from memory.

If authoritative documents conflict, stop and report the conflict with the affected files and passages. Do not silently choose an interpretation, rewrite a rule, or resolve the conflict without approval.

## Current Phase

- M1 — Product Foundation is complete.
- M2 — Technical Design & Architecture is active.
- M2.1 rules design is documented, but its completion gate has not passed.
- The next approved activity is the Group 9 internal consistency review.
- Production coding is not currently authorized.

Production coding may begin only after the required documentation gates—including the SRD/IP/content-provenance audit—are complete and the user explicitly approves implementation.

This section is a convenience summary only. `docs/project/PROJECT_STATUS.md` remains authoritative. When an approved milestone transition occurs, update this section and `PROJECT_STATUS.md` together.

## Scope and Approval

Work only on the explicitly approved task.

- Do not expand scope because adjacent work appears useful.
- Do not introduce deferred or future features.
- Do not modify V1 scope or approved rules without explicit approval.
- Do not create architecture, schemas, APIs, services, or implementation plans ahead of their approved M2 activity.
- Do not add, remove, or upgrade dependencies without explicit approval.
- Ask before making a decision that would materially change product behavior, rules, architecture, scope, or persistent data.

Before editing, state:

1. The files intended to change.
2. The proposed changes to each file.
3. Any assumptions, risks, or documentation conflicts that affect the work.

After presenting the intended files and proposed changes, wait for explicit user approval before editing. Understanding the task does not itself authorize changes.

## Preserve Existing Work

Treat all existing working-tree changes as user-owned.

- Preserve user changes, including unrelated or partially completed work.
- Never discard, overwrite, revert, reset, clean, or replace user changes without explicit permission.
- Inspect the working tree and relevant diffs before editing.
- Keep changes limited to approved files.
- If existing changes overlap the requested work, report the overlap before proceeding.

## Rules and State Authority

RealmWeaver follows these governing principles:

> **AI tells the story. Rules decide what happens.**

> **AI proposes. RealmWeaver validates.**

Deterministic rules, validated system state, and persisted campaign state are authoritative.

AI narration must never directly modify authoritative state, including:

- Hit Points or Temporary Hit Points
- Inventory or equipment
- Currency
- Spell slots, prepared spells, or other resources
- Quest or objective state
- Conditions or Exhaustion
- Character progression
- Dice results or mechanical outcomes
- NPC mechanical state
- Persistent world state
- Any other authoritative campaign data

AI may interpret intent, make permitted proposals, control approved AI actors, and narrate validated outcomes. State changes must pass through deterministic validation, resolution, and persistence boundaries.

## Documentation Changes

Documentation corrections must preserve approved design decisions unless the user explicitly approves a rule or scope change.

When correcting documentation:

- Distinguish editorial corrections from design changes.
- Preserve source-of-truth relationships and rule authority.
- Report contradictions rather than masking them.
- Keep terminology and cross-references consistent.
- Record intentionally deferred decisions as deferred.
- Do not present unresolved provenance or licensing questions as settled.

Update relevant documentation and `docs/project/PROJECT_STATUS.md` at approved milestone checkpoints. Do not prematurely mark a group, activity, gate, or milestone complete.

## Implementation and Verification

When production implementation is eventually authorized:

- Explain unfamiliar code, workflows, and important technical decisions in clear language; RealmWeaver is also a guided-learning project.
- Follow the approved architecture, requirements, rules, acceptance criteria, and Definition of Done.
- Prefer small, focused changes over broad rewrites.
- Maintain deterministic rule enforcement and transactional state integrity.
- Add or update relevant tests.
- Run proportionate tests, static checks, formatting checks, and other applicable verification after changes.
- Report checks that could not be run and why.
- Do not claim completion when acceptance criteria or required verification remain outstanding.

After making changes, provide for review:

1. A concise summary of changes.
2. Verification commands and results.
3. Known limitations, risks, or unresolved questions.
4. The relevant diff, or a clear diff summary when the full diff is impractical.

## Git Safety

Do not perform any of the following without explicit permission:

- Commit
- Push
- Merge
- Rebase
- Amend commits
- Reset or rewrite Git history
- Delete branches
- Open or modify pull requests
- Force-push
- Tag or publish releases

When commits are explicitly authorized, keep them small, focused, descriptive, and limited to approved files. Review the working-tree and staged diffs before committing, and never include unrelated user changes.
