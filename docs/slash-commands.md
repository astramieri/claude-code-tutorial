# Slash Commands

Reference: [Interactive mode](https://code.claude.com/docs/en/interactive-mode.md)

Type `/` in a session to see all available commands. Type `/` followed by letters to filter.

## Most Useful Commands

| Command | Description |
|---------|-------------|
| `/init` | Generate a `CLAUDE.md` for the current project |
| `/model [model]` | Switch model (`sonnet`, `opus`, `haiku`, `opusplan`) |
| `/plan` | Enter Plan Mode (analyze without modifying files) |
| `/clear` | Reset context window. Aliases: `/reset`, `/new` |
| `/compact [instructions]` | Summarize conversation to free up context |
| `/rewind` | Restore conversation and/or code to a previous checkpoint. Alias: `/checkpoint` |
| `/permissions` | View and edit tool permission rules. Alias: `/allowed-tools` |
| `/memory` | Edit `CLAUDE.md` files and manage auto-memory |
| `/hooks` | Manage pre/post tool-use hook configurations |
| `/mcp` | Manage MCP server connections |
| `/context` | Visualize current context usage as a colored grid |
| `/cost` | Show token usage statistics for the session |
| `/status` | Show version, model, account, and connectivity info |
| `/help` | List all available commands |
| `/exit` | Exit Claude Code. Alias: `/quit` |

## Other Handy Commands

| Command | Description |
|---------|-------------|
| `/diff` | Interactive viewer for uncommitted changes and per-turn diffs |
| `/review` | Review a pull request (requires `gh` CLI) |
| `/pr-comments [PR]` | Fetch comments from a GitHub PR |
| `/rename [name]` | Rename the current session |
| `/resume [session]` | Resume a past session by name or ID |
| `/branch [name]` | Create a branch of the current conversation. Alias: `/fork` |
| `/skills` | List available custom skills (see [Custom Skills](#custom-skills) below) |
| `/vim` | Toggle Vim editing mode |
| `/theme` | Change the color theme |
| `/sandbox` | Toggle sandbox mode (OS-level isolation) |
| `/fast [on\|off]` | Toggle fast mode |
| `/btw <question>` | Ask a quick side question without adding it to conversation history |
| `/security-review` | Analyze pending changes for security vulnerabilities |
| `/doctor` | Diagnose installation and settings issues |
| `/insights` | Generate a report on Claude Code sessions |
| `/stats` | Visualize usage and session history |
| `/schedule` | Create and manage cloud scheduled tasks |
| `/release-notes` | View the changelog |
| `/remote-control` | Enable remote control from claude.ai. Alias: `/rc` |
| `/reload-plugins` | Reload plugins without restarting |

## Custom Skills

Reference: [Skills](https://code.claude.com/docs/en/skills.md)

Custom commands have been **merged into skills**. Both `.claude/commands/review.md` and `.claude/skills/review/SKILL.md` create a `/review` command and work the same way. Existing `.claude/commands/` files keep working — no migration needed.

Skills are the recommended approach going forward, as they add more capabilities:

| | Old (`.claude/commands/`) | New (`.claude/skills/`) |
|---|---|---|
| Format | Single `.md` file | Directory with `SKILL.md` |
| Supporting files | No | Yes |
| Claude auto-invokes | No | Yes (via `description` field) |
| Invocation control | No | Yes (`disable-model-invocation`) |
| Subagent execution | No | Yes (`context: fork`) |
| Tool restrictions | No | Yes (`allowed-tools`) |
| Path-specific activation | No | Yes (`paths`) |

### Minimal skill example

```
.claude/skills/fix-issue/SKILL.md
```

```yaml
---
name: fix-issue
description: Fix a GitHub issue by number
disable-model-invocation: true
---

Fix GitHub issue $ARGUMENTS following our coding standards.
1. Read the issue, implement the fix, write tests, create a commit.
```

Invoke with `/fix-issue 123`. The `$ARGUMENTS` placeholder is replaced with `123`. You can also use `$ARGUMENTS[N]` or `$N` to reference individual arguments, and `${CLAUDE_SKILL_DIR}` to reference the skill's directory.

### Skill locations

| Location | Path | Scope |
|----------|------|-------|
| Personal | `~/.claude/skills/<name>/SKILL.md` | All your projects |
| Project | `.claude/skills/<name>/SKILL.md` | This project only |
