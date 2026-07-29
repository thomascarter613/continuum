# Continuum Context Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

## 1. Purpose

The Continuum Context Model defines how project knowledge is selected, assembled, validated, bounded, and delivered to AI systems.

The purpose is to provide an AI with the **minimum sufficient, highest-value project knowledge required for a particular task** while preserving provenance, uncertainty, historical context, and reproducibility.

---

# 2. Fundamental Principle

Context is not memory.

Context is not knowledge.

Context is not state.

Context is not a prompt.

The conceptual relationship is:

```text
Memory
    ↓
Knowledge
    ↓
Current State
    ↓
Task Understanding
    ↓
Context Selection
    ↓
Context Package
    ↓
Prompt Compilation
    ↓
AI
```

---

# 3. Memory

Memory represents everything Continuum retains.

Memory may include:

* events
* conversations
* observations
* artifacts
* claims
* decisions
* evidence
* provenance
* sessions
* historical states

Memory is persistent project history.

---

# 4. Knowledge

Knowledge represents Continuum's structured understanding of project information.

Knowledge includes:

* claims
* propositions
* observations
* interpretations
* decisions
* evidence
* relationships
* epistemic states

Knowledge is derived from and linked to Memory.

---

# 5. State

State represents a derived view of the project's condition at a given point in time.

Example:

```text
current branch
current architecture
active requirements
active constraints
current implementation
open work
known conflicts
known uncertainties
```

State is derived from historical information.

---

# 6. Context

Context is a task-specific projection of project knowledge and state.

```text
Project Knowledge
        │
        │ projection
        ▼
     Context
        │
        ▼
        AI
```

Context must be task-specific.

---

# 7. Context as a First-Class Object

A Context Package is a persistent, inspectable, reproducible object.

Conceptually:

```text
Context
├── identity
├── purpose
├── task
├── project
├── scope
├── temporal boundary
├── selected knowledge
├── selected artifacts
├── constraints
├── assumptions
├── uncertainties
├── conflicts
├── exclusions
├── budget
├── selection rationale
├── project revision
├── knowledge cutoff
└── provenance
```

---

# 8. Context Layers

Context may be organized into the following conceptual layers.

```text
L0 Identity
L1 Mission
L2 Current State
L3 Task
L4 Constraints
L5 Decisions
L6 Relevant Knowledge
L7 Evidence
L8 Artifacts
L9 History
L10 Uncertainty
```

Not every Context Package must contain every layer.

---

# 9. L0 — Identity

Identity establishes the environment in which the task is occurring.

Potential information:

```text
project
repository
branch
workspace
environment
revision
version
```

Identity prevents accidental mixing of projects, branches, or environments.

---

# 10. L1 — Mission

Mission provides strategic project context.

Example:

```text
Build a standalone persistent memory and context
system for AI-assisted software engineering.
```

Mission establishes the larger purpose against which task-level decisions may be evaluated.

---

# 11. L2 — Current State

Current State describes the relevant current condition of the project.

Potential information:

```text
repository status
architecture status
active work
completed work
open issues
current implementation
current dependencies
current constraints
known conflicts
```

Current State should be derived from project history rather than manually duplicated wherever practical.

---

# 12. L3 — Task

Task describes the immediate activity.

Potential information:

```text
task identity
objective
desired outcome
deliverables
acceptance criteria
active actor
```

Task is one of the primary drivers of context selection.

---

# 13. L4 — Constraints

Constraints represent conditions that must be respected.

Examples:

```text
architectural constraints
technical constraints
security requirements
performance requirements
organizational constraints
compatibility requirements
deployment constraints
```

Relevant constraints should receive high context priority.

---

# 14. L5 — Decisions

Relevant accepted decisions should be included explicitly.

Example:

```text
DEC-001:
Continuum is standalone.

DEC-002:
Continuum must be AI-provider agnostic.

DEC-003:
Continuum must support arbitrary software-engineering projects.
```

Decisions should include their rationale and provenance when relevant.

---

# 15. L6 — Relevant Knowledge

This layer contains dynamically selected project knowledge.

Potential objects:

```text
requirements
claims
decisions
architecture
constraints
dependencies
work items
domain concepts
known risks
```

Selection is task-dependent.

---

# 16. L7 — Evidence

Evidence provides grounding for relevant knowledge.

Potential evidence:

```text
tests
source inspection
runtime observations
build results
commits
logs
documentation
external references
```

