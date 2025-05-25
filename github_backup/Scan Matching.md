### Overview
Scan matching is process of aligning a new Lidar scan with:
- A previous scan or
- An existing map
to estimate:
- robot's relative motion between current and previous scan or 
- the absolute pose relative to the existing map

**Common Algorithms**
1. Iterative Closest Point (ICP)
	Align point clouds by iteratively minimizing point distances
2. Correlative Scan Matching (CSM)
	Searching over all possible poses to find best match
3. Normal Distribution Transform (NDT)
	Fits Gaussian to scan region + fit via grad descent
4. Hector SLAM methods

**Example Workflow**
1. Get an state estimation from EKF: continuous robot poses (at high ~100hz)
2. Get the newest 2D Lidar scan (slower rate ~10hz)
	 - Align the scan with the previous scan using ICP or CSM
3. Map Update
	 - Update the global occupancy grid map based on corrected pose
	 - Ex: Get the list of cells the ray casted out goes through + the cell that it hits. 
	 - Each cell contains log odds of occupancy: 
		 - add a small (+) value to increase occupancy chance
		 - add a small (-) value to decrease occupancy chance
4. EKF Feedback
	 - Estimate robot's actual movement by getting state difference between scans (feed this back into EKF to correct for drift)
