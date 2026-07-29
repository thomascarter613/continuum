# Continuum Event & Activity Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

## 1. Purpose

The Event & Activity Model defines how Continuum represents things that happen within and around a software-engineering project.

Continuum must preserve not only what exists, but how the project changes over time.

The model provides the temporal activity layer connecting:

```text
Objects
    ↓
Events
    ↓
State
    ↓
Knowledge
    ↓
Context
    ↓
AI Activity
    ↓
New Events
```

---

# 2. Fundamental Principle

Continuum distinguishes between:

```text
Object
    = something that exists or can be identified

Event
    = something that happens

State
    = a derived description of what is true at a point in time
```

These concepts must not be collapsed into one representation.

---

# 3. Event

An Event represents something that happened or was observed to happen.

An Event should be:

```text
identifiable
timestamped
ordered where possible
associated with an actor
associated with affected objects
provenance-aware
immutable as historical record
```

Examples:

```text
file created
file modified
file deleted
command executed
test completed
build completed
commit created
requirement changed
decision accepted
claim contradicted
context compiled
AI response generated
```

---

# 4. Event vs Activity

An Event records something that occurred.

An Activity represents an operation, process, or unit of work that may produce one or more Events.

Conceptually:

```text
Activity
    │
    ├── begins
    ├── performs work
    ├── produces Events
    └── completes
```

Example:

```text
Activity:
    "Implement authentication middleware"

Events:
    file modified
    test created
    test executed
    build executed
    commit created
```

Activity is therefore broader than Event.

---

# 5. Event Immutability

Historical Events should be treated as immutable records.

If an Event is later determined to be incorrect, Continuum should not silently rewrite history.

Instead it should record:

```text
correction
supersession
retraction
reinterpretation
```

Example:

```text
Event A:
    "Build succeeded."

Later:
    Event B:
        "Build result was incorrectly reported."

```

The historical record remains intact while the knowledge derived from it may change.

---

# 6. Event Identity

An Event should have a stable identity.

Conceptually:

```text
Event
├── event_id
├── event_type
├── occurred_at
├── recorded_at
├── actor
├── subject
├── affected_objects
├── source
└── payload
```

The exact schema remains implementation-specific.

---

# 7. Event Time

Continuum should distinguish at least:

```text
occurred_at
recorded_at
```

`occurred_at` represents when the event happened.

`recorded_at` represents when Continuum learned or recorded it.

These may differ significantly.

Example:

```text
Commit created:
    July 20

Imported into Continuum:
    July 29
```

The event occurred on July 20 even though it was recorded on July 29.

---

# 8. Event Ordering

Events should support temporal ordering where reliable.

Potential ordering mechanisms include:

```text
timestamp
sequence number
causal relationship
repository revision
activity ordering
external event identifier
```

Wall-clock timestamps alone may not establish causality.

---

# 9. Causality

Events may have causal relationships.

Example:

```text
Requirement changed
        ↓
Decision changed
        ↓
Source modified
        ↓
Tests modified
        ↓
Build executed
        ↓
Commit created
```

Continuum should eventually represent these relationships explicitly where known.

---

# 10. Event Categories

Potential event categories include:

```text
Observation
Mutation
Interaction
Execution
Decision
Communication
Synchronization
Lifecycle
Evaluation
```

These categories are conceptual and may evolve.

---

# 11. Observation Events

Observation Events represent something being observed.

Examples:

```text
file discovered
repository inspected
build output observed
runtime behavior observed
external issue retrieved
test result observed
```

Observation does not necessarily imply that the observed condition was caused by the observer.

---

# 12. Mutation Events

Mutation Events represent changes to project objects.

Examples:

```text
file created
file modified
file deleted
requirement changed
decision changed
configuration changed
artifact relocated
```

Mutation Events may produce new Artifact Versions or State.

---

# 13. Interaction Events

Interaction Events represent communication or interaction between actors and systems.

Examples:

```text
user message
AI response
tool invocation
approval
rejection
question
instruction
feedback
```

These events are especially important for AI-assisted engineering.

---

# 14. Execution Events

Execution Events represent the running of an operation.

Examples:

```text
command executed
test executed
build executed
lint executed
formatter executed
deployment executed
migration executed
agent task executed
```

Execution Events should capture relevant inputs, outputs, and environment where practical.

---

# 15. Decision Events

Decision Events represent transitions in decision status.

Examples:

```text
decision proposed
decision reviewed
decision accepted
decision rejected
decision superseded
decision reversed
```

A Decision Event does not replace the Decision object.

