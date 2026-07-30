# Continuum Context Compilation

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

Context Compilation defines how Continuum transforms project knowledge, current state, retrieved information, task information, and relevant history into a coherent context package that can be supplied to an AI agent.

The central problem is:

> An AI cannot effectively operate on an entire software project at once. Continuum must compile the right project context for the current action.

Context compilation therefore acts as the bridge between:

```text
Project Memory
      ↓
Retrieval
      ↓
Context Compilation
      ↓
AI Agent
      ↓
Action
```

---

# 2. The Core Principle

Continuum should not simply "stuff context into the prompt."

It should compile context deliberately.

The objective is:

> Produce the smallest sufficiently complete, trustworthy, coherent, and actionable representation of project state required for the AI to perform the current task.

---

# 3. Context Is a Compiled Artifact

A context package should be treated as an artifact.

Conceptually:

```text
Context Package
├── identity
├── task
├── project state
├── constraints
├── relevant knowledge
├── decisions
├── artifacts
├── history
├── uncertainties
├── instructions
├── provenance
└── metadata
```

The package should be reproducible from its inputs.

---

# 4. Compilation Pipeline

The conceptual pipeline is:

```text
Task
  ↓
Task Understanding
  ↓
Project State Resolution
  ↓
Retrieval
  ↓
Relevance Ranking
  ↓
Context Planning
  ↓
Context Selection
  ↓
Compression
  ↓
Ordering
  ↓
Instruction Construction
  ↓
Provenance Attachment
  ↓
Validation
  ↓
Context Package
```

---

# 5. Context Compilation Is Not Retrieval

Retrieval answers:

> What information might matter?

Compilation answers:

> What information should the AI actually receive, and in what form?

These are separate responsibilities.

---

# 6. Context Compilation Inputs

A compiler may consume:

```text
task
project identity
repository state
branch
commit
environment
active work item
requirements
constraints
decisions
knowledge
artifacts
events
conversation state
retrieved candidates
agent capabilities
context budget
security policy
```

---

# 7. Context Compilation Outputs

The compiler produces:

```text
ContextPackage
```

containing:

```text
identity
objective
current state
relevant knowledge
constraints
decisions
artifacts
history
uncertainties
instructions
provenance
```

---

# 8. Context Package Identity

Every compiled context should have an identity.

Conceptually:

```text
context_id
project_id
session_id
task_id
compiled_at
compiler_version
```

This allows Continuum to determine exactly what context an AI received.

---

# 9. Reproducibility

Given equivalent inputs, the compiler should be capable of producing substantially equivalent context.

This enables:

```text
debugging
auditing
evaluation
replay
comparison
```

---

# 10. Context Compilation Is Deterministic Where Possible

The compiler should prefer deterministic behavior for:

```text
selection rules
ordering
scope filtering
authority rules
state resolution
provenance
validation
```

AI-generated summarization may be nondeterministic, but the overall compilation process should remain observable and reproducible.

---

# 11. Context Layers

Context should be layered.

A useful conceptual model:

```text
Layer 0 — Identity
Layer 1 — Objective
Layer 2 — Current State
Layer 3 — Constraints
Layer 4 — Decisions
Layer 5 — Knowledge
Layer 6 — Artifacts
Layer 7 — History
Layer 8 — Uncertainty
Layer 9 — Instructions
Layer 10 — Provenance
```

---

# 12. Identity Layer

The identity layer establishes:

```text
what project
what repository
what branch
what commit
what environment
what session
what task
what agent
```

Example:

```text
Project:
    Continuum

Repository:
    continuum

Branch:
    feature/context-compilation

Commit:
    abc123

Environment:
    local-development

Task:
    Define context compilation
```

---

# 13. Objective Layer

The objective layer answers:

> What are we trying to accomplish right now?

It should distinguish:

```text
project objective
current milestone
active work item
immediate task
```

Example:

```text
Project objective:
    Build an AI continuity system.

Milestone:
    Define runtime architecture.

Current task:
    Define context compilation.
```

---

# 14. Current State Layer

Current state should describe:

```text
what exists
what is incomplete
what is active
what is blocked
what recently changed
```

