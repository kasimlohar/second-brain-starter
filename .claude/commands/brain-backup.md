---
description: Commit and push the vault to its private backup remote
argument-hint: [optional commit message]
---

Back up this vault.

1. Run `git status --short` and `git remote -v` from the vault root.
2. Confirm the remote is the private backup repository you intend to use. If it is missing, public, or unfamiliar, stop and report — do not push.
3. If the working tree is clean, say so and stop.
4. Otherwise stage all changes, show `git diff --cached --stat`, and commit as `capture: <summary>`. Use `$ARGUMENTS` as the message if one is given.
5. Push to `origin main`, then report the commit hash, files changed, and push result.
