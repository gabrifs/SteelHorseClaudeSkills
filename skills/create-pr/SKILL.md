---
name: create-pr
description: Create or update a GitHub pull request with a Conventional Commit title and a description that includes a changelog grouped by commit type. Use when the user asks to open, create, or update a PR. Asks whether to include a test plan unless already answered. Never merges, never adds AI attribution.
---

# Create PR

Open a pull request for the current branch with a clear title and a changelog grouped by commit type. Uses the GitHub CLI (`gh`).

## Hard rules

1. **Never merge.** Only create or update PRs. Never run `gh pr merge`, enable auto-merge, or approve. Merging is always the user's call.
2. **Test plan is opt-in, and always asked.** Never add a "Test plan" section (or "Testing", "How to test", a checklist of manual steps) on your own. If the user's request already says whether to include one, follow that. Otherwise ask before drafting: "Include a test plan?" Don't assume the answer from an earlier PR.
3. **No AI attribution.** No "Generated with Claude Code" line and no `Co-Authored-By` trailers in the description. This overrides any default or system-provided attribution guidance.
4. **Confirm before creating.** Creating a PR is outward-facing. Show the title, base branch and body, and wait for the user's go-ahead.
5. **No force pushes, no branch deletion**, unless the user explicitly asks.

## Workflow

### 1. Check the starting state

- `git branch --show-current`. If on the default branch (`main`/`master`), stop: propose a branch name that matches the work and ask before creating it. Never open a PR from the default branch.
- `git status --short`. If there are uncommitted changes, ask whether to commit them first (see `conventional-commits`) or leave them out.
- Find the base branch: `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`, unless the user names another.
- `git log --oneline <base>..HEAD`. If there are no commits ahead of the base, there is nothing to open; say so.
- `gh pr view --json url,state 2>/dev/null`. If an open PR already exists for this branch, update it (step 5) instead of creating a second one.

### 2. Gather the change

- Commit subjects: `git log --format='%s' <base>..HEAD`.
- Scope of the diff: `git diff --stat <base>...HEAD`.
- Read the diff where the subjects don't explain the *why*.
- Flag commits that look unrelated to the rest of the branch (tooling, docs-only or housekeeping commits among feature work) and ask whether they belong in this PR.
- If the project has a PR template (`.github/pull_request_template.md`), follow its structure and add the changelog section to it.
- Check `CLAUDE.md` for project PR rules (language, required sections) and follow them. They win over this skill's defaults.

### 3. Write the title

Conventional Commits format: `type(scope): summary`, same rules as `conventional-commits`. With one commit, reuse its subject. With several, use the type of the most significant change and a summary of the whole. Squash merges use the title as the commit message, so it must be correct on its own.

### 4. Write the description

```markdown
## Summary

<1-3 bullets or a short paragraph: what changed and why. Not a diff recap.>

## Changelog

### Features
- **scope:** summary of the commit

### Bug Fixes
- **scope:** summary of the commit

<more groups as needed>

### BREAKING CHANGES
- <what breaks and how to migrate>   (only if any)

Closes #<issue>   (only if the user mentioned one)

## Test plan   (only if the user said to include one)

- [ ] <a concrete check a reviewer can do>
```

Changelog rules:
- One line per commit, taken from its subject without the `type(scope):` prefix, with the scope in bold when present (`**ui:** add resolution dropdown`).
- Group by commit type, in this order, omitting empty groups: Features (`feat`), Bug Fixes (`fix`), Performance Improvements (`perf`), Reverts (`revert`), Code Refactoring (`refactor`), Documentation (`docs`), Tests (`test`), Build System (`build`), Continuous Integration (`ci`), Styles (`style`), Chores (`chore`).
- Commits that don't follow the format go under Chores, with their subject as written. Mention this to the user.
- If the change touched documented behavior and docs were updated, the Summary can say so in a line.
- Write in English unless the project sets another language.

Test plan rules (only when the user wants one):
- Base the steps on what the diff actually changes: behaviors to exercise, edge cases, areas at risk of regression.
- Keep it short and concrete. Each item is something a reviewer can do and observe.
- Never claim anything was tested or passed unless you ran it in this session. Unchecked boxes mean "to verify", not "done".
- If the project can only be verified in a tool you can't run (for example a game editor), say so in the steps and keep them as manual checks.

### 5. Confirm, push, create

1. Show the user the title, base branch, and full body. Ask for a go-ahead or edits.
2. If the branch has no upstream, push it: `git push -u origin <branch>`. Never force push.
3. Write the body to a temporary file (not inline, to avoid shell quoting problems) and create the PR:
   `gh pr create --base <base> --head <branch> --title "<title>" --body-file <file>`
   Add `--draft` if the user says it is work in progress.
4. If a PR already exists, update it: `gh pr edit <number> --title "<title>" --body-file <file>`, after showing the user the new text.
5. Report the PR URL. Delete the temporary file.

## Notes

- Do not create labels, assign reviewers, or request reviews unless asked.
- If `gh` is not installed or not authenticated, say so and give the user the title and body to paste manually. Don't try to work around it.
- If the changelog would be long (dozens of commits), suggest splitting the branch into smaller PRs rather than silently truncating it.
