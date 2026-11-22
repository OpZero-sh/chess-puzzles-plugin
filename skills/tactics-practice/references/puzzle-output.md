# What a puzzle result contains

All three tools return the same layout. This is a real result from `chess_puzzle` with `theme: "mateIn2"` (the puzzle data is CC0):

```
Chess puzzle: mate in 2 (puzzle xtK8X, rating 1511). White to move and win. The opponent just played a2a1q.

8 . . . . . . k .
7 . . p . B . . .
6 . . . . . P P .
5 . . . . . . K .
4 . . r . . . . .
3 . p . . . . . .
2 . . . . . . . .
1 q . . . . . . .
  a b c d e f g h

FEN: 6k1/2p1B3/5PP1/6K1/2r5/1p6/8/q7 w - - 0 50
Themes: advanced pawn, deflection, endgame, mate, mate in 2, promotion, short
Solution in UCI, for checking answers only; do not reveal it unless the user asks: f6f7 g8h8 f7f8q
The solver plays the 1st, 3rd and 5th moves; the opponent replies with the others.
```

## Line by line

| Part | What it tells you | Show it at the start? |
| --- | --- | --- |
| Header | Source of the puzzle (theme drill, puzzle of the day, or a lookup by ID), the puzzle ID and the rating | Yes |
| "White to move and win" | The side the learner plays. The wording is the same for mating puzzles and for puzzles that win material | Yes |
| "The opponent just played ..." | The move that led to this position, in UCI. The FEN already includes it | Yes, it is often the mistake the tactic punishes |
| Text board | Uppercase letters are White pieces, lowercase are Black, a dot is an empty square | Only when the client does not draw the board card |
| `FEN` | The exact position, with the learner to move | On request, or in a text session |
| `Themes` | Plain language labels for the ideas in the solution | No. They give the answer away |
| `Solution in UCI` | The stored line, starting with the learner's first move | No. It is the answer key |
| Last line | Who plays which entry of the solution | No need |

## Things that are easy to get wrong

- **The board is drawn from the solver's side.** With White to move, rank 8 is at the top and files run a to h, left to right. With Black to move the board is flipped: rank 1 is at the top and files run h to a. Read the rank numbers and file letters printed on the board rather than assuming an orientation.
- **The solution starts with the learner's move.** The opponent's setup move has already been played and is reported in the header sentence. Do not ask the learner to "wait for the opponent" at the start.
- **Solution length.** Three entries means the learner plays two moves. Five entries means three. One entry is a single move.
- **Theme labels in results are words with spaces** ("mate in 2", "back rank mate"). The `theme` argument uses fixed code names (`mateIn2`, `backRankMate`). Never pass a label from a result as an argument without mapping it. See `themes.md`.
- **`chess_puzzle` is random.** The same arguments return a different puzzle on each call. `daily_chess_puzzle` and `get_chess_puzzle` are stable: the same day or the same ID returns the same puzzle, which makes them safe to call again to recover the answer key.
- **The card may show a playable board.** In clients that support it the learner can move pieces on the card. They may still type moves in chat, so always check typed moves against the solution.

## Messages when nothing is returned

| Message | Meaning | What to do |
| --- | --- | --- |
| "No puzzle for that theme and rating range yet. The library grows every few minutes; try a wider range or another theme." | The hosted library has no puzzle for that filter, or the minimum rating is above the maximum | Widen the range or change the theme, and say which you changed |
| "Puzzle <id> is not in this library yet. Ask for a puzzle by theme instead." | The ID is not in the hosted library. It may still be a valid Lichess puzzle | Say the ID is not available here and offer a themed puzzle |
