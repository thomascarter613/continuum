# Continuum Event Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

This document defines the Event Model for Continuum.

Events provide the historical record of things that happened within or around a software-engineering project.

The Event Model exists to preserve:

* what happened
* when it happened
* where it happened
* who or what caused it
* what entity it affected
* what state changed
* what evidence resulted
* what knowledge was produced
* what consequences followed

The Event Model is therefore the temporal backbone of Continuum.

---

# 2. Core Model

Continuum distinguishes four fundamental concepts:

```text
Entity
    ↓
State
    ↓
Event
    ↓
Transition
```

An entity exists in a state.

Something happens.

That event may cause a state transition.

Example:

```text
Task
    state = ready

        │
        │ TaskStarted
        ▼

Task
    state = in_progress
```

---

# 3. Event Definition

An Event is:

> A recorded occurrence that is meaningful to the project, its actors, its artifacts, or its knowledge.

Examples:

```text
TaskCreated
TaskStarted
TaskCompleted

FileCreated
FileModified
FileDeleted

CommandExecuted
TestExecuted
TestFailed

DecisionAccepted
RequirementChanged

SessionStarted
SessionEnded

ObservationRecorded
KnowledgeUpdated
```

---

# 4. Event vs Action

An Action is something an actor does.

An Event is something that happened as a result of an action or independently.

Example:

```text
Agent
    performs
Action: edit_file

Action
    causes
Event: FileModified
```

Therefore:

```text
Action ≠ Event
```

---

# 5. Event vs State

State describes:

> What is true.

Event describes:

> What happened.

Example:

```text
State:
Task = completed

Event:
TaskCompleted
```

The Event explains the transition into the State.

---

# 6. Event vs Observation

An Event records an occurrence.

An Observation records an actor's observation of something.

Example:

```text
Event:
TestExecuted

Observation:
Test returned exit code 1.
```

An Event may produce an Observation.

---

# 7. Event Structure

A canonical Event should conceptually contain:

```text
Event
├── event_id
├── event_type
├── timestamp
├── actor
├── session
├── subject
├── target
├── trigger
├── payload
├── provenance
├── causation
├── correlation
└── metadata
```

Not every field must be mandatory in the first implementation.

---

# 8. Event Identity

Every persisted Event must have a globally unique identifier.

Recommended conceptual field:

```text
event_id
```

Event IDs must remain stable.

They must not be reused.

---

# 9. Event Type

Every Event must have an explicit type.

Examples:

```text
project.created
project.updated

task.created
task.started
task.blocked
task.completed

artifact.created
artifact.modified
artifact.deleted

action.requested
action.started
action.succeeded
action.failed

session.started
session.ended

observation.recorded
knowledge.created
knowledge.updated
```

Event types should be semantically precise.

---

# 10. Event Timestamp

Every Event must have a timestamp.

Conceptually:

```text
occurred_at
```

This represents when the event occurred.

Additional timestamps may eventually include:

```text
recorded_at
processed_at
observed_at
```

These should not be conflated.

---

# 11. Occurred At vs Recorded At

An Event may happen at:

```text
10:00:00
```

but be recorded at:

```text
10:00:04
```

Therefore:

```text
occurred_at ≠ recorded_at
```

This distinction becomes important for distributed systems and imported history.

---

# 12. Actor

An Event should identify its originating actor when known.

Possible actors:

```text
Human
Agent
Tool
System
ExternalSystem
Unknown
```

Example:

```text
actor:
    Agent: Claude

event:
    FileModified
```

---

# 13. Session

An Event may belong to a Session.

Example:

```text
Session-123
    ├── Event-1
    ├── Event-2
    ├── Event-3
    └── Event-4
```

However, events may also exist outside interactive Sessions.

---

# 14. Subject

The Subject identifies the entity primarily affected by the Event.

Example:

```text
TaskCompleted
    subject = Task-123
```

---

# 15. Target

Some Events involve an additional target.

Example:

```text
AgentModifiedArtifact
    subject = Agent-1
    target = Artifact-123
```