It records the lifecycle event affecting the Decision.

---

# 16. Communication Events

Communication Events represent exchanges between actors.

Examples:

```text
human → AI
AI → human
human → human
AI → AI
system → actor
external system → Continuum
```

Communication Events may contain or reference:

```text
message
instruction
question
answer
feedback
request
```

Sensitive content may require access controls or redaction.

---

# 17. Synchronization Events

Synchronization Events represent importing or reconciling information from an external source.

Examples:

```text
repository synchronized
issue tracker synchronized
GitHub data imported
CI result imported
runtime telemetry imported
external document synchronized
```

Synchronization events should preserve the external source identity.

---

# 18. Lifecycle Events

Lifecycle Events describe creation, activation, completion, suspension, or termination.

Examples:

```text
project created
session started
task started
task completed
context created
context invalidated
agent started
agent stopped
deployment activated
deployment terminated
```

---

# 19. Evaluation Events

Evaluation Events represent judgments or assessments.

Examples:

```text
test passed
test failed
review approved
review rejected
validation succeeded
validation failed
context deemed sufficient
context deemed insufficient
claim confirmed
claim contradicted
```

Evaluation results may contribute to Knowledge.

---

# 20. Actors

Events should identify the actor responsible for or associated with the event where possible.

Potential actor categories:

```text
human
AI
agent
tool
service
automation
external system
unknown
```

An Event may have:

```text
initiator
executor
observer
approver
```

These roles may be distinct.

---

# 21. Event Subject

An Event should identify the primary subject when applicable.

Examples:

```text
file modified
    subject = file

decision accepted
    subject = decision

context compiled
    subject = context

test executed
    subject = test
```

An Event may affect multiple additional objects.

---

# 22. Affected Objects

Events may affect multiple objects.

Example:

```text
Commit Created
    │
    ├── repository
    ├── branch
    ├── files
    ├── changes
    └── work item
```

The model should support many-to-many Event/Object relationships.

---

# 23. Event Payload

Events may contain structured details specific to the event type.

Examples:

```text
FileModified:
    path
    previous_version
    new_version
    diff

CommandExecuted:
    command
    working_directory
    exit_code
    stdout_reference
    stderr_reference

TestCompleted:
    test
    result
    duration
    environment
```

Payload schemas are event-type-specific.

---

# 24. Event Source

Every Event should preserve its source where possible.

Potential sources:

```text
filesystem
repository
terminal
IDE
AI provider
agent framework
CI system
issue tracker
runtime system
human input
external API
```

Source identity supports provenance and reconciliation.

---

# 25. Event Confidence

Continuum may assign confidence to Events when the event itself is inferred rather than directly observed.

Examples:

```text
Directly observed:
    high

Imported from trusted source:
    high

Inferred from related events:
    medium

Reconstructed from incomplete history:
    low
```

Event confidence is distinct from Claim confidence.

---

# 26. Event Relationships

Events may relate to other Events.

Potential relationships:

```text
caused_by
triggered_by
preceded_by
followed_by
supersedes
corrects
retracts
correlates_with
derived_from
```

These relationships help reconstruct activity chains.

---

# 27. Activity

An Activity represents a bounded unit of work or operation.

Examples:

```text
implement feature
debug failure
review pull request
run test suite
compile context
perform repository synchronization
conduct architecture analysis
```

An Activity may have:

```text
activity_id
type
initiator
start_time
end_time
status
goal
inputs
outputs
events
```

---

# 28. Activity Lifecycle

An Activity may move through states such as:

```text
planned
started
paused
blocked
completed
failed
cancelled
abandoned
```

The exact state machine remains open.

---

# 29. Activity Inputs

Activities may consume:

```text
requirements
tasks
artifacts
knowledge
context
instructions
configuration
external data
```

Example:

```text
Activity:
    Debug authentication failure

Inputs:
    issue
    stack trace
    source
    tests
    context package
```

---

# 30. Activity Outputs

Activities may produce:

```text
changes
artifacts
events
knowledge
decisions
test results
build results
context packages
messages
recommendations
```

---

# 31. Activity and AI

AI-assisted work should be represented as Activity.

Example:

```text
AI Activity
    │
    ├── task
    ├── context
    ├── instructions
    ├── tool calls
    ├── generated output
    ├── proposed changes
    └── resulting events
```

This allows Continuum to reconstruct what an AI did and what information it had available.

---

# 32. AI Interaction

An AI Interaction represents an interaction between an AI actor and another actor or system.

Example:

```text
Human
  │
  │ instruction
  ▼
AI
  │
  │ response
  ▼
Human
```

An interaction may contain multiple Events.

---

# 33. AI Context Provenance

Every AI Activity should, where possible, reference the Context Package supplied to it.

Conceptually:

```text
AI Activity
    │
    ├── uses
    │     └── Context Package
    │
    ├── produces
    │     └── AI Output
    │
    └── causes
          └── Events
```

This establishes the relationship:

```text
What did the AI know?
What did the AI do?
What happened afterward?
```

---

# 34. Tool Invocation

Tool usage should be modeled as Activity or Event depending on granularity.

Example:

```text
AI
 ↓
Tool Invocation
 ↓
filesystem.read
 ↓
Result
```

Tool invocation should capture, where appropriate:

```text
tool
arguments
actor
start
end
result
error
affected objects
```

Secrets and sensitive arguments must not be stored indiscriminately.

---

# 35. Command Execution

Command execution is a particularly important Event type for software engineering.

Potential information:

```text
command
working directory
environment
actor
start time
end time
exit code
stdout
stderr
affected artifacts
```

Command output should preferably be stored by reference when large.

---

# 36. Repository Events

Continuum should recognize repository activity such as:

```text
repository discovered
branch created
branch switched
file added
file modified
file deleted
file moved
commit created
tag created
merge performed
rebase performed
working tree changed
```

These events should integrate with the Artifact & Repository Model.

---

# 37. Build Events

Build activity may generate:

```text
build started
build completed
build failed
artifact generated
dependency resolution changed
compiler warning
compiler error
```

Build Events should be associated with:

```text
source revision
environment
configuration
output artifacts
result
```

---

# 38. Test Events

Test activity may generate:

```text
test started
test passed
test failed
test skipped
test errored
test suite completed
```

Test Events should connect to Test Results and Evidence.

---

# 39. Knowledge Events

Knowledge itself can change.

Examples:

```text
claim created
claim updated
claim confirmed
claim contradicted
claim deprecated
claim superseded
evidence added
evidence removed
confidence changed
interpretation changed
```

These Events should not silently rewrite prior Knowledge.

---

# 40. Context Events

Context-related activity should also be represented.

Examples:

```text
context requested
context compiled
context validated
context rejected
context expanded
context reduced
context invalidated
context refreshed
```

These events provide an audit trail for AI context construction.

---

# 41. Event → State

State is derived from Events.

Conceptually:

```text
Event₁
Event₂
Event₃
Event₄
   │
   ▼
State Reconstruction
   │
   ▼
Current State
```

The current state should not require every previous Event to be manually interpreted by an AI.

State projection should eventually be computational.

---

# 42. Event → Knowledge

Events may generate or modify Knowledge.

Example:

```text
Test failed
    ↓
Observation
    ↓
Claim:
    Authentication fails under condition X.
```

The Event itself is not necessarily the Claim.

Instead:

```text
Event
    ↓
Observation
    ↓
Claim
    ↓
Knowledge
```

This preserves epistemic distinctions.

---

# 43. Event → Context

Events may invalidate or alter Context.

Example:

```text
Context compiled
      ↓
Architecture changed
      ↓
Context becomes stale
```

Relevant Events should therefore be capable of triggering Context invalidation.

---

# 44. Event Sourcing

Continuum may eventually use event-sourcing techniques for selected domains.

However:

> Continuum is not required to implement the entire system as a pure event-sourced architecture.

Event sourcing is an implementation strategy.

The conceptual requirement is that important historical activity remains reconstructable.

---

# 45. Event Deduplication

The same Event may be observed through multiple sources.

Example:

```text
GitHub:
    commit abc123

Local Git:
    commit abc123

CI:
    commit abc123
```

Continuum should be able to recognize that these records may refer to the same underlying event or occurrence.

Deduplication and reconciliation rules remain open.

---

# 46. Event Reconciliation

When sources disagree, Continuum should preserve the disagreement rather than silently choosing one.

Example:

```text
Source A:
    build succeeded

Source B:
    build failed
```

Continuum should record:

```text
conflict
sources
observations
timestamps
resolution status
```

The resulting Knowledge may remain uncertain until resolved.

---

# 47. Event Retention

Not all Events require identical retention.

Potential retention classes:

```text
permanent
long-term
normal
ephemeral
derived
reconstructable
```

Retention policies must not destroy information required for auditability or reproducibility.

---

# 48. Event Granularity

Continuum must avoid both extremes:

Too coarse:

```text
"AI worked on project."
```

