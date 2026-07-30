# Continuum Knowledge Lifecycle

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-30

---

# 1. Purpose

The Knowledge Lifecycle defines how Continuum captures, evaluates, establishes, maintains, challenges, supersedes, retracts, and archives knowledge.

The central problem is:

> How does Continuum determine what it should believe about a software project, why it should believe it, how strongly it should believe it, and when that belief should change?

Continuum must distinguish between:

```text
statement
observation
claim
hypothesis
evidence
belief
knowledge
verified knowledge
authoritative knowledge
```

Without these distinctions, AI-generated statements can silently become project "truth," creating accumulated errors that become increasingly difficult to detect.

---

# 2. Core Principle

Continuum must never equate:

```text
being stated
```

with:

```text
being true
```

Nor:

```text
being retrieved frequently
```

with:

```text
being authoritative
```

Knowledge must have a lifecycle.

---

# 3. Knowledge Lifecycle

The conceptual lifecycle is:

```text
                 OBSERVATION
                      │
                      ▼
                   CLAIM
                      │
                      ▼
                 HYPOTHESIS
                      │
                      ▼
                   EVIDENCE
                      │
                      ▼
                 EVALUATION
                      │
             ┌────────┼────────┐
             ▼        ▼        ▼
          REJECTED  UNCERTAIN  SUPPORTED
                       │        │
                       └────┬───┘
                            ▼
                         VERIFIED
                            │
                            ▼
                      AUTHORITATIVE
                            │
                ┌───────────┼───────────┐
                ▼           ▼           ▼
             CURRENT     CHALLENGED   SUPERSEDED
                            │           │
                            ▼           ▼
                         REVIEWED     HISTORICAL
                            │
                      ┌─────┴─────┐
                      ▼           ▼
                   CONFIRMED   RETRACTED
```

This is a conceptual model.

Not every item must pass through every state.

---

# 4. Knowledge Is a Representation

Continuum should distinguish between:

```text
Reality
   ↓
Observation
   ↓
Representation
   ↓
Claim
   ↓
Knowledge
```

Knowledge is therefore not reality itself.

It is Continuum's current representation of what it believes to be true about the project.

---

# 5. Reality vs Knowledge

For example:

```text
Reality:
    authentication middleware actually validates JWTs.

Observation:
    source code contains JWT validation logic.

Claim:
    authentication middleware validates JWTs.

Knowledge:
    authentication middleware validates JWTs.
```

The knowledge representation is supported by evidence.

---

# 6. Statements

A Statement is something communicated by an actor.

Examples:

```text
Human:
    "We decided to use PostgreSQL."

AI:
    "This service probably uses Redis."

Tool:
    "The test suite contains 142 tests."

Documentation:
    "The system requires OAuth."
```

A Statement is not automatically a Claim accepted by Continuum.

---

# 7. Observations

An Observation represents something directly detected or recorded.

Examples:

```text
Git:
    commit abc123 exists.

Compiler:
    compilation failed.

Filesystem:
    src/auth.ts exists.

Test runner:
    test X failed.

Human:
    "I manually reproduced the error."
```

Observations should preserve their source and timestamp.

---

# 8. Claims

A Claim is a proposition about something.

Examples:

```text
service X depends on Redis
authentication requires OAuth
module A calls module B
deployment requires environment variable X
```

A Claim may be:

```text
true
false
uncertain
conditional
outdated
context-dependent
```

---

# 9. Hypotheses

A Hypothesis is a Claim explicitly treated as provisional.

Example:

```text
Observation:
    authentication tests fail.

Hypothesis:
    token expiration is causing the failures.
```

The hypothesis should not automatically become project Knowledge.

---

# 10. Evidence

Evidence is information that increases or decreases confidence in a Claim.

Examples:

```text
source code
configuration
test results
Git history
logs
documentation
human testimony
tool output
external documentation
deployment state
observed behavior
```

Evidence may support or contradict a Claim.

---

# 11. Evidence Is Not Truth

Evidence can itself be:

```text
incorrect
incomplete
stale
ambiguous
misinterpreted
malicious
untrusted
```

