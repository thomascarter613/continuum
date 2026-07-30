# Continuum Runtime Architecture

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

The Runtime Architecture defines how Continuum operates as a living system during AI-assisted software engineering.

Previous specifications established:

* what Continuum remembers
* how information becomes Knowledge
* how Knowledge evolves
* how relevant information is retrieved
* how context is compiled

This specification connects those concepts into an operational lifecycle.

The fundamental runtime loop is:

```text
PROJECT
   ↓
OBSERVE
   ↓
RECORD
   ↓
INTERPRET
   ↓
KNOWLEDGE
   ↓
RETRIEVE
   ↓
COMPILE CONTEXT
   ↓
AI AGENT
   ↓
ACTION
   ↓
OBSERVE RESULT
   ↓
RECORD
   └──────────────────────→
```

This loop is the heart of Continuum.

---

# 2. The Central Idea

Continuum is not primarily a database.

It is not primarily a prompt manager.

It is not primarily a vector database.

It is not primarily an AI agent framework.

Continuum is a **continuity runtime** that maintains an evolving model of work across time and uses that model to provide AI systems with the context required to continue that work intelligently.

---

# 3. Runtime Model

At runtime, Continuum exists between the project and the AI.

```text
┌─────────────────────────────────────────────────────────────┐
│                         PROJECT                              │
│                                                             │
│  source code • files • Git • tests • CI • docs • tools      │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               │ observe
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                        CONTINUUM                             │
│                                                             │
│  Observation → Memory → Knowledge → Retrieval → Context    │
│                                                             │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               │ context
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                         AI AGENT                             │
│                                                             │
│  reason → plan → act → inspect → modify → test              │
└──────────────────────────────┬──────────────────────────────┘
                               │
                               │ actions/results
                               ▼
                         PROJECT STATE
```

---

# 4. Runtime Components

The conceptual runtime consists of:

```text
Continuum Runtime
├── Project Interface
├── Observation Engine
├── Event Recorder
├── Memory Store
├── Knowledge Engine
├── State Engine
├── Retrieval Engine
├── Context Compiler
├── Agent Interface
├── Action Observer
├── Provenance Engine
├── Continuity Engine
└── Runtime API
```

These are logical responsibilities.

They do not yet imply a particular implementation language, database, or deployment architecture.

---

# 5. Runtime Boundaries

Continuum should maintain a strong distinction between:

```text
PROJECT
```

and:

```text
CONTINUUM
```

and:

```text
AI AGENT
```

The project is the thing being worked on.

Continuum is the continuity system.

The AI agent is the actor performing work.

---

# 6. Project

The project exists independently of Continuum.

Examples:

```text
Git repository
filesystem
IDE workspace
GitHub repository
CI environment
documentation
database
deployment environment
```

Continuum should never assume that it owns the project.

---

# 7. Continuum

Continuum observes and models the project.

It may:

```text
observe
index
remember
interpret
retrieve
compile
record
correlate
evaluate
```

But it should not silently alter the project.

---

# 8. AI Agent

The AI agent may:

```text
reason
plan
read
write
execute commands
modify source
run tests
inspect results
ask questions
make proposals
```

Continuum supplies context and records outcomes.

---

# 9. Agent Independence

Continuum should not require a particular agent.

Potential clients include:

```text
ChatGPT
Claude
Gemini
Codex
Cursor
Aider
OpenCode
custom agents
local models
IDE agents
human developers
```

Continuum should operate as an independent continuity layer.

---

# 10. Runtime Lifecycle

A typical interaction follows:

```text
1. Agent connects
2. Continuum identifies project
3. Current project state is observed
4. Task is received
5. Relevant Knowledge is retrieved
6. Context is compiled
7. Context is provided to agent
8. Agent reasons
9. Agent performs actions
10. Results are observed
11. Observations are recorded
12. Knowledge is updated
13. Project state is updated
14. Continuity state is checkpointed
```

---

# 11. Session Lifecycle

A session is a bounded period of active work.

Conceptually:

```text
Session
├── initialization
├── context acquisition
├── active work
├── observations
├── decisions
├── actions
├── results
└── closure
```

---

# 12. Session Initialization

When an AI begins work, Continuum should establish:

```text
project
repository
branch
commit
environment
agent
task
user
session
```

