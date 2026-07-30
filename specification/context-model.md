# Continuum Context Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

This document defines the Context Model for Continuum.

Continuum exists partly because long-running software-engineering projects exceed the practical context capacity of individual AI sessions.

The Context Model addresses this problem.

Its purpose is to determine:

* what information an actor needs
* why that information is relevant
* where the information comes from
* how it is selected
* how it is prioritized
* how it is compressed
* how it is validated
* how it is presented
* how its provenance is preserved
* when it becomes stale
* how context changes over time

The fundamental problem is:

```text
Project History
    ↓
Project State
    ↓
Current Intent
    ↓
Context Selection
    ↓
Context Package
    ↓
Actor
```

---

# 2. The Central Principle

Continuum must distinguish:

```text
Memory
Context
State
Knowledge
History
```

These are related but different.

---

# 3. Memory

Memory answers:

> What has happened or been retained?

Memory may contain:

```text
sessions
events
observations
decisions
knowledge
artifacts
summaries
historical context
```

Memory is potentially enormous.

---

# 4. Context

Context answers:

> What does this actor need to know right now?

Context is a **projection of memory and current state**.

Therefore:

```text
Context ≠ Memory
```

Instead:

```text
Context = f(
    current_state,
    current_intent,
    relevant_history,
    relevant_knowledge,
    constraints,
    actor,
    task,
    available_budget
)
```

---

# 5. State

State answers:

> What is true about the project right now?

Examples:

```text
current branch
current task
current requirements
current decisions
current artifacts
current dependencies
current blockers
current environment
```

State should generally outrank historical memory when determining what is currently true.

---

# 6. Knowledge

Knowledge answers:

> What does the system currently understand or believe?

Knowledge may include:

```text
facts
inferences
patterns
hypotheses
decisions
constraints
uncertainties
```

Knowledge must retain epistemic status.

---

# 7. History

History answers:

> How did we get here?

History includes:

```text
events
transitions
actions
observations
decisions
artifact changes
failed attempts
previous sessions
```

History is essential when current state alone cannot explain why the current state exists.

---

# 8. Context as a Projection

Context should be understood as a projection:

```text
                 Project
                    │
       ┌────────────┼────────────┐
       │            │            │
     State       Knowledge     History
       │            │            │
       └────────────┼────────────┘
                    │
              Context Builder
                    │
                    ▼
             Context Package
                    │
                    ▼
                  Actor
```

The Context Package is therefore not the canonical source of project truth.

---

# 9. Context Is Purpose-Bound

Context should always be generated for some purpose.

Examples:

```text
continue_task
debug_failure
review_change
make_decision
plan_work
implement_feature
understand_architecture
resume_session
perform_code_review
investigate_regression
```

The same project may produce radically different Context Packages depending upon purpose.

---

# 10. Context Request

A ContextRequest expresses the need for context.

Conceptually:

```text
ContextRequest
├── request_id
├── project_id
├── actor_id
├── purpose
├── task_id
├── scope
├── constraints
├── budget
├── requested_at
└── metadata
```

---

# 11. Context Request Example

Example:

```text
purpose:
    continue_task

task:
    TASK-123

actor:
    coding_agent

budget:
    40,000 tokens
```

This is fundamentally different from:

```text
purpose:
    architecture_review
```

even if both concern the same project.

---

# 12. Context Package

A ContextPackage is the result of satisfying a ContextRequest.

Conceptually:

```text
ContextPackage
├── package_id
├── request_id
├── project_id
├── generated_at
├── expires_at
├── purpose
├── items
├── summary
├── warnings
├── uncertainties
├── provenance
└── metadata
```

---

# 13. Context Item

A ContextItem is one piece of information selected for inclusion.

Examples:

```text
current task
architectural decision
relevant file
failed test
previous attempt
constraint
known issue
recent observation
dependency
requirement
```

Each ContextItem should preserve its source.

---

# 14. Context Item Structure

Conceptually:

```text
ContextItem
├── item_id
├── source_entity
├── source_type
├── content
├── relevance
├── importance
├── freshness
├── confidence
├── provenance
├── inclusion_reason
└── metadata
```

---

# 15. Context Item Provenance

Every significant ContextItem should be able to answer:

```text
Where did this come from?
```

Example:

```text
ContextItem:
    "Authentication uses session cookies."

source:
    Decision-42

source_type:
    decision
```

Another:

