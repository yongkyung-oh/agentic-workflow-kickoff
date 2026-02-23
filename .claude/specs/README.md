# Specs Index

| # | Spec | Status | Created | Updated |
|---|------|--------|---------|---------|
| — | Add a row when creating a new spec | — | — | — |

## Workflow

1. Copy `_template.md` to `NNN-spec-name.md`
2. Add a row to this README's table
3. Status: `draft` → `in-progress` → `done` | `cancelled`
4. Update this README when status changes

## Scaling

As specs accumulate:

- **Subfolders by domain**: `specs/auth/`, `specs/api/`, `specs/infra/` etc.
- **Archive completed work**: move `done` / `cancelled` specs to `specs/archive/`
- **Keep this table lean**: only active specs (`draft`, `in-progress`) in the table above
- **Naming**: always use `NNN-kebab-name.md` prefix for sort order, even inside subfolders
