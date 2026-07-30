# Continuum Domain Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

This document defines the canonical domain model for Continuum.

The domain model establishes the fundamental entities, relationships, identities, lifecycles, and boundaries that make up the Continuum system.

The model must provide a coherent foundation for:

* persistence
* retrieval
* context compilation
* runtime execution
* project synchronization
* AI-agent integration
* session continuity
* traceability
* future implementation

The goal is not to define database tables.

The goal is to define:

> **What exists in Continuum, what each thing means, and how those things relate.**

---

# 2. Domain Model Principle

Continuum exists to preserve continuity across:

```text
human
AI
project
session
time
tool
environment
```

The domain model therefore needs to represent both:

```text
things
```

and:

```text
changes
```

and:

```text
relationships
```

and:

```text
knowledge about things
```

---

# 3. The Core Ontology

At the highest level:

```text
Continuum
│
├── Project
│   ├── Repository
│   ├── Workspace
│   ├── Environment
│   └── Artifact
│
├── Work
│   ├── Goal
│   ├── Task
│   ├── Requirement
│   ├── Constraint
│   └── Plan
│
├── Activity
│   ├── Session
│   ├── Action
│   ├── Observation
│   ├── Event
│   └── Outcome
│
├── Knowledge
│   ├── Memory
│   ├── Fact
│   ├── Hypothesis
│   ├── Decision
│   ├── Pattern
│   ├── Question
│   └── Uncertainty
│
├── Context
│   ├── ContextRequest
│   ├── ContextPackage
│   └── ContextItem
│
└── Actors
    ├── Human
    ├── Agent
    ├── Model
    └── Tool
```

This is the initial canonical ontology.

---

# 4. Entity Categories

Continuum entities fall into several broad categories.

```text
Identity
State
Work
Evidence
Knowledge
Activity
Context
Actors
Artifacts
Relationships
```

These categories should remain conceptually distinct.

---

# 5. Identity

Identity answers:

> What thing are we talking about?

Examples:

```text
Project
Repository
Workspace
Artifact
Task
Session
Agent
```

Identity should remain stable even when state changes.

---

# 6. State

State answers:

> What is true about the thing at a particular point in time?

Examples:

```text
Task:
    active

Repository:
    branch = feature/auth

Session:
    status = active
```

State is temporal.

---

# 7. Work

Work represents intentional activity.

Examples:

```text
Goal
Task
Requirement
Constraint
Plan
Decision
```

---

# 8. Evidence

Evidence represents observable support.

Examples:

```text
Observation
Test result
Build result
Git commit
File state
Tool output
External documentation
```

---

# 9. Knowledge

Knowledge represents what Continuum believes it knows about the project.

Examples:

```text
Fact
Decision
Pattern
Hypothesis
Question
Uncertainty
```

Knowledge must retain epistemic status.

---

# 10. Activity

Activity represents things that happen.

Examples:

```text
Session
Action
Event
Observation
Outcome
```

---

# 11. Context

Context represents the information assembled for a particular actor and task.

Examples:

```text
ContextRequest
ContextPackage
ContextItem
```

---

# 12. Actors

Actors represent entities capable of initiating or performing activity.

```text
Human
Agent
Tool
External System
```

A model itself is not necessarily an actor.

A model is generally a capability used by an Agent.

---

# 13. Project

A Project is the primary continuity boundary.

It represents a software engineering endeavor that Continuum is tracking.

A Project may contain:

```text
repositories
workspaces
artifacts
tasks
requirements
decisions
knowledge
sessions
environments
```

---

# 14. Project Identity

A Project should have:

```text
project_id
name
description
created_at
updated_at
status
```

Potential status values:

```text
active
paused
archived
completed
abandoned
```

---

# 15. Project Boundary

The Project defines the scope within which Continuum normally operates.

Knowledge belonging to one project should not automatically appear in another.

Cross-project knowledge must be explicitly related.

---

# 16. Repository

A Repository represents a version-controlled source of project artifacts.

Examples:

```text
Git repository
Mercurial repository
other VCS repository
```

A project may contain multiple repositories.

---

# 17. Repository Identity

A Repository may contain:

```text
repository_id
provider
remote
path
default_branch
current_branch
current_revision
```

