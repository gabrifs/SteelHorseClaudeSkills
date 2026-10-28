# SteelHorseClaudeSkills

Reusable [Claude Code](https://claude.com/claude-code) skills for game development workflows, packaged as a plugin marketplace. Install once, get updates, and keep every project on the same version.

These skills are built for Claude Code only. They rely on its plugins, `CLAUDE.md`, and the `git` and `gh` CLIs, so they are not meant for Claude.ai or the API.

## Install (recommended)

In a Claude Code session:

```
/plugin marketplace add gabrifs/SteelHorseClaudeSkills
/plugin install steelhorse@steelhorse-skills
/plugin install steelhorse-unity@steelhorse-skills   # Unity projects only
```

`steelhorse-unity` depends on `steelhorse`, so installing it pulls the core plugin in automatically.

Plugin skills are prefixed with the plugin name, for example `/steelhorse:create-pr`. Claude also picks them up on its own when your request matches a skill's description.

**To share with your team,** run `claude plugin marketplace add gabrifs/SteelHorseClaudeSkills --scope project` in the repo and commit the `.claude/settings.json` it writes. Teammates get the marketplace registered when they trust the folder.

**Updates:** run `/plugin marketplace update steelhorse-skills`, or turn on auto-update for the marketplace in `/plugin`. These plugins don't pin a version, so installs follow the latest commit.

## Plugins and skills

### `steelhorse` (core)

| Skill | Purpose | Status |
|---|---|---|
| [`conventional-commits`](plugins/steelhorse/skills/conventional-commits/SKILL.md) | Commit messages in Conventional Commits format, no AI attribution | Ready |
| [`create-pr`](plugins/steelhorse/skills/create-pr/SKILL.md) | Pull requests with a grouped changelog; asks about a test plan; never merges | Ready |
| [`plan-before-execute`](plugins/steelhorse/skills/plan-before-execute/SKILL.md) | Propose a plan and get alignment before non-trivial work | Ready |
| [`docs-sync`](plugins/steelhorse/skills/docs-sync/SKILL.md) | Keep architecture, decisions, feature docs and README in sync with code | Ready |
| [`project-setup`](plugins/steelhorse/skills/project-setup/SKILL.md) | Write the CLAUDE.md working-agreements block for installed skills and scaffold docs; manual (`/steelhorse:project-setup`) | Ready |

### `steelhorse-unity`

| Skill | Purpose | Status |
|---|---|---|
| [`unity-dev`](plugins/steelhorse-unity/skills/unity-dev/SKILL.md) | Unity conventions and safety rules, with a bundled [`.editorconfig`](plugins/steelhorse-unity/skills/unity-dev/assets/.editorconfig) | Ready |

An Unreal plugin is planned.

## What each skill does

Claude loads a skill on its own when your request matches it. Examples of prompts that trigger them:

| Skill | Try saying | What happens |
|---|---|---|
| `conventional-commits` | "commit my staged changes" | Reads the diff and recent history, writes a `type(scope): summary` message, splits unrelated changes into separate commits, and adds no AI attribution. |
| `create-pr` | "open a PR for this branch" | Checks the branch and base, drafts a Conventional Commit title and a changelog grouped by commit type, asks whether to include a test plan, shows everything, and only creates the PR after you confirm. Never merges. |
| `plan-before-execute` | "add a health pickup script" | For anything beyond a tiny edit, states a short plan (goal, steps, risks, recommendation) and waits for your go-ahead before changing files. |
| `docs-sync` | "document the inventory system", "audit the docs" | Updates the affected docs in the same pass as the code, keeps a code-to-doc map and a decisions log, and can report drift without changing anything (audit mode). Follows an existing docs layout when there is one. |
| `project-setup` | `/steelhorse:project-setup` | Makes sure `CLAUDE.md` exists, writes a managed block listing the skills you have installed, warns about missing companions, and offers to scaffold docs. Safe to re-run. Manual only. |
| `unity-dev` | "write a script that rotates an object" | Follows your project's patterns, never hand-edits `.meta`, `.unity`, `.prefab` or `.csproj` files, offers the bundled `.editorconfig` if the project has none, and writes code that satisfies it. |

## Install manually (fallback)

If you can't use plugins, clone the repo and copy the skill folders you want from `plugins/*/skills/` into your project's `.claude/skills/` (or `~/.claude/skills/` for every project). Copied skills use their bare names, such as `/create-pr`.

Keep these companions together, because skills refer to each other:

| Skill | Also copy |
|---|---|
| `create-pr` | `conventional-commits` |
| `unity-dev` | `plan-before-execute`, `docs-sync` |
| `docs-sync` | `plan-before-execute` (recommended) |

Copies don't update. Re-copy the folder to upgrade, and don't edit the copy if you want to keep receiving changes.

## License

[MIT](LICENSE)
