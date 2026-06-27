# ComCody Composability Guide

This guide is part of the source. It must be updated whenever the coding rules are hardened.

## 1. Assembly-first rule

Before creating a new block, check whether existing tested blocks can be assembled.

Allowed order:

1. Reuse an existing primitive.
2. Assemble existing primitives into a template block.
3. Add a new primitive only when the missing capability is truly atomic.

Forbidden:

- duplicating movement logic in a second block
- adding hidden JavaScript for behavior that could be represented by pieces
- creating Tetris-only move-left/move-right/gravity code when Move and Face Direction already exist

## 2. One representation model

A reusable behavior must be represented as one of these:

- **flow piece** — a normal draggable block in the Program Stack
- **template piece** — a higher-level block with a visible nested body
- **child piece** — a visible nested sub-piece inside a loop, template, or settings-owned event binding

No reusable utility may exist only as buried code.

## 3. Loop and template equivalence

A template must behave like a loop in structure:

```text
Template Block
  Child Piece
  Child Piece
```

The same nested child tree must drive:

- visual representation
- generated code
- runtime execution
- settings editing
- import/export definitions

## 4. Nested depth rule

Children may themselves be containers.

Valid shape:

```text
Template A
  Primitive
  Template B
    Primitive
    Loop C
      Primitive
```

All editing and rendering must use path-based tree access, not one-level loop-only helpers.

## 5. Piece editing rule

Every visible piece opens from the piece itself.

Rules:

- double-click any visible piece to open its settings
- do not add settings wheels to Program Stack pieces
- keep a compact remove `×` on the right side of each block row
- Expert Code opens inside the settings panel only
- no bottom expert section and no separate expert piece rail

This applies to top-level pieces, loop children, template children, and imported pieces.

## 6. Container control rule

Higher-level pieces such as loops, templates, and input/event routers are clean containers.

Rules:

- no floating controls beside the nested body
- double-click the container piece to open settings
- simple delete stays row-aligned as `×`
- children may themselves be containers
- controls must never visually overlap a child body

## 7. Event-router exception

Some blocks are not sequential flow. Example: `Tablet Input`.

Those blocks may keep their child chains inside the piece settings panel because gestures are event bindings, not top-to-bottom execution.

But the children are still real pieces:

```text
Swipe Left
  Face Left
  Move

Swipe Down
  Face Down
  Move
```

Runtime input events must be safe:

- they reuse the same child pieces
- they do not consume the main Run/Test step budget
- they do not crash the game when a Tetris side move is blocked
- they are queued so repeated taps/swipes do not corrupt the active tick loop

## 8. Piece Store layout rule

The Piece Store is an overlay drawer, not a permanent column that steals space from the Program Stack.

Rules:

- keep Program Stack visible and wide
- open the Piece Store with the `Pieces` button
- place the drawer over the grid area
- close or collapse the drawer after choosing a piece

## 9. Shared cell semantics

If a game rule says two cell sources mean the same thing, the runtime must use one shared semantic query.

For Tetris Lite row clearing:

```text
occupied_for_line_clear = locked_cells OR manual_walls
```

Therefore manual walls placed on the Tetris grid count as occupied Tetris cells for line clear. When a row clears, manual walls and locked cells above the row collapse together.

Scoring is not hidden inside the clear primitive. `Clear Rows` sets `lastClearRows`; the visible `Score Event` piece decides how many points to award.

## 10. Grid theme rule

Grid visuals are not hard-coded into individual game modes.

Shared configurable grid items:

- board background
- empty cell
- wall / manual block
- goal marker
- locked fallback cell

Maze and Tetris Lite must read the same grid theme state.

## 11. Current primitive inventory

Already tested and reusable:

- `Move`
- `Turn Left`
- `Turn Right`
- `Loop`
- `Timer Step`
- `Random Wall`
- `Score Event`
- `Face Direction` (`Face Up`, `Face Right`, `Face Down`, `Face Left`)
- `Rotate Shape`

Tetris may assemble from these before requesting new primitives.

## 12. Loop-count-one templates

A higher-level assembly that runs once must use the existing Loop model with `count = 1`.

Preferred shape:

```text
Gravity = Face Down + Move · 1×
  Face Down
  Move
```

This is not a separate composite runtime. It is a named loop/template using the same nested block tree as Loop.

## 13. Current valid Tetris assemblies

```text
Gravity Step · 1×
  Face Down
  Move
```

```text
Swipe Left
  Face Left
  Move
```

```text
Swipe Right
  Face Right
  Move
```

```text
Swipe Down
  Face Down
  Move
```

Only truly missing Tetris primitives remain new:

- `Spawn Random Piece`
- `Lock Piece`
- `Clear Rows`


## 13. Reuse must include the machine type

A reusable assembly is not fully composable if it only copies another piece's behavior while keeping a special machine type.

For named assemblies that are just a loop with children, the machine object must still be a Loop:

```yaml
label: Gravity = Face Down + Move
type: repeat
behavior: repeat
count: 1
children:
  - face_down
  - move
```

This guarantees the same editor opens, the same visual nesting is rendered, and the same runtime execution is used at every nesting depth.

Legacy imported aliases like `gravity_step` may be accepted, but they must be normalized to `type: repeat` before rendering or execution.


## v41 Double-Click Settings Fix

All Program Stack pieces, including top-level Loop, Tetris Tick Loop, and nested Gravity = Face Down + Move, must open through the same shared double-click settings editor. Loop editor code is a shared function, not a local helper buried inside one branch.
