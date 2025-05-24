The mailbox representation is a common way to store the chess board state in memory, since it aligns most with how traditional chess is visualized

The board is stored inside an array (exact size depends on the implementation strategy) but the core ideas are:
- Each element of the array holds a piece
- Move generation is by modifying the access index

### 0x88 Representation
- Uses a 128 size array to represent the board
- No need to access memory to verify that a move is out of bounds
- 'Ray' movement can be cleanly computed

Visualization of the 128-sized array:
![[Pasted image 20241219161816.png]] 
- Left 8x8 square is the valid board squares
- Right 8x8 is unused out of bounds area
- All the values can be can be converted into base-16 which shows how both the places in base-16th needs to be < 8
![[Pasted image 20241219162044.png]]

From this, we know that:
- The valid squares have their 8th (MSB) and 4th bits must be 0 in binary
- by `AND` the base-2 converted index with 0x88 (0b10001000 in binary), we can check the bounds
	- Result in non-zero: out of bounds. Ex: [58] AND 0x88 == 0b111010 & ob10001000 ==  0b1.... which is clearly non-zero
	- Result in zero: in-bounds. Ex: [75] AND 0x88 == 0b01001011 & ob10001000 ==  0b00000000