This is one of the most important layers for session continuity.

---

# 15. Constraint Layer

Constraints should be explicit.

Examples:

```text
technical constraints
resource constraints
platform constraints
security constraints
architectural constraints
business constraints
user requirements
```

Constraints should generally be prioritized over preferences.

---

# 16. Decision Layer

Relevant accepted decisions should be surfaced.

For each decision:

```text
decision
rationale
status
authority
date
alternatives
consequences
```

The AI should not be forced to rediscover decisions that have already been made.

---

# 17. Knowledge Layer

Relevant project Knowledge should be included.

Knowledge should preserve epistemic state:

```text
verified
supported
probable
candidate
unknown
disputed
superseded
```

The AI should be able to distinguish established facts from hypotheses.

---

# 18. Artifact Layer

Relevant artifacts may include:

```text
source files
directories
modules
schemas
documents
tests
configuration
interfaces
APIs
designs
```

Artifacts should be referenced precisely.

For source code, prefer:

```text
path
symbol
line range
relationship
purpose
```

over unnecessarily reproducing entire files.

---

# 19. History Layer

History should only be included when it helps the current task.

Useful history includes:

```text
previous attempts
failed approaches
recent changes
relevant decisions
superseded designs
important conversations
```

Historical information should not overwhelm current state.

---

# 20. Uncertainty Layer

The compiler must explicitly represent uncertainty.

Examples:

```text
Unknown:
    whether feature X is still required.

Conflict:
    documentation and implementation disagree.

Hypothesis:
    failure may be caused by configuration Y.

Missing:
    no test currently verifies requirement Z.
```

---

# 21. Instruction Layer

Context may include operational instructions relevant to the current task.

Examples:

```text
do not modify generated files
run tests before committing
preserve public API compatibility
follow architecture rule X
```

These must be distinguishable from project facts.

---

# 22. Provenance Layer

Every important context element should have provenance.

For example:

```text
Source:
    docs/architecture/authentication.md

Origin:
    repository

Observed:
    2026-07-30

Status:
    accepted

Authority:
    project documentation
```

---

# 23. Context Categories

The compiler should distinguish at least:

```text
FACT
DECISION
REQUIREMENT
CONSTRAINT
OBSERVATION
EVENT
ARTIFACT
INSTRUCTION
HYPOTHESIS
QUESTION
UNCERTAINTY
```

This prevents all text from being treated as equivalent.

---

# 24. Context Priority

Not all context is equally important.

A conceptual priority ordering:

```text
critical constraints
        ↓
current task
        ↓
current state
        ↓
accepted decisions
        ↓
required knowledge
        ↓
relevant artifacts
        ↓
supporting history
        ↓
optional background
```

---

# 25. Context Budget

The compiler must operate within a context budget.

The budget may include:

```text
maximum tokens
maximum characters
maximum artifacts
maximum retrieval depth
maximum latency
maximum external calls
```

---

# 26. Budget Allocation

The context budget should not be allocated uniformly.

A conceptual allocation:

```text
Objective + State:
    fixed / mandatory

Constraints:
    high priority

Decisions:
    high priority

Relevant artifacts:
    variable

History:
    variable

Background:
    lowest priority
```

---

# 27. Mandatory Context

Some context should be mandatory.

Examples:

```text
project identity
current task
critical constraints
current repository state
relevant security rules
```

These should not be displaced merely because more semantically similar information exists.

---

# 28. Optional Context

Optional context may include:

```text
historical discussion
related examples
older decisions
background documentation
similar implementations
```

These are candidates for removal when the budget is constrained.

---

# 29. Context Selection

Selection should optimize:

```text
relevance
coverage
authority
freshness
diversity
coherence
token efficiency
```

---

# 30. Context Coverage

The compiler should determine whether the selected context covers the task.

For example:

```text
Task:
    implement feature X

Required context:
    requirement ✓
    constraints ✓
    architecture ✓
    relevant implementation ✓
    tests ✓
    acceptance criteria ✓
```

If important categories are missing, the compiler should retrieve more.

---

# 31. Context Sufficiency

The compiler should eventually estimate:

```text
sufficiency:
    insufficient
    adequate
    strong
```

