# Continuum Object Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

## 1. Purpose

The Continuum Object Model defines the structural characteristics shared by persistent project objects.

It establishes the distinction between:

* identity
* representation
* version
* event
* state
* relationship
* provenance
* temporal validity
* authority
* confidence

This document remains conceptual and does not define a storage schema.

---

# 2. Foundational Rule

A persistent project object is not equivalent to its representation.

Therefore:

```text
Object identity
    ≠
File path
    ≠
Filename
    ≠
Document title
    ≠
Serialization
    ≠
Database record
```

Representations may change while object identity persists.

---

# 3. Object Identity

Every persistent object must possess a stable identity within its project scope.

Conceptually:

```text
Object Identity
├── project
├── object type
└── object identifier
```

The final identifier syntax is intentionally unspecified.

An identifier must remain stable across ordinary representation changes.

---

# 4. Object Type

Every persistent object has a semantic type.

Examples include:

```text
project
participant
artifact
claim
requirement
constraint
proposal
hypothesis
assumption
decision
event
evidence
source
work
context
relation
```

Type identifies meaning.

It does not necessarily imply implementation inheritance.

---

# 5. Object Representation

An object may have one or more representations.

Examples:

```text
Markdown
YAML
JSON
database record
CLI representation
API representation
UI representation
```

A representation is not the object's identity.

Multiple representations may refer to the same object.

---

# 6. Version

An object may have multiple versions.

Example:

```text
REQ-001
├── v1
├── v2
└── v3
```

Versioning preserves the evolution of a single conceptual object.

Version identity is distinct from object identity.

---

# 7. Revision

A revision modifies an existing conceptual object while retaining its identity.

Example:

```text
REQ-001 v1
    ↓ revision
REQ-001 v2
```

The rules governing which changes constitute a revision remain to be specified.

---

# 8. Supersession

Supersession occurs when a new object replaces an existing object or understanding.

Example:

```text
DEC-001
Use React.

       superseded_by

DEC-019
Use SolidJS.
```

Supersession does not erase the historical existence of the superseded object.

Revision and supersession are distinct operations.

---

# 9. Object Envelope

Most persistent objects are expected to share a conceptual envelope:

```text
Object
├── identity
├── type
├── project
├── version
├── lifecycle
├── provenance
├── temporal validity
└── content
```

The exact representation is not yet defined.

---

# 10. Content

The content portion contains object-specific semantic information.

Examples:

```text
Requirement:
    required condition

Decision:
    chosen direction

Claim:
    proposition

Artifact:
    artifact metadata

Work:
    objective and execution information
```

Content is governed by the object's semantic type.

---

# 11. Lifecycle

Lifecycle describes the operational status of an object.

Potential lifecycle states include:

```text
DRAFT
ACTIVE
IN_PROGRESS
COMPLETED
ABANDONED
REJECTED
SUPERSEDED
ARCHIVED
```

The final lifecycle model remains to be specified.

Lifecycle is distinct from epistemic status.

---

# 12. Epistemic Status

Epistemic status describes the project's understanding of a Claim.

Potential values include:

```text
UNKNOWN
OBSERVED
DERIVED
ASSUMED
HYPOTHESIZED
PROPOSED
ACCEPTED
VERIFIED
CONTRADICTED
REJECTED
SUPERSEDED
CONFLICTED
```

Epistemic status applies primarily to knowledge and Claims rather than all object types.

---

# 13. Event

An Event represents something that happened.

Examples:

```text
Decision accepted
Requirement revised
Artifact modified
Test executed
Claim verified
Claim contradicted
Work started
Work completed
Conflict detected
Context generated
```

Events provide historical continuity.

Events should be treated as historical facts once recorded, subject to later correction or invalidation mechanisms.

---

# 14. Event vs State

An Event answers:

> What happened?

State answers:

> What is currently believed to be true?

Example:

```text
Event:
    Test T-001 passed at 14:03.

Derived State:
    Requirement R-001 is currently verified.
```

State is generally derived from Events, Claims, Evidence, and Relations.

---

# 15. Derived State

Continuum should distinguish historical records from derived current understanding.

Conceptually:

```text
Events
Claims
Evidence
Relations
      │
      ▼
State Derivation
      │
      ▼
Current Project State
```

Derived State may include:

* current requirements
* active decisions
* current architecture
* known conflicts
* requirement coverage
* current work
* current project status

Derived state may change without modifying historical events.

---

# 16. Relation

A Relation connects project objects.

Conceptually:

```text
Relation
├── source
├── predicate
├── target
├── provenance
├── temporal validity
└── status
```

Examples:

```text
REQ-001
    implements
ART-019

DEC-001
    establishes
CLAIM-004

CLAIM-004
    supported_by
EVIDENCE-009
```

---

# 17. Relations Are First-Class

Relationships may themselves be disputed, revised, or invalidated.

Therefore a relationship must be capable of possessing identity and metadata.

Example:

```text
REL-017

source:
    Component A

predicate:
    depends_on

target:
    Component B

status:
    disputed
```

This allows Continuum to represent disagreement about the graph itself.

---

# 18. Provenance

