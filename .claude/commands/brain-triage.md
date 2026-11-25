---
description: Turn inbox material into entity notes, with approval first
argument-hint: [optional inbox file or focus]
---

Triage the vault inbox into entity notes.

1. List `00-Inbox/` and read the file named in `$ARGUMENTS` if one is given. If several inbox items exist, ask which to triage before reading them all.
2. Read `CLAUDE.md`, `90-Meta/schema.md`, and any relevant project hub.
3. Propose a table — title, type, observation count, relations, target folder — and write nothing yet. Target mix: project hubs, decisions, people, sources, topic notes.
4. After approval, write notes to `00-Inbox/Agent/Triage/` with `bm_write`. Every note needs at least three observations and two relations to existing notes.
5. Never delete or move the source material. Note in each new note where it came from.
