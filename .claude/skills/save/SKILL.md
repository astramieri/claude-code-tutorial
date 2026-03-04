---
name: save
description: Commit all pending changes and push to origin
allowed-tools: Bash(git *)
---

Commit all pending changes and push them to origin using these steps:

1. Run `git status` to see what has changed
2. Run `git add -A` to stage all changes
3. Run `git commit -m "<message>"` with a short, useful commit message that summarises the changes (no co-author line)
4. Run `git push origin HEAD` to push to the remote

Keep the commit message concise (under 72 characters). Use imperative mood (e.g. "Add slash commands doc", "Update memory scopes notes").
