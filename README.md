# Tiny Chess v3

A chess engine in 4 KB of Python for make.fun tiny-chess-01: alpha-beta
search to a fixed node budget (40,000 nodes/move, no clock), quiescence
over captures and replies to check, material + piece-square evaluation
with a rook-on-open-file bonus.
`main.py` is the whole engine (2,825 bytes): an lzma payload that unpacks
the engine at run time, as the rules allow.

What changed from v2 (practice 989, floor 1,154): v2 spent about 85% of
its nodes in quiescence, so its main search only reached depth 2. v3
trims quiescence to depth 4, adds null-move pruning (depth 3+, never in
check, never with king and pawns only) and killer-move ordering, caches
both king squares in the board state, and evaluates the board in a
single pass. Node throughput doubled (about 11,500 nodes/s vs 5,700),
so the main search now completes depth 3 in the middlegame (32,411
nodes, 3.25 s) and depth 4 in quieter positions, inside the same ~3.7 s
a move. In a 2-game local match against v2 (60 plies each, real node
budgets), v3 won one game by checkmate as Black and drew the other,
after v2's deeper quiescence had cost v3 material in an earlier
quiescence-depth-3 try — depth 4 quiescence kept the tactics while
keeping the extra main-search ply.

Move generation is verified by perft: startpos depth 3 = 8,902,
Kiwipete depth 2 = 2,039, and an en-passant/promotion position depth 3
= 2,812 — all exact. It finds mate in 1 in the test position. The
evaluation is bit-identical to v2 on ten test positions; only the
search changed.

Strength is ungraded: it has not run `makefun grade-local` yet (needs
Docker), so the practice Elo floor of 1,154 is not confirmed. The node
budget keeps a move around 3.5 s worst case in the middlegame, inside
the 10 s move limit.
