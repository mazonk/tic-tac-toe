# Roadmap – Tic-tac-toe

**Status: DRAFT – awaiting approval**

Each milestone is one or more branches/PRs. A milestone is done when all acceptance criteria are met, tests pass, the code-reviewer approves and the user approves the merge.

## M0 – Setup
- [ ] `git init`, `.gitignore`, initial commit
- [ ] `package.json` with Vitest and `test` / `coverage` scripts
- [ ] GitHub repo created, remote added, `main` pushed

**Done when:** `npm test` runs (even with zero tests) and `main` is on GitHub.

## M1 – Game logic (`src/game.js`)
- [ ] Create empty 3×3 board
- [ ] Make a move; reject moves on occupied cells or after the game ends
- [ ] Alternate turns X → O
- [ ] Detect a win (3 rows, 3 columns, 2 diagonals) and return the winning line
- [ ] Detect a draw
- [ ] Reset the game

**Done when:** all of the above are covered by unit tests, and coverage of `game.js` is ≥ 90%.

## M2 – Playable UI
- [ ] `index.html` with a 3×3 grid
- [ ] Clicking a cell places X/O
- [ ] Status line: whose turn / winner / draw
- [ ] Restart button

**Done when:** a full game can be played in the browser, and UI tests (jsdom) cover click → state → render.

## M3 – Polish
- [ ] Highlight the winning line
- [ ] Score counter (X wins / O wins / draws) for the session
- [ ] Responsive layout (phone to desktop)
- [ ] Keyboard play and ARIA labels

**Done when:** it is playable with keyboard only, and it works at 360px width.

## M4 – AI opponent (optional)
- [ ] Mode toggle: 2 players / vs computer
- [ ] Easy AI (random valid move)
- [ ] Hard AI (minimax – never loses)

**Done when:** tests show the hard AI never loses from any starting position.

## M5 – Release
- [ ] README with how to play and run
- [ ] Deploy to GitHub Pages
- [ ] Tag `v1.0.0`

**Done when:** the game is live at the Pages URL.