Subject and target should not be collapsed when the distinction is meaningful.

---

# 16. Trigger

A Trigger identifies what caused the Event.

Possible triggers:

```text
human_action
agent_action
tool_execution
external_event
scheduled_event
state_transition
system_process
unknown
```

---

# 17. Payload

The Event payload contains event-specific data.

Example:

```text
TestExecuted

payload:
    command
    exit_code
    duration
    output_reference
```

The payload should contain information specific to the Event type rather than arbitrary unrelated state.

---

# 18. Provenance

Events should preserve their provenance whenever possible.

Questions:

```text
Where did this Event originate?

Which tool produced it?

Which system reported it?

Was it directly observed?

Was it inferred?
```

---

# 19. Causation

Events may have causal relationships.

Example:

```text
Action-1
    causes
Event-1
```

and:

```text
Event-1
    causes
Event-2
```

This produces a causal chain.

---

# 20. Causation ID

An Event may include:

```text
causation_id
```

which identifies the immediately preceding event or action responsible for it.

Example:

```text
Action A
    ↓
Event B
    causation_id = Action A
```

---

# 21. Correlation

Multiple Events may belong to a larger operation.

Example:

```text
Correlation:
    build-2026-07-30-001
```

Events:

```text
BuildRequested
DependencyResolved
CompilationStarted
CompilationCompleted
TestsStarted
TestsFailed
BuildFailed
```

Correlation allows the entire operation to be reconstructed.

---

# 22. Causation vs Correlation

These are different.

Causation:

```text
A caused B
```

Correlation:

```text
A and B belong to the same operation
```

Events may share a correlation identifier without directly causing one another.

---

# 23. Event Ordering

Events require ordering.

The minimum ordering mechanism is:

```text
occurred_at
```

However, timestamps alone may be insufficient.

Future implementations should support:

```text
sequence_number
logical_clock
causation_id
parent_event_id
```

---

# 24. Sequence Number

Within an event stream, Events may have a monotonically increasing sequence.

Example:

```text
1 TaskCreated
2 TaskStarted
3 ActionRequested
4 ActionStarted
5 FileModified
6 TestExecuted
7 TaskCompleted
```

This provides deterministic local ordering.

---

# 25. Logical Clock

Distributed systems may require logical clocks to establish ordering when wall-clock timestamps are insufficient.

Continuum should remain compatible with:

```text
Lamport clocks
Vector clocks
Hybrid logical clocks
```

The initial implementation does not need to choose one.

---

# 26. Event Stream

Events may be grouped into streams.

Example:

```text
Project Stream
    ├── Event
    ├── Event
    └── Event

Task Stream
    ├── Event
    ├── Event
    └── Event
```

A project-wide stream and entity-specific streams may coexist.

---

# 27. Project Event Stream

A Project Event Stream represents the chronological history of a project.

Example:

```text
Project
 │
 ├── ProjectCreated
 ├── RequirementCreated
 ├── TaskCreated
 ├── SessionStarted
 ├── ActionStarted
 ├── FileModified
 ├── TestExecuted
 ├── TestFailed
 ├── DecisionAccepted
 └── SessionEnded
```

---

# 28. Entity Event Stream

An entity can have its own history.

Example:

```text
Task-123
 │
 ├── TaskCreated
 ├── TaskStarted
 ├── TaskBlocked
 ├── TaskResumed
 └── TaskCompleted
```

---

# 29. Event Categories

Continuum events are grouped into categories.

```text
Lifecycle
Execution
Artifact
Repository
Requirement
Task
Decision
Knowledge
Session
Context
Agent
Human
System
Integration
Observation
Governance
```

---

# 30. Lifecycle Events

Examples:

```text
created
activated
paused
resumed
completed
cancelled
archived
superseded
reopened
```

---

# 31. Project Events

Examples:

```text
project.created
project.updated
project.archived
project.restored
```

---

# 32. Repository Events

Examples:

```text
repository.connected
repository.disconnected
repository.synced
repository.changed
repository.branch_created
repository.branch_deleted
```

