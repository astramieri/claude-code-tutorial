# Planning and Thinking

References: [Best Practices](https://code.claude.com/docs/en/best-practices.md) · [Model Configuration](https://code.claude.com/docs/en/model-config.md)

## Plan Mode

Plan Mode lets Claude explore and reason without modifying files or running commands. Toggle it with `Ctrl+G` or set `defaultMode: "plan"` in `settings.json`.

Recommended four-phase workflow:

1. **Explore** *(Plan Mode)* — Claude reads files and understands the codebase
2. **Plan** *(Plan Mode)* — Claude produces a detailed implementation plan; press `Ctrl+G` to open and edit it before proceeding
3. **Implement** *(Normal Mode)* — Claude codes and verifies against the plan
4. **Commit** *(Normal Mode)* — Claude stages, writes a commit message, and opens a PR

> Skip planning for small, well-scoped changes. Plan mode is most valuable when the task touches multiple files or you're uncertain about the approach.

## Extended Thinking & Effort Levels

Effort levels control how much reasoning Claude applies before responding. Three levels are available: **low**, **medium**, **high**.

- Adjust during a session via `/model` (use arrow keys on the effort slider)
- Set permanently with `CLAUDE_CODE_EFFORT_LEVEL=low|medium|high`
- Or configure `effortLevel` in `settings.json`

Supported on Opus 4.6 and Sonnet 4.6. Opus 4.6 defaults to **medium** for Max and Team subscribers.

## `opusplan` Model Alias

A hybrid model mode that automatically switches models based on phase:

| Phase | Model used |
|-------|-----------|
| Plan Mode active | Opus (deep reasoning) |
| Execution (normal) mode | Sonnet (fast generation) |

Enable it with `/model opusplan` or set `"model": "opusplan"` in `settings.json`.

## Let Claude Interview You

For large, underspecified features, ask Claude to gather requirements before planning:

```
I want to build [brief description]. Interview me in detail using the AskUserQuestion tool.
Ask about technical implementation, UI/UX, edge cases, and tradeoffs.
When done, write a complete spec to SPEC.md.
```

Then start a **fresh session** to implement from the spec, keeping context clean.
