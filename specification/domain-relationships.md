# Continuum Domain Relationship Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

This document formally defines the relationships between entities in the Continuum domain model.

The purpose is to establish:

* relationship semantics
* cardinality
* ownership
* dependency
* causality
* temporal validity
* provenance
* epistemic relationships
* traceability
* state transitions

This document is subordinate to:

```text
specification/domain-model.md
```

The domain model defines **what exists**.

This document defines **how those things relate**.

---

# 2. Relationship Philosophy

Continuum is fundamentally a relationship system.

A project is not merely a collection of files, tasks, and memories.

Its meaning emerges from relationships such as:

```text
Requirement
    └── satisfied_by → Task

Task
    └── implemented_by → Artifact

Action
    └── modifies → Artifact

Observation
    └── supports → Knowledge

Decision
    └── constrains → Task

Outcome
    └── changes → Knowledge

Context
    └── derived_from → Knowledge
```

The graph is therefore a first-class part of the domain.

---

# 3. Relationship Model

Every relationship can conceptually be represented as:

```text
Subject
    |
    | relationship type
    |
    v
Object
```

Example:

```text
Task-123
    |
    | implements
    |
    v
Requirement-456
```

A relationship may additionally possess:

```text
relationship_id
type
source
confidence
authority
created_at
valid_from
valid_until
status
metadata
```

---

# 4. Relationship Categories

Continuum recognizes the following relationship categories:

```text
Structural
Ownership
Containment
Dependency
Semantic
Temporal
Causal
Epistemic
Traceability
Participation
Derivation
Provenance
Supersession
```

---

# 5. Structural Relationships

Structural relationships describe how entities are organized.

Examples:

```text
Project contains Repository
Repository contains Artifact
Task contains Subtask
Project contains Task
```

Structural relationships generally change slowly.

---

# 6. Ownership Relationships

Ownership indicates responsibility or logical belonging.

Examples:

```text
Task belongs_to Project
Repository belongs_to Project
Session belongs_to Project
Artifact belongs_to Repository
```

Ownership does not necessarily mean legal ownership.

It means:

> This entity is logically governed by or scoped to another entity.

---

# 7. Containment

Containment indicates that one entity contains another.

Examples:

```text
Project
 └── Repository
      └── Artifact

Task
 └── Subtask
```

Containment is stronger than simple association.

---

# 8. Dependency

Dependency means that one entity relies upon another.

Examples:

```text
Task A
    depends_on
Task B
```

```text
Artifact A
    depends_on
Artifact B
```

Dependency should not be confused with ownership.

---

# 9. Semantic Relationships

Semantic relationships describe meaning.

Examples:

```text
Artifact implements Requirement
Document describes Artifact
Task addresses Requirement
Pattern applies_to Artifact
```

---

# 10. Temporal Relationships

Temporal relationships describe ordering or succession.

Examples:

```text
Event A precedes Event B
Decision A supersedes Decision B
ArtifactVersion B follows ArtifactVersion A
```

Temporal relationships must preserve historical order.

---

# 11. Causal Relationships

Causal relationships describe cause and effect.

Examples:

```text
Action
    causes
Event
```

```text
Requirement
    causes
Task
```

```text
Outcome
    causes
KnowledgeUpdate
```

Causal claims should carry confidence when causality is inferred rather than directly established.

---

# 12. Epistemic Relationships

Epistemic relationships describe relationships between evidence and knowledge.

Examples:

```text
Evidence supports Knowledge
Evidence contradicts Knowledge
Observation suggests Hypothesis
Question challenges Knowledge
Knowledge supersedes Knowledge
```

These relationships are central to Continuum's trust model.

---

# 13. Traceability Relationships

Traceability relationships connect intention to implementation and outcome.

Canonical chain:

```text
Goal
 ↓
Requirement
 ↓
Task
 ↓
Action
 ↓
Artifact Change
 ↓
Test
 ↓
Observation
 ↓
Outcome
 ↓
Knowledge
```

The chain may contain additional nodes.

---

# 14. Participation Relationships

Participation describes involvement.

Examples:

```text
Human participates_in Session
Agent participates_in Session
Agent performs Action
Tool participates_in Action
```

---

# 15. Derivation Relationships

Derivation describes information produced from other information.

Examples:

```text
Knowledge derived_from Observation
ContextPackage derived_from Knowledge
Summary derived_from Session
Pattern derived_from Events
```

Derivation is different from causality.

---

# 16. Provenance Relationships

Provenance identifies the source of information.

Examples:

```text
Observation sourced_from TestRunner
Knowledge supported_by Observation
Decision proposed_by Agent
Decision accepted_by Human
```

---

# 17. Supersession

Supersession represents replacement without erasing history.

Example:

```text
Decision A
    superseded_by
Decision B
```

The original remains historically valid as an artifact of project evolution.

---

# 18. Relationship Cardinality

Cardinality describes how many entities may participate in a relationship.

Notation:

```text
1      exactly one
0..1   zero or one
*      many
1..*   one or more
```

---

# 19. Project Relationships

```text
Project
 ├── contains → Repository       0..*
 ├── contains → Workspace        0..*
 ├── contains → Environment      0..*
 ├── contains → Artifact         0..*
 ├── contains → Goal             0..*
 ├── contains → Requirement      0..*
 ├── contains → Constraint       0..*
 ├── contains → Task             0..*
 ├── contains → Plan             0..*
 ├── contains → Decision         0..*
 ├── contains → Session          0..*
 ├── contains → Event            0..*
 └── contains → Knowledge        0..*
```

A Project may exist with no current Tasks.

---

# 20. Repository Relationships

```text
Repository
 ├── belongs_to → Project          1
 ├── contains → Artifact           0..*
 └── has → ArtifactVersion         0..*
```

---

# 21. Workspace Relationships

```text
Workspace
 ├── belongs_to → Project          1
 ├── associated_with → Repository  0..*
 └── operates_in → Environment     0..1
```

---

# 22. Artifact Relationships

```text
Artifact
 ├── belongs_to → Repository       0..1
 ├── belongs_to → Project          1
 ├── has → ArtifactVersion         0..*
 ├── implements → Requirement      0..*
 ├── related_to → Task              0..*
 ├── modified_by → Action           0..*
 ├── observed_by → Observation      0..*
 └── tested_by → Artifact           0..*
```

An Artifact may exist outside a Repository.

Examples include generated artifacts or external resources.

---

# 23. Artifact Dependency

Artifacts may depend upon other artifacts.

```text
Artifact A
    depends_on
Artifact B
```

Cardinality:

```text
Artifact → Artifact
0..* → 0..*
```

Dependency relationships may include:

```text
runtime
compile
test
build
configuration
semantic
```

---

# 24. Goal Relationships

```text
Goal
 ├── belongs_to → Project           1
 ├── decomposes_to → Goal           0..*
 ├── satisfied_by → Requirement     0..*
 └── realized_by → Task             0..*
```

---

# 25. Requirement Relationships

```text
Requirement
 ├── belongs_to → Project           1
 ├── derived_from → Goal            0..*
 ├── constrained_by → Constraint    0..*
 ├── satisfied_by → Task             0..*
 ├── implemented_by → Artifact      0..*
 └── verified_by → Artifact         0..*
```

---

# 26. Constraint Relationships

```text
Constraint
 ├── belongs_to → Project           1
 ├── applies_to → Task              0..*
 ├── applies_to → Artifact          0..*
 └── applies_to → Requirement       0..*
```

---

# 27. Task Relationships

```text
Task
 ├── belongs_to → Project           1
 ├── parent_of → Task               0..1
 ├── depends_on → Task              0..*
 ├── supports → Goal                0..*
 ├── addresses → Requirement        0..*
 ├── constrained_by → Constraint    0..*
 ├── implemented_by → Artifact      0..*
 ├── planned_by → Plan              0..*
 ├── worked_on_in → Session         0..*
 ├── produces → Outcome             0..*
 └── informed_by → Knowledge        0..*
```

---

# 28. Task Dependency Graph

Task dependencies form a directed graph:

```text
Task A
  |
  | depends_on
  v
Task B
  |
  | depends_on
  v
Task C
```

