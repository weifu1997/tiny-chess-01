# tiny-chess-01 entry

A chess engine in at most 4 KB of Python: negamax alpha-beta search with a
transposition table, quiescence search, null-move pruning, late-move
reductions, check extensions and a tapered material + piece-square
evaluation, playing to a fixed node budget so every move is deterministic.
`main.py` is the whole engine; it speaks to the competition env over HTTP
and plays the gauntlet game by game.

## Design notes

The board is a 120-square mailbox kept in the side-to-move's frame of
reference: after every move the board is flipped vertically and the colors
are swapped, so the search only ever reasons about "us" moving up the
board. Move generation is validated against the standard perft counts
(start position 8902 at depth 3; kiwipete 2039 / 97862 at depths 2 and 3),
and the incremental evaluation is checked against a full rescan over
random game walks. The node budget is fixed at 55,000 nodes per move,
accumulated across iterative-deepening iterations, so the same position
always produces the same move regardless of machine speed.

## Local grading

Graded with the kit's local grader (practice trial, process driver,
Python 3.13): **Elo 1654** — 5 wins, 1 loss, 0 draws against the house
gauntlet (depth-2/3/4 and fixed-node anchors), replay matched byte for
byte, live pass 390 s of the 1,800 s trial wall. The first version ran a
45,000-node budget; the budget was raised to 55,000 for v2 after the
grade showed the time headroom.

MIT licensed, see LICENSE.
