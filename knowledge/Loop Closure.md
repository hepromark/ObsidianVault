### Overview
As the robot builds a map of its environment while travelling through, eventually the robot can revisit an old starting point
- Since there is error in the mapping process, drift introduced
- The same starting location in real life won't correspond to the same map state in software

**How Loop Closure Works**
1. Detection
	- Compare current lidar scan to earlier scans / current built map
	- If good match (use some sort of scan matching alg), then same physical position is assumed.

2. Add loop closure constraint
	- Essentially a new edge in the pose graph between prev node & the original node

3. Run [[Pose Graph Optimization]]
	- Globally adjusts the entire trajectory (every single state)

4. Map correction
	- Warp the existing map to match corrected trajectory