The graph should not permit dependency cycles unless explicitly supported by a future workflow model.

---

# 29. Plan Relationships

```text
Plan
 ├── belongs_to → Project           1
 ├── contains → Task                1..*
 ├── realizes → Goal                0..*
 ├── addresses → Requirement        0..*
 ├── constrained_by → Constraint    0..*
 └── superseded_by → Plan           0..1
```

---

# 30. Proposal Relationships

```text
Proposal
 ├── belongs_to → Project           1
 ├── proposes → Decision            0..1
 ├── addresses → Task               0..*
 ├── supported_by → Evidence        0..*
 ├── proposed_by → Actor            1
 └── becomes → Decision             0..1
```

---

# 31. Decision Relationships

```text
Decision
 ├── belongs_to → Project           1
 ├── addresses → Requirement        0..*
 ├── constrains → Task              0..*
 ├── constrains → Artifact          0..*
 ├── based_on → Evidence            0..*
 ├── proposed_by → Actor            0..*
 ├── accepted_by → Actor            1
 └── supersedes → Decision          0..*
```

---

# 32. Decision Authority

An accepted Decision must have an authority.

For example:

```text
Decision
    accepted_by
    Human
```

or:

```text
Decision
    accepted_by
    GovernancePolicy
```

An Agent may propose a Decision but should not silently become its authority.

---

# 33. Session Relationships

```text
Session
 ├── belongs_to → Project           1
 ├── involves → Actor               1..*
 ├── works_on → Task                0..*
 ├── uses → ContextPackage          0..*
 ├── contains → Action              0..*
 ├── contains → Event               0..*
 ├── contains → Observation         0..*
 └── produces → Outcome             0..*
```

---

# 34. Session Scope

A Session must have:

```text
project_id
started_at
```

A Session may optionally have:

```text
task_id
actor_id
ended_at
context_package_id
```

---

# 35. Action Relationships

```text
Action
 ├── performed_by → Actor           1
 ├── occurs_in → Session             1
 ├── addresses → Task               0..*
 ├── uses → Tool                    0..1
 ├── reads → Artifact               0..*
 ├── modifies → Artifact            0..*
 ├── produces → Outcome             0..1
 └── causes → Event                 0..*
```

---

# 36. Action and Artifact Mutation

An Action may modify multiple Artifacts.

```text
Action
   |
   ├── modifies → Artifact A
   ├── modifies → Artifact B
   └── modifies → Artifact C
```

Each modification should eventually be traceable to a specific ArtifactVersion or repository revision.

---

# 37. Event Relationships

Events form the temporal history.

```text
Event
 ├── occurs_in → Session             0..1
 ├── caused_by → Action              0..*
 ├── precedes → Event                0..*
 ├── affects → Entity                0..*
 └── produces → Observation          0..*
```

---

# 38. Event Ordering

Events should have an ordering mechanism.

Minimum:

```text
timestamp
```

Preferred future support:

```text
sequence
logical_clock
causal_parent
```

Wall-clock time alone is insufficient for deterministic reconstruction in distributed systems.

---

# 39. Observation Relationships

```text
Observation
 ├── belongs_to → Session            0..1
 ├── observes → Entity               1..*
 ├── produced_by → Actor             1
 ├── sourced_from → Evidence         0..*
 ├── supports → Knowledge            0..*
 ├── contradicts → Knowledge         0..*
 ├── suggests → Hypothesis            0..*
 └── raises → Question               0..*
```

---

# 40. Evidence Relationships

```text
Evidence
 ├── belongs_to → Project             1
 ├── supports → Knowledge            0..*
 ├── contradicts → Knowledge         0..*
 ├── informs → Decision              0..*
 └── sourced_from → Provenance       1..*
```

---

# 41. Knowledge Relationships

```text
Knowledge
 ├── belongs_to → Project             1
 ├── supported_by → Evidence         0..*
 ├── contradicted_by → Evidence      0..*
 ├── derived_from → Knowledge        0..*
 ├── related_to → Artifact            0..*
 ├── related_to → Task                0..*
 ├── constrains → Task               0..*
 ├── informs → ContextPackage        0..*
 └── supersedes → Knowledge          0..*
```

