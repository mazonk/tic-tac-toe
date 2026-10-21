# Handoff

**Last updated:** 2026-10-21, after M0 was finished.
**Next up:** M1 – Game logic (`src/game.js`)

## Start here (new chat)
1. Read [CLAUDE.md](../CLAUDE.md). Its rules are binding, especially the agent workflow and "never add a trailer to commits".
2. Read [roadmap.md](../roadmap.md). It is approved by the user; M0 is done and M1 is next.
3. Skim the end of [docs/reports/REPORTS.md](reports/REPORTS.md) for the latest agent reports.
4. Run `git switch main` and `git pull --ff-only`, then `npm install` and `npm test` (exits 0 with no tests).

## Current state
- Repo: https://github.com/mazonk/tic-tac-toe (public). The `gh` CLI is logged in as `mazonk`.
- `main` holds the setup docs, the 3 agents, `package.json` and `package-lock.json`. There is no game code yet.
- Only the `main` branch exists. All merged branches are deleted, and PRs #1–#3 are merged.
- Toolchain: Node 24.11.1, npm 11.6.2, vitest 5.0.5, @vitest/coverage-v8 5.0.5, jsdom 26.1.0. `npm audit` finds 0 vulnerabilities.
- `gh` 2.102.1 is installed at `%LOCALAPPDATA%\Programs\gh\bin` (portable zip, on the user PATH). winget is not available on this machine.

## Agents (`.claude/agents/`)
| Agent | Job |
|---|---|
| `test-agent` | Writes and runs Vitest tests in `tests/`. Never edits `src/`. |
| `code-reviewer` | Reviews the diff, including untracked files, and returns `VERDICT: APPROVE` or `VERDICT: CHANGES REQUESTED`. Doesn't edit code. |
| `github-agent` | Handles branches, commits, push, PRs, and branch cleanup after a merge. Merges, tags and Pages only after the user approves. |

Workflow for each roadmap item: main Claude writes the code → test-agent → code-reviewer → github-agent opens a PR → **the user merges on GitHub** → github-agent deletes the branch. Each agent appends its report to REPORTS.md, and main Claude relays every report to the user in chat.

## Decisions and lessons so far
- **No commit trailers**, ever. This is a user rule and is also saved in Claude's memory.
- The user merges PRs on GitHub, and squash merges are used. So always branch from an up-to-date `main` and target `main`; never stack PRs. PR #2 was stacked and merged into the wrong branch, so PR #3 had to redo it.
- jsdom is pinned to ^26 because jsdom 30 needs Node ≥24.15. Upgrading Node would allow moving to jsdom 30.
- `npm run coverage` exits 1 while there are no tests. This is expected until M1 adds them.
- Agents running in parallel must not both write to REPORTS.md. Have one of them return its report so it can be added later.

## Next step: M1 (game logic)
Before starting, check that the `chore/post-m0-docs` PR (which contains this handoff) is merged. Then pull `main`.
Create the branch `feat/m1-game-logic` from that up-to-date `main`. Build a pure `src/game.js` with no DOM code that covers:
- creating the board
- making moves, and rejecting moves on taken cells or after the game is over
- switching turns between X and O
- detecting a win and returning the winning line (3 rows, 3 columns, 2 diagonals)
- detecting a draw
- resetting the game

The test-agent covers this in `tests/game.test.js` with ≥90% coverage. Then the code-reviewer reviews it, the github-agent opens the PR, and the user merges.