```text
ContextItem:
    "tests/auth.test.ts is currently failing."

source:
    TestObservation-731

source_type:
    observation
```

---

# 16. Inclusion Reason

Continuum should preserve why an item was selected.

Examples:

```text
current_state
directly_relevant
dependency
constraint
recent_change
historical_explanation
decision
known_failure
required_for_task
high_importance
uncertainty
```

This is valuable for debugging the Context Builder itself.

---

# 17. Context Selection

Context selection is the process of deciding what belongs in the package.

Conceptually:

```text
Candidate Information
        ↓
Relevance
        ↓
Importance
        ↓
Freshness
        ↓
Dependency
        ↓
Authority
        ↓
Budget
        ↓
Selection
```

---

# 18. Candidate Context

Potential context sources include:

```text
Project State
Current Task
Task Dependencies
Requirements
Constraints
Decisions
Recent Events
Relevant History
Knowledge
Observations
Evidence
Artifacts
Artifact Versions
Repository State
Environment State
Previous Sessions
Known Failures
Open Questions
Unresolved Uncertainty
```

---

# 19. Context Priority

Not all information deserves equal weight.

A conceptual priority hierarchy is:

```text
1. Current state
2. Current task
3. Active constraints
4. Active requirements
5. Applicable decisions
6. Immediate dependencies
7. Recent relevant events
8. Relevant verified knowledge
9. Relevant historical context
10. General project background
11. Low-relevance historical information
```

This is a starting policy rather than a final algorithm.

---

# 20. Relevance

Relevance asks:

> How directly does this information affect the current purpose?

For example, when fixing a TypeScript error in authentication code:

Highly relevant:

```text
authentication architecture
affected files
current error
recent changes
relevant tests
authentication decisions
```

Low relevance:

```text
unrelated UI work
old documentation redesign
historical experiments with unrelated libraries
```

---

# 21. Importance

Importance asks:

> How costly would it be for the actor to miss this information?

An item may be highly important even if it is not directly related to the current task.

Example:

```text
Constraint:
    "Do not introduce a PostgreSQL dependency."
```

That may be extremely important even if it is not explicitly mentioned in the current task.

---

# 22. Freshness

Freshness asks:

> How likely is this information to still reflect current reality?

Examples:

```text
Current repository state:
    extremely fresh

Yesterday's build result:
    moderately fresh

Six-month-old memory:
    potentially stale
```

Freshness should influence selection.

---

# 23. Authority

Authority asks:

> How authoritative is this information?

Example precedence:

```text
verified current artifact state
        ↓
verified execution state
        ↓
accepted decision
        ↓
established knowledge
        ↓
historical memory
        ↓
inference
        ↓
hypothesis
```

This is not absolute; domain-specific precedence may override it.

---

# 24. Context Budget

Context generation must respect a budget.

Possible budget dimensions:

```text
tokens
characters
bytes
items
latency
retrieval cost
model-specific context window
```

The initial implementation should support at least a token-oriented conceptual budget.

---

# 25. Budget Allocation

A ContextPackage should not spend its entire budget on one category.

Conceptually:

```text
Current State           15%
Current Task            15%
Constraints             10%
Requirements            10%
Decisions               10%
Relevant Artifacts      20%
Relevant History        10%
Knowledge               10%
```

These percentages are illustrative rather than normative.

The actual allocator should be adaptive.

---

# 26. Adaptive Context

Context allocation should depend on purpose.

For debugging:

```text
failure evidence
recent changes
affected artifacts
execution history
```

should receive more budget.

For architecture planning:

```text
requirements
constraints
decisions
architecture
patterns
tradeoffs
```

should receive more budget.

---

# 27. Context Layers

A useful conceptual model is layered context.

```text
Layer 0 — Identity
Layer 1 — Current State
Layer 2 — Current Intent
Layer 3 — Constraints
Layer 4 — Requirements
Layer 5 — Decisions
Layer 6 — Relevant Knowledge
Layer 7 — Relevant History
Layer 8 — Artifacts
Layer 9 — Supporting Evidence
Layer 10 — Optional Background
```

Not every request needs every layer.

---

# 28. Layer 0 — Identity

Identity establishes what the actor is looking at.

Example:

```text
Project:
    Continuum

Repository:
    continuum

Branch:
    main

Current Session:
    Session-92
```

---

# 29. Layer 1 — Current State

Current state establishes:

```text
current task
current branch
current environment
current blockers
current artifacts
current tests
current work status
```

