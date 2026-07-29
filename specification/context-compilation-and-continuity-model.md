# Continuum Context Compilation & Continuity Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

# 1. Purpose

The Context Compilation & Continuity Model defines how Continuum transforms persistent project information into an actionable Context Package for an AI system.

The central problem is:

> Given everything Continuum knows about a software project, what does an AI need to know right now to work effectively?

Continuum must solve this without requiring the AI to ingest the entire project history, repository, conversation history, or knowledge base.

The model therefore establishes:

```text
Persistent Project Memory
        ↓
Current Situation
        ↓
Context Selection
        ↓
Context Compilation
        ↓
Context Package
        ↓
AI
        ↓
Result
        ↓
Knowledge / State Update
```

---

# 2. Fundamental Principle

Continuum should not attempt to give an AI everything it knows.

It should provide:

> The smallest sufficiently complete representation of project reality required for the AI to perform the current task correctly.

This creates two simultaneous objectives:

```text
maximize relevance
maximize sufficiency
```

while minimizing:

```text
irrelevance
redundancy
staleness
contradiction
token cost
cognitive load
```

---

# 3. Context Is Compiled

Context should be treated as a compiled artifact.

Conceptually:

```text
PROJECT MEMORY
      │
      ├── Knowledge
      ├── Artifacts
      ├── Requirements
      ├── Decisions
      ├── Work
      ├── Events
      ├── State
      └── Constraints
              │
              ▼
        CONTEXT COMPILER
              │
              ▼
       CONTEXT PACKAGE
              │
              ▼
             AI
```

The Context Package is therefore analogous to a compiled program:

```text
source information → compilation → executable context
```

---

# 4. Context Package

A Context Package is a bounded, provenance-aware representation of project information intended for consumption by an AI system.

It should answer:

```text
What is this project?
What are we doing?
Why are we doing it?
What has already happened?
What is true now?
What decisions constrain us?
What does the AI need to know?
What should the AI do?
What should the AI avoid?
```

---

# 5. Context Package Identity

Every Context Package should have stable identity.

Conceptually:

```text
ContextPackage
├── context_id
├── project_id
├── work_id
├── session_id
├── created_at
├── compiler_version
├── source_state
├── selection_policy
├── contents
├── provenance
├── constraints
└── validation
```

---

# 6. Context Compilation

Context compilation is the transformation:

```text
Source State
     +
Task
     +
Selection Policy
     +
Token / Resource Budget
     ↓
Context Compiler
     ↓
Context Package
```

The compiler should be deterministic or reproducible where possible.

---

# 7. Context Compilation Pipeline

The conceptual pipeline is:

```text
1. Identify Objective
2. Establish Current State
3. Discover Relevant Objects
4. Retrieve Relevant Knowledge
5. Retrieve Relevant History
6. Resolve Conflicts
7. Evaluate Freshness
8. Rank Information
9. Remove Redundancy
10. Compress Where Safe
11. Assemble Context
12. Validate Context
13. Produce Context Package
```

---

# 8. Step 1 — Identify Objective

The compiler must first determine what the AI is being asked to accomplish.

Examples:

```text
implement authentication
debug failing test
design database schema
review pull request
refactor module
write documentation
investigate architecture
plan next milestone
```

The objective becomes the primary relevance signal.

---

# 9. Objective Representation

An Objective should ideally include:

```text
objective
desired outcome
scope
constraints
success criteria
known dependencies
priority
```

Example:

```text
Objective:
    Fix authentication test failures.

Desired outcome:
    Full authentication test suite passes.

Scope:
    auth service and related tests.

Constraint:
    Do not change public API.
```

---

# 10. Step 2 — Establish Current State

Before retrieving context, Continuum should determine the project's current state.

Potential state inputs:

```text
repository revision
working tree
active branch
build status
test status
deployment status
open work
recent changes
active requirements
active decisions
known blockers
```

The compiler should prefer current state over stale historical state.

---

# 11. Temporal Relevance

Information should be evaluated against time.

A fact may be:

```text
current
recent
historical
superseded
stale
unknown
```

Example:

```text
Architecture Decision:
    PostgreSQL is required.

Later:
    Decision superseded by DEC-032.
```

The old decision remains historical information but should not normally enter a current implementation Context Package as an active constraint.

---

# 12. Step 3 — Discover Relevant Objects

The compiler should identify objects related to the Objective.

Potential objects:

