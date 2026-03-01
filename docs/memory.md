# Claude Code Memory Scopes

## 3 Main Memory Levels

1. `CLAUDE.md` (Project root)
    - Committed to git → shared with team
    - Use for: build commands, architecture, team standards

2. `CLAUDE.local.md` (Project root) ⚠️ **DEPRECATED**
    - Auto git-ignored → personal only
    - Use for: your preferences in THIS project
    - **Note**: No longer supported in newer versions of Claude Code. Use `~/.claude/CLAUDE.md` for personal preferences instead.

3. `~/.claude/CLAUDE.md` (Home directory)
    - Global → applies to ALL your projects
    - Use for: universal personal preferences

**Loading Order**:
Global → Project → Local → Subdirectories (hierarchical)

### Quick Rules

- Team conventions? → `CLAUDE.md` (commit it)
- Personal project preferences? → `CLAUDE.local.md` (git-ignored)
- Universal personal style? → `~/.claude/CLAUDE.md` (all projects)

**Bonus**: You can also create CLAUDE.md files in subdirectories for module-specific instructions. Claude recursively loads all relevant files when working in that area.