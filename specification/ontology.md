# Continuum Ontology

**Status:** Initial Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

## 1. Purpose

This document establishes the initial conceptual ontology of Continuum.

The ontology identifies the kinds of things Continuum must be able to represent.

It is intentionally incomplete.

The purpose of this version is to establish the conceptual boundary of the system before schemas, APIs, storage models, and implementation are designed.

---

## 2. Ontological Principle

Continuum represents a software project as a network of persistent entities and relationships rather than as a collection of independent documents.

The central model is:

```text
Project
  │
  ├── Intent
  ├── Goals
  ├── Requirements
  ├── Constraints
  ├── Decisions
  ├── Assumptions
  ├── Questions
  ├── Proposals
  ├── Hypotheses
  ├── Concepts
  ├── Architecture
  ├── Artifacts
  ├── Work
  ├── State
  ├── Evidence
  ├── Events
  └── Relationships
```

These entities are connected through explicit relationships.

---

## 3. Initial Entity Classes

### 3.1 Project

The persistent software project whose understanding Continuum maintains.

A Project is the root scope for project memory.

---

### 3.2 Participant

A human, AI agent, tool, service, or external system interacting with the project.

---

### 3.3 Session

A bounded interaction involving one or more Participants and a Project.

---

### 3.4 Intent

The fundamental purpose or desired outcome underlying a project or project activity.

---

### 3.5 Goal

A desired outcome pursued by the project.

Goals may be strategic, product-oriented, technical, operational, or local to a particular work effort.

---

### 3.6 Requirement

A condition, capability, behavior, or property that the project is expected or required to satisfy.

---

### 3.7 Constraint

A limitation or boundary that restricts acceptable project decisions or implementations.

---

### 3.8 Decision

An explicitly accepted determination regarding the project.

A Decision must eventually support:

* rationale
* authority
* evidence
* alternatives
* consequences
* temporal validity
* supersession

---

### 3.9 Proposal

A suggested course of action or possible project state that has not yet acquired decision authority.

---

### 3.10 Hypothesis

A proposition that may be useful for reasoning but has not yet been sufficiently established as project truth.

---

### 3.11 Assumption

A proposition temporarily treated as true for the purpose of reasoning or action.

Assumptions must remain distinguishable from verified facts.

---

### 3.12 Question

An unresolved request for information, clarification, judgment, or decision.

---

### 3.13 Concept

A meaningful domain, technical, architectural, or project-specific concept recognized by the project.

---

### 3.14 Architecture

The structural organization of the software system and the relationships between its significant parts.

Architecture may include:

* components
* boundaries
* interfaces
* dependencies
* patterns
* deployment structures
* data flows
* architectural decisions

---

### 3.15 Artifact

A persistent or observable project object.

Examples include:

* source files
* tests
* configuration
* specifications
* schemas
* diagrams
* documentation
* generated files
* commits
* external resources

---

### 3.16 Work

A bounded effort undertaken to change, investigate, verify, or otherwise advance the project.

Work may include tasks, work packets, investigations, implementation efforts, refactoring, migrations, or other activities.

---

### 3.17 State

A representation of what is currently believed to be true about some project entity, subsystem, artifact, or the project as a whole.

State is temporal and may change.

---

### 3.18 Evidence

An observable source supporting or challenging a claim about the project.

Evidence may originate from:

* source code
* tests
* configuration
* execution
* human statements
* external documentation
* commits
* tool output
* other project artifacts

---

### 3.19 Event

A record that something happened.

Examples include:

* decision accepted
* requirement changed
* artifact modified
* test executed
* conflict detected
* work started
* work completed
* context generated

Events provide historical continuity.

---

### 3.20 Relationship

An explicit connection between entities.

Examples include:

```text
implements
depends_on
satisfies
constrains
supports
contradicts
supersedes
derived_from
evidenced_by
proposed_by
decided_by
affects
contains
references
```

The relationship model will be formally designed later.

---

### 3.21 Conflict

An explicit representation of incompatible claims, states, decisions, requirements, evidence, or other project knowledge.

A Conflict must not be silently resolved merely by choosing the most convenient interpretation.

---

### 3.22 Context

A generated, task-specific representation of relevant project knowledge assembled for a participant or operation.

Context is derived from persistent project memory and evidence.

Context is not itself the authoritative source of project truth.

---

## 4. Initial Epistemic States

Project knowledge may eventually require explicit epistemic classification.

Initial candidates:

```text
OBSERVED
DERIVED
PROPOSED
ASSUMED
DECIDED
REJECTED
SUPERSEDED
UNKNOWN
CONFLICTED
```

These classifications are provisional until the formal knowledge model is designed.

---

## 5. Initial Lifecycle States

Entities may also require lifecycle states distinct from epistemic states.

Potential lifecycle concepts include:

```text
DRAFT
ACTIVE
ACCEPTED
IN_PROGRESS
COMPLETED
ABANDONED
REJECTED
SUPERSEDED
ARCHIVED
```

Epistemic state and lifecycle state must not be conflated.

For example:

```text
A proposal may be:
  epistemic state: PROPOSED
  lifecycle state: ACTIVE
```

---

## 6. Initial Temporal Model

Continuum must support the distinction between:

* when something was created
* when something became valid
* when something stopped being valid
* when something was observed
* when something was recorded
* when something was superseded

The final temporal model is not yet defined.

---

## 7. Initial Provenance Model

Project knowledge must eventually support provenance sufficient to answer:

```text
Who or what produced this?
When?
From what source?
By what process?
Under what authority?
Based upon what evidence?
```

The exact provenance model remains to be specified.

---

## 8. Ontology Design Rule

The ontology must remain independent from:

* programming language
* serialization format
* database technology
* AI provider
* CLI implementation
* UI implementation

Those are implementation concerns.

The ontology defines what Continuum means, not how it stores or displays it.

---

## 9. Deliberate Unknowns

The following are intentionally unresolved:

* exact entity identifiers
* exact schemas
* entity inheritance
* relationship cardinality
* temporal representation
* provenance representation
* authority model
* versioning model
* event model
* storage format
* query language
* context ranking
* context assembly algorithm
* AI protocol
* CLI design
* API design

These will be resolved through subsequent specifications and architectural decisions.

---

## 10. Next Ontological Task

The next task is to determine whether the initial entity classes represent genuinely distinct concepts or whether some should be unified, split, or reclassified.

No implementation schema should be created until this analysis is complete.
