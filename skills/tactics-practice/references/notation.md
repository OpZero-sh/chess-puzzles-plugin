# FEN and move notation

The server gives positions as FEN and solutions as UCI moves. Learners usually type standard algebraic notation (SAN). This file covers reading the first two and converting from the third.

## Squares

Files are the letters a to h from White's left to right. Ranks are the numbers 1 to 8 from White's side to Black's. A square is file then rank: `e4`, `g8`.

## FEN

A FEN has six fields separated by spaces. Example: `6k1/2p1B3/5PP1/6K1/2r5/1p6/8/q7 w - - 0 50`

1. **Piece placement.** Eight ranks separated by `/`, starting with rank 8 and ending with rank 1. Within a rank, squares run from file a to file h. Letters are pieces: `K` king, `Q` queen, `R` rook, `B` bishop, `N` knight, `P` pawn. Uppercase is White, lowercase is Black. A digit is that many empty squares in a row.
2. **Side to move.** `w` or `b`. In a puzzle result this is always the learner's side.
3. **Castling rights.** Any of `K`, `Q` (White short, long) and `k`, `q` (Black short, long), or `-` for none. A right listed here does not mean castling is legal right now.
4. **En passant target.** The square a pawn just skipped over, or `-`.
5. **Halfmove clock.** Moves since the last capture or pawn move.
6. **Fullmove number.** Starts at 1 and goes up after each Black move.

In the example the first field reads: rank 8 has six empty squares, a black king on g8 and one empty square. Rank 7 has two empty squares, a black pawn on c7, one empty, a white bishop on e7, three empty.

## UCI moves

A UCI move is the from square followed by the to square, four characters, with no piece letter and no capture or check mark.

| Case | Form | Example |
| --- | --- | --- |
| Ordinary move or capture | from, to | `f5f3` (whatever stands on f5 goes to f3) |
| Promotion | from, to, then the new piece in lowercase | `f7f8q`, `a2a1q`, `g7g8n` |
| Castling | the king's two square move | `e1g1` and `e1c1` for White, `e8g8` and `e8c8` for Black |
| En passant | the capturing pawn's from and to squares | `e5d6` |

To know which piece a UCI move refers to, look up the from square in the current position.

## Converting what the learner types

1. Work out the current position: start from the FEN and apply every solution move already played, in order.
2. Turn the learner's move into from square, to square and promotion piece.
   - UCI input (`h3h7`) is already in that form.
   - SAN gives the piece and the to square: `Qxh7+` is a queen arriving on h7. Find the queen of the learner's colour that can legally reach h7. That is the from square. A pawn move such as `f7` or `exd5` has no piece letter. `O-O` is the king's short castle and `O-O-O` the long one.
   - Ignore `x`, `+`, `#`, `!` and `?` for matching. They carry no extra information about squares.
   - Words ("rook takes f3") are handled like SAN.
3. If two pieces of the same kind could reach the square and the learner did not say which, ask. Do not guess in favour of the solution.
4. Compare with the solution entry: from square, to square, and the promotion letter if there is one. A pawn reaching the last rank with no piece named: ask which piece, since an underpromotion is sometimes the point.

## Writing moves back to the learner

Give moves in SAN, since that is what most players read, and add the UCI form when the learner has been typing UCI. Build SAN from the position: piece letter (none for a pawn), `x` for a capture, the to square, `=Q` style for promotion, `+` for check and `#` for mate. Only add `+` or `#` when you have checked it on the board.

## Worked example

Position: `6k1/2p1B3/5PP1/6K1/2r5/1p6/8/q7 w - - 0 50`, solution `f6f7 g8h8 f7f8q`.

- The learner types `f7+`. A pawn move to f7: the white pawn on f6 can advance there, so the move is `f6f7`. It matches entry 1.
- Reply with entry 2, `g8h8`: the black king steps from g8 to h8, written `Kh8`.
- The learner types `f8=Q`. The pawn now on f7 promotes on f8 to a queen: `f7f8q`. It matches entry 3, the last one. The puzzle is solved, and the theme labels confirm it is mate.
