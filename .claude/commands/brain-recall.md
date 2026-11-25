---
description: Answer a question from the vault, with citations
argument-hint: <question>
---

Answer this from the second brain: $ARGUMENTS

Follow the retrieval rules in `CLAUDE.md`:

1. Build a hybrid query from the question and call `bm_search`. Use semantic mode if the question is paraphrased, text mode if it is a name or code.
2. Read the top 2–3 hits with `bm_read` before relying on them. Use `bm_context` when the answer spans linked notes.
3. If notes disagree, prefer the newer `[decision]` and say which one supersedes which.
4. Answer concisely, citing every factual claim as `[[Note Title]]`.
5. If the vault does not contain the answer, say so plainly and suggest what to capture. Never invent vault contents.
