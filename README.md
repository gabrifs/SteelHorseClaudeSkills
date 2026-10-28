# SteelHorseClaudeSkills

A collection of reusable [Claude Code](https://claude.com/claude-code) skills for game development workflows. Each skill is a self-contained folder: copy the ones you want into a project and they work.

## Installing a skill

Copy the skill folder into your project's `.claude/skills/` directory (commit it so collaborators get it too), or into `~/.claude/skills/` to make it available in every project on your machine.

```
your-project/
└── .claude/
    └── skills/
        └── conventional-commits/
            └── SKILL.md
```

## Skills

| Skill | Purpose | Status |
|---|---|---|
| [`conventional-commits`](skills/conventional-commits/SKILL.md) | Commit messages in Conventional Commits format, no AI attribution | Ready |
| `create-pr` | Pull requests with a grouped changelog section | Planned |
| `plan-before-execute` | Propose a plan and get alignment before non-trivial work | Planned |
| `unity-dev` | Unity conventions and safety rules | Planned |
| `docs-sync` | Keep architecture, decisions, feature docs and README in sync with code | Planned |

## License

[MIT](LICENSE)
