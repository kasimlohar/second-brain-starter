# Note Grammar

Every note is an **entity**: a project, person, decision, source, or topic. An entity carries **observations** (atomic, categorized claims) and **relations** (typed links to other entities).

## Frontmatter

```yaml
---
title: Note Title
type: note            # project | person | decision | source | note
tags: [topic, area]
---
```

`title` must match the H1 and the filename. Keep tags few and reusable.

## Observations

One claim per line, prefixed with a category:

| Category | Use for |
|---|---|
| `[fact]` | Verifiable state of the world |
| `[decision]` | A choice, with the why |
| `[preference]` | How you want work done |
| `[todo]` | A concrete next action |
| `[resource]` | A pointer to a source |
| `[question]` | An open unknown |

```markdown
## Observations
- [fact] The vault is indexed locally with FastEmbed.
- [decision] Chose Basic Memory over a hosted memory API — local-first, plain files.
- [todo] Seed 25 real notes.
```

## Relations

Typed wiki-links, one per line:

```markdown
## Relations
- uses [[Basic Memory]]
- relates_to [[Second Brain Format]]
- supersedes [[Old Stack Decision]]
```

Common types: `relates_to`, `requires`, `supersedes`, `owned_by`, `affects`, `part_of`, `uses`.

## Why this shape

Prose is hard to retrieve precisely and easy to misread. Atomic observations let a search return a sentence rather than a document, and typed relations let an agent follow meaning instead of relying on similarity alone.

A note without observations is a stub. A note without relations is an orphan. Both are signals to fix, not styles to prefer.