```text
requirements
work items
artifacts
files
directories
symbols
modules
services
decisions
constraints
tests
issues
commits
deployments
documentation
knowledge claims
```

Relevance should be relationship-aware.

---

# 13. Relationship-Based Retrieval

Relevant context is not limited to direct keyword matches.

Example:

```text
Objective:
    Fix login failure

Relevant:
    login service
       ↓
    token validator
       ↓
    authentication configuration
       ↓
    failing test
       ↓
    recent commit
       ↓
    architecture decision
```

The compiler should follow meaningful relationships through the project graph.

---

# 14. Step 4 — Retrieve Relevant Knowledge

Knowledge retrieval should consider:

```text
topic
semantic similarity
object relationships
causal relationships
task relevance
recency
confidence
authority
source quality
current state
```

The system should prefer authoritative project Knowledge over unverified conversational statements.

---

# 15. Knowledge Priority

A possible conceptual priority order:

```text
1. Current authoritative project state
2. Active requirements
3. Accepted decisions
4. High-confidence verified facts
5. Relevant evidence
6. Recent observations
7. Historical information
8. Unverified claims
9. Speculation
```

This is a starting model, not a final ranking algorithm.

---

# 16. Step 5 — Retrieve Relevant History

History should be retrieved when it helps explain current state.

Examples:

```text
Why was this architecture chosen?
Why does this workaround exist?
What was tried already?
Why was this dependency rejected?
When did this regression begin?
What caused this file to change?
```

History should be included because it provides rationale and prevents repeated mistakes.

---

# 17. History Compression

Historical information may be compressed when its details are not directly relevant.

For example:

```text
Raw history:
    27 interactions
    14 tool calls
    9 file modifications
    4 failed approaches
    3 successful experiments
```

may become:

```text
Previous attempts:
    Approaches A and B failed because X.
    Approach C succeeded under condition Y.
```

The underlying history remains available.

---

# 18. Step 6 — Resolve Conflicts

Context may contain conflicting information.

Example:

```text
Claim A:
    Service uses Redis.

Claim B:
    Service migrated to PostgreSQL.

Repository:
    Current configuration uses PostgreSQL.
```

The compiler should not simply merge these statements.

Instead it should classify them:

```text
A = historical
B = current
Repository evidence = supporting evidence
```

---

# 19. Conflict Representation

The Context Package should preserve unresolved conflicts where necessary.

Example:

```text
CONFLICT
---------
Subject:
    authentication provider

Position A:
    Provider X

Position B:
    Provider Y

Evidence:
    ...

Resolution:
    unresolved
```

The AI should be told when uncertainty exists.

---

# 20. Step 7 — Evaluate Freshness

Each selected item should have an effective freshness.

Potential factors:

```text
age
source
last verification
related changes
dependency changes
state changes
supersession
```

A source file modified yesterday may be more relevant than documentation written six months ago.

---

# 21. Freshness vs Truth

Freshness does not equal truth.

A recent statement may be incorrect.

Therefore:

```text
freshness
    ≠
confidence
    ≠
authority
```

These dimensions should remain separate.

---

# 22. Step 8 — Rank Information

Candidate information should be ranked.

Potential ranking dimensions:

```text
task relevance
dependency relevance
semantic relevance
structural relevance
temporal relevance
authority
confidence
freshness
causal relevance
explicit user priority
```

The ranking algorithm should remain replaceable.

---

# 23. Context Relevance Score

A conceptual relevance function may be:

```text
R(item, objective) =
    semantic relevance
  + structural relevance
  + temporal relevance
  + causal relevance
  + authority
  + confidence
  + explicit priority
  - staleness
  - redundancy
```

The exact mathematical implementation is deliberately unspecified.

---

# 24. Step 9 — Remove Redundancy

Multiple sources may communicate the same fact.

Example:

```text
README:
    PostgreSQL is used.

Architecture document:
    PostgreSQL is used.

Decision:
    PostgreSQL selected.

Source configuration:
    PostgreSQL connection exists.
```

The compiler should avoid consuming unnecessary context space while retaining sufficient evidence and provenance.

---

# 25. Evidence Compression

Redundant evidence may be represented as:

```text
Claim:
    PostgreSQL is the active persistence layer.

Evidence:
    DEC-012
    architecture.md
    database configuration
```

This preserves confidence and provenance while reducing context volume.

---

# 26. Step 10 — Compression

