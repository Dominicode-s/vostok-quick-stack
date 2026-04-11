# Changelog

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