This creates the initial runtime identity.

---

# 13. Session Context

Continuum should then reconstruct:

```text
Where are we?
What are we doing?
Why are we doing it?
What has already happened?
What constraints apply?
What decisions have been made?
What remains?
```

This is the **continuity bootstrap**.

---

# 14. Continuity Bootstrap

The bootstrap process:

```text
Session Start
     ↓
Identify Project
     ↓
Resolve Current State
     ↓
Resolve Active Work
     ↓
Retrieve Relevant Knowledge
     ↓
Compile Resume Context
     ↓
Present Context to Agent
```

---

# 15. Active Work

Continuum should maintain awareness of active work.

Potential states:

```text
not started
planned
active
paused
blocked
completed
abandoned
superseded
```

---

# 16. Work State

The runtime should be able to determine:

```text
current objective
current task
current subtask
current blocker
current artifact
current branch
current environment
```

This provides a stable reference for context compilation.

---

# 17. Runtime State

Continuum's runtime state may be conceptualized as:

```text
RuntimeState
├── project
├── repository
├── source_state
├── work_state
├── knowledge_state
├── session_state
├── agent_state
└── environment_state
```

---

# 18. Project State

Project state describes the actual project.

Examples:

```text
files
branches
commits
dependencies
build status
test status
deployment status
configuration
```

---

# 19. Knowledge State

Knowledge state describes Continuum's understanding.

Examples:

```text
requirements
decisions
constraints
architecture
patterns
observations
hypotheses
unknowns
```

---

# 20. Session State

Session state describes what is happening now.

Examples:

```text
current task
current context
recent conversation
actions
tool calls
observations
pending questions
```

---

# 21. Agent State

Agent state may include:

```text
agent identity
model
capabilities
current objective
available tools
permissions
current execution state
```

---

# 22. Environment State

Environment state may include:

```text
operating system
runtime versions
installed tools
container state
environment variables
network state
database availability
CI state
```

This can be critical during debugging.

---

# 23. Observation Engine

The Observation Engine watches for changes.

Sources may include:

```text
filesystem
Git
CLI
test runner
compiler
CI
issue tracker
IDE
agent tool calls
user input
external services
```

---

# 24. Observation Principle

An observation should answer:

> What happened or what is currently observable?

Examples:

```text
file changed
test failed
commit created
command executed
build succeeded
dependency installed
branch switched
```

---

# 25. Observation vs Interpretation

Continuum should distinguish:

```text
Observation:
    test X failed with error Y.
```

from:

```text
Interpretation:
    error Y is probably caused by configuration Z.
```

The first is evidence.

The second is reasoning.

---

# 26. Observation vs Knowledge

An observation does not automatically become established Knowledge.

Example:

```text
Observation:
    test failed.

Knowledge:
    configuration X causes the failure.
```

The latter requires evidence or reasoning.

---

# 27. Event Recorder

Events provide the runtime timeline.

Examples:

```text
SessionStarted
TaskStarted
FileChanged
CommandExecuted
TestStarted
TestFailed
TestPassed
CommitCreated
DecisionProposed
DecisionAccepted
TaskBlocked
TaskCompleted
SessionEnded
```

---

# 28. Event Stream

Conceptually:

```text
t0 ─ SessionStarted
t1 ─ TaskStarted
t2 ─ FileModified
t3 ─ TestFailed
t4 ─ DiagnosisProposed
t5 ─ FileModified
t6 ─ TestPassed
t7 ─ CommitCreated
t8 ─ TaskCompleted
```

This timeline is extremely valuable for reconstructing what happened.

---

# 29. Memory Store

Memory preserves observations and experience.

It may contain:

```text
events
conversation
tool calls
attempts
results
snapshots
summaries
historical states
```

Memory should preserve history rather than merely the latest state.

---

# 30. Knowledge Engine

The Knowledge Engine transforms observations and other evidence into structured project Knowledge.

Conceptually:

```text
Observation
     ↓
Interpretation
     ↓
Evidence
     ↓
Knowledge Candidate
     ↓
Validation
     ↓
Knowledge
```

---

# 31. Knowledge Engine Responsibilities

The Knowledge Engine should eventually support:

```text
extraction
classification
validation
confidence assignment
relationship discovery
supersession
contradiction detection
summarization
```

