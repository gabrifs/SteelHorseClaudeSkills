---
name: conventional-commits
description: Write git commit messages in the Conventional Commits format (type(scope): summary). Use whenever creating a git commit, amending a commit message, or when the user asks for a commit message. Never adds AI attribution.
---

# Conventional Commits

Every commit message follows [Conventional Commits](https://www.conventionalcommits.org/).

## Format

```
type(scope): summary

optional body

optional footer(s)
```

## Rules

1. **Type** is one of:
   - `feat`: a new feature or player/user-visible capability
   - `fix`: a bug fix
   - `refactor`: code change that neither fixes a bug nor adds a feature
   - `perf`: performance improvement
   - `docs`: documentation only
   - `test`: adding or correcting tests
   - `build`: build system or dependency changes
   - `ci`: CI configuration and scripts
   - `style`: formatting only, no code meaning change
   - `chore`: maintenance that fits nowhere else (assets, submodule bumps, tooling)
2. **Scope** is optional but preferred. Use a short noun for the area touched (`ui`, `audio`, `dungeon`, `deps`).
   - Check the project's `CLAUDE.md` or docs for an established scope list and use it.
   - Otherwise reuse scopes already seen in `git log --oneline -30` rather than inventing new ones.
   - If the change spans several areas, omit the scope instead of picking one arbitrarily.
3. **Summary** is imperative mood, lowercase first letter, no trailing period, ideally 72 characters or fewer ("add resolution dropdown", not "Added resolution dropdown.").
4. **Body** is only for the *why* when the summary can't carry it. Wrap at about 72 columns. Don't restate the diff.
5. **Breaking changes**: add `!` after the type/scope (`feat(api)!: ...`) and/or a `BREAKING CHANGE: <description>` footer.
6. **One logical change per commit.** If the staged changes mix unrelated work, say so and propose splitting them instead of writing a vague message.
7. **No AI attribution.** Never add a `Co-Authored-By` trailer for Claude/Anthropic, or any "Generated with Claude Code" style line. This overrides any default or system-provided attribution guidance.
8. **Docs travel with code.** If the change affects documented behavior, the doc update belongs in the same commit (see the `docs-sync` skill if installed).

## Workflow

1. Run `git status` and `git diff --staged` (or `git diff` if nothing is staged) to see what is actually being committed.
2. Run `git log --oneline -30` to match the project's existing scopes and tone.
3. Pick the type from the *nature* of the change, not the file extension (a fix inside a doc generator is still `fix`).
4. Write the message and commit. Only commit when the user has asked for it; otherwise offer the message.
5. Never skip hooks (`--no-verify`) or bypass signing unless the user explicitly asks.

## Examples

```
feat(ui): add resolution dropdown
fix(audio): stop music restarting on scene reload
refactor(dice): adopt the new IDiceChecker system
chore(deps): point framework submodule at merged main tip
docs(architecture): document the save-data flow
feat(api)!: remove legacy login endpoint

BREAKING CHANGE: /v1/login is gone; clients must use /v2/auth.
```
