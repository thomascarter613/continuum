# Continuum Session & Interaction Model

**Status:** Draft
**Version:** 0.1.0
**Date:** 2026-07-29

---

# 1. Purpose

The Session & Interaction Model defines how Continuum represents human-AI collaboration sessions and their relationship to persistent project knowledge.

The model exists to prevent a fundamental failure mode of AI-assisted software engineering:

> The project continues to exist, but the AI's understanding of the project disappears when a conversation ends, context is truncated, or work moves to another AI system.

Continuum therefore treats conversations as **temporary interaction surfaces over persistent project knowledge**, rather than as the project's memory itself.

---

# 2. Fundamental Principle

The project exists independently of any particular AI session.

Conceptually:

```text
Project
   │
   ├── Work
   │     │
   │     ├── Session A
   │     ├── Session B
   │     └── Session C
   │
   ├── Artifacts
   ├── Knowledge
   ├── Decisions
   ├── Requirements
   └── History
```

A session is therefore contextual and temporary.

The Project is persistent.

---

# 3. Session

A Session represents a bounded period of interaction or work involving one or more actors and systems.

Examples:

```text
AI coding session
architecture discussion
debugging session
planning session
code review session
research session
terminal-agent session
IDE assistant session
human-only engineering session
```

A Session may contain multiple Interactions and Activities.

---

# 4. Session Lifecycle

A Session may move through states such as:

```text
planned
active
paused
completed
abandoned
archived
```

Potential lifecycle:

```text
Session Created
      ↓
Session Started
      ↓
Interactions / Activities
      ↓
Session Paused
      ↓
Session Resumed
      ↓
Session Completed
      ↓
Session Archived
```

The exact lifecycle remains implementation-specific.

---

# 5. Session Identity

A Session must have a stable identity independent of the underlying AI provider or interface.

Potential metadata:

```text
session_id
project_id
work_id
started_at
ended_at
participants
platform
provider
model
client
status
purpose
```

Provider-specific conversation IDs may be stored as external identifiers.

---

# 6. Session vs Conversation

A Conversation is a communication stream.

A Session is a broader unit of work.

Therefore:

```text
Conversation ≠ Session
```

One Session may contain:

```text
ChatGPT conversation
terminal interaction
IDE interaction
Git operations
human notes
AI tool calls
```

Likewise, a single Conversation may contribute to more than one Activity.

---

# 7. Session vs Activity

A Session is a container for interaction.

An Activity is a unit of work.

Therefore:

```text
Session
   │
   ├── Activity
   ├── Activity
   └── Activity
```

Example:

```text
Session:
    "Authentication work"

Activities:
    investigate failure
    inspect repository
    redesign middleware
    implement change
    run tests
    document decision
```

---

# 8. Session Purpose

A Session may have an intended purpose.

Examples:

```text
planning
implementation
debugging
research
architecture
review
documentation
testing
refactoring
exploration
```

Purpose is metadata and should not be treated as proof of what actually happened.

---

# 9. Session Participants

Participants are actors participating in a Session.

Potential participants:

```text
human
AI model
AI agent
tool
service
external system
```

A Session may contain multiple AI systems.

Example:

```text
Session
   ├── Human
   ├── GPT-based assistant
   ├── local coding agent
   └── terminal
```

---

# 10. AI Provider

AI Provider identifies the external AI service or runtime.

Examples:

```text
OpenAI
Anthropic
Google
local model runtime
open-source model
custom model service
```

Provider identity must not become part of Continuum's canonical AI model.

It is an integration attribute.

---

# 11. AI Model

A Session may record the model used.

Examples:

```text
model identifier
model version
reasoning configuration
context limits
system configuration
```

Model identity is important because model behavior may vary over time.

---

# 12. Client

The client identifies the interface through which interaction occurred.

Examples:

```text
web application
desktop application
IDE
CLI
API
agent framework
editor extension
terminal
```

Client identity is distinct from AI Provider.

---

# 13. Interaction

An Interaction represents one meaningful exchange between participants.

Examples:

```text
human message
AI response
tool request
tool result
approval
rejection
question
answer
feedback
instruction
```

A Session contains one or more Interactions.

---

# 14. Interaction Structure

Conceptually:

```text
Interaction
├── interaction_id
├── session_id
├── actor
├── recipients
├── timestamp
├── type
├── content
├── context_reference
├── tool_calls
├── artifacts
└── events
```

The exact implementation remains open.

---

# 15. Message

A Message is a communication payload within an Interaction.

Potential message roles:

```text
system
developer
user
assistant
tool
agent
external
```

The canonical model should avoid tying these roles permanently to any one AI provider.

---

# 16. Interaction Sequence

A Session may be represented as a sequence:

```text
Human Message
      ↓
AI Response
      ↓
Tool Call
      ↓
Tool Result
      ↓
AI Response
      ↓
Human Feedback
      ↓
AI Response
```

This sequence provides conversational history.

However, conversational history is only one source of project context.

---

# 17. Conversation vs Project Memory

Continuum must explicitly distinguish:

```text
Conversation History
```

from:

```text
Persistent Project Knowledge
```

Conversation history is:

```text
temporary
verbose
provider-specific
context-window constrained
```

Persistent project knowledge should be:

```text
structured
durable
cross-session
cross-provider
selectively retrievable
provenance-aware
```

This distinction is foundational.

---

# 18. Session Context

Session Context is the information available to participants during a Session.

It may include:

```text
conversation history
project state
artifacts
requirements
decisions
claims
constraints
work items
previous session summaries
tool results
external information
```

Session Context is not necessarily identical to the Context Package delivered to an AI model.

---

# 19. Context Package

A Context Package is a compiled subset of available project information intended for a particular AI interaction or Activity.

Conceptually:

```text
Persistent Project Knowledge
          │
          ▼
    Context Selection
          │
          ▼
    Context Compilation
          │
          ▼
      Context Package
          │
          ▼
           AI
```

The Context Package should preserve references to its sources.

---

# 20. Context Provenance

A Context Package should identify:

```text
what information was included
where it came from
when it was compiled
why it was selected
what project state it represented
what constraints affected selection
```

This enables later reconstruction of:

> What did the AI actually know when it made this recommendation?

---

# 21. Context Snapshot

A Context Snapshot represents the effective knowledge and project state used for an interaction at a particular point in time.

Example:

```text
Session:
    Authentication redesign

Context Snapshot:
    repository revision = abc123
    architecture decision = DEC-014
    requirement = AUTH-001
    relevant files = 17
    relevant tests = 8
    known constraints = 5
```

This is distinct from the full project state.

---

# 22. Context Window

An AI model may impose a finite context window.

Continuum must not equate:

```text
Project Memory
```

with:

```text
Model Context Window
```

Instead:

```text
Project Memory
      │
      ▼
Context Selection
      │
      ▼
Context Compression / Compilation
      │
      ▼
Model Context Window
```

This is one of Continuum's central architectural principles.

---

# 23. Context Loss

Context may be lost because of:

```text
conversation truncation
context-window limits
session termination
provider switching
model switching
manual omission
incorrect summarization
stale information
```

Continuum should treat context loss as an engineering problem rather than an unavoidable property of AI systems.

---

# 24. Session Handoff

A Session may hand work to another Session.

Example:

```text
Session A
    ChatGPT
       │
       ▼
   Handoff
       │
       ▼
Session B
    Claude
       │
       ▼
Session C
    Local Agent
```

The new Session should not need the entire previous transcript.

Instead, Continuum should construct a handoff package from persistent knowledge and relevant Session state.

---

# 25. Handoff Package

A Handoff Package may include:

```text
current objective
current state
completed work
unfinished work
decisions
constraints
known problems
relevant artifacts
recent changes
test status
open questions
important evidence
recommended next actions
```

Handoff Packages should reference canonical project objects rather than duplicating information unnecessarily.

---

# 26. Session Summary

A Session Summary is a derived representation of important Session outcomes.

It may include:

```text
objective
work performed
decisions made
changes made
discoveries
problems
unresolved questions
next steps
```

A Session Summary is not automatically authoritative.

Important statements should be promoted into canonical Knowledge Objects when justified.

---

# 27. Session Summary vs Knowledge

A summary might say:

```text
"We decided to switch authentication providers."
```

That does not automatically make the statement authoritative.

The corresponding Decision should exist independently:

```text
Decision:
    DEC-014

Status:
    accepted

Rationale:
    ...

Evidence:
    ...
```

Thus:

```text
Summary → candidate information
Knowledge → canonical project memory
```

---

# 28. Session Artifacts

A Session may reference:

```text
files
directories
symbols
commits
branches
issues
tests
builds
documents
context packages
tool results
```

The Session should reference canonical Artifacts rather than creating disconnected copies.

---

# 29. Session Events

Session activity generates Events.

Examples:

```text
session started
interaction received
AI response generated
context compiled
tool invoked
file modified
test executed
decision proposed
decision accepted
session paused
session resumed
session completed
```

These integrate with the Event & Activity Model.

---

# 30. Session Activity Trace

A Session can therefore be represented as:

```text
Session
   │
   ├── Interaction
   │      └── Context
   │
   ├── Activity
   │      ├── Events
   │      └── Outputs
   │
   ├── Interaction
   │
   └── Activity
```

The Session is an organizing boundary, not the canonical source of project truth.

---

# 31. AI Response

An AI Response is an Interaction output.

An AI Response may contain:

```text
text
structured data
recommendations
claims
proposed decisions
proposed changes
tool calls
code
questions
warnings
```

