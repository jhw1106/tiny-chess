[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/jhw1106/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/jhw1106/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess

The strongest chess engine in 4 KB of Python — a single-file, stdlib-only
engine for the [tiny-chess-01](https://make.fun/bounty/tiny-chess-01) bounty.

## Design

- **Board**: 0x88 mailbox, absolute colours (positive = white), make/unmake
  with full state restore (castling rights, en passant, halfmove, Zobrist hash).
- **Search**: negamax alpha-beta with iterative deepening under a **fixed
  node budget** (70,000 nodes/move — no wall clock anywhere), a transposition
  table with mate-score ply adjustment, late move reduction (LMR) for late
  quiet moves, MVV-LVA capture ordering plus two killer moves per ply, and
  quiescence over good captures/promotions (and all evasions in check) with
  delta pruning.
- **Evaluation**: material + tapered piece-square tables (middlegame blended
  to endgame by remaining non-pawn material), pawn structure (passed pawns by
  rank, doubled and isolated penalties), bishop pair, rook on open/semi-open
  files, king pawn shield, and a small tempo bonus.
- **Protocol**: talks to the trial environment over HTTP — `POST /game`,
  `POST /move`, `POST /resign` — with UCI moves, one env request at a time,
  fully deterministic (same position + history + trial seed gives the same move).

## Files

- `main.py` — the compressed entrypoint (≤ 4,096 bytes), produced by the
  kit's own `compress` command from the readable source.
- `engine.py` — the readable source the compressed entrypoint was built from.

## Local results (practice trials, kit env + scorer)

- Practice trial-0: Elo **1191** (2 wins, 1 draw, 3 losses)
- Practice trial-1 and trial-2: see the submission report.

The engine clears the 1,154 Elo practice floor. It plays a solid
positional game with a real search; its ceiling is the hand-crafted
evaluation, not the search.

## Licence

MIT — see `LICENSE`.
