---
name: tactics-practice
description: "Use for a chess tactics lesson, a daily puzzle or a themed practice session with hints that preserve the solve."
---

Choose the entry point from the learner's request. Use `daily_chess_puzzle` for today's UTC puzzle, `get_chess_puzzle` for a specific library ID, or `chess_puzzle` with a theme and optional minimum and maximum rating for a drill. If a narrow request has no results, say which filter needs to widen.

Present one position at a time. Give the side to move and show the board when the client supports it. Otherwise present the position from the tool and offer its FEN for a board editor. The backend supplies a UCI solution for checking moves. Keep the solution and tactic labels that would give it away out of the initial challenge.

As the learner plays, keep the puzzle ID and index into the returned line. The learner plays the first, third and subsequent odd-numbered plies. Compare from-square, to-square and promotion suffix. On a match, show the opponent's next reply and wait for the next attempt. For an ambiguous move ask for its from and to squares. For an alternative outside the puzzle line, explain that the source line differs. Evaluate an alternative only when you can substantiate it.

Offer hints in stages: a forcing-move question, then the relevant piece or target, then a first move if the learner wants it. When the puzzle ends or the user requests the solution, give the line and explain the decisive threat in the position.
