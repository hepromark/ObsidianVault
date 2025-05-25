### Overview
Generation of moves is a basic part of a chess engine that is used in the [[Chess Search|search]] routine. The exact implementation heavily depends on the board representation, but there are 2 main types:
1. Legal move generation
2. Pseudo-legal move generation

#### Moves
A 'move' in traditional physical chess is 1 move from both sides, ex: 1. e4 e5
- A 'half-move' is considered the single side's piece movement

In computer chess, a 'move' is actually a half-move but is called 'move' for simplicity
#### Pseudo-legal move
- A pseudo-legal move is a move that obeys their normal rules of movement
- Leaving the king in check is **not considered** in pseudo-legal move generation
- It is left to a later component (move-making function) to test the move (ex: for [[Checks]])
#### Legal move
- These moves must be legal in physical chest, i.e. both movement rules AND check rules are met
- Complexity arise from pins / en-passent / double-checks / etc.

### Generating Moves for 0x88 boards

Encoding moves:
- From square
- To square
- Move type:
![[Pasted image 20241220142128.png]]
- Encoding the piece type isn't necessary, since that can be inferred via the associated board state