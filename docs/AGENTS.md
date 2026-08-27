# Agent Contracts

NovelIDE agents should have narrow responsibilities and structured outputs.

## Shared Rules

Every agent must:

1. receive an explicit task and bounded context;
2. distinguish canonical data from proposed data;
3. cite project entity/revision identifiers when making state-sensitive claims;
4. return structured output that can be validated;
5. never directly mutate canonical state;
6. surface uncertainty and conflicts instead of inventing resolutions;
7. respect protected constraints supplied by the workflow.

## Interviewer

### Purpose

Turn a vague premise into an actionable Story Intent through adaptive questions.

### Inputs

- premise;
- previous questions and answers;
- optional author preferences;
- interview budget/stop condition.

### Outputs

```yaml
questions:
  - id: q1
    text: string
    reason: string
    priority: high | medium | low

resolved_intent:
  genre: []
  tone: []
  themes: []
  protagonist: object
  central_conflict: string | null
  constraints: []

missing_decisions: []
ready_for_architecture: boolean
```

## Architect

### Purpose

Create or revise the structural foundation of the novel.

### Owns proposals for

- Story Bible;
- Style Bible;
- world rules;
- core cast;
- factions/locations;
- major plot threads;
- high-level ending constraints.

### Must not

- write full chapters;
- silently replace accepted canon;
- over-specify every future chapter.

## Planner

### Purpose

Translate high-level intent into hierarchical plans.

### Output contract

A chapter plan should contain at least:

```yaml
chapter_number: integer
objective: string
pov_character_id: uuid | null
participants: [uuid]
active_plot_threads: [uuid]
required_beats: []
required_setups: []
required_payoffs: []
protected_information: []
state_changes_expected: []
ending_function: string
prior_dependencies: []
```

## Character Agent

### Purpose

Reason from canonical state about character behavior without drafting prose.

### Questions it answers

- What does this character want now?
- What do they know at this chapter?
- What do they incorrectly believe?
- What emotional state carries into this scene?
- What actions are plausible under current constraints?

### Output

```yaml
characters:
  - character_id: uuid
    goals: []
    knowledge_available: []
    false_beliefs: []
    emotional_state: object
    likely_actions: []
    forbidden_actions: []
```

## Writer

### Purpose

Produce scene/chapter prose from an approved plan and compiled context.

### Must

- obey POV and style constraints;
- preserve protected reveals;
- avoid inventing unsupported canon when possible;
- flag necessary new facts for later extraction.

### Output

```yaml
prose: string
introduced_candidates:
  facts: []
  entities: []
notes: []
```

The candidates are not canon.

## Continuity Reviewer

### Purpose

Find state, timeline, causality and canon problems.

### Output

```yaml
issues:
  - severity: info | warning | error | blocking
    type: knowledge | timeline | location | world_rule | relationship | object_state | plot | other
    message: string
    evidence: []
    affected_entities: []
    suggested_resolution: string | null
```

Blocking issues prevent canon commit unless explicitly resolved.

## Style Reviewer

### Purpose

Review writing quality independently of canon correctness.

### Categories

- voice;
- pacing;
- repetition;
- dialogue;
- exposition;
- POV;
- scene construction;
- chapter ending.

### Output

Structured findings with locations/ranges where possible.

## Editor

### Purpose

Create a new revision from:

- current revision;
- author feedback;
- reviewer findings;
- protected constraints.

### Important

The Editor creates a **new child revision**. It does not overwrite its parent.

## Impact Analyzer

### Purpose

Predict downstream effects of a proposed revision.

### Inputs

- proposed revision;
- current graph;
- canonical facts;
- future plans/chapters;
- protected constraints.

### Outputs

- affected chapters;
- affected characters;
- affected plot threads;
- invalidated facts;
- risky dependencies;
- severity;
- recommended revalidation scope.

## Memory Curator

### Purpose

After a canon commit, derive compact memory artifacts.

### Produces

- chapter summary;
- scene summaries;
- retrieval chunks;
- importance scores;
- entity/plot-thread tags.

Derived memory must never override canonical source data.

## Graph Curator

### Purpose

Translate accepted narrative changes into proposed graph/state updates for deterministic commit processing.

### Produces

- nodes to create/update;
- edges to add/invalidate;
- timeline events;
- plot-thread transitions;
- setup/payoff transitions.

## Orchestrator

The Orchestrator is not a creative super-agent. It coordinates workflows.

Responsibilities:

- choose the next bounded agent task;
- build context through the Context Engine;
- validate agent outputs;
- persist run/proposal records;
- pause for human approval at required boundaries;
- resume interrupted workflows;
- never bypass canon commit rules.

## Provider Independence

Agent contracts should be independent of a specific model vendor.

A provider adapter may support:

- OpenAI-compatible APIs;
- Anthropic-compatible APIs;
- Gemini;
- Ollama/local inference;
- future providers.

Model routing may be configured per task, for example:

```yaml
routing:
  interviewer: fast-model
  architect: reasoning-model
  planner: reasoning-model
  writer: prose-model
  continuity_reviewer: reasoning-model
  style_reviewer: prose-review-model
  editor: prose-model
```
