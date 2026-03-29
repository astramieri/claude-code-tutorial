# MCP Servers

Reference: [MCP](https://code.claude.com/docs/en/mcp.md)

MCP (Model Context Protocol) is an open standard that lets Claude connect to external tools and data sources — databases, issue trackers, APIs, design tools, and more.

Use `/mcp` to view connection status and available tools in any session.

## Scopes

MCP servers can be registered at three levels:

| Scope | Storage | Shared with team? | Best for |
|-------|---------|:-----------------:|----------|
| `local` (default) | `~/.claude.json` | No | Personal servers, sensitive credentials |
| `project` | `.mcp.json` in project root | Yes (via git) | Team-wide tools for this repo |
| `user` | `~/.claude.json` | No | Personal utilities across all projects |

> Project-scoped servers require one-time approval before first use. Run `claude mcp reset-project-choices` to reset approvals.

## Adding Servers

```bash
# Remote server (HTTP — recommended)
claude mcp add --transport http github https://api.github.com/mcp

# Remote server with auth header
claude mcp add --transport http sentry https://mcp.sentry.io \
  --header "Authorization: Bearer YOUR_TOKEN"

# Local process (stdio)
claude mcp add --transport stdio postgres -- npx -y @modelcontextprotocol/server-postgres

# Scoped to the project (saved to .mcp.json)
claude mcp add --transport http notion https://mcp.notion.com/mcp --scope project
```

Options (`--transport`, `--scope`, `--header`, `--env`) must come **before** the server name.

## Transport Types

| Transport | Use when |
|-----------|----------|
| `http` | Remote/cloud service — stateless, recommended |
| `stdio` | Local process — runs on your machine, full system access |
| `sse` | Legacy remote streaming — deprecated, prefer `http` |

## Managing Servers

```bash
claude mcp list          # List all registered servers
claude mcp get github    # Inspect a server's config
claude mcp remove github # Remove a server
```

## Permissions

MCP tools follow the same allow/deny rules as built-in tools. Use `mcp__<servername>` as the specifier:

```json
{
  "permissions": {
    "allow": ["mcp__github", "mcp__postgres"],
    "deny":  ["mcp__*"]
  }
}
```

You can also manage these via `/permissions` in the UI.

## Tool Access in Sessions

MCP tools are loaded on demand (deferred) to keep context usage low. Reference MCP resources with `@` mentions (e.g. `@github:issue://123`) and use MCP prompts as slash commands: `/mcp__<server>__<prompt>`.
