# Continuum State & Lifecycle Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

This document defines the state machines and lifecycle semantics for Continuum domain entities.

The domain model defines:

> What exists.

The relationship model defines:

> How things relate.

The lifecycle model defines:

> How things change.

Continuum must preserve not only current state, but the transitions that produced that state.

Therefore:

```text
Current State
    +
State History
    +
Transition History
```

is more important than current state alone.

---

# 2. Core Principle

An entity's current state is a projection of its history.

Conceptually:

```text
Initial State
     ↓
Transition
     ↓
State
     ↓
Transition
     ↓
State
     ↓
...
     ↓
Current State
```

Continuum should therefore be capable of reconstructing:

```text
What state was this entity in?
When?
Why did it change?
Who or what changed it?
What evidence supported the transition?
What was the resulting state?
```

---

# 3. State vs Event

A critical distinction:

```text
State
```

describes **what is true**.

```text
Event
```

describes **something that happened**.

Example:

```text
Task State:
    in_progress
```

Event:

```text
TaskStarted
```

The event explains how the task entered the state.

---

# 4. State vs Status

A status is a representation of state.

For example:

```text
status = "in_progress"
```

is an implementation representation.

The domain concept is:

```text
Task is currently in progress.
```

The implementation must not obscure the semantic meaning.

---

# 5. State Transition

A state transition can be represented as:

```text
Current State
      │
      │ event / action
      ▼
Transition
      │
      │ validation
      ▼
New State
```

Conceptually:

```text
S₁ --E--> S₂
```

where:

```text
S₁ = previous state
E  = transition event
S₂ = resulting state
```

---

# 6. Transition Record

A transition should eventually be representable as:

```text
Transition
├── transition_id
├── entity_id
├── entity_type
├── from_state
├── to_state
├── trigger
├── actor
├── timestamp
├── reason
├── evidence
├── session
└── metadata
```

Not every field must be implemented immediately.

---

# 7. Transition Invariants

A valid transition must have:

```text
1. A subject entity.
2. A prior state.
3. A resulting state.
4. A transition trigger.
5. A temporal position.
```

Important transitions should additionally preserve:

```text
actor
reason
provenance
evidence
session
```

---

# 8. General Lifecycle

Most Continuum entities follow a conceptual lifecycle:

```text
proposed
    ↓
accepted
    ↓
active
    ↓
completed
    ↓
superseded / archived
```

Not every entity uses every state.

---

# 9. State Categories

Continuum distinguishes several kinds of state:

```text
Existence State
Activity State
Validity State
Epistemic State
Approval State
Execution State
Freshness State
Lifecycle State
```

These should not automatically be collapsed into one universal status field.

---

# 10. Existence State

Existence describes whether an entity exists within the system.

Conceptual states:

```text
created
active
deleted
```

However, deletion should generally be logical rather than destructive.

Preferred:

```text
active
deleted
```

with historical preservation.

---

# 11. Activity State

Activity describes whether an entity is currently being acted upon.

Typical states:

```text
inactive
active
paused
blocked
```

---

# 12. Validity State

Validity describes whether an entity or assertion remains valid.

Typical states:

```text
valid
invalid
disputed
superseded
unknown
```

---

# 13. Epistemic State

Epistemic state describes the system's relationship to knowledge.

Typical progression:

```text
unknown
    ↓
observed
    ↓
interpreted
    ↓
hypothesized
    ↓
supported
    ↓
verified
```

This does not mean every piece of knowledge must pass through every state.

---

# 14. Approval State

Approval describes whether an artifact, proposal, or decision has been accepted by the appropriate authority.

Typical states:

```text
unreviewed
proposed
approved
rejected
revoked
```

---

# 15. Execution State

Execution describes whether an operation has been attempted and what happened.

Typical states:

```text
pending
running
succeeded
failed
cancelled
```

---

# 16. Freshness State

Freshness describes whether information remains current.

Typical states:

```text
fresh
aging
stale
expired
unknown
```

Freshness is distinct from truth.

A fact can remain true while becoming stale for the purpose of current decision-making.

---

# 17. Lifecycle State

Lifecycle state describes where an entity is in its overall existence.

Typical states:

```text
draft
active
completed
superseded
archived
```

---

# 18. Task Lifecycle

The Task lifecycle is one of Continuum's primary state machines.

```text
draft
  ↓
ready
  ↓
in_progress
  ↓
completed
```

Alternative transitions:

```text
ready
  ↓
blocked

in_progress
  ↓
blocked

in_progress
  ↓
cancelled

blocked
  ↓
ready

blocked
  ↓
in_progress
```

---

# 19. Task State Machine

```text
                    ┌─────────────┐
                    │             │
                    ▼             │
draft ────────► ready ────────► in_progress
                  │                  │
                  │                  │
                  ▼                  ▼
               blocked ◄───────────┘
                  │                  │
                  │                  ▼
                  └──────────────► completed

in_progress ───────────────► cancelled
```

---

# 20. Task State Semantics

### draft

The task exists but is not yet ready for execution.

### ready

The task has sufficient definition and prerequisites to begin.

### in_progress

Work is actively occurring.

### blocked

Work cannot meaningfully continue because a prerequisite is unresolved.

### completed

The intended work has been successfully completed.

### cancelled

The task will no longer be pursued.

---

# 21. Task Completion

A Task should not be marked completed merely because an AI claims it is complete.

Completion should ideally be supported by:

```text
Artifact change
+
Verification
+
Expected outcome
```

The exact requirements depend on task type.

---

# 22. Task Blocked State

A blocked Task should identify its blocker.

Examples:

```text
blocked_by → Task
blocked_by → Decision
blocked_by → Requirement
blocked_by → ExternalDependency
blocked_by → HumanInput
```

---

# 23. Requirement Lifecycle

Requirements generally follow:

```text
proposed
    ↓
accepted
    ↓
active
    ↓
satisfied
```

Alternative:

```text
proposed
    ↓
rejected
```

or:

```text
active
    ↓
superseded
```

---

# 24. Requirement State Machine

```text
                 ┌───────────► rejected
                 │
proposed ───────► accepted ─────► active ─────► satisfied
                                      │
                                      ▼
                                  superseded
```

---

# 25. Requirement Semantics

### proposed

Someone has suggested that the requirement should exist.

### accepted

The requirement has authority.

### active

The requirement currently applies.

### satisfied

The requirement has been demonstrated as fulfilled.

### superseded

Another requirement has replaced it.

### rejected

The requirement was explicitly declined.

---

# 26. Goal Lifecycle

Goals generally follow:

```text
proposed
    ↓
active
    ↓
achieved
```

Alternative:

```text
active
    ↓
abandoned
```

or:

```text
active
    ↓
superseded
```

---

# 27. Decision Lifecycle

Decisions are especially important because they establish project direction.

```text
proposed
    ↓
under_review
    ↓
accepted
```

Alternative paths:

```text
under_review
    ↓
rejected
```

or:

```text
accepted
    ↓
superseded
```

---

# 28. Decision State Machine

```text
proposed
    │
    ▼
under_review
   / \
  /   \
 ▼     ▼
accepted rejected
  │
  ▼
superseded
```

---

# 29. Decision Meaning

A Decision is not merely a statement.

A Decision represents:

> An accepted choice that constrains future behavior.

Therefore, an accepted Decision should be traceable to:

```text
reason
alternatives
evidence
authority
scope
time
```

---

# 30. Proposal Lifecycle

```text
draft
  ↓
proposed
  ↓
under_review
  ├──► accepted
  └──► rejected
```

An accepted Proposal may generate:

```text
Decision
```

or:

```text
Task
```

or:

```text
Requirement
```

---

# 31. Session Lifecycle

A Session is fundamentally temporal.

```text
scheduled
    ↓
active
    ↓
ended
```

Alternative:

```text
scheduled
    ↓
cancelled
```

---

# 32. Session State Machine

```text
scheduled ─────► active ─────► ended
     │
     └──────────────────────► cancelled
```

A Session should not be silently rewritten after it ends.

---

# 33. Session Continuity

The Session is not the unit of continuity.

It is one episode in a longer continuity chain:

```text
Session 1
    ↓
Memory / Knowledge / State
    ↓
Session 2
    ↓
Memory / Knowledge / State
    ↓
Session 3
```

The project must survive the end of any individual Session.

---

# 34. Action Lifecycle

Actions are execution records.

```text
requested
    ↓
authorized
    ↓
running
    ↓
succeeded
```

Alternative outcomes:

```text
running
    ├──► failed
    ├──► cancelled
    └──► timed_out
```

---

# 35. Action State Machine

```text
requested
    ↓
authorized
    ↓
running
  /  |   \
 /   |    \
▼    ▼     ▼
success failed cancelled
```

---

# 36. Action Authorization

Not every requested Action should automatically be authorized.

Authorization may depend on:

```text
Actor
Policy
Permission
Scope
Risk
Human approval
```

---

# 37. Artifact Lifecycle

Artifacts generally follow:

```text
identified
    ↓
created
    ↓
modified
    ↓
verified
    ↓
current
```

Alternative:

```text
current
    ↓
superseded
    ↓
archived
```

---

# 38. Artifact Version Lifecycle

An ArtifactVersion is immutable once persisted.

```text
created
    ↓
recorded
    ↓
verified
    ↓
superseded
```

A new version should not mutate the previous version.

Instead:

```text
Version 1
    ↓
Version 2
    ↓
Version 3
```

---

# 39. Knowledge Lifecycle

Knowledge requires special care because its lifecycle is epistemic.

A generalized model:

```text
unknown
    ↓
observed
    ↓
interpreted
    ↓
hypothesized
    ↓
supported
    ↓
verified
    ↓
current
```

Alternative transitions include:

```text
supported
    ↓
challenged

verified
    ↓
retracted

current
    ↓
superseded
```

---

# 40. Knowledge State Machine

```text
                    ┌──────────────┐
                    │              ▼
unknown → observed → interpreted → hypothesized
                                      │
                                      ▼
                                  supported
                                  /       \
                                 /         ▼
                                ▼        challenged
                           verified
                              │
                              ▼
                           current
                           /     \
                          ▼       ▼
                    superseded  retracted
```

---

# 41. Knowledge State Semantics

### unknown

The system does not currently know the proposition.

### observed

Something was directly observed.

### interpreted

An observation has been interpreted.

### hypothesized

A possible explanation has been proposed.

### supported

Evidence supports the claim.

### verified

The claim has passed a defined verification process.

### current

The claim is currently accepted as applicable.

### challenged

Evidence or reasoning calls the claim into question.

### superseded

A newer understanding replaces it.

### retracted

The claim is explicitly withdrawn.

---

# 42. Important Knowledge Principle

These are not equivalent:

```text
high confidence
verified
authoritative
current
true
```

Continuum must preserve these distinctions.

---

# 43. Memory Lifecycle

Memory has a different lifecycle from Knowledge.

A generalized lifecycle:

```text
captured
    ↓
processed
    ↓
stored
    ↓
retrieved
    ↓
recontextualized
    ↓
stale
    ↓
archived
```

Memory does not necessarily become Knowledge.

---

# 44. Context Package Lifecycle

Context is generated for a specific purpose.

```text
requested
    ↓
assembled
    ↓
validated
    ↓
delivered
    ↓
consumed
    ↓
expired
```

---

# 45. Context Package State Machine

```text
requested
    ↓
assembling
    ↓
ready
    ↓
delivered
    ↓
consumed
    ↓
expired
```

Failure may occur during assembly:

```text
assembling
    ↓
failed
```

---

# 46. Context Freshness

Context Packages should be considered time-sensitive.

A ContextPackage may become invalid when:

```text
project state changes
artifact changes
task changes
decision changes
knowledge changes
constraints change
```

Therefore:

> Context is a projection of state, not a permanent state container.

---

# 47. Evidence Lifecycle

