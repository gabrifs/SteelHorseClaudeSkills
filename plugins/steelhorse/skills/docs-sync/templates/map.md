# Docs map

Which docs describe which code. Check it before changing code, and update it when code or docs move.

Use folder-level globs. Finer rows are only worth it when one folder holds several unrelated systems.

| Code path | Doc |
|---|---|
| `path/to/system/**` | [features/system.md](features/system.md) |
| `path/to/other/**` | [features/other.md](features/other.md), [architecture.md](architecture.md) |

Cross-cutting docs with no single code path: [architecture.md](architecture.md), [decisions.md](decisions.md).