Therefore:

```text
Evidence
    ≠
Truth
```

Evidence contributes to evaluation.

---

# 12. Evidence Provenance

Each Evidence item should preserve:

```text
source
source type
origin
timestamp
collector
collection method
location
content reference
integrity information
```

This allows Continuum to determine where the Evidence came from.

---

# 13. Evidence Strength

Evidence may have different strength.

Conceptually:

```text
very strong
strong
moderate
weak
very weak
```

Strength depends on context.

A passing automated integration test may be stronger evidence for behavior than an AI's unsupported assertion.

---

# 14. Evidence Independence

Multiple evidence items are more valuable when they are independently derived.

For example:

```text
source code says X
test confirms X
runtime behavior confirms X
documentation says X
```

These provide stronger support than four AI-generated summaries all derived from the same original statement.

Continuum should eventually distinguish:

```text
independent evidence
```

from:

```text
duplicated evidence
```

---

# 15. Claim Evaluation

A Claim should be evaluated against available evidence.

Conceptually:

```text
Claim
  │
  ├── supporting evidence
  │
  ├── contradicting evidence
  │
  ├── source authority
  │
  ├── freshness
  │
  └── contextual applicability
          │
          ▼
       Evaluation
```

---

# 16. Evaluation Outcomes

Evaluation may produce:

```text
supported
strongly supported
weakly supported
uncertain
contradicted
rejected
verified
```

These are semantic outcomes, not necessarily final database statuses.

---

# 17. Confidence

Confidence describes how strongly Continuum should believe a Claim.

Example:

```text
Claim:
    service X uses PostgreSQL.

Confidence:
    0.97
```

Confidence is not necessarily a mathematically precise probability.

It is a structured representation of epistemic strength.

---

# 18. Confidence Must Have Reasons

A confidence score without explanation is insufficient.

Instead:

```text
Confidence:
    high

Reasons:
    source configuration confirms it
    integration tests confirm it
    deployment configuration confirms it
```

Continuum should preserve the reasoning behind confidence.

---

# 19. Confidence vs Authority

These must remain distinct.

Confidence asks:

> How likely is this Claim to be correct?

Authority asks:

> How much should this source or Claim govern project behavior?

Example:

```text
AI assertion:
    confidence = moderate
    authority = low

Accepted architecture decision:
    confidence = high
    authority = high
```

---

# 20. Confidence vs Importance

Importance asks:

> How consequential is this Claim?

A Claim can be:

```text
high confidence
high importance
```

or:

```text
high confidence
low importance
```

or:

```text
low confidence
high importance
```

High-importance, low-confidence Claims should receive attention.

---

# 21. Freshness

Knowledge may become stale.

Continuum should track freshness independently from confidence.

Example:

```text
Claim:
    project uses framework X.

Confidence:
    high

Freshness:
    low
```

The Claim may have been true but no longer be current.

---

# 22. Temporal Validity

Claims may have validity intervals:

```text
effective_from
effective_until
```

Example:

```text
Framework X:
    valid from January 1
    until April 10

Framework Y:
    valid from April 10
    onward
```

This preserves historical correctness.

---

# 23. Contextual Validity

Some Claims are only true under conditions.

Example:

```text
The service requires Redis.
```

May actually mean:

```text
The service requires Redis in production.
```

but not:

```text
The service requires Redis during local development.
```

Knowledge must therefore support contextual scope.

---

# 24. Scope

Claims may be scoped to:

```text
organization
project
repository
branch
environment
service
module
artifact
work item
session
time interval
```

A Claim must not automatically be generalized beyond its scope.

---

# 25. Claim Identity

Continuum should eventually determine when two Claims represent the same proposition.

For example:

```text
"PostgreSQL is our database."
"Our persistence layer uses PostgreSQL."
"We use PostgreSQL for storage."
```

These may represent substantially the same underlying Claim.

This enables consolidation.

---

# 26. Claim Structure

A conceptual Claim structure:

```text
Claim
├── subject
├── predicate
├── object
├── qualifiers
├── scope
├── temporal validity
├── confidence
├── authority
├── importance
├── provenance
└── evidence
```