This should be based on evidence rather than context length.

---

# 32. Context Completeness Is Not Context Size

A larger context does not necessarily mean a better context.

Example:

```text
10,000 tokens of unrelated history
```

may be worse than:

```text
3,000 tokens containing all required facts.
```

The goal is **sufficient completeness**, not maximal volume.

---

# 33. Context Coherence

The final context should form a coherent model.

The compiler should avoid producing:

```text
current architecture:
    PostgreSQL

historical architecture:
    Redis

superseded architecture:
    SQLite

```

without clearly explaining which is current.

---

# 34. Temporal Resolution

The compiler must distinguish:

```text
current
historical
future
superseded
planned
```

This prevents stale information from being presented as current truth.

---

# 35. State Resolution

If multiple records describe the same subject, the compiler should resolve them according to:

```text
authority
status
freshness
scope
timestamp
supersession
```

---

# 36. Contradiction Handling

When contradictions remain unresolved, the compiler should surface them.

Example:

```text
CONFLICT

Source A:
    authentication uses sessions.

Source B:
    authentication uses JWT.

Resolution:
    unknown.

Action:
    inspect current implementation.
```

---

# 37. No Silent Conflict Resolution

The compiler must not silently choose one conflicting statement and discard the other merely to make the context cleaner.

This can create false project truth.

---

# 38. Context Compression

Compression may be necessary.

However:

> Compression must preserve information necessary for correct reasoning.

Compression should prioritize preserving:

```text
constraints
decisions
causal relationships
uncertainties
negative knowledge
current state
provenance
```

---

# 39. Compression Hierarchy

Compression should occur progressively:

```text
Remove duplicates
      ↓
Remove low-value detail
      ↓
Collapse related observations
      ↓
Summarize history
      ↓
Summarize supporting artifacts
```

Critical information should be compressed last.

---

# 40. Source Preservation

Compression must never destroy source references.

Example:

```text
Summary:
    Authentication was migrated to JWT.

Sources:
    ADR-014
    commit abc123
    src/auth/token.ts
```

---

# 41. Context Ordering

Context should generally be presented in reasoning order.

A recommended ordering:

```text
1. Identity
2. Objective
3. Current state
4. Constraints
5. Requirements
6. Decisions
7. Relevant knowledge
8. Relevant artifacts
9. History
10. Uncertainties
11. Instructions
12. Provenance
```

---

# 42. Current State Before History

Current state should normally precede history.

This prevents the AI from anchoring on obsolete information.

---

# 43. Decisions Before Alternatives

When a decision has already been accepted:

```text
accepted decision
```

should normally appear before:

```text
historical alternatives
```

This helps the AI understand what is already settled.

---

# 44. Constraints Before Design

When asking an AI to design or implement something:

```text
constraints
```

should appear before:

```text
possible solutions
```

The AI should reason within the project boundaries rather than rediscovering them.

---

# 45. Context as a Model

The final context should not be treated as a random collection of documents.

It should represent a model:

```text
PROJECT
   │
   ├── Objective
   ├── State
   ├── Constraints
   ├── Requirements
   ├── Decisions
   ├── Architecture
   ├── Artifacts
   ├── History
   └── Uncertainty
```

---

# 46. Context Graph

The compiler may internally construct a temporary context graph.

Example:

```text
Task
 │
 ├── requires → Requirement
 │                 │
 │                 └── constrained-by → Constraint
 │
 ├── affects → Component
 │                │
 │                └── implemented-by → Artifact
 │
 └── governed-by → Decision
```

This graph can guide selection.

---

# 47. Context Compilation Strategies

Continuum should support multiple strategies.

Potential strategies:

```text
minimal
balanced
deep
debug
historical
architectural
implementation
review
resume
```

---

# 48. Minimal Strategy

The minimal strategy should provide only the essential context.

Useful when:

```text
task is simple
context budget is small
latency is important
```

---

# 49. Balanced Strategy

Balanced should be the default.

It should include:

```text
objective
state
constraints
decisions
relevant artifacts
limited history
uncertainties
```

---

# 50. Deep Strategy

Deep context may include:

