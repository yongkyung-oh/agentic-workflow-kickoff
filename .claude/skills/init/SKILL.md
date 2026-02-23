---
name: init
description: Analyze the project and complete Claude Code bootstrap. Use when setting up a new project with the bootstrap files.
disable-model-invocation: true
---

Analyze the project and complete Claude Code bootstrap.

> If `/init` does not work (skills not enabled, different CLI version), use the bootstrap prompt from README.md instead.

## Steps

1. Read @_BOOTSTRAP_GUIDE.md for full instructions
2. Analyze the project and fill AGENTS.md placeholders:
   - [PROJECT_NAME], one-line description
   - Tech Stack (detect from package.json, pyproject.toml, etc.)
   - Build & Run commands
   - Directory Structure (reflect actual structure)
   - Code Conventions (detect linter/formatter configs, add pointers)
3. Ensure .claude/ directory structure exists: specs, sessions, tasks
   - If template files already exist (from bootstrap copy), keep them
   - If missing, create them from the template definitions in _BOOTSTRAP_GUIDE.md
4. Move bootstrap files:
   - KNOWLEDGE.md → .claude/KNOWLEDGE.md
   - _BOOTSTRAP_GUIDE.md → .claude/_BOOTSTRAP_GUIDE.md
5. Add to .gitignore (if not already present):
   - CLAUDE.local.md
   - .claude/settings.local.json
6. Ensure .claude/skills/_template/ exists (official skills structure)
7. Verify and print summary

## Verification

Before printing the summary, confirm each checkpoint:

- [ ] AGENTS.md has no remaining placeholders (`[PROJECT_NAME]`, `[e.g.,`, `[description]`)
- [ ] `.claude/KNOWLEDGE.md` exists and root `KNOWLEDGE.md` is removed
- [ ] `.claude/_BOOTSTRAP_GUIDE.md` exists and root `_BOOTSTRAP_GUIDE.md` is removed
- [ ] `.gitignore` includes `CLAUDE.local.md` and `.claude/settings.local.json`
- [ ] `.claude/specs/README.md` and `.claude/specs/_template.md` exist
- [ ] `.claude/sessions/README.md` and `.claude/sessions/_template.md` exist
- [ ] `.claude/tasks/lessons.md` exists
- [ ] `.claude/skills/_template/SKILL.md` exists

If any check fails, fix it before completing. Report failures in the summary.