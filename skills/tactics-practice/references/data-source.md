# Data source, ratings and limits

## Where the puzzles come from

- The Chess Puzzles server at chess-puzzles.mcp.opzero.sh hosts puzzles taken from the Lichess open puzzle database (https://database.lichess.org/#puzzles), which Lichess releases under CC0.
- OpZero runs the server. Neither the server nor this plugin is affiliated with or endorsed by Lichess.
- The server needs no account. Its own description says requests are not stored.

## What is hosted

- The library is a growing part of the full database. The server's own messages say it "grows every few minutes". A theme and rating combination can be empty today and filled later, and a valid Lichess puzzle ID can be missing.
- An ID returned by this server can be opened again with `get_chess_puzzle`.

## Ratings

- The rating in a result is the Lichess puzzle rating for that puzzle. It reflects how solvers on Lichess have done against it.
- It is its own scale. Do not convert it to a FIDE, national federation or other site rating, and do not tell a learner their playing strength from puzzles solved here.
- `min_rating` and `max_rating` accept whole numbers from 400 to 3200.

Starting bands when the learner gives no number. These are a rule of thumb for choosing a first puzzle, not a standard, and should be adjusted after one or two puzzles:

| The learner describes themself as | First range to try |
| --- | --- |
| New to tactics | 600 to 1000 |
| Plays casually, knows the basic motifs | 1000 to 1400 |
| Club player or regular online player | 1400 to 1800 |
| Strong and wants a fight | 1800 to 2200 |

If a learner knows their Lichess puzzle rating, centre the range on it and use about 100 points each side.

## The daily puzzle

- `daily_chess_puzzle` returns one puzzle per UTC day, the same for everyone who asks that day. It changes at 00:00 UTC, which can be the previous evening or the next morning for the learner.
- It is this service's pick from its library. The result does not say it matches the daily puzzle shown on lichess.org, so do not claim that it does.
- It takes no arguments, so the theme and rating cannot be chosen. The rating can be well above a beginner's level.

## What the server does not do

- No engine evaluation and no analysis of positions outside the library.
- No accounts, scores, streaks or history. Progress exists only in the current conversation.
- No filter by opening name, by piece type, by popularity or by game source.
- No link to the original game in the result.