Too granular:

```text
every CPU instruction
every keystroke
every internal implementation detail
```

Event granularity should be determined by:

```text
relevance
auditability
reproducibility
storage cost
privacy
security
usefulness
```

---

# 49. Event Privacy

Events may contain sensitive information.

Potentially sensitive content includes:

```text
credentials
tokens
private source
personal information
secrets
customer information
proprietary data
```

Continuum must support:

```text
redaction
classification
access control
retention policy
secure references
```

Sensitive information must not be indiscriminately copied into Event payloads.

---

# 50. Event Graph

Conceptually:

```text
                 ACTIVITY
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       EVENT      EVENT      EVENT
          │         │         │
          └────┬────┴────┬────┘
               │         │
               ▼         ▼
             OBJECTS   KNOWLEDGE
               │         │
               └────┬────┘
                    ▼
                  STATE
                    │
                    ▼
                 CONTEXT
                    │
                    ▼
                    AI
                    │
                    ▼
                 ACTIVITY
```

This creates the temporal feedback loop of Continuum.

---

# 51. Activity Trace

An Activity should be reconstructable as a trace.

Example:

```text
Activity:
    Debug authentication failure

    10:01  task started
    10:02  context compiled
    10:03  source inspected
    10:04  test executed
    10:04  test failed
    10:05  AI generated hypothesis
    10:06  file modified
    10:07  test executed
    10:07  test passed
    10:08  build completed
    10:09  activity completed
```

This is substantially more useful than a transcript alone.

---

# 52. Activity Outcome

Activities should eventually support outcome classification:

```text
successful
partially successful
failed
blocked
cancelled
inconclusive
```

Outcome should be derived from relevant Events and results where possible.

---

# 53. Event Ordering and Causality

Continuum should distinguish:

```text
temporal ordering
causal ordering
logical dependency
```

These are not equivalent.

For example:

```text
Event A occurred before Event B
```

does not necessarily mean:

```text
Event A caused Event B
```

Causality should only be asserted when supported by evidence or explicit inference.

---

# 54. Event Confidence vs Knowledge Confidence

The following are distinct:

```text
Event Confidence
    = confidence that the event occurred as represented

Claim Confidence
    = confidence that the proposition represented by the claim is true
```

Example:

```text
Event:
    test failed
Confidence:
    high

Claim:
    authentication architecture is defective
Confidence:
    low
```

A known event can support an uncertain interpretation.

---

# 55. Open Questions

The following remain unresolved:

1. Event schema
2. Activity schema
3. Event taxonomy
4. Event ordering
5. Causal inference
6. Event deduplication
7. Event reconciliation
8. Event retention
9. Event compression
10. Event storage
11. Event indexing
12. Activity state machine
13. AI interaction representation
14. Tool invocation model
15. Command execution model
16. Event privacy model
17. Event access control
18. Event provenance granularity
19. Event replay strategy
20. State projection strategy
21. Event-sourcing boundaries
22. Distributed event ingestion
23. External event normalization
24. Clock skew handling
25. Offline event ingestion

---

# 56. Design Rules

Continuum establishes the following principles:

1. Objects represent what exists.
2. Events represent what happens.
3. State represents what is currently or historically true.
4. Events should be historically immutable.
5. Corrections should be represented as new Events.
6. Event time and recording time are distinct.
7. Temporal order is distinct from causality.
8. Activities represent bounded units of work.
9. Activities may produce multiple Events.
10. AI-assisted work should be represented as Activity.
11. AI context should be linked to AI Activity where possible.
12. Tool invocation should be observable.
13. Repository activity should be represented as Events.
14. Build and test activity should produce Events and Results.
15. Events may produce Knowledge but are not themselves necessarily Knowledge.
16. Event confidence and Claim confidence are distinct.
17. External event sources must preserve provenance.
18. Conflicting event sources should remain distinguishable.
19. Event granularity should be useful rather than exhaustive by default.
20. Sensitive event data must be protected.
21. Historical activity should be reconstructable where required.
22. State should be derivable from relevant historical Events where practical.

---

# 57. Design Rule

Continuum must preserve not merely **what the project is**, but **how it became what it is**.

The central temporal chain is:

```text
Intent
    ↓
Activity
    ↓
Event
    ↓
Change
    ↓
Artifact / State
    ↓
Observation
    ↓
Knowledge
    ↓
Context
    ↓
AI Activity
    ↓
New Event
```

This creates the foundation for continuous project continuity across humans, AI systems, tools, repositories, and time.