Provenance describes how an object, assertion, event, or relationship came into existence.

Conceptually:

```text
Provenance
├── actor
├── session
├── source
├── method
├── timestamp
├── parent knowledge
└── authorization
```

Provenance must eventually allow Continuum to answer:

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

# 19. Actor

An Actor is the participant responsible for producing an object, event, observation, decision, or other project action.

Actors may include:

```text
Human
AI agent
Tool
Service
System process
External system
```

Actor identity is distinct from authority.

---

# 20. Session

A Session represents a bounded interaction between one or more Actors and a Project.

Sessions provide context for:

* observations
* proposals
* decisions
* work
* changes
* AI interactions
* human interactions

Session identity allows project history to be traced back to the interaction in which it originated.

---

# 21. Authority

Authority describes what an Actor is permitted to establish or modify.

Authority should not initially be represented as a simple scalar.

It is expected to be capability-oriented.

Potential authority capabilities include:

```text
establish_requirement
approve_decision
modify_architecture
verify_claim
record_observation
authorize_work
accept_change
```

The final authorization model remains unspecified.

---

# 22. Confidence

Confidence describes belief in the correctness of a Claim, inference, interpretation, or conclusion.

Confidence is independent of authority.

Example:

```text
AI analysis:
    confidence = high

AI authority:
    may propose
    may not approve architecture
```

The final confidence model remains unspecified.

---

# 23. Temporal Model

Continuum must distinguish multiple temporal concepts.

Potential temporal fields include:

```text
created_at
observed_at
recorded_at
valid_from
valid_until
superseded_at
```

These represent different meanings.

For example:

```text
observed_at:
    when an observation occurred

recorded_at:
    when Continuum recorded it

valid_from:
    when the resulting knowledge became applicable
```

Temporal semantics will be formally specified later.

---

# 24. Scope

Claims and other project knowledge may apply only within a particular scope.

Examples:

```text
development
test
production
desktop
server
mobile
specific component
specific environment
specific version
specific subsystem
```

A Claim should therefore eventually support explicit scope.

Example:

```text
Claim:
    database = SQLite

Scope:
    local development
```

and:

```text
Claim:
    database = PostgreSQL

Scope:
    production
```

These Claims may coexist without contradiction.

---

# 25. Immutability

The following categories should be treated as historically immutable in principle:

* Events
* recorded observations
* Evidence records
* provenance records
* accepted Decisions as historical events

If an historical record is determined to be incorrect, Continuum should preserve the original record and add corrective information rather than silently rewriting history.

Implementation details remain unspecified.

---

# 26. Mutable / Derived Information

The following may be derived or recomputed:

* current state
* summaries
* context
* dependency views
* requirement coverage
* current architecture views
* conflict indexes
* project dashboards
* search indexes

Derived information may be regenerated from authoritative project history.

---

# 27. Historical Reconstruction

A major capability of Continuum should eventually be:

> Reconstruct the project's recognized state at a specified point in time.

For example:

```text
continuum state --at 2026-07-01
```

could conceptually reconstruct:

* active requirements
* active constraints
* accepted decisions
* known architecture
* unresolved conflicts
* current work
* known project state

This depends upon correct temporal and event modeling.

---

# 28. Current State

Current project state should be treated as a derived view rather than the sole source of truth.

Conceptually:

```text
Historical Events
       +
Claims
       +
Evidence
       +
Relations
       +
Temporal Rules
       +
Authority Rules
       ↓
Current State
```

The exact state derivation algorithm will be specified later.

---

# 29. Knowledge Graph

The object model produces a project graph:

```text
                    PROJECT
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       ENTITIES      CLAIMS       EVENTS
          │            │            │
          └────────────┼────────────┘
                       │
                    RELATIONS
                       │
                       ▼
                    EVIDENCE
                       │
                       ▼
                     SOURCES
```

The graph is conceptual.

It does not mandate graph-database storage.

---

# 30. Fundamental Distinctions

Continuum must preserve the following distinctions:

```text
Identity       ≠ Representation
Identity       ≠ Version
Revision       ≠ Supersession
Event          ≠ State
Claim          ≠ Decision
Evidence       ≠ Truth
Confidence     ≠ Authority
Source         ≠ Authority
Observation    ≠ Inference
Proposal       ≠ Decision
Conversation   ≠ Project Memory
Memory         ≠ Context
Context        ≠ Truth
```

These distinctions are foundational.

---

# 31. Open Questions

The following remain unresolved:

1. Exact object identifier format
2. Version numbering model
3. Revision rules
4. Supersession rules
5. Event structure
6. Event correction model
7. Relation identity format
8. Relation versioning
9. Provenance representation
10. Authority capabilities
11. Confidence representation
12. Temporal semantics
13. Scope model
14. State derivation algorithm
15. Claim equivalence
16. Claim deduplication
17. Conflict representation
18. Immutable versus mutable boundaries
19. Snapshot strategy
20. Storage model

These must be resolved before implementation schemas are finalized.

---

# 32. Design Rule

The object model defines the semantic structure of Continuum.

Storage, serialization, APIs, CLI commands, and user interfaces must implement this model rather than silently redefining it.