Evidence generally follows:

```text
collected
    ↓
recorded
    ↓
evaluated
    ↓
accepted
```

Alternative:

```text
evaluated
    ↓
rejected
```

or:

```text
accepted
    ↓
challenged
```

---

# 48. Observation Lifecycle

Observations should be treated as historical records.

```text
captured
    ↓
recorded
    ↓
interpreted
```

The original observation should not be overwritten by later interpretation.

---

# 49. Hypothesis Lifecycle

```text
proposed
    ↓
under_test
    ├──► supported
    ├──► rejected
    └──► unresolved
```

A hypothesis should not become a fact simply because it is plausible.

---

# 50. Pattern Lifecycle

Patterns are derived from repeated observations.

```text
suspected
    ↓
observed
    ↓
candidate
    ↓
validated
```

Alternative:

```text
candidate
    ↓
rejected
```

Patterns may later become stale.

---

# 51. Outcome Lifecycle

Outcomes represent consequences of actions.

```text
anticipated
    ↓
observed
    ↓
evaluated
    ↓
confirmed
```

Alternative:

```text
observed
    ↓
unexpected
```

---

# 52. State Transition Preconditions

A transition may have preconditions.

Example:

```text
Task:
    ready → in_progress
```

may require:

```text
all required dependencies satisfied
```

Similarly:

```text
Task:
    in_progress → completed
```

may require:

```text
required artifact changes exist
verification succeeds
```

---

# 53. State Transition Postconditions

A transition may also establish postconditions.

Example:

```text
Task completed
```

may produce:

```text
ArtifactVersion
Observation
Outcome
Knowledge update
```

---

# 54. Transition Rejection

If preconditions are not satisfied, the transition should be rejected rather than silently performed.

Example:

```text
Task:
    blocked → completed
```

should normally fail unless an explicit override exists.

---

# 55. Transition Authority

Transitions may require different authorities.

Example:

```text
Task:
    in_progress → completed
```

may be performed by:

```text
Agent
Human
Automated verifier
```

while:

```text
Decision:
    proposed → accepted
```

may require:

```text
Human
Governance authority
```

depending on project policy.

---

# 56. Transition Provenance

Every important state transition should answer:

```text
Who or what caused this transition?
Why?
When?
Based upon what?
During which Session?
```

---

# 57. Transition History

For an entity:

```text
Task-123
```

Continuum should eventually be able to reconstruct:

```text
09:00 draft
09:15 ready
09:30 in_progress
10:20 blocked
11:05 in_progress
12:30 completed
```

rather than merely:

```text
status = completed
```

---

# 58. State Reconstruction

Current state may be derived from:

```text
initial state
+
ordered transitions
```

Conceptually:

```text
State₀
   + Transition₁
   + Transition₂
   + Transition₃
   ...
   = CurrentState
```

This provides an auditable history.

---

# 59. Event-Sourced Direction

Continuum should remain compatible with event-sourced modeling.

However:

> Continuum is not required to implement full event sourcing everywhere.

The important principle is:

```text
Important state changes must be reconstructable.
```

Storage architecture remains a later decision.

---

# 60. State Projection

Current state may be treated as a projection:

```text
Event History
      ↓
Transition Processor
      ↓
Current State
```

This allows future implementations to optimize reads without destroying history.

---

# 61. State and Memory

Memory may record:

```text
"Task X was completed."
```

But the authoritative state should come from:

```text
Task State History
```

Memory is contextual evidence about state, not necessarily the authoritative state itself.

---

# 62. State and Knowledge

Knowledge can describe state:

```text
Knowledge:
    Task X is complete.
```

But if the underlying Task transitions back to:

```text
in_progress
```

the knowledge becomes stale.

Current state takes precedence over historical knowledge about state.

---

# 63. State and Context

A ContextPackage should normally contain:

```text
current state
+
relevant historical transitions
+
relevant knowledge
+
relevant decisions
```

not simply a snapshot of old conversation.

---

# 64. State and AI Continuity

This is central to Continuum.

