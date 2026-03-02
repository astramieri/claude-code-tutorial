# Context in Claude Code

## What is Context?

"Context" refers to everything Claude can see and reason about during a session. It's the cumulative information loaded into Claude's **context window** — the total capacity available for processing your conversation, code, and project information.

## What Goes Into the Context Window?

- Conversation history (all messages back and forth)
- File contents Claude reads during the session
- Shell command outputs
- `CLAUDE.md` files at all levels (org, user, project, local)
- Auto memory (`MEMORY.md` — first 200 lines loaded at session start)
- Tool definitions from MCP servers
- System instructions and configuration

## When Context Fills Up

Claude Code manages context automatically:

1. **Clears older tool outputs first** — command results and file reads from early in the session are discarded first
2. **Summarizes conversation** — earlier exchanges are compressed while preserving key code and decisions
3. **CLAUDE.md always survives** — all `CLAUDE.md` files are reloaded every request, so they outlast conversation compaction
4. **Auto memory survives** — `MEMORY.md` is reloaded from disk after compaction as well

> This is why putting rules in `CLAUDE.md` is better than repeating instructions in chat — they survive context resets automatically.

## CLAUDE.md Levels

Multiple `CLAUDE.md` files can coexist and all load at session start:

| Scope | Location |
|-------|----------|
| Organization | `/etc/claude-code/CLAUDE.md` (Linux) |
| User (global) | `~/.claude/CLAUDE.md` |
| Project | `./CLAUDE.md` or `./.claude/CLAUDE.md` |
| Local (per-machine) | `./CLAUDE.local.md` |

## A Note on Context Quality

> Too much unnecessary context can lead to **less accurate results**. Irrelevant information dilutes the signal, diffuses attention, and can push out genuinely useful content. Focused, relevant context consistently outperforms large, cluttered context.

## Tips for Managing Context

- **Keep `CLAUDE.md` lean** — include only what Claude would genuinely forget
- **Keep `MEMORY.md` concise** — lines after 200 are truncated at session load
- **Use `/clear` between unrelated tasks** — a fresh context outperforms a long, cluttered one
- **Use `/compact [instructions]`** to manually summarize; pass focus instructions to control what's preserved
- **Delegate to subagents** — they run in separate context windows and return only summaries, keeping your main session clean
- **Disconnect unused MCP servers** — their tool definitions consume context on every request
- **Be specific upfront** — precise prompts reduce back-and-forth corrections that bloat context

## Key Commands

| Command | What it does |
|---------|-------------|
| `/context` | Shows current context usage as a colored grid |
| `/memory` | Lists all loaded CLAUDE.md/rules files; toggle auto memory on/off |
| `/compact [instructions]` | Summarizes conversation, optionally with focus instructions |
| `/clear` | Resets the context window entirely (aliases: `/reset`, `/new`) |
| `/exit` | Exits Claude Code (aliases: `/quit`, `Ctrl+D`) |
| `Esc Esc` | Opens the rewind menu (same as `/rewind` or `/checkpoint`) |

### Rewind Example

Imagine you ask Claude to refactor a function and the result is worse than the original:

```
You:    "Refactor the login function to be cleaner"
Claude: [rewrites it — but you don't like the result]

You:    [press Esc Esc  —or—  type /rewind]
        → A scrollable list appears showing all your previous prompts
        → You select the point before the refactor
        → You choose what to restore:
            1. Restore code + conversation  ← full undo
            2. Restore conversation only    ← keep current code
            3. Restore code only            ← keep current conversation
            4. Summarize from here          ← compress context from this point
            5. Never mind                   ← cancel
```

> **Note:** Checkpoints track **file edits only** — bash command side-effects (e.g. files created by a shell command) are not reverted. Checkpoints persist across sessions and are auto-cleaned after 30 days.
>
> **VSCode extension:** `/rewind` and `Esc Esc` are terminal CLI features. In the IDE, hover over any message in the Claude Code panel — a rewind button appears with three options:
> - **Fork conversation from here** — new conversation branch, code unchanged
> - **Rewind code to here** — revert file changes, conversation kept
> - **Fork conversation and rewind code** — both

This is useful when Claude takes a wrong turn and you want to go back and try a different approach, without starting the whole session over with `/clear`.
