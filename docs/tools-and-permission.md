# Tools & Permissions

Reference: [Configure permissions](https://code.claude.com/docs/en/permissions.md)

## Built-in Tools

| Tool | Approval required |
|------|------------------|
| `Read`, `Grep`, `Glob` (read-only) | No |
| `Edit`, `Write` (file modification) | Yes — until session end |
| `Bash` (shell execution) | Yes — permanently per project + command |
| `WebFetch`, `WebSearch` | Yes |
| MCP tools | Yes |

## Permission Modes

Set via `defaultMode` in `settings.json`:

| Mode | Behavior |
|------|----------|
| `default` | Prompts on first use of each tool |
| `acceptEdits` | Auto-accepts file edit permissions for the session |
| `plan` | Claude can analyze but not modify files or run commands |
| `dontAsk` | Auto-denies tools unless pre-approved |
| `bypassPermissions` | Skips all prompts — only use in isolated environments |

## Permission Rules

Rules follow the format `Tool` or `Tool(specifier)`. Evaluated in order: **deny → ask → allow** (first match wins).

```json
{
  "permissions": {
    "allow": ["Bash(npm run *)", "Bash(git commit *)"],
    "deny":  ["Bash(git push *)"]
  }
}
```

Common specifiers:
- `Bash(npm run build)` — exact command
- `Bash(npm *)` — any npm command
- `Read(./.env)` — specific file
- `Edit(/src/**/*.ts)` — path relative to project root
- `WebFetch(domain:example.com)` — domain restriction
- `mcp__puppeteer` — all tools from an MCP server

## Managing Permissions

Use `/permissions` in the Claude Code UI to view and edit active rules and their source `settings.json`.