Context compression may operate at multiple levels.

```text
Level 0:
    raw artifact

Level 1:
    extracted structure

Level 2:
    summary

Level 3:
    semantic representation

Level 4:
    claim / fact

Level 5:
    compact state representation
```

The compiler should select the least expensive representation that preserves task-relevant meaning.

---

# 27. Lossless vs Lossy Compression

Compression should be classified as:

```text
lossless
loss-aware
lossy
```

For critical information, lossless representation should be preferred.

For historical details, lossy summarization may be acceptable when the underlying source remains retrievable.

---

# 28. Context Budget

AI systems impose limits.

Continuum should treat context as a resource budget.

Potential dimensions:

```text
tokens
characters
bytes
latency
cost
model capacity
attention budget
```

The compiler should produce Context Packages appropriate for the target AI.

---

# 29. Context Budget Allocation

A Context Package may allocate its budget approximately among:

```text
identity / orientation
objective
current state
constraints
decisions
relevant knowledge
relevant artifacts
recent history
instructions
uncertainties
```

The exact allocation should be dynamic.

---

# 30. Context Should Be Layered

Context should not be treated as one undifferentiated blob.

A conceptual structure:

```text
LAYER 0 — Orientation
    project identity

LAYER 1 — Objective
    what needs to happen

LAYER 2 — Current State
    what is true now

LAYER 3 — Constraints
    what cannot be violated

LAYER 4 — Knowledge
    what is known

LAYER 5 — Decisions
    what has been decided

LAYER 6 — Relevant Artifacts
    what must be inspected

LAYER 7 — History
    what explains the current situation

LAYER 8 — Uncertainty
    what is unknown or disputed

LAYER 9 — Instructions
    what the AI should do
```

---

# 31. Orientation Context

Orientation should allow a new AI to understand the project quickly.

Potential contents:

```text
project name
project purpose
system type
architecture overview
technology stack
repository layout
current milestone
current work
important conventions
```

Orientation should be concise.

---

# 32. Objective Context

The Objective layer should explicitly state:

```text
what we are doing
why we are doing it
what success looks like
what is in scope
what is out of scope
```

This prevents the AI from solving the wrong problem.

---

# 33. Current-State Context

Current State should answer:

```text
Where are we now?
```

Examples:

```text
branch
revision
working tree state
build state
test state
deployment state
current work item
known blockers
```

---

# 34. Constraint Context

Constraints should be explicitly surfaced.

Examples:

```text
must use Rust
cannot introduce external service
must preserve API compatibility
must run on Linux
must support offline development
must stay within memory budget
```

Constraints should be prioritized.

---

# 35. Decision Context

The AI should receive decisions relevant to its current task.

Example:

```text
DEC-014
Authentication uses OAuth 2.0.

Status:
    accepted

Rationale:
    ...

Do not:
    introduce password-based authentication.
```

---

# 36. Artifact Context

Artifact context should be selective.

Instead of:

```text
entire repository
```

the compiler should provide:

```text
relevant files
relevant modules
relevant symbols
dependency paths
interfaces
configuration
tests
```

The AI may then request additional artifacts.

---

# 37. History Context

History should answer relevant questions such as:

```text
What was already tried?
Why did we choose this?
What failed?
What changed recently?
```

History should not become an automatic transcript dump.

---

# 38. Uncertainty Context

Uncertainty should be explicit.

Examples:

```text
UNKNOWN:
    root cause of memory leak

CONFLICT:
    documentation disagrees with configuration

LOW CONFIDENCE:
    hypothesis that cache invalidation causes failure
```

This prevents false certainty.

---

# 39. Instruction Context

Instructions define what the AI should do with the Context Package.

Examples:

```text
inspect before modifying
do not change public API
run tests after changes
ask before changing architecture
prefer existing abstractions
```

Instructions may come from:

```text
user
project policy
work item
organization
system configuration
```

Priority must be preserved.

---

# 40. Context Priority

Conflicting instructions require precedence.

Conceptually:

```text
higher authority
      ↓
system constraints
project governance
work constraints
user objective
task instructions
AI-generated suggestions
      ↓
lower authority
```

The exact hierarchy must eventually be formalized.

---

# 41. Context Freshness

A Context Package should have a freshness state.

Potential states:

```text
fresh
aging
stale
invalid
```

A Context Package becomes stale when significant source information changes.

---

# 42. Context Invalidation