---

# 32. State Engine

The State Engine determines the current state of the project.

It synthesizes:

```text
current observations
events
artifacts
Git state
work state
Knowledge
```

into:

```text
CurrentState
```

---

# 33. State Is Derived

Current state should not necessarily be stored as one immutable document.

It may be derived from:

```text
events
snapshots
artifacts
Knowledge
external state
```

This allows reconstruction.

---

# 34. State Snapshots

For efficiency, Continuum may periodically create snapshots.

Example:

```text
Event 1
Event 2
Event 3
...
Event 100
       ↓
Snapshot A

Event 101
Event 102
...
       ↓
Snapshot B
```

Snapshots reduce reconstruction cost.

---

# 35. Retrieval Engine

The Retrieval Engine receives a task and current state.

```text
Task
 +
CurrentState
 +
ContextProfile
        ↓
Retrieval
```

It returns candidate information.

---

# 36. Context Compiler

The Context Compiler transforms candidates into an AI-ready context package.

```text
Candidates
    ↓
Selection
    ↓
Compression
    ↓
Ordering
    ↓
Validation
    ↓
ContextPackage
```

---

# 37. Agent Interface

The Agent Interface provides context to an AI agent and receives interactions.

Conceptually:

```text
Continuum
    │
    │ Context
    ▼
Agent
    │
    │ Requests / Actions
    ▼
Continuum
```

---

# 38. Agent Actions

An agent may request:

```text
read file
write file
run command
run test
inspect Git
search code
query Continuum
ask user
```

Continuum should observe these actions when possible.

---

# 39. Action Observation

An agent action should produce:

```text
Action
   ↓
Execution
   ↓
Result
   ↓
Observation
```

Example:

```text
Action:
    run npm test

Result:
    17 tests passed

Observation:
    test suite passed at time T.
```

---

# 40. Tool Calls as Events

Tool interactions may be recorded as events:

```text
ToolCallRequested
ToolCallStarted
ToolCallCompleted
ToolCallFailed
```

This allows reconstruction of agent behavior.

---

# 41. Action Provenance

Each action should eventually be traceable to:

```text
agent
session
task
context
decision
timestamp
tool
result
```

This provides AI-action traceability.

---

# 42. Decision Lifecycle

AI-assisted work frequently involves decisions.

The runtime should support:

```text
DecisionProposed
      ↓
DecisionEvaluated
      ↓
DecisionAccepted / Rejected
      ↓
DecisionRecorded
```

---

# 43. Decisions Are Not Automatically Accepted

An AI suggestion is not automatically project truth.

For example:

```text
AI:
    "I recommend using SQLite."

Continuum:
    Proposal

User:
    "Yes, let's use SQLite."

Continuum:
    Accepted Decision
```

---

# 44. Human Authority

Continuum should preserve the distinction between:

```text
AI proposal
human decision
automatically verified fact
external evidence
```

This is critical to epistemic integrity.

---

# 45. Agent Proposals

The runtime should be capable of recording:

```text
proposal
reasoning summary
supporting evidence
alternatives
expected consequences
```

A proposal may later become a decision.

---

# 46. Action Execution

An action may modify the project.

Example:

```text
AI
 ↓
edit src/auth.ts
 ↓
filesystem
 ↓
Git diff
 ↓
Observation
```

Continuum should be able to correlate the modification with the action.

---

# 47. Causal Chain

The ideal runtime trace is:

```text
Task
 ↓
Context
 ↓
AI Decision
 ↓
Agent Action
 ↓
Project Change
 ↓
Observation
 ↓
Test Result
 ↓
Knowledge Update
```

This creates a causal chain across the entire work lifecycle.

---

# 48. Causal Traceability

Continuum should eventually allow queries such as:

```text
Why was this file changed?

Which task caused this commit?

Which decision led to this architecture?

Why was this dependency added?

Which test failure led to this change?

What did the AI know when it made this change?
```

---

# 49. Project Change Detection

Continuum should independently observe project changes.

This is important because not every change is made by an AI.

Changes may come from:

```text
human
AI
CI
external automation
dependency update
external collaborator
```

---

# 50. External Changes

If a developer modifies code outside an AI session:

```text
human edits file
      ↓
Continuum observes change
      ↓
event recorded
      ↓
state updated
      ↓
future context reflects change
```

This keeps Continuum synchronized.

---

# 51. Synchronization

Continuum should support synchronization with the project.

Possible states:

```text
synchronized
partially synchronized
stale
unknown
```

---

# 52. Stale Continuum State

If Continuum's model is stale:

```text
Continuum:
    AuthService uses implementation X.

Repository:
    AuthService now uses implementation Y.
```

Continuum must detect and correct the discrepancy.

---

# 53. Source of Truth

For current implementation state:

```text
actual project state
```

should generally outrank:

```text
Continuum memory
```

Continuum models reality; it does not redefine reality.

---

# 54. Knowledge vs Source of Truth

For some questions, Continuum Knowledge may be authoritative.

Example:

```text
"What was the rationale for decision D?"
```

For others:

```text
"What code is currently in production?"
```

the external project state may be authoritative.

The runtime must understand source authority by domain.

---

# 55. Runtime Reconciliation

When project state and Knowledge disagree:

```text
Project
   │
   ▼
Observation
   │
   ▼
Reconciliation
   │
   ├── Knowledge still valid
   ├── Knowledge superseded
   ├── Conflict
   └── Unknown
```

---

# 56. Knowledge Supersession

Example:

```text
Old:
    "Use Redis."

New:
    repository no longer contains Redis.

Continuum:
    old Knowledge marked superseded.
```

The historical record remains.

---

# 57. Runtime Checkpoints

Long-running work should support checkpoints.

A checkpoint may capture:

```text
current state
task state
recent observations
decisions
context fingerprint
repository state
known blockers
next action
```

---

# 58. Checkpoint Purpose

Checkpoints allow:

```text
resume
rollback analysis
session recovery
handoff
agent switching
human review
```

---

# 59. Session Handoff

A session may be handed from one AI agent to another.

Example:

```text
Agent A
   ↓
checkpoint
   ↓
Continuum
   ↓
Agent B
```

Agent B should not need the original conversation to continue.

This is a core Continuum capability.

---

# 60. Agent Switching

The system should eventually support:

```text
Claude → Codex
Codex → local model
local model → human
human → AI
```

without losing project continuity.

---

# 61. Model Independence

Continuity belongs to the project, not to the model.

The project should not become dependent on:

```text
one model
one provider
one chat
one IDE
one agent framework
```

---

# 62. Conversation Independence

Conversation history is useful but should not be the foundation of continuity.

Continuity should survive:

```text
conversation deletion
agent change
model change
IDE change
machine change
session expiration
```

---

# 63. Human Handoff

A human should be able to resume from Continuum too.

For example:

```text
Current Task:
    Implement authentication persistence.

Completed:
    schema created

Blocked:
    migration test failing

Decision:
    PostgreSQL remains the persistence layer

Next:
    fix migration fixture
```

---

# 64. Runtime API

Continuum should eventually expose an API around core operations.

Conceptually:

```text
project
session
task
observe
remember
query
retrieve
compile
checkpoint
record
state
knowledge
```

---

# 65. Core Runtime Operations

The initial conceptual API may include:

```text
initialize()
attach()
observe()
record()
remember()
query()
retrieve()
compile()
checkpoint()
resume()
reconcile()
```

The exact API will be defined later.

---

# 66. Attach

`attach` connects Continuum to an existing project.

Conceptually:

```text
continuum attach ./project
```

It establishes:

```text
project identity
repository identity
initial state
observation sources
```

---

# 67. Initialize

`initialize` creates Continuum metadata for a project.

Conceptually:

```text
continuum init
```

It should not require the project to already contain Continuum-specific files beyond what is necessary.

---

# 68. Observe

Observation may be:

```text
automatic
manual
event-driven
polling
agent-triggered
```

---

# 69. Record

Record explicitly stores an event or observation.

Example:

```text
continuum record decision "Use PostgreSQL for persistence"
```

The exact CLI syntax is deferred.

---

# 70. Query

Query allows direct exploration of Continuum.

Examples:

```text
What did we decide about authentication?

What have we already tried?

What remains?

Why did we choose PostgreSQL?
```

---

# 71. Retrieve

Retrieve is a lower-level operation.

It returns candidate information relevant to a query.

---

