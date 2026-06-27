# ComCody Master Coding Rules

These rules are mandatory for every next change.

## 1. Do not reinvent tested pieces

Before writing a new behavior, check the current primitive inventory.

Reuse in this order:

1. Existing primitive piece.
2. Template assembled from existing pieces.
3. New primitive only if the behavior is truly atomic and missing.

Forbidden examples:

- new `move_left` when `Face Left + Move` exists
- new `gravity_move` when `Face Down + Move` exists
- hidden input movement code when Tablet Input can run nested child chains

## 2. Every reusable behavior must be visible

A reusable behavior must exist as one of:

- flow piece
- template piece
- nested child piece
- settings-owned event binding made from child pieces

No reusable behavior may exist only as buried JavaScript.

## 3. Piece editing is direct

Do not add per-piece settings wheels in the Program Stack.

Required UI:

- double-click any piece to open settings
- keep one compact remove `×` on the right side of each block row
- show Expert Code only inside the settings panel
- no bottom expert section

## 4. Higher-level pieces are containers

Loops, templates, and event routers are not special code islands.

They are containers whose children define the behavior.

Container rules:

- No floating controls beside a nested body.
- Double-click the container to edit.
- Keep delete row-aligned as a compact `×`.
- Children may themselves be containers.
- Deeper nesting must keep working.

## 5. One tree, many views

The piece child tree is the source of truth for:

- visual representation
- settings panel
- expert code view
- generated flow code
- runtime execution
- import/export

Never create a second hidden representation that can drift out of sync.

## 6. Game semantics are shared

If two cell sources mean the same thing for a rule, use one semantic query.

For Tetris line clearing:

```text
occupied_for_line_clear = locked_cells OR manual_walls
```

Manual walls and locked Tetris cells must clear and collapse together.

## 7. Layout is product behavior

A feature is not complete if it steals essential working space.

Required UI:

- Program Stack must remain visible and wide enough for nested blocks.
- Piece Store must be hidden by default and opened as an overlay drawer.
- Controls must never float over nested bodies.
- Tablet landscape and portrait must both remain usable.

## 8. Theme values are state, not magic CSS

Grid colors must be configurable through shared grid settings.

Required shared colors:

- board background
- empty cell
- wall/manual block
- goal marker
- locked fallback

## 9. Quality gate before delivery

Before delivering a ZIP:

- JavaScript syntax check must pass.
- ZIP integrity check must pass.
- README must match the version.
- This file and `COMPOSABILITY_GUIDE.md` must be updated if rules changed.
- Imported block packs must follow the same UI/settings/expert path as native pieces.


## 10. Loop-count-one template rule

If a higher-level behavior is only an ordered child chain that runs once, do not invent a new container type.

Use the existing Loop container with `count = 1`.

Example:

```text
Gravity = Face Down + Move
  count = 1
  Face Down
  Move
```

Reason: this reuses existing visual nesting, settings editing, generated code, runtime execution, drag/drop, and expert code paths.

Forbidden: custom `composite` or hidden `gravity` execution when `Loop(count=1)` can express the behavior.


## 11. Real type reuse, not behavior imitation

When reusing an existing container, the piece must use the real existing type, not a renamed special type that only imitates behavior.

Correct:

```text
Gravity = Face Down + Move
  type: repeat
  behavior: repeat
  count: 1
```

Wrong:

```text
Gravity = Face Down + Move
  type: gravity_step
  behavior: repeat
```

Reason: settings, visual nesting, generated code, drag/drop, import/export, runtime, and tests must all use one shared object path and one shared implementation.


## v41 Double-Click Settings Fix

All Program Stack pieces, including top-level Loop, Tetris Tick Loop, and nested Gravity = Face Down + Move, must open through the same shared double-click settings editor. Loop editor code is a shared function, not a local helper buried inside one branch.
