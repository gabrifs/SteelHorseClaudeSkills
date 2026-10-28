# Unity conventions and pitfalls

General Unity gotchas that cause hard-to-trace bugs. Read the relevant section before writing that kind of code.

## Lifecycle and initialization order

- `Awake`/`OnEnable`/`Start` order **between different components is not guaranteed** unless you set it. Do not make one component's `Awake` depend on another's.
- Use `[DefaultExecutionOrder(n)]` when one component must run before others, or give the dependency an explicit `Setup()`/`Initialize()` call and have a single owner invoke it in a fixed order.
- For services shared across scenes, prefer explicit initialization over `Awake`/`Start`. It is deterministic and removes a whole class of race conditions.
- If something may not exist yet at `OnEnable`, poll or subscribe for it; do not assume it is ready.

## Singletons and duplicates

- A `DontDestroyOnLoad` object placed in every scene creates a duplicate on every scene load. `Destroy()` is **deferred to end of frame**, so the duplicate's `Awake`/`OnEnable` still run first.
- Any side effect in those methods that touches shared or global state (an `AudioMixer` parameter, a static, a `PlayerPrefs` key) will hit the real, persistent instance. Keep `Awake` free of global side effects on objects that may be duplicates, or initialize explicitly from the surviving instance only.
- Detect the duplicate and bail out before doing any work.

## ScriptableObjects

- ScriptableObject assets are shared references. Mutating one at runtime changes it for everyone, and in the Editor the change persists after Play mode. Copy (`Instantiate`) or keep runtime state in a separate class.
- Reference other assets by serialized reference when they live in the project. Use a stable key or ID only for data that must survive renames or be saved to disk.
- Creating asset instances and registering them on lists or databases is an Editor task. Give the user the steps.

## Serialization

- Renaming a serialized field loses its value unless you add `[FormerlySerializedAs("oldName")]`.
- Only fields Unity can serialize show in the Inspector: public or `[SerializeField]`, non-static, non-readonly, and a supported type. Prefer `[SerializeField] private` over `public`.
- Interfaces and abstract classes do not serialize on their own. Use `[SerializeReference]` or a concrete base type.
- Changing a serialized field's type or removing it can silently drop data in existing assets and scenes. Mention this to the user when doing it.

## Events and references

- Unsubscribe in `OnDisable`/`OnDestroy` what you subscribed to in `OnEnable`/`Awake`. Leaked subscriptions keep destroyed objects alive.
- Unity overloads `==` for destroyed objects. A destroyed `UnityEngine.Object` is `== null` but not `is null`; do not use `?.` or `??` on them.
- Prefer code-wired events for stable references over per-button Inspector `UnityEvent`s when several scripts must stay in sync; they are easier to review in a diff.

## Performance habits

- Cache `GetComponent` results; avoid calling it, `Find*`, or `Camera.main`-style lookups every frame.
- Avoid per-frame allocations (LINQ, string concatenation, `new` collections) in `Update`.
- Use `CompareTag` instead of `tag ==`.

## Version control

- Commit `.meta` files alongside their assets; never commit one without the other.
- Do not commit `Library/`, `Temp/`, `Logs/`, `Obj/` or user-specific settings.
- Scene and prefab merges are painful. Keep changes to them small and warn the user before work that touches the same scene as someone else.
