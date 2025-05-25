### Overview
A chess engine needs an internal board representation to maintain chess positions for:
1. Search
2. Evaluation
3. Game-play

To fully specify a chess position, information required is:
- Piece placement
- Side to move
- Castling rights
- En passant target square
- Number of reversible moves (to track the 50 move rule)

### Internal Board Data Structures
This is the pure data structure to represent the board and pieces on the board. 

#### Piece-centric Representation
This representation keeps lists, arrays or sets of all pieces still on the board & what square the pieces are on.
1. Piece-Lists: 
	- Arrays of up to 32 pieces on the board. 
	- Type and color are associated by a certain index range. 
	- Each element of the array of each piece associates the square occupied by this piece.
1. Piece-Sets: 
	- A set-wise representation with one bit for each piece inside one 32-bit word (16-bit word per side)
	- Each set bit associates an index inside a piece-list
1. [[Bitboards]]:
	- Each piece on the board is represented by an a 64-bit word (basically a boolean 2D matrix in 1D form)
	- Piece-wise operators are used to do quick computations between the 32 bitboards (16 bitboards per side)

#### Square-centric Representation
This is the opposite representation of piece-centric; it stores whether a particular square is empty or has some piece.
1. [[Mailbox Representation]].
	- This is the most popular variant, which have arrays that store direct piece-codes (including empty squares & out of bounds codes)
	- Slightly less computationally efficient compared to Bitboards, BUT is easier to implement

#### Hybrid Solutions
Different algorithms may prefer different representations (i.e. piece or square) but it is common for either of these representation to keep a redundant data structure of the opposite type to gain its advantages cheaply.

### Move Generation

Board representations are designed with the generation of moves in mind. Depending on the data structure, different [[Move Generators]] are required.