When a new AI session begins, the AI should not need to infer the project's current state solely from previous conversation.

Instead:

```text
Project State
      +
State History
      +
Knowledge
      +
Decisions
      +
Current Task
      +
Relevant Context
```

should establish continuity.

---

# 65. Lifecycle Invariants

The following invariants should hold:

```text
1. Current state must be derivable.

2. Important state transitions must be traceable.

3. Historical states must not be silently rewritten.

4. State and event must remain conceptually distinct.

5. Knowledge about state must not automatically override state.

6. Memory about state must not automatically override state.

7. Invalid transitions must be rejected.

8. Transitions may require authority.

9. Transitions may require evidence.

10. State machines should be explicit rather than implicit.

11. Different semantic dimensions of state should not be collapsed unnecessarily.

12. Historical continuity must survive across Sessions.
```

---

# 66. Universal Lifecycle Pattern

Many Continuum entities can be modeled using:

```text
Created
   ↓
Active
   ↓
Completed
   ↓
Superseded
   ↓
Archived
```

But this is a pattern, not a universal state machine.

Each domain entity should define its own valid transitions.

---

# 67. Invalid Transition Principle

A transition not explicitly permitted by the entity's lifecycle should be considered invalid.

Example:

```text
completed → draft
```

should not occur implicitly.

If reopening is required, the model should explicitly support:

```text
completed → reopened
```

followed by:

```text
reopened → in_progress
```

---

# 68. Reopening

Some entities need explicit reopening.

For example:

```text
Task
completed
    ↓
reopened
    ↓
in_progress
```

Reopening should preserve the fact that the task had previously been completed.

---

# 69. Cancellation vs Failure

These must remain distinct.

```text
cancelled
```

means:

> The operation was intentionally stopped or abandoned.

```text
failed
```

means:

> The operation attempted execution but did not achieve its intended result.

---

# 70. Blocked vs Failed

These are also distinct.

```text
blocked
```

means:

> Work cannot proceed because a prerequisite is unresolved.

```text
failed
```

means:

> An attempt was made and did not succeed.

---

# 71. Superseded vs Deleted

These are fundamentally different.

```text
superseded
```

means:

> A newer entity or state replaced this one.

```text
deleted
```

means:

> The entity is no longer active in the operational model.

Neither should erase historical provenance.

---

# 72. Rejected vs Invalid

```text
rejected
```

means:

> An authorized actor declined it.

```text
invalid
```

means:

> It does not satisfy the applicable validity conditions.

These states must not be conflated.

---

# 73. Current vs Verified

A verified fact can become non-current.

Example:

```text
Fact:
    "System uses Redis."

Verified:
    yes

Current:
    no
```

because the system later migrated to PostgreSQL.

---

# 74. Current vs True

Current applicability and truth are distinct dimensions.

A historical fact may remain true without describing current state.

Therefore:

```text
historical truth
```

must be preserved separately from:

```text
current applicability
```

---

# 75. Lifecycle and Continuity

Continuum's core continuity property is:

```text
Session N ends
      ↓
State persists
      ↓
Knowledge persists
      ↓
History persists
      ↓
Session N+1 begins
      ↓
Current state reconstructed
```

The next actor therefore inherits the project rather than merely inheriting a transcript.

---

# 76. Lifecycle Objective

The lifecycle system exists so Continuum can answer:

```text
What is happening now?

What happened before?

What changed?

Why did it change?

Who changed it?

What caused the change?

Was the transition valid?

What evidence supports the current state?

What remains unresolved?

What can happen next?
```

---

# 77. Final Principle

Continuum must preserve **state transitions, not merely state snapshots**.

A snapshot tells us:

> Where we are.

A transition history tells us:

> How we got here.

A complete continuity system needs both.

Therefore:

```text
Continuum
=
Domain Model
+
Relationships
+
State
+
Transitions
+
History
+
Knowledge
+
Context
```

The ultimate goal is not simply persistent memory.

It is **reconstructable continuity of project state and understanding across time, sessions, humans, agents, and tools.**
