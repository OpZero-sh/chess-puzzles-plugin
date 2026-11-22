# Themes

## Values the `theme` argument accepts

These seventeen values are the whole list. Anything else is rejected by the tool.

| Value | Label in results | Meaning |
| --- | --- | --- |
| `mix` | (none) | No theme filter. Any puzzle in the rating range |
| `mateIn1` | mate in 1 | The learner's first move is checkmate |
| `mateIn2` | mate in 2 | Checkmate on the learner's second move |
| `mateIn3` | mate in 3 | Checkmate on the learner's third move |
| `fork` | fork | One piece attacks two or more targets at once and the opponent can save only one |
| `pin` | pin | A piece cannot move, or moves at a cost, because a more valuable piece stands behind it on the same line |
| `skewer` | skewer | A line attack on a valuable piece that has to move and exposes the piece behind it |
| `sacrifice` | sacrifice | Material is given up for a larger gain a few moves later |
| `discoveredAttack` | discovered attack | A piece moves away and uncovers an attack from the rook, bishop or queen behind it |
| `backRankMate` | back rank mate | Mate on the home rank against a king boxed in by its own pieces, usually pawns |
| `hangingPiece` | hanging piece | A piece is undefended or defended too few times and can simply be taken |
| `promotion` | promotion | A pawn promotes, or the threat to promote decides the line |
| `opening` | opening | The position comes from the opening phase of a game |
| `middlegame` | middlegame | The position comes from the middlegame |
| `endgame` | endgame | The position comes from the endgame |
| `short` | short | The learner plays two moves |
| `long` | long | The learner plays three moves |

A puzzle carries several labels at once. A `mateIn2` result is normally also labelled short, and a phase such as middlegame.

## Mapping requests to a value

| The learner says | Use |
| --- | --- |
| "any puzzle", "surprise me" | `mix` |
| "mate in one", "checkmate in 1" | `mateIn1` |
| "double attack", "knight forks" | `fork` (the tool cannot limit a fork to one piece type) |
| "x ray", "skewers" | `skewer` |
| "discovered check" | `discoveredAttack` |
| "loose pieces", "free pieces" | `hangingPiece` |
| "pawn endings", "rook endgames" | `endgame` (the tool cannot narrow by piece type) |
| "quick ones" | `short` or `mateIn1` |
| "longer calculation" | `long` or `mateIn3` |

## Labels seen in results that are not filters

These appeared in live results. They help when explaining a solved puzzle but cannot be requested. The list is not complete.

| Label | Meaning |
| --- | --- |
| mate | The line ends in checkmate |
| advantage | The line wins a clear advantage rather than mating |
| crushing | The line wins an overwhelming advantage |
| one move | The learner plays a single move |
| deflection | A defender is lured away from the square or piece it guards |
| attraction | A piece, often the king, is forced onto a square where a tactic works |
| capturing defender | The piece that guards a target is removed, and the target falls next |
| advanced pawn | A pawn close to promotion is central to the idea |
| quiet move | A move without check or capture that sets up a threat the opponent cannot meet |
| defensive move | A precise move that is needed to hold on to the advantage |
| kingside attack | The attack is aimed at a king that castled short |
| pillsburys mate | A named mating pattern in which a rook gives mate while a bishop covers the king's escape squares |
| master, master vs master | The game the puzzle came from was played by titled players |

## Using labels without spoiling

- Before the solve, the only label the learner should know is the one they asked for.
- A hint may use a label at the level where the hint ladder allows naming the idea.
- After the solve, use the labels to name what happened, and point to the exact move that carries each one.