---

# 33. Artifact Events

Examples:

```text
artifact.created
artifact.read
artifact.modified
artifact.deleted
artifact.renamed
artifact.moved
artifact.version_created
artifact.verified
```

---

# 34. Task Events

Examples:

```text
task.created
task.updated
task.started
task.blocked
task.unblocked
task.completed
task.cancelled
task.reopened
```

---

# 35. Requirement Events

Examples:

```text
requirement.proposed
requirement.accepted
requirement.rejected
requirement.activated
requirement.satisfied
requirement.superseded
```

---

# 36. Goal Events

Examples:

```text
goal.created
goal.activated
goal.achieved
goal.abandoned
goal.superseded
```

---

# 37. Decision Events

Examples:

```text
decision.proposed
decision.review_started
decision.accepted
decision.rejected
decision.revoked
decision.superseded
```

---

# 38. Proposal Events

Examples:

```text
proposal.created
proposal.submitted
proposal.review_started
proposal.accepted
proposal.rejected
proposal.withdrawn
```

---

# 39. Session Events

Examples:

```text
session.created
session.started
session.paused
session.resumed
session.ended
session.cancelled
```

---

# 40. Action Events

Examples:

```text
action.requested
action.authorized
action.denied
action.started
action.succeeded
action.failed
action.cancelled
action.timed_out
```

---

# 41. Tool Events

Examples:

```text
tool.invoked
tool.completed
tool.failed
tool.timed_out
```

---

# 42. Agent Events

Examples:

```text
agent.connected
agent.started
agent.received_context
agent.generated_proposal
agent.requested_action
agent.completed
agent.disconnected
```

---

# 43. Context Events

Examples:

```text
context.requested
context.assembled
context.validated
context.delivered
context.consumed
context.expired
context.failed
```

---

# 44. Observation Events

Examples:

```text
observation.recorded
observation.updated
observation.invalidated
```

The original observation should generally remain historically recoverable.

---

# 45. Knowledge Events

Examples:

```text
knowledge.created
knowledge.supported
knowledge.challenged
knowledge.verified
knowledge.retracted
knowledge.superseded
knowledge.staled
```

---

# 46. Evidence Events

Examples:

```text
evidence.collected
evidence.recorded
evidence.evaluated
evidence.accepted
evidence.rejected
evidence.challenged
```

---

# 47. Pattern Events

Examples:

```text
pattern.detected
pattern.proposed
pattern.validated
pattern.rejected
pattern.superseded
```

---

# 48. Integration Events

Continuum may receive events from external systems.

Examples:

```text
github.pull_request_opened
github.commit_created
github.check_failed
github.issue_updated

ci.build_started
ci.build_completed

editor.file_saved
terminal.command_executed
```

External events should retain their external identity.

---

# 49. External Event Identity

An imported event may have:

```text
external_event_id
external_system
external_timestamp
```

Example:

```text
external_system = github
external_event_id = 123456
```

This prevents duplication when synchronization occurs.

---

# 50. Event Ingestion

External events enter Continuum through an ingestion boundary:

```text
External System
      ↓
Event Adapter
      ↓
Normalization
      ↓
Validation
      ↓
Continuum Event
      ↓
Event Store
```

---

# 51. Event Normalization

Different systems may describe the same semantic event differently.

For example:

```text
GitHub:
    pull_request.closed

GitLab:
    merge_request.closed
```

Continuum may normalize both into a common semantic category where appropriate.

---

# 52. Event Fidelity

Normalization must not destroy source-specific information.

The normalized event should preserve:

```text
canonical event type
+
source event type
+
source payload
+
source identity
```

---

# 53. Event Immutability

Once an Event is persisted, its historical meaning should not be silently changed.

Preferred model:

```text
append
```

rather than:

```text
overwrite
```

Corrections should themselves be represented as Events.

---

# 54. Event Correction

If an Event was recorded incorrectly:

```text
Event A
    ↓
EventCorrection
    ↓
Corrected interpretation
```