---

# 18. Workspace

A Workspace represents a local or remote working environment associated with a Project.

Examples:

```text
local checkout
developer machine
container
remote development environment
CI workspace
```

A Project may have many Workspaces.

---

# 19. Environment

An Environment represents the runtime context in which work occurs.

Examples:

```text
development
test
staging
production
CI
container
local machine
```

---

# 20. Artifact

An Artifact is a concrete project object.

Examples:

```text
source file
directory
module
package
configuration file
schema
test
document
API
database migration
Dockerfile
workflow
```

Artifacts are generally observable.

---

# 21. Artifact Identity

An Artifact should have a stable identity where possible.

Potential identifiers include:

```text
artifact_id
repository
path
symbol
revision
artifact_type
```

---

# 22. Artifact Version

Artifacts change over time.

Continuum should therefore distinguish:

```text
Artifact
```

from:

```text
ArtifactVersion
```

An Artifact represents the logical object.

An ArtifactVersion represents its state at a point in time.

---

# 23. Artifact Relationships

Artifacts may relate to one another:

```text
imports
depends_on
implements
tests
documents
generates
configures
extends
contains
references
```

These relationships form part of the project graph.

---

# 24. Goal

A Goal represents a desired future state.

Example:

```text
Goal:
    Provide reliable authentication for the application.
```

Goals may contain:

```text
description
priority
status
deadline
success criteria
```

---

# 25. Requirement

A Requirement represents something the project is expected to satisfy.

Examples:

```text
Users must authenticate with email and password.

The API must support OAuth.

Authentication must work offline.
```

Requirements may be:

```text
functional
nonfunctional
technical
business
security
operational
```

---

# 26. Constraint

A Constraint represents a boundary within which work must occur.

Examples:

```text
Must run on Linux.

Cannot use proprietary dependencies.

Must remain compatible with PostgreSQL.

Memory budget is limited.

Must preserve public API compatibility.
```

---

# 27. Requirement vs Constraint

A Requirement says:

> What must be achieved.

A Constraint says:

> What boundaries apply while achieving it.

---

# 28. Task

A Task represents a concrete unit of work.

Example:

```text
Task:
    Implement JWT authentication.
```

A Task may be related to:

```text
goal
requirements
constraints
artifacts
decisions
sessions
actions
outcomes
```

---

# 29. Task Lifecycle

Possible Task states:

```text
proposed
planned
ready
active
blocked
paused
completed
failed
cancelled
superseded
```

---

# 30. Task Hierarchy

Tasks may contain subtasks.

```text
Task
├── Subtask
├── Subtask
└── Subtask
```

This creates a work hierarchy.

---

# 31. Plan

A Plan describes an intended sequence or strategy for accomplishing work.

A Plan may contain:

```text
tasks
dependencies
milestones
expected outcomes
risks
```

Plans are not necessarily truth.

They represent intended future activity.

---

# 32. Decision

A Decision represents an accepted choice.

Example:

```text
Decision:
    PostgreSQL will remain the primary persistence layer.
```

A Decision should include:

```text
decision_id
statement
rationale
status
authority
created_at
```

---

# 33. Decision Status

Possible states:

```text
proposed
accepted
rejected
superseded
reconsidered
```

---

# 34. Decision Authority

Continuum should record who or what established the decision.

Possible authorities:

```text
human
team
project governance
automated verification
external standard
```

An AI proposal is not automatically an accepted Decision.

---

# 35. Session

A Session represents a bounded period of active interaction.

A Session may involve:

```text
human
agent
project
tasks
actions
observations
context
decisions
outcomes
```

---

# 36. Session Identity

A Session should have:

```text
session_id
project_id
started_at
ended_at
status
initiator
```

---

# 37. Session Status

Possible values:

```text
initializing
active
paused
completed
aborted
failed
```

---

# 38. Session Continuity

A Session is temporary.

Continuity must survive the Session.

Therefore:

> Session state must eventually become durable project memory and Knowledge where appropriate.

---

# 39. Action

An Action represents an intentional operation performed by an actor.

Examples:

```text
read_file
write_file
run_command
run_test
create_commit
query_knowledge
modify_configuration
```

