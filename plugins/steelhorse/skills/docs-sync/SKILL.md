---
name: docs-sync
description: Keep project documentation in sync with the code - architecture, per-feature docs, a design-decisions log, and the README. Use after adding, changing or removing a feature or system, after making a non-obvious design decision, when the user asks to document something, to audit docs for drift, or to set up docs in a new project.
---

# Docs Sync

Docs are only useful if they match the code. This skill keeps them current with small, targeted edits made in the same pass as the code change, and structured so you can find and edit the right file without reading all of them.

## Doc layout

Default layout (create only what the project needs):

```
README.md                  # human-facing: pitch, setup, overview
CLAUDE.md                  # your entry point: rules + index of docs
docs/
├── architecture.md        # how the code is organized, subsystems, data flow
├── decisions.md           # log of non-obvious decisions and why
├── features/<feature>.md  # one file per feature or system
└── map.md                 # code path -> doc mapping (the sync key)
```

**Follow an existing layout.** If the project already documents things elsewhere (for example `.claude/notes/`, a wiki folder, topic files under `architecture/`), use that structure and conventions instead of creating a parallel one. Add `docs/map.md` pointing at the existing files if there is no map. Only use the default layout for new projects, or when the user asks for it.

Templates: [templates/](templates/) (`architecture.md`, `feature.md`, `decisions.md`, `map.md`).

## Modes

Pick the mode from the request, then read its file:

| Mode | When | Read |
|---|---|---|
| **Update** (default) | After a code change, or when asked to document something | [modes/update.md](modes/update.md) |
| **Audit** | "Check the docs", "are docs up to date?", before a release | [modes/audit.md](modes/audit.md) |
| **Init** | New project, or a project with no docs | [modes/init.md](modes/init.md) |

## Principles

1. **Same pass, same commit.** A code change that affects documented behavior updates the docs in the same pass, so they land together.
2. **One home per fact.** Put each fact in one file and link to it elsewhere. Duplicates are what go stale.
3. **Document the why, not the what.** Skip anything the code already says clearly (signatures, obvious flow). Write the contracts, the reasons, the gotchas, and how pieces connect.
4. **Small, focused files.** One topic per file, roughly under 200 lines. Split when a file grows past that. Stable headings, so a section can be edited without rereading the file.
5. **Reference by path and symbol, never by line number.** Line numbers rot immediately. Use `Path/To/File.cs` and `ClassName.MethodName`.
6. **Link related docs.** End each topic file with a `**Related:**` line pointing at the docs it touches, so cross-cutting changes are traceable.
7. **No dates in prose.** Only the decisions log carries dates. Don't write "as of <date>" or "last verified" lines; they go stale.
8. **Don't duplicate the changelog.** Release history belongs to the project's changelog tooling, not these docs.

## Autonomy

- **Edit directly:** feature docs, architecture docs, the decisions log, and `map.md`.
- **Propose first:** the `README.md` and `CLAUDE.md`. Show the proposed change and apply it after approval. These are the project's public face and the user's own rules.
- **Never touch:** generated files, changelogs maintained by tooling, and files the user marked as hand-written only.

## Language

Write docs in English unless the project says otherwise. Check `CLAUDE.md` for a docs-language setting (for example `Docs language: pt-BR`) and follow it. When editing an existing doc, keep its language and tone.

## Integration

- Engine and workflow skills (such as `unity-dev`) invoke this skill after non-trivial changes.
- Commits that change documented behavior include the doc update (see `conventional-commits`), and PR descriptions mention it (see `create-pr`).
- For large or multi-file doc work, follow `plan-before-execute` first.
