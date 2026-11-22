---
name: daily-puzzle
description: "Play the chess puzzle of the day, the same position for everyone during a UTC day, with move checking and hints on request. Use when someone says today's puzzle, the daily chess puzzle, puzzle of the day, let's do the daily, or wants a shared puzzle to compare with a friend."
---

# Daily puzzle

Fetch the puzzle of the day from the Chess Puzzles server and coach the learner through it without giving the answer away.

## When to use

- "Let's play today's puzzle." "What's the daily?" "Did you see today's puzzle? I got stuck."
- A group or pair who want the same position: everyone who asks on the same UTC day gets the same puzzle.

For a chosen theme or rating use `tactics-practice`. For a puzzle from an earlier day, the learner needs its ID and `review-a-puzzle`: the server has no archive of past daily puzzles and no tool takes a date.

## Workflow

1. Call `daily_chess_puzzle`. It takes no arguments. It is read only and needs no sign in.
2. Present the position only: the puzzle ID, the rating, the side to move and the opponent's last move. If the client shows the playable board card, do not reprint the board. In a text session show the returned text board in a code block and offer the FEN. Leave out the `Themes` and `Solution` lines.
3. Mention the rating plainly if it looks far from the learner's level. The daily puzzle cannot be filtered, so offer a themed puzzle at their level as an alternative. Do not swap it without asking.
4. Wait for the learner's move. Check it against the stored solution: the learner plays the 1st, 3rd and 5th entries, the opponent the others. On a match, play the opponent's reply and wait again. On a miss, say it is not the puzzle's move and let them retry.
5. Give hints only when asked, one level at a time.
6. After the solve, or when they ask for the answer, give the full line in standard notation, name the idea, and repeat the puzzle ID so they can share or reopen it.

The detailed rules are shared with the `tactics-practice` skill and live beside it:

- `../tactics-practice/references/puzzle-output.md`: what each line of the result means, including the flipped board when Black is to move.
- `../tactics-practice/references/notation.md`: FEN, UCI moves and converting typed moves.
- `../tactics-practice/references/hint-ladder.md`: the hint levels and what to say after the solve.
- `../tactics-practice/references/themes.md`: the theme labels used in the explanation.

## Things to know about "today"

- The day is a UTC day. The puzzle changes at 00:00 UTC, so a learner in the Americas sees the new one the evening before and a learner in Asia or Oceania later in their morning. If someone says a friend got a different puzzle, the date line is the likely reason.
- Calling the tool again on the same UTC day returns the same puzzle. That makes it safe to call again later in the conversation to recover the answer key instead of guessing.
- It is the service's daily pick from its hosted library of Lichess puzzles. Do not describe it as the daily puzzle shown on lichess.org.

## Errors and edge cases

- **The learner has already solved it today.** Offer to review the line with them, or move to a themed drill.
- **The learner asks for yesterday's puzzle.** There is no tool for that. If they kept the ID, use `get_chess_puzzle`.
- **The tool fails.** Say so and retry once. Do not make up a position or reuse a puzzle from memory.
- **Two people compare answers.** Give each the same treatment: no solution until they ask. If one asks for the answer in a shared chat, warn that it will spoil it for the other.

## Limits to state honestly

Puzzles come from the Lichess open puzzle database, released under CC0. The plugin and the service are not affiliated with Lichess. The rating is a Lichess puzzle rating on its own scale. Nothing is stored: there is no streak, score or history beyond this conversation.

## Examples

**"Let's play today's puzzle, then explain the threat I missed."**
Call `daily_chess_puzzle`, present the position, wait. After the solve or a request for the answer, walk through the line and point to the opponent's last move and what it allowed.

**"I'm about 900. Is today's too hard?"**
Fetch it and report the rating. If it is far above 900, say so, and offer either to try it with hints or to switch to `tactics-practice` with a range near 900.