The original record remains available.

---

# 55. Event Retraction

An Event may be retracted if it is determined to be invalid.

Example:

```text
Event:
    TestPassed

Later:

Event:
    TestResultRetracted
```

The original event is not erased.

---

# 56. Event Validation

Events should be validated before persistence.

Validation may include:

```text
event type valid
timestamp valid
actor valid
subject exists
payload valid
provenance valid
schema version supported
```

---

# 57. Event Schema Version

Events should include a schema version.

Example:

```text
schema_version = 1
```

This allows future evolution without destroying historical interpretability.

---

# 58. Event Type Versioning

Event semantics may evolve.

Example:

```text
task.completed.v1
task.completed.v2
```

However, versioning should not be used casually.

Prefer stable semantic event types with versioned payload schemas where possible.

---

# 59. Event Processing

An Event may trigger downstream processing.

Example:

```text
FileModified
    ↓
RepositoryStateChanged
    ↓
AnalysisRequested
    ↓
ObservationRecorded
    ↓
KnowledgeUpdated
```

This creates an event-driven continuity loop.

---

# 60. Event Consumers

Potential consumers include:

```text
State Projection
Knowledge Engine
Context Builder
Audit System
Search Index
Analytics
Notification System
Memory System
Agent Runtime
```

---

# 61. Event Processing Must Be Idempotent

Processing the same Event twice should not produce unintended duplicate state changes.

Therefore event consumers should support idempotency.

Conceptually:

```text
process(event_id)
```

must recognize:

```text
already_processed(event_id)
```

---

# 62. Event Delivery

Event delivery may eventually be:

```text
at-most-once
at-least-once
exactly-once
```

The initial system should not assume exactly-once delivery.

Idempotent processing is therefore essential.

---

# 63. Event Ordering Guarantees

Continuum should distinguish:

```text
global ordering
```

from:

```text
stream ordering
```

A project may not require a single total ordering across all events.

It may instead require deterministic ordering within relevant streams.

---

# 64. Event Causality Graph

Events may form:

```text
Event A
  │
  ▼
Event B
  │
  ├──► Event C
  │
  └──► Event D
```

This provides a causal graph rather than merely a chronological list.

---

# 65. Event Timeline

A timeline is a projection of the event graph.

Example:

```text
09:00  Task created
09:05  Task started
09:07  Agent invoked
09:08  File modified
09:09  Tests executed
09:10  Tests failed
09:12  Agent diagnosed failure
09:15  File modified
09:17  Tests passed
09:20  Task completed
```

The timeline is derived from Events.

---

# 66. Event Graph vs Timeline

Timeline:

```text
A → B → C → D
```

Causal graph:

```text
       B
      / \
     C   D
    /
   E
```

Continuum should eventually support both.

---

# 67. Event-to-State Projection

Example:

```text
TaskCreated
    ↓
Task = draft

TaskReady
    ↓
Task = ready

TaskStarted
    ↓
Task = in_progress

TaskCompleted
    ↓
Task = completed
```

The current Task state can therefore be reconstructed from its Event history.

---

# 68. Event-to-Knowledge Projection

Example:

```text
TestFailed
    ↓
Observation:
    test X failed

AnalysisCompleted
    ↓
Hypothesis:
    dependency Y is incompatible

TestPassed
    ↓
Knowledge:
    dependency Y was the cause
```

The knowledge graph grows from the event history.

---

# 69. Event-to-Context Projection

When a new Session begins:

```text
Recent Events
      +
Current State
      +
Knowledge
      +
Decisions
      ↓
Context Builder
      ↓
ContextPackage
```

The Event Model therefore directly contributes to continuity.

---

# 70. Event Retention

Not every event necessarily requires permanent retention at the same fidelity.

Future policies may classify events as:

```text
critical
important
normal
ephemeral
```

However, retention policy must not undermine the ability to reconstruct important project history.

---

# 71. Critical Events

Critical events should generally include:

```text
decisions
requirements
major artifact changes
state transitions
security events
governance events
human approvals
knowledge corrections
```

