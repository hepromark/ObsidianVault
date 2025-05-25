### Overview

Evaluation of a board position is to compute the relative value of that position using a [[Heuristic Function]]. Since the goal of chess is to win, the relative value represents the "chance" of winning.

If the engine can see the end of the game in every line: 
- The evaluation will only give -1 (lose), 0 (draw) or 1 (win)
- The engine will only need to search at depth 1 to choose the winning move.

However, in practice, we don't know the exact value of a position:
- Need to make an approximation of the value
- Use this value to compare different positions
- Engine needs to search deeply and find the highest score within some time period

There are **2 main ways** to build an evaluator:
1. Traditional hand-crafted evaluation
2. [[Neural Networks]]

### Hand-Crafted Evaluation

The traditional HCE functions consider the explicit features of a chess position. 
- we score the *resulting position of a result of a move*, not the move itself

#### Scoring
Minimax framework:
- White side is the max player
- black side is the min player
- Always evaluate from the white point of view

NegaMax framework:
- Symmetric evaluation, depending on whose side is to move
- ex: $f(p) = {M - M^\prime}$ where $M$ is the total material of the side to move, and the prime indicates the other side

### Evaluation Philosophy

Knowledge vs. Speed:
- Evaluation is typically a 'rule of thumb' from history of human experience in chess.
- All eval engines have to balance knowledge vs. speed.

Light vs. Heavy:
- Light eval functions are easier to implement, debug, and allows more time for search to work
- Heavy eval functions have more knowledge (i.e. can evaluate a position more accurately)
- Modern chess engines prefers light eval functions, opting to spend more time for searching.