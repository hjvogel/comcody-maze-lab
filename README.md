# ComCody Lab v47 — Clean Pack Studio Export

This version fixes the release-blocking export runtime failure where some browsers/environments did not provide a usable `URL.createObjectURL` path.

ComCody is moving from a single prototype game into a reusable no-code/code ecosystem.

```text
no-code piece
  ↕
piece.yaml manifest
  ↕
piece.js implementation
  ↕
tests.yaml smoke tests
  ↕
import/export ZIP
```


## What is fixed in v47

- Removed the redundant top-right **Export Game** button from the global header.
- Full game export now lives in **📦 Packs / Pack Studio**, where status, fallback download link, and export log are visible together.
- Kept v46 fallback behavior for browsers/environments where `URL.createObjectURL` is missing or throws.
- Updated `THINKINGRESULTS.md` with the `URL.createObjectURL is not a function` issue and the tested fallback rule.

## What is fixed in v46

- Export no longer depends only on `URL.createObjectURL`.
- Export first tries the normal object URL path.
- If object URL creation is unavailable or throws, export falls back to a base64 data URL.
- Pack Studio now has a visible **Export log**.
- Export logs every important phase: build ZIP, blob size, URL path, fallback path, automatic click, visible link.
- Export functions are async so fallback generation completes before success is reported.
- v45 import/export button routing remains intact.

## What Pack Studio supports

- **Import Pack** from the Piece Store, Game Setup, or Pack Studio.
- Import supports:
  - legacy block YAML
  - `piece.yaml + piece.js + tests.yaml` ZIP packs
  - full game bundles with `game.json`
- Every imported piece is registered into the existing Piece Store.
- Imported pieces use the same Program Stack, settings, Expert Code, generated code, and runtime paths as native pieces.
- Any Program Stack piece can be exported as a ZIP from Pack Studio.
- The full game can be exported as a ZIP with:
  - `game.yaml`
  - `game.json`
  - `game.js`
  - per-piece `piece.yaml`
  - per-piece `piece.js`
  - `tests.yaml`
  - `README.md`

## ComCody Pack v1 contract

A reusable piece ZIP should contain:

```text
piece.yaml
piece.js
tests.yaml
README.md
```

Minimal `piece.yaml`:

```yaml
version: 1.0
kind: piece
id: comcody.example.dash_two
type: dash_two
name: Dash 2
label: Dash 2
icon: ⚡
role: flow
behavior: move
class: movePiece
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

## Important rule

No-code UX does not mean fake code.

Every reusable block must have a visible no-code representation and real source under it. The source can be imported, inspected inside the piece settings panel, and exported again.

Read before changing code:

- `CHATGPT_BUILD_PROCESS_HELPER.md`
- `MASTER_CODING_RULES.md`
- `COMPOSABILITY_GUIDE.md`

## Quality gate used

- JavaScript syntax check passed.
- ZIP integrity check passed.
- Deterministic export harness passed for normal object URL export.
- Deterministic export harness passed for object URL throwing `URL.createObjecturl is not a function`.
- Deterministic export harness passed for missing object URL API.
- ZIP integrity check passed.


## v46 QA note

See `QA_REPORT_v46.md` for the exact export fallback tests and logging checks.
