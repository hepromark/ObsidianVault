### Kinds of Checks
1. Single Check: 
	- attacked by 1 piece
	- Either piece just moved is attack 
	- or a revealed check by a different sliding piece
2. Double Check
	- Both moved piece is checking 
	- and a sliding piece has a revealed check

### In Check Detection

A common way is: By Last Move
- Consider the previous move made by the opponent
- 2 checks: direct and discovered
- Directs: check if the target square (square piece just moved to) can attack the king
- Discovered: check if any sliding piece behind the old square attacking the king
- En-passant edge case complicated: 2 pawns disappear. 