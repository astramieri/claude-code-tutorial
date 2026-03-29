# Effective Workflows: Commands, Skills & Subagents

This guide answers the practical question: **given a task, which tool should I reach for?**

---

## Quick Decision Matrix

| Situation | Use | Why |
|-----------|-----|-----|
| Look up how to do something in Claude Code | `/help` or built-in slash command | Instant, no overhead |
| Repeatable workflow you trigger manually | Custom skill (`disable-model-invocation: true`) | You control when it runs |
| Background knowledge for Claude to auto-apply | Custom skill (`user-invocable: false`) | Claude invokes it, not you |
| Research/explore without touching files | Plan Mode or Explore subagent | Safe — no edits can happen |
| Parallel independent tasks | Subagents (run concurrently) | Each gets its own context window |
| Large search / noisy command output | Subagent | Keeps your main context clean |
| Isolated file changes on a branch | Subagent with `isolation: "worktree"` | Own git worktree, auto-cleaned |
| Complex multi-step task | Subagent or skill with `context: fork` | Isolated context, full autonomy |
| Persistent team conventions | `CLAUDE.md` (committed) | Survives compaction, shared with team |
| Personal cross-project preferences | `~/.claude/CLAUDE.md` | Global, never in git |

---

## When to Use Slash Commands

Slash commands are **built-in** — you cannot change their logic, only invoke them.

**Use slash commands when you want to:**

- Manage the session (`/clear`, `/compact`, `/rewind`)
- Inspect state (`/context`, `/cost`, `/status`, `/permissions`)
- Switch modes on the fly (`/plan`, `/model`, `/effort`)
- Trigger a one-off action (`/init`, `/diff`, `/review`, `/doctor`)

**Don't create a custom skill to replicate built-in commands.** If the feature exists as a slash command, use it directly.

### Most Actionable Commands

| Command | When to use |
|---------|-------------|
| `/plan` | Before any task touching multiple files — explore safely first |
| `/effort high` | Complex debugging or architectural decisions |
| `/compact` | When context is filling up but you want to continue the session |
| `/clear` | When switching to an unrelated task — fresh context is better |
| `/rewind` | When Claude took the wrong turn — undo without starting over |
| `/model opusplan` | Long sessions: Opus for planning, Sonnet for execution |

---

## When to Use Custom Skills

Custom skills are **reusable prompts** stored in `.claude/skills/<name>/SKILL.md`. Use them for workflows that recur across sessions.

### Invocation modes

| Frontmatter | Who invokes it | Best for |
|-------------|---------------|----------|
| *(default)* | You or Claude | General-purpose automation |
| `disable-model-invocation: true` | Only you (`/skill-name`) | Deployments, commits — things with side effects you must control |
| `user-invocable: false` | Only Claude (auto) | Background knowledge, style guides Claude should just "know" |

### Run a skill in its own context

Add `context: fork` to execute the skill in an isolated subagent instead of polluting the main conversation:

```yaml
---
name: review
description: Review changed files for issues
context: fork
---
Review all staged changes for bugs, security issues, and style violations.
```

### Practical examples

- **`/save`** — stages, commits, and pushes (disable-model-invocation so you always pull the trigger)
- **`/fix-issue $1`** — reads a GitHub issue number and implements the fix
- **Style guide skill** — loaded automatically by Claude every session (user-invocable: false)
- **`/deploy staging`** — runs your deploy script with pre-flight checks

### Skill vs CLAUDE.md

Use a **skill** for a workflow (sequence of steps). Use **CLAUDE.md** for persistent rules or facts (conventions, constraints) that Claude should always know.

---

## When to Use Subagents

Subagents are isolated Claude instances with their own context window. The parent receives a single summary; the noise stays inside the subagent.

### Primary use cases

**1. Parallel independent work**
Spawn multiple subagents in the same message — they run concurrently:

```
Search the API docs AND run the test suite at the same time.
→ Two subagents, one message, both return results together.
```

**2. Protecting the main context**
Large grep results, verbose test logs, or big file reads belong inside a subagent. Only the useful summary comes back.

**3. Read-only exploration**
Use the `Explore` agent type when you only need to understand code, not change it. It's fast and enforces read-only access.

**4. Isolated changes on a branch**
Use `isolation: "worktree"` — the subagent gets its own git branch. If it makes no changes, the worktree is cleaned up automatically.

**5. Specialized model or tool set**
Subagents can use a different model, a subset of tools, or specific MCP servers — without affecting your main session.

### Built-in agent types

| Type | Use when |
|------|----------|
| `Explore` | Fast, read-only codebase exploration |
| `Plan` | Designing implementation strategy before coding |
| `general-purpose` | Multi-step research, complex tasks, writing code |
| `claude-code-guide` | Questions about Claude Code or the Anthropic API |

### When NOT to use a subagent

- You're searching for a specific file — use `Glob` or `Grep` directly.
- You're reading 2–3 known files — use the `Read` tool.
- The task depends on context already in the main conversation — pass it explicitly or stay in the main session.
- The overhead of spinning up a subagent exceeds the benefit for trivial tasks.

---

## Combining All Three

These tools are not mutually exclusive. A common pattern:

```
1. /plan              → explore the codebase in Plan Mode (read-only)
2. Approve the plan   → switch to normal mode
3. Subagents (parallel) → run tests + search docs concurrently
4. Main session       → implement the change
5. /save (skill)      → commit and push
```

For large, underspecified features, consider the **Interview → Spec → Fresh session** pattern:

```
Ask Claude to interview you with AskUserQuestion, then write a SPEC.md.
Start a new session (/clear) and implement from the spec.
```

This keeps the context clean and the plan explicit.

---

## Effort Level Guidance

| Level | When to use |
|-------|-------------|
| `low` | Quick lookups, formatting, simple generation |
| `medium` *(default)* | Most coding tasks |
| `high` | Complex debugging, architectural decisions, security analysis |
| `max` | Deepest reasoning — Opus 4.6 only, slower, one-off use |

Set for the session: `/effort high`
Set permanently: `CLAUDE_CODE_EFFORT_LEVEL=high` or `effortLevel` in `settings.json`
One-off deep thinking: include "ultrathink" anywhere in your prompt.