---

# 42. Knowledge Graph

Knowledge relationships form a graph:

```text
Knowledge A
    |
    | derived_from
    v
Knowledge B
    |
    | supported_by
    v
Evidence C
```

Knowledge can therefore be both:

```text
derived
```

and:

```text
evidence
```

for subsequent knowledge.

---

# 43. Fact Relationships

A Fact is a specialized Knowledge object.

```text
Fact
 ├── supported_by → Evidence         0..*
 ├── contradicted_by → Evidence      0..*
 └── superseded_by → Fact            0..1
```

---

# 44. Hypothesis Relationships

```text
Hypothesis
 ├── proposed_by → Actor              1
 ├── supported_by → Evidence         0..*
 ├── contradicted_by → Evidence      0..*
 ├── tested_by → Experiment          0..*
 └── resolves_to → Knowledge         0..1
```

---

# 45. Question Relationships

```text
Question
 ├── belongs_to → Project             1
 ├── raised_by → Actor                1
 ├── concerns → Entity                0..*
 ├── generated_by → Observation       0..*
 ├── resolved_by → Knowledge         0..1
 └── generates → Task                0..*
```

---

# 46. Uncertainty Relationships

Uncertainty may qualify any Knowledge object.

```text
Uncertainty
    qualifies
    ↓
Knowledge
```

Uncertainty may represent:

```text
confidence
ambiguity
missing evidence
conflicting evidence
staleness
```

---

# 47. Pattern Relationships

```text
Pattern
 ├── derived_from → Observation      1..*
 ├── applies_to → Artifact           0..*
 ├── applies_to → Task               0..*
 ├── informs → Knowledge             0..*
 └── detected_by → Actor             0..*
```

---

# 48. Memory Relationships

Memory is a durable representation of experience.

```text
Memory
 ├── belongs_to → Project             1
 ├── derived_from → Session           0..*
 ├── derived_from → Event             0..*
 ├── derived_from → Observation       0..*
 ├── contains → Knowledge             0..*
 └── informs → ContextPackage         0..*
```

---

# 49. Memory Is Not the Source of Truth

Memory may be stale.

Current project state should generally be established from:

```text
current artifacts
current repository state
current configuration
current verified observations
```

Memory supplies historical and contextual information.

---

# 50. ContextRequest Relationships

```text
ContextRequest
 ├── requested_by → Actor             1
 ├── for_project → Project             1
 ├── for_task → Task                  0..1
 ├── for_session → Session            0..1
 └── produces → ContextPackage        1..*
```

---

# 51. ContextPackage Relationships

```text
ContextPackage
 ├── produced_for → Actor             1
 ├── requested_by → ContextRequest   1
 ├── derived_from → Knowledge         0..*
 ├── derived_from → Artifact          0..*
 ├── derived_from → Event             0..*
 ├── derived_from → State             0..*
 └── used_in → Session                0..*
```

---

# 52. ContextItem Relationships

Each ContextItem should preserve its source.

```text
ContextItem
    sourced_from
    ↓
Entity / Event / Knowledge / Artifact
```

This allows an AI to distinguish:

```text
fact
inference
historical event
current state
recommendation
uncertainty
```

---

# 53. Actor Relationships

```text
Actor
 ├── participates_in → Session        0..*
 ├── performs → Action                0..*
 ├── proposes → Proposal              0..*
 ├── proposes → Decision              0..*
 ├── provides → Evidence              0..*
 └── receives → ContextPackage        0..*
```

---

# 54. Human Relationships

```text
Human
 ├── participates_in → Session        0..*
 ├── performs → Action                0..*
 ├── approves → Decision              0..*
 └── provides → Evidence              0..*
```

---

# 55. Agent Relationships

```text
Agent
 ├── uses → Model                    1..*
 ├── uses → Tool                     0..*
 ├── participates_in → Session       0..*
 ├── performs → Action               0..*
 ├── proposes → Proposal             0..*
 └── receives → ContextPackage       0..*
```

