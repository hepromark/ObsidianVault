
### Architecture

0x88 Board class `board.h`
- Iterative-updates to the board
- singleton object

MoveGenerator class
- Generates pseudo-legal moves

MoveMaker class
- Makes *legal* moves -> rejects illegal moves (moves that will place own side into check)
- Mutates singleton Board
- Unmakes moves
- Contains a history of board states for unmaking
- Calculates if a move puts opponent or self in check:
	- If self in check: reject move
	- If opponent in check: sets a "Check" flag

Search class
- Uses move-maker and move-gen classes to find and make moves
- Calls evaluation on board states achieved by move-maker

Evaluator class
- Computes the score for an input position
- 





### TODO List
1. [ ] Board Representation
	2. [x] [[Mailbox Representation|0x88 Representation]] ✅ 2024-12-19
	3. [ ] Board metadata
	4. [ ] Move generation
		5. [x] King ✅ 2024-12-21
			6. [x] Normal ✅ 2024-12-21
			7. [x] Castles ✅ 2024-12-23
		6. [x] Knight ✅ 2024-12-21
		7. [x] Bishop ✅ 2024-12-21
		8. [x] Rook ✅ 2024-12-21
		9. [x] Queen ✅ 2024-12-21
		10. [x] Pawn ✅ 2024-12-23
			11. [x] Moves ✅ 2024-12-21
			12. [x] Takes ✅ 2024-12-21
			13. [x] En-passant ✅ 2024-12-23
			14. [x] Promotions ✅ 2024-12-23
			
	15. [ ] Move making
		16. [ ] Zobrist keys to Hash board states
		17. [x] Update castling rights ✅ 2024-12-30
		18. [x] Update en passant states ✅ 2024-12-30
		19. [x] Reversible move clock ✅ 2024-12-30
		20. [x] Make the move ✅ 2024-12-30
			21. [x] quiet moves ✅ 2024-12-30
			22. [x] capturing moves ✅ 2024-12-30
			23. [x] castles ✅ 2024-12-30
			24. [x] en passant ✅ 2024-12-30
			25. [x] promotions ✅ 2024-12-30