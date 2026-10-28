# Audit mode

Compare the docs against the code and report drift. **Audit is report-only.** Fix nothing until the user approves the list.

For a large project, state the scope you will check (all docs, or one area) and get a go-ahead first, following `plan-before-execute`.

## Checks

1. **Dead references.** For each path, file name, class or method a doc mentions, confirm it still exists (Grep/Glob). Report the ones that don't, with the doc that mentions them.
   - **Match whole words and resolve paths in context.** A substring hit is not a match (`CharacterCreatorController` inside `PlayerCharacterCreatorController` is a different name). A partial path like `Flame/FlameConfigs` is relative to whichever folder the doc's own text implies, so search by file name before calling it missing.
   - **Skip intentional history.** A living doc may name removed things on purpose ("renamed from X", "replaces Y", "tried and dropped"). Read the surrounding sentence; report the name only if the doc presents it as current.
2. **Map integrity.** Every `map.md` row's code path matches something, and its doc exists. Report broken rows.
3. **Undocumented code.** List top-level code areas (folders/systems) that no `map.md` row covers. Rank by how central they look (size, how many places reference them).
4. **Contradictions.** Spot-check behavior claims in the docs against the code, prioritizing recently changed code (`git log` on the mapped paths since the doc last changed). Report claims the code now contradicts.
5. **Decisions log.** Check each entry's `Affects:` paths still exist. A missing path means the entry is outdated or the code moved; report it. Never edit old entries to fix history.
6. **Orphans and links.** Docs not linked from the architecture index, and links that point nowhere.
7. **README / CLAUDE.md.** Setup steps, feature list and structure overview still match reality.

## Report format

Group by severity, most useful first:

```
Contradicts the code (N)
- docs/features/dungeon.md: says X, but Path/File.cs does Y

Dead references (N)
- docs/architecture.md: mentions `OldClass`, not found

Undocumented (N)
- Assets/Game/Inventory/ - no map row, ~40 files

Housekeeping (N)
- docs/features/audio.md not linked from architecture.md
```

End with a short proposed fix plan and ask which items to apply. Apply them through [update.md](update.md) rules (direct edits for feature/architecture/decisions/map; propose first for README and CLAUDE.md).

## Limits

You can verify that things exist and spot clear contradictions. You cannot prove every sentence is true. Say so, and list what you did not check.
