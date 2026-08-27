# NovelIDE

**An agentic development environment for long-form fiction.**

NovelIDE treats a novel like a software project: requirements become story intent, architecture becomes the story bible, modules become volumes and chapters, runtime state becomes character/world state, tests become continuity checks, and version control preserves every meaningful revision.

> **Core idea:** write novels with an AI workflow closer to Claude Code / Cursor than to a one-shot text generator.

## Product Goals

NovelIDE is designed for serialized and long-form fiction where the author needs to stay in control across dozens, hundreds, or potentially thousands of chapters.

The system should support:

- guided idea development through AI questions before generation;
- Story Bible, Style Bible, world rules, factions, locations, terminology and constraints;
- character cards, relationships, goals, knowledge, emotional state and state history;
- hierarchical planning: novel → volume → arc → chapter → scene;
- visual story graphs for events, plot threads, characters and dependencies;
- draft → review → feedback → revision → approval workflows;
- explicit separation between **draft/proposal** and **canon**;
- continuity checking before and after writing;
- long-term structured memory plus retrieval;
- chapter-level and entity-level version history;
- diff, restore, branch and snapshot concepts inspired by Git;
- multi-agent collaboration with the author as final authority;
- CLI-first automation plus a visual workspace.

## Conceptual Mapping

| Software development | NovelIDE |
| --- | --- |
| Requirements | Premise, genre, themes, author intent |
| Architecture | Story Bible / narrative architecture |
| Modules | Volumes / arcs |
| Source files | Chapters / scenes |
| Runtime state | Character / world state |
| Dependencies | Plot threads / causal links |
| Tests | Continuity / canon checks |
| Linter | Style and prose review |
| Git history | Revision history |
| Branch | Alternative storyline / revision path |
| IDE debugger | Timeline, graph and state inspector |
| Coding agent | Planner / Writer / Reviewer / Editor agents |

## Target Workflow

```text
Initial idea
    ↓
Guided interview
    ↓
Story intent + constraints
    ↓
Story Bible / Characters / Plot Threads
    ↓
Visual narrative graph
    ↓
Volume → Arc → Chapter → Scene planning
    ↓
Context build
    ↓
Draft generation
    ↓
Continuity + style review
    ↓
Author feedback
    ↓
Revision preview
    ↓
Approve / Reject / Edit
    ↓
CANON COMMIT
    ↓
Update timeline + character state + plot threads + memory
    ↓
Next chapter
```

## Core Principle: Draft Is Not Canon

AI-generated content must never silently mutate the canonical story state.

```text
AI proposal
   ↓
Draft revision
   ↓
Review
   ↓
Author approval
   ↓
Canon commit
```

Only an accepted revision may update facts, character states, timeline events, relationships or plot-thread progress.

## Planned Agents

- **Interviewer** — develops vague ideas by asking focused questions.
- **Architect** — creates and maintains Story Bible and narrative constraints.
- **Planner** — plans volumes, arcs, chapters and scenes.
- **Character Agent** — reasons about goals, knowledge, emotion and likely actions.
- **Writer** — produces prose from an approved plan and context pack.
- **Continuity Reviewer** — checks contradictions and missing dependencies.
- **Style Reviewer** — evaluates voice, pacing, repetition and prose quality.
- **Editor** — creates controlled revision proposals from feedback.
- **Memory Curator** — extracts facts, summaries and state changes after approval.
- **Graph Curator** — maintains events, relations, causal links and plot-thread graph state.

## Planned Project Model

```text
novel-project/
├── project.yaml
├── story/
│   ├── story-bible.md
│   ├── style-bible.md
│   ├── world/
│   ├── characters/
│   └── plot-threads/
├── manuscript/
│   ├── volume-001/
│   │   ├── chapter-001/
│   │   │   ├── outline.md
│   │   │   ├── scenes/
│   │   │   └── chapter.md
│   │   └── ...
│   └── ...
├── graph/
│   ├── events.jsonl
│   ├── relations.jsonl
│   └── timeline.jsonl
├── memory/
├── reviews/
├── revisions/
└── snapshots/
```

The physical layout may evolve; the important rule is that canonical content, proposals, reviews, state and history remain inspectable.

## Planned CLI

```bash
novel init
novel interview
novel architect
novel plan
novel plan chapter 12
novel write chapter 12
novel review chapter 12
novel revise chapter 12
novel approve chapter 12

novel graph plot
novel graph characters
novel timeline
novel state character <name>

novel log chapter 12
novel diff chapter 12
novel restore chapter 12 --revision <id>
```

## Design Influences

NovelIDE studies useful ideas from open-source long-form fiction systems such as ABook, AuthorOS, ElyHa, NovelForge, Novel Studio AI, NovelClaw, ainovel-cli, AI-Novel and traditional writing tools such as Manuskript.

The goal is not to clone any single project, but to combine several proven concepts into one coherent authoring environment:

- guided Q&A and human-assisted generation;
- coding-like CLI workflows;
- visual narrative graphs;
- structured canon and character state;
- retrieval-based long-term memory;
- inspectable multi-agent runs;
- revision history and author-controlled commits.

## Status

**Phase: foundation / architecture design.**

See:

- [`VISION.md`](VISION.md)
- [`ROADMAP.md`](ROADMAP.md)
- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
- [`docs/DATA_MODEL.md`](docs/DATA_MODEL.md)
- [`docs/WORKFLOW.md`](docs/WORKFLOW.md)

## Project Philosophy

> The AI may propose. The author decides. The canon remembers.
