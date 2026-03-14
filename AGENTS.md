# justdoit

Collection of Codex-first skills and docs for execution planning.

## Critical Rules

- Keep `README.md` and `README.ru.md` aligned.
- Every skill folder must contain `SKILL.md`, `README.md`, and `README.ru.md`.
- New skill or new top-level component = MINOR version bump on release.
- Skill updates, docs-only changes, and fixes = PATCH version bump on release.
- Do not release without updating `CHANGELOG.md`.

## Structure

| Path | Purpose |
| --- | --- |
| `skills/` | Installable skills (Codex-first, Claude Code-compatible) |
| `assets/` | Shared visuals used by READMEs |

## Repo Contract

- Root `README` files explain the repo at a product level.
- Skill READMEs explain the concrete skill, when to use it, and how to install it.
- Local execution docs may be generated during runs, but they are not committed in this repo.

## Release Checklist

1. Update `CHANGELOG.md`
2. Update root READMEs if repo-level positioning changed
3. Update skill READMEs if install or behavior changed
4. Verify asset links render correctly
5. Tag and release only after docs are in sync
