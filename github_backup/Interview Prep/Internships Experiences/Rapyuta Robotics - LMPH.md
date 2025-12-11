### Context

Team: Simulation / Algorithms team for a warehouse startup. 2 intern (incl. me) + 1 full-time engineer.
Timeline: Long-term project, did it from Aug - Dec

### Technical Stack

Languages: Python
Libraries: NumPy
### Problem Definition

**Main Goal:** Create a framework to benchmark algorithmic and parameter changes.
**Sub-goals:**
1. Create metrics tracking: storage density, robotic utilisation times, etc.
2. Create a fast simulator that can compute metrics ^ from input parameters
3. Create an optimization algorithm to search for parameters satisfying metric goals

**Constraints**
1. Compute constraint: runnable on GPU-less laptops
2. Algorithm agnostic: we're a startup experimenting with algorithms, the decision making and sim framework can not be coupled because algorithms are constantly changing -> [[Dependency Injection]]

### System Design

![[LPMH.drawio.png]]

### Implementation Details

**Path Planning Component**
The most complex in the stack. Handles robot movement and updates the internal occupancy grid (world model)

Idea:
- Eventually, the production system will achieve near-perfect path planning optimality (or that's the goal)
- Since we use this as a test on the exposed APIs & system params, fix the path planning to the *optimal* level
- Hence, design the *best* planner possible given working contraints

Implementation - For each available new order to be handled, the same steps are ran:
1. Identify closest robot and assign them to task
2. Decide the 'steps' needed by each robot to achieve task -> many combinations here, [[Dynamic Programming]] is used
3. Gives the optimal path for Execution 

**Notable Problems**
- Choosing the appropriate data structures to 

### Results & Impact

Delivered:
- Engineering demos
- Business team tools

Impact:
- Usable pipeline to test new algorithms + for business side to create custom layouts for customers
- Benchmarked current algorithms and used them as reference for future ref
- Helped the leads / manager make decisions using quantifiable data

### Learning:
1. LCs is useful outside of interviews
2. Genetic Algorithm implementation
3. Simulation design & making conscious choice to leave things at "just works" vs. "fully optimize"





