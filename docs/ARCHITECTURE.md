# Architecture

## Overview

NovelIDE is designed around a **canonical project state** with explicit proposal and approval boundaries.

```text
User / Author
    ↓
CLI / Web IDE / MCP
    ↓
Agent Orchestrator
    ↓
Context Engine
    ↓
Specialized Agents
    ↓
Proposal Store
    ↓
Review / Diff / Impact Analysis
    ↓
Author Approval
    ↓
Canon Commit Service
    ↓
Canonical Stores
    ├── Manuscript
    ├── Story Bible
    ├── Character State
    ├── Plot Threads
    ├── Timeline
    ├── Story Graph
    ├── Memory Index
    └── Revision History
```

## Architectural Rules

### Rule 1 — proposals cannot mutate canon

All model output is initially non-canonical.

### Rule 2 — canon commits are atomic

Accepting a revision should update all derived state consistently or fail without a partial commit.

### Rule 3 — derived memory is reproducible

Summaries, embeddings and graph indexes should be rebuildable from canonical source data where practical.

### Rule 4 — temporal validity matters

Facts may be valid only across a chapter/time range.

### Rule 5 — historical state must be queryable

The system should be able to answer questions such as:

- What did character A know at Chapter 12?
- Where was item X after Chapter 8?
- When did relationship A/B change?
- Which revision introduced this fact?

## Suggested Core Modules

```text
core/
├── project
├── canon
├── revisions
├── graph
├── timeline
├── memory
├── context
├── agents
├── workflow
└── providers
```

### Project

Owns project configuration, identifiers and repository layout.

### Canon

Owns accepted story state and validation rules.

### Revisions

Owns proposals, parent-child revisions, diffs, approval status and restoration.

### Graph

Owns narrative entities and edges such as:

- participates_in;
- causes;
- reveals;
- depends_on;
- knows;
- owns;
- located_at;
- loves;
- distrusts;
- setup_for;
- payoff_of.

### Timeline

Owns chronological events and story-order references.

### Memory

Owns summaries, retrieval chunks and indexes.

### Context

Builds bounded context packs for specific agent tasks.

### Agents

Defines specialized agent contracts.

### Workflow

Orchestrates multi-step operations and human approval pauses.

### Providers

Abstracts LLM and embedding backends.

## Agent Runtime

A typical chapter generation run:

```text
Planner
  ↓
Character Reasoner
  ↓
Context Compiler
  ↓
Writer
  ↓
Continuity Reviewer
  ↓
Style Reviewer
  ↓
Editor (optional)
  ↓
Impact Analyzer
  ↓
Author Approval
  ↓
Canon Commit
  ↓
Memory Curator + Graph Curator
```

## Context Compiler

The Context Compiler should produce task-specific structured packages instead of raw transcript dumps.

Example chapter context:

```yaml
chapter: 18
intent:
  objective: "The confrontation fails but the Chapter 24 reveal remains protected"

constraints:
  - "Do not reveal the king's identity"
  - "Lan still believes Minh is innocent"

characters:
  - id: nam
    goal: "recover the letter"
    emotional_state: "desperate"
    knowledge:
      - "letter exists"

plot_threads:
  - id: secret-letter
    state: escalating
    protected_payoff_chapter: 24

recent_context:
  - chapter: 17
    summary: "..."

retrieved_facts:
  - "Nam injured his left hand in Chapter 11"
```

## Persistence Strategy

Initial implementation should favor local-first persistence.

A pragmatic first stack could be:

- Markdown/YAML for human-editable primary artifacts;
- SQLite for revisions, graph facts, temporal state and run metadata;
- optional vector index for semantic retrieval;
- content hashes for integrity and revision links.

The architecture should not depend on a hosted database.

## Event-Sourced Thinking

Canonical state changes can be represented as commit events:

```text
ChapterAccepted
CharacterStateChanged
RelationshipChanged
FactAsserted
FactInvalidated
PlotThreadAdvanced
TimelineEventAdded
StoryBibleRevised
```

This allows rebuilding projections and inspecting why the current state exists.

## UI Architecture Direction

The future IDE can be composed of synchronized views:

```text
┌───────────────┬─────────────────────────┬──────────────────┐
│ Project Tree  │ Manuscript / Graph      │ AI / Review      │
│               │                         │                  │
│ Story Bible   │ chapter text            │ conversation     │
│ Characters    │ graph canvas            │ proposals        │
│ Plot Threads  │ timeline                │ impact report    │
│ Chapters      │ diff/history            │ approve/reject   │
└───────────────┴─────────────────────────┴──────────────────┘
```

Graph and manuscript views must operate over the same underlying project state.