---

# 40. Action Identity

An Action may contain:

```text
action_id
actor
session
task
tool
input
started_at
completed_at
result
status
```

---

# 41. Action Status

Possible states:

```text
requested
started
completed
failed
cancelled
timeout
```

---

# 42. Observation

An Observation represents something observed.

Examples:

```text
file changed
test failed
build succeeded
dependency was missing
branch changed
command returned exit code 1
```

---

# 43. Observation Structure

An Observation should preserve:

```text
observation_id
subject
observation_type
value
timestamp
source
provenance
confidence
```

---

# 44. Observation Is Evidence

An Observation is evidence about the project.

It is not automatically a conclusion.

---

# 45. Event

An Event represents something that occurred in the runtime timeline.

Examples:

```text
SessionStarted
TaskStarted
FileChanged
TestFailed
DecisionAccepted
CommitCreated
TaskCompleted
```

Events are particularly useful for reconstructing history.

---

# 46. Observation vs Event

An Event answers:

> What happened?

An Observation answers:

> What did we observe about something?

They may overlap but should remain conceptually distinct.

Example:

```text
Event:
    TestCompleted

Observation:
    3 tests failed with error X.
```

---

# 47. Outcome

An Outcome represents the result of an action, task, experiment, or attempt.

Examples:

```text
success
failure
partial success
inconclusive
blocked
```

An Outcome may be supported by one or more Observations.

---

# 48. Attempt

An Attempt represents an effort to accomplish something.

This is important because failed attempts contain valuable continuity information.

Example:

```text
Attempt 1:
    Tried Redis.
    Failed due to deployment complexity.

Attempt 2:
    Tried PostgreSQL.
    Succeeded.
```

Continuum should preserve both.

---

# 49. Memory

Memory is preserved experience.

It may contain:

```text
events
observations
conversations
attempts
actions
results
snapshots
summaries
```

Memory answers:

> What happened?

---

# 50. Knowledge

Knowledge represents interpreted understanding.

Knowledge answers:

> What do we believe is true or important?

---

# 51. Memory vs Knowledge

This distinction is foundational.

```text
MEMORY
"What happened?"

KNOWLEDGE
"What does it mean?"
```

Example:

```text
Memory:
    Test failed after changing configuration X.

Knowledge:
    Configuration X is likely incompatible with test environment Y.
```

---

# 52. Fact

A Fact represents a proposition considered sufficiently established.

Example:

```text
Fact:
    The project uses PostgreSQL.
```

Facts should have evidence or provenance.

---

# 53. Hypothesis

A Hypothesis represents a proposition that may be true but is not sufficiently established.

Example:

```text
Hypothesis:
    The connection pool may be causing intermittent failures.
```

---

# 54. Question

A Question represents unresolved information needed by the project.

Examples:

```text
Why does test X fail only in CI?

Should authentication support OAuth?

Which database should be used for local development?
```

Questions can drive future retrieval and work.

---

# 55. Uncertainty

Uncertainty represents incomplete confidence or unresolved ambiguity.

Examples:

```text
unknown
conflicting
ambiguous
low confidence
incomplete evidence
```

Uncertainty should be explicit.

---

# 56. Pattern

A Pattern represents a recurring relationship or behavior observed across the project.

Examples:

```text
All services use dependency injection.

Every API endpoint has an integration test.

Build failures frequently originate from stale generated code.
```

Patterns may be:

```text
architectural
implementation
behavioral
operational
organizational
```

---

# 57. Knowledge Lifecycle

Knowledge may move through:

```text
candidate
supported
accepted
superseded
disputed
rejected
```

Knowledge should not be silently deleted when it becomes obsolete.

---

# 58. Evidence

Knowledge should be traceable to Evidence.

Evidence may include:

```text
Observation
Artifact
Event
Test result
Commit
External source
Human statement
```

---

# 59. Evidence Relationship

Conceptually:

```text
Evidence
    │
    ├── supports → Knowledge
    ├── contradicts → Knowledge
    └── qualifies → Knowledge
```

---

# 60. Provenance

Provenance describes where information came from.

Potential sources:

```text
human
AI
repository
file
Git
test runner
CI
external documentation
tool
other system
```

