# Continuum Ontology

**Status:** Draft
**Version:** 0.2.0
**Date:** 2026-07-29

---

## 1. Purpose

This document defines the conceptual ontology of Continuum.

The ontology establishes what kinds of things Continuum must be capable of representing independently of:

* programming language
* storage technology
* serialization format
* AI provider
* user interface
* CLI
* API

This document defines meaning, not implementation.

---

# 2. Foundational Model

Continuum represents a project as a persistent graph of:

```text
Entities
Claims
Events
Relations
Evidence
Sources
```

These primitives interact to form a model of project understanding and project history.

Conceptually:

```text
                    PROJECT
                       │
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
      ENTITIES       CLAIMS        EVENTS
         │             │             │
         └─────────────┼─────────────┘
                       │
                       ▼
                  RELATIONS
                       │
                       ▼
                    EVIDENCE
                       │
                       ▼
                    SOURCES
```

The project graph is a conceptual model.

It does not prescribe a graph database.

---

# 3. Ontological Primitives

Continuum initially recognizes four primary conceptual primitives:

1. Entity
2. Claim
3. Event
4. Relation

Evidence and Source provide provenance and support for the graph.

---

# 4. Entity

An Entity represents a thing that the project recognizes as existing or being relevant.

Examples include:

* Project
* Participant
* Artifact
* Component
* Interface
* Service
* Database
* Domain Concept
* Repository
* Environment

Entities have persistent identity.

Their representation may change without changing their identity.

---

# 5. Claim

A Claim is a persistent proposition asserting that some relationship, property, state, requirement, possibility, or interpretation holds within a specified scope and temporal context.

Examples:

```text
The application uses SQLite.

The application must operate offline.

SQLite is preferred for local persistence.

The persistence bug may be caused by transaction ordering.
```

Claims do not inherently possess equal authority or truth.

Their status must be established through evidence, reasoning, observation, or explicit project decisions.

---

# 6. Claim Structure

A Claim conceptually consists of:

```text
Claim
├── subject
├── predicate
├── object/value
├── scope
├── epistemic status
├── temporal validity
├── provenance
└── relationships
```

The final representation is not yet specified.

The subject, predicate, and object/value model is conceptual and may later be expanded to support richer propositions.

---

# 7. Specialized Claim Types

Several previously proposed entities are now understood as specialized forms of Claim.

### Requirement

A Claim expressing a condition the project is expected or obligated to satisfy.

Example:

```text
The application must support offline operation.
```

### Constraint

A Claim expressing a limitation on acceptable project choices or implementations.

Example:

```text
The application must not require a proprietary cloud service.
```

### Proposal

A Claim representing a suggested project direction that has not acquired decision authority.

Example:

```text
Use SQLite for local persistence.
```

### Hypothesis

A Claim representing a proposition used for investigation or reasoning whose validity has not been established.

Example:

```text
The startup failure may be caused by configuration ordering.
```

### Assumption

A Claim temporarily treated as true for the purpose of reasoning or action.

Example:

```text
Assume the database schema is backward compatible.
```

### Observation

A Claim derived directly from an observation of project or external state.

Example:

```text
The repository currently contains a SQLite adapter.
```

These distinctions are semantic classifications, not necessarily implementation inheritance relationships.

---

# 8. Decision

A Decision is not merely a Claim.

A Decision is an authoritative Event in which an authorized participant establishes or changes project direction.

Example:

```text
Decision:
Use SQLite for local persistence.

Authority:
Project owner.

Rationale:
The application is local-first.
```

A Decision may establish one or more Claims.

Conceptually:

```text
Decision Event
      │
      └── establishes ──► Claim
```

This distinction preserves the difference between:

```text
What is true?
What should be true?
What has the project decided to do?
```

---

# 9. Event

An Event represents something that happened within the project or within the project's development process.

Examples:

* Decision accepted
* Requirement changed
* Artifact modified
* Work started
* Work completed
* Test executed
* Claim verified
* Claim refuted
* Conflict detected
* Context generated
* Participant joined
* Project state changed

Events provide temporal continuity.

