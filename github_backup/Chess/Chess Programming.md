
### Getting Started Guide

The [[Board Representation]] is the foundation of chess engines
- The "back end" of the chess engine.
- How the engine keeps track of the board & rules of the game
- Is the **most important step and must be bug free** for engine to work.
- There is online debugging tests to verify that the board representation works

[[Chess Search|Search]] and [[Evaluation]] is the "brains" behind a a chess engine
- This is what decides what move to pick
- A simple starting algorithm is [[Negamax]] with [[Iterative Deepening]] and pure material eval is a good first step

[[Time Management]] is the logic that controls how long the engine searches for moves
- Needs this for chess engine to play under time control

[[Universal Chess Interface]] is the standard protocol for engine communication
- Open source GUI uses this to display games visually
- Enables proper testing of engine via tools like `OpenBench`, `cutechess-cli`, and `fast-chess`

**Scientific Development**
- Extremely important to make sure there are NO BUGS in the engine first before making new improvements
- Self-play **SPRT** is commonly regarded as the best / most efficient way to reach statistical certainty on if a change improved Elo or not.