This is not yet a final schema.

---

# 27. Claim Relationships

Claims may relate through:

```text
supports
contradicts
refines
qualifies
depends_on
derived_from
supersedes
superseded_by
duplicates
generalizes
specializes
```

These relationships are essential to knowledge evolution.

---

# 28. Derived Knowledge

Knowledge may be derived from other Knowledge.

Example:

```text
K1:
    service A uses PostgreSQL.

K2:
    PostgreSQL is unavailable in environment X.

Derived:
    service A cannot operate normally in environment X.
```

Derived Knowledge should preserve its dependency chain.

---

# 29. Derived Knowledge Is Conditional

Derived Knowledge should identify its premises.

```text
K3:
    service A cannot operate normally in environment X.

Depends on:
    K1
    K2
```

If either premise changes, K3 may require reevaluation.

---

# 30. Knowledge Dependency Graph

Conceptually:

```text
K1 ──────┐
         ├──→ K3
K2 ──────┘
```

A large knowledge graph can therefore contain chains of dependent conclusions.

---

# 31. Knowledge Invalidation

If supporting Knowledge becomes invalid:

```text
K1
 ↓
invalidated
 ↓
dependent claims
 ↓
re-evaluation
```

Continuum should not leave derived Knowledge silently standing on invalid premises.

---

# 32. Knowledge Propagation

A change can propagate through the Knowledge Graph.

Example:

```text
Database changed
      ↓
architecture changed
      ↓
configuration changed
      ↓
deployment procedure changed
      ↓
documentation changed
```

Continuum should eventually detect potentially affected Knowledge.

---

# 33. Knowledge Verification

Verification means obtaining sufficient evidence to establish a Claim according to a defined verification policy.

Verification is contextual.

For example:

```text
Code existence:
    filesystem inspection may be sufficient.

Behavior:
    automated test may be required.

Security property:
    stronger validation may be required.

Architecture decision:
    human approval may be required.
```

---

# 34. Verification Is Not Universal

There is no single universal verification mechanism.

Verification depends upon:

```text
claim type
risk
importance
authority requirements
available evidence
project policy
```

---

# 35. Verification Levels

A conceptual scale:

```text
UNVERIFIED
    ↓
OBSERVED
    ↓
SUPPORTED
    ↓
CORROBORATED
    ↓
VERIFIED
    ↓
AUTHORITATIVE
```

These represent increasing epistemic and governance strength.

---

# 36. Unverified

A Claim is unverified when:

```text
it has been asserted
but insufficient evidence exists.
```

Example:

```text
AI:
    "The service probably uses Redis."
```

This may remain useful but must not be represented as established truth.

---

# 37. Observed

A Claim may be considered observed when directly supported by an Observation.

Example:

```text
Observation:
    package.json lists redis.

Claim:
    project declares a Redis dependency.
```

Observation is stronger than unsupported assertion.

---

# 38. Supported

A Claim is supported when relevant Evidence provides meaningful justification.

Example:

```text
configuration
+
source code
+
tests
```

all indicate the same behavior.

---

# 39. Corroborated

A Claim is corroborated when multiple independent sources support it.

Example:

```text
source code
    +
integration test
    +
runtime observation
```

Corroboration increases confidence.

---

# 40. Verified

A Claim is Verified when it meets the verification criteria established for its type and context.

Verification should be explicit.

```text
verification_method
verification_actor
verification_time
verification_evidence
```

should be preserved.

---

# 41. Authoritative Knowledge

Authoritative Knowledge is Knowledge that Continuum is permitted to treat as governing project reality or policy.

Examples:

```text
accepted architecture decision
approved requirement
verified project configuration
current repository state
explicit human-approved constraint
```

Authority may require human approval depending on policy.

---

# 42. Authority Is Contextual

A human statement is not necessarily authoritative about every subject.

An automated compiler is highly authoritative about:

```text
whether a particular source revision compiles
```

but not:

```text
why the architecture was chosen
```

Authority therefore depends on domain.

---

# 43. Source Authority

Potential source categories:

```text
project owner
authorized human
repository state
automated tool
test system
deployment system
official project documentation
external documentation
AI agent
unknown source
```

Each may have different authority depending on Claim type.

---

# 44. Knowledge Acceptance

A Claim may become accepted Knowledge through:

```text
automated verification
human confirmation
policy-defined threshold
multiple independent evidence sources
formal validation
```

The acceptance mechanism should be recorded.

---

# 45. Human Confirmation

For important Claims, Continuum may require human confirmation.

Example:

```text
AI proposes:
    "Use PostgreSQL for all persistent state."

Continuum:
    Candidate Decision

Human:
    Accept
```

Only then does the proposal become authoritative project Knowledge.

---

# 46. AI Confirmation Is Not Human Confirmation

Multiple AI agents agreeing does not necessarily equal human authorization.

For example:

```text
Agent A → X
Agent B → X
Agent C → X
```

does not automatically mean:

```text
Human approved X
```

Consensus among AI systems is evidence, not necessarily authority.

---

# 47. Knowledge States

A practical state model may include:

```text
candidate
supported
verified
accepted
challenged
superseded
deprecated
retracted
archived
```

These states represent lifecycle status rather than confidence alone.

---

# 48. Candidate

Candidate Knowledge is potentially useful but not yet established.

```text
candidate
```

is an appropriate destination for AI-generated discoveries.

---

# 49. Supported

Supported Knowledge has meaningful evidence but may not yet satisfy project verification requirements.

---

# 50. Verified

Verified Knowledge has passed its defined verification process.

---

# 51. Accepted

Accepted Knowledge has been explicitly accepted under project governance.

Acceptance may be:

```text
human
automated
policy-driven
organizational
```

depending on the Claim.

---

# 52. Challenged

Knowledge becomes Challenged when credible evidence calls it into question.

Example:

```text
Current Knowledge:
    service uses PostgreSQL.

New Observation:
    deployment configuration uses MySQL.
```

The existing Knowledge should become:

```text
challenged
```

rather than silently overwritten.

---

# 53. Challenge Lifecycle

```text
CURRENT
   ↓
CHALLENGE
   ↓
INVESTIGATION
   ↓
┌──────────────┬──────────────┐
▼              ▼              ▼
CONFIRMED   CORRECTED      RETRACTED
   │              │              │
   └──────────────┼──────────────┘
                  ▼
              CURRENT
```

---

# 54. Superseded

Knowledge becomes Superseded when a newer representation replaces it.

Example:

```text
K1:
    use Redis

K2:
    use PostgreSQL

K2 supersedes K1
```

K1 remains historically important.

---

# 55. Supersession Does Not Mean Error

A Claim can be correct historically and still become superseded.

Example:

```text
January:
    Redis is used.

April:
    Redis is replaced by PostgreSQL.
```

The January Claim was not false.

It was temporally bounded.

---

# 56. Deprecated

Deprecated Knowledge remains potentially useful but should no longer guide new work.

Example:

```text
Old deployment procedure
```

may remain available for historical debugging while being excluded from current recommendations.

---

# 57. Retracted

Retracted Knowledge is determined to have been incorrect or invalid.

Retraction should preserve:

```text
original Claim
reason for retraction
evidence
decision
timestamp
actor
```

---

# 58. Archived

Archived Knowledge remains retained but is no longer active.

Archive is a lifecycle state, not necessarily a truth state.

---

# 59. Contradiction

Contradictions should be represented explicitly.

Example:

```text
K1:
    service uses PostgreSQL.

K2:
    service uses MySQL.

Relationship:
    contradicts
```

Continuum should not simply choose whichever Claim was created last.

---

# 60. Contradiction Resolution

Resolution may require:

```text
scope analysis
time analysis
source authority
evidence comparison
environment analysis
human judgment
```

The result may be:

```text
K1 is correct
K2 is correct
both are conditionally correct
one supersedes the other
both are invalid
insufficient information
```

---

# 61. Temporal Contradiction

Some apparent contradictions are actually temporal.

```text
K1:
    PostgreSQL used before migration.

K2:
    MySQL used after migration.
```

