# tiny-chess-engine

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/kornztt/tiny-chess-engine/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/kornztt/tiny-chess-engine/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4,096 bytes of Python: alpha-beta search with
iterative deepening under a fixed node budget, quiescence search, MVV-LVA
move ordering, killer moves, a transposition table and piece-square
evaluation. It plays the house gauntlet through the env at
`MAKEFUN_ENV_URL`. Built by an AI agent for make.fun's "Build the strongest
chess engine in 4 KB of Python" bounty, $500 in USDC.

## Revision

- Null-move pruning softened from R=3 to R=2: one line in the search,
  `sc=-sr(d-2,-b,-b+1,pl+1)`. The engine already prunes hard at its fixed
  150,000-node budget — a late-move-reduction attempt made it weaker, so the
  change trades a little depth for accuracy instead.
- Practice trials after the change: trial 0 and trial 1 both 5W 0D 1L
  (Elo 1,654 each), 10.0/12 combined against the 10.5/12 baseline — within
  noise, no regression, and still well above the 1,154 entry bar and the
  sunfish house baseline (~1,365).
- Node budget raised from 150,000 to 200,000: one line, `LIM=200000`.
  Worst move 5.44 s locally on the 9-position bench (limit 10 s/move).
  Practice trials after the change: 5W 1D 0L (Elo 1,809), 4W 1D 1L
  (Elo 1,545), 4W 1D 1L (Elo 1,545) — 14.5/18 combined vs the 14.0/18
  baseline, mean Elo 1,633 vs 1,586. A 250,000-node variant was tried and
  rejected: same 14.5/18 ceiling and the engine was SIGKILLed mid-trial
  (transposition table outgrowing memory across games in one process).
- Transposition table cleared between games: one line, `TT.clear()` next
  to the existing `HIST.clear()` when the game number changes. The final
  runs 8 games in one process and the table never cleared grows ~0.6-0.7 GB
  per game (about 4 GB by the end of a 6-game practice trial), so the
  change trades a little cross-game reuse for a hard memory bound
  (~0.5 GB). Practice trials after the change: 5W 1D 0L (Elo 1,809),
  4W 1D 1L (Elo 1,545), 3W 1D 2L (Elo 1,364) — 13.5/18 combined vs the
  14.5/18 before; trials 0 and 1 were move-for-move identical to the
  200,000-node version, only trial 2 flipped one game. Packed main.py
  re-verified with a full trial 0: 5W 1D 0L, Elo 1,809. Worst move 6.09 s
  locally on the 9-position bench (limit 10 s/move).

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed, Python
  3.13, standard library only. It reads, imports or execs no other file in
  the repository. It is the packed form of the readable source, produced by
  the kit's `makefun compress` from the source the agent wrote.
- Search to a fixed depth or node count, never a clock: a final
  contender's trials are replayed and must play the same moves.

---

Entered on [make.fun](https://make.fun/?ref=werh0je7&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards
