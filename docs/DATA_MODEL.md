# Data Model

## Goals

The NovelIDE data model must support:

- long-form continuity;
- historical state queries;
- author-controlled canon;
- reversible revisions;
- plot and character graphs;
- retrieval for future chapters;
- impact analysis when earlier content changes.

## Core Entities

### Project

```yaml
Project:
  id: uuid
  title: string
  premise: string
  language: string
  genre: [string]
  status: planning | writing | complete | archived
  created_at: datetime
  updated_at: datetime
```

### StoryArtifact

Generic versioned artifact for Story Bible, Style Bible, world rules and similar documents.

```yaml
StoryArtifact:
  id: uuid
  type: story_bible | style_bible | world_rule | faction | location | terminology
  key: string
  current_revision_id: uuid
```

### Character

```yaml
Character:
  id: uuid
  name: string
  aliases: [string]
  role: string
  first_appearance_chapter: integer | null
  status: active | inactive | dead | unknown
```

### CharacterState

Character state is temporal and revision-aware.

```yaml
CharacterState:
  id: uuid
  character_id: uuid
  valid_from_chapter: integer
  valid_to_chapter: integer | null
  location: string | null
  physical_state: object
  emotional_state: object
  goals: [string]
  knowledge: [FactRef]
  possessions: [EntityRef]
  relationship_snapshot: object
  source_revision_id: uuid
```

### PlotThread

```yaml
PlotThread:
  id: uuid
  title: string
  type: main | subplot | mystery | romance | thematic | setup_payoff
  status: planned | active | paused | resolved | abandoned
  introduced_chapter: integer | null
  target_resolution_chapter: integer | null
  priority: integer
```

### Chapter

```yaml
Chapter:
  id: uuid
  number: integer
  volume_id: uuid | null
  arc_id: uuid | null
  title: string
  outline_revision_id: uuid | null
  canonical_revision_id: uuid | null
  status: planned | drafting | review | accepted | locked
```

### Scene

```yaml
Scene:
  id: uuid
  chapter_id: uuid
  order: integer
  pov_character_id: uuid | null
  location_id: uuid | null
  objective: string
  conflict: string
  outcome: string
```

## Revision Model

Every mutable narrative artifact should use the same core revision abstraction.

```yaml
Revision:
  id: uuid
  entity_type: chapter | scene | story_artifact | character | plot_thread
  entity_id: uuid
  parent_revision_id: uuid | null
  branch_id: uuid | null
  author_type: human | agent | system
  author_name: string
  reason: string
  instruction: string | null
  content_hash: string
  created_at: datetime
  status: proposed | accepted | rejected | superseded
  affected_entities: [EntityRef]
  affected_plot_threads: [uuid]
```

The stored content may be Markdown, structured JSON or both depending on entity type.

## Canon Commit

A CanonCommit is the acceptance boundary.

```yaml
CanonCommit:
  id: uuid
  revision_id: uuid
  chapter_id: uuid | null
  message: string
  approved_by: string
  created_at: datetime
  previous_commit_id: uuid | null
```

A commit may generate multiple state transitions atomically.

## Canon Fact

Facts should have temporal validity and provenance.

```yaml
CanonFact:
  id: uuid
  subject_id: uuid
  predicate: string
  object: scalar | EntityRef
  valid_from_chapter: integer
  valid_to_chapter: integer | null
  confidence: float
  source_commit_id: uuid
  source_scene_id: uuid | null
  status: active | invalidated
```

Examples:

```text
(Nam, located_at, Da Nang)
(Lan, knows, secret_letter_exists)
(Sword01, owned_by, Minh)
(Minh, alive, false)
```

## Graph Model

### NarrativeNode

Node types may include:

- character;
- event;
- scene;
- chapter;
- location;
- object;
- faction;
- plot thread;
- secret;
- setup;
- payoff.

### NarrativeEdge

```yaml
NarrativeEdge:
  id: uuid
  from_node_id: uuid
  to_node_id: uuid
  type: string
  valid_from_chapter: integer | null
  valid_to_chapter: integer | null
  source_commit_id: uuid
  metadata: object
```

Common edge types:

```text
participates_in
causes
depends_on
reveals
knows
owns
located_at
member_of
loves
distrusts
setup_for
payoff_of
contradicts
```

## Event Model

```yaml
StoryEvent:
  id: uuid
  title: string
  chronological_order: number | null
  story_order: number
  chapter_id: uuid
  scene_id: uuid | null
  participants: [uuid]
  location_id: uuid | null
  causes: [uuid]
  consequences: [uuid]
  source_commit_id: uuid
```

Separating chronological order from story order allows flashbacks, nonlinear narratives and delayed reveals.

## Knowledge Model

Knowledge should be tracked per character for mystery and information-sensitive stories.

```yaml
KnowledgeState:
  character_id: uuid
  fact_id: uuid
  acquired_chapter: integer
  acquired_scene_id: uuid | null
  certainty: known | believes | suspects | false_belief
  source_commit_id: uuid
```

This allows a continuity rule such as:

> A character cannot act on a fact before the chapter in which they learned it.

## Plot Thread Progress

```yaml
PlotThreadEvent:
  id: uuid
  plot_thread_id: uuid
  chapter_id: uuid
  scene_id: uuid | null
  action: introduced | advanced | complicated | paused | resolved | reopened
  note: string
  source_commit_id: uuid
```

## Setup and Payoff

```yaml
SetupPayoff:
  id: uuid
  setup_event_id: uuid
  payoff_event_id: uuid | null
  plot_thread_id: uuid | null
  intended_payoff_chapter: integer | null
  status: planted | reinforced | paid_off | abandoned
```

This makes forgotten foreshadowing queryable.

## Memory Chunk

```yaml
MemoryChunk:
  id: uuid
  source_commit_id: uuid
  chapter_id: uuid | null
  entity_ids: [uuid]
  plot_thread_ids: [uuid]
  text: string
  embedding_ref: string | null
  temporal_weight: float
  importance: float
```

Memory chunks are derived data, not primary canon.

## Run and Proposal Records

```yaml
AgentRun:
  id: uuid
  task_type: interview | plan | write | review | revise | extract | impact_analysis
  agent_name: string
  model: string
  input_snapshot_id: uuid
  status: running | completed | failed | cancelled
  started_at: datetime
  completed_at: datetime | null
```

```yaml
Proposal:
  id: uuid
  run_id: uuid
  entity_type: string
  entity_id: uuid | null
  revision_id: uuid | null
  impact_report_id: uuid | null
  status: pending | accepted | rejected
```

## Impact Report

When an earlier chapter changes, NovelIDE should calculate downstream effects.

```yaml
ImpactReport:
  id: uuid
  proposed_revision_id: uuid
  affected_chapters: [uuid]
  affected_characters: [uuid]
  affected_plot_threads: [uuid]
  invalidated_facts: [uuid]
  risky_dependencies: [NarrativeEdgeRef]
  protected_constraints: [string]
  severity: low | medium | high | blocking
```

## Important Invariants

1. A proposed revision does not alter canonical state.
2. Every canonical fact has provenance.
3. Temporal facts cannot overlap incompatibly without conflict resolution.
4. Historical character state remains queryable after updates.
5. Derived memory can be rebuilt from accepted commits.
6. A restored old revision becomes a new commit; history is never erased.
7. Downstream impact must be calculated before accepting changes to already-canonical earlier chapters.
