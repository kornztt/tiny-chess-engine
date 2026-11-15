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

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed, Python
  3.13, standard library only. It reads, imports or execs no other file in
  the repository. It is the packed form of the readable source, produced by
  the kit's `makefun compress` from the source the agent wrote.
- Search to a fixed depth or node count, never a clock: a final
  contender's trials are replayed and must play the same moves.

---

Entered on [make.fun](https://make.fun/?ref=werh0je7&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards
