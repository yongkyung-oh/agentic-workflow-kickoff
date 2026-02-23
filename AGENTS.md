<!-- This file is read by all AI coding agents: Claude, Codex, Gemini CLI, Antigravity, etc. -->

# [PROJECT_NAME]

[One-line project description]

## Tech Stack

- Language: [e.g., TypeScript 5.x, Python 3.12]
- Framework: [e.g., Next.js 15, FastAPI]
- Database: [e.g., PostgreSQL 16, MongoDB]
- Package Manager: [e.g., pnpm, uv]

## Build & Run

```
[build]    : # e.g., npm run build | python -m build | make
[dev]      : # e.g., npm run dev | uvicorn main:app --reload
[test]     : # e.g., npm run test | pytest
[test:one] : # e.g., npm run test -- path/to/test | pytest path/to/test.py -v
[lint]     : # e.g., npm run lint | ruff check .
[typecheck]: # e.g., npx tsc --noEmit | mypy src/
```

## Directory Structure

```
project/
├── src/                # [description]
│   ├── components/     # [description]
│   ├── lib/            # [description]
│   └── ...
├── tests/              # [description]
├── docs/               # [description]
└── ...
```

<!-- Replaced with actual structure when /init is run -->

## Code Conventions

- Style config: @.eslintrc* / @.prettierrc* / @pyproject.toml (enforced by linter/formatter)
- [Project-specific naming conventions here]
- [File naming patterns here]

## Core Principles

- **Simplicity First**: Make every change as simple as possible. Impact minimal code.
- **No Laziness**: Find root causes. No temporary fixes. Senior developer standards.
- **Minimal Impact**: Changes should only touch what's necessary. Avoid introducing bugs.

## Git

- Branch: `feat/`, `fix/`, `chore/`, `refactor/` prefix
- Commit: Conventional Commits format
- Never force push to main/master

## Constraints

- NEVER commit .env files, secrets, or credentials
- New API endpoints / functions must include tests
- Ask for approval before adding external packages
- State the reason when deleting existing code
- Protected paths — no direct edits: migrations/, dist/, build/, .env