Context should be invalidated when relevant source state changes.

Examples:

```text
architecture decision changed
relevant file modified
requirement changed
branch changed
dependency changed
test result changed
work item completed
```

Not every project change should invalidate every Context Package.

Invalidation should be dependency-aware.

---

# 43. Context Dependency Graph

Conceptually:

```text
Context Package
      │
      ├── Requirement R1
      ├── Decision D4
      ├── File F17
      ├── Test T9
      └── State S3
```

If:

```text
Decision D4 changes
```

Continuum can identify:

```text
Context Package
      ↓
potentially stale
```

This is superior to invalidating all context globally.

---

# 44. Context Sufficiency

Relevance alone is insufficient.

A Context Package must also be sufficiently complete for the task.

For example:

```text
Relevant:
    authentication service

Missing:
    authentication configuration
```

The context may be highly relevant but insufficient.

---

# 45. Sufficiency Evaluation

Continuum should eventually estimate:

```text
Can the AI reasonably perform this task with the selected context?
```

Potential outputs:

```text
sufficient
probably sufficient
insufficient
unknown
```

---

# 46. Missing Context

When context is insufficient, Continuum should identify what is missing.

Example:

```text
Missing:
    OAuth configuration

Reason:
    required to determine provider behavior

Recommended retrieval:
    config/auth.yaml
```

The AI should be able to request additional context.

---

# 47. Context Expansion

Context should be dynamically expandable.

```text
Initial Context
      ↓
AI identifies missing information
      ↓
Context Request
      ↓
Continuum Retrieval
      ↓
Context Expansion
      ↓
AI continues
```

This creates an iterative context loop.

---

# 48. Context Refinement

The AI may discover that the initial objective was incomplete or incorrectly framed.

Continuum should support:

```text
Initial Objective
      ↓
Investigation
      ↓
Refined Objective
      ↓
New Context Compilation
```

The original objective should remain historically preserved.

---

# 49. Context Feedback

The system should learn from context outcomes.

Potential feedback:

```text
AI requested missing artifact
AI ignored irrelevant information
AI identified contradiction
AI produced correct solution
AI produced incorrect solution
AI lacked critical context
AI received excessive context
```

These observations can improve future compilation.

---

# 50. Context Quality

Context quality should eventually be evaluated along multiple dimensions:

```text
relevance
sufficiency
accuracy
freshness
consistency
provenance
compactness
actionability
```

A single "context quality" score should not replace these dimensions.

---

# 51. Context Evaluation

After an AI Activity, Continuum may evaluate:

```text
Was the context sufficient?
Was relevant information missing?
Was irrelevant information included?
Were contradictions surfaced?
Did the AI misuse or misunderstand context?
```

This creates a feedback loop:

```text
Context
   ↓
AI
   ↓
Outcome
   ↓
Evaluation
   ↓
Compiler Improvement
```

---

# 52. Context Package Provenance

Every Context Package should retain source references.

Example:

```text
Context Item:
    PostgreSQL is the persistence layer.

Sources:
    DEC-012
    architecture.md
    config/database.yaml
```

The AI-facing representation may be compact while the internal representation remains fully traceable.

---

# 53. Context Reproducibility

Continuum should eventually be able to answer:

> What exact context was given to the AI when this decision was made?

This requires recording:

```text
source state
compiler version
selection policy
context contents
model target
timestamp
```

---

# 54. Context Determinism

Given equivalent inputs, Continuum should strive for reproducible context compilation.

Conceptually:

```text
Same Project State
+
Same Objective
+
Same Policy
+
Same Compiler Version
        ↓
Equivalent Context Package
```

Pure determinism may not always be possible when semantic retrieval or external services are involved.

---

# 55. Context Versioning

Context compilation should produce version information.

Potential identifiers:

```text
compiler_version
policy_version
context_schema_version
source_revision
```

This permits historical reconstruction.

---

# 56. Context Caching

Compiled Context Packages may be cached when source dependencies remain unchanged.

Caching may reduce:

```text
latency
cost
computation
retrieval overhead
```

Cache invalidation must respect the Context Dependency Graph.

---

# 57. Context Reuse

A Context Package may be reused across multiple AI interactions if:

```text
objective remains compatible
source state remains sufficiently current
constraints remain valid
context remains sufficient
```

However, reuse must not become an excuse for stale context.

---

# 58. Context Handoff

