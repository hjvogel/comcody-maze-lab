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


## v47 Pack Studio export UX

Full game export belongs in Pack Studio, not as a duplicate global header action. Export UI must show status, fallback download link, and log in the same place.

## v48 Creator story rule

Do not ship reusable behavior as a nameless or unexplained block.

For every new or exported piece, include:

- intention
- where it is used
- how to reuse or extend it
- tags for discovery
- visible dependencies or source

If a block cannot explain its purpose to a no-code creator, it is not ready for Pack Studio.


## v49 Clean Front UX Rule

No feature may add a wall of explanatory text to the first-use game screen. Use icon actions, concise labels, collapsed details, and double-click inspectors. Story metadata is required for reuse, but it must be discoverable on demand, not spammed at launch.


## v55 learning
Bundle Builder is the next platform step: export selected piece, piece + dependencies, or Program Stack template without adding front-screen text. Keep QA reports capped at two.

- v58 learning: Test runs must be sandboxed; creator-authored grid/setup data must be restored after Test, even when runtime modules clear or collapse rows.

## v65 rule — save compositions before coding primitives
If a user assembles working pieces in the Program Stack, ComCody must let that stack become a reusable block before any new primitive is added.

- Tablet arcade UX gate: no panel may stretch into empty space when content is short; cap scroll at the inner list, not the whole page.
