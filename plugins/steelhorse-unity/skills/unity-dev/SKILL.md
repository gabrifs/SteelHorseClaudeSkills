---
name: unity-dev
description: Conventions and safety rules for working in a Unity project (C# scripts, ScriptableObjects, prefabs, scenes). Use whenever reading, writing or changing code or assets in a Unity project, especially before editing scripts or touching .unity, .prefab or .meta files.
---

# Unity Development

Rules for working in a Unity project. Sections marked **author's defaults** are personal preferences: they apply unless the project's own `CLAUDE.md` says otherwise.

## Before you start

1. Read the project's `CLAUDE.md` and its architecture/docs (typically under `docs/`) for the systems you are about to touch. Project docs win over anything here.
2. For non-trivial work, follow the `plan-before-execute` skill: state the plan and get alignment first. If an existing pattern looks wrong or you would have to break it, say so and discuss before deviating.
3. Match the surrounding code: naming, structure, event wiring, how services and singletons are accessed. Do not introduce a new style because you prefer it.

## Hard rules: the Editor owns these

The Unity Editor generates and manages these files. Hand-editing them corrupts references or gets silently overwritten.

- **Never create or edit `.meta` files.** The Editor creates them automatically (it is usually running in the background). Do not generate GUIDs.
- **Never hand-edit `.unity` or `.prefab` files.** When a change needs scene or prefab work (adding a component, wiring a reference, creating an asset instance), do the code, then give the user exact Inspector steps: which object, which component, which field, which asset.
- **Never hand-edit generated project files** (`*.csproj`, `*.sln`, `*.slnx`). Unity regenerates them.
- **Do not assume you can compile or run.** Compiling and running happen in the Editor. After code changes, ask the user to check the Console for compile errors instead of claiming the code works.
- Do not change `ProjectSettings/ProjectVersion.txt` or the Editor version.

## EditorConfig

This skill bundles a Unity-oriented `.editorconfig` at `assets/.editorconfig` (in this skill's folder). It belongs at the project root, next to `Assets/`.

- **Missing:** if the project root has no `.editorconfig` (ignore the ones inside `Library/` and `Packages/`), offer to install the bundled one. Ask first and never install it silently. Warn that it may raise warnings on existing code, since it enforces naming and style rules.
- **Already present:** never overwrite it. If the user wants the bundled rules, summarize the differences and propose a merge.
- **Once installed, it is authoritative** for formatting and naming (indentation, braces, `var` usage, member naming, line endings). It beats my habits and the author's defaults below.
- **Do not read it in full.** It is thousands of lines, mostly generated IDE settings. Grep for the specific rule you need (for example `dotnet_naming_rule`, `csharp_style_var`, `end_of_line`).
- **Write compliant code.** When the file is installed, the code you write must satisfy its rules the first time (naming, explicit types vs `var`, braces, event naming, modifier order), so the IDE shows no new warnings. Grep the relevant rule before writing in an area you haven't checked. Existing code that violates a rule is not yours to rewrite; follow the rule in what you write or change.
- **Line endings:** new files follow the `end_of_line` it sets. Check that files you create match, and fix them if not. Do not mass-convert existing files; a repo's `.gitattributes` or mixed history may already handle that.

## After you change something

If the change affects documented behavior, adds a feature or system, or makes a non-obvious design decision, invoke the `docs-sync` skill (if installed) in the same pass so docs and code land together.

## Author's defaults

Overridable in the project's `CLAUDE.md`.

- **Enum branching uses `switch`.** Use a `switch` statement or `switch` expression for any branch on an enum value, even with two cases. No ternaries or if/else chains on enums. Every case stays visible on its own line and the code scales as the enum grows.
- **Build the seam when "more later" is announced.** If the user says "just X for now, more later", create the chaining or wrapper method now (for example `ApplyModifiers` that currently calls one modifier) instead of inlining the single case.
- **Comments are short and about the *why*.** Do not narrate what the code literally does; well-named code explains itself. Comment what the code can't say: a non-obvious reason, a Unity quirk, an ordering constraint, a workaround, a trade-off. One line is usually enough. Not dense, not empty. If the surrounding code has an established comment style that differs, follow it.

```csharp
// Bad: narrates the code
// Loop through all items and add them to the list
foreach (var item in items) list.Add(item);

// Good: explains what the code can't
// Destroy() is deferred, so the duplicate's Awake still runs this frame
if (Instance != null) { Destroy(gameObject); return; }
```

## More

See [conventions.md](conventions.md) for Unity pitfalls worth knowing before writing lifecycle, singleton, ScriptableObject and serialization code.
