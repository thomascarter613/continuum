# Continuum Temporal & Provenance Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

## 1. Purpose

This document defines how Continuum represents:

* time
* historical state
* discovery
* observation
* recording
* validity
* provenance
* source lineage
* actor attribution
* session context
* corrections
* authority
* confidence

The objective is to preserve the history and epistemic lineage of project knowledge.

---

# 2. Fundamental Temporal Principle

Continuum must not conflate:

```text
when something happened
when something was observed
when something was discovered
when something was recorded
when something became valid
```

These are distinct temporal concepts.

---

# 3. Temporal Dimensions

Continuum recognizes the following conceptual temporal dimensions.

### Event Time

When an underlying event occurred.

Example:

```text
Build failed at 14:32.
```

### Observation Time

When an Actor observed a state.

Example:

```text
Developer inspected package.json at 14:40.
```

### Discovery Time

When the project became aware of a fact, event, or interpretation.

Example:

```text
A defect existed on July 10.
It was discovered on July 20.
```

### Record Time

When information was recorded into Continuum.

### Valid Time

The interval during which a Claim, Decision, or other project state is applicable.

---

# 4. Temporal Representation

Conceptually, an object may contain:

```text
event_time
observed_at
discovered_at
recorded_at
valid_from
valid_until
```

Not every object requires every field.

Temporal semantics must be determined by object type.

---

# 5. Historical vs Knowledge Time

Continuum should preserve the distinction between:

```text
Project / Reality Time
        │
        ▼
What happened?

Knowledge Time
        │
        ▼
When did the project know?
```

Example:

```text
Bug introduced:
    July 10

Bug discovered:
    July 20

Bug recorded:
    July 20
```

The project may therefore know on July 20 that something happened on July 10.

---

# 6. Bitemporal Capability

Continuum should eventually support reconstruction based upon at least two temporal dimensions:

```text
Event / Valid Time
        +
Knowledge / Record Time
```

This enables questions such as:

```text
What actually happened on July 10?

What did we believe on July 10?

What did we know on July 20?

What did we believe on July 20 about July 10?
```

The implementation may use a richer temporal model.

---

# 7. Validity Intervals

Claims and Decisions may have validity intervals.

Conceptually:

```text
valid_from
valid_until
```

Example:

```text
Decision D-001

valid_from:
2026-07-01

valid_until:
2026-07-29
```

The Decision remains historically real after its validity ends.

Its historical existence and current applicability are distinct.

---

# 8. Provenance

Provenance records how knowledge entered the project.

Conceptually:

```text
Provenance
├── actor
├── session
├── source
├── method
├── timestamp
├── parent objects
└── authorization
```

Provenance should eventually allow Continuum to answer:

```text
Who created this?

When?

From what source?

Using what method?

Based on what information?

Under whose authority?

During which session?
```

---

# 9. Provenance Is a Graph

Provenance is not merely metadata.

It may form chains such as:

```text
Claim
  ↓
Inference
  ↓
Evidence
  ↓
Artifact
  ↓
Commit
  ↓
Session
  ↓
Actor
```

or:

```text
Decision
  ↓
Rationale
  ↓
Claims
  ↓
Evidence
  ↓
Sources
```

Therefore provenance participates in the project graph.

---

# 10. Actor

An Actor is an entity capable of producing an observation, assertion, event, decision, or other project action.

Potential Actor types include:

```text
Human
AI Agent
Tool
Service
System Process
External System
```

Actor identity must be persistent.

---

# 11. Source

A Source provides information or evidence.

Potential Source types include:

```text
Human statement
AI output
Source file
Repository
Git commit
Test result
Build result
Runtime observation
Tool output
Documentation
External resource
System state
```

A Source is not inherently authoritative.

---

# 12. Actor vs Source

Actor and Source are distinct concepts.

Example:

```text
Actor:
    AI Agent A

Source:
    Repository R

Action:
    Agent A analyzed Repository R

Result:
    Claim C-001
```

Therefore:

```text
Actor ≠ Source
```

---

# 13. Session

A Session represents a bounded interaction in which one or more Actors interact with a Project.

A Session may contain:

```text
participants
AI models
tools
repository state
environment state
task
context
inputs
outputs
events
resulting changes
```

Session identity provides an anchor for reconstructing the circumstances surrounding project activity.

---

# 14. Context Provenance

Context supplied to an AI must itself be traceable.

Conceptually:

```text
Context Packet
├── included objects
├── excluded objects
├── selection rationale
├── source project state
├── generated_at
├── generated_by
└── delivered_to
```

This enables the question:

> What did the AI actually have available when it produced this output?

---

# 15. AI Output Provenance

AI-generated information should preserve, where available:

```text
actor
model/provider
session
prompt/context reference
input objects
output
timestamp
tool calls
resulting claims
resulting proposed actions
```

Continuum must not depend upon storing private model chain-of-thought.

It should preserve observable inputs, outputs, evidence, and resulting project actions instead.

---

# 16. Authority

Authority describes what an Actor is authorized to establish or modify.

Authority should be modeled as capabilities rather than merely a scalar.

Potential capabilities include:

```text
establish_requirement
approve_decision
modify_architecture
verify_claim
record_observation
authorize_work
accept_change
```

Authority is contextual and may depend on:

* project
* scope
* object type
* role
* environment
* operation

---

# 17. Authority vs Confidence

These are independent dimensions.

Example:

```text
AI Agent:
    confidence = high
    authority = proposal only
```

An AI may have high confidence in an architectural recommendation without being authorized to establish the architecture.

Therefore:

```text
Confidence ≠ Authority
```

---

# 18. Authority Transfer

Authority may be transferred or exercised through explicit events.

Conceptual example:

```text
AI proposes
    ↓
Human reviews
    ↓
Human approves
    ↓
Decision established
    ↓
Project state updated
```

The approval event creates the authority necessary for the Decision.

---

# 19. Confidence

Confidence represents belief in the correctness of a Claim, inference, interpretation, or conclusion.

Confidence may depend upon:

* evidence
* source reliability
* testing
* reasoning
* consistency
* corroboration
* uncertainty

Confidence does not establish authority.

The final confidence representation remains unspecified.

---

# 20. Corrections

Historical records should not normally be silently rewritten.

If a Claim is discovered to be incorrect, Continuum should preserve the original and record corrective information.

Example:

```text
CLAIM-001
"The application uses PostgreSQL."

        contradicted_by

CLAIM-014
"The application uses SQLite."
```

The original Claim remains part of project history.

---

# 21. Correction vs Contradiction

Correction and contradiction are distinct.

### Correction

The original statement was erroneous and is replaced by a better understanding.

```text
Claim A
    ↓
corrected_by
    ↓
Claim B
```

### Contradiction

Two Claims currently conflict.

```text
Claim A
    ↕
contradicts
    ↕
Claim B
```

Contradiction evaluation must consider:

```text
subject
predicate
object
scope
time
version
```

before declaring a true conflict.

---

# 22. Provenance Chains

Knowledge may be derived through multiple stages.

Example:

```text
Source File
    ↓
Observation
    ↓
Assertion
    ↓
Claim
    ↓
Inference
    ↓
Proposal
    ↓
Decision
    ↓
Implementation
    ↓
Test Evidence
    ↓
Verification
```

Continuum should preserve these relationships.

This creates an auditable knowledge lineage.

---

# 23. Evidence Lineage

Evidence should be traceable to its source.

Example:

```text
Verification V-001
      │
      └── based_on
             │
             ▼
Test Result T-019
             │
             └── executed_against
                    │
                    ▼
                Artifact A-017
                    │
                    └── version
                           │
                           ▼
                       Commit C-201
```

This allows Continuum to establish not merely that something is believed, but why.

---

# 24. Context Lineage

A Context Packet must be traceable to the project state from which it was generated.

Example:

```text
Context CXT-042
      │
      ├── Requirement R-001
      ├── Decision D-004
      ├── Claim C-018
      ├── Artifact A-011
      └── Evidence E-020

generated_from:

Project State S-031
```

This permits later reconstruction of AI inputs.

---

# 25. Historical Reconstruction

Continuum should eventually support temporal reconstruction.

Conceptual query:

```text
state_at(time)
```

Example:

```text
state_at(2026-07-01)
```

should be capable of deriving the project state recognized at that point.

A richer future query may distinguish:

```text
state_at(
    event_time = ...,
    knowledge_time = ...
)
```

---

# 26. Temporal Conflict Resolution

Two apparently contradictory Claims may coexist when they differ in:

```text
scope
time
environment
version
authority
epistemic status
```

Example:

```text
Claim A:
development database = SQLite

Claim B:
production database = PostgreSQL
```

These are not contradictory.

The conflict engine must therefore evaluate Claims contextually.

---

# 27. Provenance and Trust

Provenance does not automatically imply correctness.

A perfectly traceable Claim may still be false.

Therefore:

```text
Provenance ≠ Truth
```

Provenance establishes lineage.

Evidence, verification, authority, and reasoning contribute to epistemic evaluation.

---

# 28. Historical Immutability

The following should be treated as historically immutable in principle:

* Events
* Observations
* Evidence records
* Provenance records
* Session records
* Authority events
* accepted Decision events

Corrections should normally be represented as additional records.

---

# 29. Derived State

Current state is derived from historical information.

Conceptually:

```text
Historical Events
Claims
Evidence
Relations
Temporal Rules
Authority Rules
        │
        ▼
State Derivation
        │
        ▼
Current Project State
```

Derived state may be regenerated.

---

# 30. Core Temporal-Provenance Rules

Continuum establishes the following rules:

1. Event time is distinct from record time.
2. Observation time is distinct from discovery time.
3. Validity time is distinct from event time.
4. Historical records should not be silently rewritten.
5. Provenance is part of project knowledge.
6. Actors and Sources are distinct concepts.
7. Authority and Confidence are distinct concepts.
8. AI output must be attributable when possible.
9. Context supplied to AI must itself be traceable.
10. Corrections should preserve historical knowledge.
11. Contradictions must be evaluated in context.
12. Current state is derived rather than the sole historical source of truth.

---

# 31. Open Questions

The following remain unresolved:

1. Exact temporal storage model
2. Clock and timestamp precision
3. Time-zone normalization
4. Event ordering
5. Concurrent events
6. Clock skew
7. Temporal uncertainty
8. Open-ended intervals
9. Provenance graph storage
10. Provenance compression
11. AI model identity
12. Prompt/context retention policy
13. Privacy boundaries
14. Source reliability model
15. Authority inheritance
16. Authority delegation
17. Authority expiration
18. Confidence representation
19. Correction semantics
20. Conflict resolution rules
21. State reconstruction algorithm
22. Snapshot strategy

---

# 32. Design Rule

Continuum must preserve enough temporal and provenance information to answer:

> What happened?

> When did it happen?

> When did we observe it?

> When did we discover it?

> When did we record it?

> When was it considered valid?

> Who or what told us?

> What evidence supported it?

> Who had authority?

> What did the AI know at the time?

> What did the project believe at the time?

> What changed afterward?

These questions are fundamental requirements of persistent project memory.