---

# 61. Provenance Is First-Class

Provenance should not be an optional annotation added only for debugging.

It is fundamental to Continuum's trust model.

---

# 62. ContextRequest

A ContextRequest represents a request for context.

It may contain:

```text
task
actor
objective
scope
strategy
budget
constraints
```

---

# 63. ContextPackage

A ContextPackage is the compiled representation supplied to an AI or human actor.

It contains:

```text
identity
objective
state
requirements
constraints
decisions
knowledge
artifacts
history
uncertainty
instructions
provenance
```

---

# 64. ContextItem

A ContextItem is an individual piece of information included in a ContextPackage.

It should preserve:

```text
source
type
relevance
authority
freshness
provenance
```

---

# 65. Context Fingerprint

Every ContextPackage may have a fingerprint.

```text
context_fingerprint =
    hash(normalized context)
```

This allows context comparison and replay.

---

# 66. Actor

An Actor represents an entity capable of initiating or performing actions.

Possible Actor types:

```text
Human
Agent
Automation
ExternalSystem
```

---

# 67. Human

A Human represents a person participating in the project.

Humans may:

```text
create tasks
make decisions
approve proposals
perform actions
provide evidence
override AI recommendations
```

---

# 68. Agent

An Agent represents an AI-driven or automated actor operating on behalf of a human or project.

An Agent may have:

```text
agent_id
name
provider
model
capabilities
permissions
configuration
```

---

# 69. Model

A Model represents the underlying AI model used by an Agent.

Examples:

```text
GPT-class model
Claude-class model
local LLM
specialized coding model
```

The Model should not own project continuity.

The Agent uses the Model.

---

# 70. Tool

A Tool represents a capability an Agent may invoke.

Examples:

```text
filesystem
shell
Git
compiler
test runner
browser
database
GitHub
issue tracker
Continuum
```

---

# 71. Tool Invocation

A Tool Invocation is an Action performed through a Tool.

Conceptually:

```text
Agent
  ↓
Action
  ↓
Tool
  ↓
Execution
  ↓
Outcome
```

---

# 72. Capability

An Agent may have capabilities.

Examples:

```text
read_repository
write_repository
execute_shell
run_tests
query_continuum
modify_git
deploy
```

Capabilities should eventually be used for authorization.

---

# 73. Permission

Permissions define what an Actor is allowed to do.

Examples:

```text
read
write
execute
commit
deploy
delete
administer
```

Permissions are distinct from capabilities.

A capability says:

> What the agent can technically do.

A permission says:

> What the agent is authorized to do.

---

# 74. Relationship

Continuum should treat relationships as first-class conceptual objects.

Examples:

```text
Task
    depends_on
Task

Artifact
    implements
Requirement

Decision
    constrains
Task

Observation
    supports
Knowledge

Action
    modifies
Artifact

Session
    executes
Task
```

---

# 75. Relationship Types

Potential relationship categories:

```text
structural
temporal
causal
semantic
epistemic
dependency
ownership
traceability
```

---

# 76. Structural Relationships

Examples:

```text
Project contains Repository
Repository contains Artifact
Task contains Subtask
Workspace belongs_to Project
```

---

# 77. Temporal Relationships

Examples:

```text
Event precedes Event
Decision supersedes Decision
ArtifactVersion follows ArtifactVersion
Task follows Task
```

---

# 78. Causal Relationships

Examples:

```text
Test failure caused investigation
Decision caused implementation
Action caused file change
Requirement caused Task
```

Causal relationships are particularly valuable for debugging and traceability.

---

# 79. Semantic Relationships

Examples:

```text
Artifact implements Requirement
Document describes Architecture
Task relates_to Component
Pattern applies_to Artifact
```

---

# 80. Epistemic Relationships

Examples:

```text
Evidence supports Knowledge
Evidence contradicts Knowledge
Knowledge qualifies Knowledge
Question challenges Knowledge
Observation suggests Hypothesis
```

---

# 81. Traceability Relationships

Examples:

```text
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
Outcome
```

This forms a traceability chain.

---

# 82. The Core Traceability Graph

Continuum should eventually be able to represent:

```text
Goal
  ↓
Requirement
  ↓
Task
  ↓
Plan
  ↓
Decision
  ↓
Action
  ↓
Artifact
  ↓
Test
  ↓
Observation
  ↓
Outcome
  ↓
Knowledge
```

Not every workflow contains every node.

---

# 83. The Continuity Graph

At the highest level:

```text
                    PROJECT
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
       ARTIFACT       WORK       KNOWLEDGE
          │            │            │
          │            ▼            │
          │          TASK           │
          │            │            │
          └──────┐     ▼     ┌──────┘
                 │   ACTION  │
                 │     │     │
                 ▼     ▼     ▼
                    EVENT
                      │
                      ▼
                  OBSERVATION
                      │
                      ▼
                   MEMORY
                      │
                      ▼
                  KNOWLEDGE
                      │
                      ▼
                   CONTEXT
                      │
                      ▼
                     AI
                      │
                      ▼
                   ACTION
```

This graph represents the central continuity mechanism.

---

# 84. Entity Relationships

At minimum:

```text
Project
 ├── has_many → Repository
 ├── has_many → Workspace
 ├── has_many → Environment
 ├── has_many → Artifact
 ├── has_many → Goal
 ├── has_many → Requirement
 ├── has_many → Constraint
 ├── has_many → Task
 ├── has_many → Decision
 ├── has_many → Session
 ├── has_many → Knowledge
 └── has_many → Event
```

---

# 85. Task Relationships

A Task may:

```text
belong_to → Project
belong_to → Goal
satisfy → Requirement
constrained_by → Constraint
depend_on → Task
implemented_by → Artifact
executed_in → Session
produce → Outcome
lead_to → Decision
```

---

# 86. Session Relationships

A Session may:

```text
belong_to → Project
involve → Actor
work_on → Task
use → ContextPackage
contain → Action
contain → Observation
produce → Event
produce → Outcome
```

---

# 87. Action Relationships

An Action may:

```text
performed_by → Actor
part_of → Session
addresses → Task
uses → Tool
reads → Artifact
modifies → Artifact
produces → Outcome
causes → Event
```

---

# 88. Observation Relationships

An Observation may:

```text
occur_in → Session
observe → Artifact
observe → Action
observe → Environment
support → Knowledge
contradict → Knowledge
produce → Question
```

---

# 89. Knowledge Relationships

Knowledge may:

```text
belong_to → Project
supported_by → Evidence
contradicted_by → Evidence
derived_from → Knowledge
supersede → Knowledge
relate_to → Artifact
relate_to → Task
constrain → Task
inform → Context
```

---

# 90. Context Relationships

A ContextPackage may:

```text
requested_for → Task
compiled_from → Knowledge
compiled_from → Artifact
compiled_from → State
provided_to → Actor
used_in → Session
```

---

# 91. Actor Relationships

An Actor may:

```text
participate_in → Session
perform → Action
propose → Decision
provide → Evidence
receive → ContextPackage
```

---

# 92. Artifact Relationships

An Artifact may:

```text
belong_to → Repository
exist_in → Workspace
implement → Requirement
related_to → Task
modified_by → Action
observed_by → Observation
tested_by → Artifact
```

---

# 93. State Is Temporal

Any entity whose properties change over time should be understood as having state history.

Conceptually:

```text
Entity
   │
   ├── State @ T1
   ├── State @ T2
   ├── State @ T3
   └── Current State
```

---

# 94. Identity vs State

This distinction is crucial.

Example:

```text
Artifact:
    src/auth.ts
```

remains the same logical Artifact while:

```text
ArtifactVersion:
    v1
    v2
    v3
```

changes.

---

# 95. Event Sourcing Compatibility

The domain model should remain compatible with event-sourced implementations.

The canonical state of a project could potentially be derived from:

```text
Events
    ↓
State Projection
```

This is not yet a commitment to event sourcing.

---

# 96. Snapshot Compatibility

The model should also support snapshot-based persistence.

```text
Events
    ↓
Snapshot
    ↓
New Events
```

Both approaches should remain possible.

---

# 97. Temporal Queries

The domain should eventually support questions such as:

```text
What did we believe last Tuesday?

What was the repository state when decision D was made?

What did the AI know before changing this file?

What attempts had already failed before this solution?
```

---

# 98. Historical Truth

Continuum should preserve historical truth.

If:

```text
Fact A
```

later becomes:

```text
Fact B
```

Continuum should not erase A.

Instead:

```text
A
 ↓
superseded_by
 ↓
B
```

This preserves the project's history of understanding.

---

# 99. Negative Knowledge

Continuum should explicitly preserve failed approaches.

Example:

```text
Attempt:
    Use library X.

Outcome:
    Failed because library X does not support environment Y.

Knowledge:
    Do not use library X for this environment.
```

This prevents repeated mistakes.

---

# 100. Unknowns

Unknowns are first-class.

Examples:

```text
Unknown:
    Why does CI fail?

Unknown:
    Whether feature X is required.

Unknown:
    Which dependency introduced the regression.
```

An unknown may generate:

```text
Question
Task
Investigation
Retrieval
Experiment
```

---

# 101. Questions as Work Generators

A Question may lead to:

```text
Question
   ↓
Investigation Task
   ↓
Action
   ↓
Observation
   ↓
Knowledge
```

This allows uncertainty to drive productive work.

---

# 102. Experiments

Continuum should eventually support explicit Experiments.

An Experiment represents a controlled attempt to learn something.

Conceptually:

```text
Hypothesis
    ↓
Experiment
    ↓
Action
    ↓
Observation
    ↓
Outcome
    ↓
Knowledge Update
```

Experiment is not yet a mandatory core entity but should remain compatible with the model.

---

# 103. Proposal

A Proposal represents a suggested change or decision that has not yet been accepted.

Examples:

```text
Architecture proposal
Implementation proposal
Decision proposal
Refactoring proposal
```

Proposal should remain distinct from Decision.

---

# 104. Decision vs Proposal

```text
Proposal:
    "We should migrate to PostgreSQL."

Decision:
    "We will migrate to PostgreSQL."
```

---

# 105. Intent

Intent represents what an actor is trying to accomplish.

Intent may come from:

```text
human
AI
task
plan
system
```

Intent is useful because actions do not always reveal their purpose.

---

# 106. Intent and Action

Conceptually:

```text
Intent
  ↓
Decision
  ↓
Action
```

This gives Continuum a richer causal model.

---

# 107. Outcome and Learning

Outcomes should feed back into Knowledge.

```text
Action
  ↓
Outcome
  ↓
Observation
  ↓
Knowledge
```

This is how experience accumulates.

---

# 108. The Learning Loop

The domain model therefore supports:

```text
Intent
 ↓
Action
 ↓
Outcome
 ↓
Observation
 ↓
Interpretation
 ↓
Knowledge
 ↓
Future Context
 ↓
Future Action
```

---

# 109. The Agent Continuity Loop

For AI-assisted engineering:

```text
Task
 ↓
Context
 ↓
Agent
 ↓
Decision
 ↓
Action
 ↓
Outcome
 ↓
Observation
 ↓
Knowledge
 ↓
Next Context
```

This is the central operational loop.

---

# 110. Canonical Entity Set

The initial canonical entity set is:

```text
Project
Repository
Workspace
Environment
Artifact
ArtifactVersion

Goal
Requirement
Constraint
Task
Plan
Proposal
Decision

Session
Intent
Action
Outcome
Event
Observation
Attempt

Memory
Knowledge
Fact
Hypothesis
Question
Uncertainty
Pattern
Evidence
Provenance

ContextRequest
ContextPackage
ContextItem

Actor
Human
Agent
Model
Tool

Relationship
```

---

# 111. Entity Priority

Not every entity must be implemented immediately.

Priority:

```text
Tier 1 — Core
    Project
    Task
    Session
    Event
    Observation
    Memory
    Knowledge
    Context
    Actor
    Action

Tier 2 — Essential
    Artifact
    Repository
    Requirement
    Constraint
    Decision
    Outcome
    Evidence
    Provenance

Tier 3 — Advanced
    Goal
    Plan
    Proposal
    Attempt
    Pattern
    Question
    Uncertainty
    Environment
    Workspace

Tier 4 — Specialized
    Experiment
    ArtifactVersion
    Model
    Tool
    Relationship
```

