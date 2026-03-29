# Claude Code Memory Scopes

## Memory Levels (Highest → Lowest Priority)

1. **Managed policy** — organization-wide, set by IT/DevOps, cannot be excluded
   - Windows: `C:\Program Files\ClaudeCode\CLAUDE.md`
   - macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`
   - Linux/WSL: `/etc/claude-code/CLAUDE.md`

2. **Project** — `./CLAUDE.md` or `./.claude/CLAUDE.md`
   - Committed to git → shared with team
   - Use for: build commands, architecture, team standards

3. **User (global)** — `~/.claude/CLAUDE.md`
   - Applies to ALL your projects
   - Use for: universal personal preferences

4. ~~`CLAUDE.local.md`~~ ⚠️ **DEPRECATED** — use `~/.claude/CLAUDE.md` instead

**Loading Order**: Managed → User (global) → Project → Subdirectories (hierarchical)

### Quick Rules

- Team conventions? → `./CLAUDE.md` (commit it)
- Personal project preferences? → `~/.claude/CLAUDE.md` (global)
- Universal personal style? → `~/.claude/CLAUDE.md` (same file, applies everywhere)

### Path-Specific Rules (Advanced)

For larger projects, organize rules into separate files in `.claude/rules/` with YAML frontmatter so they only load when Claude is editing matching files:

```markdown
---
paths:
  - "src/api/**/*.ts"
---
# API Development Rules
- Use OpenAPI documentation
- Include input validation
```

This reduces context overhead — rules load on demand, not at every request.

**Bonus**: You can also create CLAUDE.md files in subdirectories for module-specific instructions. Claude recursively loads all relevant files when working in that area.