# 72. Compile

Compile transforms project state and retrieval results into an AI context.

Conceptually:

```text
continuum compile --task "implement authentication"
```

---

# 73. Resume

Resume reconstructs continuity.

Conceptually:

```text
continuum resume
```

It should answer:

```text
where we are
what we were doing
what changed
what remains
what happens next
```

---

# 74. Checkpoint

Checkpoint captures a resumable state.

Conceptually:

```text
continuum checkpoint
```

---

# 75. Reconcile

Reconcile compares Continuum's model with project reality.

Conceptually:

```text
continuum reconcile
```

It identifies:

```text
new changes
stale Knowledge
conflicts
missing observations
```

---

# 76. Runtime Event Flow

A complete example:

```text
User:
    "Continue implementing authentication."

        ↓

Continuum:
    identifies project

        ↓

Continuum:
    observes current repository

        ↓

Continuum:
    identifies active task

        ↓

Continuum:
    retrieves relevant Knowledge

        ↓

Continuum:
    compiles context

        ↓

AI:
    receives context

        ↓

AI:
    decides to modify AuthService

        ↓

AI:
    edits source

        ↓

Continuum:
    observes file change

        ↓

AI:
    runs tests

        ↓

Continuum:
    observes test failure

        ↓

AI:
    diagnoses failure

        ↓

AI:
    modifies test fixture

        ↓

Tests:
    pass

        ↓

Continuum:
    records result

        ↓

Continuum:
    updates Knowledge

        ↓

Continuum:
    creates checkpoint
```

---

# 77. The Runtime Loop

The essential runtime loop is therefore:

```text
┌───────────────────────────────┐
│                               │
│        PROJECT STATE          │
│                               │
└───────────────┬───────────────┘
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
            RETRIEVAL
                │
                ▼
        CONTEXT COMPILATION
                │
                ▼
             AI AGENT
                │
                ▼
             ACTION
                │
                ▼
          PROJECT CHANGE
                │
                └───────────────┐
                                │
                                ▼
                           OBSERVATION
```

This is the fundamental Continuum architecture.

---

# 78. Runtime as a Feedback System

Continuum can therefore be understood as a feedback system.

```text
Current project state
        ↓
Knowledge model
        ↓
Context
        ↓
AI action
        ↓
New project state
        ↓
New evidence
        ↓
Updated Knowledge
```

The system continually reduces the distance between:

```text
what the project is
```

and:

```text
what the AI understands about the project.
```

---

# 79. Runtime Temporal Model

Continuum operates across three temporal dimensions:

```text
Past
 ↓
Memory / History

Present
 ↓
Current State

Future
 ↓
Plans / Tasks / Intentions
```

The AI acts in the present while relying upon the past to influence the future.

---

# 80. Past

The past contains:

```text
events
decisions
attempts
failures
changes
conversations
```

---

# 81. Present

The present contains:

```text
current repository
current branch
current work
current constraints
current blockers
current Knowledge
```

---

# 82. Future

The future contains:

```text
plans
tasks
goals
intentions
proposals
expected outcomes
```

---

# 83. Temporal Continuity

A useful model is:

```text
PAST
  │
  │ memory
  ▼
PRESENT
  │
  │ reasoning
  ▼
FUTURE
  │
  │ action
  ▼
NEW PRESENT
  │
  └──────────→ PAST
```

This is why Continuum must preserve history rather than only current state.

---

# 84. Runtime Trust Model

The runtime should maintain a trust hierarchy.

Conceptually:

```text
Observed project state
        ↓
Verified evidence
        ↓
Accepted decisions
        ↓
Supported Knowledge
        ↓
Hypotheses
        ↓
AI proposals
```

The exact hierarchy may vary by information type.

---

# 85. AI Does Not Define Reality

The AI may propose:

```text
"Perhaps the bug is caused by X."
```

Continuum should record:

```text
Hypothesis:
    X may cause bug.
```

not:

```text
Fact:
    X causes bug.
```

unless evidence supports that transition.

---

# 86. Runtime Safety Principle

Continuum should preserve epistemic distinctions throughout:

```text
observation
interpretation
hypothesis
decision
fact
unknown
```

These distinctions should survive:

```text
storage
retrieval
compression
context compilation
agent interaction
handoff
```

