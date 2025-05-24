Developing Autonomous "Driving" for Ice Bergs:
- A ship wants to go through type of water- needs to know what is new / old ice
- Old - went through several summers (ice very hard) want to avoid this type
- Newer Ice - more brittle, can go through
- Ice Berg - avoid it
- Brush ice - slushy easy
- Segmentation System
- Have to generate labelled data (tedious & long work)
- Also factor in temporal information to improve segmentation
- Sometimes have ice / water droplets blocking the lense (occlusions)
	- Use ML & temporal information to fill in the gaps
- Current Research Direction:
	- Fusing Infrared / other camera types 


Model exists:
- SOTA right now
- We want to fuse more data to improve
- There is a fundamental difference between ship sensor systems vs. car sensors
	- Ships don't care (that much) about sensor size & price
	- Weather conditions worse

PIV Pressure
- Try using Robin condition (Neuman + Dirichlet): 

