# Continuum Retrieval & Relevance Architecture

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

The Retrieval & Relevance Architecture defines how Continuum determines which information should be surfaced to an AI agent or human at a particular moment.

The central problem is:

> Given everything Continuum knows about a project, what information is relevant to the current task, and what is the minimum sufficient context required to act intelligently?

Continuum must therefore solve two related problems:

```text
Retrieval
    What information might matter?

Relevance
    What information actually matters here?
```

These are not the same problem.

---

# 2. The Core Problem

A long-running software project may eventually contain:

```text
millions of observations
thousands of decisions
hundreds of requirements
thousands of artifacts
many conversations
many work sessions
large volumes of source code
test results
design documents
architectural models
historical decisions
failed approaches
```

An AI cannot consume all of this context at once.

Therefore Continuum must transform:

```text
PROJECT MEMORY
     ↓
RETRIEVAL
     ↓
CANDIDATE INFORMATION
     ↓
RELEVANCE
     ↓
CONTEXT ASSEMBLY
     ↓
AI
```

---

# 3. Retrieval Is Not Context Assembly

Retrieval finds potentially useful information.

Context Assembly determines what actually gets supplied to the AI.

```text
Retrieval:
    "Here are 200 potentially relevant items."

Context Assembly:
    "These 17 items are sufficient for the current task."
```

These must remain separate architectural responsibilities.

---

# 4. The Retrieval Principle

Continuum should not rely upon a single retrieval mechanism.

Instead:

```text
                  QUERY / TASK
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
     Semantic       Structural      Temporal
     Retrieval      Retrieval       Retrieval
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                  Graph Retrieval
                       │
                       ▼
                  State Retrieval
                       │
                       ▼
                Candidate Set
                       │
                       ▼
                   RANKING
                       │
                       ▼
                 RELEVANCE
                       │
                       ▼
              CONTEXT ASSEMBLY
```

---

# 5. Retrieval Dimensions

Continuum should retrieve using multiple dimensions.

At minimum:

```text
semantic
lexical
structural
graph
temporal
state
scope
authority
confidence
dependency
causal
intent
```

---

# 6. Semantic Retrieval

Semantic retrieval answers:

> What information means something similar to the current request?

Example:

```text
Query:
    "Why does authentication keep failing?"

Relevant memories might contain:
    JWT validation
    token expiration
    OAuth middleware
    auth session handling
```

Even if the exact words "authentication keeps failing" never appear.

Semantic retrieval is therefore useful for conceptual similarity.

---

# 7. Lexical Retrieval

Lexical retrieval answers:

> What information contains matching terms, identifiers, names, or exact phrases?

This is essential for software engineering.

Example:

```text
Query:
    DATABASE_URL

Potential matches:
    .env.example
    config.ts
    deployment.yaml
    README.md
    test fixtures
```

Exact identifiers often matter more than semantic similarity.

---

# 8. Structural Retrieval

Structural retrieval uses project relationships.

Example:

```text
service/auth
    ↓
src/token.ts
    ↓
JwtValidator
    ↓
test/token.test.ts
```

If the task concerns `JwtValidator`, structural retrieval should naturally surface its surrounding artifacts.

---

# 9. Graph Retrieval

Continuum's knowledge graph allows retrieval through relationships.

Example:

```text
Task
 ↓
Decision
 ↓
Architecture component
 ↓
Service
 ↓
Source files
 ↓
Tests
```

This allows retrieval to follow meaning rather than merely similarity.

---

# 10. Temporal Retrieval

Time matters.

The system must distinguish:

```text
what was true
```

from:

```text
what is true now
```

For example:

```text
January:
    Redis was used.

April:
    Redis was removed.

July:
    PostgreSQL is used.
```

A current task should generally prioritize July.

A historical debugging task may need January.

---

# 11. State Retrieval

Retrieval should understand project state.

Potential states:

```text
planned
active
blocked
completed
abandoned
superseded
deprecated
failed
under investigation
```

A task about implementing authentication should prioritize:

```text
active
blocked
current
```

over:

```text
completed
historical
superseded
```

unless history is relevant.

---

# 12. Scope Retrieval

Information should be scoped.

Possible scopes:

```text
organization
project
repository
branch
workspace
service
package
module
artifact
feature
task
environment
session
```

A request concerning:

```text
services/auth
```

should not automatically retrieve unrelated information about:

```text
services/billing
```

unless a dependency makes it relevant.

---

# 13. Authority Retrieval

Retrieval should prefer authoritative information.

For example:

```text
Current source code
        >
AI speculation
```

and:

```text
Accepted architecture decision
        >
unaccepted AI proposal
```

Authority should therefore influence ranking.

---

# 14. Confidence Retrieval

Confidence should influence ranking.

For example:

```text
verified claim
        >
supported claim
        >
candidate claim
        >
unverified assertion
```

However, low-confidence information may still be useful when diagnosing uncertainty.

Therefore:

> Low confidence should reduce ranking, not necessarily eliminate retrieval.

---

# 15. Dependency Retrieval

If artifact A depends on B, a task concerning A may require information about B.

Example:

```text
AuthController
     ↓
AuthService
     ↓
TokenService
     ↓
JWT library
```

A retrieval engine should be able to traverse these dependencies.

---

# 16. Causal Retrieval

Dependency is not the same as causality.

Continuum should eventually support causal relationships such as:

```text
configuration change
      ↓
test failure
      ↓
deployment failure
      ↓
rollback
```

A debugging task may need this chain even when the individual events are semantically dissimilar.

---

# 17. Intent Retrieval

The same query can require different context depending on intent.

Consider:

```text
"What is authentication?"
```

versus:

```text
"Fix authentication."
```

versus:

```text
"Why did authentication break?"
```

versus:

```text
"Design authentication."
```

The retrieval requirements differ substantially.

---

# 18. Task Intent

Continuum should classify or infer task intent.

Potential intents:

```text
understand
implement
modify
debug
review
design
plan
decide
test
document
refactor
investigate
explain
resume
```

---

# 19. Intent Determines Retrieval

Example:

```text
Intent:
    debug
```

Prioritize:

```text
recent failures
logs
test results
recent changes
relevant source
known issues
related decisions
```

Whereas:

```text
Intent:
    design
```

Prioritize:

```text
requirements
constraints
architecture
decisions
tradeoffs
existing patterns
future goals
```

---

# 20. Query Expansion

A user's request may not contain enough vocabulary for retrieval.

Example:

```text
User:
    "Let's continue the auth work."
```

Continuum may expand the query using project knowledge:

```text
authentication
JWT
OAuth
AuthService
token
session
login
authorization
```

Query expansion should be contextual rather than generic.

---

# 21. Query Decomposition

Complex tasks should be decomposed.

Example:

```text
"Fix the authentication bug and update the tests."
```

becomes:

```text
Task A:
    investigate authentication failure

Task B:
    identify relevant implementation

Task C:
    identify affected tests

Task D:
    determine expected behavior
```

Each subtask may have different retrieval needs.

---

# 22. Retrieval Candidate Set

Retrieval should initially favor recall.

The first stage should answer:

> What could possibly matter?

not:

> What definitely matters?

This prevents important information from being eliminated too early.

---

# 23. Ranking

The second stage should favor precision.

Conceptually:

```text
Candidate Set
      ↓
relevance scoring
      ↓
deduplication
      ↓
contradiction handling
      ↓
authority adjustment
      ↓
freshness adjustment
      ↓
dependency expansion
      ↓
final ranking
```

---

# 24. Relevance

Relevance is multidimensional.

A candidate's relevance may depend on:

```text
semantic similarity
lexical similarity
scope match
task intent
temporal proximity
state
authority
confidence
importance
dependency distance
graph proximity
causal relationship
artifact relationship
recency
```

---

# 25. Relevance Is Contextual

The same memory can be:

```text
highly relevant
```

to one task and:

```text
irrelevant
```

to another.

Therefore relevance must never be treated as an immutable property of a memory.

---

# 26. Relevance Score

A conceptual relevance function:

```text
R(x, q, c) =
    semantic
  + lexical
  + structural
  + graph
  + temporal
  + state
  + scope
  + authority
  + confidence
  + intent
  + dependency
  + causal
```

where:

```text
x = candidate information
q = query
c = current project context
```

The exact mathematical formulation is intentionally deferred.

---

# 27. Relevance Must Be Explainable

Continuum should eventually answer:

> Why did you include this information?

Example:

```text
Included because:

    semantic similarity: high
    scope match: exact
    dependency distance: 1
    authority: verified
    freshness: current
```

This is essential for debugging the retrieval system itself.

---

# 28. Retrieval Sources

Continuum may retrieve from:

```text
conversation memory
knowledge graph
artifact index
source repository
documentation
decision records
requirements
work items
events
test results
logs
tool outputs
external references
```

Not all information needs to be stored identically.

---

# 29. Retrieval Federation

Continuum should be capable of federated retrieval.

Conceptually:

```text
                    Continuum
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
       Git repo      Knowledge      Documents
                       store
          │             │             │
          └─────────────┼─────────────┘
                        ▼
                     Ranking
```

This prevents Continuum from requiring every external artifact to be copied into its own database.

---

# 30. Retrieval Adapters

External systems should be accessed through adapters.

Examples:

```text
Git adapter
filesystem adapter
GitHub adapter
IDE adapter
issue tracker adapter
documentation adapter
CI adapter
database adapter
```

---

# 31. Local vs Remote Retrieval

Continuum should distinguish:

```text
local retrieval
remote retrieval
cached retrieval
live retrieval
```

Live project state may be more authoritative than stale cached memory.

---

# 32. Retrieval Freshness

Candidate information should have freshness metadata.

For example:

```text
source code:
    extremely fresh

cached documentation:
    potentially stale

historical conversation:
    intentionally historical
```

Freshness should affect ranking according to task intent.

---

# 33. Current-State Preference

For tasks concerning current implementation, Continuum should generally prefer:

```text
current repository
current branch
current configuration
current tests
current environment
```

over historical descriptions.

---

# 34. Historical Preference

For historical questions:

```text
"What did we originally decide?"
```

Continuum should prioritize:

```text
decision history
conversation history
superseded knowledge
Git history
historical artifacts
```

---

# 35. Temporal Query Interpretation

Queries may explicitly imply time.

Examples:

```text
"Why did we change this?"
"What are we doing now?"
"What did we plan for later?"
"What was the original architecture?"
```

Temporal semantics should influence retrieval.

---

# 36. Project State as Retrieval Context

Every retrieval request should ideally carry project state:

```text
project
repository
branch
commit
environment
active work item
current session
current agent
current user
```

This greatly improves retrieval precision.

---

# 37. Session Context

The current session is itself a retrieval signal.

If the last several turns concern:

```text
authentication
```

then a request such as:

```text
"what should we do next?"
```

should inherit that context.

---

# 38. Session Context Is Not Project Truth

Conversation context should not automatically become project Knowledge.

The system must preserve:

```text
session context
```

separately from:

```text
established project Knowledge
```

---

# 39. Conversational Retrieval

Continuum should support queries such as:

```text
"Where were we?"

"What did we decide?"

"Why did we do that?"

"What was the next step?"

"What have we already tried?"

"Did we ever fix that?"

"What remains?"

```

These are not ordinary semantic search queries.

They require project-state reconstruction.

---

# 40. Resume Retrieval

A "resume" request should retrieve:

```text
current objective
active work item
last known state
recent activity
unfinished work
recent decisions
known blockers
next planned action
relevant artifacts
```

This creates a coherent continuation context.

---

# 41. Resume Context

Conceptually:

```text
RESUME
  │
  ├── Goal
  ├── Current State
  ├── Completed
  ├── In Progress
  ├── Blocked
  ├── Decisions
  ├── Relevant Knowledge
  ├── Recent Events
  └── Next Action
```

---

# 42. "What Have We Tried?"

This query requires episodic retrieval.

The system should retrieve:

```text
attempts
experiments
failed approaches
rejected designs
errors
workarounds
results
```

This prevents the AI from repeatedly proposing approaches that already failed.

---

# 43. Negative Knowledge

Negative Knowledge is extremely valuable.

Examples:

```text
does not work
was rejected
was already tried
is incompatible
causes failure
is intentionally prohibited
```

Negative Knowledge should be first-class.

---

# 44. Negative Retrieval

When planning an implementation, Continuum should retrieve relevant negative knowledge.

Example:

```text
Proposed:
    library X

Retrieved:
    library X was previously rejected because
    it does not support requirement Y.
```

This prevents repeated mistakes.

---

# 45. Retrieval of Decisions

When a design choice is relevant, retrieve:

```text
decision
rationale
alternatives
constraints
date
authority
current status
supersession state
```

Not merely the decision title.

---

# 46. Retrieval of Requirements

Requirements should be retrieved together with:

```text
source
priority
status
constraints
acceptance criteria
related decisions
related implementation
```

---

# 47. Retrieval of Constraints

Constraints are often more important than preferences.

Examples:

```text
must run on Linux
cannot require cloud infrastructure
must remain open source
must support SQLite
must operate under 8 GB RAM
```

Constraints should receive high retrieval priority when applicable.

---

# 48. Retrieval of Architecture

Architecture retrieval should include:

```text
components
boundaries
interfaces
dependencies
decisions
constraints
patterns
known exceptions
```

---

# 49. Retrieval of Artifacts

Artifact retrieval should support:

```text
exact path
symbol
type
dependency
ownership
history
semantic meaning
relationship
```

---

# 50. Retrieval of Code

For software engineering, code retrieval must eventually support more than embeddings.

Potential signals:

```text
file path
symbol
AST
imports
exports
call graph
dependency graph
types
tests
Git history
comments
documentation
```

---

# 51. Code Retrieval Principle

For code:

> Structural relationships are often more informative than semantic similarity.

Therefore Continuum should eventually combine:

```text
semantic code search
+
symbol search
+
AST search
+
dependency graph
+
call graph
+
Git history
```

---

# 52. Retrieval of Tests

Tests are especially valuable because they encode expected behavior.

For a source artifact, retrieve:

```text
unit tests
integration tests
E2E tests
fixtures
mocks
test failures
coverage information
```

---

# 53. Retrieval of Failures

For debugging, prioritize:

```text
current failure
similar historical failures
recent code changes
relevant tests
logs
known causes
previous attempted fixes
```

---

# 54. Retrieval of Recent Changes

Debugging should heavily weight recent changes.

Potential sources:

```text
Git commits
pull requests
file modifications
configuration changes
dependency updates
environment changes
```

---

# 55. Retrieval of Similar Historical Incidents

Historical incidents may provide valuable patterns.

Example:

```text
Current failure:
    database connection refused.

Historical:
    identical failure occurred after
    environment variable migration.
```

This may be more useful than generic semantic matches.

---

# 56. Deduplication

Retrieval may return many representations of the same information.

Example:

```text
README:
    PostgreSQL is used.

Architecture doc:
    PostgreSQL is used.

Decision:
    PostgreSQL was selected.

AI summary:
    PostgreSQL is used.
```

These should be consolidated or grouped.

---

# 57. Representation Diversity

Deduplication must not eliminate useful diversity.

For example:

```text
Decision:
    why PostgreSQL was chosen.

Source code:
    how PostgreSQL is implemented.

Test:
    whether PostgreSQL behavior works.

```

These are related but not redundant.

---

# 58. Contradiction-Aware Retrieval

Retrieval should intentionally surface contradictions when relevant.

Example:

```text
Current documentation:
    Redis is used.

Current source:
    PostgreSQL is used.
```

The retrieval system should not hide the conflict simply because one source ranks higher.

---

# 59. Contradiction Budget

