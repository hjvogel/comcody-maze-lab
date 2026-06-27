# ComCody Lab v40 — Gravity Is a Real Loop Block

This version fixes the remaining Gravity mistake strictly by the build helper workflow.

The issue was not only runtime behavior. The visible piece still carried the old machine type `gravity_step`, so it could look like a loop but fail the normal loop/settings path in some imported/autosetup trees.

## What changed in v40

### 1. Gravity now has the real Loop type

Correct model:

```text
Gravity = Face Down + Move · 1×
  Face Down
  Move
```

Internally this is now the normal `repeat` piece:

```text
type: repeat
behavior: repeat
count: 1
children:
  - Face Down
  - Move
```

It is not `type: gravity_step` with repeat-like behavior.

### 2. One settings path for every Program Stack piece

Top-level pieces and nested pieces now both open through the same object-path editor:

```text
open piece by tree path -> edit actual piece object
```

That removes the split between `openSettings(type, index)` and nested piece settings.

### 3. Legacy self-heal remains

Old imported packs or older saved program trees may still contain:

```text
type: gravity_step
behavior: gravity
```

Those are normalized into the real Loop piece before rendering, code generation, and runtime use.

### 4. Gravity is not added as a separate Store primitive

The Tetris scaffold may create a named gravity loop, but the Piece Store does not expose Gravity as a separate primitive. Users can always create the same thing from the existing Loop block.

Read before changing code:

- `CHATGPT_BUILD_PROCESS_HELPER.md`
- `MASTER_CODING_RULES.md`
- `COMPOSABILITY_GUIDE.md`

## Quality gate used

- JavaScript syntax check passed.
- ZIP integrity check passed.
- Static semantic checks confirm:
  - scaffold uses `makeGravityLoop()`
  - `make('gravity_step')` aliases to a real Loop
  - old gravity pieces are normalized
  - Program Stack double-click edits by tree path
  - Gravity is skipped as separate Store primitive


## v41 Double-Click Settings Fix

All Program Stack pieces, including top-level Loop, Tetris Tick Loop, and nested Gravity = Face Down + Move, must open through the same shared double-click settings editor. Loop editor code is a shared function, not a local helper buried inside one branch.
