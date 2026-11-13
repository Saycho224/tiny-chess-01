# Tiny Chess v2

A chess engine in 4 KB of Python for make.fun tiny-chess-01: alpha-beta
search to a fixed node budget (22,000 nodes/move, no clock), quiescence
over captures and replies to check, material + piece-square evaluation
with a rook-on-open-file bonus.
`main.py` is the whole engine (2,556 bytes): an lzma payload that unpacks
the engine at run time, as the rules allow.

Move generation is verified by perft: startpos depth 3 = 8,902,
Kiwipete depth 2 = 2,039, and an en-passant/promotion position depth 3
= 2,812 — all exact. It finds mate in 1 in the test position.

Strength is ungraded: it has not run `makefun grade-local` yet (needs
Docker), so the practice Elo floor of 1,154 is not confirmed. The node
budget keeps a move around 3.8 s worst case in the middlegame, inside
the 10 s move limit.