Context should avoid overwhelming the AI with irrelevant conflicts.

Therefore contradictions should be prioritized according to:

```text
relevance
importance
authority difference
freshness
impact
```

---

# 60. Context Diversity

A final context set should ideally include different knowledge types.

For example:

```text
1 requirement
2 relevant decisions
3 current architecture
4 relevant source
2 tests
1 previous failure
1 current blocker
```

rather than:

```text
10 nearly identical summaries
```

---

# 61. Context Assembly

After ranking, Continuum should assemble a coherent context.

Conceptually:

```text
Candidates
    ↓
Select
    ↓
Group
    ↓
Order
    ↓
Summarize where appropriate
    ↓
Preserve provenance
    ↓
Compile context
```

---

# 62. Context Ordering

Context ordering should reflect reasoning needs.

A possible structure:

```text
CURRENT OBJECTIVE
CURRENT STATE
CONSTRAINTS
RELEVANT DECISIONS
CURRENT KNOWLEDGE
RELEVANT ARTIFACTS
RECENT EVENTS
FAILURES / NEGATIVE KNOWLEDGE
OPEN QUESTIONS
```

---

# 63. Context Compression

When too much information is retrieved, Continuum should compress it.

Compression should preserve:

```text
meaning
provenance
confidence
authority
time
scope
relationships
uncertainty
```

---

# 64. Summaries Must Not Become Truth

A generated summary is a representation.

It should not silently replace its source material.

The system should preserve:

```text
summary
source references
generation time
generator
scope
```

---

# 65. Hierarchical Context

Continuum should eventually support:

```text
Project Summary
    ↓
Subsystem Summary
    ↓
Component Summary
    ↓
Artifact Detail
    ↓
Source Detail
```

This allows progressive disclosure.

---

# 66. Progressive Retrieval

The AI should not necessarily receive everything immediately.

Instead:

```text
Initial context
      ↓
AI reasons
      ↓
requests more information
      ↓
Continuum retrieves
      ↓
AI continues
```

This creates an iterative retrieval loop.

---

# 67. Retrieval Loop

Conceptually:

```text
          ┌───────────────┐
          │ Current Task  │
          └───────┬───────┘
                  ▼
              Retrieve
                  ▼
               Rank
                  ▼
             Assemble
                  ▼
                  AI
                  │
          ┌───────┴────────┐
          │                │
      sufficient       insufficient
          │                │
          ▼                ▼
       execute         retrieve more
```

---

# 68. Retrieval-on-Demand

An AI should be able to ask:

```text
retrieve:
    relevant decisions

retrieve:
    implementation of AuthService

retrieve:
    failed approaches

retrieve:
    current tests
```

rather than requiring the entire project context up front.

---

# 69. Retrieval Budget

Every context assembly should have a budget.

Possible dimensions:

```text
token budget
latency budget
query budget
source budget
complexity budget
```

---

# 70. Minimum Sufficient Context

Continuum should optimize toward:

> The smallest context that enables reliable action.

This is preferable to:

> The largest context available.

---

# 71. Context Sufficiency

Eventually Continuum may estimate:

```text
context sufficiency:
    low
    moderate
    high
```

based on:

```text
task coverage
constraint coverage
knowledge coverage
artifact coverage
uncertainty
missing dependencies
```

---

# 72. Missing Context Detection

If the system detects:

```text
critical unknown
```

it should not hallucinate.

It should identify:

```text
missing information
```

and retrieve it or ask the user.

---

# 73. Retrieval Failure

If retrieval cannot establish sufficient context, Continuum should expose uncertainty.

Example:

```text
I found three conflicting descriptions of the
authentication architecture and cannot establish
which one reflects the current implementation.
```

This is preferable to false certainty.

---

# 74. Retrieval Observability

Continuum should record retrieval activity.

Potential telemetry:

```text
query
intent
candidate count
selected items
ranking scores
sources
latency
context size
missing information
retrieval iterations
```

---

# 75. Retrieval Evaluation