```text
broader architecture
related subsystems
historical decisions
previous attempts
dependency chains
similar implementations
```

Useful for difficult tasks.

---

# 51. Debug Strategy

Debug context should prioritize:

```text
failure
symptoms
recent changes
logs
tests
dependencies
previous attempts
known causes
environment
```

---

# 52. Resume Strategy

Resume context should prioritize:

```text
what we were doing
where we stopped
what changed
what remains
what was planned next
what is blocked
```

---

# 53. Historical Strategy

Historical context should prioritize:

```text
timeline
past decisions
previous implementations
superseded designs
past attempts
rejected alternatives
```

---

# 54. Architectural Strategy

Architectural context should prioritize:

```text
system boundaries
components
interfaces
dependencies
constraints
architecture decisions
patterns
tradeoffs
```

---

# 55. Implementation Strategy

Implementation context should prioritize:

```text
requirements
acceptance criteria
target artifacts
related source
interfaces
tests
architecture rules
relevant decisions
```

---

# 56. Review Strategy

Review context should prioritize:

```text
changed artifacts
requirements
acceptance criteria
architecture
security constraints
tests
known risks
```

---

# 57. Context Profiles

A context profile may define:

```text
strategy
required categories
preferred categories
excluded categories
budget
compression policy
retrieval depth
```

---

# 58. Agent-Specific Context

Different AI agents may require different context.

For example:

```text
planner
coder
reviewer
tester
architect
documentation agent
```

The compiler should eventually support agent profiles.

---

# 59. Planner Context

Planner context might emphasize:

```text
goal
requirements
constraints
architecture
dependencies
risks
decisions
```

---

# 60. Coding Agent Context

Coding context might emphasize:

```text
target files
symbols
interfaces
tests
architecture constraints
implementation patterns
recent changes
```

---

# 61. Review Agent Context

Review context might emphasize:

```text
diff
requirements
acceptance criteria
architecture rules
security constraints
tests
known risks
```

---

# 62. Testing Agent Context

Testing context might emphasize:

```text
requirements
expected behavior
test conventions
target implementation
existing tests
known failure modes
```

---

# 63. Context Instructions

Context should clearly distinguish project information from instructions to the AI.

For example:

```text
PROJECT FACT:
    PostgreSQL is the persistence layer.

TASK:
    Add user-session persistence.

INSTRUCTION:
    Do not introduce another database.
```

These should not be conflated.

---

# 64. Instruction Precedence

Context compilation should eventually support explicit precedence.

For example:

```text
system policy
    >
security policy
    >
project governance
    >
task constraints
    >
agent preferences
    >
historical suggestions
```

The exact hierarchy is to be defined separately.

---

# 65. Context Injection Safety

Retrieved text may contain instructions.

The compiler must distinguish:

```text
information
```

from:

```text
instructions
```

A document saying:

```text
"Ignore all previous instructions..."
```

must not automatically become an instruction to the AI.

---

# 66. Provenance and Trust

Every externally sourced context item should carry provenance.

Example:

```text
source:
    external documentation

trust:
    unverified

role:
    reference material
```

This protects against untrusted content being treated as project truth.

---

# 67. Context Package Schema

The eventual schema should conceptually resemble:

```text
ContextPackage
├── metadata
├── identity
├── task
├── state
├── constraints
├── requirements
├── decisions
├── knowledge
├── artifacts
├── history
├── uncertainties
├── instructions
└── provenance
```

The formal schema will be defined later.

---

# 68. Context Metadata

Metadata may include:

```text
context_id
project_id
session_id
task_id
agent_id
compiled_at
compiler_version
strategy
budget
retrieval_version
```

---

# 69. Context Fingerprinting

A compiled context should eventually have a fingerprint.

Conceptually:

```text
context_fingerprint =
    hash(normalized_context)
```

This permits:

```text
comparison
deduplication
replay
cache lookup
evaluation
```

---

# 70. Context Caching

Compiled contexts may be cached.

Useful cases:

```text
same task
same repository state
same branch
same relevant knowledge
same context profile
```

However, cache invalidation must account for project changes.

---

# 71. Cache Invalidation

A context should become stale when relevant inputs change.