---

# 56. Tool Relationships

```text
Tool
 ├── invoked_by → Agent              0..*
 └── produces → Outcome              0..*
```

---

# 57. Model Relationships

```text
Model
    used_by
    ↓
Agent
```

A Model should not directly own project state.

---

# 58. Provenance Model

Every important information-bearing object should eventually be able to answer:

```text
Where did this come from?
Who produced it?
When was it produced?
What was it based upon?
How was it transformed?
```

---

# 59. Provenance Chain

Example:

```text
Repository File
    ↓
Tool Read
    ↓
Observation
    ↓
AI Inference
    ↓
Hypothesis
    ↓
Decision Proposal
    ↓
Human Approval
    ↓
Decision
```

The chain must remain reconstructable.

---

# 60. Epistemic Graph

Continuum's knowledge graph should distinguish:

```text
Observed
    ↓
Interpreted
    ↓
Hypothesized
    ↓
Proposed
    ↓
Accepted
```

These are different epistemic states.

---

# 61. Epistemic State Transition

A common transition is:

```text
Observation
    ↓
Hypothesis
    ↓
Evidence
    ↓
Fact
```

But the system must never assume the transition automatically.

Evidence must actually support the conclusion.

---

# 62. Confidence

Relationships involving inference may include confidence.

Examples:

```text
Observation
    suggests
Hypothesis
    confidence = 0.72
```

Confidence must not be confused with truth.

---

# 63. Authority

Authority identifies who is permitted to establish or approve something.

Examples:

```text
Human
Project policy
Governance process
External authority
Automated verification
```

AI confidence does not itself establish authority.

---

# 64. Temporal Validity

Relationships may have:

```text
valid_from
valid_until
```

Example:

```text
Decision A
    valid_from = T1
    valid_until = T2

Decision B
    valid_from = T2
```

---

# 65. Current Truth

Continuum must distinguish:

```text
historically true
```

from:

```text
currently true
```

Example:

```text
"The project used Redis."
```

may be historically true while:

```text
"The project currently uses PostgreSQL."
```

is currently true.

Both can coexist.

---

# 66. Staleness

Knowledge and ContextItems may become stale.

A stale item should not automatically be deleted.

Instead:

```text
Knowledge
    status = stale
```

or:

```text
ContextItem
    freshness = expired
```

---

# 67. Contradiction

Two Knowledge items may conflict.

Example:

```text
Knowledge A:
    Authentication uses JWT.

Knowledge B:
    Authentication uses session cookies.
```

Continuum must preserve both until the contradiction is resolved.

---

# 68. Contradiction Relationship

```text
Knowledge A
    contradicts
Knowledge B
```

The contradiction itself is valuable knowledge.

---

# 69. Resolution

A contradiction may be resolved by:

```text
new evidence
human decision
artifact inspection
experiment
test
time
explicit invalidation
```

---

# 70. Supersession vs Contradiction

These are different.

```text
Contradiction:
    A and B cannot both be true.

Supersession:
    B replaces A as the current accepted understanding.
```

A superseded item may no longer be current without having been incorrect at the time.

---

# 71. Causal Confidence

Causal relationships should support:

```text
direct
strongly_inferred
weakly_inferred
unknown
```

This prevents Continuum from presenting guesses as causality.

---

# 72. Traceability Matrix

The following represents the intended traceability model:

| Source         | Relationship   | Target         |
| -------------- | -------------- | -------------- |
| Goal           | decomposes_to  | Goal           |
| Goal           | produces       | Requirement    |
| Requirement    | constrained_by | Constraint     |
| Requirement    | addressed_by   | Task           |
| Task           | depends_on     | Task           |
| Task           | planned_by     | Plan           |
| Task           | worked_on_in   | Session        |
| Session        | contains       | Action         |
| Action         | modifies       | Artifact       |
| Action         | causes         | Event          |
| Event          | produces       | Observation    |
| Observation    | supports       | Knowledge      |
| Knowledge      | informs        | ContextPackage |
| ContextPackage | provided_to    | Agent          |
| Agent          | performs       | Action         |

---

# 73. Core Graph

