# agent-bootstrap — Bootstrap Guide

> Last verified: 2026-02 | Optimized for Claude Code, portable to Codex, Gemini CLI, Antigravity, and other file-aware agents

## Getting Started

### 1. Copy files to your project

```bash
cd your-project
cp /path/to/agent-bootstrap/{AGENTS.md,CLAUDE.md,KNOWLEDGE.md,_BOOTSTRAP_GUIDE.md} .
cp -r /path/to/agent-bootstrap/.claude .claude
```

### 2. Bootstrap your project

**Claude Code (recommended):**

```
> /init
```

`/init` runs the built-in skill that bootstraps automatically:
1. Analyze project → fill AGENTS.md placeholders
2. Ensure `.claude/` directories exist (specs, sessions, tasks, skills)
3. Verify or create template files (specs, sessions, tasks/lessons.md)
4. Move `KNOWLEDGE.md` → `.claude/KNOWLEDGE.md`
5. Update `.gitignore`

If `/init` doesn't work (skills not enabled, different CLI version), use the manual prompt below.

**Other agents (or manual fallback):**

Paste this prompt into your agent:

```
Read @_BOOTSTRAP_GUIDE.md and @AGENTS.md.
Analyze this project and fill in all placeholders in AGENTS.md:
- Project name, description, tech stack
- Build/test/lint commands
- Directory structure
- Code conventions
Then set up .claude/ directories (specs, sessions, tasks) if missing.
Move KNOWLEDGE.md to .claude/KNOWLEDGE.md.
Move _BOOTSTRAP_GUIDE.md to .claude/_BOOTSTRAP_GUIDE.md.
Print a summary of what was configured.
```

This works in any agent that reads markdown files — Codex, Gemini CLI, Antigravity, etc.

---

## File Structure

```
agent-bootstrap/
├── AGENTS.md                     ← Universal agent instructions (all AI tools)
├── CLAUDE.md                     ← Claude Code execution rules
├── KNOWLEDGE.md                  ← Domain knowledge guide
├── _BOOTSTRAP_GUIDE.md           ← This guide
└── .claude/
    ├── skills/
    │   ├── init/SKILL.md         ← /init skill (bootstrap setup)
    │   └── _template/SKILL.md    ← skill template (frontmatter)
    ├── agents/_template.md       ← custom agent template (frontmatter)
    ├── tasks/lessons.md          ← Correction tracking with promotion path
    ├── specs/
    │   ├── README.md             ← spec index + status
    │   └── _template.md          ← spec template
    └── sessions/
        ├── README.md             ← latest handoff
        └── _template.md          ← session template
```

After bootstrap:

```
your-project/
├── AGENTS.md                     ← stays at root (all agents discover)
├── CLAUDE.md                     ← stays at root (Claude auto-loads)
└── .claude/
    ├── _BOOTSTRAP_GUIDE.md       ← moved after bootstrap (reference)
    ├── KNOWLEDGE.md              ← moved after bootstrap
    ├── specs/
    │   ├── README.md             ← spec index + status
    │   └── _template.md          ← spec template
    ├── sessions/
    │   ├── README.md             ← latest handoff
    │   └── _template.md          ← session template
    ├── tasks/
    │   └── lessons.md            ← Correction tracking with promotion path
    ├── skills/                   ← Skills and slash commands (.claude/skills/<name>/SKILL.md)
    └── agents/                   ← Custom subagents (optional)
```

---

## Do You Need This Bootstrap?

Not every project needs the full set. **Incremental adoption** is recommended.

### When It Helps

| Scenario | Why |
|----------|-----|
| **Multi-session projects (3+)** | Context loss between sessions is the biggest cost. Specs/Sessions solve this |
| **Multi-agent environments** | Claude + Codex together — AGENTS.md covers both with one file |
| **Recurring domain mistakes** | KNOWLEDGE.md and skills record patterns to prevent recurrence |

### When It's Overkill

| Scenario | Alternative |
|----------|-------------|
| **1-2 session projects** | Just write a minimal AGENTS.md |
| **Exploration / prototyping** | Adopt after architecture settles |
| **Solo + Claude only** | Single CLAUDE.md with only what you need |

### Incremental Adoption Strategy

```
Step 1: Write AGENTS.md only, start working
           ↓ when sessions exceed 3
Step 2: Add specs/sessions (session continuity)
           ↓ when the same mistakes repeat
Step 3: Add KNOWLEDGE.md + skills/ (domain knowledge)
```

### Warning: More Files = More Cost