Claims with material uncertainty should preferentially expose supporting evidence.

---

# 17. L8 — Artifacts

Artifacts are the concrete project materials relevant to the task.

Examples:

```text
source files
tests
schemas
configuration
documentation
issues
pull requests
design documents
generated files
```

Artifact selection should be task-specific.

---

# 18. L9 — History

History provides selected historical information.

History should be semantic rather than merely conversational.

Example:

```text
July 12:
PostgreSQL decision changed.

July 18:
Authentication architecture revised.

July 24:
JWT approach rejected.
```

Historical information should be included when it materially affects current reasoning.

---

# 19. L10 — Uncertainty

Context should explicitly represent uncertainty.

Potential categories:

```text
unknown
uncertain
assumed
conflicted
unverified
stale
incomplete
```

Example:

```text
Known:
    API uses OIDC.

Uncertain:
    Whether refresh tokens are persisted.

Conflicting:
    Documentation and implementation disagree.

Unverified:
    Production deployment configuration.
```

The system must not force uncertain information into false certainty.

---

# 20. Context Boundaries

Every Context Package should establish boundaries where applicable.

Potential boundaries:

```text
project
scope
time
environment
branch
revision
artifact version
knowledge cutoff
```

Example:

```text
Project:
    Continuum

Scope:
    specification/

Environment:
    development

Revision:
    abc123

Knowledge cutoff:
    2026-07-29T14:00:00Z
```

---

# 21. Context Selection

Context selection determines which project objects should be included.

Selection should consider:

```text
task relevance
semantic relevance
graph proximity
dependency relevance
temporal validity
scope
authority
confidence
recency
artifact relationship
risk
```

Semantic similarity alone is insufficient.

---

# 22. Context Relevance

Conceptually:

```text
Context Relevance =
f(
    semantic similarity,
    graph proximity,
    task dependency,
    temporal validity,
    authority,
    confidence,
    scope,
    recency,
    risk
)
```

The exact algorithm remains unspecified.

---

# 23. Context Graph Expansion

Starting from the active Task, Continuum may traverse relationships such as:

```text
Task
 ├── addresses → Requirement
 ├── constrained_by → Constraint
 ├── depends_on → Decision
 ├── modifies → Artifact
 ├── verified_by → Test
 ├── blocked_by → Issue
 └── related_to → Knowledge
```

Relevant graph neighbors may then be candidates for Context inclusion.

---

# 24. Required, Recommended, and Optional Context

Context candidates should eventually be classified as:

### Required

Information without which the AI cannot safely perform the task.

### Recommended

Information that materially improves reasoning but is not essential.

### Optional

Useful background information that may be omitted under budget pressure.

This classification supports context optimization.

---

# 25. Context Budget

Context is constrained by finite resources.

Potential budget dimensions:

```text
token budget
latency budget
retrieval budget
source count
processing budget
complexity budget
```

Context selection should maximize useful information within those constraints.

---

# 26. Context Completeness

A Context Package should be evaluable for sufficiency.

Conceptually:

```text
Context Completeness
├── sufficient
├── insufficient
└── unknown
```

Example:

```text
Task:
    modify authentication middleware

Required:
    authentication architecture
    relevant source
    security constraints
    tests

Missing:
    production authentication configuration

Result:
    INSUFFICIENT
```

The AI should be able to request additional Context rather than silently guessing.

---

# 27. Context Confidence

Context itself may have a confidence profile.

Example:

```text
Architecture:
    high confidence

Current implementation:
    high confidence

Production state:
    low confidence

Historical rationale:
    medium confidence
```

Context confidence is distinct from Claim confidence.

---

# 28. Context Provenance

Every Context Package should be traceable to the knowledge from which it was assembled.

Conceptually:

```text
Context
    │
    ├── selected_object
    ├── selection_reason
    ├── source_state
    ├── knowledge_cutoff
    ├── generated_by
    └── generated_at
```

This allows reconstruction of what the AI received.

---

# 29. Context Manifest

A Context Package should have a manifest containing, conceptually:

```text
context_id
project_id
task_id
generated_at
knowledge_cutoff
project_revision
scope
budget
objects
exclusions
warnings
uncertainties
selection_strategy
```

The exact schema remains unspecified.

---

# 30. Context Exclusion

The system should support explicit exclusion records.

Example:

```text
Excluded:
    billing subsystem

Reason:
    unrelated to authentication task
```

or:

```text
Excluded:
    ADR-003

Reason:
    superseded by ADR-017
```

Exclusions make selection behavior explainable.

---

# 31. Context Invalidation

A Context Package may become stale when relevant project knowledge changes.

Potential invalidation triggers include:

```text
relevant commit
decision change
requirement change
constraint change
environment change
new contradiction
artifact modification
```

A stale Context Package should be detectable.

---

# 32. Context Snapshot

A Context Package is a snapshot of project knowledge for a particular task and point in time.

This permits questions such as:

```text
What context did the AI receive?

What did Continuum know at that time?

Which decisions were included?

Which relevant information was excluded?

What project revision was used?
```

---

# 33. Context vs Prompt

Context is structured project knowledge.

A Prompt is a provider-facing representation of:

```text
instructions
+
task
+
context
+
provider-specific formatting
```

Therefore:

```text
Context
    ↓
Prompt Compiler
    ↓
Prompt
```

The Prompt is not the canonical project memory.

The Context Package is.

---

# 34. Provider Independence

The canonical Context Model must be independent of any specific AI provider.

Potential provider adapters include:

```text
OpenAI
Anthropic
Google
local models
agent frameworks
CLI coding agents
IDE assistants
```

Provider-specific formatting belongs to adapters.

---

# 35. AI Role Profiles

Different AI roles require different Context profiles.

Potential roles:

```text
architect
planner
coder
debugger
reviewer
tester
researcher
documentarian
release agent
security auditor
```

Example:

A debugger may prioritize:

```text
error
stack trace
recent changes
affected code
tests
runtime state
```

An architect may prioritize:

```text
requirements
constraints
decisions
system topology
dependencies
tradeoffs
```

The same project may therefore produce different Context Packages for different roles.

---

# 36. Context Compilation

The Context pipeline is conceptually:

```text
Task
  ↓
Task Understanding
  ↓
Knowledge Retrieval
  ↓
Graph Expansion
  ↓
Relevance Ranking
  ↓
Conflict Detection
  ↓
Context Selection
  ↓
Budget Optimization
  ↓
Context Assembly
  ↓
Context Validation
  ↓
Prompt Compilation
  ↓
AI
```

This process is called **Context Compilation**.

---

# 37. Context API

A future Continuum API may conceptually expose:

```text
context.create(task)
context.inspect(context_id)
context.validate(context_id)
context.explain(context_id)
context.refresh(context_id)
context.invalidate(context_id)
context.compile(context_id, provider)
```

These are conceptual operations only and do not define the implementation.

---

# 38. Context Quality Requirements

A high-quality Context Package should be:

```text
relevant
sufficient
bounded
current
traceable
reproducible
provider-independent
uncertainty-aware
conflict-aware
task-specific
```

---

# 39. Design Rules

Continuum establishes the following rules:

1. Context is a projection of project knowledge.
2. Context is task-specific.
3. Context is not the canonical memory representation.
4. Context must preserve provenance.
5. Context should expose material uncertainty.
6. Context should expose material conflicts.
7. Context should respect temporal validity.
8. Context should respect scope.
9. Context should be bounded by explicit budgets.
10. Context should be evaluable for completeness.
11. Context should be reproducible.
12. Context should remain independent of AI providers.
13. Prompt formatting belongs outside the canonical Context Model.
14. Context selection must use more than lexical similarity.
15. Historical information should be included when materially relevant.
16. Exclusions should be explainable when practical.
17. Stale Context must be detectable.
18. AI should be able to request missing Context rather than guessing.

---

# 40. Open Questions

The following remain unresolved:

1. Context selection algorithm
2. Relevance scoring
3. Graph traversal strategy
4. Context budget optimization
5. Token estimation
6. Context compression
7. Context summarization
8. Context layering implementation
9. Context completeness algorithm
10. Context confidence model
11. Context invalidation rules
12. Context cache strategy
13. Context versioning
14. Context serialization
15. Prompt compilation interface
16. Provider adapter contract
17. Role-specific context policies
18. Context conflict presentation
19. Context omission policies
20. User override mechanisms
21. Context security and access control
22. Sensitive-data filtering
23. Context provenance granularity
24. Context reproducibility guarantees

---

# 41. Design Rule

Continuum must not attempt to give an AI everything it knows.

It must determine:

> **What does this AI need to know, for this task, at this moment, in this project state, and why?**

Context is therefore a compiled, bounded, traceable projection of project knowledge.