Context Packages are the primary mechanism for Session handoff.

```text
Session A
    ↓
Project Knowledge Update
    ↓
Context Compilation
    ↓
Handoff Package
    ↓
Session B
```

Session B does not need Session A's entire conversation.

---

# 59. Context Across AI Providers

A Context Package should be provider-neutral.

Continuum may transform it into provider-specific forms:

```text
Canonical Context
      │
      ├── OpenAI adapter
      ├── Anthropic adapter
      ├── local-model adapter
      ├── IDE adapter
      └── agent adapter
```

The canonical information remains independent of provider prompt conventions.

---

# 60. Context Across Models

Different models may require different context packaging.

For example:

```text
Model A:
    larger context window

Model B:
    smaller context window

Model C:
    stronger retrieval ability
```

Continuum should adapt packaging without changing underlying project Knowledge.

---

# 61. Context Across Modalities

Future Context Packages may contain:

```text
text
code
structured data
images
diagrams
logs
audio
video
metrics
```

The conceptual model should support multimodal context without assuming text-only operation.

---

# 62. Context and Tool Use

Context compilation should account for what the AI can retrieve itself.

If the AI has direct repository access:

```text
Context:
    architecture overview
    relevant files
    key constraints
```

rather than:

```text
entire repository contents
```

Tool capability therefore influences context requirements.

---

# 63. Context and Agentic Systems

For autonomous agents, Context Compilation may happen repeatedly.

```text
Objective
   ↓
Context
   ↓
Agent action
   ↓
Observation
   ↓
State update
   ↓
Context recompilation
   ↓
Next action
```

Continuum therefore becomes part of the agent's working memory architecture.

---

# 64. Context Control Loop

The complete control loop is:

```text
              ┌──────────────────────┐
              │   Persistent Memory   │
              └──────────┬───────────┘
                         │
                         ▼
                  Context Compiler
                         │
                         ▼
                   Context Package
                         │
                         ▼
                         AI
                         │
                         ▼
                     Action
                         │
                         ▼
                     Events
                         │
                         ▼
                  State / Knowledge
                         │
                         └───────────────┐
                                         │
                                         ▼
                                  Context Compiler
```

This loop is the core operating mechanism of Continuum.

---

# 65. Context as an Artifact

A Context Package should itself be treated as an Artifact.

This enables:

```text
identity
versioning
provenance
comparison
storage
retrieval
audit
reproduction
evaluation
```

---

# 66. Context as a First-Class Object

Context should not merely be an ephemeral string passed to an AI API.

It should be a first-class Continuum object.

This enables questions such as:

```text
What context was used?
Why was this information included?
What was omitted?
What project state did it represent?
Was it stale?
Did it contain contradictions?
Did it lead to a successful result?
```

---

# 67. Context Diff

Continuum should eventually support comparing Context Packages.

Example:

```text
Context A
    repository revision abc123

Context B
    repository revision def456
```

Diff:

```text
Added:
    DEC-019
    auth/config.ts

Removed:
    DEC-014

Changed:
    authentication architecture
```

This is useful for debugging AI continuity.

---

# 68. Context Audit

A Context Audit should eventually answer:

```text
Why did the AI receive this information?
Why was that information omitted?
Which policy selected it?
Which sources supported it?
What information was stale?
What contradictions existed?
```

This is important for trustworthy AI-assisted engineering.

---

# 69. Context Failure Modes

Continuum should recognize common failure modes:

```text
context too small
context too large
context stale
context contradictory
context incomplete
context irrelevant
context misleading
context over-compressed
context under-compressed
context provenance missing
context incorrectly prioritized
```

These should eventually become measurable failure classes.

---

# 70. Context Recovery

When a Session fails because of inadequate context:

```text
AI Failure
   ↓
Context Evaluation
   ↓
Missing Information
   ↓
Context Expansion
   ↓
Retry
```

The failure should become useful information rather than simply another failed conversation.

---

# 71. Context and Human Judgment

The human remains capable of overriding compilation.

For example:

```text
Compiler selected:
    17 artifacts

Human:
    Include architecture history.

Compiler:
    Adds historical decision chain.
```

Human context directives should be recorded.

---

# 72. Context Requests

An AI should eventually be able to express requests such as:

```text
"I need the history of this module."

"Show me the decision that established this API."

"I need the current deployment configuration."

"What changed since the last successful build?"
```