---

# 87. Runtime Auditability

A future investigator should be able to reconstruct:

```text
what the AI knew
what the AI was asked to do
what context it received
what it decided
what it changed
what happened afterward
```

This is a foundational property.

---

# 88. Runtime Replay

If sufficient telemetry exists, Continuum should eventually support replay:

```text
session
   ↓
state reconstruction
   ↓
context reconstruction
   ↓
action reconstruction
   ↓
result reconstruction
```

This enables debugging and evaluation.

---

# 89. Runtime Determinism

Not everything can be deterministic because AI models are probabilistic.

However, Continuum should make deterministic everything it controls:

```text
state resolution
retrieval scope
provenance
context selection rules
event ordering
checkpointing
project synchronization
```

---

# 90. Runtime Extensibility

The runtime should support adapters for:

```text
AI providers
agent frameworks
Git providers
IDEs
CI systems
issue trackers
documentation systems
databases
observability systems
```

---

# 91. Runtime Independence

Continuum must remain useful without any one external integration.

At minimum:

```text
filesystem
Git
local storage
AI interface
```

should be sufficient to operate a basic system.

---

# 92. Offline Operation

A foundational implementation should eventually support local operation.

The core continuity loop should not require:

```text
cloud services
proprietary databases
external AI APIs
```

unless the user chooses them.

---

# 93. Cloud Optionality

Cloud services may improve:

```text
collaboration
synchronization
remote agents
backup
multi-device access
```

but should remain optional.

---

# 94. Local-First Principle

The default architecture should prefer:

```text
local project
local Continuum state
local indexes
local history
```

with optional remote synchronization.

---

# 95. Portability

A project should be able to move between machines without losing continuity.

Conceptually:

```text
Machine A
    ↓
Continuum state
    ↓
portable representation
    ↓
Machine B
```

---

# 96. Project Portability

Continuum state should eventually be portable independently of:

```text
machine
OS
AI provider
IDE
agent framework
database engine
cloud provider
```

---

# 97. Runtime Architecture Principle

The runtime should therefore remain:

```text
AI-independent
IDE-independent
provider-independent
cloud-independent
database-independent
repository-provider-independent
```

where practical.

---

# 98. The Continuum Runtime Contract

At its highest level:

```text
INPUT:

    project state
    human intent
    AI intent
    external events

PROCESS:

    observe
    remember
    interpret
    retrieve
    compile
    act
    reconcile

OUTPUT:

    continuity
    context
    traceability
    updated Knowledge
    updated project state
```

---

# 99. The Fundamental Loop

The entire architecture can be reduced to:

```text
        ┌──────────────────┐
        │      PROJECT     │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │     OBSERVE      │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │      MEMORY      │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │    KNOWLEDGE     │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │    RETRIEVAL     │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │ CONTEXT COMPILER │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │    AI AGENT      │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │      ACTION      │
        └────────┬─────────┘
                 │
                 ▼
        ┌──────────────────┐
        │ PROJECT CHANGES  │
        └────────┬─────────┘
                 │
                 └───────────────────────┐
                                         │
                                         ▼
                                      OBSERVE
```

---

# 100. Architectural Conclusion

Continuum's fundamental role is now clear.

It sits between **project reality** and **AI cognition**.

It continuously observes the project, preserves experience, builds Knowledge, retrieves what matters, compiles task-specific context, supplies that context to an AI, observes what happens, and incorporates the results back into the continuity model.

Therefore:

> **Continuum is an independent runtime for maintaining continuity between humans, AI agents, and evolving software projects across sessions, models, tools, and time.**

The fundamental abstraction is not "memory."

It is:

```text
CONTINUITY
```

And the fundamental mechanism is:

```text
OBSERVE
→ REMEMBER
→ UNDERSTAND
→ RETRIEVE
→ COMPILE
→ ACT
→ OBSERVE
→ UPDATE
→ CONTINUE
```

---

# 101. Architectural Status

The following conceptual subsystems have now been established:

```text
Memory
Knowledge
Knowledge Evolution
Retrieval
Relevance
Context Compilation
Runtime Continuity
```

The next architectural work should define the actual **domain model** that connects them.

That work begins with the question:

> **What are the fundamental entities that exist inside Continuum, and how do they relate to one another?**
