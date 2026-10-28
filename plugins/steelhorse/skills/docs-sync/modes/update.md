# Update mode

Run after a code change, or when the user asks to document something. Goal: the smallest set of doc edits that makes the docs true again.

## 1. Decide whether docs are affected

Update docs when the change:
- adds, removes or renames a feature, system, or public API
- changes behavior that a doc describes
- changes how pieces connect (new dependency, new event, new data flow)
- makes a non-obvious design decision (a choice a future reader would otherwise have to re-derive or might reverse)
- changes setup, build, or run steps

Skip when it is a typo fix, a pure refactor with no behavior or structure change, formatting, or a bug fix that restores documented behavior. If unsure, check whether any doc mentions the touched code (step 2); no mention and no decision to record means nothing to do.

## 2. Find the affected docs

1. Look up the changed paths in `docs/map.md` (or the project's equivalent). Each matching row names the docs that describe that code.
2. Grep the docs for the changed class, file or feature names to catch mentions the map misses.
3. If a new feature or system has no doc, create one from [../templates/feature.md](../templates/feature.md) and add a `map.md` row.

## 3. Edit

- Edit only the sections that changed. Do not rewrite a whole file.
- **Feature/architecture docs:** keep Purpose, How it works, Key files, API/events, Gotchas, Related current. Remove statements that are no longer true instead of leaving them next to the new text.
- **New decision:** add an entry at the **top** of `decisions.md` in the log's format (see [../templates/decisions.md](../templates/decisions.md)). Use today's date. Never edit or delete an old entry to change history; if a decision is reversed, add a new entry with a `Supersedes:` line naming the old one.
- **Renames/moves:** update every doc and `map.md` row that mentions the old path.
- **Deletions:** remove the doc (or section) and its `map.md` row, and fix links that pointed to it.
- **README / CLAUDE.md:** propose the edit, show it, and apply after approval. Only touch the parts that are now wrong (feature list, setup steps, structure overview).
- Keep a doc's existing language and tone. Check for any docs-language setting in `CLAUDE.md` for new files.

## 4. Check links and finish

- Every link and path you wrote exists.
- New docs are linked from the architecture index (or the equivalent) and have a `map.md` row.
- Tell the user, in a line or two, which docs you changed and why. If you skipped docs on purpose, say so.
