# Workflow

## 1. Project Creation

The user begins with a premise, which may be incomplete.

```text
User premise
   ↓
Interviewer
   ↓
Clarifying questions
   ↓
Author answers
   ↓
Story Intent
```

The interviewer should discover only information that materially improves planning, such as:

- genre and subgenre;
- tone;
- target audience;
- expected length;
- core protagonist;
- central conflict;
- themes;
- desired ending direction;
- content constraints;
- pacing preferences;
- inspirations to emulate at a high level without copying protected text.

The user can stop the interview and request a draft architecture at any time.

## 2. Story Architecture

The Architect proposes:

- Story Bible;
- Style Bible;
- World rules;
- character cards;
- factions and locations;
- plot threads;
- high-level ending constraints.

Every artifact is editable before acceptance.

```text
Architect proposal
       ↓
Author review
   ┌───┼────┐
   ▼   ▼    ▼
 Edit Reject Accept
              ↓
        Canon foundation
```

## 3. Planning

Planning should be hierarchical.

```text
Novel
  ↓
Volumes
  ↓
Arcs
  ↓
Chapters
  ↓
Scenes
```

Higher-level plans are constraints and intent, not immutable prose specifications.

Each chapter plan should include:

- chapter objective;
- POV;
- participating characters;
- active plot threads;
- required setups/payoffs;
- expected state transitions;
- protected information;
- chapter ending function;
- dependencies on prior events.

## 4. Pre-write Context Compilation

Before a chapter is written, NovelIDE compiles only relevant context.

```text
Story Bible
Style Bible
Chapter Plan
Recent Accepted Chapters
Character State
Knowledge State
Plot Threads
Graph Facts
Timeline
Retrieved Memories
Continuity Constraints
        ↓
   Context Pack
```

The pack is inspectable by the user.

## 5. Drafting

The Writer creates a proposal, not canon.

```text
Context Pack
    ↓
Writer
    ↓
Draft Revision R1
```

The system records:

- agent/model;
- prompt inputs or reproducible references;
- context snapshot;
- revision parent;
- token/runtime metadata where available.

## 6. Automated Review

The draft passes through independent review stages.

### Continuity review

Checks include:

- invalid character knowledge;
- impossible locations;
- dead/inactive character violations;
- item ownership conflicts;
- timeline conflicts;
- broken world rules;
- contradictory relationships;
- missed protected constraints;
- premature reveals;
- setup/payoff problems.

### Style review

Checks include:

- voice drift;
- repeated phrasing;
- pacing;
- exposition load;
- dialogue quality;
- POV consistency;
- unwanted summary instead of scene;
- chapter-ending effectiveness.

Reviews annotate the revision; they do not directly mutate it.

## 7. Human Feedback Loop

The author may provide natural-language feedback.

```text
Author:
"The argument is too easy. Let Lan win the exchange, but do not reveal the letter yet."
```

The Editor must convert this into an explicit revision instruction and constraints.

```text
Current R1
+ Author Feedback
+ Review Findings
+ Protected Constraints
       ↓
Editor
       ↓
Proposed R2
       ↓
Diff + Impact Report
```

The author can:

- accept R2;
- edit R2 manually;
- reject it;
- request another revision;
- compare with earlier revisions.

## 8. Impact Analysis

Any revision may change story meaning. Earlier canonical chapters require stricter analysis.

Example:

```text
Change Chapter 8
      ↓
Graph dependency traversal
      ↓
Potential impacts
  ├── Chapter 12 assumes Nam has the key
  ├── Chapter 15 references the old wound
  ├── Plot thread P03 payoff depends on Event E81
  └── Lan's knowledge state changes
```

Blocking conflicts should be shown before acceptance.

## 9. Canon Commit

Once the author approves a revision, NovelIDE performs an atomic canon commit.

```text
Approved Revision
       ↓
Validate
       ↓
Canon Commit
       ├── manuscript pointer update
       ├── fact transitions
       ├── character-state transitions
       ├── relationship transitions
       ├── timeline events
       ├── plot-thread progress
       ├── graph edges/nodes
       └── revision history
```

If validation fails, no partial canon update should remain.

## 10. Post-commit Derivation

After a successful commit, derived systems update:

- chapter/scene summary;
- retrieval memory chunks;
- embeddings;
- graph projections;
- timeline projections;
- searchable indexes.

These derived artifacts must be rebuildable.

## 11. Continue

The next chapter uses the new accepted state.

```text
Canon N
   ↓
Compile Chapter N+1 context
   ↓
Plan / Write / Review / Revise / Approve
   ↓
Canon N+1
```

## Revision Workflow as Git-like Mental Model

```text
canonical chapter r4
       │
       ├── proposal r5a — rejected
       │
       └── proposal r5b
              │
              ├── manual edit r6
              │       ↓
              │    accepted
              │
              └── alternative r5c — retained as branch
```

NovelIDE does not need to literally use Git internally for narrative revisions, but the experience should preserve the useful concepts:

- immutable history;
- parent revisions;
- diff;
- branches;
- restore;
- commit messages;
- inspectable provenance.

## Agent Safety Boundaries

Agents must not silently:

- accept their own output as canon;
- delete history;
- rewrite unrelated chapters;
- update character state from rejected prose;
- expose future knowledge to a character;
- ignore protected plot constraints;
- resolve high-impact contradictions without author approval.

## Example End-to-End Session

```text
> novel init
Premise: A detective can enter other people's memories.

Interviewer:
What is the cost of entering a memory?

User:
Each visit erases one of his own memories.

Interviewer:
Should this cost be known to other characters at the start?

User:
No.

...

Architect:
Story Bible proposal ready.

User:
Approve, but make the setting contemporary Seoul instead of London.

...

> novel plan chapter 1
> novel write chapter 1

Reviewer:
1 continuity warning, 2 style suggestions.

User:
Make the opening less explanatory and show the power through action.

Editor:
Revision R3 prepared.
Diff: 42 additions, 38 removals.
Canon impact: low.

User:
Approve.

NovelIDE:
Chapter 1 committed.
Character states: 3 updates.
Timeline: 2 events added.
Plot thread "Memory Cost" introduced.
```