This should usually be near the beginning of the Context Package.

---

# 30. Layer 2 — Current Intent

Intent answers:

```text
What are we trying to accomplish right now?
```

Examples:

```text
finish task
fix failing test
design subsystem
review architecture
investigate bug
implement feature
```

Intent is critical because relevance depends upon it.

---

# 31. Layer 3 — Constraints

Constraints define boundaries.

Examples:

```text
Linux required
no cloud services
8 GB RAM
Rust backend
Tauri desktop app
must remain offline-capable
must not break existing API
```

Constraints should receive high priority because violating them can invalidate otherwise good solutions.

---

# 32. Layer 4 — Requirements

Requirements establish what the system must accomplish.

Context should include only relevant requirements when possible.

---

# 33. Layer 5 — Decisions

Relevant architectural and product decisions should be included.

Especially important are decisions that constrain current work.

Example:

```text
Decision:
    Use SQLite for local persistence.

Reason:
    Offline-first requirement.
```

---

# 34. Layer 6 — Knowledge

Knowledge provides understanding beyond raw state.

Examples:

```text
known architecture
known failure modes
established patterns
previous discoveries
verified facts
known limitations
```

---

# 35. Layer 7 — History

History explains why current state exists.

History should be selectively included.

For example:

```text
Current bug
    ↓
Relevant previous attempts
    ↓
Previous failed solutions
```

is valuable.

An exhaustive transcript of every previous conversation is not.

---

# 36. Layer 8 — Artifacts

Artifacts provide direct evidence.

Examples:

```text
source files
configuration
schemas
tests
documentation
logs
repository metadata
```

Artifact inclusion should be driven by relevance.

---

# 37. Layer 9 — Evidence

Evidence provides support for claims.

Examples:

```text
test result
build output
compiler error
runtime observation
benchmark
human decision
external documentation
```

---

# 38. Layer 10 — Background

Background information provides broad orientation.

Examples:

```text
project history
long-term goals
architectural philosophy
non-current experiments
```

Background should usually have the lowest priority.

---

# 39. Context Assembly Pipeline

The Context Builder should conceptually operate as:

```text
ContextRequest
      ↓
Understand Intent
      ↓
Determine Scope
      ↓
Retrieve Candidate Information
      ↓
Rank Candidates
      ↓
Resolve Conflicts
      ↓
Allocate Budget
      ↓
Compress / Summarize
      ↓
Validate
      ↓
Package
      ↓
Deliver
```

---

# 40. Scope Determination

Scope determines where the Context Builder should search.

Possible scopes:

```text
project
repository
workspace
task
artifact
session
decision
requirement
subsystem
```

Scope should be explicit whenever possible.

---

# 41. Retrieval

Candidate information may be retrieved through multiple mechanisms.

Examples:

```text
direct lookup
graph traversal
event history
semantic search
keyword search
dependency analysis
artifact inspection
state queries
recent activity
```

Continuum should remain storage-independent.

---

# 42. Multi-Modal Retrieval

Context retrieval should not depend solely upon semantic vector search.

A robust system may combine:

```text
exact identifiers
graph relationships
recency
semantic similarity
dependency relationships
state
importance
authority
user intent
```

---

# 43. Graph Retrieval

The relationship graph can provide context through explicit connections.

Example:

```text
Current Task
    ↓
implements
    ↓
Requirement
    ↓
constrained_by
    ↓
Decision
```

The Context Builder can follow those relationships to discover relevant information.

---

# 44. Semantic Retrieval

Semantic search can identify conceptually related information.

Example:

Current task:

```text
"Fix authentication timeout"
```

Potentially relevant memories:

```text
"Session expiration investigation"
"Token refresh bug"
"Authentication middleware"
```

even when exact terms differ.

---

# 45. Temporal Retrieval

Recent events may be especially relevant.

For example:

```text
last 20 minutes
last session
last 24 hours
last successful implementation
last failed attempt
```

Temporal retrieval should be purpose-sensitive.

---

# 46. Dependency Retrieval

Context should follow dependencies.

If Task A depends on Task B:

```text
Task A
    depends_on
Task B
```

the current context may need the state and history of Task B.

---

# 47. Constraint Retrieval

Constraints should be retrieved even when they are not semantically similar to the current task.

This is because constraint relevance is often structural rather than semantic.

---

# 48. Decision Retrieval

