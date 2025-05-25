
### Search Overview
Making a good move randomly in chess is statistically difficult. Chess engines rely on search algorithms to make good moves by looking ahead at different possible moves and [[Evaluation|evaluating]] positions after those moves.

A 2-player zero-sum game with perfect information can be represented as:
- Traversing a tree-like data structure where nodes are board-states and edges are chess moves.
- Search algorithms are used to find the best traversal route through this tree.

#### Shannon's Types
1. Type A: brute force search, looking at every possible variation to a given depth.
2. Type B: selective search looking at "important" branches only. 

Type B is heavily preferred in the past, but with increased strength of computing most algorithms are Type A with aspects of Type B to 'prune' the search tree.

### Search Tree
The search tree is:
- a subset of the total search space
- a [[Graph Theory#Graph Terminology|Directed Acrylic Graph]]. Cycles may exist initially but they are always searched and "removed" by the algorithm
- The root of the search tree is the starting position of the search, from where the best move for that position is searched for
- Leaf nodes in the search tree are either:
	- Terminal nodes (mate, stalemate)
	- Nodes with an assigned heuristic value (desired depth from the root is reached)

### Traversing the Tree

The edge of the tree represent a 'ply', or a 'half-move' in traditional over the board chess. In computer chess, this is just one move. 
- Making a move implies traversing between 2 board states
- Un-making a move means moving from the child board state to its parent previous state
- Not all parent states can be reached from child board state (ex: losing castling rights but can't gain it via making a move a gain), hence needing special arrays / stacks that maintain board metadata. 
- New nodes can be created as new board states (i.e. [[Copy-Make Moves]]) or the existing board state via making and unmaking ([[Incremental Position Updates]])