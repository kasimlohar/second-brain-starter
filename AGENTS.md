# Second Brain — Agent Manual

This vault is a local-first second brain served over MCP by Basic Memory. Markdown files are the source of truth; the index is a rebuildable cache. Any MCP-capable agent may use this vault — Claude Code, Codex, Gemini CLI, OpenCode, Cursor, or another client. These rules apply regardless of which one is reading.

## Layout
- `00-Inbox/` — untriaged capture. `00-Inbox/Agent/` = default target for agent-created notes.
- `10-Projects/` — active outcomes, one folder per project, with a hub note.
- `20-Areas/` — ongoing responsibilities.
- `30-Resources/` — topics, sources, people.
- `40-Archive/` — inactive material. Never delete; move here.
- `90-Meta/` — schema, templates, maps of content, review queue.

## Note grammar
Frontmatter:
```yaml
---
title: Claim-shaped title
type: note | person | project | source | decision | daily
tags: [broad-topic, project-slug]
---
```
Body:
- `## Observations` — atomic facts, one per bullet, categorized:
  `[fact]`, `[decision]`, `[preference]`, `[todo]`, `[resource]`, `[question]`.
- `## Relations` — typed wiki-links: `relates_to [[X]]`, `requires [[Y]]`,
  `supersedes [[Z]]`, `owned_by [[Person]]`.

One idea per note. The title states the claim, not the topic. ISO dates.

## Rules
1. **Read before write.** Read the note, then edit it. Never blind-overwrite.
2. New knowledge goes to `00-Inbox/Agent/` unless the user names a path.
3. Show a short diff/summary before editing an existing note.
4. Never delete. Move inactive notes to `40-Archive/`; deletion is a human action.
5. Link every new note to at least one existing note. No orphans.
6. Prefer observations over prose; keep them atomic and categorized.
7. Record decisions with the *why*, not just the outcome.
8. Prefer existing notes over creating near-duplicates; search first.

## Retrieval
- For questions about the user or their work: search, walk the best hit's relations, then read it. Answer with `[[note titles]]` as citations.
- For "what have I been working on": check recent activity before searching.
- Basic Memory exposes tools such as `bm_search`, `bm_context`, `bm_read`, `bm_write`, `bm_recent`. Some clients prefix them with the server name; use whatever the client exposes.
- If the vault does not contain the answer, say so. Never invent vault contents.

## Writeback
- End of a work session: append `[decision]` / `[fact]` / `[todo]` observations to the relevant project hub, and link anything new.

## Multiple agents
- The five `/brain-*` commands are Claude Code-specific. Other clients follow these rules in prose.
- Let one agent own writeback. If several write, namespace captures under `00-Inbox/Agent/<client>/`.
- Every client runs its own server process against the same files and index. A write from one is searchable by the next within seconds, but conversations never sync.
- Deletion stays disabled for every client.
