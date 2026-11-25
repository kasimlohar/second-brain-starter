---
description: Weekly review — orphans, duplicates, stale projects
argument-hint: [project name, default brain]
---

Run a second-brain review for project `$ARGUMENTS` (default: `brain`). Stay read-only until the user approves a plan.

1. Inventory notes with `bm_list` and `bm_find`.
2. Orphans: notes with no inbound or outbound relations. Propose two specific links for each.
3. Duplicates: near-identical notes. Propose a canonical note and a merge; archive the duplicate, never delete it.
4. Staleness: project hubs with no activity in 30 days. Propose an updated `## Status` line or a move to `40-Archive/`.
5. Hub hygiene: list `[todo]` items that are done and `[decision]` items that have been superseded.
6. Present the plan as a checklist with exact `bm_edit` and `bm_move` operations. Apply only what the user approves. Never call `bm_delete`.
