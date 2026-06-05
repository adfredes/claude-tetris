# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Game

No build step required. Open `index.html` directly in a browser, or serve with:
```
python -m http.server 8000
```

No package manager, no dependencies, no linting/test tooling.

## Architecture

Single-page vanilla JS game with three files:

- **`index.html`** — two `<canvas>` elements (`#board` 300×600px, `#next-canvas` 120×120px), a stats sidebar (score/lines/level), and a control overlay for pause/game-over states.
- **`game.js`** — all game logic (~305 lines). Module-scope variables hold all state (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`). Key functions: `loop()` (rAF-based game loop with delta-time accumulator), `draw()` / `drawNext()` (rendering), `collide()` / `tryRotate()` (physics with wall-kick), `clearLines()` (scoring), `spawn()` / `lockPiece()` (piece lifecycle).
- **`style.css`** — dark arcade theme, flexbox layout, no dynamic classes.

## Key Mechanics

- Board: 10×20 grid, 30px blocks. Pieces are 2D color-ID matrices; rotation = transpose + reverse columns.
- Speed: `dropInterval = max(100, 1000 - (level-1) * 90)` ms. Level up every 10 lines.
- Scoring: `LINE_SCORES[n] * level` (100/300/500/800 for 1–4 lines). Soft drop +1/row, hard drop +2/cell.
- Ghost piece rendered at `ghostY()` with 20% alpha.
- Restart calls `init()` which reinitializes all state variables.

## CI/CD

`.github/workflows/claude.yml` — Claude Code interactive PR assistant.  
`.github/workflows/claude-code-review.yml` — automated Claude PR review on push.