There is no contradiction if:

```text
K1.valid_until < K2.effective_from
```

---

# 62. Scope Contradiction

Some apparent contradictions are scope-dependent.

```text
K1:
    Redis is required in production.

K2:
    Redis is not required locally.
```

Both can be correct.

---

# 63. Knowledge Resolution

Continuum should distinguish:

```text
contradiction
```

from:

```text
qualification
```

Example:

```text
"System uses PostgreSQL."

"System uses PostgreSQL for transactional storage."
```

The second may refine rather than contradict the first.

---

# 64. Knowledge Consolidation

Multiple Claims may be consolidated.

Example:

```text
K1:
    PostgreSQL is used.

K2:
    PostgreSQL is used for transactional storage.

K3:
    PostgreSQL is required in production.
```

These may consolidate into:

```text
PostgreSQL is the production transactional datastore.
```

while preserving the original Claims.

---

# 65. Knowledge Compression

Consolidation should not destroy lineage.

The system should retain:

```text
canonical representation
+
supporting Claims
+
Evidence
+
derivation
```

This provides both efficiency and explainability.

---

# 66. Knowledge Provenance

Every important Knowledge object should answer:

```text
Where did this come from?
Who introduced it?
What evidence supports it?
When was it established?
What changed it?
What does it depend upon?
```

---

# 67. Provenance Graph

Conceptually:

```text
Source
  ↓
Observation
  ↓
Claim
  ↓
Evidence
  ↓
Evaluation
  ↓
Knowledge
  ↓
Decision
  ↓
Action
  ↓
Outcome
```

This is the backbone of explainable project history.

---

# 68. Knowledge Lineage

A Knowledge item should be traceable backward and forward.

Backward:

```text
Knowledge
    ↓
Evidence
    ↓
Observations
    ↓
Sources
```

Forward:

```text
Knowledge
    ↓
Decision
    ↓
Implementation
    ↓
Outcome
```

---

# 69. Knowledge Impact

Knowledge may influence:

```text
decisions
plans
code
tests
configuration
deployments
documentation
agent behavior
```

Continuum should eventually be able to identify those dependencies.

---

# 70. Knowledge Change Impact

When Knowledge changes:

```text
K1
 ↓
affected decisions
 ↓
affected work
 ↓
affected artifacts
 ↓
affected procedures
```

This enables semantic change impact analysis.

---

# 71. Knowledge Review

Important Knowledge may require periodic review.

Review triggers can include:

```text
age
staleness
dependency changes
contradictory evidence
security concerns
technology changes
project milestones
```

---

# 72. Knowledge Freshness Policies

Different Knowledge may have different freshness requirements.

For example:

```text
current branch:
    extremely fresh

dependency versions:
    fresh

architecture rationale:
    comparatively stable

historical decision:
    intentionally immutable
```

Freshness should therefore be policy-driven.

---

# 73. Knowledge Expiration

Some Knowledge can have explicit expiration.

Example:

```text
temporary workaround is valid until release 2.0
```

At expiration:

```text
active
   ↓
review required
```

rather than automatic deletion.

---

# 74. Knowledge Confidence Updating

Confidence may change as new evidence arrives.

Example:

```text
Initial:
    0.45

New test:
    0.72

Independent runtime observation:
    0.91

Contradictory evidence:
    0.63
```

The system should preserve the history of significant confidence changes.

---

# 75. Confidence Is Not Bayesian by Default

Continuum should not assume that every confidence update is mathematically Bayesian.

Different verification domains may use:

```text
rules
weights
scoring
human judgment
formal proofs
statistical methods
```

The epistemic model should remain flexible.

---

# 76. Knowledge Policies

Projects should be able to define policies such as:

```text
AI may create candidate Claims.
AI may not create authoritative architecture decisions.
Tests may automatically verify behavior Claims.
Security Claims require human approval.
Production state is authoritative for deployment status.
```

This creates a governance layer over Knowledge.

---

# 77. Knowledge Classes

Potential Knowledge classes include:

```text
descriptive
architectural
behavioral
procedural
requirement
constraint
decision
configuration
environment
operational
security
historical
derived
```

Different classes may have different lifecycle policies.

---

# 78. Descriptive Knowledge

Descriptive Knowledge answers:

> What exists?

Example:

```text
repository contains service A.
```

---

# 79. Behavioral Knowledge

Behavioral Knowledge answers:

> What does it do?

Example:

```text
service A retries failed requests three times.
```

---

# 80. Architectural Knowledge

Architectural Knowledge answers:

> How is the system structured and why?

Example:

```text
API gateway routes requests to bounded services.
```

---

# 81. Procedural Knowledge

Procedural Knowledge answers:

> How should work be performed?

Example:

```text
run migration verification before deployment.
```

---

# 82. Requirement Knowledge

Requirement Knowledge answers:

> What must be true?

Example:

```text
authentication must support OAuth.
```

---

# 83. Constraint Knowledge

Constraint Knowledge answers:

> What must not or cannot happen?

Example:

```text
production credentials must never be committed to Git.
```

---

# 84. Decision Knowledge

Decision Knowledge answers:

> What did we decide?

Example:

```text
PostgreSQL was selected over MongoDB.
```

Decision Knowledge must preserve rationale and alternatives.

---

# 85. Configuration Knowledge

Configuration Knowledge answers:

> How is the system configured?

Example:

```text
service A listens on port 8080.
```

Configuration should be cross-checked against actual artifacts where possible.

---

# 86. Environment Knowledge

Environment Knowledge describes runtime conditions.

Examples:

```text
local
CI
staging
production
developer workstation
container
Kubernetes cluster
```

Environment Knowledge is highly temporal.

---

# 87. Operational Knowledge

Operational Knowledge describes how the system behaves in operation.

Examples:

```text
deployment procedure
backup procedure
incident response
monitoring expectations
recovery procedure
```

---

# 88. Security Knowledge

Security Knowledge includes:

```text
security requirements
threat assumptions
security controls
trust boundaries
known vulnerabilities
security decisions
```

This class should generally have elevated governance requirements.

---

# 89. Historical Knowledge

Historical Knowledge describes what was once true.

Example:

```text
service used Redis before migration.
```

Historical Knowledge should remain available for forensic reasoning.

---

# 90. Derived Knowledge

Derived Knowledge is generated through reasoning over existing information.

It must preserve:

```text
premises
derivation method
derivation time
confidence
```

---

# 91. Knowledge vs Decision

These must remain distinct.

Knowledge:

```text
PostgreSQL is supported.
```

Decision:

```text
We choose PostgreSQL.
```

A Decision may depend on Knowledge but is not identical to it.

---

# 92. Knowledge vs Requirement

Knowledge:

```text
OAuth is currently implemented.
```

Requirement:

```text
OAuth must be implemented.
```

One describes reality.

The other describes desired reality.

---

# 93. Knowledge vs State

Knowledge:

```text
system uses PostgreSQL.
```

State:

```text
PostgreSQL is currently unavailable.
```

---

# 94. Knowledge vs Observation

Observation:

```text
configuration file contains DATABASE_URL.
```

Knowledge:

```text
the application uses a database connection defined by DATABASE_URL.
```

Knowledge may be derived from observations.

---

# 95. Knowledge vs Hypothesis

Hypothesis:

```text
DATABASE_URL failure is causing the outage.
```

Knowledge:

```text
the outage is caused by DATABASE_URL failure.
```

The second requires stronger evidence.

---

# 96. Knowledge Lifecycle and AI

AI systems are particularly prone to converting uncertainty into fluent assertions.

Continuum must therefore enforce a boundary:

```text
AI Generation
      ↓
Candidate Claim
      ↓
Evidence / Evaluation
      ↓
Knowledge
```

not:

```text
AI Generation
      ↓
Knowledge
```

---

# 97. AI Hallucination Containment

If an AI produces:

```text
"The project uses Kafka."
```

Continuum should be able to represent:

```text
Claim:
    project uses Kafka

Source:
    AI agent

Status:
    candidate

Confidence:
    unknown / low

Verification:
    pending
```

