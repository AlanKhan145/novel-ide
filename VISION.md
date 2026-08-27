# NovelIDE Vision

## Product Definition

NovelIDE is an **agentic development environment for long-form fiction**.

It is intended for writers who want AI assistance without surrendering control of the manuscript, canon, character arcs or story structure.

The product should feel closer to an AI coding environment than to a chat-based story generator.

## The Problem

Most AI fiction tools optimize for generating text. Long-form fiction requires much more:

- consistent world rules;
- persistent character knowledge and state;
- causal plot dependencies;
- unresolved promises and foreshadowing;
- chapter-by-chapter planning;
- selective retrieval of prior context;
- controlled revisions;
- inspectable history;
- author approval before canon changes.

A model can write a good scene while still damaging the larger novel.

NovelIDE therefore treats prose generation as only one step in a larger development workflow.

## Product Principles

### 1. Author sovereignty

The model proposes. The author decides.

No generated prose, extracted fact, character-state change or plot mutation becomes canonical without an explicit acceptance boundary.

### 2. Canon is structured state

Canon is more than manuscript text.

It includes:

- accepted chapter prose;
- world rules;
- character identities and states;
- relationship states;
- timeline events;
- object ownership and locations;
- revealed and unrevealed knowledge;
- plot-thread state;
- promises, setup and payoff;
- narrative constraints.

### 3. Context is compiled, not dumped

The Writer should not receive the entire manuscript blindly.

The Context Engine builds a chapter-specific package from:

- global constraints;
- current planning artifacts;
- active characters;
- relevant graph facts;
- recent summaries;
- retrieved historical details;
- unresolved plot threads;
- forbidden contradictions.

### 4. Revision is first-class

Every meaningful generated or human-authored revision should be traceable.

A revision has:

- a parent;
- an author/agent;
- a reason;
- instructions;
- a diff;
- affected entities;
- affected plot threads;
- status: proposed, rejected, accepted or superseded.

### 5. Graphs explain the novel

The system should make structure visible.

Users should be able to inspect:

- event dependency graphs;
- character relationship graphs;
- plot-thread progress;
- timeline ordering;
- chapter-to-character participation;
- setup/payoff links;
- knowledge flow.

### 6. Agents have bounded responsibilities

Different tasks should be handled by specialized agents with explicit inputs and outputs instead of one giant prompt.

### 7. Long-form resilience

The system must survive:

- hundreds of chapters;
- interrupted generation;
- manual edits;
- regeneration of earlier material;
- branching experiments;
- changing model providers;
- partial failures;
- context-window limits.

## Intended Experience

A user can begin with something as vague as:

> I want to write a dark fantasy about a city where memories can be bought.

NovelIDE should not immediately generate Chapter 1.

It should enter an interview loop, discover intent and constraints, present an editable story architecture, and only then move into planning and drafting.

A later workflow might look like:

```text
User: Chapter 18 feels too easy. Make the confrontation fail,
but do not change the reveal planned for Chapter 24.

NovelIDE:
- identifies affected scenes and plot threads;
- checks downstream dependencies;
- proposes a Chapter 18 revision;
- previews changes to character state and graph state;
- reports which later chapter plans may be impacted;
- waits for approval.
```

That is the core experience: **story development with dependency awareness**.

## Long-Term Product Shape

NovelIDE may expose several interfaces over the same project model:

- CLI for agentic operation and automation;
- desktop/web IDE for visual editing;
- graph explorer;
- manuscript editor;
- timeline/state inspector;
- MCP server for external AI clients;
- API for integrations.

All interfaces should operate on the same canonical project state.
