# Vault Operating Manual

This vault is a local-first second brain. Markdown files are the source of truth; the Basic Memory index is a cache that can be rebuilt.

## Layout

- `00-Inbox/` — unfiled capture. `00-Inbox/Agent/` is the agent's default write target.
- `10-Projects/` — active outcomes with a defined end.
- `20-Areas/` — ongoing responsibilities without an end date.
- `30-Resources/` — topics, sources, people, references.
- `40-Archive/` — inactive material. Nothing is deleted.
- `90-Meta/` — the schema, templates, and evaluation questions.

## Agent rules

1. **Read before write.** Search first, read the notes you intend to change.
2. **New knowledge goes to `00-Inbox/Agent/`.** Do not file into projects or areas without approval.
3. **Never delete.** Move inactive notes to `40-Archive/`. Deletion is disabled by default and stays that way.
4. **Show the diff before editing** an existing note. Never overwrite by title collision.
5. **Link every new note** to at least one existing note with a typed relation.
6. **One idea per note** with a claim-shaped title.
7. **Atomic observations**, categorized as `[fact]`, `[decision]`, `[preference]`, `[todo]`, `[resource]`, or `[question]`.
8. **Decisions record the why** — the rationale, alternatives, and consequence, not just the outcome.

## Retrieval

1. `bm_search` with a hybrid query built from the question.
2. `bm_context` when the answer spans linked notes.
3. `bm_read` the top hits before relying on them.
4. Answer with `[[note titles]]` as citations.

Use `bm_recent` for "what have I been working on?"

If the vault does not contain the answer, say so. Do not invent vault contents.

## Writeback

At the end of a work session, append the durable outcome to the relevant project hub:

- `[decision]` for a choice and its why
- `[fact]` for new state
- `[todo]` for the next action
- relations for anything new that connects

Keep the hub current; it is the first note a future session reads.
