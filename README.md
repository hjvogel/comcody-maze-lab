# ComCody Lab v51 — Piece Inspector MVP

ComCody keeps the arcade-clean surface, but every block can now explain and prove itself on demand.

## What is new

- Double-click any Program Stack piece to open the **Piece Inspector**.
- Inspector includes compact sections for:
  - settings
  - composition tree
  - story / reuse / health
  - source YAML + JS
  - tests
- Added **Test Piece** inside the inspector.
- Added **Export Piece ZIP** inside the inspector.
- No new front-screen text or gameplay clutter.

## Creator flow

```text
Find piece → inspect → test → edit → compose → export/share
```

## Quality gate used for v51

- ZIP integrity check
- JavaScript syntax check
- static checks for Piece Inspector, Test Piece, Export Piece ZIP, and hidden/collapsed source/story sections
- no new front-screen documentation spam

## Creator levels

### 1. Gamer / no-code builder

A gamer can:

```text
open Piece Store
search by purpose
add blocks
double-click blocks to understand them
play/test
export the full game ZIP
share it
```

No JavaScript is required.

### 2. Advanced no-code composer

A builder can assemble existing pieces into a higher-level block:

```text
Gravity = Face Down + Move
  Face Down
  Move
```

The higher-level block still has a story, settings, children, source view, tests, and export path.

### 3. Coder

A coder can inspect and share the real implementation:

```text
piece.yaml
piece.js
tests.yaml
README.md
```

No-code block and code source must describe the same behavior.

## ComCody Pack v1.2 contract

A reusable piece ZIP should contain:

```text
piece.yaml
piece.js
tests.yaml
README.md
```

Minimal `piece.yaml` with story metadata:

```yaml
version: 1.2
kind: piece
id: comcody.example.dash_two
type: dash_two
name: Dash 2
label: Dash 2
icon: ⚡
role: flow
behavior: move
class: movePiece
story:
  intent: Move two cells quickly.
  used: Fast movement puzzles and speed challenges.
  how: Reuse wherever a stronger Move block is needed.
  tags: movement, dash, maze
defaults:
  steps: 2
source:
  file: piece.js
```

Minimal `piece.js`:

```js
export async function run(ctx, piece) {
  await ctx.move(piece.steps ?? 1)
}
```

## Full game bundle

A full game ZIP includes:

```text
comcody-pack.yaml
bundle-story.md
game.yaml
game.json
game.js
tests.yaml
README.md
pieces/*/piece.yaml
pieces/*/piece.js
pieces/*/tests.yaml
pieces/*/README.md
```

The bundle story helps other creators decide:

- what the game is for
- who should use it
- what can be reused
- what should be extended next

## Important rule

No-code UX does not mean fake code.

Every reusable block must have:

```text
visible block
+ story/intention
+ YAML manifest
+ optional child composition
+ real JS source or generated source
+ tests
+ export path
```

## Read before changing code

- `CHATGPT_BUILD_PROCESS_HELPER.md`
- `MASTER_CODING_RULES.md`
- `COMPOSABILITY_GUIDE.md`
- `THINKINGRESULTS.md`

## Quality gate used for v49

- JavaScript syntax check passed.
- ZIP integrity check passed.
- Static checks confirmed:
  - Pack Studio has Bundle Story fields.
  - Piece settings include Story / Intent / Reuse.
  - Piece Store has story search.
  - Full game export includes `bundle-story.md`.
  - Piece export includes story metadata in `piece.yaml` and `README.md`.
- Browser smoke check confirmed:
  - Pack Studio opens.
  - Piece Store search input appears.
  - A Program Stack piece opens with Story / Intent / Reuse section.


## v49 Arcade Clean UI

Creator stories remain part of every piece and bundle, but they are no longer pushed onto the first screen. The rule is now:

- first screen = build, play, share
- details = reveal on demand
- story, source, tests, and logs = hidden behind inspector/details
- no explanatory wall of text in front of no-code creators

Pack Studio now opens as a compact action panel with Import, Export Game, and Export Piece. Bundle story, technical contents, and export logs are collapsed until needed.


## v54

Small UX fix: saving a piece from the inspector now closes the popup, matching Remove behavior.


## v56 — Bundle Builder

Pack Studio now includes a hidden-on-demand Bundle Builder for creators:

- Export selected piece
- Export selected piece + dependencies
- Export current Program Stack as a reusable template
- Run a bundle readiness check before sharing

The front UX stays arcade-clean. Technical files remain available in exports and Expert panels, not sprayed across the play screen.


## v56 Fix

Piece inspector editing is now state-safe. Opening Tablet Input and closing it without saving restores the exact game state. Save/Remove commit intentionally.

## v57 Line Clear Rule

Full-row clearing now uses one shared occupancy rule: manual wall/paint cells, temporary cells, and locked falling-piece cells all count as occupied. A full line clears and scores no matter which piece type filled it.


## v59 — Test Sandbox Persistence

Test runs now snapshot the creator game before execution and restore it afterward. Line clears still work during Test, but maker-authored grid setup such as manual walls is not destroyed by a test run.


## v59 Builder / Play Sandbox

Run and Test now mutate only the Play grid copy. The Builder/Maze setup is persistent source-of-truth; Reset copies Builder → Play before another test.


## v60 — Play Sandbox Render Isolation

Run/Test now mutate and render only the Play sandbox. The Builder/Maze grid is the persistent source-of-truth and no longer animates or clears while a test is running. Reset copies Builder → Play.
