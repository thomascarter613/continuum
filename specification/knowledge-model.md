# Continuum Knowledge Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

## 1. Purpose

The Continuum Knowledge Model defines how project information becomes structured knowledge and how knowledge relates to other project knowledge.

The model distinguishes:

* observations
* assertions
* interpretations
* propositions
* claims
* evidence
* decisions
* relationships
* epistemic states
* semantic equivalence
* entailment
* contradiction
* compatibility
* derivation
* dependency

The model is conceptual and does not prescribe storage technology.

---

# 2. Information vs Knowledge

Continuum distinguishes raw information from interpreted project knowledge.

Conceptually:

```text
Information
    ↓
Observation
    ↓
Interpretation
    ↓
Claim / Knowledge
```

The transformation between stages must remain traceable.

---

# 3. Observation

An Observation represents something directly encountered or measured.

Examples include:

```text
A source file exists.
A test exited with status 1.
A package.json contains a dependency.
A Git commit changed a file.
An API returned HTTP 500.
```

Observations should be attributable to a Source and Actor where possible.

---

# 4. Assertion

An Assertion is a statement made by an Actor.

Examples:

```text
Developer:
    "We will use PostgreSQL."

AI:
    "The authentication layer appears to use JWT."

Documentation:
    "Production requires Redis."
```

An Assertion is not automatically true.

```text
Assertion ≠ Fact
```

---

# 5. Interpretation

An Interpretation is an inferred meaning derived from one or more observations.

Example:

```text
Observation:
    src/auth/jwt.ts exists.

Observation:
    authentication middleware imports jwt.ts.

Interpretation:
    JWT participates in authentication.
```

Interpretations must be traceable to their supporting observations.

---

# 6. Proposition

A Proposition is semantic content that may be expressed by one or more Assertions.

Example:

```text
PROPOSITION-001

subject:
    production

predicate:
    uses_database

object:
    PostgreSQL
```

Multiple Actors may independently assert the same Proposition.

---

# 7. Assertion vs Proposition

An Assertion represents an Actor's expression.

A Proposition represents the semantic content of that expression.

Therefore:

```text
Proposition
    ≠
Assertion
```

Example:

```text
PROPOSITION-001
"The production database is PostgreSQL"

ASSERTION-001
asserted_by: AI Agent A
supports: PROPOSITION-001

ASSERTION-002
asserted_by: Developer B
supports: PROPOSITION-001
```

---

# 8. Claim

A Claim is a proposition that Continuum tracks as project knowledge.

Conceptually:

```text
Claim
├── subject
├── predicate
├── object
├── scope
├── temporal validity
├── epistemic status
├── provenance
└── confidence
```

Claims should be semantically decomposable rather than stored only as natural-language strings.

---

# 9. Claim Identity

Claims may express equivalent or near-equivalent propositions.

Continuum should eventually support semantic normalization so that:

```text
"The app uses Postgres."

"Production's database is PostgreSQL."

"We use PostgreSQL in production."
```

can potentially be recognized as semantically related rather than treated as unrelated text.

Semantic equivalence must not be based solely on string similarity.

---

# 10. Knowledge Lifecycle

A conceptual knowledge lifecycle is:

```text
OBSERVED
    ↓
INTERPRETED
    ↓
PROPOSED
    ↓
ACCEPTED
    ↓
SUPPORTED
    ↓
VERIFIED
```

Alternative outcomes may include:

```text
REJECTED
CONTRADICTED
CONFLICTED
SUPERSEDED
```

Not every Claim must pass through every state.

---

# 11. Epistemic States

Potential epistemic states include:

```text
UNKNOWN
ASSUMED
HYPOTHESIZED
OBSERVED
INTERPRETED
PROPOSED
ACCEPTED
SUPPORTED
VERIFIED
CONTRADICTED
CONFLICTED
REJECTED
SUPERSEDED
```

Epistemic state describes the project's current understanding of knowledge.

It does not by itself establish objective truth.

---

# 12. Semantic Relationships

Continuum begins with the following conceptual relation vocabulary.

### Identity

```text
same_as
equivalent_to
alias_of
instance_of
```

### Logic

```text
implies
entails
contradicts
compatible_with
inconsistent_with
```

### Temporal

```text
precedes
follows
active_during
supersedes
superseded_by
```

### Causal

```text
causes
caused_by
contributes_to
prevents
```

### Structural

```text
contains
part_of
depends_on
composed_of
implements
implemented_by
```

### Knowledge

```text
derived_from
supported_by
contradicted_by
evidenced_by
explains
justifies
```

### Decision

```text
proposes
accepted_as
rejected_as
establishes
invalidates
```

### Work

```text
addresses
blocks
blocked_by
produces
consumes
tests
verified_by
```

This vocabulary is provisional.

---

# 13. Relation Properties

Relations may have semantic properties such as:

```text
directed
symmetric
transitive
reflexive
inverse
```

Examples:

```text
depends_on:
    directed
    non-symmetric

equivalent_to:
    potentially symmetric
    potentially transitive

part_of:
    directed
    potentially transitive

contradicts:
    potentially symmetric
```

Final relation algebra remains unspecified.

---

# 14. Entailment

Entailment means one proposition logically or semantically implies another.

Example:

```text
Claim A:
The system uses PostgreSQL 16.

Claim B:
The system uses PostgreSQL.
```

Conceptually:

```text
A
│
└── entails → B
```

Entailment is directional.

```text
A → B
```

does not necessarily imply:

```text
B → A
```

---

# 15. Equivalence

Two propositions are equivalent when they represent the same semantic proposition under the relevant scope and temporal conditions.

Example:

```text
A:
Production uses PostgreSQL.

B:
The production database is PostgreSQL.
```

Potentially:

```text
A ↔ B
```

