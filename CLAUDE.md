# Claude Code Instructions

> This file is auto-loaded by Claude Code at session start.
> It defines Claude Code-specific execution rules. For project context shared with all agents, see @AGENTS.md.

For project context (commands, architecture, conventions), see @AGENTS.md.
For domain expertise, see @.claude/KNOWLEDGE.md.

## Workflow Orchestration

### 1. Plan Node Default

- Enter plan mode for ANY non-trivial task (3+ steps or architectural decisions)
- If something goes sideways, STOP and re-plan immediately — don't keep pushing
- Use plan mode for verification steps, not just building
- Write detailed specs upfront to reduce ambiguity
- Always ask clarifying questions if requirements are ambiguous

### 2. Subagent Strategy

- Use subagents liberally to keep main context window clean
- Offload research, exploration, and parallel analysis to subagents
- For complex problems, throw more compute at it via subagents
- One task per subagent for focused execution

### 3. Self-Improvement Loop

- After ANY correction from the user: update `.claude/tasks/lessons.md` with the pattern
- Write rules for yourself that prevent the same mistake
- Ruthlessly iterate on these lessons until mistake rate drops
- Review lessons at session start for relevant project

### 4. Verification Before Done

- Never mark a task complete without proving it works
- Run tests, check logs, demonstrate correctness
- Diff behavior between main and your changes when relevant
- Ask yourself: "Would a staff engineer approve this?"

### 5. Demand Elegance (Balanced)

- For non-trivial changes: pause and ask "is there a more elegant way?"
- If a fix feels hacky: step back and implement the elegant solution
- Skip this for simple, obvious fixes — don't over-engineer

### 6. Autonomous Bug Fixing

- When given a bug report: just fix it. Don't ask for hand-holding
- Point at logs, errors, failing tests — then resolve them
- Start by writing a test that reproduces it, then fix until it passes
- Never delete or skip a failing test to make the suite pass

## Specs & Sessions

- When starting a new task, check or create a spec in .claude/specs/
- At session start, read .claude/sessions/README.md for the latest handoff
- At session end, write a session record in .claude/sessions/ and update README.md
- When spec status changes, update .claude/specs/README.md as well
- As specs grow, organize with subfolders and archive completed specs. Keep README indexes current.

## Model Routing (Token Efficiency)

- Planning (spec writing, architecture design, code review) → reasoning-heavy model
- Implementation (coding, tests, refactoring) → fast model
- Rule of thumb: "multiple valid answers" → reasoning-heavy, "clear spec exists" → fast
- Suggest model switch after Technical Approach is approved

## Hooks (Optional)

Hooks run deterministic shell commands on tool events (lint, format, path blocking).
Set up in `.claude/settings.json` — see `.claude/_BOOTSTRAP_GUIDE.md` for examples.
Not required for basic usage.