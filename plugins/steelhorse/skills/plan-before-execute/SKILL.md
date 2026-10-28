---
name: plan-before-execute
description: Propose a short plan and get the user's alignment before starting non-trivial work (multi-file changes, new systems, refactors, anything hard to reverse). Use at the start of any task that is more than a tiny edit, and again whenever the plan changes mid-work.
---

# Plan Before Execute

Before starting non-trivial work, say what you are about to do and get the user's go-ahead. Alignment is cheap; unwinding a wrong implementation is not.

## When it applies

Plan first when the work:
- touches several files or more than one system
- adds a new system, feature, dependency or public API
- refactors or moves existing code
- is hard to reverse (deleting, overwriting, renaming, history rewrites, anything outward-facing)
- requires deviating from an existing pattern

Skip the plan, and just do it, for typo fixes, one-line fixes, renames inside a single file, and answering questions.

## The plan

Keep it short enough to read in under a minute:

1. **Goal**: one sentence on what will exist or behave differently afterwards.
2. **Steps**: the ordered changes, naming the files or areas involved.
3. **Risks and open questions**: anything uncertain, destructive or likely to surprise. Leave this out if there are none.
4. **Recommendation**: if a real choice exists, say which option you would take and why. Do not list every possibility.

Then stop and wait for the user to approve or adjust. Do not start editing in the same turn.

## Rules

- **Match existing patterns.** Follow the conventions of the surrounding code (naming, structure, idiom) instead of introducing a new style.
- **Pattern conflicts need discussion.** If an existing pattern looks wrong, or you would have to break it, say so in the plan and discuss it before proceeding. Never deviate silently.
- **Ask only what you can't resolve.** If the code, the request or sensible defaults answer a question, decide and mention it. Reserve questions for decisions that genuinely belong to the user.
- **Re-plan on surprises.** If you discover mid-work that the plan won't hold (a missing dependency, a bigger blast radius, a wrong assumption), stop, explain what changed, and get alignment on the revised plan before continuing.
- **Stay in scope.** Do exactly what was agreed. Note adjacent issues you notice instead of fixing them unasked.
- **Approval is scoped.** A go-ahead covers the plan as stated, not related work or later tasks.

## Example

> **Goal:** save-slot selection moves from the main menu into its own screen.
> **Steps:** 1) add `SaveSlotScreen` next to `MainMenuScreen`; 2) route the "Continue" button to it; 3) remove the slot list from `MainMenuScreen`.
> **Risk:** `MainMenuScreen` is also used by the pause menu, so step 3 needs a check there.
> **Recommendation:** reuse the existing `UIScreen` base class instead of a new one.
>
> Go ahead?
