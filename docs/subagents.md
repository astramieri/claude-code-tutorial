# Subagents

Reference: [Agent tool](https://code.claude.com/docs/en/agent-tool.md)

Subagents are specialized Claude instances that the main agent can spawn to handle tasks in parallel or in isolation. Each subagent runs with its own context window, returns a single result, and then terminates.

## Why Use Subagents

| Scenario | Benefit |
|----------|---------|
| Independent tasks (e.g. search + test run) | Execute in parallel, saving time |
| Large search results | Protect the main context from noise |
| Isolated file changes | Work in a separate git worktree |
| Specialized work | Use a purpose-built agent type |

## Built-in Agent Types

| Type | Best for |
|------|----------|
| `general-purpose` | Multi-step research, code search, complex tasks |
| `Explore` | Fast codebase exploration (read-only) |
| `Plan` | Designing implementation strategies |
| `claude-code-guide` | Questions about Claude Code features or the API |

## Key Behaviors

- **Context isolation** — the subagent does not share the parent's conversation history; give it all the context it needs in the prompt.
- **Single result** — the subagent returns one message; the parent decides what to show the user.
- **Background mode** — use `run_in_background: true` for fire-and-forget tasks; the parent is notified on completion.
- **Worktree isolation** — use `isolation: "worktree"` to give the subagent its own git branch; the worktree is cleaned up automatically if no changes are made.

## When NOT to Use Subagents

- Simple, directed searches — use `Glob` or `Grep` directly instead.
- Reading 2–3 specific files — use the `Read` tool.
- Tasks that depend on results the parent already has.

> Prefer parallel subagent calls when tasks are independent. Avoid duplicating work the subagent is already doing — if you delegate research, don't repeat the same searches yourself.