The essential operational graph is:

```text
Goal
 │
 ▼
Requirement
 │
 ▼
Task
 │
 ▼
Session
 │
 ▼
Action
 │
 ├──────────────► Artifact
 │
 ▼
Event
 │
 ▼
Observation
 │
 ▼
Evidence
 │
 ▼
Knowledge
 │
 ▼
ContextPackage
 │
 ▼
Agent
 │
 ▼
Action
```

This creates a closed continuity loop.

---

# 74. Continuity Loop

The fundamental loop is:

```text
CURRENT STATE
     ↓
CONTEXT
     ↓
ACTOR INTENT
     ↓
ACTION
     ↓
PROJECT CHANGE
     ↓
OBSERVATION
     ↓
KNOWLEDGE UPDATE
     ↓
CURRENT STATE
```

Continuum exists to preserve this loop across sessions and actors.

---

# 75. Relationship Metadata

Every persisted relationship should be capable of eventually carrying:

```text
relationship_id
subject_id
predicate
object_id

created_at
valid_from
valid_until

source
authority
confidence

status
metadata
```

Not every implementation must expose all fields immediately.

---

# 76. Relationship Status

Possible states:

```text
proposed
active
inactive
superseded
invalidated
disputed
```

---

# 77. Relationship Immutability

Historical relationships should generally be append-only.

Rather than:

```text
change relationship
```

prefer:

```text
create new relationship
invalidate old relationship
```

This preserves historical reconstruction.

---

# 78. Relationship Provenance

A relationship should eventually be traceable to its source.

Example:

```text
Artifact A
    implements
Requirement B

source:
    human statement
```

or:

```text
Artifact A
    implements
Requirement B

source:
    static analysis
```

---

# 79. Relationship Confidence

Some relationships are explicit.

Others are inferred.

Example:

```text
Task A
    depends_on
Task B
```

may be:

```text
explicit
```

because a human declared it.

Another may be:

```text
inferred
confidence = 0.81
```

based on dependency analysis.

---

# 80. Relationship Authority

Authority should be distinct from provenance.

Example:

```text
Source:
    Agent

Authority:
    Human
```

This means:

> The AI proposed the relationship, but the human accepted it.

---

# 81. Ownership Is Not Causality

Important distinction:

```text
Task belongs_to Project
```

does not mean:

```text
Project caused Task
```

Likewise:

```text
Artifact belongs_to Repository
```

does not mean:

```text
Repository caused Artifact
```

---

# 82. Association Is Not Dependency

A relationship such as:

```text
Task relates_to Artifact
```

does not necessarily imply:

```text
Task depends_on Artifact
```

Relationship semantics must remain precise.

---

# 83. Observation Is Not Knowledge

An observation:

```text
Test X failed.
```

does not automatically justify:

```text
Dependency Y is broken.
```

The latter is an interpretation.

Continuum must preserve the distinction.

---

# 84. Context Is Not Memory

Memory is:

```text
what happened
```

Context is:

```text
what an actor needs right now
```

Context is generated from memory, knowledge, state, artifacts, and current intent.

---

# 85. Context Is Not Truth

A ContextPackage is a projection.

It is not authoritative merely because it was sent to an AI.

The authoritative information remains in the underlying project state and domain graph.

---

# 86. AI Is Not Authority by Default

An AI Agent may:

```text
observe
infer
propose
execute
summarize
recommend
```

But it should not automatically:

```text
establish policy
accept decisions
declare facts
override human authority
```

unless explicitly authorized.

---

# 87. Human Override

Human decisions may override AI inference.

Example:

```text
Agent:
    Hypothesis A

Human:
    Reject Hypothesis A

Knowledge:
    Hypothesis A
        status = rejected
```

The original hypothesis remains historically visible.

---

# 88. Project State vs Knowledge

The project itself may contain direct evidence.

For example:

```text
package.json
```

may directly establish:

```text
dependency X exists.
```

A historical memory saying:

```text
dependency X exists
```

should not override current repository state if the dependency has since been removed.

---

# 89. Source Precedence

Future Continuum implementations should support source precedence.

