# Tiny Chess — a 4 KB Python chess engine

[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/itserwe/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/itserwe/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in at most 4,096 bytes of Python that plays the house
gauntlet through the env at `MAKEFUN_ENV_URL`. `main.py` is the whole
engine; the readable source it was packed from is `engine.py` (packed
with the kit's `makefun compress`, LZMA).

## How it plays

- **Board:** 0x88, pieces as signed integers.
- **Search:** negamax alpha-beta with iterative deepening to a **fixed
  node budget** per move (never a clock), so a replay reproduces every
  move byte for byte.
- **Quiescence:** captures and promotions, with full move search while
  in check, so mates are not missed past the horizon.
- **Ordering:** transposition-table move first, then MVV-LVA captures,
  then killer moves.
- **Evaluation:** material plus piece-square tables (pawn, knight,
  bishop, rook, king), bishop pair via the tables, and a king
  centralisation term in the endgame. Scores are maintained
  incrementally as moves are made and unmade.
- **Determinism:** no wall clock, no unseeded randomness; the only
  randomness is the fixed-seed Zobrist table built at startup.

## The size rule

- `main.py` is the whole engine: at most 4,096 bytes as committed,
  Python 3.13, standard library only. It reads, imports or execs no
  other file in the repository.
- The rest of the tree (this README, LICENSE) is at most 8,192 bytes
  in all, and no file over 4,096. MIT licence.

## Grade it locally

You need Docker and the `makefun` CLI. Download the competition's dev
kit, unpack it, and from inside it run:

    makefun grade-local --kit . --bot <your repository> --driver docker

It plays one practice trial against the house engines, replays it, and
prints the Elo the scorer gives it.