rather than immediately inserting it into authoritative project Knowledge.

---

# 98. Tool Evidence

Tool-generated evidence can be substantially stronger.

Example:

```text
grep:
    Kafka dependency found.

package manager:
    Kafka library installed.

source:
    Kafka client instantiated.
```

The Claim may then become strongly supported.

---

# 99. Automated Verification

Some Claims can be verified automatically.

Examples:

```text
file exists
dependency exists
test passes
build succeeds
endpoint responds
configuration key exists
Git branch exists
```

These should use deterministic verification where possible.

---

# 100. Human Verification

Some Claims require human judgment.

Examples:

```text
architecture preference
product intent
business requirement
design quality
organizational policy
strategic priority
```

Continuum must preserve human approval distinctly.

---

# 101. Verification Policies

A future verification policy may specify:

```text
Claim Type:
    security requirement

Required:
    evidence from source
    automated test
    human approval
```

Another:

```text
Claim Type:
    file existence

Required:
    filesystem observation
```

---

# 102. Knowledge Lifecycle Automation

Continuum may automate:

```text
candidate detection
evidence collection
contradiction detection
staleness detection
dependency analysis
verification
promotion
supersession detection
review scheduling
```

Automation should remain governed.

---

# 103. Knowledge Lifecycle Events

Significant transitions should produce events.

Examples:

```text
claim.created
evidence.attached
claim.supported
claim.verified
knowledge.accepted
knowledge.challenged
knowledge.corrected
knowledge.superseded
knowledge.retracted
knowledge.archived
```

These become part of Episodic Memory.

---

# 104. Knowledge Lifecycle Audit

A Knowledge item's history should be reconstructable.

Example:

```text
K42 created
    ↓
Evidence E11 attached
    ↓
Evidence E19 attached
    ↓
Verified
    ↓
Accepted by human
    ↓
Used in Decision D7
    ↓
Challenged by Observation O91
    ↓
Superseded by K88
```

---

# 105. Knowledge Lifecycle Example

Consider:

```text
AI:
    "The application uses Redis."
```

Continuum creates:

```text
Claim C1
Status:
    candidate
```

Then source inspection finds:

```text
package.json:
    redis dependency
```

Continuum adds Evidence E1.

Then source inspection finds:

```text
Redis client instantiated in cache module.
```

Evidence E2.

Then integration tests confirm Redis behavior.

Evidence E3.

C1 becomes:

```text
supported
```

and potentially:

```text
verified
```

depending on project policy.

---

# 106. Knowledge Change Example

Later:

```text
Redis dependency removed.
PostgreSQL cache implementation added.
```

Continuum detects:

```text
new evidence contradicts C1
```

C1 becomes:

```text
challenged
```

Investigation establishes:

```text
Redis was replaced by PostgreSQL.
```

Then:

```text
C1 → superseded by C2
```

C1 remains historical.

---

# 107. Knowledge Correction Example

Suppose the original Claim was simply wrong:

```text
AI:
    "Service A uses MongoDB."
```

Repository inspection shows no MongoDB anywhere.

The Claim becomes:

```text
retracted
```

with:

```text
reason:
    unsupported and contradicted by repository evidence
```

The system preserves the correction history.

---

# 108. Knowledge Lifecycle and Context

Context compilation should prefer:

```text
current
verified
authoritative
high-confidence
relevant
fresh
```

Knowledge.

It should normally exclude or clearly label:

```text
retracted
stale
challenged
low-confidence
unverified
```

information.

---

# 109. Context Must Preserve Epistemic Status

If uncertain information is included in Context, its uncertainty must remain visible.

Instead of:

```text
Redis is used by the application.
```

Context should potentially say:

```text
Candidate claim:
    Redis may be used by the application.

Evidence:
    AI statement only.

Verification:
    pending.
```

---

# 110. Knowledge Retrieval

Knowledge retrieval should support filters such as:

```text
status
confidence
authority
freshness
scope
type
validity
source
```

Example:

```text
Find authoritative current architecture knowledge
related to persistence.
```

---

