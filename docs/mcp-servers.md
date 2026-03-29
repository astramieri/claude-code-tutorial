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

---

## Oracle Database & APEX: Spec-Driven Development

### The Problem MCP Solves

Without MCP, Claude is blind to your live database. You paste DDL manually, Claude guesses column names, and generated code drifts from reality.

With MCP, Claude reads your **live schema** — tables, columns, constraints, packages — and generates code that is correct by construction.

### Official Oracle MCP Server (SQLcl)

Oracle ships an official MCP server as part of **SQLcl** (free CLI). It gives Claude direct access to:

- Schema introspection (tables, views, sequences, indexes, packages)
- Query execution against live data
- Works with Oracle 19c → 23ai, on-prem and cloud (OCI, AWS, Azure, GCP)
- All interactions logged in `DBTOOLS$MCP_LOG`

```bash
# Register in Claude Code after installing SQLcl
claude mcp add --transport stdio oracle -- sql /nolog
```

### Spec-Driven Development Pattern

Define a spec (Markdown or JSON) as the single source of truth. Claude reads your live schema via MCP and generates everything consistently.

**Example spec:**

```markdown
# Feature: Customer Orders Module

## Tables
- ORDERS (order_id, customer_id FK, order_date, status, total_amount)
- ORDER_LINES (line_id, order_id FK, product_id FK, qty, unit_price)

## Business Rules
- Status flow: DRAFT → SUBMITTED → APPROVED → SHIPPED
- Total = sum of (qty * unit_price) across lines

## Deliverables
- DDL with constraints and sequences
- PKG_ORDERS package (create_order, submit_order, approve_order, get_order)
- APEX page: Order Entry form with tabular form for lines
```

Claude applies your naming conventions, error-handling patterns, and coding standards automatically — no boilerplate, no drift.

### Key References

- [Oracle SQLcl MCP Docs](https://docs.oracle.com/en/database/oracle/sql-developer-command-line/25.2/sqcug/using-oracle-sqlcl-mcp-server.html)
- [Oracle MCP GitHub](https://github.com/oracle/mcp)
- [MCP Server Registry](https://registry.modelcontextprotocol.io/)
