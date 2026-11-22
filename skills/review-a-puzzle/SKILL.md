---
name: review-a-puzzle
description: "Reopen a chess puzzle by its ID and either retry it or walk through the solution move by move with the tactical idea explained. Use when someone gives a puzzle ID, says reopen puzzle xtK8X, explain that last puzzle, why does this work, show me the solution, what was the tactic, or wants to revisit puzzles they missed earlier."
---

# Review a puzzle

Bring back one puzzle from the Chess Puzzles server by ID and help the learner understand it.

## When to use

- The learner has a puzzle ID from an earlier session, from a friend, or from a tally of missed puzzles.
- A puzzle in this conversation is finished or given up and they want it explained.
- They ask what a theme label on a solved puzzle means.

For a new puzzle use `tactics-practice` or `daily-puzzle`. This skill does not analyse the learner's own games or positions pasted as FEN: the server has no engine and only returns puzzles from its library.

## Workflow

1. **Get the puzzle.** If the puzzle is already in the conversation, use that result. Otherwise call `get_chess_puzzle` with `puzzle_id`. IDs are short and case sensitive (for example `xtK8X`), so pass the ID exactly as given. The call is read only and returns the same puzzle every time.
2. **Ask which mode, unless the request already says.**
   - **Retry:** present the position without themes or solution and run the solve loop: the learner plays the 1st, 3rd and 5th solution entries, you play the opponent's replies, hints only on request.
   - **Walk through:** go to step 3.
3. **Walk through the line.** For each move give the move in standard notation, what it threatens or captures, and why the reply is forced or best. Work from the FEN and apply the moves in order. State only what you can see on the board: a check, a capture, an attacked piece, a square the king cannot use.
4. **Name the idea.** Use the returned theme labels and attach each to the move that carries it. Explain any label the learner may not know.
5. **Point back to the cause.** The header reports the opponent's last move. When the position makes it clear, say what that move gave up: a guard removed, a line opened, a piece left loose.
6. **Close with one cue to carry forward** and the puzzle ID. Offer a fresh puzzle on the same theme through `tactics-practice` if a filter for it exists.

Shared reference files live beside the `tactics-practice` skill:

- `../tactics-practice/references/puzzle-output.md`: the layout of a result and the board orientation.
- `../tactics-practice/references/notation.md`: FEN, UCI and standard notation.
- `../tactics-practice/references/themes.md`: filter values and the meaning of labels seen in results.
- `../tactics-practice/references/hint-ladder.md`: hint levels for a retry and the outline for an explanation after the solve.

## Evidence rules

- The stored line is the source of truth for what the puzzle expects. Do not present another line as the solution.
- No engine is available. Do not give evaluations in pawns or claim a move is "best" beyond the stored line. If the learner asks about a different move, follow it only as far as you can show concretely, such as a recapture or a mate, and say where your certainty ends.
- Rebuild the position from the FEN and the moves each time. If you are unsure where a piece stands, say so and check before continuing.
- Give `+` and `#` marks only when you have confirmed the check or mate on the board.

## Errors and edge cases

- **"Puzzle <id> is not in this library yet."** The hosted library is a growing part of the Lichess database, so a valid Lichess ID can be absent. Check the capitalisation with the learner once, then say it is not available here and offer a themed puzzle.
- **The learner gives a lichess.org puzzle link.** The ID is the last part of the link's path. Try it, and expect that it may not be hosted.
- **No ID and no puzzle in the conversation.** Ask for the ID. The server has no search by position, date or description and keeps no history of what the learner solved.
- **Several missed puzzles to review.** Take them one at a time, in the order the learner prefers.

## What the answer should look like

A short header with the ID, rating and side to move. Then the line as a numbered list, one move per item with its reason. Then two or three sentences on the idea and the cue. Keep it under a screen unless the learner asks for more depth.

## Limits to state honestly

Puzzles come from the Lichess open puzzle database, released under CC0. The plugin and the service are not affiliated with Lichess. Explanations are the assistant's reading of the stored line, not engine analysis.

## Examples

**"Reopen puzzle xtK8X and let me try again."**
Call `get_chess_puzzle` with `puzzle_id: "xtK8X"`. Present ID, rating, side to move, last move and board. Wait for a move.

**"I gave up on that one. Why does the queen sacrifice work?"**
Use the puzzle already in the conversation. Walk through each move, show which squares the king loses after the sacrifice, name the labels that apply, and repeat the ID.

**"Explain puzzle abcde."** The tool answers that the puzzle is not in the library yet.
Say so, ask whether the ID is typed exactly, and offer a themed puzzle instead.
