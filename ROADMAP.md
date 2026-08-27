# NovelIDE Roadmap

## Phase 0 — Foundation

Goal: establish the project model and the rules that every future interface and agent must respect.

- [x] Define product vision.
- [x] Define high-level workflow.
- [ ] Define canonical data model.
- [ ] Define revision model.
- [ ] Define graph model.
- [ ] Define context-pack contract.
- [ ] Define agent input/output schemas.
- [ ] Choose initial implementation stack.

## Phase 1 — CLI MVP

Goal: build the smallest useful agentic novel-development loop.

### Project lifecycle

- [ ] `novel init`
- [ ] project configuration
- [ ] model/provider configuration
- [ ] local project persistence

### Guided interview

- [ ] premise intake
- [ ] adaptive clarifying questions
- [ ] author constraints
- [ ] genre/tone/theme extraction
- [ ] approval before Story Bible generation

### Story foundation

- [ ] Story Bible
- [ ] Style Bible
- [ ] character cards
- [ ] plot threads
- [ ] world rules
- [ ] editable artifacts

### Planning

- [ ] volume planning
- [ ] arc planning
- [ ] chapter planning
- [ ] scene planning
- [ ] dependency-aware plan validation

### Writing loop

- [ ] context-pack builder
- [ ] draft generation
- [ ] continuity review
- [ ] style review
- [ ] revision proposal
- [ ] user approval
- [ ] canon commit
- [ ] post-commit memory extraction

## Phase 2 — History and Story State

Goal: make changes inspectable and reversible.

- [ ] immutable revision records
- [ ] parent-child revision chain
- [ ] chapter history
- [ ] character-state history
- [ ] Story Bible history
- [ ] diff viewer data
- [ ] snapshot creation
- [ ] restore workflow
- [ ] alternate revision branches

Proposed commands:

```bash
novel log chapter 18
novel diff chapter 18 --from r12 --to r15
novel restore chapter 18 --revision r12
novel snapshot create "before-volume-2"
```

## Phase 3 — Continuity Engine

Goal: make long-form consistency a system capability rather than a prompt instruction.

- [ ] canonical fact store
- [ ] character state store
- [ ] relationship state
- [ ] location tracking
- [ ] item ownership tracking
- [ ] knowledge tracking
- [ ] timeline events
- [ ] future-information gate
- [ ] alive/dead constraints
- [ ] conflict detection
- [ ] contradiction reports

## Phase 4 — Memory and Retrieval

Goal: scale beyond the model context window.

- [ ] chapter summaries
- [ ] scene summaries
- [ ] structured facts
- [ ] semantic memory chunks
- [ ] hybrid retrieval
- [ ] temporal weighting
- [ ] entity-aware retrieval
- [ ] plot-thread-aware retrieval
- [ ] context budget manager

## Phase 5 — Visual Novel IDE

Goal: provide a real visual development environment.

### Workspace

- [ ] project explorer
- [ ] manuscript editor
- [ ] chapter/scene navigator
- [ ] AI chat/control panel
- [ ] review and approval panel
- [ ] diff/history panel

### Graphs

- [ ] event graph
- [ ] character relationship graph
- [ ] plot-thread graph
- [ ] chapter participation graph
- [ ] setup/payoff graph
- [ ] knowledge-flow graph

### Timeline

- [ ] chronological event view
- [ ] character-specific timeline
- [ ] story-order vs chronological-order comparison

## Phase 6 — Agent Runtime

Goal: support reliable multi-agent work with inspectable runs.

- [ ] Interviewer
- [ ] Architect
- [ ] Planner
- [ ] Character Agent
- [ ] Writer
- [ ] Continuity Reviewer
- [ ] Style Reviewer
- [ ] Editor
- [ ] Memory Curator
- [ ] Graph Curator
- [ ] run logs
- [ ] retry/recovery
- [ ] resumable workflows
- [ ] model routing by agent/task

## Phase 7 — External Agent Integration

- [ ] MCP server
- [ ] Claude Desktop integration
- [ ] coding-agent integration
- [ ] REST API
- [ ] automation hooks

## Phase 8 — Advanced Long-Form Features

- [ ] rolling arc planning
- [ ] hundreds/thousands of chapter support
- [ ] automatic downstream impact analysis
- [ ] alternative storyline branches
- [ ] merge/reconciliation tools
- [ ] series/sequel inheritance
- [ ] multi-book universe canon
- [ ] collaborative author workflows

## MVP Success Criteria

The first meaningful MVP is complete when a user can:

1. create a project from a vague premise;
2. answer guided questions;
3. approve a generated Story Bible and character set;
4. plan several chapters;
5. generate one chapter from structured context;
6. give revision feedback;
7. compare the new revision with the old one;
8. approve one revision as canon;
9. see character/timeline/plot state update only after approval;
10. continue to the next chapter with the accepted state.