Decisions should be retrieved through explicit relationships.

Example:

```text
Task
    constrained_by
Decision
```

This is stronger than discovering a decision through semantic similarity alone.

---

# 49. Failed Attempt Retrieval

One of the highest-value categories of context is:

```text
previous attempts that did not work
```

Example:

```text
Attempt 1:
    switched library X
    result: failed

Attempt 2:
    modified configuration Y
    result: partial success

Attempt 3:
    reverted X
    result: current approach
```

This prevents AI from repeating known mistakes.

---

# 50. Failure Memory

Failed attempts should be treated as first-class project knowledge.

A failure should ideally capture:

```text
what was attempted
why it was attempted
what changed
what happened
why it failed
what was learned
whether it should ever be retried
```

---

# 51. Context Compression

When relevant information exceeds the budget, Continuum should compress it.

Compression strategies include:

```text
summarization
deduplication
hierarchical summarization
temporal aggregation
fact extraction
decision extraction
event aggregation
artifact diffing
```

---

# 52. Compression Must Preserve Meaning

Compression must not erase critical information.

In particular, it should preserve:

```text
constraints
decisions
requirements
current state
known failures
uncertainty
provenance
important causal relationships
```

---

# 53. Hierarchical Context

Large histories should be represented hierarchically.

Example:

```text
Project
 ├── Milestone
 │    ├── Session
 │    │    ├── Events
 │    │    └── Observations
 │    └── Outcome
 └── Current State
```

The Context Builder can expand only the branches relevant to the current task.

---

# 54. Progressive Disclosure

Context should support progressive disclosure.

Start with:

```text
high-level summary
```

then allow the actor to request:

```text
details
```

then:

```text
raw evidence
```

This is more scalable than placing everything into the initial context.

---

# 55. Context References

A ContextPackage should be able to contain references.

Example:

```text
Decision-42
Artifact-123
Session-91
Event-8472
```

The actor can retrieve additional details when necessary.

---

# 56. Context Package as an Interface

The ContextPackage is effectively an interface between:

```text
Continuum's persistent world
```

and:

```text
AI's limited working context
```

Therefore:

```text
Continuum
    ↓
ContextPackage
    ↓
AI
```

is one of Continuum's most important architectural boundaries.

---

# 57. Context Package Should Be Self-Describing

A ContextPackage should explain its own scope.

Example:

```text
Purpose:
    continue task

Task:
    TASK-123

Generated:
    2026-07-30T04:30:00Z

Project state:
    ...

Relevant decisions:
    ...

Known blockers:
    ...

Relevant history:
    ...

Uncertainty:
    ...
```

---

# 58. Context Summary

Every ContextPackage should ideally include a concise executive summary.

The summary should answer:

```text
Where are we?
What are we doing?
Why?
What changed recently?
What matters?
What is blocked?
What remains uncertain?
What should happen next?
```

---

# 59. Context Warnings

The ContextPackage may include warnings.

Examples:

```text
WARNING:
Current architecture decision conflicts with repository state.

WARNING:
The following memory may be stale.

WARNING:
Two sources disagree about the implementation.

WARNING:
Required dependency could not be verified.
```

Warnings are preferable to silently choosing one interpretation.

---

# 60. Uncertainty Section

The package should explicitly preserve uncertainty.

Example:

```text
Uncertain:
    It is unclear whether module X is still required.

Evidence:
    package.json suggests yes.
    Current source imports suggest no.
```

This allows the AI to investigate rather than assume.

---

# 61. Contradiction Handling

If context sources conflict, Continuum should not silently merge them.

Example:

```text
Decision:
    use SQLite

Repository:
    PostgreSQL dependency detected
```

Context should represent the contradiction.

---

# 62. Context Validity

A ContextPackage is valid relative to the state from which it was generated.

If significant project state changes afterward, the package may become stale.

---

# 63. Context Freshness

Context should have:

```text
generated_at
expires_at
source_versions
state_version
```

where appropriate.

---

# 64. Context Invalidation

A ContextPackage may become invalid when:

```text
current task changes
critical decision changes
requirements change
relevant artifact changes
blocking issue resolves
new failure occurs
environment changes
knowledge is retracted
```

---

# 65. Context Regeneration

When context becomes stale:

```text
Old Context
    ↓
Invalidated
    ↓
New ContextRequest
    ↓
New ContextPackage
```

The old ContextPackage remains historically useful.

---

# 66. Context Versioning