Retrieval quality must eventually be measurable.

Potential metrics:

```text
precision
recall
context sufficiency
irrelevant-context rate
contradiction detection
retrieval latency
token efficiency
task success rate
```

---

# 76. Retrieval Feedback

AI outcomes can provide feedback.

If the AI says:

```text
"I don't know because I couldn't find the relevant decision."
```

Continuum can record a retrieval failure.

Likewise:

```text
AI successfully resumes work using retrieved context.
```

can provide positive evidence.

---

# 77. Retrieval Learning

Over time Continuum should learn:

```text
which sources are useful
which representations are redundant
which retrieval strategies work
which context structures improve outcomes
```

However:

> Retrieval optimization must not silently alter project Knowledge.

Learning retrieval behavior and learning project truth are separate concerns.

---

# 78. Retrieval Security

Retrieval must respect authorization.

A user or agent should only retrieve information they are permitted to access.

Potential controls:

```text
tenant
project
repository
environment
artifact
classification
identity
role
agent permissions
```

---

# 79. Retrieval Isolation

Information from one project must not accidentally contaminate another.

Project boundaries should be enforced structurally.

```text
Project A
    ↓
isolated retrieval scope

Project B
    ↓
isolated retrieval scope
```

Cross-project knowledge should require explicit authorization.

---

# 80. Retrieval Contamination

Continuum must guard against:

```text
irrelevant project memory
wrong branch
stale documentation
superseded decisions
untrusted external information
AI hallucinations
```

These are retrieval contamination risks.

---

# 81. Retrieval Trust

Each retrieved item should carry epistemic metadata.

For example:

```text
SOURCE:
    architecture.md

STATUS:
    accepted

CONFIDENCE:
    high

AUTHORITY:
    high

FRESHNESS:
    current

SCOPE:
    project
```

This enables the AI to reason appropriately.

---

# 82. Retrieval Result Envelope

Conceptually, each retrieved item may eventually have:

```text
result
├── content
├── source
├── relevance
├── confidence
├── authority
├── freshness
├── scope
├── status
├── relationships
└── provenance
```

This is a conceptual model, not yet a final schema.

---

# 83. Retrieval Modes

Continuum should support explicit retrieval modes.

Potential modes:

```text
resume
explore
debug
implement
design
review
plan
investigate
historical
decision
```

---

# 84. Resume Mode

Prioritize:

```text
current state
unfinished work
recent session
next action
open blockers
recent decisions
```

---

# 85. Debug Mode

Prioritize:

```text
failure
recent changes
tests
logs
dependencies
previous failures
previous attempted fixes
```

---

# 86. Implementation Mode

Prioritize:

```text
requirements
constraints
architecture
relevant decisions
patterns
target artifacts
tests
examples
```

---

# 87. Design Mode

Prioritize:

```text
requirements
constraints
existing architecture
decisions
tradeoffs
alternatives
patterns
future goals
```

---

# 88. Review Mode

Prioritize:

```text
requirements
acceptance criteria
changed artifacts
tests
architecture rules
security constraints
known risks
```

---

# 89. Historical Mode

Prioritize:

```text
past decisions
Git history
previous sessions
superseded knowledge
previous implementations
previous failures
```

---

# 90. Decision Mode

Prioritize:

```text
problem
requirements
constraints
alternatives
prior decisions
evidence
tradeoffs
consequences
```

---

# 91. Retrieval Architecture Layers

The eventual system should conceptually contain:

```text
Layer 1 — Query Understanding
Layer 2 — Query Expansion
Layer 3 — Candidate Retrieval
Layer 4 — Graph Expansion
Layer 5 — Candidate Ranking
Layer 6 — Contradiction Detection
Layer 7 — Context Selection
Layer 8 — Context Compression
Layer 9 — Context Assembly
Layer 10 — Retrieval Telemetry
```

---

# 92. Query Understanding

Input:

```text
"Why did we choose Tauri?"
```

may become:

