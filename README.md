# Notebook Tic-Tac-Toe

A single-page tic-tac-toe game styled like a sketch on graph paper (with a blackboard dark mode).

## Features

- Play against the computer (Easy or Hard) or a friend on the same device
- Board sizes 3×3 (3 in a row), 4×4 and 5×5 (4 in a row)
- Ultimate Tic-Tac-Toe: nine small boards; the square you pick sends your opponent to the matching small board, and three won boards in a row win the game
- On 3×3 the Hard computer is unbeatable (full minimax); on bigger boards and Ultimate it looks ahead with alpha-beta search for ~0.3s per move
- Scores and settings are saved in the browser

## How the computer plays

**Easy (every mode):** 60% of the time it plays a random legal move; otherwise it plays the same move as Hard.

**3×3 — Hard ("Unbeatable"): full minimax.**
It plays out every possible continuation to the end of the game and scores finished games +1 (O wins), −1 (X wins) or 0 (draw), assuming both sides play perfectly (O maximises, X minimises). The 3×3 game tree is small enough to search completely, so it never loses. Equally good moves are chosen at random so it doesn't always play the same game.

**4×4 and 5×5 — Hard: time-limited alpha-beta search.**
These trees are too big to search to the end, so it uses:

- **Iterative deepening:** search 1 move ahead, then 2, then 3, … until about 0.3 s has passed, and play the best move from the deepest search that finished.
- **Alpha-beta pruning:** skip moves that can't change the result, so it sees further in the same time. Centre cells are tried first, which makes pruning more effective.
- **Scoring unfinished positions (open lines):** for each possible winning line that only one player has marks in, add 10^(marks) for O and subtract it for X. Lines containing both X and O are dead and score nothing.
- **Wins and losses:** a forced win scores very high, and sooner wins score higher than later ones. If it sees it will lose whatever it does, it keeps the move from the shallower search, which still blocks immediate threats, so a human can still slip up.

**Ultimate — Hard: the same time-limited alpha-beta search**, with Ultimate's rules and its own scoring:

- **Legal moves** follow the "sent to a small board" rule; a finished board gives a free choice.
- **Scoring:** ±100 per small board won; ±30 / ±300 for big-board lines where one player holds 1 / 2 won boards; ±1 / ±6 for open lines with 1 / 2 marks inside small boards that are still open; ±20 against the side that hands the opponent a free choice.
- **Move order:** small-board centres first, then corners, then edges.

**3D Cube:** no computer yet — the cube is 2-player only for now. Its 49 winning lines use the same tables as the search above, so a computer opponent can be added by reusing it.

## Run

Open `index.html` in any modern browser — no build step or dependencies required.