These requests should map to structured Continuum retrieval operations.

---

# 73. Context Query

A Context Query can conceptually contain:

```text
query
purpose
scope
required confidence
freshness requirement
budget
urgency
```

Example:

```text
Purpose:
    determine why authentication broke

Scope:
    authentication subsystem

Freshness:
    current

Required confidence:
    high
```

---

# 74. Context Retrieval Loop

A complete AI interaction may therefore be:

```text
AI receives Context
       ↓
AI determines missing information
       ↓
AI issues Context Query
       ↓
Continuum retrieves information
       ↓
Continuum evaluates it
       ↓
Continuum returns Context Expansion
       ↓
AI continues
```

This is preferable to forcing all context into the initial prompt.

---

# 75. Context Sufficiency as a Contract

Before beginning significant AI work, Continuum may eventually produce:

```text
Context Readiness:
    READY
    PARTIAL
    NOT READY
```

With reasons.

Example:

```text
READY

Objective:
    clear

Current state:
    known

Constraints:
    known

Relevant artifacts:
    available

Known uncertainty:
    documented
```

---

# 76. Context Continuity Score

Continuum may eventually measure continuity across Sessions.

Potential dimensions:

```text
objective continuity
state continuity
knowledge continuity
decision continuity
artifact continuity
constraint continuity
history continuity
```

This should not collapse into a single opaque score internally.

---

# 77. Continuity Failure

A continuity failure occurs when a new Session lacks information required to correctly continue existing Work.

Examples:

```text
AI repeats already-rejected approach
AI violates established architecture decision
AI modifies the wrong subsystem
AI misunderstands current implementation state
AI asks questions already answered
AI resurrects superseded requirements
```

These failures are measurable.

---

# 78. Continuity Test

A useful future test is:

> Could a fresh AI, with no access to the previous conversation, continue the Work correctly using only Continuum?

If yes:

```text
continuity succeeded
```

If no:

```text
continuity failed
```

This becomes one of Continuum's defining quality criteria.

---

# 79. Continuity Invariant

Continuum establishes the following invariant:

> No essential project understanding should exist solely inside an AI conversation.

If information is important enough to affect future work, it should eventually be represented in persistent project state, Knowledge, Artifact relationships, Decisions, Requirements, or another canonical structure.

---

# 80. Context Compiler Architecture

The eventual Context Compiler may conceptually consist of:

```text
Context Compiler
├── Objective Analyzer
├── State Resolver
├── Graph Traversal
├── Knowledge Retriever
├── History Retriever
├── Freshness Evaluator
├── Conflict Resolver
├── Relevance Ranker
├── Redundancy Eliminator
├── Compression Engine
├── Budget Manager
├── Context Assembler
├── Sufficiency Evaluator
├── Validator
└── Provider Adapter
```

These are conceptual responsibilities, not yet implementation modules.

---

# 81. Context Compiler Inputs

The compiler may eventually accept:

```text
project
objective
work item
session
actor
target AI
target client
available budget
retrieval policy
security policy
freshness policy
context policy
```

---

# 82. Context Compiler Outputs

The compiler should produce:

```text
Context Package
```

along with metadata such as:

```text
selection rationale
provenance
coverage
uncertainty
freshness
sufficiency
budget utilization
warnings
```

---

# 83. Context Compiler Warnings

Potential warnings:

```text
STALE_INFORMATION
UNRESOLVED_CONFLICT
INSUFFICIENT_CONTEXT
LOW_CONFIDENCE_SOURCE
MISSING_ARTIFACT
OUTDATED_DECISION
BUDGET_EXCEEDED
LOSSY_COMPRESSION
UNKNOWN_PROJECT_STATE
```

Warnings should be visible to the AI and/or human when appropriate.

---

# 84. Context Security

The Context Compiler must enforce security boundaries.

Information may be excluded because:

```text
actor lacks authorization
artifact is classified
secret must not be exposed
customer data is restricted
policy forbids disclosure
provider is untrusted
```

Security filtering occurs before context delivery.

---

# 85. Context Minimization

Continuum should follow the principle:

> Give the AI enough information to do the job, but no more sensitive information than necessary.

This reduces:

```text
privacy risk
security exposure
token consumption
noise
cognitive load
```

---

# 86. Context Integrity

A Context Package should preserve distinctions between:

```text
fact
claim
hypothesis
decision
requirement
instruction
observation
historical statement
unknown
```

The compiler must not flatten these into generic prose.

---

# 87. Context Serialization

The canonical Context Package should have a structured representation.

Potential representations:

```text
JSON
YAML
TOML
Markdown
structured message protocol
```

The canonical serialization format remains undecided.

Provider-specific representations may differ.

---

# 88. Human-Readable Context

Although Context Packages are machine-oriented, Continuum should support a human-readable representation.

Example:

```text
PROJECT
    Continuum

OBJECTIVE
    Implement Context Compiler

CURRENT STATE
    Specification phase

DECISIONS
    ...

CONSTRAINTS
    ...

RELEVANT ARTIFACTS
    ...

KNOWN UNCERTAINTIES
    ...

NEXT ACTION
    ...
```

This makes continuity inspectable.

---

# 89. Context Explainability

A human should eventually be able to ask:

> Why did Continuum give the AI this information?

The answer should identify:

```text
selection reason
source
relevance
authority
freshness
dependency
policy
```

---

# 90. Context Compilation Example

Suppose the user says:

```text
"Fix the failing authentication tests."
```

Continuum may discover:

```text
Objective:
    Fix authentication tests

Current state:
    3 tests failing

Relevant files:
    auth/service.ts
    auth/token.ts
    auth.test.ts

Relevant decision:
    DEC-014
    OAuth provider must remain unchanged

Recent history:
    token handling changed yesterday

Relevant evidence:
    failures began after commit abc123

Constraint:
    preserve public API

Unknown:
    exact root cause
```

The AI receives this rather than the entire repository and every prior conversation.

---

# 91. Context Compilation Example — Handoff

Session A ends with:

```text
Work:
    authentication debugging

Completed:
    reproduced failure
    identified regression window

Unresolved:
    exact root cause

Next action:
    inspect token expiration logic
```

Session B starts later.

Continuum compiles:

```text
PROJECT
    Continuum

WORK
    Authentication debugging

CURRENT STATE
    Failure reproducible

RECENT DISCOVERY
    Regression introduced in commit abc123

CONSTRAINT
    Preserve OAuth provider

NEXT ACTION
    Inspect token expiration logic

RELEVANT ARTIFACTS
    token.ts
    auth.service.ts
    auth.test.ts

UNRESOLVED
    Root cause unknown
```

The new AI can continue without seeing the old transcript.

---

# 92. Context Compilation Example — Provider Change

Session A:

```text
Provider:
    ChatGPT
```

Session B:

```text
Provider:
    Claude
```

Session C:

```text
Provider:
    local model
```

All consume:

```text
Continuum Canonical Project State
```

The provider changes.

The project's memory does not.

---

# 93. Context Compilation Example — Model Change

If the model context budget decreases:

```text
Large Context
      ↓
Compiler
      ↓
Compressed Context
```

The project Knowledge does not change.

Only its representation changes.

---

# 94. Context Compilation Example — Expanded Context

An AI determines:

```text
"I need to know why this database abstraction exists."
```

Continuum retrieves:

```text
DEC-008
    ↓
Architecture rationale
    ↓
Historical migration
    ↓
Current constraint
```

The AI receives only the relevant explanation.

---

# 95. Context Compilation Example — Contradiction

Continuum discovers:

```text
Documentation:
    Redis is required.

Configuration:
    Redis removed.

Decision:
    Redis removed in DEC-023.
```

Context becomes:

```text
CURRENT:
    Redis is not part of the active architecture.

HISTORICAL:
    Redis was previously used.

DECISION:
    DEC-023 removed Redis.

WARNING:
    Documentation appears stale.
```

This is significantly safer than simply returning all three statements.

---

# 96. Context Compilation Example — Unknown

If no reliable information exists:

```text
UNKNOWN:
    Why does service X restart?

Evidence:
    insufficient

Recommended action:
    inspect runtime logs
```

Continuum must preserve unknowns rather than manufacture certainty.

---

# 97. Context Compiler Feedback

After AI work:

```text
AI Outcome
     ↓
Context Evaluation
     ↓
What helped?
What was missing?
What was stale?
What was unnecessary?
     ↓
Compiler Feedback
```

This allows the Context Compiler to improve.

---

# 98. Context Learning

Continuum may learn patterns such as:

```text
authentication tasks usually require:
    auth service
    token module
    auth tests
    security decision records
```

However, learned retrieval patterns must not override authoritative project relationships.