ContextPackages should be versionable.

Example:

```text
ContextPackage v1
ContextPackage v2
ContextPackage v3
```

This allows comparison between what different AI sessions received.

---

# 67. Context Provenance

A ContextPackage should preserve:

```text
which sources were selected
why they were selected
when they were selected
what ranking was applied
what was compressed
what was omitted
```

This is essential for debugging AI behavior.

---

# 68. Context Selection Explainability

The system should eventually be able to answer:

```text
Why was this item included?
```

Example:

```text
Artifact:
    src/auth/session.ts

Reason:
    directly referenced by current Task.
```

---

# 69. Context Omission Explainability

It should eventually also answer:

```text
Why was this item omitted?
```

Possible reasons:

```text
low relevance
stale
duplicate
budget exceeded
outside scope
low authority
already represented by summary
```

---

# 70. Context Optimization

The Context Builder should optimize for:

```text
maximum useful information
within available budget
```

Conceptually:

```text
maximize:
    expected decision usefulness

subject to:
    token budget
    latency budget
    retrieval cost
```

---

# 71. Context Utility

An information item's utility may conceptually depend upon:

```text
relevance
importance
freshness
authority
uncertainty
dependency
novelty
cost
```

A future scoring model may represent this as:

```text
utility =
    relevance
  × importance
  × freshness
  × authority
  × dependency
  × novelty
```

The exact formula is intentionally deferred.

---

# 72. Context Diversity

Context should avoid spending its entire budget on redundant information.

For example:

```text
20 memories all saying the same thing
```

should not necessarily consume twenty times the context budget.

Deduplication and diversity should therefore be considered during ranking.

---

# 73. Context Coverage

A good ContextPackage should cover the dimensions necessary for the task.

For example, implementation context may need:

```text
intent
requirements
constraints
architecture
affected artifacts
tests
previous attempts
current state
```

The goal is not simply maximizing similarity.

It is maximizing **task-relevant coverage**.

---

# 74. Context Completeness

Context completeness should be purpose-specific.

A package is not complete because it contains everything.

It is complete when:

> The actor has enough relevant information to safely and effectively perform the requested task.

---

# 75. Context Sufficiency

Continuum should eventually support a concept of:

```text
sufficient context
```

A ContextPackage is sufficient when the expected value of additional information is below an acceptable threshold.

This allows context assembly to stop intelligently.

---

# 76. Context Escalation

If the system cannot produce sufficient context within the available budget, it should be able to signal:

```text
context_insufficient
```

rather than pretending that the context is complete.

---

# 77. Context Escalation Example

```text
AI:
    I need the history of the authentication redesign.

Continuum:
    Initial context insufficient.

Available:
    current architecture
    current files
    latest decision

Missing:
    rationale for migration from JWT to sessions

Action:
    retrieve historical decision chain
```

---

# 78. Context-on-Demand

The AI should be able to request additional context.

Examples:

```text
give me the previous failed attempts
show me the decision history
show me the evidence
show me the full file
show me what changed yesterday
```

This creates an iterative context loop.

---

# 79. Context Interaction Loop

```text
AI
 ↓
Context Request
 ↓
Continuum
 ↓
Context Package
 ↓
AI
 ↓
Question / Need
 ↓
Additional Context Request
 ↓
Continuum
```

This is more powerful than a single static prompt.

---

# 80. Context as a Query Language

The ContextRequest may eventually become expressive.

Example:

```text
context for:
    task = TASK-123

include:
    current_state
    requirements
    constraints
    decisions
    affected_artifacts
    recent_failures

exclude:
    unrelated_sessions

budget:
    30k tokens
```

---

# 81. Context Profiles

Different tasks may use different Context Profiles.

Examples:

```text
implementation
debugging
planning
architecture
review
research
handoff
incident_response
```

A Context Profile specifies default retrieval and ranking behavior.

---

# 82. Implementation Context Profile

Typical priorities:

```text
current task
requirements
constraints
affected artifacts
architecture
relevant decisions
tests
recent changes
previous failed attempts
```

---

# 83. Debugging Context Profile

Typical priorities:

```text
current failure
recent changes
error output
affected artifacts
execution history
previous debugging attempts
environment
related decisions
```

---

# 84. Architecture Context Profile

Typical priorities:

```text
goals
requirements
constraints
architectural decisions
patterns
tradeoffs
dependencies
current architecture
historical rationale
```

---