---

# 72. Ephemeral Events

Some events may be operationally useful but not permanently important.

Examples:

```text
UI hover
cursor movement
temporary status polling
heartbeat
```

These should not necessarily become durable domain events.

---

# 73. Domain Event vs Telemetry

Continuum must distinguish:

```text
Domain Event
```

from:

```text
Telemetry
```

A domain event means something semantically meaningful happened.

Telemetry describes system behavior.

Examples:

```text
Domain:
TaskCompleted

Telemetry:
CPU = 82%
```

Both may be useful, but they serve different purposes.

---

# 74. Event Classification

Every Event should eventually be classifiable as:

```text
domain
integration
system
telemetry
audit
```

---

# 75. Audit Events

Audit events record actions that matter for accountability.

Examples:

```text
decision.accepted
permission.changed
artifact.deleted
policy.overridden
human.approval.recorded
```

Audit events require especially strong provenance.

---

# 76. Event Security

Events may contain sensitive project information.

Therefore the Event Model must eventually support:

```text
access scope
visibility
classification
redaction
retention
encryption
```

These are implementation concerns to be addressed later.

---

# 77. Event Privacy

An Event should contain only the information necessary for its purpose.

For example:

```text
password
secret
API token
private key
```

should never be stored merely because it appeared in an event payload.

---

# 78. Event Payload References

Large payloads should generally be referenced rather than embedded.

Example:

```text
TestExecuted
    output_reference → Artifact-Output-123
```

rather than embedding megabytes of output directly into the Event.

---

# 79. Event and Artifact Relationship

Artifact changes should be traceable to Events.

Example:

```text
Action
    ↓
FileModified Event
    ↓
ArtifactVersion
```

This enables:

```text
Why did this file change?
```

to be answered.

---

# 80. Event and Decision Relationship

A Decision may produce many downstream Events.

Example:

```text
DecisionAccepted
    ↓
TaskCreated
    ↓
ArtifactModified
    ↓
TestExecuted
```

This enables causal tracing from architectural intent to implementation.

---

# 81. Event and Requirement Relationship

Requirements may generate Tasks.

```text
RequirementAccepted
    ↓
TaskCreated
```

Later:

```text
TaskCompleted
    ↓
RequirementSatisfied
```

This produces end-to-end traceability.

---

# 82. Event and Knowledge Relationship

Events may change knowledge.

```text
ObservationRecorded
    ↓
KnowledgeCreated
```

or:

```text
EvidenceCollected
    ↓
KnowledgeChallenged
```

or:

```text
VerificationCompleted
    ↓
KnowledgeVerified
```

---

# 83. Event and Context Relationship

A ContextPackage may record which Events contributed to it.

Example:

```text
ContextPackage
    derived_from:
        Event A
        Event B
        Event C
```

This enables context provenance.

---

# 84. Context Reproducibility

Given:

```text
ContextRequest
+
project state at time T
+
selection policy
```

Continuum should eventually be capable of reconstructing approximately the same ContextPackage.

This is essential for debugging AI behavior.

---

# 85. AI Decision Reconstruction

Suppose an AI made an incorrect change.

Continuum should eventually allow:

```text
What did the AI know?
        ↓
What context did it receive?
        ↓
What did it infer?
        ↓
What did it decide?
        ↓
What action did it perform?
        ↓
What changed?
        ↓
What happened afterward?
```

The Event Model is what makes this possible.

---

# 86. Event Chain Example

A complete software-engineering episode might look like:

```text
RequirementAccepted
        ↓
TaskCreated
        ↓
SessionStarted
        ↓
ContextRequested
        ↓
ContextDelivered
        ↓
AgentStarted
        ↓
ActionRequested
        ↓
ActionAuthorized
        ↓
ActionStarted
        ↓
FileModified
        ↓
TestExecuted
        ↓
TestFailed
        ↓
ObservationRecorded
        ↓
HypothesisCreated
        ↓
FileModified
        ↓
TestExecuted
        ↓
TestPassed
        ↓
KnowledgeVerified
        ↓
TaskCompleted
        ↓
SessionEnded
```

