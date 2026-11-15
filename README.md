# Tiny Chess v4

A chess engine in 4 KB of Python for make.fun tiny-chess-01: alpha-beta
search to a fixed node budget (100,000 nodes/move, no clock), quiescence
over captures and replies to check, and a 0x88 board with make/unmake
(about 20,000+ nodes/s). Evaluation covers material + piece-square
tables, pawn structure (doubled/isolated/passed), rook open files and
7th rank, king pawn-shield and endgame centralisation, and the bishop
pair. Search uses a working Zobrist transposition table, PVS with
null-window searches, null-move pruning, killer moves and a history
heuristic, with MVV-LVA capture ordering.
`main.py` is the whole engine (3,812 bytes): an lzma payload that unpacks
the engine at run time, as the rules allow.

What changed from v3 (practice 997, floor 1,154): v3's transposition
table never hit (0 hits in 425 probes — a wrong king square in the
in-check test made the engine think it was in check at nearly every
node, so quiescence exploded). v4 fixes that bug, switches to a 0x88
board with make/unmake instead of copying the board per node, and adds
PVS plus a stronger evaluation. The main search now completes depth 5
in the middlegame (about 88,000 nodes, 3–4 s) against v3's depth 3,
and depth 4 from the start position. In a 2-game local match against
v3 (60 plies each, real node budgets), v4 did not lose either game and
was ahead by about 1.8 pawns in one of them.

Move generation is verified by perft: startpos depth 3 = 8,902,
Kiwipete depth 2 = 2,039, and an en-passant/promotion position depth 3
= 2,812 — all exact. It finds mate in 1 in the test position in 0.01 s.
The worst middlegame move takes about 5 s, inside the 10 s move limit.

Strength is ungraded: it has not run `makefun grade-local` yet (needs
Docker), so the practice Elo floor of 1,154 is not confirmed. Depth 5
against the depth-4 house anchor (1,199 Elo) is suggestive, not proof.