This is an implementation priority, not an indication that Tier 4 entities are conceptually unimportant.

---

# 112. Entity Identity Principle

Every durable entity should have a stable unique identifier.

Identifiers should not depend solely on:

```text
file path
display name
human-readable title
timestamp
```

---

# 113. Human-Readable Identity

Stable IDs should coexist with human-readable names.

Example:

```text
task_id:
    task_01J...

name:
    Implement authentication
```

---

# 114. Relationship Identity

Important relationships may also require stable identity.

This becomes useful for:

```text
relationship history
provenance
confidence
temporal validity
```

---

# 115. Relationship Attributes

Relationships may have:

```text
confidence
authority
created_at
valid_from
valid_until
source
status
```

This enables richer knowledge graphs.

---

# 116. Entity Lifecycle

Durable entities should generally follow:

```text
created
active
modified
superseded
archived
```

Deletion should be carefully controlled.

---

# 117. Soft Deletion

Continuum should generally prefer:

```text
superseded
archived
invalidated
```

over destructive deletion.

Historical continuity is more important than storage simplicity.

---

# 118. Domain Invariants

The following invariants should hold:

```text
1. Every Task belongs to a Project.

2. Every Session belongs to a Project.

3. Every Action belongs to a Session.

4. Every Observation has provenance.

5. Every accepted Decision has an authority.

6. Every Knowledge item has epistemic status.

7. Historical Knowledge is not silently deleted.

8. AI proposals are not automatically accepted Decisions.

9. Current project state takes precedence over stale project memory for current implementation facts.

10. Context is derived from project state and Knowledge rather than becoming the source of truth.

11. An Outcome may invalidate a Hypothesis.

12. Evidence may support or contradict Knowledge.

13. Continuity must survive individual Sessions.

14. Continuity must survive changes in AI model or agent.

15. Unknown information must not be represented as established fact.
```

---

# 119. The Domain Model as a Graph

Continuum should ultimately be understood as a graph rather than merely a collection of records.

The graph contains:

```text
nodes
+
typed relationships
+
temporal validity
+
provenance
+
epistemic state
```

---

# 120. Domain Model Objective

The model must enable Continuum to answer:

```text
What is this project?

What are we trying to accomplish?

What is currently happening?

What has already happened?

What have we learned?

What decisions have been made?

Why were they made?

What has failed?

What remains unknown?

What is the AI currently working on?

What does the AI need to know?

What did the AI do?

What changed because of it?

What happened afterward?

What should happen next?
```

---

# 121. Final Domain Principle

The domain model exists to preserve the chain:

```text
INTENT
  ↓
WORK
  ↓
ACTION
  ↓
CHANGE
  ↓
OBSERVATION
  ↓
EVIDENCE
  ↓
KNOWLEDGE
  ↓
CONTEXT
  ↓
INTENT
```

That cycle is the fundamental structure of continuity.

---

# 122. Architectural Conclusion

The Continuum domain is therefore not fundamentally:

```text
documents
vectors
prompts
chat histories
```

It is a temporal, evidence-backed graph of:

```text
work
actions
changes
observations
knowledge
decisions
context
actors
```

The purpose of the graph is to allow an AI—or human—to re-enter an evolving software project with enough understanding to continue meaningful work without requiring the entire history of interaction to be present in the active conversation.

The canonical conceptual relationship is:

```text
PROJECT
  │
  ├── WORK
  │    ├── Goals
  │    ├── Requirements
  │    ├── Constraints
  │    ├── Tasks
  │    └── Plans
  │
  ├── ARTIFACTS
  │
  ├── ACTIVITY
  │    ├── Sessions
  │    ├── Actions
  │    ├── Events
  │    ├── Observations
  │    └── Outcomes
  │
  ├── KNOWLEDGE
  │    ├── Facts
  │    ├── Decisions
  │    ├── Hypotheses
  │    ├── Questions
  │    ├── Patterns
  │    └── Uncertainty
  │
  └── CONTEXT
       ├── Requests
       ├── Packages
       └── Items
```

This model is the conceptual backbone of Continuum.

The next step is to formalize it.