Equivalence must consider semantic meaning, scope, time, and object identity.

---

# 16. Contradiction

Two Claims contradict when their semantic content cannot simultaneously hold under the same relevant conditions.

Example:

```text
Claim A:
Production uses PostgreSQL.

Claim B:
Production uses SQLite.
```

Potential contradiction:

```text
A
↕
contradicts
↕
B
```

Contradiction evaluation must consider:

```text
subject
predicate
object
scope
time
version
environment
epistemic status
```

---

# 17. Compatibility

Claims may coexist without implying one another.

Example:

```text
Frontend uses SolidJS.

Backend uses Go.
```

These are potentially:

```text
compatible_with
```

rather than equivalent, entailing, or contradictory.

---

# 18. Dependency

A Dependency expresses a condition in which one object relies upon another.

Examples:

```text
Service → Library

Feature → Requirement

Test → Implementation

Decision → Constraint

Implementation → Decision
```

Dependencies may eventually be typed:

```text
runtime
build
development
conceptual
organizational
decision
```

---

# 19. Derivation

`derived_from` indicates that knowledge was inferred from one or more other objects.

Example:

```text
Test passed
    +
Test covers Requirement R-001
    ↓
Requirement R-001 is verified
```

Conceptually:

```text
Claim
    derived_from
       ├── Observation A
       └── Observation B
```

Derivation should preserve provenance.

---

# 20. Evidence

Evidence provides support for or against a Claim.

Examples:

```text
Claim
    supported_by
Test Result

Claim
    contradicted_by
Observed Failure
```

Evidence is not identical to truth.

It is information that affects epistemic evaluation.

---

# 21. Evidence Graph

Example:

```text
CLAIM-001
"API supports OIDC"
      │
      ├── supported_by → TEST-019
      │
      └── contradicted_by → BUG-022
```

The resulting epistemic state may therefore be:

```text
CONFLICTED
```

rather than automatically true or false.

---

# 22. Contextual Truth

Claims must be evaluated within context.

Relevant contextual dimensions may include:

```text
time
scope
environment
version
component
tenant
deployment
configuration
```

Example:

```text
Claim A:
development database = SQLite

Claim B:
production database = PostgreSQL
```

These Claims coexist because their scopes differ.

---

# 23. Semantic Normalization

Continuum will eventually require semantic normalization capable of determining whether different representations refer to:

```text
the same proposition
related propositions
more specific propositions
more general propositions
contradictory propositions
compatible propositions
```

This normalization must not rely solely on lexical similarity.

---

# 24. Specificity

A proposition may be more specific than another.

Example:

```text
A:
The application uses PostgreSQL 16.

B:
The application uses PostgreSQL.
```

A is more specific than B.

Conceptually:

```text
A
│
└── entails → B
```

This relationship should eventually support hierarchical knowledge retrieval.

---

# 25. Knowledge Graph

Continuum's knowledge graph conceptually contains:

```text
                         PROJECT
                            │
          ┌─────────────────┼──────────────────┐
          ▼                 ▼                  ▼
       OBJECTS          PROPOSITIONS         EVENTS
          │                 │                  │
          │          ┌──────┴──────┐           │
          │          ▼             ▼           │
          │     ASSERTIONS      CLAIMS         │
          │          │             │           │
          └──────────┼─────────────┼───────────┘
                     │             │
                     └──────┬──────┘
                            ▼
                       RELATIONSHIPS
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
           EVIDENCE      SOURCES       CONTEXT
```

The graph is semantic, not necessarily a graph database implementation.

---

# 26. Knowledge Graph Requirements

The knowledge graph must eventually support:

* identity resolution
* semantic equivalence
* entailment
* contradiction detection
* compatibility detection
* dependency traversal
* provenance traversal
* evidence traversal
* temporal filtering
* scope filtering
* authority filtering
* epistemic-state filtering

---

# 27. Reasoning Without Hidden Chain-of-Thought

Continuum should support explainable reasoning without depending on private AI reasoning traces.

It should preserve:

```text
inputs
observations
claims
evidence
relations
rules
outputs
actions
```

rather than requiring hidden model chain-of-thought.

This allows Continuum to answer:

> Why does the system currently believe this?

through explicit project evidence and relationships.

---

# 28. AI as a Knowledge Participant

AI is treated as a participant in the knowledge system.

AI may:

```text
observe
interpret
assert
propose
infer
summarize
retrieve
recommend
execute
```

AI does not automatically gain authority by performing these actions.

Authority remains an independent property.

---

# 29. Knowledge Integrity

Continuum should avoid silently collapsing distinct semantic states.

In particular:

```text
Assertion ≠ Claim
Claim ≠ Truth
Evidence ≠ Truth
Observation ≠ Interpretation
Proposal ≠ Decision
Confidence ≠ Authority
Similarity ≠ Equivalence
Contradiction ≠ Difference
Current State ≠ Historical Record
```

These distinctions are foundational.

---

# 30. Open Questions

The following remain unresolved:

1. Proposition identity algorithm
2. Semantic normalization
3. Claim deduplication
4. Equivalence detection
5. Entailment engine
6. Contradiction detection
7. Compatibility detection
8. Relation ontology governance
9. Relation algebra
10. Ontology extension mechanism
11. Predicate vocabulary
12. Object/property semantics
13. Knowledge state transitions
14. Evidence weighting
15. Source reliability
16. Confidence calculation
17. Scope inheritance
18. Temporal reasoning
19. Semantic versioning
20. AI-assisted semantic classification

---

# 31. Design Rule

Continuum must preserve the distinction between what was observed, what was said, what was inferred, what was believed, what was decided, and what was verified.

Semantic relationships must be explicit enough that an AI can reconstruct not merely project information, but the project's evolving understanding of that information.