# 85. Handoff Context Profile

This profile is especially important for Continuum.

It should emphasize:

```text
current state
what was accomplished
what remains
what failed
why decisions were made
known issues
open questions
next recommended actions
```

The Handoff Context Profile is the mechanism that allows a new AI to inherit an ongoing project.

---

# 86. Context and Conversation

Conversation history may be useful, but it should not be the primary continuity mechanism.

The preferred hierarchy is:

```text
Project State
    ↓
Domain Knowledge
    ↓
Event History
    ↓
Relevant Session History
    ↓
Conversation Transcript
```

Conversation is evidence about project activity, not the project itself.

---

# 87. Transcript as Source

A transcript may contribute:

```text
human intent
unstated rationale
brainstorming
hypotheses
questions
decisions
```

But extracted information should eventually become structured domain objects where appropriate.

---

# 88. Conversation Extraction

A future Continuum pipeline may transform:

```text
Conversation
    ↓
Intent Detection
    ↓
Decision Detection
    ↓
Requirement Detection
    ↓
Knowledge Extraction
    ↓
Task Detection
    ↓
Event Recording
```

This reduces dependence upon raw transcripts.

---

# 89. Context and Human Memory

Continuum should also support human users who forget what they were doing.

The same Context Model should therefore serve:

```text
AI
Human
AI + Human
```

A human reopening a project may receive:

```text
You were working on:
    authentication redesign

Last completed:
    session persistence

Current blocker:
    migration tests fail

Last decision:
    retain SQLite

Next recommended action:
    investigate migration fixture
```

---

# 90. Context as Shared Working Memory

Continuum therefore becomes a shared working-memory system between:

```text
Human
    ↕
Continuum
    ↕
AI
```

The system is not merely AI memory.

It is **project continuity infrastructure**.

---

# 91. Context Safety

The Context Builder should avoid presenting uncertain information as established fact.

Every significant item should ideally preserve:

```text
source
confidence
authority
freshness
epistemic state
```

---

# 92. Context Conflict Resolution

When conflicting information is encountered, the Context Builder should:

```text
1. Detect conflict.
2. Preserve both sources.
3. Determine authority.
4. Determine freshness.
5. Determine verification.
6. Present the conflict when material.
```

It should not silently discard contradictory evidence.

---

# 93. Context Security

Context Packages may contain:

```text
source code
credentials references
internal architecture
business information
private discussions
security findings
```

Therefore context generation must eventually support access control.

An actor should receive only information they are authorized to access.

---

# 94. Context Boundary

The Context Builder is a security boundary.

Conceptually:

```text
Persistent Project World
          │
          ▼
Authorization
          │
          ▼
Context Selection
          │
          ▼
Context Package
          │
          ▼
Actor
```

---

# 95. Context Auditability

Continuum should eventually record:

```text
who requested context
what purpose was specified
what sources were included
what sources were excluded
what policy was applied
when context was generated
```

This becomes especially important for autonomous agents.

---

# 96. Context Reproducibility

Given identical:

```text
project state
event history
knowledge state
request
selection policy
budget
```

the Context Builder should strive to produce substantially equivalent context.

Perfect byte-for-byte determinism is not required initially.

Semantic reproducibility is the goal.

---

# 97. Context Feedback

After an AI uses context, Continuum may eventually observe whether the context was useful.

Possible signals:

```text
task succeeded
AI requested missing information
AI ignored information
AI repeated known mistake
human corrected AI
AI asked for clarification
```

This can improve future context selection.

---

# 98. Context Learning Loop

The Context Builder can eventually learn:

```text
Context
    ↓
AI Action
    ↓
Outcome
    ↓
Evaluate Context Quality
    ↓
Adjust Retrieval / Ranking
```

This creates an adaptive context system.

---

# 99. Context Quality Metrics

Potential future metrics include:

```text
task success rate
repeated mistake rate
context utilization
context omission rate
retrieval precision
retrieval recall
token efficiency
human correction rate
additional-context request rate
```

---

# 100. Context Quality Principle

The best ContextPackage is not:

> The largest possible package.

It is:

> The smallest package that contains enough trustworthy information for the actor to make good decisions and perform the task safely.

---

# 101. Minimum Context Package

A minimum useful ContextPackage should generally contain:

```text
project identity
current state
current intent
current task
active constraints
relevant requirements
relevant decisions
known blockers
relevant artifacts
relevant recent history
known uncertainty
```

---

# 102. Handoff Minimum

For a new AI entering an existing project, the minimum handoff context should answer:

```text
What is this project?

What are we building?

Where are we now?

What are we currently doing?

Why are we doing it?

What has already been completed?

What has failed?

What decisions have been made?

What constraints exist?

What remains unresolved?

What should happen next?
```

---

# 103. Context Package Example

Conceptually:

```text
CONTINUUM CONTEXT

Project:
    Continuum

Current objective:
    Build persistent AI project continuity infrastructure.

Current task:
    Define Context Model.

Current state:
    Domain model complete.
    Relationship model complete.
    Lifecycle model complete.
    Event model complete.

Important decisions:
    Continuum is standalone.
    Continuum must be AI-agnostic.
    Continuum must be project-agnostic.
    Continuum must not depend on Monad.

Current constraints:
    Must work with any AI-assisted software project.
    Must preserve provenance.
    Must support long-running projects.
    Must avoid requiring entire history in model context.

Recent work:
    Event Model completed.

Known unresolved issues:
    Exact persistence architecture not yet defined.
    Context ranking algorithm not yet defined.

Next action:
    Define Context Model implementation boundaries.
```

---

# 104. Context Package Structure

The conceptual output should resemble:

```text
ContextPackage
│
├── Identity
├── CurrentState
├── Intent
├── Constraints
├── Requirements
├── Decisions
├── CurrentTask
├── RelevantKnowledge
├── RelevantHistory
├── Artifacts
├── Evidence
├── Failures
├── Uncertainty
├── Recommendations
├── Warnings
└── Provenance
```

---

# 105. Context Model Invariants

The following invariants should hold:

```text
1. Context is a projection, not the canonical project state.

2. Context is purpose-bound.

3. Context must preserve provenance.

4. Context must distinguish current state from historical information.

5. Context must preserve uncertainty.

6. Context must identify contradictions when material.

7. Context must respect access boundaries.

8. Context must operate within an explicit budget.

9. Context selection must be explainable.

10. Context should prioritize relevance over recency alone.

11. Context should prioritize authority over confidence alone.

12. Context should preserve important failed attempts.

13. Context should be regenerable.

14. Context should support progressive disclosure.

15. Context should remain useful to both humans and AI agents.

16. Context must not require the entire project history to fit inside one model context window.
```

---

# 106. The Continuum Context Equation

The conceptual model can be summarized as:

```text
Context =
    Current State
  + Current Intent
  + Relevant Knowledge
  + Relevant History
  + Applicable Constraints
  + Applicable Decisions
  + Relevant Artifacts
  + Evidence
  + Uncertainty
  + Actor Needs
  − Irrelevant Information
  − Redundancy
  − Stale Information
  − Unauthorized Information
```

subject to:

```text
Context Budget
```

---

# 107. Context as an Information Bottleneck

The AI context window is an information bottleneck.

Continuum therefore sits upstream of the AI:

```text
             Entire Project
                   │
                   ▼
             Continuum
                   │
        ┌──────────┴──────────┐
        │                     │
    Persistent             Context
      Memory               Builder
        │                     │
        │                     ▼
        │                Context Budget
        │                     │
        └─────────────────────┤
                              ▼
                             AI
```

The Context Builder determines what crosses the bottleneck.

---

# 108. Context Is the Product Boundary

This leads to a critical architectural principle:

> The AI should not need to understand Continuum's entire internal storage model.

The AI should interact with Continuum primarily through:

```text
Context
Query
Event
Action
State
Knowledge
```

This allows Continuum to evolve internally without coupling every AI integration to its persistence architecture.

---

# 109. Final Principle

The purpose of Continuum is not to put an infinite memory into an AI.

The purpose is to give an AI **the right memory at the right moment**.

Therefore:

```text
Memory
    = everything worth retaining

Context
    = what matters now

State
    = what is true now

History
    = how we got here

Knowledge
    = what we understand

Intent
    = what we are trying to accomplish
```

And:

```text
Continuity
=
State
+
History
+
Knowledge
+
Intent
+
Context
```

The Context Model is the mechanism that transforms persistent project memory into usable intelligence.

A future AI should not have to ask:

> "Can you remind me what we were doing?"

Instead, Continuum should be able to answer before the AI asks:

> **"Here is where we are, how we got here, why we're here, what we know, what we don't know, what we've already tried, and what needs to happen next."**
