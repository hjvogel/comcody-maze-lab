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

## 3. Pack-first sharing model

Every shareable piece must round-trip through ComCody Pack v1:

```text
piece.yaml
piece.js
tests.yaml
README.md
```

The manifest is for no-code users. The JavaScript file is for coders. Both must describe the same behavior.

## 4. Loop and template equivalence

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

## 5. Nested depth rule

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

## 6. Piece editing rule

Every visible piece opens from the piece itself.

Rules:

- double-click any visible piece to open its settings
- do not add settings wheels to Program Stack pieces
- keep a compact remove `×` on the right side of each block row
- Expert Code opens inside the settings panel only
- no bottom expert section and no separate expert piece rail

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

## 8. Import/export rule

Imported pieces are not second-class objects.

After import, a piece must:

- appear in the normal Piece Store
- open the normal settings panel
- show `piece.js` under Expert Code
- execute through an existing behavior or explicit `run(ctx, piece)` source hook
- export again as a normal ComCody piece pack

## 9. Full game bundle rule

A full game export must contain enough data to restore the editable no-code state:

```text
game.json
game.yaml
game.js
pieces/*/piece.yaml
pieces/*/piece.js
tests.yaml
README.md
```


## Pack action controller

Import/export is part of composability. It must not be wired separately for each button.

Required pattern:

```text
one visible action
  -> one shared controller
  -> one import/export implementation
  -> visible status
  -> same piece registry/runtime/settings path
```

This prevents Pack Studio, Game Setup, and Piece Store from drifting into different behaviors.


## v45 QA note

See `THINKINGRESULTS.md` for the export regression analysis and the deterministic tool path used for this fix.


## v47 Pack Studio export UX

Full game export belongs in Pack Studio, not as a duplicate global header action. Export UI must show status, fallback download link, and log in the same place.

## v48 Story and discovery rule

A shareable block is not complete until it tells creators why it exists.

Every reusable piece should answer:

```text
What is the intention?
Where is it already used?
How can I reuse or extend it?
What tags help me find it later?
What child pieces or source does it depend on?
```

This story must travel with import/export:

```text
piece.yaml -> story block
README.md -> human explanation
Piece Store -> searchable summary
Piece Settings -> editable Story / Intent / Reuse panel
```

Bundles follow the same rule. A game ZIP must include `bundle-story.md` so another creator can decide what to reuse before reading the code.


## v49 Clean Front UX Rule

No feature may add a wall of explanatory text to the first-use game screen. Use icon actions, concise labels, collapsed details, and double-click inspectors. Story metadata is required for reuse, but it must be discoverable on demand, not spammed at launch.


## v55 learning
Bundle Builder is the next platform step: export selected piece, piece + dependencies, or Program Stack template without adding front-screen text. Keep QA reports capped at two.

- v58 learning: Test runs must be sandboxed; creator-authored grid/setup data must be restored after Test, even when runtime modules clear or collapse rows.

## v65 — No-code piece creation
A saved Program Stack becomes a normal piece:

- behavior: `repeat`
- count: `1`
- children: visible existing pieces
- story/tags/license: attached metadata
- source: generated wrapper calling `ctx.runChildren(piece)`

This keeps reuse honest: compose first, code only when needed.