An Event may create, modify, establish, invalidate, or relate to other project objects.

---

# 10. Event vs State

An Event describes:

> **Something that happened.**

State describes:

> **What is currently believed to be true.**

Example:

```text
Event:
Decision D-001 was accepted on July 29.

State:
SQLite is the accepted persistence technology.
```

Events provide history.

State provides current understanding.

They must not be conflated.

---

# 11. Relation

A Relation explicitly connects two or more objects.

Potential relations include:

```text
implements
depends_on
requires
constrains
supports
contradicts
supersedes
derived_from
evidenced_by
proposed_by
decided_by
affects
references
contains
part_of
verifies
invalidates
```

The final relation vocabulary and cardinality rules remain to be defined.

Relations are first-class conceptual objects even if the eventual storage representation uses edges, references, or other mechanisms.

---

# 12. Evidence

Evidence is information supporting, challenging, or informing a Claim.

Examples:

* source code
* tests
* configuration
* runtime behavior
* logs
* build output
* human statements
* AI analysis
* external documentation
* commits
* measurements
* tool output

Evidence is not automatically truth.

Evidence must be interpreted in relation to Claims, Sources, authority, reliability, and temporal context.

---

# 13. Source

A Source identifies the origin of an Assertion or Evidence item.

Potential sources include:

```text
Human participant
AI participant
Source file
Test
Commit
Documentation
External resource
Runtime observation
Tool
System
```

Source identity and source authority are distinct concepts.

For example, an AI-generated statement may have high technical confidence while lacking authority to establish an architectural decision.

---

# 14. Assertion

An Assertion represents a particular statement, observation, or expression concerning a Claim.

The distinction is:

```text
Claim:
    The project uses SQLite.

Assertion:
    src/database/sqlite.ts imports the SQLite adapter,
    observed on 2026-07-29.
```

A Claim represents the proposition.

An Assertion represents a specific expression or observation concerning that proposition.

Assertions allow Continuum to preserve multiple observations or statements concerning the same Claim without collapsing them into a single undifferentiated record.

---

# 15. Knowledge vs Decision

Continuum explicitly distinguishes:

### Fact / State Claim

A proposition about what is believed to be true.

```text
The application currently uses SQLite.
```

### Requirement

A proposition about what the project must satisfy.

```text
The application must operate offline.
```

### Decision

An authoritative project action establishing a chosen direction.

```text
The project will use SQLite for local persistence.
```

These may concern the same subject but have different semantics.

---

# 16. Epistemic Status

Epistemic status describes the project's current understanding of a Claim.

Initial candidates include:

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

These states remain provisional.

The final epistemic model will distinguish belief, evidence, acceptance, verification, and authority more precisely.

---

# 17. Lifecycle Status

Lifecycle status describes the operational lifecycle of an object.

Potential values include:

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

Epistemic status and lifecycle status are separate dimensions.

For example:

```text
Proposal:
    epistemic status = PROPOSED
    lifecycle status = ACTIVE
```

---

# 18. Authority

Authority describes the degree to which a Participant or Source is authorized to establish or modify project truth or direction.

Authority is distinct from:

* confidence
* evidence quality
* source reliability
* technical expertise
* correctness

An AI may have high confidence while lacking authority to establish an architectural Decision.

---

# 19. Confidence

Confidence represents belief in the correctness of a Claim, inference, interpretation, or conclusion.

Confidence is distinct from Authority.

Example:

```text
AI analysis:
    confidence = high
    authority = insufficient for architectural decision
```

The final confidence model may be qualitative, quantitative, probabilistic, or hybrid.

---

# 20. Temporal Model

Continuum must preserve the temporal dimensions of project knowledge.

Potential timestamps include:

```text
created_at
observed_at
recorded_at
valid_from
valid_until
superseded_at
```

These represent different concepts and must not be collapsed into one timestamp.

Historical project knowledge should remain recoverable.

---

# 21. Identity

Every persistent object must eventually possess a stable identity independent of its representation.

For example:

```text
Requirement R-001
```

may move between files, change wording, or change representation while retaining its identity.

Therefore:

```text
Object identity
    ≠
File path
    ≠
Document title
    ≠
Representation
```

The final identifier scheme remains undefined.

---

# 22. Identity vs Version

Object identity must be distinguished from object version.

For example:

```text
Requirement R-001
    ├── version 1
    ├── version 2
    ├── version 3
    └── current
```

Changing the representation or content of an object does not necessarily create a new identity.

However, some changes may instead constitute a new object that supersedes the previous one.

The rules governing this distinction remain to be defined.

---

# 23. Supersession

Supersession represents the replacement of one project object or understanding by another while preserving historical existence.

Example:

```text
Decision D-001:
    Use React.

Decision D-042:
    Use SolidJS.

D-042 supersedes D-001.
```

D-001 remains historically valid as a past decision.

Current project understanding is determined through temporal validity and supersession.

---

# 24. Contradiction

Continuum must explicitly represent contradictory Claims, Evidence, Decisions, Requirements, or States.

Example:

```text
Decision D-001:
    Use SQLite.

Observed repository state:
    PostgreSQL configuration is active.
```

This should result in an explicit conflict representation.

Continuum must not silently select whichever interpretation is easiest for an AI participant.

---

# 25. Project Graph

The conceptual project graph may be represented as:

```text
                       PROJECT
                          │
              ┌───────────┼───────────┐
              ▼           ▼           ▼
           ENTITIES     CLAIMS       EVENTS
              │           │           │
              └───────────┼───────────┘
                          │
                       RELATIONS
                          │
                          ▼
                       EVIDENCE
                          │
                          ▼
                        SOURCES
```

A more detailed example:

```text
Decision D-001
      │
      │ establishes
      ▼
Claim C-001
"The project uses SQLite."
      ▲
      │ supported_by
      │
Evidence E-001
      │
      │ observed_in
      ▼
Artifact A-001
src/database/sqlite.ts
```

The graph allows Continuum to answer not only:

> What does the project believe?

but also:

> Why does it believe this?

and:

> Who or what established it?

and:

> What evidence supports it?

and:

> What contradicts it?

---

# 26. Knowledge Flow

A conceptual knowledge lifecycle may resemble:

```text
Observation
    │
    ▼
Assertion
    │
    ▼
Claim
    │
    ├──► Investigation
    │
    ├──► Evidence
    │
    └──► Decision
              │
              ▼
          Accepted Claim
              │
              ▼
        Implementation
              │
              ▼
            Evidence
              │
              ▼
          Verification
              │
              ▼
       Updated Project State
```

This is a conceptual model, not yet an implementation requirement.

---

# 27. What We Have Established

The current conceptual model distinguishes:

```text
Entity
    Something the project recognizes as existing.

Claim
    A proposition about the project.

Event
    Something that happened.

Relation
    A connection between project objects.

Evidence
    Information supporting or challenging a proposition.

Source
    The origin of evidence or assertions.

Decision
    An authoritative event establishing project direction.

State
    Current project understanding derived from the graph.
```

---

# 28. Open Ontological Questions

The following remain unresolved:

1. Is Claim sufficiently expressive as a primitive?
2. Should Assertion remain distinct from Evidence?
3. Should Requirement and Constraint be specialized Claims?
4. Should Decision be represented exclusively as an Event?
5. Should State be a first-class object or a derived view?
6. Should Evidence be an Entity, a Relation, or both?
7. How should authority be modeled?
8. How should confidence be modeled?
9. How should temporal validity be modeled?
10. How should Claims be merged?
11. How should equivalent Claims be identified?
12. How should contradictions be represented?
13. How should AI-generated Claims differ from human Claims?
14. What makes a Claim "verified"?
15. Which changes require human authorization?
16. Which objects must be immutable?
17. Which objects may be revised?
18. How should historical state be reconstructed?

These questions must be resolved before implementation schemas are finalized.

---

# 29. Ontology Design Rule

The ontology defines the semantic world of Continuum.

It must remain independent of:

* database technology
* file format
* programming language
* AI provider
* model architecture
* API
* CLI
* UI

Implementation mechanisms must conform to the ontology rather than redefining it implicitly.
