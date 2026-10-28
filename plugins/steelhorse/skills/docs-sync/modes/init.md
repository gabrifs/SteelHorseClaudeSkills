# Init mode

Set up docs in a new project, or bootstrap them from an existing codebase. This is multi-file work: follow `plan-before-execute` and get approval on the plan before creating files.

## 1. Survey

- Check what docs already exist (`README.md`, `CLAUDE.md`, `docs/`, `.claude/notes/`, wiki folders). If a layout exists, adopt it (see SKILL.md, "Follow an existing layout") and only add what is missing, usually `map.md`.
- For an existing codebase, list the top-level code folders and identify the main systems (read entry points, folder names, and any engine/project config; do not read every file).
- Read `CLAUDE.md` for a docs-language setting. Default to English.

## 2. Plan (show before doing)

Propose:
- the doc files to create, one line each on what it will cover
- the initial `map.md` rows (coarse, folder-level globs)
- the `CLAUDE.md` addition (below)
- anything you will mark as unknown instead of guessing

Wait for approval.

## 3. Create

1. `docs/architecture.md` from [../templates/architecture.md](../templates/architecture.md): code structure, key patterns, and an index of the feature docs.
2. `docs/decisions.md` from [../templates/decisions.md](../templates/decisions.md), empty except for the format header. Do not invent past decisions; only record ones the user or the code history clearly states.
3. `docs/features/<feature>.md` from [../templates/feature.md](../templates/feature.md) for each main system. For a brownfield project, start with the three to five most central systems, not everything.
4. `docs/map.md` from [../templates/map.md](../templates/map.md).
5. Add a short docs index to `CLAUDE.md` (propose first, apply after approval):

```markdown
## Docs

Docs live in `docs/` and are kept in sync with the code by the `docs-sync` skill.
- `docs/architecture.md` - code structure and index of feature docs
- `docs/decisions.md` - log of non-obvious decisions and why
- `docs/map.md` - which code paths each doc describes; check it before and after changing code
```

6. `README.md`: only propose changes. If missing, offer a short one (what it is, setup, structure).

## 4. Rules for brownfield content

- Write only what you can verify from the code or the user. Mark gaps as `TODO: <what is unknown>` instead of guessing.
- Prefer short, accurate docs over complete, speculative ones. Docs grow through update mode.
- Never copy large blocks of code into docs.

## 5. Finish

Summarize what was created and what is intentionally missing. Suggest an audit later, once the project has some history.