- AGENTS.md + CLAUDE.md + KNOWLEDGE.md all consume context tokens per session
- Rules must earn their keep — only add what actually prevents mistakes
- Files must be maintained as the project evolves — outdated rules cause harm
- **Unused rules should be deleted**

---

## Quick Start

```bash
# 1. Copy bootstrap files
cd your-project
cp /path/to/agent-bootstrap/{AGENTS.md,CLAUDE.md,KNOWLEDGE.md,_BOOTSTRAP_GUIDE.md} .
cp -r /path/to/agent-bootstrap/.claude .claude

# 2. Bootstrap
# Claude Code: > /init
# Other agents: paste the bootstrap prompt from "Getting Started" above

# 3. (Optional) Review and refine AGENTS.md
vim AGENTS.md
```

---

## Specs & Sessions Templates

Template files are included in `.claude/specs/` and `.claude/sessions/`. During bootstrap (or `/init`), these are set up in the project as-is.

- `.claude/specs/_template.md` — Spec template (goal, requirements, scope, technical approach)
- `.claude/specs/README.md` — Spec index table with status tracking
- `.claude/sessions/_template.md` — Session template (goal, what was done, handoff)
- `.claude/sessions/README.md` — Latest Handoff block + session history table

Each README includes scaling guidelines (subfolders, archiving) for when files accumulate.

---

## The Three Documents

### Why Three?

Putting everything in one file causes two problems:

1. **Agent compatibility** — Codex, Gemini CLI, Antigravity don't read CLAUDE.md, but AGENTS.md is the industry standard they all read
2. **Context waste** — Loading domain knowledge every session wastes tokens

```
CLAUDE.md    → Claude behavior layer (execution rules)
AGENTS.md    → Project-level policies (agent-agnostic)
KNOWLEDGE.md → Domain context (loaded on-demand)
```

```
               ┌─────────────────────────────────────────────┐
               │           Total Project Knowledge            │
               └─────────────────────────────────────────────┘
                         │              │            │
              ┌──────────┴───┐   ┌──────┴─────┐  ┌──┴──────────┐
              │  AGENTS.md   │   │  CLAUDE.md  │  │ KNOWLEDGE.md│
              │  (Policies)  │   │  (Execution)│  │  (Domain)   │
              └──────────────┘   └────────────┘  └─────────────┘
               Any agent          Claude Code      On-demand
               Always loaded      Always loaded    When needed
```

### 1. AGENTS.md — "What are this project's rules?" (Portable)

**Audience:** Any file-aware AI agent (Claude, Codex, Gemini CLI, Antigravity, etc.)
**Loaded:** Auto-discovered by Codex, Gemini CLI, Antigravity; Claude Code loads via `@AGENTS.md` reference in CLAUDE.md
**Location:** Project root
**Length:** ~150 lines recommended

| Include | Exclude |
|---------|---------|
| Project name, tech stack | Agent-specific behavior (→ CLAUDE.md) |
| Build/test commands | Domain expertise (→ KNOWLEDGE.md) |
| Directory structure | Code style details (→ linter config) |
| Git conventions | |
| Technical constraints / security | |

**Key principle:** Project-level policies only. No execution behavior. Claude, Codex, and Gemini CLI should understand it identically.

### 2. CLAUDE.md — "How does Claude execute here?" (Claude-specific)

**Audience:** Claude Code only
**Loaded:** Auto-loaded at Claude Code session start
**Location:** Project root
**Length:** ~80 lines recommended

| Include | Exclude |
|---------|---------|
| @AGENTS.md reference | Build/test commands (→ AGENTS.md) |
| Workflow orchestration | Directory structure (→ AGENTS.md) |
| Specs & Sessions system | Git conventions (→ AGENTS.md) |
| Model routing | Domain expertise (→ KNOWLEDGE.md) |
| Task management | Code style (→ linter config) |
| Hooks setup pointer | |

**Key principle:** Don't duplicate AGENTS.md. Only Claude Code-specific features (specs, sessions, model routing, hooks, subagents).

### 3. KNOWLEDGE.md — "How do we do things in this domain?" (On-demand)

**Audience:** Any agent (on-demand)
**Loaded:** On-demand when working on related tasks
**Location:** `.claude/KNOWLEDGE.md` (moved after bootstrap)
**Length:** Flexible per domain

| Include | Exclude |
|---------|---------|
| Domain-specific patterns, rules | Project-wide rules (→ AGENTS.md) |
| Key concepts, terminology | Workflow (→ CLAUDE.md) |
| Good/Bad examples | Build commands (→ AGENTS.md) |
| Domain constraints | |