Learning should optimize retrieval, not redefine project truth.

---

# 99. Context and Memory

Continuum's memory architecture can therefore be summarized:

```text
LONG-TERM MEMORY
    Project Knowledge
    Artifacts
    Decisions
    Requirements
    History
    Events
        │
        ▼
WORKING MEMORY
    Context Package
        │
        ▼
MODEL CONTEXT
    AI prompt / tool environment
```

The AI's context window becomes a working-memory interface rather than the project's permanent memory.

---

# 100. Context Continuity Architecture

The complete conceptual architecture is:

```text
                    CONTINUUM
┌─────────────────────────────────────────────┐
│                                             │
│       Persistent Project Memory             │
│                                             │
│  Knowledge  Artifacts  Decisions  History   │
│      │          │          │         │      │
│      └──────────┴──────────┴─────────┘      │
│                     │                       │
│                     ▼                       │
│             Current Project State           │
│                     │                       │
│                     ▼                       │
│              Context Compiler               │
│                     │                       │
│       ┌─────────────┼─────────────┐         │
│       ▼             ▼             ▼         │
│   Retrieval     Ranking      Compression    │
│       │             │             │         │
│       └─────────────┼─────────────┘         │
│                     ▼                       │
│              Context Package                │
│                     │                       │
└─────────────────────┼───────────────────────┘
                      │
                      ▼
                     AI
                      │
                      ▼
                   Actions
                      │
                      ▼
                    Events
                      │
                      ▼
             Project State Update
                      │
                      └───────────► CONTINUUM
```

---

# 101. Core Invariants

Continuum establishes these invariants:

1. Project Memory must exist independently of AI conversations.
2. Context is compiled from persistent project information.
3. Context is task-specific.
4. Context is bounded.
5. Context is provenance-aware.
6. Context must distinguish fact from uncertainty.
7. Context must account for freshness.
8. Context must preserve important constraints.
9. Context must preserve relevant decisions.
10. Context must identify relevant current state.
11. Context must not blindly reproduce conversation history.
12. Context must not blindly reproduce the entire repository.
13. Context should maximize relevance and sufficiency.
14. Context should minimize unnecessary information.
15. Context should be security-aware.
16. Context should be inspectable.
17. Context should be reproducible where practical.
18. Context should support provider-independent continuity.
19. Context should support cross-session continuity.
20. Context should support iterative expansion.
21. Context should support invalidation.
22. Context should support evaluation.
23. Context should support handoff.
24. Context should preserve uncertainty rather than invent certainty.
25. Essential project understanding must not exist solely inside a conversation.

---

# 102. The Continuum Continuity Loop

The central loop is:

```text
              ┌─────────────────────┐
              │   PROJECT MEMORY    │
              └──────────┬──────────┘
                         │
                         ▼
                CURRENT PROJECT STATE
                         │
                         ▼
                  CONTEXT COMPILER
                         │
                         ▼
                  CONTEXT PACKAGE
                         │
                         ▼
                         AI
                         │
                         ▼
                     ACTIVITY
                         │
                         ▼
                       EVENTS
                         │
                         ▼
                 KNOWLEDGE UPDATE
                         │
                         ▼
                PROJECT MEMORY
```

This is the fundamental continuity engine of Continuum.

---

# 103. Design Rule

Continuum does not attempt to make AI systems possess infinite memory.

Instead:

> Continuum provides effectively persistent project memory and dynamically compiles the appropriate working memory for whichever AI is currently performing the work.

Therefore:

```text
AI changes
    ↓
Continuum remains

Conversation ends
    ↓
Continuum remains

Context window fills
    ↓
Continuum remains

Model changes
    ↓
Continuum remains

Client changes
    ↓
Continuum remains

Human returns weeks later
    ↓
Continuum remains
```

The AI is replaceable.

The project memory is not.

---

# 104. Final Architectural Principle

The fundamental Continuum architecture is:

```text
                  PERSISTENT MEMORY
                         │
                         ▼
                    CONTEXT ENGINE
                         │
                         ▼
                   AI WORK SESSION
                         │
                         ▼
                     OBSERVATION
                         │
                         ▼
                  MEMORY RECONSTRUCTION
                         │
                         └───────────────►
```

Continuum therefore functions as the **persistent cognitive infrastructure surrounding AI-assisted software engineering**.

The AI supplies reasoning and action.

Continuum supplies continuity.