```text
intent:
    decision

subject:
    Tauri

desired information:
    rationale

temporal preference:
    historical + current

knowledge types:
    decision
    alternatives
    constraints
    evidence
```

---

# 93. Candidate Retrieval

Candidate retrieval may query:

```text
semantic index
lexical index
graph store
artifact index
event store
knowledge store
external adapters
```

---

# 94. Graph Expansion

If the query finds:

```text
Decision D17
```

graph expansion may retrieve:

```text
D17
  ↓
requirements
  ↓
constraints
  ↓
alternatives
  ↓
architecture
  ↓
implementation
```

---

# 95. Candidate Ranking

Ranking should combine:

```text
query match
task intent
project state
scope
authority
confidence
freshness
graph proximity
dependency
importance
```

---

# 96. Contradiction Detection

Before final assembly:

```text
selected Knowledge
      ↓
check contradictory Knowledge
      ↓
include relevant conflicts
      ↓
mark epistemic differences
```

This prevents false coherence.

---

# 97. Context Selection

Selection should optimize:

```text
relevance
coverage
diversity
authority
freshness
token efficiency
```

---

# 98. Context Compression

Compression may transform:

```text
15 related events
```

into:

```text
A migration from Redis to PostgreSQL occurred in April.
The migration was completed in commit abc123 and verified
by integration tests.
```

while retaining source references.

---

# 99. Context Assembly

The final context should be organized for AI reasoning.

A conceptual structure:

```text
<continuum-context>

<task>
...
</task>

<current-state>
...
</current-state>

<constraints>
...
</constraints>

<knowledge>
...
</knowledge>

<decisions>
...
</decisions>

<artifacts>
...
</artifacts>

<events>
...
</events>

<uncertainties>
...
</uncertainties>

<provenance>
...
</provenance>

</continuum-context>
```

The exact transport format remains TBD.

---

# 100. Retrieval Is a Reasoning Primitive

Continuum should not treat retrieval as merely a database feature.

Retrieval is part of the AI's reasoning environment.

The system determines:

```text
what the AI sees
```

and therefore strongly influences:

```text
what the AI believes
what the AI proposes
what the AI changes
what the AI remembers
```

---

# 101. Retrieval Must Be Conservative

Because retrieval influences reasoning:

> Missing information can be harmful, but misleading information can be worse.

Therefore Continuum should prefer:

```text
explicit uncertainty
```

over:

```text
false completeness
```

---

# 102. Retrieval and Epistemic Safety

Retrieved information should preserve:

```text
source
confidence
authority
freshness
scope
status
```

The AI must be able to distinguish:

```text
"we know"
```

from:

```text
"we think"
```

and:

```text
"someone previously suggested."
```

---

# 103. Retrieval and Project Continuity

The ultimate objective is continuity.

A future AI session should be able to ask:

```text
Where are we?
What are we building?
Why are we building it?
What have we decided?
What has been completed?
What remains?
What have we tried?
What failed?
What should happen next?
```

and Continuum should reconstruct the answer from project state rather than relying on a single enormous conversation history.

---

# 104. The Retrieval Principle

The central principle of this architecture is:

> Continuum does not retrieve everything that might be related. It reconstructs the smallest sufficiently complete, trustworthy, current, and task-relevant model of the project required for the next intelligent action.

Therefore:

```text
                    PROJECT
                       │
                       ▼
                    QUERY
                       │
                       ▼
                INTENT ANALYSIS
                       │
                       ▼
             MULTI-MODAL RETRIEVAL
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
     Semantic       Structural      Temporal
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                   Graph / State
                       │
                       ▼
                  CANDIDATES
                       │
                       ▼
                    RANKING
                       │
                       ▼
             CONTRADICTION CHECK
                       │
                       ▼
                CONTEXT SELECTION
                       │
                       ▼
               CONTEXT ASSEMBLY
                       │
                       ▼
                       AI
                       │
                       ▼
                NEW OBSERVATIONS
                       │
                       └──────────────→ CONTINUUM
```

The objective is not merely **memory retrieval**.

It is **project-state reconstruction**.
