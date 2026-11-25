---
description: Capture a thought as a linked, well-formed vault note
argument-hint: <what to remember>
---

Capture this into the second brain: $ARGUMENTS

Read `CLAUDE.md` and `90-Meta/schema.md` first if they are not already in context. Then:

1. Search for 2–3 existing notes this connects to (`bm_search`); reuse their titles exactly in relations.
2. Propose: a claim-shaped title, `type`, tags, 3–6 observations categorized `[fact]` `[decision]` `[preference]` `[todo]` `[resource]` `[question]`, and at least two typed relations (`relates_to`, `requires`, `supersedes`, `owned_by`, `affects`).
3. Write it to `00-Inbox/Agent/` with `bm_write`. Frontmatter: `title`, `type`, `tags`.
4. If a note with that title already exists, do not overwrite — propose a `bm_edit` diff and wait for approval.
5. Reply with the permalink and the links you added.