**Key principle:** Not loaded every session — separating domain knowledge saves context.

> **Note:** For auto-discoverable, invocable skills, use the official `.claude/skills/<name>/SKILL.md` structure instead. KNOWLEDGE.md is for static domain reference; skills are for interactive, tool-equipped capabilities. See `.claude/skills/_template/SKILL.md`.

### Comparison

| | AGENTS.md | CLAUDE.md | KNOWLEDGE.md |
|---|-----------|-----------|-----------|
| **One-liner** | Project policies | Claude execution rules | Domain knowledge |
| **Audience** | Any file-aware agent | Claude Code only | Any agent (on-demand) |
| **Location** | Project root | Project root | .claude/ |
| **Loaded** | Always | Always (Claude) | On-demand |
| **Length guide** | ~150 lines | ~80 lines | Flexible |
| **Update frequency** | Architecture changes | Workflow changes | Domain knowledge additions |

---

## Specs & Sessions Workflow

### Why It's Needed

Claude Code's key limitation: all context is lost when a session ends.
Specs externalize **intent** (what & why), Sessions externalize **progress** (when & how),
ensuring continuity across sessions.

### Full Flow

```
New task starts
    │
    ▼
Write spec (draft)     ← .claude/specs/NNN-name.md
    │                     Update specs/README.md
    ▼
Approved, start work
    │
    ▼
Session 1 starts       ← Check sessions/README.md Latest Handoff
    │                     spec status → in-progress
    ▼
Session 1 ends         ← Write .claude/sessions/YYYY-MM-DD-topic.md
    │                     Update sessions/README.md Latest Handoff
    ▼
Session 2 starts       ← Read Latest Handoff, continue work
    │
    ▼
  ...repeat...
    │
    ▼
Task complete          ← spec status → done
                         Update specs/README.md
```

---

## Design Principles

### Layer Separation

```
High certainty ─────────────────────────── Low certainty

 Hooks (settings.json)    AGENTS.md       CLAUDE.md       Knowledge
 (set up directly)        (policies)      (execution)     (domain knowledge)
 ┌───────────────┐  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
 │ lint/format   │  │ Commands     │  │ Planning     │  │ Test patterns│
 │ Path blocking │  │ Architecture │  │ Specs/Sess   │  │ API design   │
 │ Auto-run      │  │ Conventions  │  │ Model Route  │  │ DB patterns  │
 └───────────────┘  └──────────────┘  └──────────────┘  └──────────────┘
  100% enforced       Any agent         Claude only       On-demand
```

### Model Routing: Reasoning Model for Thinking, Fast Model for Doing

```
 Reasoning-heavy model              Fast model
 ┌─────────────────────┐           ┌─────────────────────┐
 │ Spec writing/review  │    ──→   │ Code implementation  │
 │ Architecture design  │    ──→   │ Test writing         │
 │ Bug root cause       │    ──→   │ Bug fixing           │
 │ Code review          │           │ Refactoring          │
 └─────────────────────┘           └─────────────────────┘
       "what & why"                      "how"
```

### "Never send an LLM to do a linter's job"

- ❌ AGENTS.md: "Use semicolons, indent 2 spaces"
- ✅ settings.json hook: eslint --fix + prettier --write auto-runs

### Pointers > Copy

- ❌ Pasting code snippets in AGENTS.md
- ✅ Use `@path/to/file` references

---

## Advanced: Hooks (Optional)

> **Note:** Hooks are optional and agent-version dependent. They require `jq` installed on your system.
> Your project works fully without hooks — they add automated linting/formatting on tool events.

Set up hooks in `.claude/settings.json` for your project type. These run deterministically — no LLM judgment involved.

```jsonc
// JS/TS example
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path' | xargs npx eslint --fix && jq -r '.tool_input.file_path' | xargs npx prettier --write"
      }]
    }],
    "PreToolUse": [{
      "matcher": "Write",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path' | { read fp; if echo \"$fp\" | grep -qE '(migrations/|dist/|build/|\\.env)'; then echo 'Protected path — edit blocked' >&2; exit 2; fi; }"
      }]
    }]
  }
}
```

```jsonc
// Python example
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write|Edit",
      "hooks": [{
        "type": "command",
        "command": "jq -r '.tool_input.file_path' | xargs ruff check --fix && jq -r '.tool_input.file_path' | xargs ruff format"
      }]
    }]
  }
}
```

---

## Lesson Promotion

