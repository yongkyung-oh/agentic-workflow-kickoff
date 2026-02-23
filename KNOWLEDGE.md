# Domain Knowledge

Domain-specific expertise separated from general project rules. This file is portable — any AI coding agent can reference this knowledge.

Knowledge too specialized for AGENTS.md or CLAUDE.md, but essential when working on related tasks.

## Knowledge vs AGENTS.md vs CLAUDE.md

| | AGENTS.md | CLAUDE.md | Knowledge |
|------|-----------|-----------|-----------|
| Audience | Any file-aware agent | Claude Code only | Any agent (on-demand) |
| Content | Project-wide context | Execution rules | Domain expertise |
| Loaded | Always | Always (Claude) | On-demand |
| Length | Concise (~150 lines) | Concise (~50 lines) | Per domain, flexible |

## When to Add Domain Knowledge

- A domain pattern repeats 3+ times
- New team members (or agents) keep making the same mistake
- Knowledge is too long or specialized for AGENTS.md

## Writing Guide

For standalone, auto-discoverable skills, use the official skills system:

```
.claude/skills/
├── testing/SKILL.md        # Testing patterns (auto-discovered)
├── api-design/SKILL.md     # API design rules (auto-discovered)
└── database/SKILL.md       # DB schema/query patterns (auto-discovered)
```

See `.claude/skills/_template/SKILL.md` for the official format with frontmatter.

For simpler domain notes, write inline at the bottom of this file.

### Knowledge Template

```markdown
## [Topic Name]

### When to Apply
- [Situations where this knowledge is needed]

### Key Concepts
- [Core concepts, terms, constraints]
- [Reference files: @path/to/reference]

### Patterns

[Patterns to follow in this domain]

### Constraints
- [What to never do]
- [What to always check]

### Examples
Good: [Correct example]
Bad: [Wrong example and why]
```

---

## Knowledge Index

<!-- Update this table when adding domain knowledge -->

| Topic | Location | Description |
|-------|----------|-------------|
| — | Add a row when creating a new entry | — |
