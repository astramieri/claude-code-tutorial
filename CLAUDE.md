# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a documentation-only learning repository following The Net Ninja's Claude Code series. It contains notes on Claude Code features — there is no build system, no source code, and no test suite.

## Docs

- `docs/init.md` — What `/init` does, when to re-run it, and caveats about overwriting manual edits
- `docs/memory.md` — The three CLAUDE.md memory scopes (project, local, global) and loading order; notes that `CLAUDE.local.md` is **deprecated** in newer Claude Code versions (use `~/.claude/CLAUDE.md` for personal preferences instead)
- `docs/context.md` — How the context window works, what survives compaction, key commands (`/clear`, `/compact`, `/context`, `/memory`, `/rewind`)

## Conventions

- All notes live in `docs/` as Markdown files.
- `CLAUDE.local.md` should not be used (deprecated); personal preferences go in `~/.claude/CLAUDE.md`.

## Tutorial Resources

- [Introduction & Setup](https://www.youtube.com/watch?v=SUysp3sJHbA)
- [CLAUDE.md Files & /init](https://www.youtube.com/watch?v=i_OHQH4-M2Y)