This is the kind of history Continuum is designed to preserve.

---

# 87. Event Graph as Project Memory

The Event Graph becomes one of Continuum's deepest forms of memory.

Rather than storing only:

```text
"We fixed the authentication bug."
```

Continuum can preserve:

```text
Requirement
    ↓
Task
    ↓
Session
    ↓
Actions
    ↓
Artifact Changes
    ↓
Tests
    ↓
Observations
    ↓
Hypothesis
    ↓
Evidence
    ↓
Decision
    ↓
Outcome
```

---

# 88. Event Model Invariants

The following invariants should hold:

```text
1. Every persisted Event has a stable identity.

2. Every Event has a semantic type.

3. Every Event has a temporal position.

4. Events are historical records and should be treated as immutable.

5. Corrections should themselves be represented as Events.

6. Event causation must be distinguishable from correlation.

7. Event provenance must be preserved when possible.

8. Event ordering must not rely solely on wall-clock time.

9. Event consumers should be idempotent.

10. Domain Events must remain distinguishable from telemetry.

11. External Events must preserve source identity.

12. Event payloads must not become an uncontrolled dumping ground.

13. Events must support reconstruction of important state transitions.

14. Events must support project continuity across Sessions.
```

---

# 89. Minimum Event Vocabulary

The first implementation does not need hundreds of Event types.

A useful minimum vocabulary is:

```text
project.created

session.started
session.ended

task.created
task.started
task.blocked
task.completed
task.cancelled

action.requested
action.started
action.succeeded
action.failed

artifact.created
artifact.modified
artifact.deleted

observation.recorded

evidence.recorded

knowledge.created
knowledge.updated

decision.proposed
decision.accepted
decision.rejected

context.requested
context.delivered
```

This vocabulary can expand later.

---

# 90. Event Naming

Event names should describe something that happened.

Prefer:

```text
task.completed
```

over:

```text
task.complete
```

Prefer:

```text
artifact.modified
```

over:

```text
artifact.modify
```

The convention should be consistent throughout the system.

---

# 91. Event Semantics

Event names must be unambiguous.

For example:

```text
task.updated
```

may be too vague for important historical reasoning.

Prefer specific Events where the distinction matters:

```text
task.started
task.blocked
task.completed
task.cancelled
```

Generic update events may still exist for low-level synchronization.

---

# 92. Event Granularity

Events should be granular enough to reconstruct meaningful history but not so granular that the event stream becomes noise.

The correct unit is:

> A meaningful occurrence that could matter to future reasoning.

---

# 93. Event Compression

Future implementations may compress or summarize low-value Events.

However:

```text
compressed history
```

must remain distinguishable from:

```text
original history
```

where auditability matters.

---

# 94. Event Store Independence

The Event Model does not prescribe a storage engine.

It must work with:

```text
PostgreSQL
SQLite
EventStoreDB
Kafka
document databases
graph databases
append-only files
```

The domain semantics must remain independent of infrastructure.

---

# 95. Event Model Objective

The Event Model exists to allow Continuum to answer:

```text
What happened?

When did it happen?

Who did it?

What caused it?

What did it affect?

What changed?

What evidence resulted?

What did we learn?

What decision followed?

What happened afterward?
```

---

# 96. Final Principle

The Event Model is the historical spine of Continuum.

The fundamental chain is:

```text
INTENT
   ↓
ACTION
   ↓
EVENT
   ↓
STATE CHANGE
   ↓
OBSERVATION
   ↓
KNOWLEDGE
   ↓
DECISION
   ↓
NEXT ACTION
```

Continuum exists to preserve that chain across time.

The goal is not to remember conversations.

The goal is to remember **the evolution of the project itself**.

A future AI should therefore be able to enter a project and reconstruct:

```text
where we are
+
how we got here
+
why we are here
+
what we learned
+
what remains uncertain
+
what should happen next
```

That is the purpose of the Event Model.
