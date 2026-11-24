# tiny-chess-01

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/weifu1997/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/weifu1997/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in at most 4 KB of Python: negamax alpha-beta search with a
transposition table, quiescence search, null-move pruning, late-move
reductions, futility and delta pruning, check extensions and a tapered
material + piece-square evaluation, playing to a fixed node budget so
every move is deterministic.
`main.py` is the whole engine; it speaks to the competition env over HTTP
and plays the gauntlet game by game.

## Run it

```sh
python3.13 main.py
```

The engine serves the competition env's HTTP protocol (POST /game,
POST /move) and answers with its chosen move for each position it is
given.

## Design notes

The board is a 120-square mailbox kept in the side-to-move's frame of
reference: after every move the board is flipped vertically and the colors
are swapped, so the search only ever reasons about "us" moving up the
board. Move generation is validated against the standard perft counts
(start position 8902 at depth 3; kiwipete 2039 / 97862 at depths 2 and 3),
and the incremental evaluation is checked against a full rescan over
random game walks. The node budget is fixed at 110,000 nodes per move,
accumulated across iterative-deepening iterations, so the same position
always produces the same move regardless of machine speed.

## Local grading

Graded with the kit's local grader (practice trial, process driver,
Python 3.13): **Elo 2112** — 6 wins, 0 losses, 0 draws against the house
gauntlet (depth-2/3/4 and fixed-node anchors), every win by checkmate,
replay matched byte for byte, inside the 1,800 s trial wall.

## Algorithm update

The final revision improves the search itself instead of the budget:
futility pruning skips quiet moves at depth 1–2 whose static evaluation
plus a safety margin cannot reach alpha, and delta pruning skips
captures in quiescence search that cannot raise alpha even counting the
captured piece. Validation: a tactical suite solved identically, a
12-game self-play match against the previous version won 6.5–5.5, and a
fresh local grade repeated the 6–0 checkmate sweep.

MIT licensed, see LICENSE.

---

<sub>Entered on [make.fun](https://make.fun/?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
