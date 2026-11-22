---
name: tactics-practice
description: "Run a chess tactics drill on a chosen theme and rating, one position at a time, with move checking and graduated hints. Use when someone says give me a chess puzzle, a mate in two around 1500, quiz me on forks or pins, I want to practice tactics, train my endgames, or when they play a move or ask for a hint on a puzzle already on the board."
---

# Tactics practice

Serve rated puzzles from the Chess Puzzles server, let the learner find the moves, check each move against the stored line, and explain the idea only after the solve.

## When to use

- A themed or rated drill: "a mate in two around 1500", "five fork puzzles", "something easy".
- A move, a hint request or "I give up" on a puzzle that is already in the conversation.

Use `daily-puzzle` for today's shared puzzle and `review-a-puzzle` when the learner brings a puzzle ID or wants a finished puzzle explained. This skill does not analyse the learner's own games or arbitrary positions: the server has no engine and returns only library puzzles.

## Tools

All three are read only and need no sign in.

| Tool | Use | Arguments that matter |
| --- | --- | --- |
| `chess_puzzle` | A random puzzle that fits a filter | `theme` (default `mix`), `min_rating`, `max_rating` (integers, 400 to 3200, both optional) |
| `daily_chess_puzzle` | The puzzle of the day, same for everyone during a UTC day | none |
| `get_chess_puzzle` | One puzzle by its ID | `puzzle_id` |

`theme` accepts exactly these values: `mix`, `mateIn1`, `mateIn2`, `mateIn3`, `fork`, `pin`, `skewer`, `sacrifice`, `discoveredAttack`, `backRankMate`, `hangingPiece`, `promotion`, `opening`, `middlegame`, `endgame`, `short`, `long`. See [references/themes.md](references/themes.md) for what each means and for labels that appear in results but cannot be used as a filter.

## Workflow

1. **Pick the filter.** Map the request to one `theme` value. "Around 1500" becomes `min_rating: 1400`, `max_rating: 1600`. "Easy" or "hard" with no number: ask for a rough level once, or use the starting bands in [references/data-source.md](references/data-source.md). With no level at all, call without rating bounds and adjust after the first puzzle. Make sure the minimum is not above the maximum before calling.
2. **Call `chess_puzzle`.** Each call returns a different random puzzle for the same filter, so one call is one puzzle. Do not call again to "refresh" a puzzle in progress.
3. **Present the position, nothing more.** Give the puzzle ID, rating, side to move and the opponent's last move. When the client shows the playable board card, do not reprint the board. In a text session, show the text board in a code block exactly as returned and offer the FEN. Leave out the `Themes` line and the `Solution` line. The layout of the result is described in [references/puzzle-output.md](references/puzzle-output.md).
4. **Wait for the learner's move.** Never play the first move for them.
5. **Check the move.** The solution is a list of UCI moves. The learner plays the 1st, 3rd and 5th entries and the opponent plays the others. Convert what the learner typed (UCI such as `f6f7`, standard notation such as `Rxf3` or `O-O`, or words) into from square, to square and promotion piece, then compare with the entry at the current index. Notation rules are in [references/notation.md](references/notation.md).
   - **Match, more moves left:** confirm it, play the opponent's reply from the next entry, state the reply in standard notation, and wait again.
   - **Match on the last entry:** the puzzle is solved. Go to step 7.
   - **No match:** say it is not the puzzle's move and let them try again. Do not reveal the stored move. One exception: on the final move, a different move that gives immediate checkmate also solves the puzzle. Accept it only when you can confirm the mate square by square.
   - **Unclear move** (two pieces could make it, or the square is missing): ask for the from and to squares.
6. **Give hints only when asked,** one level per request, following [references/hint-ladder.md](references/hint-ladder.md).
7. **After the solve or a clear "show me":** give the whole line in standard notation, name the idea using the returned theme labels, and say in one or two sentences why the opponent could not avoid it. Repeat the puzzle ID so they can reopen it. Then offer the next step: same theme, or a band about 100 points higher after a clean solve and about 100 lower after a struggle.

Keep the puzzle ID and the count of moves played in every turn so the place in the line is never lost. To know where the pieces are, rebuild the position from the FEN plus the moves played so far. Do not rely on memory of the board.

## A session of several puzzles

Serve one puzzle at a time and finish or abandon it before fetching the next. Keep a short tally in the conversation: puzzle ID, rating, solved or not, hints used. The server keeps no account, score or history, so the tally lives only in this chat. Say so if the learner asks for long term progress tracking.

## Empty results and errors

- **"No puzzle for that theme and rating range yet."** The hosted library does not cover that combination. Widen the range by 100 on each side, then drop one bound, then offer a nearby theme. Tell the learner which filter you widened. The same message appears when the minimum is above the maximum.
- **A theme the tool does not offer** (deflection, smothered mate, zugzwang and so on): say it is not a filter here, offer the closest listed theme or `mix`, and do not keep pulling random puzzles hoping to land on it.
- **An alternative move the learner believes is also winning:** the stored line is the answer key for this exercise. Without an engine, judge an alternative only when the result is concrete and short enough to show, such as a mate or a piece won outright. Otherwise say the puzzle expects a different move and that you cannot rate theirs.
- **The tool fails or times out:** say so and retry once. Do not invent a position.

## What a good turn looks like

- Opening a puzzle: two or three lines. ID and rating, who moves, the last move, the board or a note that it is on the card.
- After a correct move: one line confirming it, one line with the reply, then stop.
- After a wrong move: one line, with no clue unless they ask for a hint.
- After the solve: the line, the idea, the ID, the offer.

## Limits to state honestly

- Puzzles come from the Lichess open puzzle database. The plugin and the service are not affiliated with Lichess.
- Ratings are Lichess puzzle ratings. They are their own scale and do not convert to a FIDE, national or other site rating.
- The hosted library is a growing part of the full database, so some themes, bands and IDs are missing.
- There is no engine, no analysis of other positions, and nothing is saved between conversations.

## Examples

**"Give me a mate in two around 1500. Wait for my move before any hint."**
Call `chess_puzzle` with `theme: "mateIn2"`, `min_rating: 1400`, `max_rating: 1600`. Present ID, rating, side to move, last move and the board. Stop and wait.

**The learner replies "Qxh7+" and the first solution entry is `h3h7`.**
The white queen on h3 capturing on h7 is `h3h7`, so it matches. Confirm, play the second entry as the opponent's reply ("Black takes back, Kxh7"), and wait for the next move.

**"Quiz me on deflection."**
Deflection is not one of the filter values. Say so, offer `sacrifice` or `mix`, and after the solve point out a deflection if the returned labels include it.
