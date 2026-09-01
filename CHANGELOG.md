# Changelog

### v2.6.0 — Hook-driven Interface tracking

- **Replaced the per-frame Interface scan with RTVModLib hooks.** Button injection now runs off `interface-open-post` / `interface-close-post` instead of polling `get_tree().current_scene`, walking `Core/UI` looking for a node with a `containerGrid`, and re-checking panel visibility every frame. `lib._caller` hands us the Interface node directly, and `Open()` is vanilla's single entry point for inventory, container, and trader modes (`UIManager.ToggleInterface` / `OpenContainer` / `OpenTrader` all route through it), so one hook covers every case.
- **`_process` now does only what genuinely needs a frame tick**: the Ctrl+LMB drag-select input state machine, and lock-overlay position tracking — the latter now gated on the inventory actually being open rather than running whenever any lock exists.
- **Fixed a latent crash on scene change.** `_interface` is now dropped as soon as the node goes invalid. The old scene-name comparison caught this incidentally; with hooks there is no per-frame scan to notice, so a dangling `GetHoverGrid()` call would have hit a freed node.
- Falls back to the original polling path on loaders without the hook API, so this is not a hard Metro Mod Loader 3.x requirement.
- No user-facing change: sort, transfer, take/store all, drag-select, and item locking all behave identically.

### v2.5.2 — MCM dependency declaration

- **Declared Mod Configuration Menu as an optional dependency** (`[dependencies] optional=["doinkoink-mcm"]` in `mod.txt`). MCM is still detected at runtime via `MCM_Helpers.tres` and the mod runs fine without it, but the declaration makes the loader mount MCM before this mod's autoload reaches `_register_mcm()`, closing a load-order race that could leave the config page unregistered.
- No gameplay change.

### v2.5.1
- Removed the Stack Size Multiplier feature. Patching shared ItemData resources at runtime caused inconsistent behavior depending on load order and mod interactions, and the complexity wasn't worth it for a convenience feature. Existing MCM config entries are cleaned up automatically on load.

### v2.5.0
- **Fixed character instantly dying when sorting during a consume animation** — if you pressed the sort hotkey while mid-drink/mid-heal the base game was still holding a reference to the `Item` being consumed across an `await`. Our sort was freeing that node, so when the consume finished it applied garbage effect values and killed the player. All sort, transfer, take/store-all, and drag-select operations now bail out with an error click while `gameData.isOccupied` is true
- **Fixed locked-item highlight keeping the wrong shape after rotation** — the red lock overlay now resizes to match the item's new dimensions when you rotate it before moving, so you no longer have to unlock and re-lock to clear a stale highlight
- **Added: Stack Size Multiplier** (new MCM slider, 1–10) — makes single-use items (Consumables, Medical, Literature) stack up to N per cell. Weapons, ammo, attachments, and tools are untouched so base-game balance elsewhere stays intact. Takes effect on game start; changing the slider mid-session requires a restart to re-patch the item database
- Defensive gate: the sort hotkey now verifies `gameData.interface` is open before acting, as belt-and-braces against reports of sort firing outside the inventory UI (unable to reproduce in code review — if the issue persists, please re-report with reproduction steps)

### v2.4.0
- Simplified hotkey settings using MCM v2.4.0 native mouse button support on Keycode inputs
- Sort and Lock hotkeys are now single Keycode entries — bind any key or mouse button directly
- Removed 4 redundant dropdown settings (input type selectors, mouse button pickers)
- Requires MCM v2.4.0+

### v2.3.2
- Fixed locking one item incorrectly locking all items of the same type
- Locks now per-instance using item type + grid position as key
- Lock positions auto-update when items are manually dragged

### v2.3.1
- Fixed item locks not persisting across map transitions and hideout exits
- Locks now keyed by item type instead of runtime instance — survives scene changes and game restarts
- Locks saved to disk (`user://QuickStackSort_locks.cfg`)

### v2.3.0
- Added item locking — locked items are skipped by Sort, Transfer, Store All, Take All, and drag-select
- Lock toggle via middle-click (default) or configurable keyboard/mouse button in MCM
- Locked items show a red tint and border overlay

### v2.2.0
- Added 6 configurable sort modes via MCM: Alphabetical, Type, Weight, Value, Size, Rarity
- Default sort is now Alphabetical (groups identical items together)
- All modes use alphabetical name as tiebreaker

### v2.1.2
- Fixed null crash during drag-select when grid is destroyed mid-operation
- Added safety check after AutoStack to prevent operating on freed items
- Fixed MCM config crash when config keys are missing or corrupted
- Added bounds check on mouse button config index

### v2.1.1
- Fixed Ctrl+click triggering multi-select on single items — now requires a small drag before activating
- Single Ctrl+clicks pass through to the game normally

### v2.1.0
- Added Ctrl+LMB drag-select to transfer multiple items at once
- Fixed invisible item bug and game crashes from input conflicts

### v2.0.0
- Quick Stack: transfer matching items to nearby containers
- Transfer All: move all inventory items to a container
- Sort: auto-organize items by type and size
- Merge Stacks: combine partial stacks automatically
- MCM config support (hotkey, button visibility, sort order)

### v1.0.0
- Initial release
