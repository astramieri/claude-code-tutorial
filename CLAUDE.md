# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a documentation-only learning repository following The Net Ninja's Claude Code series. It contains notes on Claude Code features — there is no build system, no source code, and no test suite.

## Docs

- `docs/init.md` — What `/init` does, when to re-run it, and caveats about overwriting manual edits
- `docs/memory.md` — The four CLAUDE.md scopes (org, global, project, local), loading order, and the deprecation of `CLAUDE.local.md` in favour of `~/.claude/CLAUDE.md`
- `docs/context.md` — What fills the context window, what survives compaction (CLAUDE.md and MEMORY.md are always reloaded), key commands (`/clear`, `/compact`, `/context`, `/memory`, `/rewind`), and the VSCode vs CLI differences for rewind/checkpoint
- `docs/tools-and-permission.md` — Built-in tools, permission modes (`default`, `acceptEdits`, `plan`, `dontAsk`, `bypassPermissions`), permission rule syntax, and `/permissions` command
- `docs/planning-and-thinking.md` — Plan Mode, effort levels, `opusplan` hybrid model alias, and the four-phase explore→plan→implement→commit workflow
- `docs/slash-commands.md` — All slash commands, custom skills vs legacy commands, skill directory structure, and the `SKILL.md` format
- `docs/mcp-servers.md` — What MCP servers are, scope levels (local/project/user), adding servers via CLI, transport types, and permission rules
- `docs/subagents.md` — Subagents overview (in progress)

## Conventions

- All notes live in `docs/` as Markdown files.
- `CLAUDE.local.md` should not be used (deprecated); personal preferences go in `~/.claude/CLAUDE.md`.
- Custom skills live in `.claude/skills/<name>/SKILL.md`.

## Custom Skills

- `/save` (`.claude/skills/save/`) — Stages all changes, commits with a short message, and pushes to origin.

## Tutorial Resources

- [Introduction & Setup](https://www.youtube.com/watch?v=SUysp3sJHbA)
- [CLAUDE.md Files & /init](https://www.youtube.com/watch?v=i_OHQH4-M2Y)