A conceptual precedence hierarchy might be:

```text
Current verified artifact state
        ↓
Current verified execution state
        ↓
Recent direct observations
        ↓
Accepted decisions
        ↓
Established knowledge
        ↓
Historical memory
        ↓
Inference
        ↓
Hypothesis
```

This is a starting principle, not yet a finalized algorithm.

---

# 90. Relationship Query Examples

The model should eventually allow questions such as:

```text
What tasks implement requirement R?

What files were changed by task T?

Why was this architectural decision made?

What evidence supports decision D?

What attempts have already failed?

What knowledge was available to the AI before action A?

What changed after action A?

Which decisions constrain this task?

Which artifacts are affected by this requirement?

What remains uncertain?
```

---

# 91. Causal Query Examples

Continuum should eventually support:

```text
Why did this test begin failing?

What action caused this file change?

What decision led to this implementation?

What evidence caused this hypothesis to be rejected?

Which task produced this regression?
```

---

# 92. Temporal Query Examples

Continuum should eventually support:

```text
What did the project look like at T?

What did the AI know at T?

Which decisions were active at T?

What was the previous implementation?

What changed between T1 and T2?
```

---

# 93. Epistemic Query Examples

Continuum should eventually support:

```text
What do we know?

How do we know it?

How confident are we?

Who established it?

What evidence supports it?

What contradicts it?

What remains uncertain?
```

---

# 94. Continuity Query Examples

The most important queries are:

```text
What were we doing?

Why were we doing it?

Where did we leave off?

What did we learn?

What failed?

What remains unfinished?

What should happen next?

What does the next AI need to know?
```

---

# 95. Minimum Relationship Set

The initial implementation does not need every relationship.

The minimum useful relationship vocabulary is:

```text
belongs_to
contains
depends_on
implements
addresses
constrained_by
works_on
performed_by
uses
reads
modifies
causes
observes
supports
contradicts
derived_from
informs
proposed_by
accepted_by
supersedes
```

---

# 96. Relationship Expansion

Additional relationship types can be introduced later.

They should not be added casually.

Every new relationship should answer:

```text
Why is an existing relationship insufficient?
What semantic distinction does this add?
Can the relationship be expressed through composition?
Does it affect queryability?
Does it affect provenance?
```

---

# 97. Domain Integrity Principle

The domain model should favor semantic precision over convenience.

For example:

```text
"related_to"
```

should not become a universal escape hatch.

If a relationship has known semantics, those semantics should be explicitly represented.

---

# 98. Relationship Graph Invariants

The following invariants should hold:

```text
1. Every relationship has a subject.

2. Every relationship has an object.

3. Every relationship has a defined semantic type.

4. Relationships must not silently change meaning.

5. Historical relationships should remain reconstructable.

6. Inferred relationships should be distinguishable from explicit relationships.

7. Relationships involving knowledge should support provenance.

8. Relationships involving inference should support confidence.

9. Authority should be distinguishable from provenance.

10. Contradiction should not result in silent deletion.

11. Supersession should preserve historical state.

12. Relationship semantics should remain stable across storage implementations.
```

---

# 99. Relationship Model Objective

The relationship model exists to allow Continuum to reconstruct:

```text
WHAT
    happened

WHEN
    it happened

WHO
    did it

WHY
    it happened

HOW
    it happened

WHAT CHANGED
    afterward

WHAT WAS LEARNED
    from it

WHAT IS BELIEVED
    now

WHY IT IS BELIEVED
    evidence

WHAT IS UNCERTAIN
    still

WHAT SHOULD HAPPEN NEXT
    based upon current intent
```

---

# 100. Final Principle

Continuum should not merely remember isolated facts.

It should preserve the **relationships that make those facts meaningful**.

The fundamental unit of continuity is therefore not:

```text
memory
```

but:

```text
entity
+
relationship
+
time
+
provenance
+
epistemic status
```

The resulting system becomes capable of reconstructing not merely:

> "What was said?"

but:

> "What was happening, what did we know, why did we believe it, what did we do, what changed, what did we learn, and what should the next actor understand?"

That is the relational foundation of Continuum.
