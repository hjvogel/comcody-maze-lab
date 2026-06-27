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

## 3. No-code and code must stay connected

Every shareable piece must be exportable as a real pack:

```text
piece.yaml
piece.js
tests.yaml
README.md
```

The no-code block, settings panel, Expert Code, generated code, runtime behavior, import manifest, and export ZIP must describe the same piece.

## 4. Piece editing is direct

Do not add per-piece settings wheels in the Program Stack.

Required UI:

- double-click any piece to open settings
- keep one compact remove `×` on the right side of each block row
- show Expert Code only inside the settings panel
- no bottom expert section

## 5. Higher-level pieces are containers

Loops, templates, and event routers are not special code islands.

Container rules:

- No floating controls beside a nested body.
- Double-click the container to edit.
- Keep delete row-aligned as a compact `×`.
- Children may themselves be containers.
- Deeper nesting must keep working.

## 6. One tree, many views

The piece child tree is the source of truth for:

- visual representation
- settings panel
- Expert Code
- generated flow code
- runtime execution
- import/export

Never create a second hidden representation that can drift out of sync.

## 7. Import/export is product behavior

A ComCody pack is not a meme/demo ZIP. It must be usable by another user.

Required:

- validate `piece.yaml`
- register into Piece Store
- show imported `piece.js` in Expert Code
- export pieces with source and tests
- export full games with `game.json` restoration data
- keep imported pieces on the same UI/settings/runtime path as native pieces

## 8. Game semantics are shared

If two cell sources mean the same thing for a rule, use one semantic query.

For Tetris line clearing:

```text
occupied_for_line_clear = locked_cells OR manual_walls
```

Manual walls and locked Tetris cells must clear and collapse together.

## 9. Layout is product behavior

Required UI:

- Program Stack must remain visible and wide enough for nested blocks.
- Piece Store must be hidden by default and opened as an overlay drawer.
- Controls must never float over nested bodies.
- Tablet landscape and portrait must both remain usable.

## 10. Quality gate before delivery

Before delivering a ZIP:

- JavaScript syntax check must pass.
- ZIP integrity check must pass.
- A targeted semantic check must cover the requested behavior.
- README, this file, schemas, and `COMPOSABILITY_GUIDE.md` must be synchronized.


## 11. Button actions are product behavior

Any user-visible import/export button must be click-tested from a fresh app start.

Required:

- one shared action controller for duplicate buttons that do the same job
- no dead modal-only handlers
- visible success/failure status after click
- fallback download link after export
- singleton file input for imports so dynamic UI cannot lose handlers


## v45 QA note

See `THINKINGRESULTS.md` for the export regression analysis and the deterministic tool path used for this fix.
