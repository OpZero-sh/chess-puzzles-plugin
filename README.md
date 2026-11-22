# Chess Puzzles

Train a tactic until you can spot it across the board. Pick forks, pins, mating patterns or endgames at a puzzle rating that suits you. The coach gives you time to find the move, then plays the reply from the puzzle line.

[Connect with Claude, ChatGPT, Muse Code, or Grok](PROVIDERS.md)

> “Give me a mate-in-two around 1500. Wait for my move before giving me a hint.”

> “Let's play today's puzzle, then explain the threat I missed.”

## Use it in Claude Code

```sh
claude plugin marketplace add OpZero-sh/plugins
claude plugin install chess-puzzles@opzero
```

Start with `/chess-puzzles:tactics-practice` or ask the **tactics-coach** agent for a drill. A supported client can present a playable board. In a text session, Claude can give you the position's FEN and accept moves as coordinates, such as `g1f3`. Keep a puzzle ID to reopen it later.

The public [Chess Puzzles service](https://chess-puzzles.mcp.opzero.sh/) hosts puzzles from the [Lichess open puzzle database](https://database.lichess.org/#puzzles), released under CC0. The daily selection uses UTC. No account is required.

## In use

Examples from the connected services in Claude.

![A mate-in-one training position](media/mate-in-one.webp)
