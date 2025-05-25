### Overview
Incremental updates keep global/shared structures up to date after making and unmaking chess moves on the internal board.
- Incremental updates modify the same board for duration of runtime
- Likely keeps the board in fast cache memory
	- Saves memory bandwidth compared to copying (copy-make approach)

Incremental updates are almost obligatory for board arrays, i.e. mailbox representation
- Single move affects a small fraction of the complete board structure
- Update cost is small compared to copying the whole board
- Usually engine doesn't need an array of boards for random access, but only the current one

There are some aspects of a chess position which can not be restored from a child position by unmaking a move:
- Ex: en passant state, castling rights, 50-move rule clock
- Needs to be replayed from root to reach the position
- Stored for random access by **ply** or on a stack
- If the captured piece data isn't inside the move object, then a separate array is needed to store that data for unmake

### State Arrays / Stacks

Since the engine doesn't need ALL board states for random access, an array or stack of old positions in the [[DFS]] can be maintained.
- The stack is pushed onto whenever a new move is made, with its corresponding board state
- The stack is popped whenever a move is unmade, with the parent board's metadata being updated to match info stored inside the stack.