All corrections go to `.claude/tasks/lessons.md`. When patterns stabilize, promote:

| Pattern | Action |
|---------|--------|
| Same mistake repeats | Promote to Hooks (settings.json) |
| Domain-specific knowledge | Move to KNOWLEDGE.md or .claude/skills/ |
| Project-wide rule | Move to AGENTS.md |
| Global rule | Promote to `~/.claude/CLAUDE.md` |
| No longer needed | Delete (save context) |

---

## Customization

### Adding Skills (Slash Commands)

Create a folder in `.claude/skills/<name>/SKILL.md` with YAML frontmatter → invoke with `/name`.
`$ARGUMENTS` is replaced with user input. See `.claude/skills/_template/SKILL.md` for the format.

### Adding Custom Agents

Create a markdown file in `.claude/agents/` with YAML frontmatter to define specialized subagents Claude can delegate to.
See `.claude/agents/_template.md` for the format. Useful when a recurring task benefits from a focused, reusable prompt.

**Example:** `.claude/agents/code-reviewer.md`

```markdown
---
name: code-reviewer
description: Reviews code changes for correctness, style, and potential issues.
model: sonnet
tools: Read, Grep, Glob
---

# Code Reviewer

## When to Delegate
- Before marking a task complete, delegate a final review to this agent
- When reviewing PRs or large diffs

## Instructions
1. Read all changed files
2. Check for: correctness, edge cases, naming, duplication, test coverage
3. Output a structured review with severity levels (critical / warning / nit)

## Constraints
- Read-only: never edit files
- Focus on logic, not style (linter handles style)
```

### Multi-Project Structure

```
~/.claude/CLAUDE.md              # Global (all projects)
~/project-a/AGENTS.md            # Project A context
~/project-a/CLAUDE.md            # Project A Claude rules
~/project-b/AGENTS.md            # Project B context
~/project-b/CLAUDE.md            # Project B Claude rules
```

### Settings Files

| File | Purpose | Git |
|------|---------|-----|
| `.claude/settings.json` | Team hooks (lint, format, path blocking) | Committed |
| `.claude/settings.local.json` | Personal permissions and hooks | Git-ignored |
| `CLAUDE.local.md` | Personal workflow overrides | Git-ignored |

- **`settings.json`**: Shared across the team. Contains hooks that enforce project standards (linting, formatting, path protection). Every team member gets the same behavior.
- **`settings.local.json`**: Personal to each developer. Contains permissions (e.g., `WebFetch` domain allowlists) and local-only hooks. Never committed.

---

## References

**Claude Code Official**
- [Claude Code Docs](https://code.claude.com/docs/en/overview) — overview, architecture, and feature index
- [Memory & CLAUDE.md](https://code.claude.com/docs/en/memory) — file hierarchy, @imports, .claude/rules/, CLAUDE.local.md
- [Hooks Guide](https://code.claude.com/docs/en/hooks-guide) — setup tutorial with copy-paste examples
- [Hooks Reference](https://code.claude.com/docs/en/hooks) — all 12 events, JSON schema, matchers, exit codes
- [Skills](https://code.claude.com/docs/en/skills) — SKILL.md frontmatter, context:fork, $ARGUMENTS
- [Subagents](https://code.claude.com/docs/en/sub-agents) — .claude/agents/ format, tools, permissionMode
- [Settings](https://code.claude.com/docs/en/settings) — settings.json schema, scopes, permissions
- [Best Practices](https://code.claude.com/docs/en/best-practices) — CLAUDE.md tips, subagent patterns, context management

**Standards & Guides**
- [AGENTS.md Standard](https://github.com/agentsmd/agents.md) — universal agent instructions format (60k+ repos)
- [Writing a Good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md) — context engineering best practices
- [Claude Code Best Practices (community)](https://rosmur.github.io/claudecode-best-practices/) — aggregated patterns from r/ClaudeAI and practitioner blogs
- [Boris Cherny's Claude Code Workflow](https://x.com/bcherny/status/2007179832300581177) — workflow practices from the creator of Claude Code

**Community & Tools**
- [awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) — curated skills, hooks, commands, and learning resources
- [claude-code-hooks-mastery](https://github.com/disler/claude-code-hooks-mastery) — full hook event implementation with examples
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) — comprehensive config collection (agents, skills, hooks, MCPs)
- [cclint](https://github.com/carlrannaberg/cclint) — CLAUDE.md linter
- [Claude Code Changelog](https://code.claude.com/docs/en/changelog) — track breaking changes to hooks, skills, settings