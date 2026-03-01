# Context in Claude Code

## What is Context?

"Context" refers to everything Claude can see and reason about during a session. It's the cumulative information loaded into Claude's **context window** — the total capacity available for processing your conversation, code, and project information.

## What Goes Into the Context Window?

- Conversation history (all messages back and forth)
- File contents Claude reads during the session
- Shell command outputs
- `CLAUDE.md` and project rules
- Tool definitions from MCP servers
- System instructions and configuration

## When Context Fills Up

Claude Code manages context automatically:

1. **Clears older tool outputs first** — command results and file reads from early in the session are discarded first
2. **Summarizes conversation** — earlier exchanges are compressed while preserving key code and decisions
3. **CLAUDE.md always survives** — persistent rules in `CLAUDE.md` are reloaded every request, so they outlast conversation compaction

> This is why putting rules in `CLAUDE.md` is better than repeating instructions in chat — they survive context resets automatically.

## Tips for Managing Context

- **Keep `CLAUDE.md` lean** — under ~200 lines; include only what Claude would genuinely forget
- **Use `/clear` between unrelated tasks** — a fresh context outperforms a long, cluttered one
- **Use `/compact`** to manually summarize when approaching limits
- **Delegate to subagents** — they run in separate context windows and return only summaries, keeping your main session clean
- **Disconnect unused MCP servers** — their tool definitions consume context on every request
- **Be specific upfront** — precise prompts reduce back-and-forth corrections that bloat context

## Key Commands

| Command | What it does |
|---------|-------------|
| `/context` | Shows current context usage breakdown |
| `/clear` | Resets the context window entirely |
| `/compact` | Manually triggers context summarization |
