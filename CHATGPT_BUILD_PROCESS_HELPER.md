# ChatGPT Build Process Helper for ComCody

Use this helper at the start of each coding iteration. It is written for **GPT-5.5 Thinking** and for browser-based HTML/JS/CSS ZIP prototypes like ComCody.

## 0. Model and working mode

Model target: **GPT-5.5 Thinking**.

Working mode:
- Think first.
- Preserve the last known good ZIP as baseline.
- Make the smallest controlled change that satisfies the request.
- Never rebuild working features from scratch unless the user explicitly asks.
- Treat bugs as architecture/regression failures, not just visual symptoms.

## 1. Core product rule

ComCody is a composable game/code system.

Every reusable behavior must be exposed as one of:
1. a flow piece,
2. a template/container piece,
3. a child piece inside another piece,
4. a configurable piece setting that still maps to visible nested pieces.

Never bury reusable utility behavior as hidden one-off code.

## 2. Assembly-first development rule

Before adding a new primitive, check whether existing tested pieces can assemble the requested behavior.

Preferred order:
1. Reuse an existing primitive.
2. Compose existing primitives into a higher-level piece.
3. Add a new primitive only if the existing set cannot express the behavior.
4. Document why the new primitive is necessary.

Example:
- Do not create a separate `gravityStep` movement implementation.
- Build gravity as: `Face Down` + `Move`.

## 3. Baseline handling

For every new version:
1. Start from the last accepted ZIP, not an older local folder.
2. Extract into a fresh `work_vNN` directory.
3. Patch only the files needed.
4. Keep README, `MASTER_CODING_RULES.md`, `COMPOSABILITY_GUIDE.md`, schemas, and block packs in sync.
5. Re-zip from the clean working folder.

Never continue from an experimental fork unless the user explicitly accepted it.

## 4. UI/UX regression rules

For every UI patch, check:
- no duplicate settings wheels,
- no obsolete expert panels at the bottom,
- no floating controls detached from their block rows,
- no hidden focus-grabbing or auto-scroll during test runs,
- program stack uses maximum useful width,
- piece store does not steal permanent workspace,
- double-click/double-tap opens piece settings consistently,
- one compact remove button per visible block row,
- nested blocks visually behave like loops/templates at every depth.

## 5. Composability regression rules

For every block/piece patch, check:
- imported pieces and native pieces use the same rendering path,
- imported pieces and native pieces use the same settings path,
- imported pieces and native pieces use the same expert-code path,
- high-level pieces are represented by nested child pieces, not duplicate code,
- generated code descends recursively through nested pieces,
- execution descends recursively through nested pieces,
- settings open/close never reset the active game preset.

## 6. Tetris/Maze shared-kernel rules

Maze and Tetris must remain presets of the same kernel.

Shared concepts:
- grid runtime,
- active piece,
- movement,
- collision,
- template/loop execution,
- timer step,
- score event,
- input bindings,
- cell occupancy.

Tetris-specific behavior must use shared primitives where possible:
- gravity = `Face Down` + `Move`,
- swipe left = `Face Left` + `Move`,
- swipe right = `Face Right` + `Move`,
- swipe down = `Face Down` + `Move`,
- tap = `Rotate Shape`,
- line clear uses occupied cells, including manual walls and locked cells.

Manual grid items placed by the user count as real cells unless explicitly configured otherwise.

## 7. Code-edit workflow that worked best

Use simple deterministic tools first:

1. `unzip` the accepted ZIP into a fresh folder.
2. Inspect with `grep`, `sed`, and small Python scripts.
3. Patch the HTML/JS/CSS directly with Python string replacements or targeted scripts.
4. Avoid broad rewrites.
5. Run static checks.
6. Run targeted semantic checks.
7. Only then use browser screenshots if the change is visual/layout-sensitive.
8. Repackage with `zip`.
9. Verify ZIP contents.

This was more reliable than trying to drive the full UI repeatedly through browser automation for every small change.

## 8. Recommended checks before delivery

Minimum checks for each release:

```bash
# ZIP exists and opens
unzip -t comcody_lab_vNN.zip

# Expected files are present
unzip -l comcody_lab_vNN.zip | grep -E 'README|MASTER_CODING_RULES|COMPOSABILITY_GUIDE|index.html|schemas|block-packs'

# JS syntax extraction/check if the app is a single HTML file
node --check extracted_app.js

# Static regression checks
grep -n "Expert Pieces\|Generated Flow Code\|settings wheel duplicate marker" index.html

grep -n "dropToLoop" index.html
```

Targeted semantic tests should be written as small scripts or static assertions for the exact bug fixed, for example:
- manual walls + locked Tetris cell complete a row,
- clear rows sets `lastClearRows`,
- score event reads `lastClearRows`,
- blocked down input does not stop the test loop,
- nested generated code descends into child pieces.

## 9. Browser/visual testing rule

Use browser screenshots only when the change affects layout, touch UX, overlays, nested blocks, or visual grid colors.

Practical rule:
- Screenshot at tablet landscape: `1024x768`.
- Screenshot at tablet portrait: `768x1024`.
- Do not spend time on fragile end-to-end automation unless the bug is interaction-specific.
- If Playwright/Chromium times out, fall back to static HTML checks and targeted JS semantics, and state that limitation.

## 10. Delivery checklist

Before giving the ZIP back:

- The requested bug is fixed.
- The fix follows assembly-first rules.
- Existing accepted behavior is preserved.
- Native and imported pieces still share one path.
- README and guides are updated if rules hardened.
- ZIP integrity passes.
- Syntax checks pass.
- At least one targeted semantic check matches the actual bug.
- Final answer lists only meaningful changes and the download link.

## 11. Prompt to paste at the start of a new ChatGPT build session

```text
We are building ComCody, a composable Blockly-like game/code system. Use GPT-5.5 Thinking. Continue from the latest accepted ZIP only. Preserve existing behavior. Do not rebuild working features. Use assembly-first development: before adding any new primitive, check existing tested pieces and compose them. Reusable behavior must be visible as a flow piece, template piece, child piece, or explicit piece setting that maps to nested pieces. Never bury reusable utility code. Keep README, MASTER_CODING_RULES.md, COMPOSABILITY_GUIDE.md, schemas, and block packs in sync. For each patch, run ZIP integrity, syntax/static checks, and at least one targeted semantic check for the bug fixed. Use browser screenshots only for visual/layout regressions, especially tablet landscape and portrait. Deliver a clean ZIP and list only meaningful changes.
```


## v41 Double-Click Settings Fix

All Program Stack pieces, including top-level Loop, Tetris Tick Loop, and nested Gravity = Face Down + Move, must open through the same shared double-click settings editor. Loop editor code is a shared function, not a local helper buried inside one branch.
