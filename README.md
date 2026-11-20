# Tiny Chess

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/cholil677/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/cholil677/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in 4 KB of Python, written from scratch for the
[make.fun Tiny Chess bounty](https://make.fun/bounty/tiny-chess-01).

`main.py` (3,914 bytes) is the whole engine: 0x88 board, negamax
alpha-beta + quiescence, transposition table, MVV-LVA + killer +
history ordering, null-move pruning, material + piece-square eval,
fixed node budget per move (no clock, no randomness) so replays are
deterministic. Python 3.13 stdlib only.

Practice Elo (6-game trials): trial-0 **1451**, trial-1 **1451**,
trial-2 **1279** (mean ~1394; sunfish ~1365, final floor 1154).
See `BUILD.md` for design and results. `src.py` is the readable
source `main.py` was packed from; not part of the 4 KB entry.
