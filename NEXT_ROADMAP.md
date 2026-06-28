# ComCody Next Roadmap — Creator Platform Path

The next work should move ComCody from “playable prototype” to “creator platform” while keeping the arcade/no-code surface clean.

## Product promise

```text
Build -> Play -> Inspect -> Test -> Bundle -> Share -> Reuse
```

A gamer should be able to build and share without reading technical text. A coder should be able to open the same block and find real YAML, JavaScript, tests, and dependency metadata.

## v51 — Piece Inspector MVP ✅ shipped

Purpose: every block can tell its story when opened, not on the front screen.

Deliver:

- double-click any piece opens the inspector/settings panel,
- compact health badge on top,
- collapsed sections:
  - Story,
  - Children / Composition,
  - Source,
  - Tests,
  - Export,
- Run Piece Test button,
- Export Piece ZIP button,
- missing dependency warnings.

Quality gate:

- no front-screen text wall,
- all details collapsed by default,
- simple Loop, Tetris Tick Loop, Gravity loop, imported pieces all use the same inspector path.

## v52 — Bundle Builder

Purpose: creators choose exactly what they share.

Deliver export modes:

- selected piece,
- selected piece + dependencies,
- current Program Stack as template,
- full game,
- full game + custom/imported pieces.

Quality gate:

- preview ZIP contents before export,
- visible fallback link remains,
- export logs collapsed but available,
- imports validate dependencies before registration.

## v53 — Piece Finder / Reuse Map

Purpose: help creators find the best existing block before making a new one.

Deliver:

- search by intent/story/tag,
- filters for core/imported/template/game-ready,
- “used by” and “depends on” view,
- similar/recommended pieces,
- extension suggestions.

Quality gate:

- search results are compact visual tiles,
- no paragraph spam,
- one tap opens inspector.

## v54 — Pack Test Runner

Purpose: imported packs should be trusted before use.

Deliver:

- parse `tests.yaml`,
- run piece tests against the shared kernel,
- compact pass/fail view,
- block or warn on broken packs,
- export QA report with bundles.

Quality gate:

- tests can run without external services,
- imported broken pack produces clear visible warning,
- no silent registration of invalid behavior.

## v56 — Template Gallery

Purpose: fast starts for gamers and builders.

Deliver:

- Blank Game,
- Maze,
- Tetris Lite,
- imported templates,
- compact cards with one-line intent,
- apply template without losing ability to inspect/edit nested pieces.

Quality gate:

- no separate games/pages,
- templates are pack-defined and inspectable,
- applying a template preserves pack/source/story/test metadata.

## Never break these rules

- No hidden reusable behavior.
- No duplicated primitive if composition can express it.
- No long documentation wall on first screen.
- No export without visible fallback link and log.
- No imported piece on a separate path from native pieces.


## v51 shipped

Piece Inspector MVP is now available behind double-click. Next step is v52 Bundle Builder for selected piece + dependencies and current Program Stack templates.


## v56 learning
Bundle Builder is the next platform step: export selected piece, piece + dependencies, or Program Stack template without adding front-screen text. Keep QA reports capped at two.

- v58 learning: Test runs must be sandboxed; creator-authored grid/setup data must be restored after Test, even when runtime modules clear or collapse rows.


## v59 note
Builder grid is source-of-truth. Play grid is a disposable runtime sandbox. Reset copies Builder → Play.
