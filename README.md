# Tiny Chess v5

A chess engine in 4 KB of Python for make.fun tiny-chess-01: alpha-beta
search to a fixed node budget (130,000 nodes/move, no clock), quiescence
over captures and replies to check, and a 0x88 board with make/unmake
(about 13,000 nodes/s). Evaluation covers material + piece-square
tables, pawn structure (doubled/isolated/passed), rook open files and
7th rank, king pawn-shield and endgame centralisation, and the bishop
pair. Search uses a working Zobrist transposition table, PVS with
null-window searches, null-move pruning, killer moves and a history
heuristic, with MVV-LVA capture ordering, plus late move reductions,
futility pruning and aspiration windows at the root.
`main.py` is the whole engine (4,013 bytes): an lzma payload that unpacks
the engine at run time, as the rules allow.

What changed from v4 (practice 1,099, floor 1,154): v4 completed depth 5
in the middlegame but needed about 269,000 nodes for depth 6 and never
reached it. v5 adds late move reductions (late quiet moves first searched
at depth-1, re-searched full depth if they beat alpha), futility pruning
at depth 1-2 (margins 120/220), and aspiration windows (+/-25, widen x4
on fail). The main search now completes depth 6 in the middlegame in
about 73,000 nodes (5.5 s), and depth 5 from the start position.
Honest note: in a 4-game local match against v4 (60 plies each, real
node budgets), v5 was behind on material in 3 of 4 games — a short
match proves nothing, but the measured gain is one extra ply of search,
not a match-play rout.

Move generation is verified by perft: startpos depth 3 = 8,902,
Kiwipete depth 2 = 2,039, and an en-passant/promotion position depth 3
= 2,812 — all exact. It finds mate in 1 in the test position in 0.01 s.
The worst middlegame move takes about 5.5 s, inside the 10 s move limit.

Strength is ungraded: it has not run `makefun grade-local` yet (needs
Docker), so the practice Elo floor of 1,154 is not confirmed. Depth 6
against the depth-4 house anchor (1,199 Elo) is suggestive, not proof.