Potential invalidation events:

```text
new commit
new decision
requirement change
constraint change
artifact change
new failure
new work item
knowledge superseded
```

---

# 72. Context Delta

Continuum should eventually support context deltas.

Instead of rebuilding everything:

```text
Context A
    ↓
small project change
    ↓
Context Delta
    ↓
Context B
```

This can improve efficiency.

---

# 73. Incremental Compilation

Context compilation should eventually support incremental updates.

Example:

```text
initial context
      ↓
AI edits file
      ↓
file state changes
      ↓
recompile affected context
```

Only affected portions need to be reconsidered.

---

# 74. Context Compilation and Events

Compilation should be event-aware.

Example:

```text
CommitCreated
DecisionAccepted
TestFailed
RequirementChanged
ArtifactModified
TaskCompleted
```

may invalidate or alter context.

---

# 75. Context Compilation and Work Sessions

A work session can produce:

```text
context
actions
observations
decisions
artifacts
results
```

At the end of the session, these become new project state.

The next session can then compile from that updated state.

---

# 76. The Continuity Loop

This creates a fundamental loop:

```text
                PROJECT STATE
                     │
                     ▼
              CONTEXT COMPILER
                     │
                     ▼
                    AI
                     │
                     ▼
                   ACTION
                     │
                     ▼
                OBSERVATIONS
                     │
                     ▼
              PROJECT MEMORY
                     │
                     └───────────────┐
                                     ▼
                              CONTEXT COMPILER
```

This is the core continuity mechanism.

---

# 77. Context Compilation Is a Control Plane

The compiler determines what information the AI can reason over.

Therefore it is not merely formatting.

It is a control-plane function governing:

```text
knowledge access
project continuity
epistemic boundaries
task focus
agent capability
context budget
```

---

# 78. Context Compilation Must Be Observable

Continuum should be able to answer:

```text
What context did the AI receive?

Why was this information included?

Why was this information excluded?

What retrieval queries were performed?

What information was compressed?

What contradictions existed?

What was unknown?
```

---

# 79. Context Compilation Trace

A compilation trace should eventually include:

```text
task interpretation
retrieval operations
candidate set
ranking
selection
compression
conflict resolution
final package
```

This becomes essential for debugging AI behavior.

---

# 80. Context-to-Action Traceability

Eventually Continuum should support:

```text
Context
   ↓
AI reasoning
   ↓
Decision
   ↓
Action
   ↓
Result
```

This creates a traceable relationship between what the AI knew and what it did.

---

# 81. Context Evaluation

Context quality should eventually be evaluated independently from model quality.

For example:

```text
Was the correct requirement present?
Was the relevant decision present?
Was the current source retrieved?
Was stale information excluded?
Was the critical constraint included?
Was contradictory information surfaced?
```

---

# 82. Context Quality Metrics

Potential metrics:

```text
context relevance
context coverage
context sufficiency
context freshness
context authority
context redundancy
context compression ratio
context token efficiency
context contradiction rate
context retrieval latency
```

---

# 83. Context Failure Modes

Continuum should explicitly recognize:

```text
missing context
stale context
irrelevant context
contradictory context
overloaded context
under-specified context
mis-scoped context
untrusted context
incorrectly summarized context
```

---

# 84. Context Failure Should Be Visible

If compilation fails to establish sufficient context:

```text
CONTEXT INSUFFICIENT

Missing:
    current authentication implementation

Reason:
    repository state unavailable

Recommended action:
    synchronize repository
```

The system should not fabricate missing information.

---

# 85. Context Compilation and Human Interaction

Humans should eventually be able to inspect and modify context.

For example:

```text
Context Preview

Included:
    ✓ ADR-014
    ✓ src/auth/token.ts
    ✓ requirement R-22

Excluded:
    ✗ old Redis architecture
    ✗ unrelated billing service

Uncertainty:
    ? session persistence requirements unclear
```

This creates human oversight.

---

# 86. Context Override

A user may intentionally request additional context.

Example:

```text
"Show me the old Redis implementation too."
```

The compiler should allow explicit expansion.

---

# 87. Context Exclusion

A user may also explicitly exclude context.