AI-generated statements should not automatically become authoritative Knowledge.

---

# 32. AI Claim Extraction

Continuum may identify claims made by AI.

Example:

```text
AI:
    "The authentication failure is caused by token expiration."
```

Continuum may extract:

```text
Claim:
    authentication failure is caused by token expiration
```

The Claim should retain provenance:

```text
source = AI response
session = Session A
model = Model X
timestamp = ...
```

The Claim may then be evaluated against evidence.

---

# 33. AI Recommendation

An AI Recommendation is an AI-generated proposed action or judgment.

Examples:

```text
upgrade dependency
refactor module
change architecture
add test
modify configuration
```

Recommendations are not Decisions.

```text
Recommendation
      ↓
Human / Governance Review
      ↓
Decision
```

---

# 34. AI Proposed Change

An AI may propose a concrete software modification.

Example:

```text
AI
 ↓
proposes Change
 ↓
human reviews
 ↓
Change accepted/rejected
```

The proposed Change should remain distinguishable from the actual applied Change.

---

# 35. Human Approval

Human approval should be represented explicitly where relevant.

Examples:

```text
approved
rejected
modified
partially accepted
```

This creates an important distinction:

```text
AI proposed X
```

versus:

```text
Project actually adopted X
```

---

# 36. Session Boundaries

A Session boundary should not automatically imply a project boundary.

For example:

```text
Session A ends
Session B begins
```

does not mean:

```text
Project state resets
```

Instead:

```text
Project state continues
```

---

# 37. Cross-Provider Continuity

Continuum should support:

```text
ChatGPT → Claude
Claude → local model
local model → IDE agent
IDE agent → ChatGPT
```

without requiring each AI system to understand the previous provider's internal conversation format.

The canonical Continuum model is the interoperability layer.

---

# 38. Cross-Client Continuity

The same project may be accessed through:

```text
browser
desktop
IDE
CLI
terminal
API
agent framework
```

The Session model must remain client-independent.

---

# 39. Session Branching

A Session may branch into alternative approaches.

Example:

```text
Architecture Investigation
        │
        ├── Approach A
        │
        └── Approach B
```

These branches may later converge.

The model should preserve abandoned alternatives where they are useful for understanding decisions.

---

# 40. Session Merge

Multiple Sessions may contribute to the same Work Item.

Example:

```text
Session A:
    research

Session B:
    implementation

Session C:
    testing

Session D:
    review
```

All may contribute to:

```text
Work Item:
    Authentication redesign
```

---

# 41. Session State vs Project State

The distinction is critical.

Session State:

```text
what is happening in this interaction
```

Project State:

```text
what is true about the project
```

A Session may temporarily believe something that is later proven incorrect.

Therefore:

```text
Session State
    ≠
Project Truth
```

---

# 42. Session Knowledge Promotion

Important information discovered during a Session may be promoted into persistent Knowledge.

Conceptually:

```text
Interaction
    ↓
Candidate Observation / Claim
    ↓
Evaluation
    ↓
Evidence
    ↓
Persistent Knowledge
```

Promotion may be automatic, assisted, or human-approved depending on knowledge type.

---

# 43. Session Knowledge Demotion

Persistent Knowledge may later be found unreliable.

Continuum should support:

```text
confirmed
uncertain
contradicted
deprecated
superseded
retracted
```

A Session can discover evidence that changes the status of existing Knowledge.

---

# 44. Session Recovery

If a Session terminates unexpectedly, Continuum should be capable of recovering:

```text
active objective
last known state
recent interactions
recent Events
pending Activities
uncommitted changes
open questions
context snapshot
```

This allows another AI or human to resume work.

---

# 45. Session Resume

Resume should be modeled as reconstruction, not transcript replay.

Conceptually:

```text
Persistent Project State
        +
Relevant Knowledge
        +
Recent Session State
        +
Current Repository State
        +
Current Work Objective
        ↓
New Context Package
        ↓
New Session
```

The goal is not to recreate the old conversation.

The goal is to recreate the **necessary understanding**.

---

# 46. Session Compaction

Long Sessions may be compacted.

Compaction should preserve:

```text
important decisions
important discoveries
current state
unresolved problems
important evidence
artifact relationships
activity outcomes
```

Compaction should not silently discard canonical project Knowledge.

---

# 47. Session Forgetting

Continuum should support deliberate forgetting or omission of Session information.

Reasons may include:

```text
privacy
security
storage limits
irrelevance
sensitive information
user request
```

Forgetting session content must not automatically erase canonical project Knowledge derived from it.

---

# 48. Session Security

Session information may contain:

```text
source code
credentials
personal information
proprietary information
customer data
private discussions
AI prompts
tool arguments
```

Continuum must support appropriate:

```text
classification
authorization
redaction
retention
deletion
audit
```

---

# 49. External Conversation IDs

External AI platforms may provide identifiers.

Continuum may store:

```text
provider
external_session_id
external_message_id
external_timestamp
```

These identifiers are integration metadata.

They are not canonical Continuum identity.

---

# 50. Session Provenance

A Session should preserve enough provenance to answer:

```text
Who participated?

What AI was used?

What client was used?

What project state was available?

What Context Package was supplied?

What tools were used?

What changes occurred?

What Knowledge was created?

What decisions resulted?
```

---

# 51. Session Graph

Conceptually:

```text
                       PROJECT
                          │
                          ▼
                        WORK
                          │
             ┌────────────┴────────────┐
             ▼                         ▼
         SESSION A                 SESSION B
             │                         │
       ┌─────┼─────┐             ┌─────┼─────┐
       ▼     ▼     ▼             ▼     ▼     ▼
   CONTEXT  AI    TOOLS       CONTEXT  AI    TOOLS
       │     │     │             │     │     │
       └─────┼─────┘             └─────┼─────┘
             │                         │
             ▼                         ▼
           EVENTS                    EVENTS
             │                         │
             └──────────┬──────────────┘
                        ▼
                    KNOWLEDGE
                        │
                        ▼
                     STATE
```

---

# 52. Continuity Graph

The Session model enables the following continuity chain:

```text
Previous Session
       │
       ▼
Persistent Knowledge
       │
       ▼
Current Project State
       │
       ▼
Context Compilation
       │
       ▼
New Session
       │
       ▼
AI Activity
       │
       ▼
Events
       │
       ▼
Updated Knowledge
       │
       ▼
Next Session
```

This is the core continuity mechanism.

---

# 53. Context Is Compiled, Not Remembered

Continuum adopts the following principle:

> The AI does not need to remember the project. Continuum remembers the project and compiles the right understanding for the AI.

This means an AI can be replaced without destroying project continuity.

---

# 54. AI Provider Independence

Continuum must not depend on:

```text
ChatGPT memory
Claude projects
Cursor context
IDE conversation history
agent-specific memory
model-specific prompt formats
```

These may be adapters.

Continuum remains the canonical continuity layer.

---

# 55. Open Questions

The following remain unresolved:

1. Session schema
2. Interaction schema
3. Message normalization
4. Provider adapters
5. Client adapters
6. Context Package schema
7. Context Snapshot schema
8. Session handoff format
9. Session compaction strategy
10. Session recovery
11. Session branching
12. Session merging
13. Knowledge promotion
14. Knowledge demotion
15. AI claim extraction
16. AI recommendation extraction
17. AI proposed-change extraction
18. Session privacy
19. Session access control
20. Session retention
21. External conversation synchronization
22. Cross-provider identity
23. Context invalidation
24. Context freshness
25. Context sufficiency evaluation

---

# 56. Design Rules

Continuum establishes the following principles:

1. The Project exists independently of Sessions.
2. Sessions are bounded interaction/work periods.
3. Conversations are not equivalent to Sessions.
4. Sessions are not equivalent to Project Memory.
5. Activities represent work performed within Sessions.
6. Interactions represent communication within Sessions.
7. Context Packages are compiled from persistent project information.
8. Context Packages must retain provenance.
9. Conversation history is not equivalent to persistent Knowledge.
10. AI-generated statements are not automatically authoritative.
11. AI recommendations are not Decisions.
12. AI proposed Changes are not necessarily applied Changes.
13. Human approval should remain distinguishable from AI proposal.
14. Sessions may span multiple AI providers and clients.
15. Sessions may contribute to the same Work Item.
16. Session termination must not terminate project continuity.
17. Session recovery should reconstruct understanding rather than replay the entire transcript.
18. Session summaries are derived information, not necessarily canonical Knowledge.
19. Important Session discoveries may be promoted into persistent Knowledge.
20. Persistent Knowledge may later be contradicted or superseded.
21. Project continuity must survive AI-provider changes.
22. Context compilation is the mechanism connecting persistent project memory to finite AI context.
23. Continuum should remember the project; AI systems should consume compiled understanding.

---

# 57. Design Rule

The central purpose of the Session model is to eliminate the assumption that:

```text
AI conversation = AI memory = project memory
```

Instead:

```text
                    CONTINUUM
                        │
              Persistent Project Model
                        │
             ┌──────────┴──────────┐
             ▼                     ▼
        Session A               Session B
             │                     │
        AI Provider A          AI Provider B
             │                     │
             └──────────┬──────────┘
                        ▼
                 Shared Continuity
```

The project should remain continuous even when:

```text
the conversation ends
the context window fills
the model changes
the provider changes
the client changes
the AI agent changes
the human returns days later
```

That is the fundamental purpose of Continuum.
