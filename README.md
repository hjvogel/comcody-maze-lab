# ComCody Lab v32 — Composable Tetris Fix

ComCody is a reusable game-kernel lab, not a set of separate mini-games.
Maze and Tetris Lite must share the same primitives: grid runtime, active piece, program stack, input, score, timer, settings, and expert-code inspection.

## What v32 fixes

- Imported and auto-scaffolded Tetris pieces now use the same clean settings system as native Maze pieces.
- One settings wheel per program piece.
- No bottom **Expert Pieces** or **Generated Flow Code** surface.
- Expert code opens only inside the selected piece settings panel.
- Tetris scaffold state is preserved when opening/closing piece settings.
- Built-in Tetris Lite pieces now include expert-code snippets.
- Imported YAML pieces can provide their own expert code using `source: |`.

## No Hidden Utility Rule

Every reusable behavior must be exposed as one of these:

```text
flow_piece      -> appears in the Program Stack
template_piece  -> configures a game preset
child_piece     -> can live inside another piece, such as Loop
```

A behavior is not ComCody-ready if it only exists as buried JavaScript.

## Current shared kernel modules

```text
grid_runtime
active_piece
timer_loop
program_stack
step_debugger
score_module
piece_store
input_module
piece_settings
inline_expert_code
```

## Example: Tetris Lite scaffold

Use **Game Setup → Auto Setup Tetris Lite**.

The lab reuses the current kernel and switches the configuration to:

```yaml
game_preset: tetris_lite_scaffold
grid_runtime:
  cols: 10
  rows: 20
active_piece:
  shape: T
score_module:
  reusable_by: [maze, tetris_lite]
```

The Program Stack is assembled from visible pieces:

```text
Tablet Input
Spawn Random Piece
Timer Step
Gravity Loop
Score Event
Checkpoint
```

## Example: imported piece with expert code

```yaml
version: 0.1
blocks:
  - type: hard_drop
    label: Hard Drop
    icon: ↧
    role: flow
    behavior: move
    class: movePiece
    defaults:
      steps: 20
    source: |
      function hardDrop(piece) {
        while (canMove(piece, 0, 1)) {
          piece.y += 1
        }
        lockPiece(piece)
      }
```

After import, this piece is available in the Piece Store. Its code is visible only from its own settings panel under **Expert Code**.

## Next correct refactor steps

1. Split `locked_cells` from maze walls so Tetris can lock pieces cleanly.
2. Add full row-clear behavior using the existing `score_module`.
3. Let imported block packs declare input bindings and templates in YAML.

The rule remains: **reuse first, compose second, duplicate never.**
