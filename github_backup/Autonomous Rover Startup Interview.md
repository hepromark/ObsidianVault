### **Autonomy**
- WATO: A student design team that creates autonomous vehicles.
	- Originally I worked on MPC controls for our full-sized KIA car
	- Jumped at opportunity to start my own project; F1Tenth competition. Smaller team + we move faster + more learning is done
- WARG: autonomous drone team
	- I made onboard clustering algorithm: the drone periodically detects landing pads, but its on a per-frame basis
	- At super long distances (kilometers) slight difference between each bounding box makes a huge difference
	- Also false negatives
	- My algorithm accumulates these detections overtime and does clustering to determine the "Center of gravity" of all the detections, remove false positives, and output a clean landing pad location

### **Multi-agent systems**
My definition
- Multiple agents (our case robots) + their environments

Rapyuta Robotics MAS
- Complex robotic agents + discretized operating space
	- The "grid" system discretizes position & heading -> discretized configuration space
- Decentralized system; there is no central server that track robot positions
- Robots communicate amongst themselves & path plan to achieve goals (very complex, we have inhouse path planning algorithm)

F1Tenth MAS
- Funding achieved, we have basic software components done
- Looking into simulating head-2-head racing (against components)
- Essentially a competitive MAS where our agents wants to drive faster than other agents

### **Resume Things**

Rapyuta Robotics Context: https://www.rapyuta-robotics.com/solutions-asrs/
- Warehouse robotics startup
- Amazon Roombas but in 3D: 3D grid structure with boxes of objects inside
- Robots traverse this grid (xyz directions) to pickup boxes and move them to desired locations

**Genetic Algorithms**
- We have pre-defined positions for static things in the 3D grid: elevators, storage positions, charging locations, etc...
- Ex: We want to put chargers not too close to elevators that it causes traffic jams; both of these are high traffic areas. But not too far because then charging takes longer than necessary
- Using genetic algorithms, I optimized placements. 

**Discrete Event Simulator**
- How does an engineer test a new algorithm / idea for performance?
	- Real system? Our real integrated testing setup is small
	- In production? 💀
	- In dockerized sim? Too slow (a whole night)
- New discrete event sim; simulates time as discretized ticks & all events happen in this tick
- Abstracts unimportant components & makes trying out new algorithms / tile placements much faster

**Bin Task Planner**
- [[Rapyuta Robotics - BTP]]

**RRT**
- Iteratively samples the free space for a node -> "connect" that node back into the closest existing node of the tree
- Keep repeating until goal reached

**Lattice Planner**
- Discretize the state space into a tree structure
- Connects those states using short motions: *motion primitives*
- Motion primitives guaranteed to fit physical constraints (max turning angle, speeds, etc.)
- Resulting path is immediately driveable
- Expensive computationally is the main concern

#### Questions for Ali:
- What type of rovers
- Hardware inhouse? What part of software inhouse?
- How funding will work

![[Pasted image 20250503222653.png]]