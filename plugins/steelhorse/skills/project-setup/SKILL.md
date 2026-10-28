---
name: project-setup
description: Set up a project to use the SteelHorse skills - make sure CLAUDE.md exists (running /init if not), write a managed block of working agreements for the installed skills, and optionally scaffold docs with docs-sync. Run when starting a new project or after installing more skills.
disable-model-invocation: true
---

# Project Setup

One-time (and re-runnable) setup that makes the installed skills dependable. Skills only load when a request matches them, but `CLAUDE.md` is always loaded, so this skill writes a short block of working agreements there that points at each installed skill.

This skill never copies, downloads or installs skills. The user installs them first (as a plugin, or by copying folders into `.claude/skills/`); this skill works with whatever is there.

Follow `plan-before-execute`: show the plan and the exact block before writing anything.

## Steps

### 1. Look at the project

- **CLAUDE.md:** does `CLAUDE.md` exist at the project root?
- **Engine:** Unity (`Assets/` + `ProjectSettings/ProjectVersion.txt`), Unreal (`*.uproject`), Godot (`project.godot`), or other.
- **Docs:** does the project already have docs (`docs/`, `.claude/notes/`, a wiki folder)?
- **Docs language:** does `CLAUDE.md` already say which language docs use?

### 2. Make sure CLAUDE.md exists

If there is no `CLAUDE.md`, invoke the built-in `init` skill to create one, and wait for it to finish. Then continue with the file it produced. If `init` is not available, offer a minimal `CLAUDE.md` (project name and a one-line description) and ask before creating it.

If one exists, leave everything outside the managed block untouched.

### 3. Find the installed skills

Use the skills available in this session, not the contents of any folder; they may come from a plugin or from `.claude/skills/`, and plugin skills carry a prefix (`steelhorse:create-pr`). Match on the bare name.

Known skills and what the block says about each:

| Skill | Directive line |
|---|---|
| `conventional-commits` | All commits follow Conventional Commits, with no AI attribution trailers. See `conventional-commits`. |
| `create-pr` | Open PRs with `create-pr`: Conventional Commit title, changelog grouped by commit type, ask about a test plan, never merge. |
| `plan-before-execute` | Before non-trivial work, state a plan and get alignment. See `plan-before-execute`. |
| `docs-sync` | Update docs in the same pass as the code change. See `docs-sync`. |
| `unity-dev` | Unity project: the Editor owns `.meta`, `.unity`, `.prefab` and `.csproj` files; follow the project `.editorconfig`. See `unity-dev`. |

Only list skills that are actually installed. Skip skills that are not in the table (the user's own).

### 4. Check companions

Skills refer to each other. Warn about what is missing and say how to add it; do not fetch anything yourself.

| If installed | Also needs |
|---|---|
| `create-pr` | `conventional-commits` |
| `unity-dev` | `plan-before-execute`, `docs-sync` |
| `docs-sync` | `plan-before-execute` (recommended) |

Also suggest `unity-dev` if this is a Unity project and it is not installed (and the matching engine skill for other engines if one exists).

For missing skills, tell the user to install the `steelhorse` plugin (which carries the core skills), or to copy the folder into `.claude/skills/`.

### 5. Show the plan

Present, then wait for approval:
- whether `CLAUDE.md` will be created (via `init`) or only updated
- the exact managed block (below)
- the companion warnings
- whether you will offer docs setup (step 7) and which docs language applies (default English)

### 6. Write the managed block

Insert (or update) this block in `CLAUDE.md`, keeping the markers exactly:

```markdown
<!-- steelhorse-skills:start -->
## Working agreements

Maintained by the `project-setup` skill. Edit outside these markers; re-running replaces what is inside.

- <one directive line per installed skill, from the table in step 3>
- Docs language: English
<!-- steelhorse-skills:end -->
```

Rules:
- **Markers present:** replace only what is between them. Add lines for newly installed skills and remove lines for skills no longer installed, and show the user the difference first.
- **No markers:** append the block at the end of `CLAUDE.md`. Do not move or reformat anything else.
- **Existing equivalents:** if `CLAUDE.md` already states a directive (for example its own commit-message rule), do not remove it. Mention the overlap and let the user decide.
- **Docs language:** keep an existing docs-language setting; otherwise write English unless the user says otherwise.
- Keep the block short. Details live in the skills.

### 7. Offer docs setup

If `docs-sync` is installed and the project has no docs, offer to run it in Init mode (see `docs-sync`). If docs already exist, offer an Audit instead. Do not run either without a yes.

### 8. Finish

Summarize what changed (file, block, skills listed) and what the user still has to do (missing companions, `.editorconfig` for Unity projects is offered by `unity-dev` on first use). Do not commit; suggest a message such as `chore(claude): add working agreements for installed skills` and let the user decide.