# 111. Knowledge Ranking

A retrieval system may rank Knowledge according to:

```text
relevance
authority
confidence
freshness
importance
scope match
dependency proximity
verification status
```

The exact algorithm belongs to the future Retrieval Architecture.

---

# 112. Knowledge Explanation

Continuum should eventually support:

> Why do you believe this?

A useful answer should provide:

```text
Claim
 ↓
Evidence
 ↓
Verification
 ↓
Authority
 ↓
History
```

---

# 113. Knowledge Challenge Interface

An AI or human should be able to challenge Knowledge.

Example:

```text
Challenge:
    "I don't think this is true anymore."

Reason:
    "The deployment configuration changed."
```

This should initiate evaluation rather than simply replacing the Claim.

---

# 114. Knowledge Acceptance Interface

A human should be able to say:

```text
Accept this as project truth.
```

The system should record:

```text
actor
timestamp
Claim
scope
reason
```

---

# 115. Knowledge Retraction Interface

A human or authorized system may say:

```text
This is wrong.
```

The system should preserve:

```text
original Claim
retraction
reason
evidence
actor
timestamp
```

---

# 116. Knowledge Supersession Interface

A new Claim can explicitly supersede another:

```text
K2 supersedes K1.
```

This is preferable to deleting K1.

---

# 117. Knowledge Lifecycle Invariants

Continuum establishes:

1. A statement is not automatically Knowledge.
2. An AI assertion is not automatically Knowledge.
3. An Observation is not automatically Knowledge.
4. Evidence supports Claims but does not guarantee truth.
5. Claims may be provisional.
6. Hypotheses must remain distinguishable from established Knowledge.
7. Confidence and authority are separate dimensions.
8. Confidence and importance are separate dimensions.
9. Freshness and confidence are separate dimensions.
10. Knowledge may be temporally valid.
11. Knowledge may be contextually valid.
12. Knowledge must have scope.
13. Important Knowledge should have provenance.
14. Important Knowledge should have supporting Evidence.
15. Derived Knowledge should preserve its premises.
16. Invalidated premises may require downstream reevaluation.
17. Contradictions must not be silently erased.
18. Supersession must preserve historical lineage.
19. Retraction must preserve the fact that the Claim existed.
20. Historical truth remains distinct from current truth.
21. AI consensus is evidence, not automatically authority.
22. Human approval is distinct from AI agreement.
23. Verification requirements depend on Claim type.
24. Automated verification should be preferred where deterministic.
25. Human verification is appropriate where judgment is required.
26. Knowledge lifecycle transitions should be auditable.
27. Context should preserve epistemic status.
28. Retrieval should account for authority and freshness.
29. Knowledge should be explainable through provenance.
30. Knowledge should be challengeable.
31. Knowledge should be correctable.
32. Knowledge should be supersedable.
33. Knowledge should be archivable.
34. Knowledge should remain traceable to evidence.
35. Knowledge should remain traceable to its history.

---

# 118. The Knowledge Principle

The central principle of the Knowledge Lifecycle is:

> Continuum does not remember statements as truth. It maintains a continuously evolving, provenance-aware representation of what the project has sufficient reason to believe, including uncertainty, contradiction, historical validity, and the evidence that justifies those beliefs.

Therefore:

```text
                   STATEMENT
                       │
                       ▼
                   OBSERVATION
                       │
                       ▼
                     CLAIM
                       │
                       ▼
                   EVIDENCE
                       │
                       ▼
                  EVALUATION
                       │
              ┌────────┴────────┐
              ▼                 ▼
           UNCERTAIN         SUPPORTED
              │                 │
              └────────┬────────┘
                       ▼
                    VERIFIED
                       │
                       ▼
                  AUTHORITATIVE
                       │
             ┌─────────┼─────────┐
             ▼         ▼         ▼
          CURRENT   CHALLENGED  SUPERSEDED
                       │
                 ┌─────┴─────┐
                 ▼           ▼
              CORRECTED   RETRACTED
```

The objective is not merely to create a database of facts.

The objective is to create **a continuously self-correcting model of project understanding**.
