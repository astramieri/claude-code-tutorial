# `/init` command

### What it does

- Analyzes your codebase and auto-generates a `CLAUDE.md` file
- Documents build commands, test scripts, architecture, and coding conventions
- Becomes part of Claude's system prompt for all future sessions in that project

### Why run it first

- **Persistent context**: Claude remembers your project setup across all conversations
- **No repetition**: Stop re-explaining how your codebase works
- **Fewer mistakes**: Claude uses correct commands and follows your conventions
- **Team benefit**: Commit `CLAUDE.md` to git so everyone gets the same AI context

### How to use

```
cd your-project
claude
/init
```

**Pro tip**: The generated file is a starting point. Review and customize it with your actual workflows, domain-specific terms, and any nuances Claude missed. Think of it as living documentation that both humans and AI need to understand your project.