Example:

```text
"Ignore the old authentication experiment."
```

Such exclusions should be scoped appropriately rather than deleting historical Knowledge.

---

# 88. Context Does Not Mutate Knowledge

Compilation is read-oriented.

It should not alter project truth merely because something was included or excluded from a context.

---

# 89. Compilation Is a Projection

The project Knowledge model is the underlying state.

Context is a projection of that state.

```text
Project Knowledge
        │
        ▼
   Context Compiler
        │
        ▼
Context Projection
```

Different tasks produce different projections of the same project.

---

# 90. Multiple Contexts From One Project

For the same project:

```text
Planner Context
Coder Context
Reviewer Context
Tester Context
Human Context
```

may all differ while representing the same underlying project.

---

# 91. Context Is Task-Relative

There is no single universally correct project context.

There is:

> the context appropriate to a particular task, agent, state, and moment.

---

# 92. Context Compilation Contract

The compiler should eventually satisfy:

```text
INPUT:
    project state
    task
    retrieval results
    context profile
    agent capabilities
    budget

OUTPUT:
    coherent context package
    provenance
    compilation trace
    sufficiency assessment
```

---

# 93. The Context Compiler

The eventual runtime component can be conceptualized as:

```text
ContextCompiler
├── TaskResolver
├── StateResolver
├── RetrievalCoordinator
├── RelevanceEngine
├── ContextSelector
├── ConflictResolver
├── CompressionEngine
├── ContextAssembler
├── ProvenanceManager
└── Validator
```

These are conceptual responsibilities, not yet implementation modules.

---

# 94. Context Compiler Invariants

The compiler should preserve the following invariants:

```text
1. Current state must not be confused with historical state.

2. Facts must not be silently transformed into instructions.

3. Uncertainty must not become certainty.

4. Superseded decisions must not appear current without qualification.

5. Provenance must survive compression.

6. Security boundaries must survive retrieval and compilation.

7. Context must remain traceable to project state.

8. Missing information must not be fabricated.

9. Critical constraints must not be displaced by low-value context.

10. Context selection must be explainable.
```

---

# 95. Context Compiler Objective

The optimization problem can be expressed conceptually as:

```text
maximize:

    relevance
  + coverage
  + authority
  + freshness
  + coherence
  + diversity
  + task usefulness

while minimizing:

    tokens
  + latency
  + redundancy
  + uncertainty
  + contradiction
  + irrelevant information
```

The exact optimization algorithm is intentionally deferred.

---

# 96. Context Compilation and AI Independence

Continuum should not depend on a particular AI model.

The compiler should produce a model-neutral representation that can be adapted to:

```text
OpenAI
Anthropic
Google
local models
open-source models
agent frameworks
IDE assistants
CLI agents
custom agents
```

---

# 97. Model Adapter

A separate adapter layer should eventually transform:

```text
Continuum ContextPackage
```

into the input format required by a particular AI.

Conceptually:

```text
Continuum Context
        │
        ▼
   Model Adapter
        │
   ┌────┼─────┐
   ▼    ▼     ▼
 Model A B     C
```

The context compiler should not become coupled to model-specific prompt syntax.

---

# 98. Context Compilation and Prompt Engineering

Prompt engineering should be downstream of context compilation.

The distinction is:

```text
Continuum:
    What should the AI know?

Prompt / Adapter:
    How should that information be presented to this model?
```

---

# 99. Context Compilation Is More Fundamental Than Prompt Engineering

Prompt templates can change frequently.

Project truth and context selection should remain independent of those templates.

This allows Continuum to remain useful as AI models evolve.

---

# 100. Final Principle

The ultimate purpose of Context Compilation is not to create a larger prompt.

It is to create a **faithful working model of the project** for an AI at a particular moment.

The system should transform:

```text
everything Continuum knows
```

into:

```text
what this AI needs to know
```

for:

```text
this task
```

at:

```text
this moment
```

under:

```text
these constraints
```

with:

```text
this level of certainty
```

and with:

```text
traceable provenance.
```

Therefore:

> **Continuum is not merely a memory system. It is a context compiler for persistent AI-assisted work.**
