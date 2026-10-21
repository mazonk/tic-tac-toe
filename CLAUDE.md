# CLAUDE.md

## Project
Browser tic-tac-toe in plain HTML/CSS/JavaScript (ES modules). Tests use Vitest on Node 24.
See [roadmap.md](roadmap.md) for scope and milestones.

## Layout (planned)
```
index.html          entry page
src/game.js         pure game logic (board, moves, turns, win/draw) – no DOM
src/ui.js           DOM rendering and event handling
src/style.css       styles
tests/*.test.js     Vitest tests
docs/reports/REPORTS.md   agent report log
.claude/agents/     project agents
```

## Commands
- `npm install` – install dev dependencies
- `npm test` – run Vitest once
- `npm run coverage` – run tests with coverage
- `npx serve .` (or open `index.html`) – play the game

## Agent workflow (required)
All work goes through the project agents. The main Claude coordinates and does not skip steps.

1. **Main Claude** plans and writes code for exactly one roadmap item.
2. **test-agent** writes/updates tests in `tests/`, runs them and reports pass/fail and coverage.
3. **code-reviewer** reviews the diff and gives a verdict: `APPROVE` or `CHANGES REQUESTED`.
   On changes requested, go back to step 1.
4. **github-agent** creates the branch, commits, pushes, opens the PR, and merges **only after the user approves**.
5. Every agent appends its report to `docs/reports/REPORTS.md`. The main Claude relays each report to the user in chat.

## Rules
- No work on a milestone until the user has approved `roadmap.md`.
- No direct commits to `main` after the initial setup commit. One roadmap item per branch: `feat/<item>`, `fix/<item>`, `chore/<item>`.
- Commit messages follow Conventional Commits (`feat:`, `fix:`, `test:`, `chore:`, `docs:`).
- **Never add a trailer to commits** – no `Co-Authored-By` or any other attribution lines.
- PR descriptions end with: `🤖 Generated with [Claude Code](https://claude.com/claude-code)`
- Never force-push. Never rewrite published history.
- Stop and ask the user at each approval gate (roadmap approval, PR merge).
- Keep `src/game.js` free of DOM code so it stays unit-testable.
